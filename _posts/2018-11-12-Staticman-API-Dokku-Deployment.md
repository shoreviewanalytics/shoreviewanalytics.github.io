---
date: "2018-10-08"
title: "Staticman API Dokku Deployment"
layout: single
classes: wide

---

Using Dokku to deploy the Staticman API
------------------------------------

If your reading this post, you have a desire to deploy the Staticman API as a stand alone application on premise or on a cloud based infrastructure. Over the past several months using the public Staticman API has become more and more difficult to use with a Jekyll blog on github pages, most likely due to usage policies on github.  

This post relates to how to get Staticman API up and running using the digitalocean infrastructure using an Ubuntu based droplet with Dokku pre-configured. Setting up my own instance of Staticman API means it will only service my blog, so hopefully because I don't generate a whole lot of traffic, it will stay well within the usage levels set by github.    

Setup Needs:
--------------

- A domain name and a way to manage it, so you can create a subdomain like staticman.shoreviewanalytics.com.          

- Create an account with digitalocean to create a Dokku droplet or similar setup.

- Create a Dokku droplet with your digital ocean account.  

- Create an additional github account.  This account will be the collaborator to your blog repository and will be used to create a github token.

- Install npm on your workstation, so you can run the tests provided with Staticman and also install json to format of .json files.  


Now the fun begins.  If you have all the above and are familiar with git, you should be able to work towards a successful deploy of Staticman API to a digitalocean Dokku droplet using the following steps.  

**Step 1**

Create a Dokku droplet and do the basic server configuration.  Also use your domain to create a subdomain that points to this droplet.
After you complete this step you should be able to ssh to your droplet using at least one account and you should be able to ping your droplet at staticman.yourdomain.com.

**Step 2**

Fork the Staticman API repository.  I actually forked a repository that had already been forked from the original Staticman API.  I did this because after doing some recon, I found that there was only one example of someone who had successfully deployed a standalone version of Staticman API using Dokku.  After trying to deploy a few times using the vanilla repository and failing, I wanted to start from a version of the code that had been successfully deployed.  You probably don't have to do that as I believe the main to do is to fork the original repository and then remove all the Docker related files.  

**Step 3**

Clone your forked version of Staticman API located under your main github account to your local environment.

**Step 4**

Create a new local branch for the cloned repository that you forked on your workstation.  Don't forget to checkout your new branch for your edits. Also, if you didn't fork a repository that had the docker files removed, then go ahead and remove them now and then update your local branch repository.  This step is really important since to deploy Staticman you will need to have a github token and a ssh key to add to the code. The goal here, at least based on my understanding is that you don't want to push any secrets out to github.  If you do, you will get a nice little message from them saying you have sensitive information like a github token and they will disable it so you will have to create another one.  

**Step 5**

Use your second github account and add it as a collaborator to your github pages blog repository.  Also create a github token from this account to be used in the config.production.json file in Step 6.       

**Step 6**  

If you look at the root directory in your staticman folder you will see a file called config.sample.json.  Copy this file to a file named config.production.json. Now that you have this new file you can edit it using the github token and a valid ssh key.  Github provides the documentation on how to create a token and how to create an ssh key.  I found that converting the standard key to .pem file works best.  After creating the config file and obtaining the token and key add them to this file.  Just be sure to use the ssh key from your github account that holds your blog and use the token from the second github account. Also when creating a ssh key, you should create one without a passphrase.  

**Step 7**

Now you should be close to pushing Staticman API, your forked / cloned version of Staticman that you've edited in your local branch to your Dokku droplet. Commit the changes made to config.production.json in the local branch.

**Step 8**

Go to the Dokku droplet and create a Dokku application called staticman in all lower case letters.  

```
dokku apps:create staticman
```

**Step 9**

Now setup a remote git repository for your Doku server on your workstation, by using the following command.  
```
git remote add dokku dokku@yourserver:staticman
```
You can check to see what remote repositories you can push to using this command.

```
git remote -v
```
Push the local branch you created earlier to your remote repository on your Dokku droplet.  Notice you are NOT pushing the master branch that you haven't updated.
```
git push dokku local:master
```
This is where you find out if you have correctly edited the config.production.json file.  If you are in doubt on whether you have a good format for your config.production.json file use the following command to check it.

```
json -f config.production.json  
```

**Step 10**

This is where it really gets cool.  You can now push Staticman to your Dokku droplet with the following command assuming you named your local git branch "local".  

```
git push dokku local:master  
```

Now if all goes well you will have pushed your local branch of Staticman to your Dokku server.  If you have your DNS setup correctly you should be able to navigate to your Staticman API deploy by opening up a browser and typing in staticman.yourdomain.com.  For me it's staticman.shoreviewanalytics.com.  

**Step 11**

Now that you have Staticman up and running you need to activate it so it can work with your blog.  To do this as provided in Staticman API documentation navigate to the an address like the following that makes sense for your deployment.  

```
http://staticman.yourdomain.com/v2/connect/yourgithubusername/yourgithubpagesblog.github.io  
```

If all goes well you should get a response of 'OK'.  If you don't there's something wrong and you'll need to dig in a bit.  In my case I had to dig in a lot.

**Step 12**

Now that you have a working deploy of Staticman so you can navigate to it as in Step 9, it's time to add the ability to navigate to the Staticman URL using HTTPS.  To do this you need to issue the following commands on your Dokku droplet server.  Essentially, if you don't add this you won't be able to use your comments so don't skip it.
This step depends on dokku-letsencrypt.  

first get it

```
	dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git
```
set an email address for notifications
```
	dokku config:set --no-restart staticman DOKKU_LETSENCRYPT_EMAIL=your email address goes here
```
enable it

```
	dokku letsencrypt staticman
```

set up auto-renewal:

```
	dokku letsencrypt:cron-job --add
```

Well, this post doesn't include all the details you will need, but if you do your research by reviewing the references below and are able to complete the above steps, you should be able to get a working deployment of the Staticman API on a digitalocean Dokku droplet.  If you to try this out and get stuck and want some clarification, guess what, you can use the comments form included on this page.  

References:
------------

[Effortlessly add HTTPS to Dokku, with Let’s Encrypt](https://medium.com/@pimterry/effortlessly-add-https-to-dokku-with-lets-encrypt-900696366890 "Effortlessly add HTTPS to Dokku, with Let’s Encrypt")

[Setting up Staticman for comments on a Jekyll blog](https://www.flyinggrizzly.net/2017/12/setting-up-staticman/ "Setting up Staticman for comments on a Jekyll blog")

[Improving static comments with Jekyll & Staticman](https://mademistakes.com/articles/improving-jekyll-static-comments/ "Improving static comments with Jekyll & Staticman")

[Deploying to Dokku](http://dokku.viewdocs.io/dokku~v0.12.13/deployment/application-deployment/ Deploying to Dokku")
