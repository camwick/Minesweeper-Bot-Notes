# Using Git

## Table of Contents

[Back to Home Page](../README.md)

- [Git](#git)
- [Using Git](#using-git)
  - [Cloning](#cloning)
  - [Branch](#branch)
  - [Adding and Committing](#adding-and-committing)
  - [Merging](#merging)
  - [Pushing](#pushing)
  - [Pulling or Fetching](#pulling-or-fetching)
- [Overview](#overview)

## Git

[Download link](https://git-scm.com/downloads)

Git is a wonderful tool for developers that let's you manage different versions of your code bases seamlessly. Github has created a [desktop application](https://desktop.github.com/) to make using Git easier, but I highly suggest you get used to using Git straight from the command line. You won't always have access to the desktop app and, as future developers, we should feel comfortable using the command line -> especially because a lot of the tech industry uses Unix systems like Linux which reach their full potential by using the command line interface.

Once downloaded and you try to use a Git command for the first time, Git will prompt you to identify yourself by providing a `user.name` and `user.email`. I believe after these have been entered you'll be able to use Git on your local device.

When using Git with GitHub, you used to be able to just log in to GitHub through Git to have the ability to push to remote repositories. GitHub has been pushing the use of [SSH Protocol](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/about-ssh) to authenticate users.
Just follow the directions [here](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/checking-for-existing-ssh-keys) and follow the links until you've given GitHub your generated SSH key. Once GitHub has your SSH key, you should be able to start using Git with GitHub without any trouble.

_You'll need an SSH Key for every computer you use to push to the remote repository. I have three keys registered to my account: 1 PC, 1 laptop, and 1 raspberry pi. It's kind of annoying to have to keep adding keys but it's how GitHub stays secure._

## Using Git

I've already gone through some of the commands, but I'll try to go more in depth here. This will hopefully make it easier to start using Git and eliminate a few questions before we start developing.

### Cloning

Usage: `git clone [<options>] [--] <repo> [<dir>]`.

The most common use for the clone command is to copy a remote repository off of GitHub onto your local machine. For our case, everyone here will clone the MineSweeper Bot repository that's been forked on the PNW CS Club GitHub Organization page using this command: `git clone https://github.com/PNW-CS-Club/Minesweeper-Bot`
This clones the main branch to your local repository.

### Branch

There are a handful of commands for creating, using, and manipulating branches. The two most important ones to us are the `branch` and `checkout` commands.

Branch usage: `git branch [<options>] [<branch name>]`.

Checkout usage: `git checkout [<branch>]`.

You'll notice after you clone the repository that you only have the main branch. The branches on GitHub are seperate from what is on your local device. You can either create your own branch or work directly on your main branch -- I suggest creating your own branch and keep your main branch updated to match the remote's. To create your own branch use the branch command followed by the name you'd like the branch to have:
`git branch testBranch`
If you the branch command without a branch name, it will list all your branches and specify which one you are currently on.

Using `git branch` now after creating your new branch, you'll see that you're still on the main branch. To switch to your new branch use the checkout command: `git checkout testBranch`. If you follow this command with another `git branch`, you'll see you're now developing on the branch you've just created.

### Adding and Committing

So you've cloned a repository and are ready to start developing. Try making some changes to the code base inside your new branch. The whole purpose of using Git is to keep track of all your changes and keep a log of everything. This allows you to backtrack through the history of your code if a bug ever occurs or for whatever reason you need to access an older version of your program.

After you've made changes to your code you need to stage the changes before committing them. You can see what files are needing to be staged by using the status command.

Usage: `git status [<options>…​] [--] [<pathspec>…​]`

Using `git status` will display all files in red that need to be staged. If you've already staged a file but not yet committed, the file will colored green. Once staged, you can commit your file(s) by using the commit command. To stage a file, you use the add command (you'll notice that `git status` tells you what to do if you read all of the output). After staging the file, you can use the commit command.

Usage: `git add [<file>]`

Usage: `git commit -m [<message>]`
_commit has lots of options, the most important is -m which lets you pass a comment/header to describe what the commit is_

By convention, the messages attached to a commit are usually typed in the first person. If you were building a website and added a button, your message would be `"Add button"`, not `"Added button"`. This just seems to be the common practice, but whenever you work for a company, you'll follow their specific conventions. When looking at GitHub, the `[<message>]` is what's displayed to tell everyone what was changed in that commit. You want the message to be descriptive enough to convey what was changed without being too wordy. You can also stage multiple files at the same time to be committed with the same message. You should only stage and commit files that would share the same commit message, otherwise stage and commit them seperately.

After you commit changes, you can see the true glory of Git. Switch back to your main branch by using `git checkout main`. If you try to edit the repository, you'll find that your files have changed (sometimes you will be prompted to re-load your IDE) to what they looked like when you first cloned the repository. Git saves all your code to the specific branches. This allows you to have multiple working versions of your programs for development. For our purpose, we have two branches being worked on at the same time on two seperate branches. Once these branches are stable and working we can merge them to the main branch.

### Merging

When working on your own projects, you can merge your branches whenever you think you're done with whatever the branch's purpose was. I.E. if you created a branch to add a button to a website -> once the button has been implemented, you can merge your now working button branch back to the main branch. Before you merge, you must switch to the branch that you will be merging to.

Usage: `git merge [<branch>]`

This command merges the branch specifed to the one you're currently in. Usually, you use the the checkout command to switch back to the main branch, then use `git merge testBranch` to move the commits from the `testBranch` to `main`.

Since we are working as a team with a remote repository we will merge our branches through the remote repository when we are done working on our seperate branches. So instead of merging our branches on our local repositories, we'll push our changes to the branch in the remote repository. Then we'd merge that remote branch to `main` in the remote repository using a pull request on GitHub.

### Pushing

Pushing allows us to take a branch we're working on in our local repository up to the remote repository. Say I make a commit on one of my local branches for the Minesweeper Bot's `boardState` branch. In order for all of you to also get this change I will need to push my branch to the `boardState` branch using the push command.

Usage: `git push -u origin [<local branch>]:[<remote branch>]`

If you're just pushing to the remote `main` branch, you can omit the `:[<remote branch>]` because Git will default to pushing to `main`. Let's say I made a commit on a local branch named `boardStateChange` and wanted to share it with you guys by pushing it to the `boardState` branch on the remote repository. I'd use the push command to achieve this: `git push origin boardStateChange:boardState`. This takes my local `boardStateChange` and applies it to the `boardState` branch on GitHub.

Now in order for you guys to have the changes on your local devices, you'll need to pull or fetch from the remote repository.

### Pulling or Fetching

Pull and Fetch commands are pretty similar. Fetch is considered the safest option because it does not merge the commits to your local repo. If you use `git fetch`, Git will grab the new/updated commits from the remote repository without merging them with your files. It is then up to you to use the `git merge origin/[<branch name>]` to get the changes onto your local machine.

If you use `git pull`, this acts just like the fetch command but automatically merges the changes to your branch you have currently selected. So let's say I've pushed my changes into the remote repo's `boardState` and now you all have to update your local repos to match what is on the GitHub so we're all working on the same code. You all will have to use either the the pull or fetch commands to do this. Unless you think something will break, it's usually alright to use the pull command in these smaller projects. You'd use `git pull` and it will update your files accordingly.

---

## Overview

To recap what our procedure will be, everyone will clone the remote repository to their local device using `git clone https://github.com/PNW-CS-Club/Minesweeper-Bot`. From here you will create a new branch on your local repo using `git branch [<Branch Name>]`. After you've made a few commits and want to push back up to the remote repository branch that you're helping develop, you'll use `git push -u origin [<your branch>]:[<remote branch>]`. To keep your local repo up to date with the remote repo, use `git pull` or `git fetch`. Once we have finished developing a branch we will create a pull request on GitHub to merge the working branch to main. The branch can then be discarded and we'll create more branches based on what we need to work on next.

---

There are a lot of commands I did not cover. I highly recommend looking at the [Git documentation](https://git-scm.com/docs). Specifially the version control commands like `diff`, `log`, `reset`, and `checkout [<commit>]`. Git not only allows you to have multiple versions of your code, but it stores every commit you make and will allow you to backtrack through the history of your commits. Say I notice a major bug in my project's code, I can switch to an older commit that didn't have the bug. This saves a lot of debugging time.

Again, I recommend getting used to using Git in the command line and just using Git in general. Try to use Git help save your programming homework assignments. Having Git listed on your resume is a big plus and will set you apart from other candidates that haven't used Git before.

Here are some good resources to read that give more insight on how to use Git and the purpose of it:

- [Git Feature Branch Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)
- [Understanding the GitHub Flow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)
- [5 Git workflows and branching strategy](https://zepel.io/blog/5-git-workflows-to-improve-development/)
- [Git Pro by Scott Chacon](https://git-scm.com/book/en/v2)
  - Free book
