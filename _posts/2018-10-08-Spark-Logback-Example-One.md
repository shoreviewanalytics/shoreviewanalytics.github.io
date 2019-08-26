---
date: "2018-10-08"
title: "Spark Logback Example One"
layout: single
classes: wide

---

Spark Logback Example One
------------------------------------

This post takes a look at how use Logback, the successor to Log4j with your spark application to create application specific logging. If you have spent any amount of time with Apache Spark you will notice that there is a ton of logging that goes on, but that logging is usually limited to what spark as an application is doing so there's lot's of logging about spark master or worker processing and what the executors are doing. While this information is important it doesn't tell you a whole lot about how your specific application is doing across your cluster. With logging setup specifically for the spark job running, you can obtain granular information about how it is running on the spark cluster and detailed  [DAG](https://stackoverflow.com/questions/25836316/how-dag-works-under-the-covers-in-rdd) information as well.  In addition, if there are system wide settings for logging of spark applications you can have just one log for all your applications.  This example shows how to create a log file for each spark job and how to use verbose logging for development and then turn it down for production to only log what you really need to log, like errors and basic feedback.    

Prerequisites:
--------------

- Spark development environment (This example uses Eclipse) or have ability to compile source code to a .jar file with all the necessary dependencies.  

- An available Spark development environment.  Access to an Apache Spark cluster is ideal if you want to run this example and collect information related to the job running on the cluster.     

**Step 1**

Clone the following repository to your workstation using the following command.
```
git clone https://github.com/shoreviewanalytics/spark-logback-example-one.git
```

**Step 2**

Compile the code to a .jar file and adjust the path to the .csv file. For example, it will be necessary to change the path to the words.csv.  

**Step 3**

Step 3 is optional if you are not working on a distributed environment. If have access to a local or cloud based cluster, you can add the words.csv to an accessible path to the user running the spark job on each node in your cluster.  For example,  home/username/data/words.csv.  It is not necessary to have this file as you can just create your own file.csv and then just create a list of words with a random number of duplicates.      

**Step 4**

Run the example using spark-submit.  The exact syntax of this command depends on the platform you are using, whether you have security enabled or not, the location of your logback.xml file, the location of the .jar file (your spark job) and the configuration of your cluster environment.  Also the following command is separated into multiple lines just for this blog post and should be run as a single line in a terminal window.    

```
dse -u username -p password spark-submit
--class com.java.spark.LoggingSample
--master dse://?
--driver-java-options "-Dlogback.configurationFile=/pathtoyour/logback.xml"
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

Finally, to test out this logging example on a distributed computing environment, I used a three node cluster with DataStax Enterprise Analytics enabled. For more information about DataStax Enterprise Analytics check out the [DataStax Enterprise Analytics  ](https://www.datastax.com/products/datastax-enterprise-analytics) page on the DataStax website.
