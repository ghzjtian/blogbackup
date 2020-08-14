---
title: Angular Code Style 
tags: [Angular]
categories: [Angular]
date: 2020-04-30 17:46:19
---


> Angular 编码风格与文件结构
> 文章搬运工，只为个人参考之用.

<!-- more -->

## 1.参考
## 2.编写易于阅读的代码
## 3.高效的 Angular 程序编写
## 4.属性的命名
## 5.方法的命名
## 6.类的命名

***
***
***

## 1.参考
* 1.[风格指南](https://angular.cn/guide/styleguide#angular-coding-style-guide)
* 2.[Clean Code Checklist in Angular](https://itnext.io/clean-code-checklist-in-angular-%EF%B8%8F-10d4db877f74)
* 3.[How to define a highly scalable folder structure for your Angular project](https://itnext.io/choosing-a-highly-scalable-folder-structure-in-angular-d987de65ec7)
* 4.[Best practices for a clean and performant Angular application](https://www.freecodecamp.org/news/best-practices-for-a-clean-and-performant-angular-application-288e7b39eb6f/)
* 5.[clean-code-javascript<Github>](https://github.com/ghzjtian/clean-code-javascript)
* 6.[The art of naming variables](https://hackernoon.com/the-art-of-naming-variables-52f44de00aad)
* 7.[Naming your variables!](https://blog.usejournal.com/naming-your-variables-f9477ba002e9)
* 8.[JavaScript Style Guide and Coding Conventions]()

## 2.编写易于阅读的代码
* 1.方法名要明确指出该方法的作用
* 2.添加小而简单的方法, 每个方法只完成一个功能.
* 3.移除没用到的代码.
* 4.避免添加注释
* 5.避免使用 any 类型.



## 3.高效的 Angular 程序编写
#### 1.懒加载 Modules
#### 2.RxJS 的应用
#### 3.Subscribe in template.(use async)
#### 4.Clean up subscriptions

```
// Before
iAmAnObservable
    .pipe(
       map(value => value.item)     
     )
    .subscribe(item => this.textToDisplay = item);


// After 
private _destroyed$ = new Subject();
public ngOnInit (): void {
    iAmAnObservable
    .pipe(
       map(value => value.item)
      // We want to listen to iAmAnObservable until the component is destroyed,
       takeUntil(this._destroyed$)
     )
    .subscribe(item => this.textToDisplay = item);
}
public ngOnDestroy (): void {
    this._destroyed$.next();
    this._destroyed$.complete();
}
    
```

#### 5.编写安全的 string

> 如果 string 只能去指定的几个值，最好定义一些可能性的值.

```
// Before
private myStringValue: string;
if (itShouldHaveFirstValue) {
   myStringValue = 'First';
} else {
   myStringValue = 'Second'
}

// After
private myStringValue: 'First' | 'Second';
if (itShouldHaveFirstValue) {
   myStringValue = 'First';
} else {
   myStringValue = 'Other'
}
// This will give the below error
Type '"Other"' is not assignable to type '"First" | "Second"'
(property) AppComponent.myValue: "First" | "Second"

```


## 4.属性的命名
* 1.Arrays

```
// great - "names" implies strings
const fruitNames = ['apple', 'banana', 'cucumber'];

// great
const fruits = [{
    name: 'apple',
    genus: 'malus'
}, {
    name: 'banana',
    genus: 'musa'
}, {
    name: 'cucumber',
    genus: 'cucumis'
}];
```

* 2.Booleans

```
// good
const isOpen = true;
const canWrite = true;
const hasFruit = true;
```

```
const checkHasFruit = (user, fruitName) => (
    user.fruits.includes(fruitName)
);

const hasFruit = checkHasFruit(user, 'apple');

```

* 3.Numbers

```
// good
const minPugs = 1;
const maxPugs = 5;
const totalPugs = 3;
```

* 4.Functions.` A good format to follow is actionResource. For example, getUser.`

```
// good
getUser(userId);
calculateTotal(items);
// I like it!
toDollars('euros', 20);
toUppercase('a string');
```

```
// good
const newFruits = fruits.map(fruit => {
    return doSomething(fruit);
});
```


## 5.方法的命名
* 1.click 点击事件

```
(click)="onCancelClick()"
(click)="onSaveClick()"
```


## 6.类的命名
#### 1.Model

> 参考 Angular [`英雄指南` ](https://angular.cn/tutorial) 中 Model 的命名规则.

* 1.文件名: `hero.model.ts`, 类名为 `Hero`, 如

```
export interface Hero {
  id: number;
  name: string;
}
```





