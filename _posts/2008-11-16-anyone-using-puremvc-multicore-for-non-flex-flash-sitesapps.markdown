---
comments: true
date: 2008-11-16 19:37:10
layout: post
slug: anyone-using-puremvc-multicore-for-non-flex-flash-sitesapps
title: Anyone Using PureMVC Multicore For Non-Flex Flash Sites/Apps?
wordpress_id: 89
categories:
- actionscript-3
- design patterns
- frameworks
---

Lately I've been experimenting with PureMVC. I'm really curious if it could aid in the development of side projects and even projects at work. In fact, I gave it a go on the project I'm currently on at work right now. All started out well, but then I ran into a dilemma. Basically, if you know anything about PureMVC, a mediator is register for a view component at runtime. Not being a total MVC genius, I figured I'd write some sort of proxy to load a section and register a mediator for the section once it was loaded.



Let me outline my situation hypothetically...

1. I have an Application class. This class is the document class of an FLA. The application has a navigation component in it. Each navigation button corresponds to a section. Hypothetically these sections are Home, Gallery and News.

2. I have 5 mediators. ApplicationMediator , NavMediator, HomeMediator, GalleryMediator, and NewsMediator. ApplicationMediator and NavMediator are registered in the shell during startup. Note, I'm using the Flash IDE so the Nav component is on the stage already and a child of Application.

3. NavMediator listens for clicks on the nav component and sends out a note to load a section. The ApplicationMediator hears this note and unloads the current section (if there is one) then loads the necessary section into a new Loader object. By default the home nav button is selected to load the home section when the app starts.

4. Once a section is loaded, lets assume its the Home section, the HomeMediator must be registered in the View. This is where I got stumped. Here are my two initial ways of register the mediator:

4.A. After loading is complete, the HomeMediator is registered in the view from ithin ApplicationMediator like so:

    
    facade.regsiterMediator( new( HomeMediator( loader.content as Home ) );


The problem with this is that it requires HomeMediator be imported into ApplicationMediator (including the mediators for the other sections as well). Additionally, within HomeMediator (and other section nediators), I have a getter for the view component that looks like this:

    
    public function get home():Home
    {
       return viewComponent as Home;
    }


This ends up compiling Home (the section's FLA document class) into the main application SWF in addition to HomeMediator. I certainly don't want this to happen. I want all the code for the home section to be contained within the home SWF.

4.B. Rather than register the mediator in the ApplicationMediator, I would register the HomeMediator in the constructor of the Home component similar to the way the Application starts up. Like so:

    
    public function Home()
    {
       ApplicationFacade.getInstance().registerMediator( new HomeMediator( this ) );
    }


In some cases I would also manually send a notification to initialize the section so that it would run standalone. This method requires that I import ApplicationFacade into the Home component, thus compiling any classes that are imported into that class (commands, and the Application view component) into the Home SWF. I don't want this to happen either because I don't want to bloat each section with main application code.

So therein lies my dilemma. Each section is unloaded when a new section is requested to keep the memory usage of the site/app to a minimum because, in the real project, each section is rather large (some even use Papervision). I'm just totally stumped when to register the mediator for the loaded section without having unnecessary code compiled into the shell or the section.

Also, thanks to Simon for making me rewrite my situation a bit better. It was perhaps a bit confusing before.

I resorted to the PureMVC forums and was told that the Multicore version of PureMVC would solve my problems. I haven't had time to look into Multicore yet, but I was wondering if anyone out there has used the Multicore version on a, what might be, smiliar project (one that loads and unloads sections). Also note, that I'm not using Flex here, I'm using the Flash IDE (and FlashDevelop) to publish my content. So I'd love to hear the thoughts, experiences, etc from any one thats gone this route or has some sort of solution for using the non-multicore version of PureMVC. Thanks in advance.
