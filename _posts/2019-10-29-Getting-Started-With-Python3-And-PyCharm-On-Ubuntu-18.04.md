---
layout: single
title: 'Getting Started with Python3 and PyCharm on Ubuntu 18.04'
description: 'This post explains how to create a virtual environment for Python development as well as how to open a Python project folder using PyCharm. As a bonus I have included a step below that explains how to install Jupyter Notebook.'
date: '2019-10-29'
classes: wide

header:
  overlay_color: "#5e616c"
  overlay_image: /assets/images/abstract.jpg
---

# Getting Started with Python3 and PyCharm on Ubuntu 18.04

If you are new to Python and haven't yet selected an IDE you might want to have a look at PyCharm. Also if you have done some Java programming and have used Intellij the look and feel of PyCharm is very similar. For this short post I will explain how to create a virtual environment for your Python development as well as how to open a Python project folder using PyCharm.  As a bonus I have included a step below that explains how to install Jupyter Notebook.  

## Recommendation

As I've worked with Ubuntu I have learned that it is often best to work with a fresh install to a virtual machine. Once you have a fresh copy of Ubuntu 18.04 installed to VirtualBox or another VM platform and updated it, proceed to the following steps. The good news is that Ubuntu 18.04 comes with Python3 version 3.6.8 already installed. 

You can check the version if you like with the following command. 

```
python3 --version
```
You should see the following.  

```
Python 3.6.8
```
### Step 1 - Installing python3-env

Great so now that you know you have Python it's time to create what is called a Python virtual environment. In order to be able to create a virtual environment it is necessary to install a package called python3-venv.  
```
sudo apt install python3-env
```
What this package does essentially is allow you to create a virtual Python3 environment within a directory of your choice. A virtual environment for Python let's you work in an isolated or virtual Python environment.  

Next create a folder where you would like to store your Python programs and create another directory where you will create the virtual environment. For example, the root folder could be called python.  Then another folder under this one could be called lpython.  So the path would be /home/your username/python/lpython.  

### Step 2 - Create a Python Virtual Environment

Next open the lpython folder in a terminal and issue the following command.
```
python3 -m venv env
```
If no issues occurred you should be able to issue the following command to activate your virtual python environment. 

### Step 3 - Activate a Python Virtual Environment

```
source env/bin/activate
```
This command activates your virtual python environment.  While in the virtual environment you can install additional packages that you know you will need during your programming. For example the following command installs numpy a commonly used library for Data Scientists and Data Analysts.
```
python3 -m pip install numpy
```
### Step 4 - Installing PyCharm

Next install PyCharm using the following command.

```
sudo snap install pycharm-community --classic
```
After you have installed PyCharm go ahead and launch it.  After you have launched PyCharm, rather than create a new project choose open.  When you choose open, navigate to the folder structure you created above or the /home/your username/python/lpython directory.  

Now click on the terminal tab at the bottom of the IDE.  Notice that when you click on it you are automatically taken into the virtual environment that you created earlier.  

In order to leave the virtual environment type the following command while within the virtual environment. 
```
deactivate
```

### Step 5 - Installing Jupyter Notebook

If you have successfully completed the above steps creating a virtual environment you can now also install Jupyter and run it in your newly created virtual Python environment.  To install Jupyter Notebook use the following command while within the Python virtual environment. 

```
pip install jupyter
```

To launch Jupyter use the following command from within the virtual environment. 

```
jupyter notebook
```

## Summary

After completing the above steps you should have a working Python virtual environment with the ability to use PyCharm or Jupyter Notebook for your Python programming.   The advantage of using a virtual environment to install Python libraries is that if you should have an issue with package dependencies, you can always remove or create a new virtual environment. If you have any issues or questions please let me know.  