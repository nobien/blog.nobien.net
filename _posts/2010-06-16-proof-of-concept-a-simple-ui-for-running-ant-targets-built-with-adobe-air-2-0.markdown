---
comments: true
date: 2010-06-16 18:53:21
layout: post
slug: proof-of-concept-a-simple-ui-for-running-ant-targets-built-with-adobe-air-2-0
title: 'Proof-Of-Concept: A Simple UI for Running Ant Targets Built with Adobe AIR
  2.0'
wordpress_id: 236
categories:
- actionscript-3
- air
- flash
- flex
---

I've really been getting into [Apache Ant](http://ant.apache.org/) lately. I really enjoy using it to manage various build processes when working on Flash projects. So when I found out that AIR 2.0 can invoke a native process I immediately thought that it'd be cool to learn how to use that capability to make an app that presents a UI for a given Ant build file and then be able to run its various targets from that UI. Thus I created a simple application that I'm calling, for the time being, rANT.

It took me a while to figure things out entirely but in the end I'm happy to have learned how to use [flash.desktop.NativeProcess](http://help.adobe.com/en_US/FlashPlatform/reference/actionscript/3/flash/desktop/NativeProcess.html). Unfortunately, in order to realize my idea faster, I ignored the fact that the application would really depend on a user's system. For instance, I have Ant installed in such a way that its available as a system command so I can run it from the command line like any old DOS command. I know not everyone is setup like this andwould require knowing where Ant is installed on a user's system. Additionally, Ant doesn't come with a good old fashioned .exe file for Windows so I invoked Ant via C:\windows\system32\cmd.exe. Here's some pseudo code that comes straight from my application...



Another (little) thing was that Adobe isn't very clear about the fact that you need to package your application in a native installer in order to use the 'extendedDesktop' profile.  Took me a while to figure it out, but the good news its pretty darn easy to package a native installer from a .air file.

At any rate, its by no means ready for the public, but I wanted to share my work thus far to see if anyone else would be interested in an application such as this. So here's how it looks and works so far:

When you start the application you're presented with a screen that allows you to load a build file...

[![](http://blog.nobien.net/wp-content/uploads/2010/06/rANT-Screenshot-01-300x231.jpg)](http://blog.nobien.net/wp-content/uploads/2010/06/rANT-Screenshot-01.jpg)

Once the file is loaded it displays information about the build file and a list of its available targets...

[![](http://blog.nobien.net/wp-content/uploads/2010/06/rANT-Screenshot-02-300x231.jpg)](http://blog.nobien.net/wp-content/uploads/2010/06/rANT-Screenshot-02.jpg)

You can then select a task and run it. Here you can see the output in the TextArea component from when I ran the _compile-docs_ target...

[![](http://blog.nobien.net/wp-content/uploads/2010/06/rANT-Screenshot-03-300x231.jpg)](http://blog.nobien.net/wp-content/uploads/2010/06/rANT-Screenshot-03.jpg)

Some of my targets require arguments to be passed in for them to work. For instance, my _compile-and-debug_ target requires a debugLevels parameter that specifies the various debug levels (info, status, etc.) to output to the debugging console (which happens to be [de MonsterDebugger](http://demonsterdebugger.com/)). So I created a little panel that lets me specify parameters for the target...

[![](http://blog.nobien.net/wp-content/uploads/2010/06/rANT-Screenshot-04-300x231.jpg)](http://blog.nobien.net/wp-content/uploads/2010/06/rANT-Screenshot-04.jpg)

Lastly, here you can see the output from the compile-and-debug target after running it with the passed in parameters...

[![](http://blog.nobien.net/wp-content/uploads/2010/06/rANT-Screenshot-05-300x231.jpg)](http://blog.nobien.net/wp-content/uploads/2010/06/rANT-Screenshot-05.jpg)

So thats it! And if anyone thinks this would be useful to them leave some comments and/or ideas.

P.S. I built it with [robotlegs](http://www.robotlegs.org/) ;)
