---
title: iconfont-bug-in-firefox
date: 2017-12-19 21:39:30
tags:
---
> Conclusion: iconfont(alibaba) can't display properly in firefox

Matter: I like iconfont and always using it as a icon in webpage, cause it will not be blurry with resizing webpage, and not need to create http request. Today, I found something weird, some icon can not display in firefox, that makes me confused. I checked my code and comfirm that there no problem in my code, so I went to goole it.
I found this problem happened to many FEers, and there are many way to solve it. Let's talk about what make this happen before giving out the solution. 
The most important reason is that firefox do not allow browser to load iconfont file when it's in cross-origin situation for security. So you fix the cross-origin then you fix this issue.
Second browser user use the other font forcely, in this case, the solution is going to set the browser and let browser can use others font like iconfont.

