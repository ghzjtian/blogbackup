---
title: Angular Material 自定义效果
tags: [Angular]
categories: [Angular]
date: 2020-05-13 17:46:19
---


> Angular Material 自定义显示的效果

<!-- more -->

## 1.参考
## 2.自定义 `MatInput` 输入框

***
***
***

## 1.参考
* 1.[Angular Material](https://material.angular.io/)
* 2.[Angular Material <CN> ](https://material.angular.cn/)




## 2.自定义 `MatInput` 输入框

#### 1.参考
* 1.[Angular Material matInput control with thousands separator](https://medium.com/angular-in-depth/angular-material-matinput-control-with-thousands-separation-ebcbb7b027f4)
* 2.[Styling mat-form-field input in Angular/Material](https://stackoverflow.com/questions/48540533/styling-mat-form-field-input-in-angular-material)
* 3.[Angular change MatInput Size](https://stackoverflow.com/questions/46502294/angular-change-matinput-size/46502615)
* 4.[:host 和 ::ng-deep 使用](https://heptaluan.github.io/2018/01/16/Angular/06/)

#### 2.提示语 `Placeholder`

###### 1.让 `placeholder ` 不自动上浮
* 1.在父组件 `mat-form-field` 上定义 `[floatLabel]="'never'"` 即可, 如 

```
        <mat-form-field floatLabel]="'never'">
          <input matInput placeholder="Input the Code">
        </mat-form-field>
```

###### 2.[自定义 `placeholder` 的样式](https://stackoverflow.com/a/49169033/13481610)
* 1.不在 input 中设置 placeholder, 在外面定义一个 Placeholder element.

```
/// html :

<mat-form-field>
    <input matInput type="text">
    <mat-placeholder class="placeholder">Search</mat-placeholder>
</mat-form-field>

/// css:

.mat-focused .placeholder {
    color: #00D318;
}
```


#### 3.修改 Input `matInput ` 的显示.

* 1.在 `Component.ts` 中导入  `ViewEncapsulation`, 具体的原理请看 [View encapsulation](https://angular.io/guide/component-styles#view-encapsulation) (这个会影响全局的 CSS 设置，请小心)如:

```
import { Component } from '@angular/core';
import { ViewEncapsulation } from '@angular/core';  // Add


@Component({
  selector: 'input-overview-example',
  styleUrls: ['input-overview-example.css'],
  templateUrl: 'input-overview-example.html',
  encapsulation: ViewEncapsulation.None   // Add
})
export class InputOverviewExample { }

```

* 2.在 Chrome `开发者工具 Developer tools` -> `Elements` 中查看对应的 `Element`, 并在 css 文件中修改该 `Element` 的样式,即可看到效果(如我这里修改了 class 为 `mat-form-field-underline` 的 `background-color: red`).

![](http://pic.pgyjz.cn/blog/Angular/Xnip2020-05-14_11-18-26.png)

* 3.自定义 颜色， padding 等等效果, [在线效果预览](https://stackblitz.com/edit/tian-angular-matinput-style-change) .如下所示(注意 `::ng-deep` 也会改变全局其它的 `angular material` 的效果, 所以要加上 `:host`):

![](http://pic.pgyjz.cn/blog/Angular/Xnip2020-05-14_21-53-08.png)

component

```
import { Component } from '@angular/core';
import { ViewEncapsulation } from '@angular/core';

/**
 * @title Basic Inputs
 */
@Component({
  selector: 'input-overview-example',
  styleUrls: ['input-overview-example.css'],
  templateUrl: 'input-overview-example.html',
  // encapsulation: ViewEncapsulation.None,
  encapsulation: ViewEncapsulation.Emulated
})
export class InputOverviewExample { }


```

html 

```
		<mat-form-field [floatLabel]="'never'" >
          <input matInput  type="text">
          <mat-placeholder class="placeholder">Search</mat-placeholder>
        </mat-form-field>
```

css 

```
:host ::ng-deep{

  /* Hide the underline transparent when not focus */
  .mat-form-field .mat-form-field-underline{
    background-color: transparent;
    height: 0
  }
  /* Hide the underline transparent when focus */
  .mat-form-field.mat-focused .mat-form-field-ripple {
    background-color: transparent;
    height: 0
  }

  /* Set background and border style */
  .mat-form-field-flex{
    border-radius: 5px;
    border: solid 1px #A9A9A9;
    background: #F8F8F8;
  }

  // Vertical center the input element
  .mat-form-field-wrapper{
    padding-bottom: 0;
  }
  /* Set input label */
  .mat-form-field-infix {
    border-top: 0;
    padding-left: 10px !important;
  }

  /* Set placeholder style  */
  .placeholder {
    color: #666;
    padding-left: 10px;
  }
  .mat-focused .placeholder {
    color: #666;
  }
  /* vertical placeholder's center  */
  .mat-form-field-label  {
    top: 50%;
  }

}
```




