# 02-branches-and-commits

In this lesson we are going to discuss how git lets you keep different versions of a set of files, along with separate histories for each of these versions.
We will discuss why you want to do this, as well as how to do this in git. We will also discuss more of the details of how git tracks changes,
and how you can tell git about a set of changes you have made. To discuss these points, we will introduce the git concepts of branches and commits.

## Commits

Up until now, we have discussed the high-level idea of what git is, as well as how you can fork and clone a public repository to your local machine. But how do
you actually make changes and record them in a way that git will know about them? Well, to make the changes you can just edit the files in whatever way you normally
like to edit code: VS Code, a Jetbrains IDE like IntelliJ IDEA, or maybe something old-school like Vim or Emacs. Once you have your changes in a state where you
feel good about them, or some "chunk" of progress has been made you tell git to record the current set of changes by making a "commit".

### What is a commit?

The easiest way to think of a git commit is that it is a snapshot of the entire repository at a point in time. On top of that, this snapshot is associated with
a message and a couple more pieces of metadata, such as the time when the commit was made and an optional cryptographic signature. This is a really powerful
idea! You can record snapshots of your entire project as it evolves! Say for example my project starts at commit `A`. I can make some new changes and then create a series
of new commits: `B`, `C`,  and `D`. At this point, git will have a history of my changes to my project that will look something like:
```
A - B - C - D
```
This can be really helpful! If I discover that I introduced a bug, I can look back at the timing of when I introduced that bug and know which commit(s) may have caused it!
I can also go back and see every change over time, along with a message associated with each change which can help me understand why lines of coded were added.

### How to make a commit

Now that we have an understanding of _what_ a commit is, let's talk about how you make one! The first thing to do is to make some changes to one or more files in the repository.
If you run `git status` at this point, you will be presented with output that looks something like:
```
On branch add-02-branches-and-commits
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	02-branches-and-commits/

nothing added to commit but untracked files present (use "git add" to track)
```
This is git's way of telling us which branch we are currently on (more on that in a second), as well as the fact that we have untracked changes. That means, there
are currently changes we have made to our files which git _knows_ about, but which it is not actively tracking yet. To tell it to track those changes, we can follow
the advice git gives us at the bottom, where it says `use "git add" to track`. To add files to track, all we need to do is run:
```bash
git add <filepath>
```
Here, we replace `<filepath>` with whatever the filepath is to the files we want to commit. This filepath can be relative or absolute - git does not care.

Once git is tracking a set of changes, we are ready to commit them! All we need to do to make our commit is to run:
```bash
git commit -m <commit message>
```
Here, you would replace `<commit message>` with a quoted string containing the message you want to have associated with your commit. For example, I may make a commit containing this 
lesson by running:
```bash
git commit -m "Created the lesson 02-branches-and-commits"
```
This creates a commit with the commit message "Created the lesson 02-branches-and-commits". However, if I don't want to pass the commit message with the `-m` flag, I can also
not specify it when I run `git commit` and my terminal will use the editor stored in the git config to make the commit. By default this will be Vim (which you can exit after
typing by entering `:wq`).

Another scenario may be that you would just like to create a commit with all of the files changes in it, not just those you have added with `git add`. To do this, all you need to
do is run:
```bash
git commit -a -m <commit message>
```

## Branches

Now that you know how to store a change in git, it's time that we talk about branches.
Branches are one of the fundamental concepts of git, helping to empower you and others to make simultaneous change to different versions of the code.

### What is a branch?

A branch is a specific history of the files of a project. Up until now, we have been discussing the history of a project as if it were linear. In the linear world,
the history would look like:

```
A - B - C - D
```

However, let's consider a situation where you want to work on a new feature or bug fix in a repository, and your collaborators also want to make changes to the same 
set of files. Earlier, we said that git could handle situations like this, but how can it do this while maintaining the history of your changes? Well, if you only want
the one linear history without any branches you can go through a lot of difficulty using tools like `git rebase` and `git merge`, although this would quickly get messy.
As such, people generally use branches these days so that is what we will talk about today. 

A branch allows you to start making an independent history of the files starting at a specific commit.
Consider that linear history above. Let's say that I want to start working on a set of changes starting at the newest change, commit `D`. In this case, I will make 
a branch which starts at commit `D` and includes any new changes (commits) that I make. If I make two new changes (commits `E` and `F`), the history would now look like:
```
A - B - C - D
             \
              E - F
```

When I am happy with my changes, I can contribute them back to the "main" branch of the project, and we will have that linear history again:
```
A - B - C - D - E - F
```

But if this all ends up in a linear history, why bother with branches? As I mentioned earlier, there may be other collaborators also making changes at the same time as me.
Let's say that one of them created a new change, commit `G`, while I was working on my changes. At this point, the history would look like:
```
A - B - C - D - G
             \
              E - F
```

Looking at this diagram, you may be able to see why these are called "branches" - they look like branches of a tree! More importantly though, my collaborator was able to 
make their changes while I was working on mine. In fact, they added commit `G` back to the "main" branch while I was still making my changes. Let's consider a (very common) scenario
where I am deploying/building my product off of the main branch. In this case, I would only want complete, working changes on my main branches so that I don't break everything 
for everyone. Since commit `G` is on the main branch, we can probably assume that it is complete. But my feature needed both commits `E` _and_ `F`. What if I had the first change, commit
`E` on the main branch _before_ commit `G` was added, but commit `F` was not there yet? Well, in this case, even though commit `G` is a full and working change, it stands to reason
that the main branch would be broken as my partial, unfinished change would also be present. Another thing to consider is that maybe my collaborator and I are working on changes
to the same functions in the same files. We may in fact be overriding each others work in our changes, which would cause endless frustration for both of us.

How do branches fix this issue? First of all, when I use a branch and only add changes back to the main branch when they are complete, then at every point in time the main branch
always works. Secondly, when I work on a branch I know for sure that all of the changes made were changes that _I_ made. At the end, when I am ready to contribute my branch back
to the main branch I may very well have some conflicting changes, but I can fix them once at the end rather than throughout the whole development process. Afer I am done my changes, I 
can merge them back into the main branch, and my history will look like:
```
A - B - C - D - G - H
             \     /
              E - F
```

In this new history, commit H represents the point where I brough my changes back onto the main branch, and resolved any issues that occured in the process. But what if I want a 
linear history, rather than this branching history? Well, in that case I could _rebase_ them back onto the main branch. If I rebase my branch onto the same main branch as before,
my new history would become:
```
A - B - C - D - G - E - F
```

### How to make a branch

Now that we have seen some of the powers of branches and what we can do with them, let's talk about how you can make one! To create a branch, all you need to do is run:
```bash
git branch <branch_name>
```
Here, you would replace `<branch_name>` with the name off the branch you will be working on. Try and use something descriptive but brief! For example, this file was initially
developed on a branch named `add-02-branches-and-commits`. 

After running this command, you will have a new branch with the name you specified. However, you will not currently be working on that branch! All your changes will still be made
on your original branch! To start making changes on your new branch, you will need to "checkout" the branch. Checking out a branch is your way of telling git which branch you want
your changes to be applied on. To see all of the branches which are available to you to checkout, run:
```bash
git branch
```

Once you know which branch you want to checkout (if you just made a new branch it's probably the one you just made!) then all you need to do is run:
```bash
git checkout <branch_name>
```

Once again, we will replace `<branch_name>` with the name of the branch we want to checkout when we run this command.

Now you might be thinking "that's a lot of work just to create and checkout a new branch!", and you are totally correct - there is an easier way! You can actually do both of the
previous steps in a single command by running:
```bash
git checkout -b <branch_name>
```
You are probably getting the idea by now, but you will once again replace `<branch_name>` with the name of the branch you want to create and checkout. It's worth noting now that
the branch name has to be unique! If there is already an existing branch with the same name, git will not let you create a new branch with the same name.

