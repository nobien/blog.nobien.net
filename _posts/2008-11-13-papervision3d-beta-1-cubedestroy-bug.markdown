---
comments: true
date: 2008-11-13 10:55:02
layout: post
slug: papervision3d-beta-1-cubedestroy-bug
title: Papervision3D Beta 1 Cube.destroy() bug?
wordpress_id: 85
categories:
- 3d
- actionscript 3
- flash
---

So I've been working with Papervision3D quite a bit lately for a project at [work](http://www.rokkan.com). I've been creating controllable boxes and using Cube's with a depth of zero as the panels for the box. I used this technique in order to apply different materials to opposite sides of each box panel.

Eventually it came time to destroy these boxes, so I did a big sweep of all the PV3D objects and made sure to remove them from the scenes and destroy their materials. Eventually I got down to the box panels (cubes) and at first glance noticed the Cube.destroy() method. So taking for granted that this is still a beta version of PV3D, I assumed all would be hunky doory and the materials would be destroyed.

Now it was time to check the memory usage. Immediately I noticed the memory usage just kept going up each time I created a scene and/or boxes. For the longest time I had no idea what was causing this. After hours of banging my head, I ended up adding a method to PV3D's MaterialManager class to loop through the materials dictionary and see if anything was still left in it after my own destroy methods. Come to find out, the materials for all the box panels (cubes) we still lingering behind, even after calling the cube's destroy method. So I had to change my own destroy() method to remove the materials manually by doing the following:

    
    _cube.materials.getMaterialByName( "all" ).destroy();
    _cube.materials.getMaterialByName( "front" ).destroy();
    _cube.materials.getMaterialByName( "back" ).destroy();
    _cube.materials.getMaterialByName( "right" ).destroy();
    _cube.materials.getMaterialByName( "left" ).destroy();
    _cube.materials.getMaterialByName( "top" ).destroy();
    _cube.materials.getMaterialByName( "bottom" ).destroy();


At any rate, I hope this helps anyone out there that may be experiencing any memory problems and using cubes!
