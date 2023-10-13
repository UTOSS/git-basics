# 03-pull-requests

In this lesson, we will be discussing how to sync your changes to a remote repository and how to contribute your changes back into the main upstream repo.
As we discuss this, we will be learning about push and pull in git, as well as the concept of Pull Requests (PRs).

## Git Push

At this point, we know how to create a fork, clone it locally, make a branch and then add a commit to that branch. But how do we share these changes
back to the remote repository? Well, it's quite simple! We need to push our changes to that remote! To do this, we will run:
```bash
git push -u origin <branch_name>
```
As in earlier lessons, you need to replace `<branch_name>` with the name of the branch that you have been working on. However, unlike the commands we have
used before, this command actually changes depending on if it is the first time you are pushing a branch to your remote repository or if you have already
pushed that branch before. If you have already pushed the branch before, the command actually simplifies to:
```bash
git push
```

## Pull Requests (PRs)

At this point you have _almost_ finished contributing your changes back to the upstream repo. You have made your changes on your local copy of your fork, and
pushed them to the remote copy of your fork on GitHub. All that is left is creating what is called a "Pull Request" from your fork to the upstream repository.

A PR is how you request that another repo, often the upstream repo, pulls changes from your repository into theirs. It's a way for you to say "Take all of the code
on this branch of my repository and add it into your repository. Here is why you should do this". All you need to do is tell GitHub which branch of your fork
to take the changes from, and then write a description of what your PR changes and why. Often, larger open source projects have a PR template for you to fill out
which can help you figure out what to write.

To open a PR on GitHub from your fork, [follow these steps](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork).

## Git Pull

So now we have learned one part of syncing our changes with our remote - we can push them! But what if we want to pull new changes from a different remote. For example,
let's say that our upstream repo has had 5 new commits while we worked on our previous task. Now that we are starting on a new task we likely want those new
commits included in our local copy, so that we can build on top of them. To pull in those changes, we will run:
```bash
git pull <remote_name>
```
This is where that earlier work we did when cloning our repo comes in handy! We can pull those changes from upstream directly to our local branch by running:
```bash
git pull upstream
```
It is important to realize that this command brings all of the new commits from the main branch of the remote repository to your local repository and then merges those into your 
current branch. So, if you do not want those on your working branch but on a different branch, remember to `git checkout <branch_name>` to switch to the branch you want 
those new changes on. You will often want to sync these changes to your `main` branch, and then make a new branch off of `main` to work on your feature. But why?

## Main Branch

In git, there is a convention to have a `main` branch (also called `master` on some older repositories). This branch is considered the "source of truth" for the current 
version of the repository, and is where the entire history of code that is built in the project lives. For open source projects, this is really important as the `main`
branch gives a view of every change that has happened to the project, in chronological order. As such, when you are trying to open a PR from your branch to the `main`
branch on an upstream repository, you __need__ to ensure that the only difference in commit history between your PR and the `main` branch is the new commits you have added.
Generally speaking, if there are other differences, your PR will simply be rejected by the community as it would be changing the history of the project after the fact.

In order to ensure that we maintain the proper history on our `main` branch so that when we contribute our changes to the upstream `main` branch there are no issues, we
need to ensure that we are never directly adding commits to our `main` branch. This is why in the previous lesson we told you to make a branch _before_ you made a commit.
As you go through contributing to open source, _always_ make a new branch from `main` before you work on a new feature. The other thing we can do to be extra safe as 
we work and to ensure that we do not accidentally add any commits onto the `main` branch is to set up our global git config to only fast forward when we use `git pull`.
This means that whenever you use `git pull`, if this would create a new commit it will fail. Luckily, if we do everything correctly this will never fail! To set this up, run:
```bash
git config --global pull.ff only
```
If you would like to read more about why this is a sensible default to set, [read this excellent blog post](https://blog.sffc.xyz/post/185195398930/why-you-should-use-git-pull-ff-only).

With this configuration set, we are now able to go through our complete development workflow. When we start a new task, we first run:
```bash
git checkout main
```
This makes sure that we are currently working on the `main` branch. We then run:
```bash
git pull upstream 
```
We now have the most up to date code from the upstream repository in our local copy. We now likely want to make sure that our remote fork also has an up-to-date `main` branch, so
we will run:
```bash
git push
```
Now that both the local and remote copies of our `main` branch are up to date with upstream, let's check out a new branch and work on our changes!
```bash
git checkout -b <new_branch_name>
```
After coding our changes, we are able to make a commit:
```bash
git commit -a -m "a descriptive commit message"
```
And finally, we can push this to our remote:
```bash
git push -u origin <new_branch_name>
```
At this point, all that is left is to follow the instructions earlier to open a PR! Well, not quite. You will also have to wait after you open your PR to get feedback from
maintainers of the upstream repository. After addressing any feedback they provide, you will need to make new commits on your branch and push those to your remote - the PR
you opened earlier will automatically detect those new changes and display them for reviewers! Once the review process is finished, someone will approve your PR and merge it.
Now you are actually done, and have made an open source contribution - congrats!

But this workflow above has a lot of steps, and it can be difficult to remember all of them. It's at this point that I will once again point you to my personal project
[git-utils](https://github.com/Cali0707/git-utils), which provides another utility for git, this time it is `git sync`. What `git sync` does is synchronize all of the changes
from the upstream repository to your local `main` branch, as well as your `remote` main branch, before returning you to whichever branch you were working on before.
This lets you skip the first three steps above! If you then wanted to create a new branch to work on you could run `git checkout main`, and `git checkout -b <new_branch_name>`.

## Practice

To practice your skills and indicate that you have completed these lessons, you should:

1. Push your local changes made in the previous lesson to your remote.
2. Open a PR from your fork to the upstream repo (in this case that would be https://github.com/Cali0707/git-basics).
3. Fix any review comments left on your PR and push those new changes to your remote again.

P.S. If you enjoyed these lessons, or found them useful in some way, please [star the repository](https://docs.github.com/en/get-started/exploring-projects-on-github/saving-repositories-with-stars).
This will help you find the repo to return to it later, and helps make it more visible to others as a good resource for them.

change
