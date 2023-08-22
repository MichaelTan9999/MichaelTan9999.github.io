---
title: es6中的let与const关键字
date: 2023-07-31 09:00:00 +0800
categories: 前端
---

本篇是在系统性学习阮一峰的 [*ECMAScript6 入门*](https://es6.ruanyifeng.com)所作的笔记。

## `let`关键字

ES6 新增了`let`命令，用来声明变量。它的用法类似于`var`，但是所声明的变量，只在`let`命令所在的代码块内有效。用`var`声明的变量存在“变量提升”，即在变量声明前可以被使用，值为`undefined`。但是`let`不存在变量提升，声明前使用会抛出错误`ReferenceError`。**只要块级作用域内存在`let`命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。**

> `for`循环的计数器，就很合适使用`let`命令
> {: .prompt-tip }

```javascript
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```

**`for`循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。**上面代码正确运行，输出了 3 次`abc`。这表明函数内部的变量`i`与循环变量`i`不在同一个作用域，有各自单独的作用域（同一个作用域不可使用 `let` 重复声明同一个变量）。

```javascript
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}

```

上面代码中，存在全局变量`tmp`，但是块级作用域内`let`又声明了一个局部变量`tmp`，导致后者绑定这个块级作用域，所以在`let`声明变量前，对`tmp`赋值会报错。ES6 明确规定，如果区块中存在`let`和`const`命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。在代码块内，使用`let`命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。**暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。**

```javascript
typeof undeclared_variable // "undefined"
typeof x; // ReferenceError
let x;
```

相反，如果一个变量根本没有被定义，上面的代码是不会报错的；而在`let`声明前使用`typeof`的话，则会抛出错误。

`let`关键字有两个重要的使用场景，第一个场景是防止内层的变量覆盖外层的同名变量，第二个场景是防止用来计数的循环变量泄露为全局变量。

## 块级作用域

ES6 允许块级作用域的任意嵌套。下面代码使用了一个三层的块级作用域，每一层都是一个单独的作用域。第四层作用域无法读取第五层作用域的内部变量。

```javascript
{{
  {let insane = 'Hello World'}
  console.log(insane); // 报错
}};
```

ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于`let`，在块级作用域之外不可引用。但是为了减轻兼容性问题，对于**浏览器**有如下规定：

- 允许在块级作用域内声明函数。
- 函数声明类似于`var`，即会提升到全局作用域或函数作用域的头部。
- 同时，函数声明还会提升到所在的块级作用域的头部。

考虑到环境导致的行为差异太大，应该避免在块级作用域内声明函数。如果确实需要，也应该写成函数表达式，而不是函数声明语句。函数表达式范例：

```javascript
// 块级作用域内部，优先使用函数表达式
{
  let a = 'secret';
  let f = function () {
    return a;
  };
}
```

## `const`命令

`const`声明一个只读的常量。一旦声明，需要立即初始化，且常量值不能修改。`const`命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。

**`const`实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。**对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，`const`只能保证这个指针指向一个固定的地址。

### `const`与对象冻结

对象冻结采用`Object.freeze()`函数，返回一个冻结后的不可变对象。但是冻结后的对象属性中如果含有对象数据类型，那么对于这些对象需要再递归冻结一次，即如下所示。

```javascript
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```

>ES6一共有6种声明变量的方法，`var`，`function`，`let`，`const`，`import`，`class`命令。
>{: .prompt-info }

## 顶层对象属性

顶层对象，在浏览器环境指的是`window`对象，在 Node 指的是`global`对象。ES5 之中，顶层对象的属性与全局变量是等价的，如下所示。

```javascript
window.a = 1;
a // 1

a = 2;
window.a // 2
```

在 ES6 中，为了保持兼容性，`var`命令和`function`命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，`let`命令、`const`命令、`class`命令声明的全局变量，不属于顶层对象的属性，如下所示。

```javascript
var a = 1;
// 如果在 Node 的 REPL 环境，可以写成 global.a
// 或者采用通用方法，写成 this.a
window.a // 1

let b = 1;
window.b // undefined
```

## `globalThis`对象

| 关键字   | 浏览器   | Web Worker | Node     |
| -------- | -------- | ---------- | -------- |
| `window` | 顶层对象 | 不支持     | 不支持   |
| `self`   | 指向顶层 | 指向顶层   | 不支持   |
| `global` | 不支持   | 不支持     | 顶层对象 |

在 ES2020 语言标准层面，可以通过`globalThis`在任何环境下获得顶层对象。在不支持 ES2020 的环境中，可以使用如下的代码片段来获取到顶层对象。

```javascript
// 方法一
(typeof window !== 'undefined'
   ? window
   : (typeof process === 'object' &&
      typeof require === 'function' &&
      typeof global === 'object')
     ? global
     : this);

// 方法二
var getGlobal = function () {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
};
```

