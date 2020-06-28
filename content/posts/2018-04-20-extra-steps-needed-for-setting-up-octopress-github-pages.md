---
layout: post
title: "Extra Steps Needed for Setting Up Octopress with GitHub Pages"
date: 2018-04-20 00:51:16 -0400
comments: true
categories: ['web development']
---

Having to set up Octopress to use with GitHub pages again, I realized there are a few extra steps I needed to perform that weren't in the <a href="http://octopress.org/docs/">default instructions</a> on the octopress website.

<!--more-->

## Create a new branch 
Create a new branch after forking the octopress repository; this new branch will be the one you work out of, as the deploy rake task deploys to the master branch (you will need to pull and merge on every commit if you do not do this).

## If your deploy is rejected
If your deploy is rejected, you will need to change to the `_deploy` directory and `git pull origin master`, then try deploying again. If on `git pull origin master` you receive an error of `fatal: refusing to merge unrelated histories`, try `git pull origin master --allow-unrelated-histories`, and that should work.

## If you are using a domain (CNAME issue)
If you are using a domain, your `CNAME` file will need to be in the `source` directory if you want it to be in the top-level directory (as it will need to be) of your master branch, which is the only branch a personal (non-project) GitHub Pages repo will serve.

And that's really it. Not too many, but enough to prompt me to make a new post.
