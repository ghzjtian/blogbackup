---
title: Angular 自定义主题
tags: [Angular]
categories: [Angular]
date: 2020-06-01 17:46:19
---


> Angular 的主题能影响到 Angular Material 所有控件的颜色显示状况.



<!-- more -->

## [1.参考](#references)
## [2.自定义一个简单的主题](#define-new-theme)
## [3.新增特殊情况下的主题](#add-special-theme)
## [4.黑白主题的切换](#light-dark-theme-switch)
## [5.自定义多一种颜色](#define-new-color)
## [6.多个主题的切换](#multi-theme-switch)
## [7.自定义文字排版](#custom-typography)

***
***
***

## 1.参考<a name="references"/>
* 1.[https://www.positronx.io/create-angular-material-8-custom-theme/](https://www.positronx.io/create-angular-material-8-custom-theme/)
* 2.[设置 Angular Material 应用的主题](https://material.angular.cn/guide/theming)
* 3.[Material COLOR TOOL](https://material.io/resources/color/)


## 2.自定义一个简单的主题<a name="define-new-theme"/>

#### 1.新建一个 `custom-material-theme.scss` 文件，写入自定义的主题颜色

```
@import '~@angular/material/theming';
@include mat-core();

// 自定义一个颜色模板
$mat-custom-green: (
  50: #e8f5e9,
  100: #c8e6c9,
  200: #a5d6a7,
  300: #81c784,
  400: #66bb6a,
  500: #4caf50,
  600: #43a047,
  700: #388e3c,
  800: #2e7d32,
  900: #1b5e20,
  A100: #b9f6ca,
  A200: #69f0ae,
  A400: #00e676,
  A700: #00c853,
  contrast: (
    50: $dark-primary-text,
    100: $dark-primary-text,
    200: $dark-primary-text,
    300: $dark-primary-text,
    400: $dark-primary-text,
    500: $light-primary-text,
    600: $light-primary-text,
    700: $light-primary-text,
    800: $light-primary-text,
    900: $light-primary-text,
    A100: $dark-primary-text,
    A200: $light-primary-text,
    A400: $light-primary-text,
    A700: $light-primary-text,
  )
);

// $mat-indigo 为 Angular 官方提前定义好的颜色模板, 路径在 `node_modules/@angular/material/_theming.scss`
$candy-app-primary: mat-palette($mat-indigo);
$candy-app-accent: mat-palette($mat-pink);
$candy-app-warn:    mat-palette($mat-custom-green, 400, 50, 900);

$candy-app-theme: mat-light-theme($candy-app-primary, $candy-app-accent, $candy-app-warn);

// All materil
@include angular-material-theme($candy-app-theme);

//// Special materil
//// Include the theme styles for only specified components.
// @include mat-core-theme($candy-app-theme);
// @include mat-button-theme($candy-app-theme);
// @include mat-checkbox-theme($candy-app-theme);

```

#### 2.在 `angular.json` 的 `build/styles` 加入 `custom-material-theme.scss` 的路径

```

"styles": [
     "./node_modules/@angular/material/prebuilt-themes/indigo-pink.css", // 导入默认的主题
     "src/styles/custom-material-theme.scss"    // 导入自定义的主题
]

```

#### 3.查看自定义主题的效果

> 如: Button 中的 文字及背景 颜色

* 1.我们定义了 warn 颜色: `$candy-app-warn:    mat-palette($mat-custom-green, 400, 50, 900);`
* 2.`$mat-custom-green` 的定义为: 

```
$dark-primary-text: rgba(black, 0.87);
$light-primary-text: white;

$mat-custom-green: (
  50: #e8f5e9,
  100: #c8e6c9,
  200: #a5d6a7,
  300: #81c784,
  400: #66bb6a,
  500: #4caf50,
  600: #43a047,
  700: #388e3c,
  800: #2e7d32,
  900: #1b5e20,
  A100: #b9f6ca,
  A200: #69f0ae,
  A400: #00e676,
  A700: #00c853,
  contrast: (
    50: $dark-primary-text,
    100: $dark-primary-text,
    200: $dark-primary-text,
    300: $dark-primary-text,
    400: $dark-primary-text,
    500: $light-primary-text,
    600: $light-primary-text,
    700: $light-primary-text,
    800: $light-primary-text,
    900: $light-primary-text,
    A100: $dark-primary-text,
    A200: $light-primary-text,
    A400: $light-primary-text,
    A700: $light-primary-text,
  )
);
```

* 3.则 `material button` 的颜色为 `#66bb6a` 或 `rgba(black, 0.87)` 如下图所示

![](http://pic.pgyjz.cn/blog/Angular/Xnip2020-06-04_10-53-07.png)



## 3.新增特殊情况下的主题<a name="add-special-theme"/>

* 1.在 `custom-material-theme.scss` 下添加以下代码

```
// Add class
.special-theme {
  $candy-alternate-primary: mat-palette($mat-green);
  $candy-alternate-accent: mat-palette($mat-blue);
  $candy-alternate-warn:    mat-palette($mat-yellow);

  $candy-alternate-theme: mat-light-theme($candy-alternate-primary, $candy-alternate-accent, $candy-alternate-warn);

  @include angular-material-theme($candy-alternate-theme);
}
```

* 2.在 html 父类中添加 `alternate-theme1 ` class.
* 3.效果如下所示.


![](http://pic.pgyjz.cn/blog/Angular/Xnip2020-06-04_11-29-02.png)



## 4.黑白主题的切换.<a name="light-dark-theme-switch"/>
* 1.[Automatic Dark Mode Detection in Angular Material](https://medium.com/@PhilippKief/automatic-dark-mode-detection-in-angular-material-8342917885a0)
* 2.[How to implement dark / light mode in Angular Material with prefers-color-scheme](https://medium.com/@svenbudak/how-to-implement-dark-light-mode-in-angular-mateiral-with-prefers-color-scheme-ce3e980e2ea5)





## 5.自定义多一种颜色<a name="define-new-color"/>

#### 1.参考

* 1.[angular material design - add custom button color](https://stackoverflow.com/a/48015134/5237440)
* 2.[Stackblitz: tian-angular-button-style](https://stackblitz.com/edit/tian-angular-button-style?embed=1&file=app/button-types-example.css)

> (除了 `primary/accent/warn` 外.)

#### 2.步骤
* 1.在 `custom-material-theme.scss` 中添加 `success` color.

```
// 定义
@mixin success-palette {
	.mat-button,
	.mat-stroked-button,
	.mat-icon-button{
	  &.mat-success {
	    color: #155724;
	  }
	}
	.mat-raised-button,
	.mat-flat-button,
	.mat-fab,
	.mat-mini-fab{
	  &.mat-success{
	    color: #f0fff3;
	    background-color: #155724;
	  }
	}
}

    // When is Disable
  $default-disable-bg-color: rgba(0,0,0,.12);

  .mat-button,
  .mat-stroked-button,
  .mat-icon-button{
    &.mat-success[disabled]{
      color: $default-disable-bg-color;
    }
  }
  .mat-raised-button,
  .mat-flat-button,
  .mat-fab,
  .mat-mini-fab{
    &.mat-success[disabled]{
        color: rgba(0,0,0,.26);
        background-color: $default-disable-bg-color;
        background-image: linear-gradient($default-disable-bg-color, $default-disable-bg-color);
    }
  }

```

```

// 导入
@include success-palette;
  
  // 如果是有 特殊情况下的主题, 如上面的步骤三，在特殊主题的类里面也添加, 如
  
	.special-theme {
	  $candy-alternate-primary: mat-palette($mat-green);
	  $candy-alternate-accent: mat-palette($mat-blue);
	  $candy-alternate-warn:    mat-palette($mat-yellow);
	
	  $candy-alternate-theme: mat-light-theme($candy-alternate-primary, $candy-alternate-accent, $candy-alternate-warn);
	
	  @include angular-material-theme($candy-alternate-theme);
	  
	  @include success-palette;
	}
  
  
```

* 2.在 html 中的使用

```
<button mat-flat-button color="success">Success</button>
```

* 3.效果如下


![](http://pic.pgyjz.cn/blog/Angular/Xnip2020-06-04_14-02-10.png)




## 6.多个主题的切换.<a name="multi-theme-switch"/>
* 1.[Dynamic themes in Angular Material](https://medium.com/grensesnittet/dynamic-themes-in-angular-material-b6dc0c88dfd7)
* 2.[Theme Change Demo](https://angular-material-switch-themes-by-toggle.stackblitz.io)


## 7.自定义文字排版<a name="custom-typography"/>
* 1.[使用 Angular Material 的主题体系为自定义组件应用主题](https://material.angular.cn/guide/theming-your-components)
* 2.[Angular Material 的排版](https://material.angular.cn/guide/typography)

#### 1.增加一个文件 `custom-typography.scss`，里面定义 Material 排版的情况, 如:

```
// custom-typography.scss

@import '~@angular/material/theming';

// Set the button's font style
$custom-button-typography-level: mat-typography-level(
  $font-size: 14px,
  $font-weight: 400
);

// Define a custom typography config that overrides the font-family
$custom-typography-config: mat-typography-config(
  $font-family: '"Helvetica Neue", Helvetica, cursive',
    $button: $custom-button-typography-level
);



```

#### 2.在 `custom-material-theme.scss` 中引入 `custom-typography.scss`

```
@import "./materials/custom-typography";

//// Override typography CSS classes (e.g., mat-h1, mat-display-1, mat-typography, etc.).
//@include mat-base-typography($custom-typography);

//// Override typography for a specific Angular Material components.
//@include mat-checkbox-typography($custom-typography);

// Override typography for all Angular Material, including mat-base-typography and all components.
@include angular-material-typography($custom-typography-config);
```

