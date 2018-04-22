---
title: node_analysis
date: 2018-04-22 13:56:47
tags: node;mechanism;
---

This article is to discuss some operation mechanism inside node.js.

> how `require function` work

let's know two concepts
  * `module wrap function` => decorate your module and let the module could be exported and required.
  * `vm` module => compile string to javascript statement that V8 can execute.

let see what `module wrap function` looks like.

```javascript
  ;(function(_module) {
    // your module code
  })
```

that's it, so brief ,right?

Then let's implement us require function for making mechanism of real require function in node.

we call our require function `_require`,and module object we call `_module`; The let's code.

```javascript
  /**
  * first, we write our functional code. A Add function
  * add.js
  */

  function add(a,b) {
    return a + b;
  }

  _module.exports = add; // caution: we use _module instead of node's module object.
```

Now, implement module wrap function & _module

```javascript
  // moduleWrapFn.js
  const wrap = source => `
    ;(function(_module) {
      ${source} \n
      return _module;
    })
  `;
  module.exports = wrap;
```

```javascript
  // _module.js
  const vm = require('vm'); // one module of node, it can transfer string into real javascript code that v8 can recognize and execute.
  const fs = require('fs'); // read the file.
  const moduleWrapFn = require('moduleWrapFn'); // our moudleWrapFn.js
  function _Module() {
    this.exports = {};
  }

  _Module.prototype._compile = function(filePath) {
    const source = fs.readFileSync(filePath,'utf-8'); // that's why require is a synchronouse function.
    const fn = vm.runInThisContext(moduleWrapFn(source)); // this fn is exported _module exactly .
    return fn;
  }

  const newModule = new _Module();
  module.exports = newModule // return real _module object.
```

```javascript
  // _require.js
  const _module = require('_module');

  module.exports = function(filePath) {
    const fn = _module._compile(filePath);
    return fn(_module).exports; // export what hang on the _module.exports.
  }
```

All functional module we have done. Let's test.

```javascript
  // main.js
  const _require = require('_require');
  const add = _require('add'); // caution: We use our _require instead of require to import add module.

  console.log('5+6 =',add(5,6)); // => 5+6 = 11

```
the result =>
![](http://p150tzuds.bkt.clouddn.com/image/node/node_require.png)

