---
title: jajavascript_util_snippets
date: 2018-01-27 16:07:50
tags: util;snippets
---
> ## A collection of CSS snippets(default use latest syntax)

> judge if arr is subset of target targetArr(for primitive type)

```javascript
  const inOf = (arr, targetArr) => {
    let res = true
    arr.map(item => {
      targetArr.indexOf(item) === -1 ? res = false : ''
    })

    return res
  }
```
> a given element exist in a targetArr

```javascript
  const oneOf = (ele, targetArr) => targetArr.indexOf(ele) !== -1
```
