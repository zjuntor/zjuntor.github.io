---
title: After having xulrunner updated to 2.0.1
author: torrge
comments: true
layout: post
permalink: /2011/05/after-having-xulrunner-updated-to-2-0-1/
categories:
  - Programming
tags:
  - Eclipse
  - WebKit
  - xulrunner
---
After a recent package update, my Eclipse began crashing with alarming frequency. After some research, it seems that Eclipse(helios) does not support xulrunner 2+, but Firefox 4 uses xulrunner 2+ now.

Instead to fix this compatibility problem, I chose to set Eclipse to use WebKitGTK.

> edit eclipse.ini in eclipse home, & add the following line:
> 
> -Dorg.eclipse.swt.browser.UseWebKitGTK=true

But this solution seems not work for me. Then, I searched it and found the problem. For using gnome 3, the WebKit library name was changed from libwebkit-x.so to libwebkitgtk-x.so. So I did

> in /usr/lib/ :
> 
> sudo ln -s libwebkitgtk-x.so libwebkit-x.so

Now, my Eclipse works as usual again.

ref: <a href="http://blog.dmaggot.org/2010/12/using-webkit-and-eclipse-helios/" target="_blank">Using WebKit and Eclipse Helios</a>