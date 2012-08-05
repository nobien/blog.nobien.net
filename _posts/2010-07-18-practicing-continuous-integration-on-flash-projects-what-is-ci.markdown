---
comments: true
date: 2010-07-18 09:00:17
layout: post
slug: practicing-continuous-integration-on-flash-projects-what-is-ci
title: 'Practicing Continuous Integration on Flash Projects : What Is CI?'
wordpress_id: 258
categories:
- actionscript-3
- continuous integration
- flash
- flex
- methodology
- process
- unit testing
---

Welcome to my blog series, "Continuous Integration and Flash". This series is a case study of all the things I learned about practicing Continuous Integration. As a reader of this series, you are most likely interested in Continuous Integration in some way or another. After reading at least this first entry I hope to give you a better understanding of what Continuous Integration is and how its practiced. In this series I will also explain how to set up a project, automate the build process, and set up a CI server using Amazon EC2. For the remainder of this series I will mostly use the acronym **CI** when referring to the term Continuous Integration.




## Introduction


So just what the heck is CI? What I've come to understand is that CI is a software development practice that is most commonly associated with[ agile software development](http://en.wikipedia.org/wiki/Agile_software_development). In practice it involves the "continuous integration" of code  and tests from the various developers of a project at a desired frequency. Integration of new code always occurs in the project's main line (trunk or master branch) of development. Once the new code is committed an automated process is invoked to run various tests and build the project (if the tests don't fail). Additionally, the automation process can, and should, include a method of deploying the project to various contexts such as a testing or production server.

My understanding of what CI is didn't come to me so easily. When I first heard the term it was in the form of "Continuous Integration Server" and its my guess that one of the most common misunderstandings that developers have with regards to CI is that it is centered around a piece of software. A CI server is just a tool that helps you practice CI. There are a lot of different CI servers out there that will help you manage the process of automating your build process. Out of all my research the two most common ones seem to be [CruiseControl](http://cruisecontrol.sourceforge.net/) and [Hudson](http://www.hudson-ci.org/). In part three of this series I'll explain  how to use Hudson on a remote Linux box to manage your build process.


## Best Practices


With any methodology, there are good and bad ways to go about practicing them. When practicing CI you should try to adhere to the following:

**Use a version control system such as Subversion, Git, or CVS.
**Using a version control system such as [Subversion](http://subversion.tigris.org/), [Git](http://git-scm.com/), or [CVS](http://www.nongnu.org/cvs/) will allow the developers on the project to easily commit their work to a single place of storage. Additionally, version control systems provide mechanisms for resolving conflicts between files edited by more than one person at a time. This type of functionality can make integration of new or changed code much easier. In part three of this series I'll explain how to configure Hudson to pull files from a Git repository.

**Automate the build process
**Building a project usually consists of running various sets of commands, moving files, generating files, etc. Lucky for us, there are plenty of tools out there to help automate these steps. Tools like [Ant](http://ant.apache.org/) and [Maven](http://maven.apache.org/) gives developers the ability to create simple commands to perform all the steps needed to build the project. Minimizing human error during the build process is essential and a good practice for any development methodology you practice.

**Make the build self testing**
There's more than one way to skin(test) a cat (your code). Regardless of how you test your code, it should be part of your automated build process. Test Driven Development is a popular technique to test code before a project can successfully be built. In part two you'll see how a tool like FlexUnit fits into the CI process.

**Everyone commits to the mainline at a desired frequency**
The desired frequency is up to you. Its generally considered that the more often you commit the better. Checking your code in often allows developers to recognize bugs earlier and thus resolve them earlier. Imagine if you didn't commit you're code for a whole week. Unless the other developers on the project are exact clones of you, there's probably going to be more conflicts than if you were to commit every day. However, thats not to say you have to do this. Another approach in agile development is to perform a "sprint". During a sprint developers write code for a desired about of time, this can last anywhere from a few days to a few weeks. At some point the developers stop working on their code and begin to merge their work together into the mainline. This can last anywhere from a day or more. Regardless of your approach, both rely heavily on good communication between the development team to ensure everyone is on the right path.

**Run the build each time you commit new code**
Whenever you commit new code you should build the project to verify the new code has integrated properly. Developers themselves should also be building the project on their local machine to ensure their code works with whatever is in the project's mainline. This is where tools like Hudson and CruiseControl come in. These server's can begin the automated build process whenever code is committed into the mainline.

**Keep the build fast**
A project's build process should be optimized to run as fast as possible. This will enable developers to quickly identify problems with integration. If its not fast then you're not going to benefit from this methodology. Most Flash projects don't take too long to build, but the more complicated your project gets the longer it may take to build. If you feel as if you're project is taking too long to build consider breaking it up into various smaller projects with a master project that pulls everything together.

**Test in a clone of the production environment whenever possible**
Naturally you'll want to ensure your project is ready to be deployed to production. This can only really be done if you have an environment that's identical to your production environment. While not always possible this will hopefully reduce any failures when deploying to production.

**Make it easy to get the latest build files**
There should always be an easy way for anyone involved in the project to retrieve the latest build files. In the case of a Flash project this could be a multitude of things, but at the very least this means a SWF file. For instance, as part of the build process, perhaps there's a protected website somewhere in which someone can download an ZIP archive of the SWF file, its HTML container and image assets.

**Everyone can see whats happening**
This is where tools like CruiseControl and Hudson come in again. They each have fancy schmancyweb consoles that show if there's a build in progress and what the result of the last build was. Otherwise, if you're manually invoking the build process, you should  have some way of indicating to the team the status of the build. I recommend getting your hands on a traffic light and hooking it up to your automation machine.

**Automate Deployment**
When a build successfully completes you'll most likely want to deploy it somewhere at some point. This process should also be automated. CI servers usually give you the ability to run a script or another process after the build completes to make deploying, say to your production server, less prone to human error.

If you'd like, you can read other descriptions of these practices elsewhere such as [Wikipedia](http://en.wikipedia.org/wiki/Continuous_integration) or [this paper](http://martinfowler.com/articles/continuousIntegration.html) written by Martin Fowler.


## Advantages & Disadvantages


So you might be thinking to yourself, "This all sounds a bit crazy to me!" Or maybe you're thinking "This all sounds awesome, but what doesn't it provide?" Well then here is a list of various advantages and disadvantages.

**Advantages**



	
  1. Reduced risk

	
  2. Provides a way of knowing what works and what doesn't at all times

	
  3. Early warnings of incompatible code

	
  4. Early warnings of conflicting changes

	
  5. Easier to find bugs and eliminate them

	
  6. Immediate testing of code

	
  7. Constant availability of a build

	
  8. Projects tend to have  less bug

	
  9. Motivates developers to produce code that works


**Disadvantages**



	
  1. Initial setup time can be long

	
  2. Good results rely heavily on well written test suites

	
  3. Difficult to introduce large scale refactoring

	
  4. Infrastructure requirements can induce cost




## Wrap Up


So that just about covers what CI is. I won't go over everything but just remember that CI is a methodology, a mind set, an attitude. Its not a piece of software or server environment. Its all up to you to decide if its practice benefits you and/or your organization. In the [next part of this series](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-project-structure/) I'm going to describe how to structure a Flash project to make practicing CI relatively easy.

_Series Index_



	
  1. [What is CI?](http://blog.nobien.net/2010/07/18/practicing-continuous-integration-on-flash-projects-what-is-ci/)

	
  2. [Project Structure](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-project-structure/)

	
  3. [Automation](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-automation/)

	
  4. [Integration Server Setup](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-integration-server-setup/)

	
  5. [Using Hudson](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-using-hudson/)


