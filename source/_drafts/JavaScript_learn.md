---
title: JavaScript 的学习记录
tags: [Angular]
categories: [Angular]
date: 2020-04-25 17:46:19
---


> 记录学习 JavaScript 的过程

<!-- more -->

# [1.参考](#references)
# [2.关键词的理解](#key_word)
# [3.typeof](#typeof)
# [4.Regular Expressions](#re)
# [5.Array](#array)

***
***
***

# 1.参考<a name="references"/>
* 1.[W3School JavaScript Tutorial](https://www.w3schools.com/js/default.asp)
* 2.[JavaScript Data Types(typeof)](https://www.w3schools.com/js/js_datatypes.asp)

# 2.关键词的理解<a name="key_word"/>

## 1.[`var` 与 `let` 的不同](https://www.w3schools.com/js/js_let.asp)

* 1.作用域

```
var x = 10;
// Here x is 10
{
  var x = 2;
  // Here x is 2
}
// Here x is 2
```

```
var x = 10;
// Here x is 10
{
  let x = 2;
  // Here x is 2
}
// Here x is 10
```

```
var i = 5;
for (var i = 0; i < 10; i++) {
  // some statements
}
// Here i is 10



let i = 5;
for (let i = 0; i < 10; i++) {
  // some statements
}
// Here i is 5
```

* 2.重复定义, `let` 不允许重复定义(也不允许与 `var` 搭配重复定义)， `var` 允许重复定义.否则出现 `Uncaught SyntaxError: Identifier 'x' has already been declared` 错误.


# 3.typeof<a name="typeof"/>

## 1.typeof 与 string 比较

```
const stringNull = null;
console.log(typeof stringNull == "string"); // false

const stringUndefined = undefined;
console.log(typeof stringUndefined == "string");  // false

const stringEmpty = "";
console.log(typeof stringEmpty == "string");  //true
console.log( stringEmpty ? "Not empty" : "empty"); // empty
// Output: is not valid
if (typeof stringEmpty == "string" && stringEmpty) {
  console.log("is valid");
} else {
  console.log("is not valid");
}


const stringValue = "Test";
console.log(typeof stringValue == "string");  // true

const numberValue = 123;
console.log(typeof numberValue == "string");  // false

```

***

# 4.Regular Expressions<a name="re"/>

## 1.参考
* 1.[String.prototype.match()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match)

## 2.match

```
const firmwareVersion: string = '717.31.1593162660';
var firmRe =  /(\d+)(\.\d+)*/i;
var firmFound = firmwareVersion.match(firmRe);
console.log(firmFound);

if (firmFound ) {
    console.log(typeof firmFound[1] == "string"); // true
    console.log(`main version: ${firmFound[1]}`); // main version: 717

/**
 * Output:
 *  [0: "717.31.1593162660"
    1: "717"
    2: ".1593162660"
    groups: undefined
    index: 0
    input: "717.31.1593162660"
    length: 3]
 * 
 */
}
```

# 5.Array <a name="array"/> 

```
let notArr = undefined;

console.log(Array.isArray(notArr));// false

let notArr2 = null;

console.log(Array.isArray(notArr2));// false

let notArr3 = 'abc';

console.log(Array.isArray(notArr3));// false

let aArr = [];

console.log(Array.isArray(aArr)); // true

console.log(aArr.length == 0); // true
```

