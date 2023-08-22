---
title: es6中的字符串扩展
date: 2023-08-02 11:00:00 +0800
categories: 前端
---

## 字符的Unicode表示法

在`\u0000`~`\uFFFF`之间的字符可以直接表示，超出该范围的可以采用双字节或大括号的形式表示。

```javascript
"\u{20BB7}"
// "𠮷"

"\u{41}\u{42}\u{43}"
// "ABC"

let hello = 123;
hell\u{6F} // 123

'\u{1F680}' === '\uD83D\uDE80'
// true
```

## 字符串的遍历器接口

在 ES6 中，可以通过`for...of`来遍历字符串。它最大的优点是可以识别大于`0xFFFF`的码点，传统基于`Array.length`的`for`循环无法识别这样的码点。

```javascript
let text = String.fromCodePoint(0x20BB7);

for (let i = 0; i < text.length; i++) {
  console.log(text[i]);
}
// " "
// " "

for (let i of text) {
  console.log(i);
}
// "𠮷"
```

## 几个不能直接使用的特殊字符

以下几个字符不能直接使用，而只能使用转义字符：

- U+005C：反斜杠
- U+000D：回车
- U+2028：行分割符
- U+2029：段分割符
- U+000A：换行符

U+2028：行分割符，U+2029：段分割符。正则表达式不允许出现这两个字符。JSON 也不允许直接包含正则表达式。所以在JSON 中使用这两个字符的话，有可能会报错。

## `JSON.stringfy()`的改造

根据标准，JSON 数据必须是 UTF-8 编码。但是，现在的`JSON.stringify()`方法有可能返回不符合 UTF-8 标准的字符串。具体来说，UTF-8 标准规定，`0xD800`到`0xDFFF`之间的码点，不能单独使用，必须配对使用。为了确保返回的是合法的 UTF-8 字符，[ES2019](https://github.com/tc39/proposal-well-formed-stringify) 改变了`JSON.stringify()`的行为。如果遇到`0xD800`到`0xDFFF`之间的单个码点，或者不存在的配对形式，它会返回转义字符串，留给应用自己决定下一步的处理。

```javascript
JSON.stringify('\u{D834}') // ""\\uD834""
JSON.stringify('\uDF06\uD834') // ""\\udf06\\ud834""
```

