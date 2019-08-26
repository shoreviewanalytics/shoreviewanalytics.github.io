---
layout: single
title: 'Create Jekyll Blog with Minimal Mistakes theme on Ubuntu 16.04'
date: '2018-02-19'
classes: wide

---

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/jekyll-logo-black-red-transparent.png" alt="Jekyll Island">

Recently, I discovered [Jekyll](https://jekyllrb.com/ "Jekyll") a static website and blog generator that let's you create a feature rich, professional looking blog and then publish it on [Github Pages](https://pages.github.com/ "Github Pages").

## Why Jekyll?

- A Jekyll blog hosted on Github Pages, gives you complete control over your content.
- Hosting on Github Pages is free (for now!).
- You desire a professional looking blog without the complexity and dependence on a hosting company.
- Because, I give Jekyll a :+1:!

## Here are the requirements I had in mind for my blog.  

- Blog should portable and not dependent on a specific hosting company, especially the content.   
- The theme used for it should be easy to maintain and have advanced features.
- The theme and blog software should be able to be updated.
- It should be low to no cost to deploy.

If you host your blog on Github Pages and abide by their [guidelines](https://help.github.com/articles/what-is-github-pages/#usage-limits) you can deploy your blog or website for no cost.     

By the way there are many web based tutorials that you can review to get started with Jekyll.  To get your feet wet check out the following tutorial.  

[Tania's tutorial](https://www.taniarascia.com/make-a-static-website-with-jekyll/ "Tania's Tutorial")

The steps below outline how to create a Jekyll powered blog using the [minimal-mistakes](https://mmistakes.github.io/minimal-mistakes/ "minimal-mistakes") theme hosting it using [github pages](https://pages.github.com/ "Github Pages"). If you review the quick start guide for the minimal-mistakes theme, it provides three different methods to install. This post explains how to use the fork method. For QA, I have gone through the steps below to build the mininmalmistakesdemo site at least two times to be sure they are accurate and lead to success.

It will be necessary to move slowly and do all the tasks in each step. Some of the steps, like creating a gh-pages layer on Github (as described in Step 10 below), can be confusing if you haven't used git.  

I have not used these steps to build a githubusername.github.io site, because I already have my blog published at that location. From my experience with Github you do not have to have a working site at githubusername.github.io to deploy to githubusername.github.io/subsite, but you do need to use this nomenclature to publish a subsite.  

The cool thing is when you complete this tutorial you should have a fully functional Jekyll blog using the minimal-mistakes theme.

[https://shoreviewanalytics.github.io/minimalmistakesjekylldemo/](https://shoreviewanalytics.github.io/minimalmistakesjekylldemo/ "minimal-mistakes theme demo")

The initial version of the site is pretty basic looking, so you will need to make adjustments and add your own content to make it more interesting. What's really cool about going this route for your blog is you will have a professional blog that looks fantastic on any browser including your mobile phone.        

## Step 1

Install dependencies

```
sudo apt-get install gcc make zlib1g-dev sqlite3 automake autoconf libtool build-essential
```

Install Ruby using the following command:
```
sudo apt-get install ruby ruby-dev
```

Install Bundler
```
sudo gem install bundler
```
Usually git is installed on Ubuntu by default, but in case it isn't, use the following command to install it.
```
sudo apt-get install git
```

If you currently do not have a Github account, you will need to create one, to complete this tutorial.  

## Step 2

After you have a setup your github account, create a new repository using the following naming conventions.

Create a repository with a name of githubusername.github.io if you want a web address like:

**https://githubusername.github.io**

Create a repository with a name minimaljekylldemo if you want a web address like:

**https://githubusername.github.io/minimaljekylldemo**

Creating a repository defaults to the root or master level and you can only have one Jekyll site at this level.  All other sites must be placed at the gh-pages level. For example, if you are reading this post and you already have a blog like I do at http://shoreviewanalytics.github.io you would create a new repository called minimalmistakesjekylldemo and would navigate to it with the following url.  https://shoreviewanalytics.github.io/minimalmistakesjekylldemo/.  

I will get into the details of how this is done and why below.  The main point of this step is to make sure you create an account on github.com and also create the desired repository.  

## Step 3

Create a new folder in your home directory to use as a sandbox for your Jekyll efforts.  I called mine websites, but you can call it whatever suites your needs.    


## Step 4

Next you will need to fork the minimal-mistakes theme.  To do this you will need to login to your github account and then search for the minimal-mistakes repository.  Once you have found the minimal-mistakes repository, click on the fork icon in the upper right hand corner of the repository.  Once the fork process has completed you will have the minimal-mistakes repository as a fork under your github account.   

## Step 5

Now that you have the minimal-mistakes repository forked to your github account, you can clone it to the folder created in Step 3.

Click on the green button that says Clone or download and then click on the copy icon to copy the URL to your clipboard. Issue the following command in a terminal when in the root of your sandbox folder.

```bash
git clone https://github.com/shoreviewanalytics/minimal-mistakes.git  
```

You should now have the minimal-mistakes repository on your local machine in your sandbox / jekyll development directory created above in step 3.

## Step 6

Clean up the minimal-mistakes directory, by removing the following folders and files.
* .github
* .git
* docs
* test
* screenshot.png
* screenshot-layouts.png
* README.md
* .gitiqnore
* .gitattributes
* .editorconfig
* banner.js
* CHANGELOG.md

## Step 7

Using the command line from the root of the minimal-mistakes directory issue the following commands.

--Initialize as git
```
git init
```
--Install bundles
```
bundle install
```
--Update bundles
```
bundle update
```
--Build the site
```
bundle exec jekyll serve watch
```
If using Ubuntu you can either copy the server address and paste it into a browser or right click on the link and choose
"open link".  

You should now see a very basic looking Jekyll blog, that uses the minimal-mistakes theme.

## Step 8

While in the minimal-mistakes directory, copy configuration file named *_config.yml* to *_config_dev.yml*.

Make the following edits to the *_config_dev.yml* file.

```
url     : "http://localhost:4000/"
```
## Step 9

Now when working locally you can issue the following command.
```
bundle exec jekyll serve --config _config_dev.yml
```
note: When working locally you might see the following error when previewing.
```
ERROR `/favicon.ico' not found.
```
This error appears to be harmless and doesn't impact the use of the site or cause any errors that I have seen when deploying to github.

## Step 10

Make the appropriate edits to the *_config.yml* and *_config_dev.yml*.  
For the purposes of this tutorial I will not discuss all of the options in this file. They are described well in the minimal-mistakes documentation.
After making edits run the following command to be sure all is well.

```
bundle exec jekyll serve --config _config_dev.yml
```
## Step 11

Make the appropriate edits to *_config.yml* based on your github account and the repository you created.

The following settings are used if you are creating a sub site or gh-pages site.
```
url     : "https://shoreviewanalytics.github.io"
baseurl : "/minimalmistakesjekylldemo"
```

To ensure the site still compiles run the following commands.
```
bundle exec jekyll --config _config.yml serve
```

Now that you have a basic blog minus any customizations that you will want, you can publish it to github.

There are two different locations where you can publish your blog on github.  

1. The root of your github account also known as the master.  This is when you want to view your blog at a web address like I am doing at shoreviewanalytics.github.io.

2. You can also serve up your blog using a project site also known as gh-pages.  This is when you want to view your blog at an address like shoreviewanalytics.github.io/minimalmistakesjekylldemo.

Note: You can only have one site published at the master level.  You might be able to have multiple repositories at the master level, but github will not recognize multiple jekyll blogs at the root level.  

If you want to setup your blog so it is served at githubusername.github.io just don't change the default branch as described below.  

If you are following along and only want to demo the minimal-mistakes theme you can create a new repository on github called minimalmistakesjekylldemo.  After creating the repository make sure you place it in a branch adjacent to master by creating a new branch called 'gh-pages'. To do this you will need to create the branch gh-pages by clicking on branch drop down of the minimalmistakesjekylldemo repository and then typing in the name gh-pages.  Once you have done this click on settings and go to branches and set this repository to the default branch of gh-pages. Make sure you do not have anything in the directory before making this change, otherwise it will all be gone after this change.  :)

Next, on your local computer open a terminal session in your sandbox root folder to clone the new gh-pages repository by issuing the following command.

```
git clone https://github.com/yourgithubusername/minimalmistakesjekylldemo.git
cd minimalmistakesjekylldemo
```
If you want to access your blog at githubusername.github.io, then you would create a new repository called githubusername.github.io.
```
git clone https://github.com/yourgithubusername/yourgithubusername.github.io.git
cd yourgithubusername.github.io
```
Regardless, each folder will be empty except for a .git folder.  


## Step 12

Next, copy the all the content minus the .git folder from the forked minimal-mistakes repository called "minimal-mistakes".  Remember the folder options above. You will either be creating a new repository at the master level or gh-pages level.

## Step 13

Using a terminal session in the root of the new repository on you local machine, issue the following command to make sure the the site is still functional.
```
bundle exec jekyll serve
```

You should be able to navigate to the site locally.

## Step 14

Now you can push minimalmistakesjekylldemo or githubusername.github.io to the repository created in step 11.

Issue the following commands if you want to push to gh-pages.
```
git add .
git commit -m "first minimalmistakesjekylldemo"
git push -u origin gh-pages
```
Issue the following commands if have a repository named githubusername.github.io.
```
git add .
git commit -m "first githubusername.github.io"
git push -u origin master
```

## Summary

Congratulations! You should now have a working jekyll blog using the minimal-mistakes theme that is served up at the root or master level or the gh-pages level of your github account.
Of course this is just the beginning to all the fun.  As you can see there are many options and features to explore.

After initial deployment of my blog using the fork method, I switched gears to the gem method, so I could take advantage of major changes to the theme and have a more tidy repository for my blog. One of my requirements above was to have a blog that I could easily maintain. If you are in a hurry and want to just get started with the gem method to use the minimal mistakes theme, you can  [fork](https://github.com/shoreviewanalytics/shoreviewanalytics.github.io "fork") my repository.


## Markdown Reference:

[Markdown-Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet "Markdown-Cheatsheet")

[Markdown Style Guide](http://www.cirosantilli.com/markdown-style-guide/ "Markdown Style Guide")
