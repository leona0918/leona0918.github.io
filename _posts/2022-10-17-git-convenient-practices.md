---
title: "Git Convenient Practices"
date: 2022-10-17 +0800
categories: [git]
tags: [git daily usage]
---

Here're some git convenient daily usage, not for git beginners, as no detail explaination for background.

## Use Case 1
It's recommened to frequently commit changes, in that case each commit could be pretty small, and not each commit could be a complete/stand alone feature or fix. Also it's not easy to review too many small commits. So instead of directly use `git commit -m "xxxx msg"`, use `git commit --amend` . 
For example

```
$ git commit -m 'Initial commit'
$ git add forgotten_file
$ git commit --amend
```

> `amend` 修正  You end up with a single commit — the second commit replaces the results of the first.
{: .prompt-info}

### amend with uncommited files
> It’s important to understand that when you’re amending your last commit, you’re not so much fixing it as replacing it entirely with a new, improved commit that pushes the old commit out of the way and puts the new commit in its place. Effectively, it’s as if the previous commit never happened, and it won’t show up in your repository history.
The obvious value to amending commits is to make minor improvements to your last commit, without cluttering your repository history with commit messages of the form, “Oops, forgot to add a file” or “Darn, fixing a typo in last commit”.
{: .prompt-info}

### amend without uncommited files --> only modify laste commit message
If there is no local changed files( unstaged files),  only modifies the last commit's message.
`git commit --amend` edit message in default editor

`git commit --amend -m 'new-msg'`

> Only amend commits that are still local and have not been pushed somewhere. Amending previously pushed commits and force pushing the branch will cause problems for your collaborators. 
{: .prompt-warning}


## Reference
- https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things
- https://tonydeng.github.io/2015/07/08/how-to-undo-almost-anything-with-git/