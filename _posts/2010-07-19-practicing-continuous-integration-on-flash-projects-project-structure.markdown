---
comments: true
date: 2010-07-19 08:48:09
layout: post
slug: practicing-continuous-integration-on-flash-projects-project-structure
title: 'Practicing Continuous Integration on Flash Projects : Project Structure'
wordpress_id: 282
categories:
- actionscript 3
- continuous integration
- flash
- flex
- methodology
- process
- unit testing
---

In the [previous part](http://blog.nobien.net/2010/07/18/practicing-continuous-integration-on-flash-projects-what-is-ci/) of my series "Practicing Continuous Integration on Flash Projects" I described what CI is and its best practices. In this next part I will explain how to set up a project to make the CI process run smoothly.


## Introduction


Practicing CI relies heavily on a consistent process. Having a well defined project structure will definitely be an important piece to the puzzle as well. Flash projects are unique in how they are structured and any seasoned developer has their preferred way of doing things. Regardless of if you are a seasoned developer or new to all of this, this part of my series is meant to serve as a simple guideline to developers who are new to the practice of CI. I truly believe that having a simple and consistent project structure is extremely important to being able to make a project portable and easily built on various platforms. That being said, I've created a very simple project called [CIExample](http://code.google.com/p/ci-example/). Its under version control at [google code](http://code.google.com/p/ci-example/). This is the project that I will be referencing throughout the rest of this series.




## Folder Structure


Open the [CIExample](http://code.google.com/p/ci-example/) project website . First, lets take a look at the folder structure. There are five folders in the project.

**lib
The _lib_ folder is where any of the SWC files that the project needs should be placed. Many third party code libraries are distributed as SWC files now and I highly recommend using them. The CIExample project currently doesn't include any SWC files, but its always nice to have a place to put them should you need to use one.**

**lib-test
The _lib-test_ folder is where any of the SWC files that the unit tests need should be placed. The CIExample project uses [FlexUnit](http://flexunit.org/) for unit testing. All the necessary FlexUnit SWC files are placed here. Its nice to have a place to keep files that are specific to test compiling  away from your project files. Unit testing is a whole other topic, and unfortunately is not within the scope of this series. I highly recommend FlexUnit. Their [website](http://www.flexunit.org) has plenty of information to get you started writing unit tests for your project.**

****resources**
The _resources_ folder is where you can place any additional files that are needed for the build process. The CIExample project uses this folder to hold two .jar files. Thes files contain additional Ant tasks that are needed for the build process. You'll learn more about these files in the Automation part of this series. In many of my projects this folder contains files and templates for generating code and HTML containers.**

****src**
The _src _folder contains all the source code for the project. The CIExample project contains but one class file which will be the project's compile target.**

****src-test**
The _src-test_ folder contains all the source code for the project's unit tests. The CIExample project includes a basic test runner and one test suite. The test suite includes one dummy test case.**


## Build Files


**At the root of the project you'll notice a few files.**

**README
The _README _file is a very underrated project file. This file should include basic instructions on how to build the project. Any new developers should be able to look at this file, follow its instructions, and get the project running on their local development environment so that they may begin to contribute.**

****build.xml**
The _build.xml_ file is the core automation file used by Apache Ant. This file will be explained in full in the Automation part of this series.**

****build.properties**
The _build.properties _file contains property names and values that are used in the build.xml file. Apache Ant reads this file before any automation is performed. **


## **Wrap Up**


**This part of the series is rather short but its meant to briefly outline a basic project structure that will make practicing CI relatively easy. Basically, there are three main areas for files: Project files (src and lib), test files (src-test and lib-test) and build files (resources). Keeping this structure will allow you to easily locate files that relate either to the project, tests or the build. Every project should also include automation file(s). The CIExample project uses Apache Ant, thus the build.xml and build.properties files are located at the root of the project. And finally, always remember to include a README file that includes instructions on how to build the project. In the [next part of this series](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-automation/) I'll explain how to automate the build process.**

**
**

_Series Index_



	
  1. [What is CI?](http://blog.nobien.net/2010/07/18/practicing-continuous-integration-on-flash-projects-what-is-ci/)

	
  2. [Project Structure](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-project-structure/)

	
  3. [Automation](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-automation/)

	
  4. [Integration Server Setup](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-integration-server-setup/)

	
  5. [Using Hudson](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-using-hudson/)


