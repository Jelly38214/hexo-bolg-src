---
title: Advanced Javascript
date: 2020-06-10 11:01:41
categories:
  - design pattern
tags:
  - javascript
cover: "/img/designpattern/factory-method.png"
---

# Binary In Javascript

- [How to represent any kind of Hex number](<https://stackoverflow.com/questions/2803145/is-there-0b-or-something-similar-to-represent-a-binary-number-in-javascript#:~:text=No%2C%20there%20isn't%20an,octal%20(prefix%200%20)%20formats.&text=In%20ECMASCript%206%20this%20will,uppercase%20B%20(e.g.%200B1111%20).>)
- [Numeric literals in ES6](http://www.ecma-international.org/ecma-262/6.0/#sec-literals-numeric-literals)
  ```javascript
  // ECMAScript 6 have added supported for binary(prefix 0b), octal(prefix 0o), hexadecimal(prefix:0x)
  // Decimal value: 15
  const binary = 0b1111; // 0b prefix
  const octal = 0o17; // 0o prefix
  const oxx = 017; // 0 prefix, it's old versions and  equal to octal
  const hexadecimal = 0xf; // 0x prefix
  ```

# Symbol

```typescript
  task -> phase -> project
```

---

# Type conversions

- `alert` => string (except `Symbol`)
- `Mathematical operations` => number

### String Conversion

### Numeric Conversion

> It the string is not a valid number, the conversion failed and return _NaN_

Numeric conversion Rules:

|       Value        |                                                     Becomes...                                                      |
| :----------------: | :-----------------------------------------------------------------------------------------------------------------: |
|     undefined      |                                                         NaN                                                         |
|        null        |                                                          0                                                          |
| `true` and `false` |                                                     `1` and `0`                                                     |
|       string       | Trim, and remaining string is still empty, the result is `0`.<br> Otherwise, the result is either `number` or `NaN` |

### Boolen conversion

The conversion Rules:

- falsy value( `0`, `null`, `undefined`, `NaN`, `false`, `empty string` ) => `false`
- Other values => `true`

### Object to primitive conversion

> toString return "[object Object]", valueOf return the object itself.
> Symbol.toPrimitive fn must return a primitive value otherwise throw error.
> If toString, valueOf return a object, this object will be converted to a primitive.

Conversion hint:

|             situation             |   hint    |
| :-------------------------------: | :-------: |
|         `alert`, `String`         | `string`  |
| `Number`, `+Obj`, `-`, `> <`, `*` | `number`  |
|  `obj+obj`, `obj == number type`  | `default` |

Conversion Order and the method called:

| order |                                            method                                             |
| :---: | :-------------------------------------------------------------------------------------------: |
|   1   |                              `obj\[`Symbol.toPrimitive`](hint)`                               |
|   2   | `toString -> valueOf` or `valueOf -> toString` <br />(which one is the first depends on hint) |

Conversion Rule:

|               hint                |              method               |
| :-------------------------------: | :-------------------------------: |
| `default` or `number` or `string` | obj\[`Symbol.toPrimitive`](hint)  |
|             `string`              | obj.toString() `||` obj.valueOf() |
|      `number`, or `default`       | obj.valueOf() `||` obj.toString() |

---

# Object property

> There are two kinds of object properties: data property, accessor property

#### Data Property descriptors

> Non-configurable doesn't mean non-writable
> Use Object.defineProperty to define a property which is not exist in the object, the method creates the property with the given value and flags, the rest flags' value are false by default.

**Object Property Descriptors**

```Typescript
  {
    value: any; // property's value
    writable: boolean; // false means value is read-only
    enumerable: boolean; // false means value isn't listed in loops
    configurable: boolean; // false means neither other descriptors nor property can be deleted and changed, BUT value can be changed.
  }
```

**Get property's descriptor?**

| method                           | result                       |
| :------------------------------- | :--------------------------- |
| Object.getOwnPropertyDescriptor  | `Descriptors` or `undefined` |
| Object.getOwnPropertyDescriptors | `Object`                     |

#### Accessor Property descriptors

For accessor properties, there is no `value` or `writable`, but instead of there are `get` and `set` functions

# Object Method

> Method shorter syntax has subtle differences in object inheritance compared with normal usage.

# This In Javascript

In Javascript `this` is free, its value is evaluated at call-time and does not depend on where the method was declared, BUT rather on what object is `before` the dot.

- Arrow function have no `this`

# Constructor

There are two convention for constructor:

- Named with capital letter.
- Be executed with `new` operator.

##### What happen when a constructor is executed:

1. A new empty object`{}` is created and assigned to `this`. // this = {};
2. The function body executes.
3. Assign Constructor's prototype property to `[[Prototype]]` of this ;
4. Return `this`;

##### whether a function is used as a constructor.

Use `new.target` inside function to know whether it's executed with `new` or not
If with `new` operator, `new.target` equal to the function self, or `undefined`.

```javascript
function User(name) {
  if (!new.target) {
    // if run it without new
    return new User(name); // add new for it.
  }

  this.name = name;
}
```

##### Return from constructor explicitlly

In a word, `return with object returns that object, or returns this.` (null also will be ignored.)

# Prototypal inheritance

`__proto__` is a historical `getter/setter` for `[[Prototype]]`;

> A function's prototype property by default is an object with the only perperty `constructor` that points back to the function itself.

```javascript
function Rabbit() {}
// Rabbit.prototype => {constructor: Rabbit}
```

# IEEE754 Specification ❗

![Alt 计算公式](https://user-images.githubusercontent.com/4928035/31599945-25a78f8e-b21a-11e7-90de-93a73f37e32c.png)

- 使用浮点数来表示整数。安全数值就是说在 **(-2^53 ~ 2^53)** 内的整数，有且仅有唯一一个浮点数与其对应
- 使用 64 位存储整数和浮点数，分三部分，S（符号位），E（指数位），M（尾数位）
- IEEE754 规定，在计算机内部保存有效数字时，默认第一位总是 1，所以舍去，只保留后面的部分。比如保存 1.01，只存 01，等读取的时候再把第一位 1 加上去。所以 52 位有效数字实际可以存储 53 位。
- 为了保证当 S ＝ 0，且 E 的首位也为 0 时不被程序当作无效片段舍弃，IEEE754 默认`E(指数位)`的首位为 1，这就使得将 E 转化为十进制时需要减去 1023（此为 64 位浮点数的情形）；

##### Reference

[Wikiwand](https://www.wikiwand.com/zh-hans/%E9%9B%99%E7%B2%BE%E5%BA%A6%E6%B5%AE%E9%BB%9E%E6%95%B8)

# Iterator & for ... of

一个数据结构只要部署了`Symbol.iterator`属性，就被视为具有`iterator`接口，就可以用`for...of`循环遍历它的成员。`for...of`循环可以作为遍历所有数据结构的统一方法

> for...of 范围：Array, Set, Map, TypeArray, Array-like Object(`arguments`, DOM NOdeList), generator, string(这些自备 iterator 接口)

普通对象 object，不自带 iterator 接口，会报错(xxx is not iterable)

# for, for...in, for...of, forEach

- forEach，`break`, `return`都不能跳出循环；
- `for...in`
  - 对于数组，其 key 的类型从 number 变 string('0', '1'...)
  - 不仅遍历己身的 key，也会遍历其原型上的 key
- `for...of`
  - 能配合`break`, `continue`, `return`一起使用

# JSON.stringify, JSON.parse

> JSON 的语法是 Javascript 的子集，因此它并不能表示 Javascript 里所有的值.
> 对象序列化（serialization）是指将对象的状态转换为字符串，也可将字符串还原为对象

JSON.stringify在`对象`中遇见undefined, function, symbol，会将其忽略, 在`数组`中则返回null以保证单元位置不变

| 值                             |       序列化       |        还原        |
| :----------------------------- | :----------------: | :----------------: |
| NaN, Infinity, -Infinity, null |        null        |        null        |
| Date 对象                      | ISO 格式日期字符串 | ISO 格式日期字符串 |
| 函数, undefined, RegExp, Error |        移除        |        移除        |



# TypedArray, ArrayBuffer, DataView

# Event Loop

> An Event loop has one or more task Queue: task, microtask(mutation observer callback, promise ), requestAnimationFrame

As per Spec, perfermance microtask queues only call stack is empty.

Only one mutation observer callback exists in microtask queue.

#### Reference

- [Microtask Event Loop](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)
- [What the heck is the event loop anyway](http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D)
- [Further advanced event loop](https://www.youtube.com/watch?v=u1kqx6AenYw&feature=youtu.be)
- [阮一峰](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)

# Implicit coercion

数组的toString经过了重新定义,将所用元素字符化后用`,`连接起来，等用`join(",")`

ToNumber对0开头的十六进制数并不按十六进制处理，而是按10进制处理

对于对象类型，在ToNumber时，如果其valueOf和toString均不返回基本类型值，则产生TypeError错误
从ES5开始，使用Object.create(null)创建的对象[[Prototype]]属性为null，并且没有valueOf()和toString()方法，因此无法进行强制类型转换

- ToString, ToNumber, ToBoolean, ToPrimitive

|           value            |    ToString    | ToNumber | ToBoolean | ToPrimitive |
| :------------------------: | :------------: | :------: | :-------: | :---------: |
|            null            |     "null"     |    0     |     x     |      x      |
|         undefined          |  "undefined"   |    NaN     |     x     |      x      |
|         true/false         | "true"/"false" |    1/0     |     x     |      x      |
| 22+位整数 /6 个 0 的浮点数 |  指数形式表示  |    x     |     x     |      x      |
| 对象/数组 | 内部\[[class]]属性/元素字符串化后用','连接起来  |    toPrimitive -> valueOf -> toString     |     x     |      x      |

#### Reference

- [你不知道的 JS 中卷](https://weread.qq.com/web/reader/f5d32510715c0190f5ddc42k65132ca01b6512bd43d90e3)
