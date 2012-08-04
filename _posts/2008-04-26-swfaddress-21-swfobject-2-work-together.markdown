---
comments: true
date: 2008-04-26 05:24:48
layout: post
slug: swfaddress-21-swfobject-2-work-together
title: SWFAddress 2.1 & SWFObject 2 Work Together.
wordpress_id: 56
categories:
- actionscript 2
- actionscript 3
- flash
- javascript
---

I spent a good hour yesterday trying to get [SWFAddress 2.1](http://www.asual.com/swfaddress/) and [SWFObject 2](http://code.google.com/p/swfobject/) to work together. First off, they do work together. Second, there's nothing to it. Then why did it take me an hour? Because I'm a dummy and I had the order in which each JavaScript set was called. In hindsight, this is common sense, but the golden ticket here is calling SWFAddress javascript BEFORE declaring the SWFObject. This more applies to people using the dynamic way of embedding the SWFObject. ie:


    
    
    <head>
        <title>SWFObject v2.0 - step 3</title>
        <meta content="text/html; charset=iso-8859-1" http-equiv="Content-Type"></meta>
        <script src="js/swfobject.js" type="text/javascript"></script>
        <script src="js/swfaddress.js" type="text/javascript"></script>
        <script type="text/javascript">
                    swfobject.embedSWF('website.swf', 'website', '100%', '100%', '9.0.45', 
                    'swfobject/expressinstall.swf', {}, {bgcolor: '#CCCCCC', menu: 'false'}, {id: 'website'});
        </script>
    </head>
    



Silly right? I saw a couple of posts out there of people having a hard time getting this going. Once I flipped the JS declaration, it worked. And I felt kinda dumb. But hey ... That's life. 

BTW: both SWFAddress and SWFObject 2 are pretty amazing and should be worked into your site/app flow if they aren't already.

 
