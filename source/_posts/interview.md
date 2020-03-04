---
title: Interview 
date: 2018-02-10 22:18:47
type: "categories"
categories:
- interview
---

Prepare for daocloud

> `for...in` & `for...of` & `forEach`

all of them above are used to traverse Object/Array. But these are some difference.

1. can't use `break` or `continue` in `forEach` to jump out loop even if you use `return`. since it does not support.


2. for...in(use for Object normally)
    * index of Array is type of `Number`. But index will change and become `string` in for...in syntax
    * in some case, `for...in` traverse key without sequence.

3. for...of(use for Array)
  * can work with `break`,`continue`,`return`
  * cant use for Object
  * use for any structure as long as it have iteration interface

> debounce & throttle

```javascript
  // debounce
  let isDebounce = null
  document.getElementById('input').oninput = () => {
    isDebounce ? clearTimeout(bounce):''
    isDebounce = setTimeout(() => {
      // code your logic here
    },300)
  }

  // throttle
  let isThrottle = fasle
  document.getElementById('input').oninput = () => {
    if(isThrottle) {
      return
    }
    isThrottle = true
    setTimeout(() => {
      // execute logic and change status
      isThrottle = false
    },300)
  }
```
> scroll overflow(称为滚动溢出，页面的某块滚动到底后再滚动也不带动整个页面滚动)

```javascript
  function preventScroll(id) {
    const _this = document.getElementById(id)
    if(navigator.userAgent.indexOf('Firefox') > 0) {
      _this.addEventListener('DOMMouseScroll',event => {
        _this.scrollTop += e.detail > 0 ? 60 : -60 // move 60px every scroll
        event.preventDefault()
      },fasle)
    } else {
      _this.onmousewhell = event => {
        event = event || window.evnet // for IE8-
        _this.scrollTop += event.wheelDelta > 0 ? -60 : 60
      }
  }
```