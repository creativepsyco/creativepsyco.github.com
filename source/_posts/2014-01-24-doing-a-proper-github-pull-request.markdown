---
layout: post
title: "Doing a proper Github pull request"
date: 2014-01-24 16:16
comments: true
categories: ["code quality", "development"] 
---
While there may be different kind of development strategies floating around the merge vs rebase git model, one thing is for sure, you would at the end of the day want a clean merge from the contributors so that it can be tracked tested and cherry-picked if it needs to. This is where pull-requests are so effective.

What is a pull-request?
----------------------
From the [Official Github Guide](https://help.github.com/articles/using-pull-requests):

>Pull requests let you tell others about changes you've pushed to a GitHub repository. Once a pull request is sent, interested parties can review the set of changes, discuss potential modifications, and even push follow-up commits if necessary.

Pull requests are useful in the shared repository model where contributors can work on different topic branches and then file a pull request to be able to initiate discussion about potential changes which does not need to affect the rest of the project.

Example
-------
Here is an [example](https://github.com/square/wire/pull/81) of a pull request I filed for Square's Wire open source project.

<!--more-->
Creating a pull request
-----------------------
These steps create a topic branch off the `master` branch.

```bash
git pull origin master
git checkout -b pull-request-demo
git push origin pull-request-demo
```
After that you can work on the branch and do indvidual commits. At this point if you need to update your local copy of the source you should not in any case do a

```bash
git pull origin master
```

This will screw up your commits by merging the two branches and will cause issues when your topic branch will is merged with `master`. Instead do a `rebase`. In short this will preserve the order of your commits and keeps the git history linear. To understand why please refer to [this](https://blogs.atlassian.com/2013/10/git-team-workflows-merge-or-rebase/)

```bash
git rebase origin master
```
> Never rebase already pushed branches. Rebase should only be applied to your local working branch about no one needs to know. This way we can always force push our changes to Github so that the pull request is bettter as we will see in the next step.

Filing up a pull request
------------------------
You will need to use the browser to visit the repository web page and file one. More details [here](http://yangsu.github.io/pull-request-tutorial/)

![Pull Request Page](http://github-images.s3.amazonaws.com/help/pull_requests/pull-request-review-page.png =800x400)

Modifying Code after discussions
--------------------------------
Once the pull request has been filed you can start discussing with the maintainer of the project about the potential changes that need to be made. For example, style fixes etc.

You can push more commits to the the topic branch and the pull request will get updated. Once the pull request is sort of approved by the maintainer, he/she will probably ask you submit only one single commit for your changes. This is very important as merging any contributed can be tracked.

You would need to squash your commits for this. You can do this by:

```bash
git rebase -i HEAD~5
```
This will allow you to pick the commits that you want to keep or squash them altogether. The disadvantage of squashing commits is that you lose your git history. However, if you want you can have a dirty branch to move the commits to in case you want the history to remain. 

And force push your changes

```bash
git push origin pull-request-demo --force
```

Your changes will appear on the filed pull request page. And if all goes will be merged with the main project branch :)
