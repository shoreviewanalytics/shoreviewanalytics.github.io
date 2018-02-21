---
layout: single
title: 'Blog using Jekyll with Minimal-Mistakes Ubuntu 16.04'
date: '2018-02-19'
---

A few years ago, I had a wordpress site hosted on one of the more well known hosting sites.  I decided that I wanted to update it or possibly look at one of my posts, so I open up a browser and navigated to my site and to my surprise it was not there.  I logged into the hosting admin area and looked around and there was nothing there either.  I called support and asked if they had any backup of it and again the there was nothing. Apparently there was a mix up with my site, which led to it being decommissioned, otherwise known as deleted.

Recently, I discovered that I can use [Jekyll](https://jekyllrb.com/ "Jekyll") on [Github](https://github.com/ "Github") to publish a blog.  Having a Jekyll blog or deploying a blog to Github is nothing new, but for me the control that I have over my the content and hosting is real value. As long as I work on my blog locally, and back it up periodically, I should always be in the drivers seat for my blog content and not lose it if a hosting company decides to delete it.

Here are the requirements I had in mind for my new blog.  

1. It should be portable and not dependent on a specific web hosting company platform, especially the content.   
2. The theme used should (hopefully), be actively maintained.
3. It should be relatively easy to maintain, have advanced features and customizable.
4. The theme and software should be able to be updated.
5. It should not cost me much to deploy.

Note: If you host your blog using Github and abide by their guidelines you can deploy your blog for no cost. So that helps me meet one of my requirements.     

Note: There are lots of great tutorials on how to use Jekyll to create a blog. I started with the following tutorial.  

[Tania's tutorial](https://www.taniarascia.com/make-a-static-website-with-jekyll/ "Tania's Tutorial")

The good news is that I encountered very few issues installing the software that is used to build a Jekyll site on Ubuntu 16.04. However, when it comes to deploying a Jekyll blog using a more advanced theme I had to go through some trial and error and did a fair amount of research.  

The following steps are how I created a Jekyll powered blog using the [minimal-mistakes](https://mmistakes.github.io/minimal-mistakes/ "minimal-mistakes") theme hosting it using [github pages](https://pages.github.com/ "github pages"). If you review the quick start guide for the minimal-mistakes theme, it provides three different methods to install. This post uses the fork method. For QA, I have gone through the steps below to build the mininmalmistakesdemo site using these steps at least two times to be sure they are accurate and lead to success.

I have not used these steps to build a githubusername.github.io site, because I already have my blog published at that location.  My assumption is you can create an empty repository at the master level and then demo various Jekyll themes as sub sites as described in this tutorial.    

After the initial deployment of my blog using the fork method, I switched gears to the gem method so I could take advantage of major changes to the theme and have a more tidy repository for my blog. One of my requirements (item 3) above was to have a blog and theme that I could easily maintain. In a future post I will describe the initial setup of a jekyll site using the minimal-mistakes with the gem method, or you can just fork my repository.

Please Note: It will be necessary to move slowly and do all the tasks in each step.  As a general warning, some things like the creation of a gh-pages layer on Github (Step 10), if you are not an expert with Github, can be a bit confusing.

When you complete this tutorial you should have a fully functional Jekyll blog using the minimal-mistakes theme as displayed at the following web address.

[https://shoreviewanalytics.github.io/minimalmistakesjekylldemo/](https://shoreviewanalytics.github.io/minimalmistakesjekylldemo/ "minimal-mistakes theme demo")

## Step 1

Install Ruby using the following command:
```
sudo apt-get install ruby
```

Install Bundler
```
gem install bundler
```
Install Git using the following command:
```
sudo apt-get install git
```

Please note: If you currently do not have a Github account you will need to create one to complete this tutorial.  

## Step 2

After you have a setup your github account, create a new repository on github, using the following naming conventions.

Create a repository with a name of githubusername.github.io if you want a web address like:

https://githubusername.github.io.

Create a repository with a name minimaljekylldemo if you want a web address like:

https://githubusername.github.io/minimaljekylldemo.

Note: Creating a repository defaults to the root or master level and you can only have one Jekyll site at this level.  All other sites must be placed at the gh-pages level. For example, if you are reading this post and you already have a blog like I do at http://shoreviewanalytics.github.io you would create a new repository called minimaljekylldemo and would navigate to it with the following url.  https://shoreviewanalytics.github.io/minimaljekylldemo.  I will get into the details of how this is done and why below.  The main point of this step is to make sure you create an account on github.com and also create the desired repository.  

## Step 3

Create a new folder in your home directory to use as a sandbox for your Jekyll efforts.  I called mine websites but you can call it whatever suites your needs.    

## Step 4

Next you will need to fork the minimal-mistakes theme.  To do this you will need to login to your github account and then search for the minimal-mistakes repository.  Once you have found the minimal-mistakes repository, click on the fork icon in the upper right hand corner of the repository.  Once the fork process has completed you will have the minimal-mistakes repository as a fork under your github account.   

## Step 5

Now that you have the minimal-mistakes repository forked to your github account, you can clone it to the folder created in Step 3.

Click on the green button that says Clone or download and then click on the copy icon to copy the URL to your clipboard. Issue the following command in a terminal when in the root of your sandbox folder.

```bash
git clone https://github.com/shoreviewanalytics/minimal-mistakes.git  
```

You should now have the minimal-mistakes repository on your local machine.

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

Using the command line from the root of the minimal-mistakes directory issue the following commands

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

You should now see Jekyll site using the minimal-mistakes theme.

## Step 8

While in the minimal-mistakes directory, copy the current _config.yml to _config_dev.yml

Make the following edits to the _config_dev.yml file.

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

Make the appropriate edits to the _config.yml and _config_dev.yml.  
For the purposes of this tutorial I will not discuss all of the options in this file. They are described well in the minimal-mistakes documentation.
After making edits run the following command to be sure all is well

```
bundle exec jekyll serve --config _config_dev.yml
```
## Step 11

Make the appropriate edits to _config.yml based on your github account and the repository you created.

The following settings are used if you are creating a sub site or gh-pages site.
```you write them
url     : "https://shoreviewanalytics.github.io"
baseurl : "/minimalmistakesjekylldemo"
```

To ensure the site still compiles run the following commands.
```
bundle exec jekyll --config _config.yml serve
```

Publishing your blogyou write them

Now that you have a basic blog minus any customizations that you will want you can publish it to github.

There are two different locations where you can publish your blog on github.  

1. The root of your github account also known as the master.  This is when you want to serve your blog to an address like I am doing at shoreviewanalytics.github.io

2. You can also serve up your blog using a project site also known as gh-pages.  This is when you already have a blog located at an address like shoreviewanalytics.github.io but your trying to share something with others as I am in writing this post to demo how to create a jekyll using the minimal-mistakes theme.  

Note: You can only have one site published at the master level.  You might be able to have multiple repositories at the master level, but github will not recognize multiple jekyll blogs at the root level.  This makes sense if you think about the wayou write themy websites work.   

If you want to setup your blog so it is served at githubusername.github.io just don't change the default branch as described below.  you write them

If you are following along and want to demo the minimal-mistakes theme go ahead and create a new repository on github called minimalmistakesjekylldemo.  After creating the repository make sure you place it in a branch adjacent to master by creating a new branch called 'gh-pages'. To do this you will need to create the branch gh-pages by clicking on branch drop down of the minimalmistakesjekylldemo repository and then typing in the name gh-pages.  Once you have done this click on settings and go to branches and set this repository to the default branch of gh-pages. Make sure you do not have anything in the directoyou write themry before making this change otherwise it will all be gone after this change.  :)

Next, on your local computer open a terminal session in your sandbox root folder to clone the new gh-pages repository by issuing the following command.

```
git clone https://github.com/yourgithubusername/minimalmistakesjekylldemo.git
cd minimalmistakesjekylldemo
```
If you want to access your blog at githubusername.github.io, then you would create a new repository called githubusername.github.io. Next
```you write them
git clone https://github.com/yourgithubusername/yourgithubusername.github.io.git
cd yourgithubusername.github.io
```
Regardless, each folder will be empty except for a .git folyou write themder.  


## Step 12

Next, copy the all the content minus the .git folder from the forked minimal-mistakes repository called "minimal-mistakes".  Remember the folder options above. You will either be creating a new repository at the master level or gh-pages level.
you write them
## Step 13

Using a terminal session in the root of the new repository on you local machine, issue the following command to make sure the the site is still funcyou write themtional.
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
git push -u origin masteryou write them
```

You should now have a working jekyll blog using the minimal-mistakes theme that is served up at the root or master level or the gh-pages level.
Of course this is just the beginning to all the fun.  As you can see there are many options and features to explore.

Congratulations! If you made it to this point in the tutorial successfully, you should consider yourself  AWESOME!

## Tip:

To assist with creating posts, I highly recommend installing Atom, a powerful text editor that is markdown aware.  Atom let's you preview your posts as you write them.  

To install Atom on Ubuntu 16.04, use the following commands:

```
sudo add-apt-repository ppa:webupd8team/atom
sudo apt update; sudo apt install atom
```

## Markdown Reference:

[Markdown-Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet"Markdown-Cheatsheet)
