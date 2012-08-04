---
comments: true
date: 2008-05-12 10:36:46
layout: post
slug: more-than-two-simultaneous-key-presses-and-keyboardeventkey_down-woes
title: More Than Two Simultaneous Key Presses and KeyboardEvent.KEY_DOWN Woes
wordpress_id: 62
categories:
- actionscript 3
- flash
---

I'm rather annoyed right now. For whatever reason, I didn't think that determining if the user has 3 keys pressed at once would be so difficult. Attempt to press all the arrow keys that correspond to the arrows being displayed...

[kml_flashembed movie="http://www.nobien.net/dev/keyboardtest/keyboard_test.swf" height="264" width="500" /]

Sometimes it works, sometimes it doesn't and I can't understand why or find a pattern to the malfunction. But basically whats happening is at some point the KEY_DOWN event just isn't dispatched for a particular arrow key once two of the other arrow keys have been pressed. Does anyone have any freaking idea why this is happening?
