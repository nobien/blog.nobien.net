---
comments: true
date: 2010-07-19 08:56:41
layout: post
slug: practicing-continuous-integration-on-flash-projects-using-hudson
title: 'Practicing Continuous Integration on Flash Projects : Using Hudson'
wordpress_id: 304
categories:
- actionscript 3
- continuous integration
- flash
- flex
- methodology
- process
- unit testing
---

In the [previous part](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-integration-server-setup/) of my series "Practicing Continuous Integration on Flash Projects" I described how to setup a CI server using Amazon's EC2 service. In this final part of the series I'm going to quickly show you how to set up your project in Hudson so that it can build it.


## Using Hudson


If you're practicing CI properly you should be integrating new code on a pretty regular basis. When new code is integrated the project should then run through its build process which should include running all the unit tests and building the project if all the unit tests pass. This is where Hudson comes in. Hudson can monitor your source repository and whenever new code is committed it will retrieve it and build the project. Now I'm going to show you how to set up the CIExample project in Hudson.



If you happen to follow the steps in the previous part of this series, or have your own Hudson server properly configured navigate to the Hudson install's home page. It should look like this:

[caption id="attachment_306" align="aligncenter" width="299" caption="Hudson dashboard"][![](http://blog.nobien.net/wp-content/uploads/2010/07/hudson-intro-299x181.jpg)](http://blog.nobien.net/wp-content/uploads/2010/07/hudson-intro.jpg)[/caption]

Click the "New Job" link in the top left. In the 'Job name' field enter 'CIExample' and select the "Build a free-style software project" option. The screen should look like this:

[caption id="attachment_308" align="aligncenter" width="299" caption="New job screen"][![New job screen](http://blog.nobien.net/wp-content/uploads/2010/07/hudson-newjob1-299x181.jpg)](http://blog.nobien.net/wp-content/uploads/2010/07/hudson-newjob1.jpg)[/caption]

Click the OK button. If you'd like you can fill out a description for the project. But for now we're going to be quick and dirty. Scroll down a bit to the Source Code Management section. Select the Subversion option and some form fields should appear. Configure the repository URL with the CIExample project SVN repository: [http://ci-example.googlecode.com/svn/trunk/](http://ci-example.googlecode.com/svn/trunk/) It should look like this:

[caption id="attachment_309" align="aligncenter" width="299" caption="Hudson source code management configuration"][![](http://blog.nobien.net/wp-content/uploads/2010/07/hudson-subversion-299x181.jpg)](http://blog.nobien.net/wp-content/uploads/2010/07/hudson-subversion.jpg)[/caption]

Scroll all the way down so that you can see the Build Environment and Build sections. First check the "Run Xvnc during build" check box. This ensures the Flash Player will be able to be used when inspecting the unit tests. Lastly, in the Build section, click the "Add build step" button and select "Invoke Ant" from the list of options. We don't need to specify any targets since the default target of the project's build file builds the project. The screen should look like this:

[caption id="attachment_312" align="aligncenter" width="299" caption="Hudson VNC and ANT configuration"][![](http://blog.nobien.net/wp-content/uploads/2010/07/hudson-vnc-ant1-299x181.jpg)](http://blog.nobien.net/wp-content/uploads/2010/07/hudson-vnc-ant1.jpg)[/caption]

Lastly, click the "Save" button to save the project. This should take you to the project's dashboard and look like this:

[caption id="attachment_311" align="aligncenter" width="299" caption="Hudson project dashboard"][![](http://blog.nobien.net/wp-content/uploads/2010/07/hudson-project-299x181.jpg)](http://blog.nobien.net/wp-content/uploads/2010/07/hudson-project.jpg)[/caption]

Now, the only thing left to do is click the "Build Now" button in the left hand navigation. After a second or two of clicking the button a notification window should appear and should look like this:

[caption id="attachment_314" align="aligncenter" width="299" caption="Hudson notification panel"][![](http://blog.nobien.net/wp-content/uploads/2010/07/hudson-notification1-299x181.jpg)](http://blog.nobien.net/wp-content/uploads/2010/07/hudson-notification1.jpg)[/caption]

If everything has been installed and configured properly the build should be successful and you'll see a little blue dot notifying you that the build has completed. It should look like this:

[caption id="attachment_315" align="aligncenter" width="299" caption="Build completed!"][![](http://blog.nobien.net/wp-content/uploads/2010/07/hudson-completed-299x181.jpg)](http://blog.nobien.net/wp-content/uploads/2010/07/hudson-completed.jpg)[/caption]

Wa-la! Thats it! You've got Hudson working and building your project for you. Pretty awesome. Now you can configure the project with various options. For instance, you can configure Hudson to build the project whenever it detects new code has been committed to the repository so that you don't have to initiate the build by hand every time.


## Wrap Up


So that concludes my "Practicing Continuous Integration on Flash Projects" series. In the first part I covered what Continuous Integration is and why it might be useful to your process. In the second part of the series I covered how to set up a project and its unit tests with the assumption that the build and tests will be automated. In the third part of the series I covered how to automate the build process using Apache Ant. In the fourth part of the series I covered how to setup a Continuous Integration server on a bare bones Fedora 8 Linux box using Amazon's EC2 service. And lastly, in this fifth and final part I covered how to setup the project in Hudson so that the build process can be automated and monitored

Series Index



	
  1. [What is CI?](http://blog.nobien.net/2010/07/18/practicing-continuous-integration-on-flash-projects-what-is-ci/)

	
  2. [Project Structure](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-project-structure/)

	
  3. [Automation](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-automation/)

	
  4. [Integration Server Setup](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-integration-server-setup/)

	
  5. [Using Hudson](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-using-hudson/)


