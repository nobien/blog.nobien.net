---
comments: true
date: 2010-07-19 08:51:15
layout: post
slug: practicing-continuous-integration-on-flash-projects-automation
title: 'Practicing Continuous Integration on Flash Projects : Automation'
wordpress_id: 278
categories:
- actionscript 3
- continuous integration
- flash
- flex
- methodology
- process
- unit testing
---

In the [previous part](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-project-structure/) one of my series of "Practicing Continuous Integration on Flash Projects" I explained how to set up a Flash project for CI. In this part of the series I'm going to explain how automation is achieved using Apache Ant and the Flex SDK.


## Introduction


One of the  most import parts of CI is the need to automate the build process. Building a Flash project means, at the very least, compiling your code into a SWF file. There are various ways to compile a SWF, but in order to be able to automate the process its best to use the mxmlc compiler thats part of the Flex SDK. If you are using the Flash IDE to compile your projects I would say that CI is not for you and/or your organization. However, thats not to say you should stop reading or stop using the IDE. I still use the IDE on nearly all my projects, just differently than I used to. Anything that requires timeline animation or is just easier to draw using the IDE I publish into a SWF. I then import the symbols from that SWF into my project when I compile with mxmlc. Explaining how to do this and just exactly how to use the mxmlc compiler is outside the scope of this blog series but you can learn more about using it on the [Flex 4 product support page](http://help.adobe.com/en_US/flex/using/index.html).




## Apache Ant


Performing the automation can be achieved in various ways. For instance, depending on your operating system you could write a batch file or shell script that contains everything you'd need to run to compile the SWF. While this method works, it also requires you to consider each operating system that the project might be built on. Knowledge of each OS would be necessary to write equivalent routines for compiling, deployment, and such. In order to get around this developers created automation software such as Ant and Maven. Ant is my tool of choice and is what I will use to illustrate how to automate the build process of a Flash project. Ant is great because its quite easy to use, has a lot of native functionality, and can be extended with third party tools. Ant reads a simple XML file that identifies commands you wish to run. For instance, if you need to copy a file, you would use the <copy> task and Ant would copy whatever files you wished to wherever you specified. This eliminates the need to know the native command for copying files.


## Compiling With Ant


Using Ant is quite simple. Real quick, if you haven't used Ant before, you simply navigate to the folder containing your build.xml file from a command prompt and run the _ant _command. Ant will run the default target specified in the _default_ attribute of the <project> node of your build file. If you want to run a different target, you simply follow the ant call with the name of the target you wish to run. More information about using Ant can be found on the [Apache Ant website](http://ant.apache.org). Compiling a SWF with mxmlc can be achieved in three different ways using Ant. The first, and probably the most obvious, is by using Ant's <exec> task. The <exec> task allows you to run an executable and specify arguments and parameters. It would look something like this:

The second method would be to use the Flex Ant tasks provided with the Flex SDK. To use them you'll have to copy them into your Ant installation or package them with your project. I recommend you always package it with your project. This will eliminate an extra step for any new developer that comes on to the project. The tasks file is located in the FLEX_HOME/ant/lib folder.By including the Flex Ant tasks you'll be given about to use a new task named <mxmlc>. Using the Flex Ant tasks would look something like this:    

The third method, my preferred method, would be to use the <java> task to invoke the mxmlc.jar file. This method would look something like the following:

There are pros and cons to each method, but most importantly, using the <mxmlc> or the <java> Ant tasks will make it easier to compile your SWFs across platforms so long as the JDK is installed.



## The Project Build File



Lets take a look at the various targets in the CIExample project's build file. Open this [link](http://code.google.com/p/ci-example/source/browse/trunk/build.xml) in a new tab or window.  Before any of the targets you will see the the following code:
 

In the previous code, four things are happening. The first is that a properties file is specified. View the properties file [here](http://code.google.com/p/ci-example/source/browse/trunk/build.properties). The properties file defines various properties that are going to be useful throughout the build file. All properties in an Ant build file are referenced by wrapping the property name like so: _${propertyname}_. After the properties file is loaded, any environment variables are inserted as Ant propreties and prefixed with "env".  After this, two additional Ant task definitions are loaded: ant-contrib.jar and flexUnitTasks.jar. Ant-Contrib is an additional set of tasks that are extremely useful when writing your buidl files. More information about Ant-Contrib can be read [here](http://ant-contrib.sourceforge.net/). The FlexUnit tasks will allow us to automate the analyzation of the unit test results. Now lets take a look at the first target in the build file:

The init target may look a bit strange, but it performs a crucial routine that I prefer to run over setting a property in the build.properties file. I've found it common practice throughout the industry to set the FLEX_HOME property in the build.properties file. I'm not a fan of this because I like to keep the build.properties file under version control. With multiple developers on a project, managing this file would get tricky as most developers have a unique development environment. Personally, I prefer to allow the configuration of the FLEX_HOME either through an environment variable or by passing the property value as a parameter when running the build. So set an environment variable named FLEX_HOME or pass the property value in the command line. It would look something like this:  _ant -DFLEX_HOME='/sdk/flex/4.1' desired-target-name _ Now lets take a look at the next two targets:  

These two targets, _clean-bin_ and _clean-test_ are relatively simple. They both delete folders that contain generated files. Specifically, the bin folder is where the project's SWF is compiled to, whereas the bin-test folder is where the unit tests SWF and resulting report is compiled to. Now lets take a look at the next two targets:

The _compile-swf_ target, well, it compiles the project into a SWF. This is the target that will be run if all the unit tests pass. The _compile-test_ target simply compiles the unit tests into a SWF. Both of these targets depend on the _init_ target and their respective clean targets. Following these targets is the _run-flexunit_ target:  

In the run-flexunit target the FlexUnit task is used to analyze the unit test SWF. It requires that you have the stand-alone debugger version of the Flash Player so that it can open it and listen to the SWF's trace output. This is what allows the task to determine if the unit tests passed or failed. After it analyzes the SWF it output's an XML file into the directory specified in the _toDir_ attribute. Additionally, it sets a property of your liking equal to a value of true that specifies if the tests failed or not. In this case its a property named _flexunit.failed_. This property will be useful later on. The following target makes use of the _flexunit.failed_ property generated by the FlexUnit ant task:

The _run-test_ target is quite simple. It depends on the _run-flexunit_ target which in turn depends on the _compile-test_ target. After the tests are compiled and analyzed, a simple condition is checked on the _flexunit.failed_ property. If its true, the Ant <fail> task is called to stop the build process. If its not true, simply echo out a message that says that all the unit tests passed. Following the _run-test_ target is the r_un-debug_ target:  

The _run-debug _target will probably be the most used target by developers on their local machine. It depends on the _compile-swf_ target and then launches the compile output in a browser to check out the results. This target simply opens the SWF itself, but if you wanted to you could create another target that generates an HTML container and launches the HTML file instead of the SWF. You'll also notice this target checks the local OS to ensure the proper method of opening the browser. And finally, the last three targets:



The _build_ target depends on the _run-test_ and _compile-swf_ targets. This will run the tests and, so long as they pass, then compile the project SWF. If the tests fail, the build will fail and the project SWF will not be compiled. The _deploy-testing_ and _deploy-production_ targets don't do anything as they would be very specific to your own needs, but they do depend on the build target to ensure the project is built properly before deploying anywhere.


## Wrap Up


Using Ant is a great way to automate your build process as it can be used on a variety of platforms. Hopefully you now have a sense of how to use Ant to automate the build process. Feel free to use the CIExample project as a template for your own projects that you wish to automate.

In the [next part of this series](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-integration-server-setup/), I'll explain how to set up a CI server using Amazon's EC2 service.

Series Index



	
  1. [What is CI?](http://blog.nobien.net/2010/07/18/practicing-continuous-integration-on-flash-projects-what-is-ci/)

	
  2. [Project Structure](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-project-structure/)

	
  3. [Automation](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-automation/)

	
  4. [Integration Server Setup](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-integration-server-setup/)

	
  5. [Using Hudson](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-using-hudson/)


