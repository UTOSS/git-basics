# 00-what-is-git

Hello, and welcome to the first lesson about git! In this lesson, we will discuss what is git, and hopefully along the way explain some of the reasons why you may want to use git.

## What is git?

If you google "what is git", you will likely find that:
> git is a distributed version control system.

But what does this mean? Well, to answer that question, we will first discuss the "version control system" aspect of git, and then we will discuss how it is "distributed".

### Version Control Systems (VCS)

Git is a version control system, which in large part means that it lets you track different versions of your files and all the changes made to them. A common question is
why we need git to do this when tools like Google Docs or MS Word will automatically save changes you make to your documents. To understand the reason for git, let us consider
the unique challenges that arise when we are saving files which contain code, as opposed to files with normal text in them. The first thing to notice is that if I have a word
document, I generally only care about the history of that document in relation to itself. That is, as I Control-Z my way through previous changes made to the document, I don't
care about what the contents of other word documents were at that point in time. However, if you think about a set of files containing code, you will quickly realize that
you _do_ need to care about the contents of those other files. If I change something in file A (e.g. by returning to before I wrote a function), and then do not change file B,
I may have just broken file B (to continue the earlier example, if file B had imported the function I had just removed from file A, file B would now be broken).

What git does which is different to MS Word or Google Docs is that it saves the state of _all the files in your project_ at a given point in time, rather than a single file.
It then allows you to step through that history and see how those files changed over time. Let's once again consider the example of file A and file B. If I use git and I return to
a point in history where file A no longer has a function imported by file B, file B will no longer import that function because file B was also reverted to that older point in time!
In addition to simply keeping this history, git also allows me to add messages to each point in history, so that I can state what was changed at each step in time. This can be 
really helpful if you want to look at what has changed between two times without trying to understand all of the code changes, or if you want to remember what the idea was
behind a change you made to the code 6 months ago.

### Distributed

As google tells us, git is not only a VCS, it is a __distributed__ VCS. So what does this mean? Well, it means that git can maintain a history of all the changes to your files
across multiple different computers. If I have one copy of the project on my computer and make some changes, and you have another copy of the project on your computer and make
other changes, git can allow us to keep those two different versions of the project history and changes and reconcile them into a cohesive, working version of the project. On 
its own, this would be an extremely powerful concept, but when you add in tools like GitHub and GitLab which add in UIs and a centralized "main" copy of the project,
you can enable teams all around the world to work on the same piece of software and effectively make their own changes.

But, once again, you may be wondering why we can't just do this with something similar to Google Docs but for code. Well, consider what would happen if you and I were working
on the same piece of code at the same time. You have just finished making a set of changes and would like to run the code and see if your changes work. However, I am still working
on my changes and there is currently a syntax error because I am not done typing. In a Google Docs type of environment, your code would fail to compile/run until I was also finished!
Instead, with git you would have your own local copy of the project and history, where you would make your own changes. You would then test your own changes, unaffected by anything I
am doing on my own local copy. I would also do the same. Then, when we are both happy with our changes, we would both be able to share our changes and have git reconcile the different
histories between our own copies. We would both be able to work on our own code at the same time!

