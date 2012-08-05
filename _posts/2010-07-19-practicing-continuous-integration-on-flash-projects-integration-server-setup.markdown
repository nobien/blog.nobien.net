---
comments: true
date: 2010-07-19 08:52:49
layout: post
slug: practicing-continuous-integration-on-flash-projects-integration-server-setup
title: 'Practicing Continuous Integration on Flash Projects : Integration Server Setup'
wordpress_id: 290
categories:
- actionscript-3
- continuous integration
- flash
- flex
- methodology
- process
- unit testing
---

In the [previous part](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-automation/) of my series "Practicing Continuous Integration on Flash Projects" I explained how to use Apache Ant and the Flex SDK to automate the build process of a project. In this part of the series I'm going to show you how to set up [Hudson](http://www.hudson-ci.org/), an integration server, on a bare bones Fedora 8 Linux box.


## Introduction


Code integration happens whenever you commit code into the project's mainline. There are essentially two camps when it comes to building the project after new code has been committed. Some developers prefer to do it manually, meaning that the developer switches to the "integration machine", checks out the new code and runs the build process. If the build is successful then the developer's commit is also successful. If its not successful, then the developer should revert their commit, make adjustments to their code, and attempt to integrate again later.

The other camp enjoys using a CI server such as Hudson or CruiseControl. A CI server will monitor the project's mainline and attempt to build the project any time new code is committed. A developer can check out the CI server website to see the progress or the eventual status of the build. When the build is complete, successful or not, the CI server should notify the developer who committed the new code. Personally I think a CI server is an invaluable tool when it comes to practicing CI and thus I'd like to show you how to get Hudson up and running.




## Amazon EC2


[Amazon EC2](http://aws.amazon.com/) is a great service that allows you to create computing resources on demand. In other words, it gives you an easy way of expanding your network infrastructure without actually having to buy hardware. In my mind, its the perfect service to run a CI server on because its easy to use and relatively cheap (you only pay for what you use). Otherwise, you'll need to find an machine in your network to install your flavor of CI server on. Personally, I'd rather not bog down an existing machine on my network with building projects. Amazon EC2 gives me nearly instant access to a virtual Linux box with root access and a decent amount of storage.


## Creating an Instance on EC2


First allow me to make a disclaimer. Just because I'm advocating the use of EC2, doesn't mean you'll need an Amazon Web Services account to run a CI server. The steps needed to get Hudson up and running will depend on the flavor of Linux you're running, but if you know Linux then it shouldn't be too hard to know what to adjust for your setup. Additionally, Hudson runs just fine on Windows. If you need information about installing Hudson on other platforms visit their [website](http://www.hudson-ci.org/).

So, before we can do anything, we'll need a machine to run Hudson on. With EC2 that means starting an instance. Before starting up an instance you should consider if you might ever want to reuse the resulting system configuration. If you think you might want to reuse it then ensure that you create an EBS backed instance. This will allow you to easily create an AMI from it and then be able to easily launch a new instance of the same configuration later on. Information about creating an EBS backed instance can be found in their [documentation](http://aws.amazon.com/documentation/ec2/).

For the purpose of this example a small instance of a bare bones, 32-bit, Fedora 8 system is just fine. Here is what I did to get my instance running:



	
  1. Log in to the AWS Management Console

	
  2. Click the **AMIs** button in the navigation panel on the left hand side

	
  3. Select **EBS Images** from the combo box labeled **Viewing** above the list of AMIs

	
  4. Click the **Owner** column label to sort the AMIs by owner.

	
  5. Select the AMI with the source name **amazon/fedora-8-i386-v1.14-std**. It should be the first one on the list.

	
  6. Click the **Launch** button above the list.

	
  7. Select the default settings on the Instance Details panel

	
  8. Create a key pair or use an existing key pair.

	
  9. Configure the firewall to allow connections on port 22, 80, 8080 and any other ports that you think might be necessary

	
  10. Review the details and launch the instance.


Eventually you're instance will show up as running in the dashboard. Once its running ensure you can connect to the instance using your favorite SSH client. I personally use PuTTy. Bear in mind that EC2 instances don't use password authentication to connect. They use the public keys. PuTTy is a little weird when it comes to public keys and requires them to be converted to a specific format. Luckily Amazon has a [quick little tutorial](http://docs.amazonwebservices.com/AmazonEC2/gsg/2007-01-19/putty.html) on how to properly configure PuTTy with the public keys.


## Software


Once you're able to connect to your instance its time to start installing all the software you'll need to run Hudson and build your Flash project. The following is a list of all the software needed:



	
  1. vnc (remote display system)

	
  2. vnc-server (to allow connections)

	
  3. X-Window (windowing agent)

	
  4. GNOME (window manager/desktop)

	
  5. Subversion (SVN)

	
  6. Java Development Kit (JDK)

	
  7. Apache Tomcat (or other Java servlet service)

	
  8. Apache Ant

	
  9. Flex SDK

	
  10. Hudson

	
  11. Xvnc plugin for Hudson


**Installing vnc and vnc-server**
Installing vnc and vnc-server is important because in order to analyze the compiled unit tests the resulting SWF needs to run in the Flash Player. Linux needs a windowing environment and a way of redirecting the windowing output to a virtual display. vnc is what does the redirecting. To install vnc and vnc-server simply type the following commands and agree to download the files:

_# yum install vnc
#yum install vnc-server _

**Installing X-Window (X11)**
As previously mentioned, you'll need a windowing environment to run the Flash Player, particularly for analyzing unit test results. To install X-Window type the following command:

_# yum -y install xorg*__ _

**Installing GNOME**
X-Window(X11) will need a window manager and, more importantly, we'll need a window manager that works with X11 and doesn't require any user input to create a screen. GNOME is a decent choice and will make connecting through a visual VNC client pretty nice as well. To install GNOME type the following command:

# yum groupinstall "GNOME Desktop Environment"

**Installing Subversion**
The CIExample project resides in a Subversion repository on Google Code, thus you'll need Subversion to be installed to retrieve it. To install Subversion simply type the following:

_# yum install subversion_

**Installing the Java Development Kit**
Installing the JDK is a little tricky mainly because Sun doesn't make finding the distributable files very easy. You'll want to download a binary package from somewhere into some location on the machine. I like to create a downloads folder at the root of the machine for this. At the time of writing this you can download the latest self extracting RPM by typing the following commands:

_# wget http://www.java.net/download/jdk6/6u21/promoted/b05/binaries/jdk-6u21-ea-bin-b05-linux-i586-29_may_2010-rpm.bin
# chmod 755 jdk-6u21-ea-bin-b05-linux-i586-29_may_2010-rpm.bin
# ./__jdk-6u21-ea-bin-b05-linux-i586-29_may_2010-rpm.bin_

To confirm the JDK was installed, you can type the following command:

_# java -version_

You should see something like the following:

_java version "1.6.0_21-ea"
Java(TM) SE Runtime Environment (build 1.6.0_21-ea-b05)
Java HotSpot(TM) Client VM (build 17.0-b15, mixed mode, sharing)_

**Install Apache Tomcat**

Apache Tomcat is needed because Hudson is written in Java. Before we install tomcat, however, a new Linux user should be created to run tomcat under. Create a user named tomcat by typing the following command:

_# useradd tomcat_

Switch to the tomcat user by typing the following command:

_# su tomcat_

Create a folder for tomcat to be installed in within the /usr/local folder by typing the following commands:

_# cd /usr/local
# mkdir tomcat_

Download Apache Tomcat into the folder you created from any of the available mirrors and extract the files. At the time of this writing I typed the follwing:

_# cd tomcat
# wget http://mirror.atlanticmetro.net/apache/tomcat/tomcat-7/v7.0.0-beta/bin/apache-tomcat-7.0.0.tar.gz
# tar -xvf apache-tomcat-7.0.0.tar.gz_

Now all the files for tomcat are located in /usr/local/tomcat/apache-tomcat-7.0.0. Last thing to do now is switch back to the root user:

_# exit _

**Install Apache Ant**
Apache Ant is need to run the automated build process. Ant doesn't require anything special for installing, simply download the binary and extract it into a convenient location. I prefer to install it in the the /usr/ant/<version> folder. Also, don't forget to switch back to the root user. At the time of this writing I typed the following:

_# cd /usr
# mkdir ant
# cd ant
# wget http://apache.opensourceresources.org/ant/binaries/apache-ant-1.8.1-bin.tar.gz
# tar -xvf apache-ant-1.8.1-bin.tar.gz_

Now all the files for Ant should reside in the folder /usr/ant/apache-ant-1.8.1.

**Install the Flex SDK**
Now we need to install the Flex SDK. This will be just like installing Ant. I like to install any SDK's in a folder structure such as this: /sdk/<name-version>. Type the following to install the Flex SDK:

_# cd /
# mkdir sdk
# cd sdk
# mkdir flex-4.1
# wget http://fpdownload.adobe.com/pub/flex/sdk/builds/flex4/flex_sdk_4.1.0.16076.zip
# unzip flex_sdk_4.1.0.16076.zip -d /sdk/flex-4.1_

**Set Environment Variables and Permissions**
In order for a lot of this software to run properly you'll need to set some environment variables, add additional directories to the system PATH variable, change some file permissions. First, lets set the JAVA_HOME, ANT_HOME, CATALINA_HOME and FLEX_HOME environment variables and add Ant to the system PATH. To do this we'll need to edit the /etc/profile file. Type the following to open the file in the basic vi text editor:

_# vi/etc/profile_

Now we need to add some new lines to the file. Using the vi text editor is a little weird if you haven't used it before. I'll describe exactly what I did.



	
  1. Navigate one line below the line that reads: "_export PATH USER LOGNAME MAIL HOSTNAME HITSIZE INPUTRC_" by using the arrow keys on your keyboard.

	
  2. Press the letter _i _on your keyboard to enter insert mode.

	
  3. Enter the following text into the file:


_export JAVA_HOME=/usr/java/jdk1.6.0_21_




_export ANT_HOME=/usr/ant/apache-ant-1.8.1_




_export CATALINA_HOME=/usr/local/tomcat/apache-tomcat-7.0.0
export FLEX_HOME=/sdk/flex-4.1_







_export PATH=$PATH:$ANT_HOME/bin
_





	
  4. Press the _escape_ key on your keyboard to exit insert mode

	
  5. Type the following to save and quit the text editor:_:wq_

	
  6. Logout of your SSH session

	
  7. Log back into your SSH session

	
  8. Verify that each environment variable has been set by typing the following:
_
# echo $JAVA_HOME
# echo $ANT_HOME
# echo $CATALINA_HOME
# echo $FLEX_HOME
_

	
  9. Verify that Ant can be called by the tomcat user from within the tomcat folder by typing the following:
_
# su tomcat
# cd /usr/local/tomcat
# ant -version _


Hopefully all went well for you there. Now we just need to change the permissions on the Flex SDK so that any user Linux user, specifically the tomcat user, can use the SDK. Type the following:

_# exit (switch back to root user)
# cd /sdk
# chmod -R 755 flex-4.1 _

Verify the tomcat user can execute the mxmlc compiler by typing the following:

_# cd /usr/local/tomcat
# su tomcat
# /sdk/flex-4.1/bin/mxmlc_

If you don't get a permission error you're good to go!

**Installing Hudson**
Hudson can be installed in many ways, but in my opinion, the easiest way to install it is to deploy through the Tomcat manager. First you'll need to download Hudson to your local machine. The most recent WAR file can always be downloaded from [this link](http://hudson-ci.org/latest/hudson.war). Save it somewhere easy to remember.

Now before you can access the Tomcat manager you're going to need to add a user with permission to view the web manager. To do this we need to modify the Tomcat user file. Type the following:

# su tomcat
# vi /usr/local/tomcat/apache-tomcat-7.0.0/conf/tomcat-users.xml

Now modify the file using the following steps:



	
  1. Navigate between the <tomcat-users></tomcat-users> XML node by using the arrow keys on your keyboard.

	
  2. Press the _i _key on your keyboard to enter insert mode.

	
  3. Enter the following text:<user username="admin" password="tomcatadmin" roles="manager-gui"/>

	
  4. Modify the username and password to your liking.

	
  5. Press the _escape_ key on your keyboard to exit insert mode.

	
  6. Type the following to save and quit the text editor
_:wq<enter>_


Now its time to startup Tomcat. To start Tomcat type the following:

_ # $CATALINA_HOME/bin/startup.sh_

You should see some output in the console. Now, open your favorite web browser and navigate to the URL of your EC2 instance on port 8080. For instance, my url looked like this:

_http://<some-ec2-id>.compute-1.amazonaws.com:8080_

Navigate to the manager console by clicking on the _Tomcat Manager _button in the left navigation under the Administration tab. Login with the user credentials you created in the tomcat-users.xml file. You should see a list of the web apps that have been packaged with Tomcat. Now its time to deploy Hudson on Tomcat. Perform the following:



	
  1. Scroll down to the _Deploy_ section of the manager console.

	
  2. Click the _Choose File _button in the _War file to deploy_ section of the console.

	
  3. Click the _deploy_ button below the _Choose File_ button.

	
  4. Wait for the file to upload

	
  5. Find Hudson in the list of applications and verify that it is running


Now there's a little tricky thing you have to do in order to get the index page of Hudson to appear. From what I understand its discrepency in the Java servlet specification. Alas, all you need to do is modify Hudson's web.xml file.

	
  1. Open Hudson's web.xml file in a text editor by typing the following:
_# vi /usr/local/tomcat/apache-tomcat-7.0.0/webapps/hudson/WEB-INF/web.xml
_

	
  2. Find the <servlet-mapping> node that looks like the following:
_<servlet-mapping>
<servlet-name>Stapler</servlet-name>
<url-pattern>/</url-pattern>
</servlet-mapping>
_

	
  3. Press the letter _i_ on your keyboard to enter insert mode.

	
  4. Modify the entry to look like the following:
_<servlet-mapping>
<servlet-name>Stapler</servlet-name>
<url-pattern>/_**_*_**_</url-pattern>
</servlet-mapping>
_

	
  5. Press the _escape_ key on your keyboard to exit insert mode.

	
  6. Type the following command to save and quit
_:wq<enter>_


Now you can view Hudson in your browser with no trouble. Go to the following URL:

_http://<some-ec2-id>.compute-1.amazonaws.com:8080/hudson/_

You should now be able see the Hudson dashboard!

**Install Hudson Plugins**
There are loads of plugins for Hudson, including this totally sweet[ Chuck Norris Plugin](http://wiki.hudson-ci.org/display/HUDSON/ChuckNorris+Plugin), that you may or may not want to install. At the very least you'll need at least one Hudson plugin to be able to run vnc so that the Flash Player can redirect its output. Download the [Xvnc plugin](http://hudson-ci.org/latest/xvnc.hpi) to your local machine.  Navigate to the Advanced panel of the Hudson plugin manager by going to the following URL:

_http://<some-ec2-id>.compute-1.amazonaws.com:8080/hudson/pluginManager/advanced_

Upload the plugin using the _Upload Plugin_ section of the plugin manager. Once both plugins are uploaded you'll need to restart Hudson. Navigate to the Tomcat manager again

_http://<some-ec2-id>.compute-1.amazonaws.com:8080/manager/html_

Find Hudson in the list of web apps again and click _Reload _button to the right under the _Commands_ column. Once this has completed, verify that the plugins have been installed by navigating to:

_http://<some-ec2-id>.compute-1.amazonaws.com:8080/hudson/pluginManager/installed_

You should now see the Git and Xvnc plugins listed.

**Configure the Flash Player Debugger**
I know this seems like a lot, but we're almost there. Time to configure the Flash Player debugger. First we'll need to extract the debugger from the Flex SDK. Make sure you switch back to the root user if you haven't already. Type the following commands:

_# cd /sdk/flex-4.1/runtimes/player/10.1/lnx
# tar -xvf flashplayerdebugger.tar.gz _

Now that the debugger has been extracted you you'll need to create a a symbolic link to it named _gflashplayer_.  To create the link type the following commands:

# cd /usr/sbin
# ln -s /sdk/flex-4.1/runtimes/player/10.1/lnx/flashplayerdebugger gflashplayer

Now, in order for the Flash Player debugger to "trust" our unit test SWF(s), we have to create the "would-be" folder that the normal Flash Player security settings would be stored in. Create the following folder tree:

_/home/tomcat/.macromedia/Flash_Player/#Security/FlashPlayerTrust_

**Configure VNC
Now, the last thing to do is to configure the VNC server for the tomcat user and ensure that its using GNOME as its window manager. Lets first configure the vnc server for the tomcat user. Make sure you're the root user and open the vnc server config file by typing the following:**

_# vi /etc/sysconfig/vncservers _

Now you'll need to add a server for the tomcat user. Do this by performing the following steps:



	
  1. Move the cursor below all the comments.

	
  2. Press the letter _i _on your keyboard to enter insert mode.

	
  3. Enter the following text:_VNCSERVERS="1:tomcat"
VNCSERVERARGS[1]="-geometry 1024x769 -depth 16"
_

	
  4. Press _escape _ to exit insert mode.

	
  5. Type the following to save and quit the text editor
_:wq<enter>_


_Now that this is done, you'll need to set the tomcat user's vnc password. Type the following:_

_# su tomcat
# vncpasswd_

_Set the password to whatever you like. Just remember this if you ever want to connect through a VNC viewer! The next thing to do is configure the tomcat user to use GNOME instead of the default window manager. To do this open the tomcat user's vnc configuration file by typing the following:_

_# vi /home/tomcat/.vnc/xstartup_

By now you should know how to edit a text file, so make it look like the following:

_#!/bin/sh
# Uncomment the following two lines for normal desktop:
unset SESSION_MANAGER
exec /etc/X11/xinit/xinitrc
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
#xsetroot -solid grey
#vncconfig -iconic &
#xterm -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
#twm
&startx &_

_Save and quit the text editor. Lastly, start the vnc server by typing the following command:_

_# service vncserver start_

Now guess what? You're done installing and configuring everything! Woohoo!

**Wrap Up**

After all that work you should now have your CI server up and running. The only thing left to do is add the project to Hudson so that it can start building it. I'll explain how to do this in [the following section](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-using-hudson/). For now, go grab yourself a well deserved beverage or snack.

_Series Index_



	
  1. [What is CI?](http://blog.nobien.net/2010/07/18/practicing-continuous-integration-on-flash-projects-what-is-ci/)

	
  2. [Project Structure](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-project-structure/)

	
  3. [Automation](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-automation/)

	
  4. [Integration Server Setup](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-integration-server-setup/)

	
  5. [Using Hudson](http://blog.nobien.net/2010/07/19/practicing-continuous-integration-on-flash-projects-using-hudson/)


