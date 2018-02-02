---
title: Es62Es5WithBabel
date: 2018-02-01 22:54:51
tags: ES6;babel
---

## How ES6 features convert into ES5 by [Babel](https://babeljs.cn/repl/#?babili=false&evaluate=true&lineWrap=false&presets=latest%2Creact%2Cstage-2&experimental=false&loose=false&spec=false&code=%5B1%2C2%2C3%5D.map(n%20%3D%3E%20n%20%2B%201)%3B&playground=true).

> spread/rest object({...{}})

```javascript
var _extend = Object.assign ||
  function(target) {
    for(var i = 1;i < arguments.length; i++) {
      var source = arguments[i]
      for(var key in source) {
        if(Object.prototype.hasOwnProperty.call(source, key)) {
          target[key] = source[key]
        }
      }
      return target
    }
  }
```

//Caution: Why use `Object.prototype.hasOwnProperty.call(source, key)` instead of `source.hasOwnProperty(key)`

Because Javascript does not protect the property name `hasOwnProperty`; thus , if the possiility exists that an object might have a property with this name, it is nessary to use an `external` `hasOwnProperty` to get correct results.

Let's see some example

```javascript
  var foo = {
    hasOwnProperty: function() {
      return false
    },
    bar: 'Here be dragons'
  }

  foo.hasOwnProperty('bar') // always returns false

  // Use another Object's hasOwnProperty
  // and call it with `this` set to foo
  ({}).hasOwnProperty.call(foo, 'bar')  // true

  // It's also possible to use the hasOwnProperty property
  // from the Object prototype for this purpose

  Object.prototype.hasOwnProperty.call(foo, 'bar') // true
```