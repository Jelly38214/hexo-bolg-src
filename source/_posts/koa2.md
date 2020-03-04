---
title: Koa2
date: 2018-04-14 16:09:11
categories:
  - koa
---

It's been a while since last post as I worked for a new company and so busy.
I can't stand the callback hell in Express, so I learn koa2 to replace express recently. I do some practices and here is my repository `git@github.com:Jelly38214/koa2.git`.

This repository include several branch and each branch is a chapter for learning basic knowledge.

1.  master
2.  http_response_type
3.  middleware
4.  router
5.  cookie
6.  upload_file
7.  debugger
8.  error_handle

you can study knowledge above step by step.

---

## Coding

> [Error Handling](https://github.com/koajs/koa/blob/master/docs/error-handling.md)

In koa2 we use async/await to handle callback instead of co&generator. When error happened, we need to know and deal it.

Koa has its default error handler to resolve error when we dont provide us error handler.

`The default error handler is essentially a try-catch at the very beggining of the middleware chain. It will use a status code of` err.status `, or by default 500. If` err.expose `is true, then` err.message `will be the reply. Otherwise, a message generated from the error code will be used(e.g.for the code 500 the message "Internal Server Error" will be used ). Finally, throw err and let global error listener capture and do something.`

```javascript
// default error handler
app.use(async (ctx, next) => {
  try {
    await next();
  } catch (err) {
    err.status = err.statusCode || err.status || 500;
    throw err; // throw err
  }
});
```

> The Error Event
> Error event listeners can be specified with `app.on('error')`. If no error listener is specified, a default error listener is used.
> `Error listener receive all errors that make their way back through the middleware chain`, if an error is caught and not thrown again, it will not be passed to the error listener(`this means if you catch error and deal it, then you dont throw it, so the error listerner neither global one you provide nor default one is cant receive the error event.`). If no error event listener is specified, then `app.onerror(default error listener)` will be used which simply log the error if `error expose` is true and `app silent` is false.

`summary: app.on('error',(err) => {}) to add a global error event listener`
