---
date: "2018-10-08"
title: "Spark Logback Example Two"
layout: single
classes: wide

header:  
  overlay_image: /assets/images/header.jpg
---

--------------------------------------------

This post is relates to my other [post](https://shoreviewanalytics.github.io/Spark-Logback-Example-One/) on how to use Logback, the successor to Log4j with your spark application to create job specific logging when using the DataStax Enterprise (DSE) Analytics platform. Essentially everything is the same for this example, except you don't need to pass the location of the logback.xml file in the spark-submit and this example also let's you pass the job name to the logback.xml file, so the name of the log file is the same as the name of the job.   

Prerequisites:
--------------

- A Java development environment (This example uses Eclipse).  

- An available DSE Analytics development environment.       

**Step 1**

Clone the following repository to your workstation using the following command.
```
git clone https://github.com/shoreviewanalytics/spark-logback-example-two.git
```

**Step 2**

Compile the code to a .jar file and adjust the path to the .csv file. For example, it will be necessary to change the path to the words.csv.  

**Step 3**

This step is optional if you are not working on a distributed environment. If have access to a local or cloud based DSE cluster, you can add the words.csv to an accessible path to the user running the job on each node in your cluster.  For example,  home/username/data/words.csv.  It is not necessary to have this file as you can just create your own file.csv and then just create a list of words with a random number of duplicates.      

**Step 4**

Run the example using dse spark-submit.  Notice that the option --driver-java-options "-Dlogback.configurationFile=/pathtoyour/logback.xml" is no longer in the spark-submit command.  This command is separated into multiple lines for formatting in this post and should be run as a single line in a terminal window.    

```
dse -u username -p password spark-submit
--class com.java.spark.LoggingSample
--master dse://?
/path to your jarfile/LoggingSample.jar
```

**Step 5**

This is where the fun happens.  Look for the following property in the logback.xml file.  

```
 <logger name="org.apache.spark" level="INFO"/>
```
 When this property is set to INFO the log will contain DAG information and more details about the job running.   

If you set the level to OFF, the output to the log file is significantly reduced, but the explicit logging messages you have added to your code will still be logged.    

```
 <logger name="org.apache.spark" level="OFF"/>
```

**Summary**

You can experiment with the various ways to configure a logback.xml file to add or reduce the verbosity of your logging. To learn more about Logback and all of its features check out the project's [website](https://logback.qos.ch/). Also note that this example includes a class called SysStreamsLogger, which I did not write, but did amend for the purpose of this example.  The SysStreamsLogger.java class redirects the output normally only available through the console to the specified log file set in logback.xml as the spark job is running.  Entries in the log file will have the logger name of SysStreamLogger.java when information is captured from the console and redirected by this class to the log file.

To test out this logging example on a distributed computing environment, I used a three node cluster with DataStax Enterprise Analytics enabled. For more information about DataStax Enterprise Analytics check out the [DataStax Enterprise Analytics  ](https://www.datastax.com/products/datastax-enterprise-analytics) page on the DataStax website. If you have questions or feedback regarding this post, you can contact me using the comments form below.  
