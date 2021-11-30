# Summary
Welcome to the Git Tutorial! As mentioned in [the slides](https://docs.google.com/presentation/d/1gJL-C79ls_R32igyXUkL9UVrQPRP2OOpqMw8oalJM-I/edit?usp=sharing), we're going to be focusing on getting you operational in Git so you can begin learning some tips and tricks.

# What is Git?
Put simply, Git is tool that aids us in the process of creating and managing versions of code. It also has features that enable the sharing of these versions for collabortion and distribution.

There is much more complexity hidden within Git that you will learn over the course of your career. With this tutorial the main goal is to introduce you to some of the basic concepts and tricks that will start you on this journey.

# Why do people use Git?
Without it, or some other version control system, managing code becomes much harder. But what is managment of code, exactly? Why is it important? 

At its core, code managment is used to describe how we manage the change of the code over time. As bugs are discovered/fix, new features are added, and portions are refactored we are managing that code. Git gives us an easy way to do this through a feature-rich CLI.

# Setup
If you're on Linux, odds are that you already have Git installed. To verify this, run the following command in your `terminal` and you should get something similar output: 

```bash
git version
# => git version 2.30.1
```

If you're on Windows or MacOS there are some slightly different installation

## MacOS
On MacOS go ahead and run:

```bash
$ git version
```

This should prompt you to install tools relevant to xCode. Go ahead and initiate this process. Once it is complete you should be good to get started!

## Windows
On Windows, go to this site and install Git for Windows:

https://git-scm.com/downloads

Make sure to click the Windows version. There are other clients but this is the one I generally use on Windows (unless I'm using WSL).

## Initial Git commands
When making changes, it is often useful to know who made what changes to a codebase. To do this, it is common practice to identify yourself through your local Git setup. To do this, simple run the following with your information filled in:

```bash
git config --global user.name "first_name last_name"
git config --global user.email "your@email.com"
```


# Tutorial
The following section is constructed to give you a guided tour of Git. Feel free to follow along with the `example.yaml` file in the this repository.

## Initializing a repository
A repository is a essentially another word for a folder or container of code. In git (unless you are working with subtrees or submodules) each repository (repo) has its own `.git` folder that houses information about commits, branches, and other tidbits of data about the repo. The TL;DR here is the you will need to create the initial `.git` folder to begin utilizing Git properly. To do this, simple run

```bash
# Updates the system to initialize with the main branch
git config --global init.defaultBranch main
# Initializes the repo
git init
```

With this being established, we need some way to share and maintain code across systems - they will not all exist locally. To do this, Git introduces the idea of `remotes`.

## Working with remotes
When utilizing Git you will most likely want to share your code for collaboration or distribution. To do this, there is the idea of remote repositories (typically referred to as a remote) that will house your `.git` folder online. By doing this people from around the world are able to contribute to and utilize your code. 

There are many common solutions for this across the internet, today we're using GitHub. However, there are many other solutions such as:

- GitLab
- BitBucket
- Code Commit
- Etc.

I've found that the easiest way to get a new project started in GitHub is to create it there and then clone it to your local machine. If you're going to be starting from an already created repository (say a template project or otherwise, such as this one) you have two options:

1. Pull directly from the original repository.
2. Fork a copy of it for yourself.

Let's go through the process of creating a fork of this repository now.

First, make sure you are on this project's [GitHub repository](https://github.com/tylerslaton/git-tutorial). Once there, look in the top right for the "fork" button as seen here:

![](https://i.imgur.com/3PfDNdf.png)

Upon clicking that button, follow the steps to create your very own fork (copy) of this project's repository! From here, we are going to take this remote code and bring it to our local machine. To do this, click on the "code" drop down box seen in figure 2, click HTTPs ([or SSH if you would like to set that up](https://docs.github.com/en/authentication/connecting-to-github-with-ssh), hint: you will likely need to at some point) and then copy the command listed via the copy icon.

![](https://i.imgur.com/83avPGR.png)

From here, we just need to paste that command into our terminal that is located in a folder that we want to clone this project into. We're now ready to start making some changes!

## Adding code
There are three main stages of code changes in Git:

1. `Unstaged` - Repo has changed from the latest code but has not been staged for commit.
2. `Staged` - Changes in the repo have been staged for commit.
3. `Committed` - Code has been commited and is saved in the repo's history.

*(If you're confused by what a commit is, checkout the commit section here)*

Let's go ahead and make a change to the `example.yaml` in our current repository (if you pulled this code locally).

