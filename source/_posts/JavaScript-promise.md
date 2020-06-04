---
title: JavaScript Promise Learn
tags: [javaScript]
categories: [javaScript]
date: 2020-05-12 17:46:19
---

> JavaScript Promise 的用法

<!-- more -->


## 1.参考
* 1.[TypeScript Promises Examples](https://www.positronx.io/angular-8-es-6-typescript-promises-examples/)
* 2.[MDN 使用 Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises#Promise_%E6%8B%92%E7%BB%9D%E4%BA%8B%E4%BB%B6)
* 3.[MDN Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
* 3.[Master the JavaScript Interview: What is a Promise?](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261)


## 2.例子

* 1.Attach Success Handler with Promise

```
function asyncAction() {
  var promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("Async is done!");
      resolve();
    }, 1500);
  });
  return promise;
}

asyncAction().then(
  () => console.log("Resolved!")
);

// Output
Async is done!
Resolved!

```

* 2.Attaching Error Handler with Promise

```
function asyncAction() {
  var promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log("Async is done!");
      reject('Rejected!');
    }, 1500);
  });
  return promise;
}

asyncAction().then(function(success) { 
    console.log(success); 
}) 
.catch(function(error) { 
   // error handler is called
   console.log(error); 
});

// Output
Async is done!
Rejected!

```

* 2.Attach Multiple Then Handlers with Promise

```
Promise.resolve("Completed")
  .then(
    (value) => {
      return 'Completed Two';
    },
    (error) => console.error(error)
  )
  .then(
    (value) => console.log(value),
    (error) => console.error(error)
  );

/*  Output:
   'Completed'
   'Completed Two'
*


Promise.reject('Rejected')
  .then(
    (value) => console.log(value)
   )
  .then(
    (value) => console.log(value),
    (error) => console.error(error)
  );
 
 /* Output:
 'Rejected'
 
 */

```