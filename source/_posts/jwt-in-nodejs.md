---
title: JWT in Node 
date: 2017-12-31 13:51:58
type: "tags"
tags:
- Node
- session
---
>Conclusion: jwt implement in node server

When a user who wanna go into our manage system must login in login page. And he's info must be verify in server and send back a token as authorization when he request other data in manage system.

It's very easy to implement jwt authorization with [jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) module in node server.

Let's start with building a node server with experss.
First, new a folder in your computer and get into it, then execute statement below

`npm init` => then press enter until terminal told you press `yes or not` infomation

`npm install experss jsonwebtoken -S` => to install experss and jsonwebtoken module

when you finished ,you should have these file and folder
![](http://p150tzuds.bkt.clouddn.com/image/jwt_in_node/structure.png)

Now let code our server, and use `postman` that is a chrome extension to test api

![](http://p150tzuds.bkt.clouddn.com/image/jwt_in_node/sendtoken.png)

![](http://p150tzuds.bkt.clouddn.com/image/jwt_in_node/verifytoke.png)

Now node server listen 5000 port.

Use postman and send http request and see what we get.
![](http://p150tzuds.bkt.clouddn.com/image/jwt_in_node/gettoken.png)

we get the token!

let send token and pass verify.

![](http://p150tzuds.bkt.clouddn.com/image/jwt_in_node/passverify.png)

> caution: look at No.20 line ,I use `secretkey` as my private key to create token, then I must use it again when I verify token (at No 26 line) from client-side. create and verify token must use the same private key . otherwist ,authorization fail.

One more thing, We dont want to let token available forever, we mush give it a expiration. pls look at `No 20 line` , I use a configure object `{expiresIn:'30s'}`, it means that this token available in 30 seconds, `when the time between your tow request larger then 30s , the token will invaild`. You can use '1h' => 1hours, '1d' or '1day'.Look jsonwebtoken docs for details.

Finally, happy new year! ^_^





