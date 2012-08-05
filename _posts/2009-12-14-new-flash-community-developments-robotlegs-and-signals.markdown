---
comments: true
date: 2009-12-14 16:19:10
layout: post
slug: new-flash-community-developments-robotlegs-and-signals
title: 'New Flash Community Developments: Robotlegs and Signals'
wordpress_id: 178
categories:
- actionscript-3
- air
- design patterns
- flash
- flex
- frameworks
---

I've come to realize that I'm always a bit hesitant to change. I was late to get a cell phone, late to join Facebook, late to join Twitter, and often late to look into developments within the Flash community. Lately I've been trying to change how often I tune into Flash community developments. As of late I've stumbled across two projects that struck my fancy: [Robotlegs](http://www.robotlegs.org/) and [Signals](http://github.com/robertpenner/as3-signals). Robotlegs is a lightweight MVCS architecture that is very focused on dependency injection. Signals is the result of Robert Penner's frustration with the Flash Player's native event system. Thus he has created an event system inspired by C# events and signals/aslots in Qt. Both looked like interesting projects so I decided to dive into them further. I'll start with robotlegs.



Having some experience with PureMVC and even having some pleasant success with it, I was quite intrigued by some claims that robotlegs is "[PureMVC done right](http://jessewarden.com/2009/10/how-to-use-robotlegs-on-top-of-gaia-part-1-of-3-quickstart.html)". Hmm...At any rate, I can't help but be intrigued by a claim such as this and I always enjoy looking reading about developers' often unwavering opinions about how an MVC pattern should be implemented. So after reading the docs, development forums and checking out the examples, I feel that robotlegs is a great concept and has fulfilled its development philosophy very well. It seems to me that its primary focus has been the use of dependency injection. This allows you to keep the layers of your application nicely decoupled. In my early research, I was a bit annoyed by the fact that the injection module, [SwiftSuspenders](http://github.com/tschneidereit/SwiftSuspenders), was only compatible with the mxmlc compiler. However, the latest version includes the ability to use XML to configure the injection points so I can now use it with the Flash IDE. Yes, I, _and a lot of other people_, still use the Flash IDE to compile applications.

My only complaint about Robotlegs is very subjective. And let me preface this with the fact that I'm not a classically trained developer (I have a degree in design, not computer science). But Robotlegs was rather difficult to get my head partially wrapped around. More specifically, [dependency injection](http://en.wikipedia.org/wiki/Dependency_injection) was, and still is, rather difficult for me to understand. I understand the point of it, but how its implemented in Robotlegs still seems like magic to me. And because I don't understand the dependency injection all that well, its hard for me to feel really comfortable using the framework to develop anything. In comparison, what I really like about [PureMVC](http://puremvc.org/) was that it was pretty easy to learn and the core concepts are also easy to understand (at least to me). Regardless, this is not to say that Robotlegs is any better or worse than PureMVC.

Unlike my apprehensiveness for Robotlegs, I am really excited about Robert Penner's [Signals](http://github.com/robertpenner/as3-signals) project. I stumbled across this project while researching Robotlegs. I didn't quite understand it at first, but after checking out the source and giving a go I became instantly hooked on the idea and hope I can integrate it into my future projects. Basically what Penner has done is create an event/notification system that can pass around strongly typed objects to listeners instead of the native/traditional [Event](http://livedocs.adobe.com/flash/9.0/ActionScriptLangRefV3/flash/events/Event.html) object. Signals are, to put it simply, an object that represents an event. Thus a signal object has its own API for registering and unregistering listeners. Signals also gives you the ability to define the types of signals an object should possess in your interfaces. This fulfills one of my biggest wishes for interfaces: a way to define the types of events an object should have to dispatch. As of now the project seems to still be in development with Penner experimenting how to make working with native events a bit easier. I highly recommend looking into his work, but don't expect much as he's apparently in Thailand until the beginning of January.
