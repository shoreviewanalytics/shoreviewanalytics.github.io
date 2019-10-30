---
date: "2018-10-08"
title: "Spark Job Specific Logging with Logback and DataStax Enterprise Analytics"
layout: single
classes: wide

header:  
  overlay_image: /assets/images/abstract.jpg

---

--------------------------------------------

This post takes a look at how use Logback, the successor to Log4j with your spark application to create application specific logging. If you have spent any amount of time with Apache Spark you will notice that there is a ton of logging that goes on, but that logging is usually limited to the master or worker processing that occurs on each node in your cluster.  While this information is important it doesn't tell you a whole lot about how your specific application is doing.  Also if you have system wide settings for logging of your spark applications you may have to sift through one log for all your applications. The example code allows you to create a log file for each of your spark applications and allows you to control the level of logging.  

Prerequisites:
---------------

- Spark development environment (This example uses Eclipse) or ability to compile source code to a .jar file with all necessary dependencies.  

- An available Spark cluster to run the example (This example uses DataStax Enterprise Analytics Cluster), but could tweak the example code to run on a standard Apache Spark cluster.

### Step 1

Clone the following repository to your workstation using the following command.
```
git clone https://github.com/shoreviewanalytics/sparkjobspecificlogging.git
```
### Step 2

Launch the project using Eclipse or your favorite IDE and make the necessary adjustments to the code (for example the path to the .csv), then compile and export it to a .jar file.

### Step 3

This step is optional if you are not working on a distributed environment. If you are working on a distributed computing environment you can add the .csv to an accessible path to the user running the spark job to each node in your cluster.  Ex. home/username/data a the words.csv file would be place in this location for each node in the cluster.    

### Step 4

Run the example using a command similar to following command. The exact syntax of this command depends on the platform you are using, the location of your logback.xml file, the location of the .jar file (your spark job) and the configuration of your DataStax / cluster environment.    
```
dse -u username -p password spark-submit --class com.java.spark.LoggingSample --master dse://? --driver-java-options "-Dlogback.configurationFile=/pathtoyour/logback.xml" /path to your jarfile/LoggingSample.jar
```
## Summary

This example is done in Java, but could have just as easily been using Scala or Python, since the majority of the logging setup is done via spark-submit and the logback.xml file and the code is just a revamp of the word count application. As you review the code you will see there are logging statements for transformations and RDDs. The most important part of this example is Step 4 where you can explicitly reference a logback.xml file in the spark-submit.  What this does is allow you to configure logging for the job you are running, by explicitly setting the log file directory and the level of logging for the log file.  There's also an experimental class (SysStreamsLogger) included in this example (which I did not write, but did amend for the purpose of the example).  The SysStreamsLogger.java class allows you to capture additional information from the console to add to the log file as the spark job is running.  Entries in the log file will have the logger name of SysStreamLogger.java when information is captured from the console and redirected by this class to the log file. Another important part of this example is the ability to change the various logging levels (see the end of the logback.xml file included in this example).  These settings allow you to have more granular control of logging, so you can have a very concise log possibly for production or a very verbose log possibly for development or troubleshooting.  

In closing there are tons of features available with logback and this example is just scratches the surface. You can find detailed information on the logback features on the project's [website]("https://logback.qos.ch/").  You can find more information about DataStax Enterprise Analytics [here]("https://www.datastax.com/products/datastax-enterprise-analytics").  
