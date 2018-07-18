---
title: aliyun
date: 2018-07-15 22:01:22
tags: 阿里云;服务器;aliyun;
---

In order to learn backend knowledge, I buy a aliyun server recently. This post records the problem I met in learning.

> nvm current null
I use `nvm` to manage node versions. when I installed it and exit aliyun teminal and log in, I found a problem that temimal told me to do this `nvm install N/A before to use nvm`. Result of command `nvm current` is null. You need to set node version again by `nvm use your node version`.

The reason of above, is `nvm use` only avaliable on current bash terminal, when you open a new terminal, `$PATH`is not included in that node directory.

You need to use a command to solve it.
```javascript
    nvm alias default <version>
    /*example*/
    nvm alias default v8.10.0
```
 to specify default node versions for nvm.


 > Use FileZilla tool to upload files to aliyun server.

something need to be noticed: 
    
    * port `22` when use `sftp` protocol
    * port `21` when use `ftp` protocol

otherview, you cant connect the server with wrong port.
