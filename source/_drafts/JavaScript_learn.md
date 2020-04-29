---
title: JavaScript 的学习记录
tags: [Angular]
categories: [Angular]
date: 2020-04-25 17:46:19
---


> 记录学习 JavaScript 的过程

<!-- more -->


# 1.参考
* 1.[W3School JavaScript Tutorial](https://www.w3schools.com/js/default.asp)


## 2.关键词的理解

#### 1.[`var` 与 `let` 的不同](https://www.w3schools.com/js/js_let.asp)

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

