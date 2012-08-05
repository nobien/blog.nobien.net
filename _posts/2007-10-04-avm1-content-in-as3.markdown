---
comments: true
date: 2007-10-04 05:46:37
layout: post
slug: avm1-content-in-as3
title: AVM1 Content in AS3?
wordpress_id: 38
categories:
- actionscript-2
- actionscript-3
---

So this is a pseudo rant/question for the masses.  

I've been trying to load up some legacy content in CS3/AS3.
I've read all of the liveDocs on this subject and have figured out that you ...
   A. AVM1 content is contained in a AVM1Movie class which you cannot extend
   B. AVM1 content can only communicate over LocalConnection
   C. AVM1 content cannot user loadMovie to load in additional SWFs.  

So what we're talking about here is that AVM1 content is really only good for animating crap when someone in your office only has Flash 8.  Here's why .. You _could_ use LocalConnection to do some cross-scripting, but that would require you to have the FLA for the legacy content ... and this isn't always possible.  At this point, should you have the FLA, the LocalConnection hack is like using the ExternalInterface class to communicate with JavaScript.  It's not bad, but not ideal for cross-scripting between Flash content. 

If you have access to the original FLA, why not just make it a CS3 doc.  I know, I know, you don't want to recode everything .. but apparently you can't use LoadMovie, and I have all but 1 project in my whole career that didn't LoadMovie for something. Apparently you can use LoadMovieNum .. or something .. but at that point you're recoding ... and why not just set up a Loader() to do the deed.  

Why the rant?  Well ... I'm trying to load legacy content into a Flash 9 swf.  However, I do not have access to the original FLAs.  No big deal.  I can load them in and play with them .. etc .. which is about as useful as loading in an animated GIF.  The problem is, each SWF is expecting a variable on _root that would typically be assigned through the HTML EMBED tag or swfObject.  Now if SWFs can get variables from the parent that embedded them, why can't legacy content receive data through the parent (Flash 9) SWF that loaded it?

I know it breaks the whole coding standards of AS3, but would it be too much to be able to assign dynamic variables to that magical AVM1Movie wrapper?  or something?   or do I have no idea what I'm talking about?

