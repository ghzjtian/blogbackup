---
title: Angular 多语言
tags: [Angular]
categories: [Angular]
date: 2019-08-13 17:46:19
---

> Angular 多语言的功能开发

<!-- more -->



## [0.参考](#references)
## [1.官方的翻译方法](#translate)
## [2.翻译文档的工具](#tools)
## [3.翻译文件的生成及部署](#deploy)
#### 方法一:
### 方法二:
## [4.例子](#example)

***
***
***
 
## 0.参考<a name="references"/>
* 1.[Angular 国际化](https://v6.angular.live/guide/i18n#create-a-translation-source-file)
* 2.[Angular Internationalization - Angular i18n Multi Language](https://angular-templates.io/tutorials/about/angular-internationalization-i18n-multi-language-app)
* 3.[Deploying an i18n Angular app with angular-cli](https://medium.com/@feloy/deploying-an-i18n-angular-app-with-angular-cli-fc788f17e358#.1xq4iy6fp)
	* 1.[使用Angular-CLI发布一个i18n（国际化）应用(译)](https://www.jianshu.com/p/ae412f2105c3)
* 4.[ngx-translate(第三方的翻译包)](http://www.ngx-translate.com)
	* 1.[用官方或这个包的讨论](https://stackoverflow.com/questions/47312962/angular-5-internationalization)
* 5.[GitHub 例子程序](https://github.com/ghzjtian/angular-cli-i18n-sample)
* 6.[GitHub: ngx-translate](https://github.com/ngx-translate/core)
* 7.[Deploying an i18n Angular app with angular-cli](https://dev.to/angular/deploying-an-i18n-angular-app-with-angular-cli-2fb9)

## 1.官方的翻译方法<a name="translate"/>
* 1.测试中通过调用 `ng serve --configuration=fr` 去测试指定语言是否正常显示.
* 2.翻译文本的目标地址配置，在项目配置文件 `package.json` 中去设置:

```
{
  [...]
  "scripts": {
    [...]
    "extract": "ng xi18n --output-path=locale"
  }
  [...]
}
```

* 3.

## 2.翻译文档的工具<a name="tools"/>
* 1.[Online XLIFF Editor](http://xliff.brightec.co.uk/)



## 3.翻译文件的生成及部署<a name="deploy"/>

> 目标为当改变 URL 时，会改变相关文字的翻译,如当地址为 `www.example.com` 显示当前地区的语言，当地址为 `www.example.com/zh` 时就显示中文.

### 方法一:
* 1.要翻译的文本很简单，如下:

```
<h1 i18n="@@greeting">Hello world2!</h1>
```

* 2.相关的翻译文件在 `src/i18n/` 文件夹下:

![](http://pic.pgyjz.cn/blog/Angular/Angular-translate/Snip20190813_2.png)


* 3.在 `angular.json` 中增加以下配置项

```
"build": {
  ...
  "configurations": {
    ...
    "zh":{
              "aot":true,
              "outputPath": "dist/angular-cli-i18n-sample-zh",
              "i18nFile": "src/i18n/messages.zh.xlf",
              "i18nFormat": "xlf",
              "i18nLocale": "zh",
              "i18nMissingTranslation": "error",
              "baseHref": "/zh/"
            }
  }
},
"serve": {
  ...
  "configurations": {
    ...
    "zh":{
              "browserTarget": "angular-cli-i18n-sample:build:zh"
            }
  }
}
```

* 4.用 `ng serve` 和 `ng serve --configuration=zh` 即可看出不同语言显示的区别

* 5.编译
	* 1.用 `ng build` 及 `ng build --configuration=zh` 编译出两套语言的程序,然后在 nginx 或其它静态服务器上,按照如下的去设置：

	![](http://pic.pgyjz.cn/blog/Angular/Angular-translate/Snip20190813_3.png)
	
	* 2.即可在浏览器中看到改变 URL 后显示的效果:

	![](http://pic.pgyjz.cn/blog/Angular/Angular-translate/Snip20190813_6.png)

### 方法二:
* 1.也可以写一个脚本完成上述的操作，如在 `package.json` 写上:

```
{
  ...
  "scripts": {
  		...
    "build-i18n": "for lang in en zh; do ng build --output-path dist/$lang --aot  --base-href /$lang/ --i18n-file src/i18n/messages.$lang.xlf --i18n-format xlf --i18n-locale $lang; done"
  },
```

然后运行 `npm run build-i18n` ，即可以看到在 dist 目录下生成的 `zh` 和 `en` 两个目录.

* 2.放到 nginx 服务器上，可以看到效果如下

![](http://pic.pgyjz.cn/blog/Angular/Angular-translate/Snip20190813_7.png)
![](http://pic.pgyjz.cn/blog/Angular/Angular-translate/Untitled.gif)

## 4.例子<a name="example"/>
* 1.在网页中动态选择语言的切换的例子

```

// app.component.ts
import { Component, LOCALE_ID, Inject } from '@angular/core';
@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  myLocaleId: string;
  languages = [
    { code: 'en', label: 'English'},
    { code: 'es', label: 'Español'},
    { code: 'fr', label: 'Français'}
  ];
  constructor(@Inject(LOCALE_ID) protected localeId: string) {
  	this.myLocaleId = this.localeId ;
  }
}

<!-- app.component.html -->
<h1 i18n>Hello World!</h1>
<ng-template ngFor let-lang [ngForOf]="languages">
  <span *ngIf="lang.code !== myLocaleId">
    <a href="/{{lang.code}}/">{{lang.label}}</a> </span>
    <span *ngIf="lang.code === myLocaleId">{{lang.label}} </span>
</ng-template>
```
 