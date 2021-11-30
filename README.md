# git-tutorial
Welcome to the Git Tutorial! As mentioned in [the slides](https://docs.google.com/presentation/d/1gJL-C79ls_R32igyXUkL9UVrQPRP2OOpqMw8oalJM-I/edit?usp=sharing), we're going to be focusing on getting you operational in Git so you can begin learning some tips and tricks.

## Introduction
First, let's try to get some initial questions out of the way if they haven't been answered already.

### What is Git?
Put simply, Git is tool that aids us in the process of creating and managing versions of code. It also has features that enable the sharing of these versions for collaboration and distribution.

There is much more complexity hidden within Git that you will learn over the course of your career. With this tutorial the main goal is to introduce you to some of the basic concepts and tricks that will start you on this journey.

### Why do people use Git?
Without it, or some other version control system, managing code becomes much harder. But what is management of code, exactly? Why is it important? 

At its core, code management is used to describe how we manage the change of the code over time. As bugs are discovered/fix, new features are added, and portions are refactored we are managing that code. Git gives us an easy way to do this through a feature-rich CLI.

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


## Getting started
The following section is constructed to give you a guided tour of Git. Feel free to follow along with the `example.yaml` file in the this repository.

### Initializing a repository
A repository is a essentially another word for a folder or container of code. In git (unless you are working with subtrees or submodules) each repository (repo) has its own `.git` folder that houses information about commits, branches, and other tidbits of data about the repo. The TL;DR here is that you will need to create the initial `.git` folder to begin utilizing Git properly. To do this, simple run

```bash
$ git config --global init.defaultBranch main
$ git init
```

With this being established, we need some way to share and maintain code across systems - they will not all exist locally. To do this, Git introduces the idea of `remotes`.

### Understanding remotes
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

### Multiple remotes
With this fork pattern, we now have created a concept of `upstream` and `downstream` versions of the project. `upstream` refers to the code that we forked from - the actual original copy of it. `downstream` is our copied version. You can think of this as a river where the changes originate from a location and trickle *downstream* to other locations.

These are easy enough to see in our local repository with a command.

```bash
$ git remote
origin
```

By default, cloning (which we did earlier) a repo will automatically give you the `origin` remote. However, what about the `upstream` repo? As that changes we want to make sure our copy is up-to-date so that our changes are relevant. Well, we just need to add another `remote` to do this.

```bash
$ git remote add upstream https://github.com/tylerslaton/git-tutorial
```

If you would like to name the upstream remote something different, you can replace `upstream` in the command with whatever you prefer. You will refer to that name from now on with remote commands, however.

```bash
$ git remote add some-awesome-name https://github.com/tylerslaton/git-tutorial
```

This process of `upstream` and `downstream` is great when you want to propose a change to a repository but might not have write permissions to it. This is because you can whatever you like to your personal copy and then propose those changes into the `upstream` version so long as it allows it.

Keep in mind that this process of adding multiple remotes is not always necessary if your team solely uses branches.

### Staging code
There are three main stages of code changes in Git:

1. `Unstaged` - Repo has changed from the latest code but has not been staged for commit.
2. `Staged` - Changes in the repo have been staged for commit.
3. `Committed` - Code has been committed and is saved in the repo's history.

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

```bash
$ git checkout example.yaml
```

However, lets keep the change for now. So it looks good and we're ready to move it into the next phase - `staged`. This is useful as we may have multiple concurrent changes going around the repo and we can pick and chose which to or not to `stage`. For now, it is simple and all we have to do is stage `example.yaml`.

```bash
$ git add example.yaml
```

Additionally, you can add everything change in the current repo (across all files) by running the following. However, keep in mind that you may not always want to add all the changes in a repo. Use with caution.

```bash
$ git add .
```

With either of these commands run, we can check the status again to see an update.

```bash
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   example.yaml
```

It now tells us that we have staged the `example.yaml` change. So how do we remove this change from `staging`? With a command, of course!

```bash
git restore --staged example.yaml
```

Will bring this change out of staging for us. From there we can do the `git checkout` trick that we saw above if desired.

### Comitting code
Lets say that we're good with the change and we want it to be save to the project. This process is called `committing` or to `commit` code. This is done simply with a command once we have some changes `staged`.

```bash
$ git commit -m "add foo bar"                                                      
[main 5c9e44a] add foo bar
 1 file changed, 2 insertions(+), 1 deletion(-)
```

There is some information to decipher here, so lets take it one step at a time.

First, with the command itself; What does the `-m` do here? It stands for `message` and is what will appear to describe the changes this commit creates in the Git history. Most often these are `present tensed` meaning that they should describe an action that a commit does, not in the `past tense`.

An example of this would be `add foo bar` instead of `added foo bar`. Thinking "what does this commit do" usually helps me here. However, you are completely free to use whatever commit message style you want here so long as it is descriptive to the change (commit messages are not optional though). 

Aside from the command itself, we can see that a new commit hash (sha) was created: `5c9e44a`. This leads to a good lesson in that every commit in Git has a unique Hash that identifies it when running commands. 

Great! We got our first change into the local repo but what if we've changed our minds and want to update the message? That's where a hand `git commit` flag comes in via `--amend`!

```bash
git commit --amend
```

This will open up the default text editor that you have set for it. If you're confused by the default one for your system, check out [this article](https://www.oreilly.com/library/view/gitlab-cookbook/9781783986842/apas07.html) about how to change it.

There will be a lot of text that comes onto the screen but you can ignore anything that begins with a `#` (though they usually have good information to read). To edit the commit message, just change the first and only `#`'d line and save/exit. Like magic, the commit message is now updated! 

This can also allow us to add new changes to a commit that we may have forgotten about. 

```bash
$ git add .
$ git commit --amend
```

These are two commands that have helped me keep my Git history clean more times than I can count.

## Wait, what are branches?
Typically, you will have a team where multiple people are working on different features concurrently. Similarly, you will have different versions of the application running at a time. This can usually be seen most evidently with a `production`, `staging` and `development` version of a system. So how do we manage all of these different streams of development? Git has the answer - `branches`!

Earlier you may have noticed that there have been multiple references to the word `main` across our commands. In fact, we ran a command (see the `Initializing a repository` section) earlier that would set the default of all newly initialized repos to be `main`. In this case, `main` is a branch! It is the default that is considered to be the most stable version of your project. How can we confirm this? With a command, of course!

```bash
$ git branch
```

This will pull up an editor that displays all of your current branches abd you should see `main` here. To exit out of this screen, simply hit the `q` key.

### When to use branches

Let's enter a new scenario. Say you are working with your group at a `hackathon` and have gotten a very basic version of your project completed. From here you can begin working on your nice-to-haves which you have decided to divvy up evenly. Your task is to add a information to the `example.yaml` about various colors. 

In this scenario, it makes sense to go ahead and make a `branch` for your change. This is ideal as it will let us compare difference to whatever other changes are going into `main` before our change. The group is thus enabled to work asynchrounously from each other easily without fear of stepping on each other's toes (Git has conflict protection we will go over soon).

Adding a new branch is simple.

```bash
$ git checkout -b my-new-branch
```

This will both create and switch to the new branch. You can also create the branch without switching to it.

```bash
$ git branch my-new-branch
```

We can easily swap back and fourth between branches with the `checkout command`

```bash
$ git checkout main
$ git checkout my-new-branch
```

We can also confirm which branches exist and which one we are currently on with the branch command. Now is a good time to add the colors change to our branch.

```yaml
hello: world
foo: bar
colors:
- red
- green
- blue
```

And of course, go through the process of committing the change.

```bash
$ git add example.yaml
$ git commit -m "add colors data to example.yaml"
```

### Git flow
When working with multiple developers on one project, the branches on that project can easily begin to get out of hand. There are many many different opinions about how to solve this. One very common one is a framework referred to as `git flow`. This is where you have branches for `main`, `develop`, and `feature-1`, `feature-2`, etc. In this methodology, `main` is considered to be the most stable and available version of the application, `develop` contains new and upcoming features and lastly `feature` branches contain new and upcoming features. 

Understanding this is most easily done when looking at a diagram, so lets look at this really good one from Atlassian. 

![](https://i.imgur.com/pbRWQHG.png)

There is a lot going on here, so let's break it down. If you look at the `light blue` lane you can notice that each is circle is pointing to a version of `main`. This is also done for the `purple` and `feature` lanes as well to represent `develop` and `feature` respectively. The main thing to take a way here is how each lane interacts with each other. `main` does not merge into another lane - it is the one that each other lane eventually merges into. Ultimately, `git flow` is seeking to define a single source of truth for the application and does so by minimizing the amount of developer toes stepped on via branches.

If you are trying to get your project done quickly (in a hackathon setting) this does not always make sense to follow. You will want to be moving as quickly as possible to get your project done in the short amount of time. However, in virtually any scenario you will want to use some form of branching to make sure that you and another developer are not creating conflicts with the other's work. If you take anything from this section, it would be that you should try to avoid committing/pushing directly to `main` most of the time.

Take a look at [this tutorial by Atlassian](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=The%20overall%20flow%20of%20Gitflow,branch%20is%20created%20from%20main&text=When%20a%20feature%20is%20complete%20it%20is%20merged%20into%20the,branch%20is%20created%20from%20main) for more information on this topic

## Working with remotes
So now we have our code changed on our branch, what's next? How do we share these code changes in our branch with others or get review? Going back to the idea of remotes, they're a two-way street. We can both `pull` (get the code history from the remote) or `push` (send our local code history to the remote). The process we're going to follow in this tutorial is fairly standard. 

1. Confirm that our `main` branch is up-to-date. If it isn't, update it.
2. Push and create our changes in the GitHub remote branch.
3. Create a pull request into `main` to look at the review process.

### Pushing our code
Let's assume that we have the changes in a branch called `my-new-branch` that we added in the `Wait, what are branches?` section. We now want to get this branch to the GitHub remote so that we can have our teammates see the changes and get review on it. Doing this is fairly simple so long that you have the correct permissions. 

```bash
$ git push upstream my-new-branch
```

Most likely, however, you won't have the ability to do this. So instead, we can push it to our `downstream` version of the project and then create a pull request from there.

```bash
$ git push origin my-new-branch
```

Now our code is in our version of the project and we can proceed!

### Creating pull requests
With all of our code up in GitHub, we can begin going through the process of getting it reviewed and merged via a `pull request`. A `pull request` is essentially the same as a review. It will display all of our code changes relative to the branch/repo we are trying to merge into. Let's a take a look at this on GitHub itself (after having pushed our branch up).

First, navigate to your personal fork. Once there, go to the `pull requests` tab. 

![](https://i.imgur.com/XtlrV0B.png)

Next, push the button to create a new `pull request`

![](https://i.imgur.com/r4nePCc.png)

On this screen, we need to specify which branch we are going to be merging from. One helpful thing to note here is that you can do this both `upstream` or in a `downstream` fork (so long as it was forked from this repo). Make sure that the `compare` on the right side matches your branch and that the `main` you are merging into matches the `upstream` (or your fork's) main.

![](https://i.imgur.com/t4Lho6b.png)

With the pull request creation, you can specify comments, links, and see details about all the different changes that this will create. Your teams will likely have some sort of template that you will fill out here. Once submitted, another maintainer of repo can come and inspect the changes, approve them, and merge them as necessary.

## Tips and tricks
Now that we have an operational understanding of Git, let's learn some tips/tricks that I use daily!

### Git stash
There have been a countless number of times that I have been coding away at a new feature and I want to save that progress as I switch to another progress. How do I do this without committing those changes or losing them all?

As is a common theme, there is a command for this!

```bash
$ git stash
```

This will store all of the changes in your local repo but they will not be committed or lost. Then, when you want to reapply those changes, simply say so!

```bash
$ git stash apply
```

There is a lot more to this command to learn, but this is how I most often use it. Take a look at [this document entry](https://git-scm.com/docs/git-stash) for more about it.
### Git rebase
It is very often that your current commits you have added will be messed up in someway. Either they are out of order, have the wrong commit messages, could be combined with another commit, etc. What if you could go back and interactively edits these commits? With `git rebase`, you can!

Usually I will use `rebase` to combine (squash) two commits together or to changes message. However, there is an insane amount that you can do with this tool. For example, you can use it to update your current branch with that of another `upstream` repo. Essentially, it lets you cross-stitch the history of two versions of the application together to create one combined one (and edit it along the way).

First, let's get a commit that we want to start with.

```bash
$ git log

(...)

commit GRAB THIS >= 71be8411cce7d89525c598b8487759ea68e4e13d <=
Author: Tyler Slaton <mtslaton1@gmail.com>
Date:   Mon Nov 29 20:56:36 2021 -0500

    initial commit
```

Copy the line pointed out in the above snippet so we can run the following command.

```bash
git rebase -i 71be8411cce7d89525c598b8487759ea68e4e13d
pick c60bb2c v0.0.1
pick 2ba6548 v0.0.2
pick 5c9e44a add foo bar

# Rebase 71be841..5c9e44a onto 71be841 (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using bash
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
```

This now takes us to the interactive rebase. We can do all sorts of things here, for now lets just squash all three commits into one (note, if you're using the default editor of Vim, hit `i` to write and then hit `:wq` in that order to exit).

```bash
pick c60bb2c v0.0.1
squash 2ba6548 v0.0.2
squash 5c9e44a add foo bar

(...)
```

After saving that file, we are now taken to the editor that we can change the commit message we are squashing into.

```bash
# This is a combination of 3 commits.
# This is the 1st commit message:

v0.0.1

# This is the commit message #2:

v0.0.2

finish remotes section
# This is the commit message #3:

add foo bar
(...)
```

Let's just make this one message now and save.

```bash
initialize repo
(...)
```

After exiting, we have successfully changed the history of the project and squash the commits!

```bash
$ git log
commit e92854ee27f6cf01ee96b602db0d760603ef2306 (HEAD -> main)
Author: HackMD <no-reply@hackmd.io>
Date:   Tue Nov 30 01:45:14 2021 +0000

    initialize repo

commit 71be8411cce7d89525c598b8487759ea68e4e13d
Author: Tyler Slaton <mtslaton1@gmail.com>
Date:   Mon Nov 29 20:56:36 2021 -0500

    initial commit
```

### Conventional Commits
When working in a repository, it is very often that you'll want a standardized messaging structure for commits. This makes it so the repo can both be automated off commit messages (via things like [Semantic Release](https://github.com/semantic-release/semantic-release)) and the git history is easily readable. Luckily, the open-source community has our back here and has an idea about how to do this - [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).

I'd recommend giving [their official documentation](https://www.conventionalcommits.org/en/v1.0.0/) a look, but I can help to summarize. The basic premise is that you specify three things with each commit:

1. Type
2. Scope
3. Description

Most often, a fully constructed conventional commit will look like this.

```bash
$ git commit -m "feat(example): add colors"
```

Deconstructing this further, the official conventional commit website gives us this.

```
<type>[optional scope]: <description>
```

From this, we understand that the type and description are not optional. Every single commit under this framework will need to look like some variant of `type: description`. However, the scope is optional and is not required. That being said, it can often help our commits read better.

```bash
$ git commit -m "feat: add colors to example.yaml"
$ # vs 
$ git commit -m "feat(example): add colors"
```

For the types, there are quite a few officially supported ones. I often refer to [this repo](https://github.com/pvdlg/conventional-changelog-metahub) for a list of them when I'm in doubt. What is cool about this is you can also setup a [commit hooks](https://github.com/conventional-changelog/commitlint) which enforce that all commits must follow this format. This helps new-comers to the repo easily understand how the maintainers prefer their commits. 