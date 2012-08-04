---
comments: true
date: 2009-06-03 16:10:03
layout: post
slug: runtime-font-loading-and-static-textfield-conflicts
title: Runtime Font Loading and Static TextField Conflicts
wordpress_id: 150
categories:
- actionscript 3
- flash
---

So I have this dilemma. It's unbelievably frustrating. I want to believe a feasible workaround exists, but after hours of messing around, I couldn't come up with anything. Allow me to explain, maybe some of you have an idea of what to do.

I'm working on a Flash site that needs to handle various languages and lots of dynamic text. In order to facilitate this, I've decided to implement runtime font loading and embedding. Each font is exported into its own SWF file and a class is attached to it. For instance, I have ArcherBold.fla which has one font item in the library named ArcherBold that is set to export for ActionScript and the class fonts.ArcherBold (which extends flash.text.Font) is attached to it.



Within the application, I load the specified fonts that are defined in a config file. This config file specifies the the font name, and the class name, and the path to the SWF file. For example, a font definition for the site for Archer Bold looks like so:

    
    <font>
      <fontName>Archer Bold</fontName>
      <fontClass>fonts.ArcherBold</fontClass>
      <fontPath>shared/fonts/archerbold.swf</fontPath>
    </font>


The site then loads the SWF file, then registers the font class with the global Font.registerFont() method. With this in place, I can then populate TextFields with HTML formatted text and font tags to render the text with the desired font. For example:

    
    <font face="Archer Bold" size="20">My Text</font>


All seems fine and dandy there. No problems whatsoever. But then an odd case came up. I started placing a few static TextFields in parts of the site that use the same font (Archer Bold) that is loaded at run time. Can you imagine what happens next? Surprise! The dynamic text fields no longer render using the run time loaded fonts. In fact, they don't render at all! 

I tried all sorts of useless things to alleviate the problem but to no avail. A coworker and I did did come up with something, but its a totally convoluted workaround. Basically, you have to have to have two copeis of the same font, but adjust the name. For instance, you would have to use a program to duplicate the font and give it another name, such as Archer Bold Static. Anyone placing static text in an FLA would have to use that font instead of the original font. That's just ridiculous if you ask me. The only other option I can see is to make all TextFields dynamically populated, but that would just be a pain in the ass. Especially if we start to include SWFs created by inexperienced developers or designers.

Mind you, I'm still on CS3 (I know, right?). Perhaps CS4 and Flash Player 10 can solve this problem. If anyone knows, I'd greatly appreciate some help.

**Update - June 4th**: So I installed the trial version of Flash CS4 and ran a test using Flash Player 10. Surprise! Same result. Arrrrgggghhh!
