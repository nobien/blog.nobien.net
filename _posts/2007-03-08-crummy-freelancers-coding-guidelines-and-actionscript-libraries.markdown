---
comments: true
date: 2007-03-08 08:52:31
layout: post
slug: crummy-freelancers-coding-guidelines-and-actionscript-libraries
title: Crummy Freelancers, Coding Guidelines and Actionscript Libraries
wordpress_id: 14
categories:
- actionscript 2
- flash
---

As of late I've been thinking about just how important it may be to have some structure for fellow Flash Developers at Rokkan. Naturally, this means two things: 1) A coding standards/guidelines document, and 2) some sort of  "company" Actionscript library to pull from to support the guidelines and techniques. I really like the idea of these two things, especially after a little incident that happened recently when we hired a freelance Flash guy to help out during a busy spell.



Unfortunately I had no part in hiring the guy so I couldn't give him a quick quiz or anything like that. But while he was working with us he took care of a few small things that we're pretty much just busy work. I didn't work with him directly, but I had once overheard him say to a designer that he felt like he was behind in his skills and wanted to "redo" the code he had written for a small project. I thought that was a good thing. But to my dismay, when I had to make a change to the work he did when he was not available,  I had one hell of a time trying to figure out the approach, let alone all the code. I didn't think people still put code on movieclip instances! So what should have been a simple promo manager was turned into a complete and utter mess of illogical variable names, horrible encapsulation and bizarre XML techniques. It took me over three hours to make a change that should have taken 20 minutes. This is what sparked my concern.

Its probably like this with most other types of projects and other programming languages, but I almost feel like Flash projects need _extra _attention in order for multiple people to be involved and have everyone understand just exactly whats going on, where to find code, where to put code, where to save assets, etc. So far it seems that every shop, every freelancer, has there own way of doing the same things. Thats the beauty of Flash, right? Yeah, I guess so. Is it bad that I'm finding it annoying when you can't look at someone elses code and immediately  understand it? I often find myself going through someones source code and restyling it, and sometimes changing routines to my liking.

At any rate, over the past week or so I've been writing a guidelines document to try and prevent things like this from happening again.  At first the idea was a bit daunting, but luckily a developer at dClick [posted his guidelines doc](http://blog.dclick.com.br/2007/02/13/adobe_flex_coding_guidelines_english/) on the company blog. This was specifically for MXML and AS3, so I just used the AS3 section as reference for my AS2 guidelines. I also added a bunch of other stuff, like our preferred project folder structure and library folder structure. Forcing myself to create this document was a good exercise is self evaluation too. I started to realize just how lazy I can get sometimes with my code, especially with techniques (I'm pretty anal about my style). So while it was one really tedious project, I'm actually _very_ happy that I forced myself to do it.

Now that I'm happy with the state of the guidelines doc, I've started to move on to working on compiling an AS library that supports the techniques and what not. The first thing that came to mind was an events package. After getting into AS3, I discovered the advantages of event types. I'll never go back to generic event objects. The second thing that came to mind was a utils package. I started organizing a bunch of utility classes that we had created over the course of a few projects into one package. These included StringUtil, ArrayUtil, Timer, LoadQueue, HTMLFormat and DateUtil. I hope to keep adding to this package. The third thing, which I'm moving onto now, is a UI package. This is a bit more complicated, as a lot of UI controls that we created were somewhat custom, and often do not include a substantial amount of flexibility for use in a different context. This will definitely take some time, but I'm up for it!

I'm pretty stoked on the idea of having all this available for contractors, freelancers, and other employees. Although, at times I almost think its total overkill. I'd like to hear some opinions about this type of stuff. In all your experiences, is a guidelines document necessary? Is it good to force people to use a standard code library for things like utils, events, etc? Am I overreacting? Do I need to take things further?

Also, if you'd like to download and critique my guidelines: [.PDF](http://blog.nobien.net/wp-content/uploads/2007/03/rokkanactionscript2codingguidelines.pdf) or [.DOC](http://blog.nobien.net/wp-content/uploads/2007/03/rokkanactionscript2codingguidelines.doc)
