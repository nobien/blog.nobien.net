---
comments: true
date: 2008-10-15 14:17:31
layout: post
slug: mipmapping-in-flash-player-unbeknownst-until-now
title: Mipmapping in Flash Player, Unbeknownst Until Now!
wordpress_id: 84
categories:
- actionscript 3
- flash
---

The other day I was messing around with Papervision3D for a prototype at work. I'm no Papervision expert by any means so I was doing all sorts of Google searches to try and find any information relating to what I was trying to achieve. At one point I stumbled across [Tinic Uro](http://www.kaourantin.net/)'s post on how the Flash Player uses [mipmaps](http://www.kaourantin.net/2007/06/mip-map-what.html). I didn't even read the whole post and at the time I thought to myself, "What the hell is this? I don't need to know this." I honestly thought it was just some nerding out about how to use [mipmapping](http://en.wikipedia.org/wiki/Mipmap) to achieve a nicely scaled down image.

That was until today! My co-worker, Faisal, at ROKKAN was having a hard time getting a relatively large PNG image to scale down smoothly with nice aliasing. The image was of a product that has a relatively fine texture on it in certain areas. So when the image was scaled down relatively small, the bitmap data was appearing to be very sharpened. No big deal really, in my opinion, but the client was going crazy about how the image should scale smoothly like it does in Photoshop.

At this point I remembered Tinic's blog post, so I quickly went back to it and read the entire thing. To my surprise there was no programming needed! All we had to do was change the dimensions of the PNG to friendly mipmapping dimensions. That meant either n^2 or n^8. If you do this the Flash Player will automatically use mipmapping when scaling it down. Totally bad ass. Here's an [example](http://blog.nobien.net/examples/mipmapping/) that shows the difference in aliasing of two images that only differ in dimension by 1 pixel.
