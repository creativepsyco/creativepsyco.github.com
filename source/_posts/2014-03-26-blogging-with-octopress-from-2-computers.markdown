---
layout: post
title: "Blogging With Octopress From 2 Computers"
date: 2014-03-26 00:47:03 +0800
comments: true
categories: ["hacks", "octopress"]
---

I use Octopress to blog from multiple computers, work and home computers, sometimes Virtual Machines even. I don't have a track of where my latest post is going to be written from.

However, recently while deploying I encountered this strange error:

```bash
## Pushing generated _deploy website
To git@github.com:creativepsyco/creativepsyco.github.com.git
 ! [rejected]        master -> master (non-fast-forward)
 error: failed to push some refs to 'git@github.com:creativepsyco/creativepsyco.github.com.git'
 hint: Updates were rejected because the tip of your current branch is behind
 hint: its remote counterpart. Integrate the remote changes (e.g.
 hint: 'git pull ...') before pushing again.
 hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```


The reason is that Octopress only does a push to the `master` branch and not a pull. To resolve this, do this:

```bash
msk@MathBook Pro ~/Projects/creativepsyco.github.com (source)$ cd _deploy
msk@MathBook Pro ~/Projects/creativepsyco.github.com/_deploy (master)$ git push origin master -f
Counting objects: 1313, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (702/702), done.
Writing objects: 100% (861/861), 99.72 KiB | 0 bytes/s, done.
Total 861 (delta 423), reused 0 (delta 0)
To git@github.com:creativepsyco/creativepsyco.github.com.git
 + 4647e10...aa94c85 master -> master (forced update)
```

After that all is well:

```bash
msk@MathBook Pro ~/Projects/creativepsyco.github.com/_deploy (master)$ cd ..
msk@MathBook Pro ~/Projects/creativepsyco.github.com (source)$ rake gen_deploy
## Generating Site with Jekyll
unchanged sass/screen.scss
unchanged sass/styles.scss
Configuration from /Users/msk/Projects/creativepsyco.github.com/_config.yml
Building site: source -> public
Configuration from /Users/msk/Projects/creativepsyco.github.com/_config.yml
Configuration from /Users/msk/Projects/creativepsyco.github.com/_config.yml
Configuration from /Users/msk/Projects/creativepsyco.github.com/_config.yml
Configuration from /Users/msk/Projects/creativepsyco.github.com/_config.yml
Successfully generated site: source -> public
## Deploying branch to Github Pages
## Pulling any updates from Github Pages
cd _deploy
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details

    git pull <remote> <branch>

    If you wish to set tracking information for this branch you can do so with:

        git branch --set-upstream-to=origin/<branch> master

        cd -
        rm -rf _deploy/about
        rm -rf _deploy/assets
        rm -rf _deploy/atom.xml
        rm -rf _deploy/blog
        rm -rf _deploy/docs
        rm -rf _deploy/favicon.png
        rm -rf _deploy/font
        rm -rf _deploy/images
        rm -rf _deploy/index.html
        rm -rf _deploy/javascripts
        rm -rf _deploy/portfolio
        rm -rf _deploy/robots.txt
        rm -rf _deploy/sitemap.xml
        rm -rf _deploy/stylesheets
        rm -rf _deploy/super-awesome

## Copying public to _deploy
cp -r public/. _deploy
cd _deploy

## Committing: Site updated at 2014-03-25 16:44:37 UTC
[master 88fada5] Site updated at 2014-03-25 16:44:37 UTC
 36 files changed, 36 insertions(+), 36 deletions(-)

## Pushing generated _deploy website
Counting objects: 166, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (75/75), done.
Writing objects: 100% (75/75), 9.29 KiB | 0 bytes/s, done.
Total 75 (delta 37), reused 0 (delta 0)
To git@github.com:creativepsyco/creativepsyco.github.com.git
   aa94c85..88fada5  master -> master

## Github Pages deploy complete
cd -
```

Cheers

---
