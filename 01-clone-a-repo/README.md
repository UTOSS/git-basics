# 01-clone-a-repo

In this lesson, we are going to discuss how you can get a local copy of a project from GitHub/GitLab onto your own personal computer. 
As we go through this process, we will cover two important topics in Open Source development with Git: Forks, and Cloning.

Before we get started, we are going to introduce one term which will be useful for our discussion today: a repository/repo. In git, the collection
of all the files in your project + the history of changes to those files all form what is called a repository, or repo for short. So, as we discuss
how you can get a local copy of a project from GitHub/GitLab onto your own personal computer, we are actually discussing how you can get a local copy
of a repository from GitHub/GitLab onto your own personal computer.

## Forks

Now you may be thinking, isn't a fork a utensil? What do they have to do with managing a repository? To start, yes, a fork is a utensil - however,
we are talking aobut forks in the sense of "a fork in the road", not in terms of the utensil you use to eat. In Open Source Software (OSS), a fork is when you
make your own copy of an existing project which you are going to maintain from now on. There are generally two reasons why you may want to do this:

1. You want to make some changes to an existing project, but the maintainers of that project do not agree with you on those changes. Here you make your own fork
which you will add your changes to and then work on the maintenace yourself to keep it in sync with the repo you forked from.

2. You want to make some changes to a repository where you have limited permissions to make changes on GitHub/GitLab. Here you make your own
fork with the intention of working on your changes and then contributing them back to the original repo.

Generally speaking, most forks are the second kind - that is to say, you will generally fork a repo with the intention of contributing your changes back to the original repo.
At this point, it is worth introducing a small piece of terminology: upstream, and downstream. In OSS, we generally refer to something as "upstream" if it is the repository
we forked from, and refer to our fork as being "downstream" of the original, upstream repo. We use these terms as they indicate the general flow of changes. Most changes will
go into that upstream repo first, and will then be pulled into our fork afterwards. Of course, we may make our own changes on our fork and then contribute those back to the upstream
repo. However, as there are generally other contributors the general flow of changes will still be: `upstream -> your fork`. 

Please note, the concept of a "fork" is a GitHub/GitLab concept, not one that is natively a part of Git. However, as most git repos these days are hosted on one of those platforms,
and because most large OSS projects use fork-based development, we are covering it today. As this is a concept on GitHub and GitLab, the way you make a fork is through the UIs of
these platforms. To make a fork, follow the links below:

- On GitHub, follow [these instructions](https://docs.github.com/en/get-started/quickstart/fork-a-repo)
- On GitLab, follow [these instructions](https://docs.gitlab.com/ee/user/project/repository/forking_workflow.html)

## Cloning 

We have so far discussed how you can make your own "copy" of a repo through forking it, but how do you get that copy locally on your computer? Simple - you clone it!
When you clone a repo, you are telling Git to take all of the code and the history of that code from the remote repository on some other computer and copy it onto your computer.
Git will also make a reference to the repo you copied it from through what is called a `remote`. A `remote` is simply a remote copy of the same repository. Later in this series of
lessons, we will discuss how you can use remotes to either update your local copy with new changes from the remote repo, or to share your local changes with the remote repository.

With this knowledge of what it means to clone a repo, let's discuss how easy it is to do! Firstly, open a terminal on your computer. Next, using your shell (or Git bash if you are
on Windows and not using WSL) run the following commands:

```bash
git clone <repo_url>
```
In order for this command to work, you will need to replace `<repo_url>` with the cloning url of the repository you are trying to clone. You can find this URL by:

1. Going to the home page of the repository on GitHub/GitLab and clicking on the "code" button seen in the image below:

![](./images;code-button.png)

2. After clicking the code button, copying either the https url or the ssh url from the dialog which appears. Note: using ssh is recommended, but you will need to set up an SSH key
with either GitHub or GitLab (depending on which platform you are trying to clone the repo from). To set up an SSH key you can:

- On GitHub, follow [these instructions](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
- On GitLab, follow [these instructions](https://docs.gitlab.com/ee/user/ssh.html)

## Cloning a Fork

Now that we know what forks are and we know how to clone a repository, let's put these ideas together and talk about how to clone a fork. Luckily for us, this is really easy!
All you need to do is:

1. Fork the repository
2. Using the fork you made in your account, follow the steps above to clone it

But if it is that easy, why are we talking about it here? Great question! There are often a few more things you will want to do when you create a clone of a fork that will
make your life easier as we continue along with developing on our local clone of the fork. Firstly, remember what we said about how a remote lets us update our local copy
with changes from the remote repo? If we also think back to what we said about how changes flow from upstream into our fork you will hopefully realize that having a remote
which points to the upstream repository would be really helpful! So how can we set that up? It's a little bit more complicated than cloning the repo, but not that much harder.
What you need to do is:

1. Run the following command:

```bash
git remote add upstream <upstream_repo_url>
```

For this command to work, you will need to replace `<upstream_repo_url>` with the url you find by following the instructions for the clone url on the upstream repo, rather than your
fork of the upstream repo. This command will then set up a new remote called `upstream` which points to the upstream repo.

2. Run the following command:

```bash
git remote set-url --push upstream no_push
```

This command will make it so that our local git will fail to push to the upstream remote. We'll discuss this more in later lessons, but this can be summarized by that flow we were
showing before of `upstream -> your fork`. This ensures that we can pull changes from upstream but can't directly cause changes to upstream (which is a good thing!).

Now you may be thinking that that is a lot of work to do __every time__ you want to clone a new fork - and you would be right! I personally don't use these steps. Instead,
I use a tool I developed [here called git-utils](https://github.com/Cali0707/git-utils) which provides some extra git commands. If you are interested, there are instructions on how to install
it in the link I provided. What you can do with git-utils installed is simply run the following command:

```bash
git clone-fork <repo_url>
````

Here, you will once again use the `<repo_url>` for your fork, found the same way as if you were cloning your fork manually. However, all of the steps afterwards would be done automatically!

## Practice

To practice the skills you learned in this lesson, we recommend that you:

1. Fork this repository into your account.
2. Clone the fork you made of this repository to your local machine.

