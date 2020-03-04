---
title: Programming Pearls
date: 2018-02-17 20:17:24
type: "tags"
tags:
- algorithm book
---

## Learning programming pearls

> call/apply的first parameter is null, then `this` point to `window`
```javascript
  var aStr = 'strInWindow'
  var bStr = 'strInWindow'
  var str ='done'
  var  object = {aStr:'strInObj',bStr:'strInObj'}
  function hello(str) {
    console.log(`aStr=${this.aStr}, bStr=${this.bStr} ${str}`)
  }

  hello.call(null, str) // => aStr=strInWindow, bStr=strInWindow done
  hello.call(object, str) // => aStr=strInObj, bStr=strInObj done
```

> 不超过32位整数表示的意思， [check here](https://zhidao.baidu.com/question/152860342.html)
