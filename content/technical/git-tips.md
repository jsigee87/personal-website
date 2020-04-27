---
title: "Git Tips"
date: 2020-04-26
categories:
- development
- git
---

## Basic Git Usage

A great reference for basic git usage, including push, pull, merge, and other operations is Travis Swicegood's ["Pragmatic Guide to Git"](https://pragprog.com/book/pg_git/pragmatic-guide-to-git).

Julia Evans also has a wonderful and easy to understand introduction to solving common user errors within git in a comic format. You can find her work [here](https://wizardzines.com/zines/oh-shit-git/).

## The .gitignore

Atlassian provides a great tutorial on `.gitignore` files [here](https://www.atlassian.com/git/tutorials/saving-changes/gitignore) explaining what they are and how to use them. You can find a collection of useful `.gitignore` files [here](https://github.com/github/gitignore).

## Writing Commit Messages

Aside from the basic `git commit -m "message"` people often think little about their commit messages until they have to revert a commit. If you want a little more insight into writing commit messages, Mattej Jellus has a concise post about it [here](https://juffalow.com/other/write-good-git-commit-message).

## Setting Meld as Your Merge Tool

If you work on Mac or Linux, [Meld](https://meldmerge.org/) is a great visual diff tool (although it can be tricky to set up on Mac sometimes.) You can configure git to use Meld as your diff tool any time you have to diff or merge.

To meld as the diff tool, add the following to your .gitconfig file.

```
[diff]
    tool = meld
[difftool]
    prompt = false
[difftool "meld"]
    cmd = meld "$LOCAL" "$REMOTE"
```

To meld as the merge tool, add the following to your .gitconfig file.

```
[merge]
    tool = meld
[mergetool "meld"]
    cmd = meld "$LOCAL" "$MERGED" "$REMOTE" --output "$MERGED"
```

When using the meld tool to merge, merge as you would normally in git, with `git merge branch_name`, then after git lists the merge info, `git mergetool`. Git will ask you file by file if you want to keep the remote or local version for deleted files. For modified files, meld will be launched. Each meld window is a merge for one file. On the left pane you will see the current local file. On the right pane you will see the remote file. In the middle pane is the merged result. Use meld as normal by transfering everything you want to keep to the middle pane. Save and exit the window.

## Changing the Default Text Editor 

If you are not a nano user, you may want to change the default text editor for git. This example shows how to change it to vim.

To set Vim as default for Git only, do *ONE OF* the following:

```bash
git config --global core.editor "vim"
```

or

```bash
echo "export GIT_EDITOR=vim" >> ~/.bashrc
source ~/.bashrc
```

To set Vim as default for Git _and_ other programs, do the following:

```bash
echo "export VISUAL=vim" >> ~/.bashrc
echo "export EDITOR=\"$VISUAL\"" >> ~/.bashrc
source ~/.bashrc
```

Note that on Mac, you will need to append to your `.bashprofile` instead of `.bashrc`. It should be in the same location.

## Using git commit --amend

"Git commit amend" is useful for things like typos or if you botched your commit message. Be very careful about amending a commit already pushed to the remote repository, this should very rarely be done.

If you already made a commit, first make your amended changes, then stage them with `git add <file>`. Then run `git commit --amend`, which should bring up the message from the commit you are trying to amend. If you have not yet pushed your changes, then just push as normal. Otherwise if you are amending a commit already in the remote, you will need to use `git push --force <branch>`. Again, you should almost never be using `push --force`, proceed with caution.

## Trunk Based Development

A possible branching strategy for your git project is called trunk-based development. It has become very popular within the DevOps community. Google provides a great blog post on this [here](https://cloud.google.com/solutions/devops/devops-tech-trunk-based-development).

---

This is a collection of notes from my personal archives accumulated over 2018 and 2019.
