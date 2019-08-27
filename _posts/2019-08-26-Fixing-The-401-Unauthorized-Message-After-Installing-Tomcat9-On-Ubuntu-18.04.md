---
layout: single
title: 'Fixing The 401 Unauthorized Message After Installing Tomcat 9 On Ubuntu 18.04'
date: '2019-08-26'
classes: wide

header:
  overlay_color: "#5e616c"
  overlay_image: /assets/images/abstract.jpg
---

--------------------------------------------

Apache Tomcat® is an open source implementation of Java Servlet, JavaServer Pages, Java Expression Language and Java WebSocket technologies. This guide explains how to setup Apache Tomcat® 9 on Ubuntu 18.04 and essentially follows the same steps that you can find in other posted tutorials (see reference below) .  It also helps to clarify the security problems that can occur after install, when you try to access the Server Status, Manager App or the Host Manager buttons on the Tomcat home page. 

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/buttons.png" alt="buttons">

You may have installed Tomcat 9 and it is up and running, but when you click one one of the above buttons you get prompted by the authentication required form, where you need to enter a username and password.  

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/credentials.png" alt="credentials">

This prompt won't go away because Tomcat doesn't recognize the username and password associated with a role that can access these administrative applications.  If you click the "Cancel" button you get.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/401.png" alt="401 Unauthorized">

### Prerequisites

In order to proceed with this guide it is necessary to have non-root user with sudo privileges setup on your server or desktop install of Ubuntu.  

## Step 1 Install Java

```
sudo apt update
sudo apt install default-jdk
```

You can check the version of Java using the following command.

```
java --version
```

You should see output like the following.

```
openjdk 11.0.4 2019-07-16
OpenJDK Runtime Environment (build 11.0.4+11-post-Ubuntu-1ubuntu218.04.3)
OpenJDK 64-Bit Server VM (build 11.0.4+11-post-Ubuntu-1ubuntu218.04.3, mixed mode, sharing)
```

## Step 2 Create The Tomcat User and Group

For security purposes it is best to run Tomcat with a non-root user. Use the following commands to create a "tomcat" group and user.  

First create the Tomcat group.

```
sudo groupadd tomcat
```

Now, create a new tomcat user and add it to the newly created tomcat group, setting the -d flag to the home directory of /opt/tomcat (the Tomcat installation directory). Set the -s flag to /bin/false (so nobody can log into the account):

```
sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
```

## Step 3 Install Tomcat

To install the latest version of Tomcat go to the download page for Tomcat 9.

https://tomcat.apache.org/download-90.cgi

Once on this page you can hover over the download link for the tar.gz download. At this time the version is 9.0.24.

Open up terminal window and cd to Downloads directory.

`cd Downloads`

Next issue the following curl command. Be sure that the curl command includes the most recent version of Tomcat 9.  

```
curl -O http://mirror.cc.columbia.edu/pub/software/apache/tomcat/tomcat-9/v9.0.24/bin/apache-tomcat-9.0.24.tar.gz
```

Next create a directory for Tomcat.

```
sudo mkdir /opt/tomcat
```

Now extract and copy the content of the extracted tar.gz file to /opt/tomcat using the following command.

```
sudo tar xzvf apache-tomcat-9*tar.gz -C /opt/tomcat --strip-components=1
```

Next cd into the /opt/tomcat directory.

```
cd /opt/tomcat
```

Give tomcat permission to the entire /opt/tomcat directory.

```
sudo chgrp -R tomcat /opt/tomcat
```

Next give read to the conf directory and all its content and execute access on the directory itself.

```
sudo chmod -R g+r conf
sudo chmod g+x conf
```

Now make the tomcat user the owner of the /webapps, /work, /temp and /logs directories.  

```
sudo chown -R tomcat webapps/ work/ temp/ logs/
```

## Step 4 - Setting Up The Tomcat Service

In order to have tomcat available as a service it is necessary to create a systemd service file.  The service file contains a line that points to the jdk installed in step 1.  To obtain the path to the jdk use the following command.

```
sudo update-java-alternatives -l
```

You should see output like this.  

```
java-1.11.0-openjdk-amd64      1111       /usr/lib/jvm/java-1.11.0-openjdk-amd64
java-8-oracle                  1081       /usr/lib/jvm/java-8-oracle
```

If you have more than one jdk on your system like I do you will see a few lines.  That's not an issue but you will need the path in the first line for the systemd service file.  

Next create a systemd service file for tomcat.  

```
sudo vi /etc/systemd/system/tomcat.service
```

Insert the following lines into the newly created service file. And make sure that the Environment=JAVA_HOME= points to the location where you have the jdk installed in step 1.

```
[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/java-1.11.0-openjdk-amd64
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```

Save and close the tomcat.service file and then reload the systemd daemon.

```
sudo systemctl daemon-reload
```

Next start tomcat.  

```
sudo systemctl start tomcat.service
```

Check the status.

```
sudo systemctl status tomcat.service
```

## Step 5 - Adjust Firewall and Test

Tomcat uses port 8080 by default.  To allow port 8080 on a server with ufw enabled, execute the following command.

```
sudo ufw allow 8080
```

To check to see that this rule is active using this command.

```
sudo ufw status
```

You should see output similar to the following.

```
Status: active

To                         Action      From
--                         ------      ----
8080                       ALLOW       Anywhere                  
8080 (v6)                  ALLOW       Anywhere (v6)  
```

Next navigate to your servers localhost:8080 or your servers IP address:8080.  You should see the home page for Tomcat.  

## Step 6 - Configure Roles, Users

In order to access the administrative pages for Tomcat it is necessary to login with administrative credentials.  To do this it is necessary to define a user and password in the tomcat-users.xml file as follows.  

Open the tomcat-users.xml file.

```
sudo vi /opt/tomcat/conf/tomcat-users.xml
```

Uncomment the lines as shown below.

```
<!--
  <role rolename="tomcat"/>
  <role rolename="role1"/>
  <user username="tomcat" password="<must-be-changed>" roles="tomcat"/>
  <user username="both" password="<must-be-changed>" roles="tomcat,role1"/>
  <user username="role1" password="<must-be-changed>" roles="role1"/>
-->
```

You should now have the following lines uncommented.

```
<role rolename="tomcat"/>
<role rolename="role1"/>
<user username="tomcat" password="<must-be-changed>" roles="tomcat"/>
<user username="both" password="<must-be-changed>" roles="tomcat,role1"/>
<user username="role1" password="<must-be-changed>" roles="role1"/>
```

Next change the default lines above to the following three lines.

```
<role rolename="manager-gui"/>
<role rolename="admin-gui"/>
<user username="username goes here!" password="password goes here!" roles="manager-gui,admin-gui"/>
```

Notice that there are now two lines for roles and one user line.  You can add more to the configuration later if needed but this is enough to get started.  

Next, add a username and a password where you see "username goes here!" and "password goes here!".  

Save and close the file when done.

## Step 7 - Granting Access To "Manager App"

After adding an admin user and password in Step 7 you are set to configure access to the Manager application.

First open the configuration file for the Manager application.

```
sudo vi /opt/tomcat/webapps/manager/META-INF/context.xml
```

Edit the the following line by adding the IP address of the server to end.

```
<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
```

The changed line should look something like the following, where the "| IP address of your server goes here!" matches your server's IP address.  Note it is necessary to add the pipe symbol to delimit between the entries of allowed IP addresses.   

```
<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1|IP address of your server goes here!" />
```

Next save the file and exit.

## Step 8 - Granting Access To "Host Manager"

First open the configuration file for the Host Manager Application.

```
sudo vi /opt/tomcat/webapps/host-manager/META-INF/context.xml
```

Complete the same activities as done in Step 7 above for this file.

Save the file and then exit.

## Step 9 - Trying it out

After all your hard work it's time to check to see if all is working as expected.  To put the above changes to work it is necessary to restart tomcat.

```
sudo systemctl restart tomcat
```

Next, close your browser and then relaunch it and then navigate to the Tomcat home page.

http://localhost:8080

or `http://your_servers_ip:8080`

Click on the button that says "Server Status".  You should be prompted for the username and password you entered in Step 6.  

## Conclusion

Well, that's all there is to it, you should now have Tomcat 9 successfully installed on your Ubuntu 18.04 server or desktop and be able to login and view the Server Status, view the Host Manager or the Manager App.  You can experiment around with additional users and roles, try the examples provided and of course there's always the documentation.   

## References

https://www.digitalocean.com/community/tutorials/install-tomcat-9-ubuntu-1804
