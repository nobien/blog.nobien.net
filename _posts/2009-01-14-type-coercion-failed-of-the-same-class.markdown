---
comments: true
date: 2009-01-14 17:55:26
layout: post
slug: type-coercion-failed-of-the-same-class
title: Type Coercion Failed of the Same Class?
wordpress_id: 112
categories:
- actionscript 3
- flash
---

So I have this issue that I just can't seem to figure out. The Flash Player keeps throwing a Type Coercion Failed error at runtime. Usually when this error pops up I have no problem figuring it out because its usually two different object types. However, in this case, this time its of the same type. It reads:




> TypeError: Error #1034: Type Coercion failed: cannot convert super.secret.package::BeverageAsset@2f2b96a1 to super.secret.package.BeverageAsset.
> 
> 





Odd, right? Ok, so now let me explain the structure a bit. At the top level is app.swf. This SWF does not include the BeverageAsset class compiled into it. This SWF manages the loading and unloading of other SWF files that represent sections. One of these sections is recipes.swf. This SWF is loaded into app.swf and is the first SWF to have BeverageAsset compiled into it. This SWF also loads various SWF display assets in order to function properly. One of these SWF's is beverage01.swf. These assets are of type BeverageAsset. This class is used as the document class for each display asset which also has two movie clips on the stage.




So, when I compile recipes.swf and it runs standalone there is no type coercion failure. I only get the error when recipes.swf is loaded and run within app.swf. Now in my experience, I thought all child SWF's loaded into an app, unless explicitly told not to, would use app.swf's application domain unless a class does not exist yet. So once recipes.swf is loaded into app.swf, since BeverageAsset is compiled into recipes.swf, it would be added to the application domain. Thus, once beverage01.swf is loaded into recipes.swf the BeverageAsset class will already be in the application domain and that one will be used.




I mean, am I totally wrong? I'm totally baffled. Any help would be greatly appreciated.




UPDATE!




Thanks Steven, for pointing me in the right direction. My assumptions about the applicationDomain for any loaded SWF was entirely wrong. I really have no idea why I assumed everything was added to the system domain as SWF's are loaded into the app. So what I have done now is define the application domain of any Loader object's context.



    
    var context:LoaderContext = new LoaderContext( false, ApplicationDomain.currentDomain );
    loader.loader( request, context );




This has eliminated any type coercion errors when loading SWF's and has allowed me to refer to them as a strongly type object. Thanks again Steven. And, man, I really need a new code formatter.




