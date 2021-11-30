# git-tutorial
Welcome to the Git Tutorial! As mentioned in [the slides](https://docs.google.com/presentation/d/1gJL-C79ls_R32igyXUkL9UVrQPRP2OOpqMw8oalJM-I/edit?usp=sharing), we're going to be focusing on getting you operational in Git so you can begin learning some tips and tricks.

## Introduction
First, let's try to get some initial questions out of the way if they haven't been answered already.

### What is Git?
Put simply, Git is tool that aids us in the process of creating and managing versions of code. It also has features that enable the sharing of these versions for collabortion and distribution.

There is much more complexity hidden within Git that you will learn over the course of your career. With this tutorial the main goal is to introduce you to some of the basic concepts and tricks that will start you on this journey.

### Why do people use Git?
Without it, or some other version control system, managing code becomes much harder. But what is managment of code, exactly? Why is it important? 

At its core, code managment is used to describe how we manage the change of the code over time. As bugs are discovered/fix, new features are added, and portions are refactored we are managing that code. Git gives us an easy way to do this through a feature-rich CLI.

## Setup
If you're on Linux, odds are that you already have Git installed. To verify this, run the following command in your `terminal` and you should get something similar output: 

```bash
$ git version
# => git version 2.30.1
```

If you're on Windows or MacOS there are some slightly different installation. For context, this tutorial is done from a MacOS perspective. You will notice a `$` at the beginning of commands, please remove these when copy and pasting commands.

### MacOS
On MacOS go ahead and run:

```bash
$ git version
```

This should prompt you to install tools relevant to xCode. Go ahead and initiate this process. Once it is complete you should be good to get started!

### Windows
On Windows, go to this site and install Git for Windows:

https://git-scm.com/downloads

Make sure to click the Windows version. There are other clients but this is the one I generally use on Windows (unless I'm using WSL).

### Initialization
When making changes, it is often useful to know who made what changes to a codebase. To do this, it is common practice to identify yourself through your local Git setup. To do this, simple run the following with your information filled in:

```bash
$ git config --global user.name "first_name last_name"
$ git config --global user.email "your@email.com"
```


## Tutorial
The following section is constructed to give you a guided tour of Git. Feel free to follow along with the `example.yaml` file in the this repository.

### Initializing a repository
A repository is a essentially another word for a folder or container of code. In git (unless you are working with subtrees or submodules) each repository (repo) has its own `.git` folder that houses information about commits, branches, and other tidbits of data about the repo. The TL;DR here is that you will need to create the initial `.git` folder to begin utilizing Git properly. To do this, simple run

```bash
$ git config --global init.defaultBranch main
$ git init
```

With this being established, we need some way to share and maintain code across systems - they will not all exist locally. To do this, Git introduces the idea of `remotes`.

### Working with remotes
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

### Staging code
There are three main stages of code changes in Git:

1. `Unstaged` - Repo has changed from the latest code but has not been staged for commit.
2. `Staged` - Changes in the repo have been staged for commit.
3. `Committed` - Code has been commited and is saved in the repo's history.

*(If you're confused by what a commit is, checkout the commit section here)*

Let's go ahead and make a change to the `example.yaml` in our current repository (if you pulled this code locally). If you're curious, [YAML](https://yaml.org/) is just a data storage language similar to JSON. Let's look at the file as it currently exists.

```yaml=
hello: world
```

Looks great, lots of information there. However, its missing something... let's add another line.

```yaml=
hello: world
foo: bar # <= new addition
```

With this new code added, we are able to see the status of our local repo relative to the last commit. 

```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   example.yaml

no changes added to commit (use "git add" and/or "git commit -a")
```

That's a lot of text, so lets boil it down. We'll cover branches in an upcoming section so we can ignore that line for now. After that line, we notice the output denoting our changes that are not yet staged for commit. If we think back to earlier we remember the three stages, and this denotes our `unstaged` changes. Looking, we can see that `example.yaml` appears as we added a change. 

If we want to remove this from the `unstaged` category, all we need to do is simply update the file (and save it) to what it was before with an undo. Git also has a solution for this where we can run a command to change this file back to what it was.

```shell
$ git checkout example.yaml
```

However, lets keep the change for now. So it looks good and we're ready to move it into the next phase - `staged`. This is useful as we may have multiple concurrent changes going around the repo and we can pick and chose which to or not to `stage`. For now, it is simple and all we have to do is stage `example.yaml`.

```shell
$ git add example.yaml
```

Additionally, you can add everything change in the current repo (accross all files) by running the following. However, keep in mind that you may not always want to add all the changes in a repo. Use with caution.

```shell
$ git add .
```

With either of these commands run, we can check the status again to see an update.

```shell
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   example.yaml
```

It now tells us that we have staged the `example.yaml` change. So how do we remove this change from `staging`? With a command, of course!

```shell
git restore --staged example.yaml
```

Will bring this change out of staging for us. From there we can do the `git checkout` trick that we saw above if desired.

### Comitting code
Lets say that we're good with the change and we want it to be save to the project. This process is called `commiting` or to `commit` code. This is done simply with a command once we have some changes `staged`.

```shell
$ git commit -m "add foo bar"                                                      
[main 5c9e44a] add foo bar
 1 file changed, 2 insertions(+), 1 deletion(-)
```

There is some information to decipher here, so lets take it one step at a time.

First, with the command itself; What does the `-m` do here? It stands for `message` and is what will appear to describe the changes this commit creates in the Git history. Most often these are `present tensed` meaning that they should describe an action that a commit does, not in the `past tense`.

An example of this would be `add foo bar` instead of `added foo bar`. Thinking "what does this commit do" usually helps me here. However, you are completely free to use whatever commit message style you want here so long as it is descriptive to the change (commit messsages are not optional though). 

Aside from the command itself, we can see that a new commit hash (sha) was created: `5c9e44a`. This leads to a good lesson in that every commit in Git has a unique Hash that identifies it when running commands. 

Great! We got our first change into the local repo but what if we've changed our minds and want to update the message? That's where a hand `git commit` flag comes in via `--amend`!

```shell
git commit --amend
```

This will open up the default text editor that you have set for it. If you're confused by the default one for your system, check out [this article](https://www.oreilly.com/library/view/gitlab-cookbook/9781783986842/apas07.html) about how to change it.

There will be a lot of text that comes onto the screen but you can ignore anything that begins with a `#` (though they usually have good information to read). To edit the commit message, just change the first and only `#`'d line and save/exit. Like magic, the commit message is now updated! 

This can also allow us to add new changes to a commit that we may have forgotten about. 

```shell
$ git add .
$ git commit --amend
```

These are two commands that have helped me keep my Git history clean more times than I can count.

## Wait, what are branches?

## Pushing code to remotes

## Creating pull requests

## Tips and tricks