---
title: Convert ES6 into ES5 with Babel 
date: 2018-02-01 22:54:51
type: "categories"
categories:
- ES6
- Babel
---

## How ES6 features convert into ES5 by [Babel](https://babeljs.cn/repl/).

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

> what is difference between commonjs module & es module

`模块化规范：即为JavaScript提供一种模块编写，模块依赖和模块运行的方案`

`AMD, CMD 运行时异步加载模块， webpack构建时模块合并分块`

`babel将还未被宿主环境直接支持的ES6 module编译为commonjs`

ES6模块的设计思想是尽量的`静态化`，使得编译时就能确定模块的依赖关系，以及输入和输出的`变量`,

CommonJS模块就是对象，输入时必须查找对象属性。`ES6模块不是对象，而是通过export命令显式指定输出的代码，再通过import命令输入`。

```javascript
  // a.js
  var a = 1;
  export a;

  // b.js
  import a from './a';

  // => 上面的代码会报错, export 命令显式指定输出的代码，然后import再引入这代码
  // 也就说，b.js文件就变成了这样
  export a;
  import a from './a' // 可以看babel编译后的源码

  而在模块里面，默认强制使用严格模式，因此不允许出现变量不声明就使用，所以不能这样写
  var a = 1;
  export a;

  而是要这样写
  export var a = 1;
  or
  var a = 1;
  export {a}'
```


*   import is hoisted
*   es module is imported by a specify path in AST. commonjs in runtime to know where module is .
*   es module will export object reference. comonjs export the copy of object;

```javascript
  // imports are hoisted
  
  // a.js
  global.log = str => console.log(str);
  import './b';

  // b.js
  global.log('hello world');

  // => global.log is undefined;
```

the actual code transformed by babel

```javascript
  // a.js
  'use strict'
  require('./b');
  globl.log = function(str) {console.log(str)};

  // b.js
  `use strict`
  global.log('hello world');
```

严格模块主要有一下限制
  1.  变量必须声明后再使用
  2.  函数的参数不能有同名属性，否则报错
  3.  不能使用with语句
  4.  不能对只读属性赋值，否则报错
  5.  不能使用前缀 `0 `表示八进制数， 否则报错
  6.  不能删除不可删除的属性， 否则报错
  7.  不能删除变量 `delete prop`，会报错，只能删除属性 `delete global[prop]`
  8.  `eval`不会在它的外层作用域引入变量
  9.  `eval`和`arguments`不能被重新赋值
  10. `arguments`不会自动反映函数参数的变化
  11. 不能使用`arguments.callee`
  12. 不能使用`arguments.caller`
  13. 禁止this指向全局对象
  14. 不能使用`fn.caller`和`fn.arguments`获取函数调用的堆栈
  15. 增加了保留字（比如 `protected`，`static` 和`interface`）

尤其注意，`this`的限制。ES6模块中，顶层的`this`指向`undefined`，即不应该在顶层代码使用`this`

`ES6模块，export命令规定的是对外的接口，必须与模块内部的变量建立一一对应的关系`

```javascript
  export 1;
  var m = 1;
  export m;

  // 上述代码都会报错，因为没输出接口
```

ES6 export 语句输出的接口，与其对应的值时`动态`绑定关系

import 语句会执行所加载的模块，因此可以有下面的写法

`import "lodash"`

### CommonJS 模块的加载原理

`require`命令第一次加载commonjs模块，就会执行整个模块，然后在内存生成一个对象
```javascript
  {
    id: '模块的名称',
    exports: {...} // 输出的对象
    loaded: true //布尔值表示是否执行完毕
  }
```
以后需要用到这个模块时，就会到这个内存对象的`exports`上取值。

即使再次执行require命令，也不会再执行这个模块，而是到缓存之中取值。 也就是说，CommonJS模块无论加载多少次，都只会在`第一次加载时运行一次`,就返回第一次运行的结果。除非手动清除系统缓存。

