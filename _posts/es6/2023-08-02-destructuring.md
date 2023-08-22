---
title: es6中的变量解构赋值
date: 2023-08-02 09:00:00 +0800
categories: 前端
---

## 数组的解构赋值

本质上来说属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值。

```javascript
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
```

如果解构不成功，那么变量的值等于`undefined`。

```javascript
let [foo] = [];
let [bar, foo] = [1];
```

另一种情况是不完全解构，即等号左边的模式，只匹配一部分的等号右边的数组。这种情况下，解构依然可以成功。

```javascript
let [x, y] = [1, 2, 3];
x // 1
y // 2

let [a, [b], d] = [1, [2, 3], 4];
a // 1
b // 2
d // 4
```

不成功的情况：等式右侧如果是不可遍历的解构——不具备`Iterator`接口的数据结构的话，遍历报错。事实上，只要数据结构具有`Iterator`接口，就可以采用数组的解构赋值。这意味着生成器函数也能作为等式右侧具有`Iterator`接口的数据结构为左侧数据赋值，如下所示。

```javascript
function* fibs() {
  let a = 0;
  let b = 1;
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

let [first, second, third, fourth, fifth, sixth] = fibs();
sixth // 5
```

### 数组解构赋值中的默认值

解构赋值允许指定默认值。ES6 内部使用严格相等运算符（`===`），判断一个位置是否有值。所以，只有当一个数组成员严格等于`undefined`，默认值才会生效。如果一个数组成员是`null`，默认值就不会生效，因为`null`不严格等于`undefined`。

```javascript
let [x = 1] = [undefined];
x // 1

let [x = 1] = [null];
x // null
```

如果默认值是一个表达式，那么这个表达式是惰性求值的，即只有在用到的时候，才会求值。下面的代码中，因为`x`能取到值，所以函数`f`根本不会执行。

```javascript
function f() {
  console.log('aaa');
}

let [x = f()] = [1];
x // 1
```

默认值可以引用解构赋值的其他变量，但该变量必须已经声明。下面最后一个表达式之所以会报错，是因为`x`用`y`做默认值时，`y`还没有声明。

```javascript
let [x = 1, y = x] = [];     // x=1; y=1
let [x = 1, y = x] = [2];    // x=2; y=2
let [x = 1, y = x] = [1, 2]; // x=1; y=2
let [x = y, y = 1] = [];     // ReferenceError: y is not defined
```

## 对象的解构赋值

对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。变量没有对应的同名属性，导致取不到值，最后等于`undefined`。对象的解构赋值的内部机制，**是先找到同名属性，然后再赋给对应的变量**。真正被赋值的是后者，而不是前者。下面代码中，`foo`是匹配的模式，`baz`才是变量。真正被赋值的是变量`baz`，而不是模式`foo`。

```javascript
let { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"
foo // error: foo is not defined
```

对象的解构赋值也支持嵌套。如果解构模式是嵌套的对象，而且子对象所在的父属性不存在，那么将会报错。

```javascript
let obj = {};
let arr = [];

({ foo: obj.prop, bar: arr[0] } = { foo: 123, bar: true });

obj // {prop:123}
arr // [true]

let {foo: {bar}} = {baz: 'baz'}; // 报错
```

对象的解构赋值可以取到继承的属性。

```javascript
const obj1 = {};
const obj2 = { foo: 'bar' };
Object.setPrototypeOf(obj1, obj2);

const { foo } = obj1;
foo // "bar"
```

### 对象解构赋值中的默认值

对象的解构也可以指定默认值。默认值生效的条件是，对象的属性值严格等于`undefined`。

```javascript
var {x = 3} = {x: undefined};
x // 3

var {x = 3} = {x: null};
x // null
```

### 一些注意点

1. 将一个已经声明的变量用于解构赋值，必须非常小心。第一段代码的写法会报错，因为 JavaScript 引擎会将`{x}`理解成一个代码块，从而发生语法错误。只有不将大括号写在行首，避免 JavaScript 将其解释为代码块，才能解决这个问题。第二段代码将整个解构赋值语句，放在一个圆括号里面，就可以正确执行。

```javascript
// 错误的写法
let x;
{x} = {x: 1};
// SyntaxError: syntax error

// 正确的写法
let x;
({x} = {x: 1});
```

2. 解构赋值允许等号左边的模式之中，不放置任何变量名。以下的代码虽然奇怪，但是是合法的。

```javascript
({} = [true, false]);
({} = 'abc');
({} = []);
```

3. 由于数组本质是特殊的对象，因此可以对数组进行对象属性的解构。

```javascript
let arr = [1, 2, 3];
let {0 : first, [arr.length - 1] : last} = arr;
first // 1
last // 3
```

## 字符串的解构赋值

字符串也可以解构赋值。这是因为此时，字符串被转换成了一个类似数组的对象。类似数组的对象都有一个`length`属性，因此还可以对这个属性解构赋值。

```javascript
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"

let {length : len} = 'hello';
len // 5
```

## 数值和布尔值的解构赋值

解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象。由于`undefined`和`null`无法转为对象，所以对它们进行解构赋值，都会报错。

```javascript
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true

let { prop: x } = undefined; // TypeError
let { prop: y } = null; // TypeError
```

## 函数参数的解构赋值

函数参数的解构也可以使用默认值。下面代码中，函数`move`的参数是一个对象，通过对这个对象进行解构，得到变量`x`和`y`的值。如果解构失败，`x`和`y`等于默认值。

```javascript
function move({x = 0, y = 0} = {}) {
  return [x, y];
}

move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]
```

注意，下面的写法会得到不一样的结果。以下代码是为函数`move`的参数指定默认值，而不是为变量`x`和`y`指定默认值，所以会得到与前一种写法不同的结果。

```javascript
function move({x, y} = { x: 0, y: 0 }) {
  return [x, y];
}

move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, undefined]
move({}); // [undefined, undefined]
move(); // [0, 0]
```

`undefined`就会触发函数参数的默认值。

```javascript
[1, undefined, 3].map((x = 'yes') => x);
// [ 1, 'yes', 3 ]
```

## 圆括号问题

原则：**只要有可能，就不要在模式中放置圆括号**。

三种解构赋值不得使用圆括号的情况：

- 变量声明语句
- 函数参数
- 赋值语句的模式

可以使用圆括号的只有一种情况：赋值语句的非模式部分。

```javascript
[(b)] = [3]; // 正确
({ p: (d) } = {}); // 正确
[(parseInt.prop)] = [3]; // 正确
```

## 解构赋值的几个实用用途

1. 交换变量的值
2. 从函数返回多个值
3. 函数参数的定义
4. 提取 JSON 数据
5. 函数参数的默认值
6. 遍历 Map 解构
7. 输入模块的指定方法



