---
layout: post
title: "Hosting Git Repos With a Web Frontend"
date: 2017-08-02 00:21:17 -0400
comments: true
categories: ['web development']
---

Hosting your git repositories yourself is as simple as `git init --bare` (or `git clone --bare` for existing repos) in a directory on your server, and then adding the remote to your local repo: `git remote add myServer ssh://me@server.address/path/to/repo/project.git`.

Having your repos on GitHub too makes for a nice redundancy (or on any other git server), and a remote on your server, on GitHub and a local copy makes for three redundancies. You can push to multiple remotes by adding a few lines in your project's git config.

<!--more-->

In your local copy of the repo, edit `.git/config` and add a new block:

    [remote "all"]
        url = ssh://me@server.address/path/to/repo/project.git
        url = git@github.com:me/project
    

That would be the url for your server and for GitHub's. And so now when you're ready to push, `git push all <branch_name>` will push your changes to both repositories. It should be noted that you can name this remote whatever you want.

Installing GitList for a Git Web Interface (and Troubleshooting)
----------------------------------------------------------------

#### (Using Nginx server on Debian 9)

For a nice way to show your code and commits much like GitHub does, there are many [git web interfaces](https://git.wiki.kernel.org/index.php/Interfaces,_frontends,_and_tools#Web_Interfaces) out there, but I chose [GitList](http://gitlist.org/) because it looked pretty good and seemed pretty easy to set up.

I used Nginx for my server, so I followed GitList's `INSTALL.md` instructions for Nginx, copying their provided configuration.  
Here are some issues I encountered and their solutions:

Getting a 500 error when trying to view repos:
----------------------------------------------

Assuming you’re using php-fpm, you’ll need to note the address in GitList’s Nginx config, within the php location block, listed as `fastcgi_pass`:

    location ~ ^/index.php.$ {
        fastcgi_pass 127.0.0.1:9000;
    }
    

Then open `/etc/php/7.0/fpm/pool.d/www.conf` and find the line `listen = /run/php/php7.0-fpm.sock`; here you’re going to change the listed socket to the address in our Nginx location block above so that it reads as: `listen = 127.0.0.1:9000`.

After restarting php-fpm, you should no longer receive a 500 error when viewing repos.

Getting blank screen (500 error) when trying to view commits:
-------------------------------------------------------------

This error persisted for a bit longer than the above for me, but looking at the error log (always check the error log!), I noticed this:

    FastCGI sent in stderr: “PHP message: PHP Fatal error:  Uncaught Error: Class ‘SimpleXmlIterator’ not found in /var/www/gitlist/vendor/klaussilveira/gitter/lib/Gitter/PrettyFormat.php:22
    

Searching for `‘SimpleXmlIterator’ not found` online lead me to find that I needed to install `php7.0-xml`, which I was able to do with apt-get, which solved the issue.
