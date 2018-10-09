---
date: "2018-10-08"
title: "Spark Job Specific Logging with Logback and DataStax Enterprise Analytics"
layout: single
classes: wide

---

Spark Job Specific Logging with Logback and DataStax Enterprise Analytics
------------------------------------

This post takes a look at how use Logback, the successor to Log4j with your spark application to create application specific logging. If you have spent any amount of time with Apache Spark you will notice that there is a ton of logging that goes on, but that logging is usually limited to the master or worker processing that occurs on each node in your cluster.  While this information is important it doesn't tell you a whole lot about how your specific application is doing across your cluster. With logging setup specifically for the spark job running, you can obtain granular information about how it is running on the spark cluster and detailed  [DAG](https://stackoverflow.com/questions/25836316/how-dag-works-under-the-covers-in-rdd) information as well.  In addition, if there are system wide settings for logging of spark applications you may have just one log for all your applications.  This example shows how to create a log file for each spark job and how to use verbose logging for development and then turn it down for production to only log what you really need to log, like errors and basic feedback.    

Prerequisites:
---------------

- Spark development environment (This example uses Eclipse) or have ability to compile source code to a .jar file with all the necessary dependencies.  

- An available Spark development environment.  Access to an Apache Spark cluster is ideal if you want to run this example and collect information related to the job running on the cluster.     

Step 1

Clone the following repository to your workstation using the following command.
```
git clone https://github.com/shoreviewanalytics/sparkjobspecificlogging.git
```

Step 2

Compile the code to a .jar file using your favorite IDE or compiler after making necessary adjustments. Ex. You will need to change the path to the words.csv.  

Step 3

This step is optional if you are not working on a distributed environment. If have access to a cluster environment, you can add the words.csv to an accessible path to the user running the spark job on each node in your cluster.  Ex. home/username/data/words.csv     

Step 4

Run the example using a command similar to following command. The exact syntax of this command depends on the platform you are using, whether you have security enabled or not, the location of your logback.xml file, the location of the .jar file (your spark job) and the configuration of your cluster environment.  Also the following command is separated into multiple lines just for this blog post and should be run as a single line in a terminal window.    

```
dse -u username -p password spark-submit
--class com.java.spark.LoggingSample
--master dse://?
--driver-java-options "-Dlogback.configurationFile=/pathtoyour/logback.xml"
/path to your jarfile/LoggingSample.jar
```

Step 5

Look for the following property in the logback.xml file.  

 <logger name="org.apache.spark" level="INFO"/>

 When this property is set to INFO the log will contain DAG information and more details about the job running.   

If you set the level to OFF, the output is reduced significantly.  

 <logger name="org.apache.spark" level="OFF"/>


Summary

This example shows you how to control the amount of logging in your spark application and how to create a log per application. The logging setup is done via spark-submit and the logback.xml file and the code itself is just a revamp of the word count application. As you review the code you will see there are logging statements for transformations and RDDs. The most important part of this example is Step 4 where you can explicitly reference a logback.xml file in the spark-submit.  What this does is allow you to configure logging specifically for the job you are running, by explicitly setting the log file directory and the level of logging for the log file.  

There's also a class called SysStreamsLogger included in this example (which I did not write, but did amend for the purpose of this example).  The SysStreamsLogger.java class redirects the output normally only available through the console to the specified log file set in logback.xml as the spark job is running.  Entries in the log file will have the logger name of SysStreamLogger.java when information is captured from the console and redirected by this class to the log file. Another important part of this example is the ability to change the various logging levels (see the end of the logback.xml file included in this example).  These settings allow you to have more granular control of logging, so you can have a very concise log possibly for production or a very verbose log possibly for development or troubleshooting.  

Logback is a very flexible and powerful logging mechanism with numerous features.  To learn more about Logback check out the project's [website](https://logback.qos.ch/).  For more information about DataStax Enterprise Analytics check out the [DataStax Enterprise Analytics  ](https://www.datastax.com/products/datastax-enterprise-analytics) web page on the DataStax website.
