---
title: wait_sleep_function
date: 2018-01-20 11:15:56
tags: async/await;promise;thread
---

> Accomplish of wait(sleep) function in javascript

There are some ways to pause javascript thread. e.g. `alert`,`confirm`,`prompt`,`http sync and time-consuming operation`.

But,javascript just has one thread to execute code, if you execute some code taking much time, it will freezes the browser(and probably cause the browser to stop running the script entirely.).

So, in some case, there is not `real` wait function.

let's accomplish wait function asynchronously.

```javascript
  const wait = (ms) => new Promise((resolve,reject) => setTimeout(resolved,ms))
```

```javascript
  const prom = wait(2000)
  const showdone = () => console.log('done')
  prom.then(showdone)

```

wait(2000) will return a promise which status is fulfill,and then execute `console.log('done')` in 2000ms.

`But there's a problem all of your code need to wrapped in the then callback, if you want them to be execute in 2000ms. beacuse promise executes asynchronously.`

```javascript
  /**use async/await & IIFE for elegant*/
  (async () => {
    await wait(2000)
    console.log('done')
  })()
```


Reference:
  * [is-it-possible-to-allow-waiting-in-the-main-thread-in-javascript](https://stackoverflow.com/questions/35875761/is-it-possible-to-allow-waiting-in-the-main-thread-in-javascript)
  * [创建一个javascript wait函数](http://www.zcfy.cc/article/let-s-make-a-javascript-wait-function-hacker-noon)
