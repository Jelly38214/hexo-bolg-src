---
title: Webpack
date: 2018-06-20 12:12:00
type: "categories"
categories:
-	webpack
---

经过webpack构建的包，本身就是一个IIFE（立即自执行函数），其接收一个模块函数表达式数组作为函数参数
```javascript
    (function(modules) { // webpackBootstrap
        // 模块缓存对象
        var installedModules = {};

        // 模块导入函数
        function __webpack_require__(moduleId) {

        }

        // 模块函数表达式数组
        __webpack_require__.m = modules;

        // 模块缓存
        __webpack_require__.c = installedModules;

        // getter定义函数（commonjs）
        __webpack_require__.d = function(exports, name, getter) {

        }

        // getter定义函数(es module) 
        __webpack_require__.n = function() {}

        // 判断是否属于自身属性
        __webpack_require__.o = function() {}

        // webpack公共路径
        __webpack_require__.p = "";

        // 加载模块并返回接口
        return __webpack_require__(__webpack_require__.s = 0);
    })([
        /* 0 */ // => 模块的索引
        (function(module,exports) {
            // code
        })
    ])
```

webpack会给每个引入的模块添加序号，用于模块缓存对象的属性值，以及模块导入函数使用来导入指定序号的模块；
通过module.__esModule来判断是否是ES6 Module;
对于一个模块，它采用commonjs导出模块(module.exports = xxxx)，那么它在模块函数表达式中长这样
```javascript
    (function(module, exports) {})
```

如果它采用ES6 Module导出（export default xxx），那么它长这样
```javascript 
    (function(module, __webpack_exports__, __webpack_require__)) {
        "use strict";
    }
```

存在一个harmony-module的模块，用于commonjs和ES module之间的转换，
当你的环境是浏览器时，建议全部使用ES6 module标准来编写模块，这样webpack就不会产生harmony-module这个模块出来。
`注意`：虽然可能打包不出错，但在用import来导入commmonjs模块，有可能webpack-bundleanalyze不会正确显示包的依赖关系

__WEBPACK_IMPORTED_MODULE_0__modulex__，webpack会递归地遍历模块依赖，并赋值给__WEBPACK_IMPORTED_MODULE_0__moduleX__的变量（X是序号）

harmony-moduel 长这样，一般位于序号0的位置
```javascript
    /* 0 */
 	/***/
 	(function (module, exports) {

 		module.exports = function (originalModule) {
 			if (!originalModule.webpackPolyfill) {
 				var module = Object.create(originalModule);
 				// module.parent = undefined by default
 				if (!module.children) module.children = [];
 				Object.defineProperty(module, "loaded", {
 					enumerable: true,
 					get: function () {
 						return module.l;
 					}
 				});
 				Object.defineProperty(module, "id", {
 					enumerable: true,
 					get: function () {
 						return module.i;
 					}
 				});
 				Object.defineProperty(module, "exports", {
 					enumerable: true,
 				});
 				module.webpackPolyfill = 1;
 			}
 			return module;
 		};
 	}),
```

转换commonjs为es6 module
```javascript
    (function (module, __webpack_exports__, __webpack_require__) {

 		"use strict";
 		/* WEBPACK VAR INJECTION */
 		(function (module) { /* harmony import */
 			var __WEBPACK_IMPORTED_MODULE_0__module3__ = __webpack_require__(4);
 			/* harmony import */
 			var __WEBPACK_IMPORTED_MODULE_0__module3___default = __webpack_require__.n(__WEBPACK_IMPORTED_MODULE_0__module3__);

 			module.exports = {
 				name: 'happy',
 				age: 23,
 				job: 'FE',
 				pet: __WEBPACK_IMPORTED_MODULE_0__module3___default.a
 			}
 			/* WEBPACK VAR INJECTION */
 		}.call(__webpack_exports__, __webpack_require__(0)(module)))

```