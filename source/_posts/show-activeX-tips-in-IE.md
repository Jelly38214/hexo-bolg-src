---
title: show-activeX-tips-in-IE
date: 2017-12-26 22:13:36
type: "tags"
tags:
- IE
---
> Conclusion: add trusted website address for showing activeX tooltip

In order to play rtsp protocol live stream in webpage, C++ developer make a plugin for IE browser in window system.
![](http://p150tzuds.bkt.clouddn.com/image/ie/bat.png)

> usage: come into plugin folder through cmd terminal then type 'register.bat' and then click enter in keyboard.

Nothing wrong happened , I click the demo.html which embeded the plugin code.(use file:// protocol to open file in your local).
The IE browser will show some message to notice you whether allow to run activeX plugin for this html.
![](http://p150tzuds.bkt.clouddn.com/image/ie/tooltip.png)
 But it's not work when I deploy the demo.html and use ip address like `http://localhost:3033/demo.html` to open it in IE browser => `the tooltip didn't show`

 That's so weird and I went to ask colleague who used this plugin, and he has not idea.
 I ask colleague who make this plugin and he also dont make sure he can figure it out. Finally he did it.
 here is the solution
 ![](http://p150tzuds.bkt.clouddn.com/image/ie/setting.png)
 ![](http://p150tzuds.bkt.clouddn.com/image/ie/point.png)

 `Get to internet setting and click security option and lower the security level and add trusted website address in trusted list.`

Maybe this issue happened about IE browser security policy, but I dont spend time in digging this issue. Beause I dont like IE, you get it ~~
