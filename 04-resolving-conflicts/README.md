# 03-resolving-conflicts

In this lesson we will be discussing how conflicts may occur, as well as how to resolve them. This is a really important skill,
as you will encounter conflicts at some point.

## Conflicts

Conflicts occur when the history of the branch you are working on and the history of another branch diverge for the same parts of a given file.
To better understand this, let's consider an example of a file `index.js`. Initially, on the `main` branch, there is one commit (which we will all commit `A`).
This commit creates the files with some initial contents:
```js
console.log("hello, world!")
```
At this point, the history for the main branch is as follows:
```
A
```
We now create another branch, called `feature` where we will update `index.js` with a new commit, commit `B`:
```js
console.log("goodbye, world!")
```
Our history for the `feature` branch is now:
```
A - B
```
However, the history for `main` is still:
```
A
```
On `main` we now add a commit `C` which also modifies `index.js`:
```js
console.log("Hello World!")
```
Now, on the `main` branch the history would be:
```
A - C
```
If we try to merge `feature` back into `main` at this point, we will get what is called a merge conflict. This is because while `main` and `feature`
share commit `A` as a common parent commit, they have a conflicting set of changes to `index.js` in commits `B` and `C`. Conflicts occur when branches
modify the same parts of a given file. If instead commit `C` on the branch had not modified the existing line but instead added another line, there 
likely would not be a conflict created.

It is common to get conflicts when trying to merge two branched, rebase one branch onto another, and cherry-picking a commit onto a branch.

## Resolving Conflicts

Resolving a conflict is a complex topic, and it generally requires you to have at least a basic understanding of the changes made on both branches that
led to the conflict. This is because resolving git conflicts required you to pick which parts of each version of the file should stay. Let us consider the
conflict created by running `git merge feature` from the `main` branch in our example above:

```js
<<<<<<< HEAD
console.log("Hello, World!")
=======
console.log("goodbye, world!")
>>>>>>> feature
```

The way to read the conflict is to understand that everything from the line starting with `<<<<<<<` until the line containing `=======` represents the version
of that part of the file from the git ref which is to the right of `<<<<<<`, which in this case is `HEAD`. After the `======` and until the `>>>>>>` represents
the other version of the history, coming from the git ref which is to the right of `>>>>>>`, which in this case is `feature` - the branch we just tried to
merge into `main`.

Here, you will note that while we see `feature` we do not see `main` in the conflict, instead we see `HEAD`. That is because git is comparing `HEAD`
is a special git reference that generally points to the newest commit on your current branch to `feature`.

To resolve the conflict, we need to pick what we want to have in the final version of the file. There isn't _necessarily_ a correct answer here, depending
on what the code is supposed to do. Conside our example: should it print `"Hello, World!"`, `"goodbye, world!"`, or both? Maybe neither? This is where _you_
need to think about what the code is supposed to do and what both sets of changes were attempting to do. In our case, it makes sense to say hello, and then
goodbye. So, let's fix the conflict to print both statements!

Fixing the conflict is quite simple, all you need to do is select which lines of code you want to keep, and then delete the rest as well as the lines which
were added to show the conflict (i.e. the `<<<<<<`, `======`, and `>>>>>>`). Let's do this to our example:
```js
console.log("Hello, World!")
console.log("goodbye, world!")
```
Now, all we need to do is commit the result, and our merge will finish with the conflicts resolved!

## Resolving conflicts on PRs

Perhaps the most common type of conflict you will come across is those in a PR. To resolve these, you will need to:
1. Update your local copy of the branch you are merging into (this will also involve updating your fork with `upstream` if you are working on a fork).
2. Merge the updated local copy of the branch you are mering into back into your working branch (for example, with `git merge main`).
3. Resolve the conflicts.
4. Commit the result.
5. Push the changes back to the remote branch.

However, note that some projects prefer rebasing over merging so be sure to follow whatever practices the project you are working on follows. The actual
process of resolving conflicts is the same for rebases as it is for merges, although you may have to resolve them more incrementally as a rebase applies
the commits one at a time, while a merge creates a new commit resolving the histories.
