---
title: Angular Material
tags: [Angular]
categories: [Angular]
date: 2019-11-24 17:46:19
---

## 1.参考
## 2.使用方法


<!-- more -->

## 1.参考
#### 1. Material design
* 1.[Material design](https://material.io/)
* 2.[如果你不熟悉Material Design，请一口吃下这篇干货！](https://www.uisdc.com/material-design-knowledge)
* 3.[Angular Material](https://material.angular.cn/)

#### 2.Flex layout
* 4.[Angular flex-layout](https://github.com/angular/flex-layout/wiki)

#### 3.Theme
* 5.[Theming Angular Material](https://material.angular.io/guide/theming)
* 6.[Theme style color](https://material.io/archive/guidelines/style/color.html#color-color-system)

***

## 2.使用方法


#### 3.Theme

* 1.新建一个 `my-custom-theme.scss` 文件，输入以下内容

```
@import '~@angular/material/theming';
@include mat-core();

$candy-app-primary: mat-palette($mat-indigo);
$candy-app-accent:  mat-palette($mat-pink, A200, A100, A400);

$candy-app-warn:    mat-palette($mat-red);

$candy-app-theme: mat-light-theme($candy-app-primary, $candy-app-accent, $candy-app-warn);
@include angular-material-theme($candy-app-theme);

// 特别样式的组件，在使用时添加以下的 class name 即可引进
.specify-theme{
	@import '~@angular/material/theming';
	@include mat-core();
	
	$candy-app-primary: mat-palette($mat-indigo);
	$candy-app-accent:  mat-palette($mat-pink, A200, A100, A400);
	
	$candy-app-warn:    mat-palette($mat-red);
	
	$candy-app-theme: mat-light-theme($candy-app-primary, $candy-app-accent, $candy-app-warn);
	@include angular-material-theme($candy-app-theme);
}
```

* 2.然后再 `style.scss` 或 `angular.json` 中导入即可.






