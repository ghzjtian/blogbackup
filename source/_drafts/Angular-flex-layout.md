---
title: Angular Flex-layout 使用
tags: [Angular]
categories: [Angular]
date: 2020-04-09 17:46:19
---


> 记录 Angular Flex-layout 的使用方法

<!-- more -->

## 1.参考
* 1.[Github flex-layout](https://github.com/angular/flex-layout)
* 2.[API-Documentation](https://github.com/angular/flex-layout/wiki/API-Documentation)
* 3.[Layout-Demo](https://tburleson-layouts-demos.firebaseapp.com/#/responsive)
* 4.[Responsive API<breakpoint 的解释>](https://github.com/angular/flex-layout/wiki/Responsive-API)
* 5.[fxFlex](https://github.com/angular/flex-layout/wiki/fxFlex-API)

## 2.动态布局资料
```
breakpoint	mediaQuery
xs	'screen and (max-width: 599px)'
sm	'screen and (min-width: 600px) and (max-width: 959px)'
md	'screen and (min-width: 960px) and (max-width: 1279px)'
lg	'screen and (min-width: 1280px) and (max-width: 1919px)'
xl	'screen and (min-width: 1920px) and (max-width: 5000px)'
lt-sm	'screen and (max-width: 599px)'
lt-md	'screen and (max-width: 959px)'
lt-lg	'screen and (max-width: 1279px)'
lt-xl	'screen and (max-width: 1919px)'
gt-xs	'screen and (min-width: 600px)'
gt-sm	'screen and (min-width: 960px)'
gt-md	'screen and (min-width: 1280px)'
gt-lg	'screen and (min-width: 1920px)'
```

## 3.使用方法

```
		<div class="content" 
           fxLayout="row"  //布局为 行
           fxLayout.xs="column"  // 布局为 列 (当屏幕尺寸为 'screen and (max-width: 599px)' 时)
           fxFlexFill >
           
          <div fxFlex="15" class="sec1" fxFlex.xs="55">
              first-section
          </div>
          <div fxFlex="30" class="sec2" >
              second-section
          </div>
          <div fxFlex="55" class="sec3" fxFlex.xs="15">
              third-section
          </div>
          
      </div>

```

## 4.注意事项
* 1.`fxFlexFill` 会与 `fxLayoutAlign ` 互斥