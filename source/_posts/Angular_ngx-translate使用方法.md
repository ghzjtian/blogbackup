---
title: Angular 多语言框架 ngx-translate 的使用
tags: [Angular]
categories: [Angular]
date: 2020-02-03 17:46:19
---

> 记录 Angular ngx-translate 多语言框架的使用方法

<!-- more -->




## 1.[参考](#refenences)
## 2.[小工具](#translate_tools)
## 3.[翻译](#translate)
## 4.[错误处理](#error_process)
## 5.[多语言的页面实现](#multi_lang_imprements)
## 6.[复数形式的处理](#one_more)


***
***
***

## 1.参考<a name="refenences"/>
* 1.[ngx-translate/core](https://github.com/ngx-translate/core)
* 2.[How to translate your Angular 7 app with ngx-translate](https://www.codeandweb.com/babeledit/tutorials/how-to-translate-your-angular7-app-with-ngx-translate)
* 3.[Internationalize Your Angular App with ngx-translate](https://alligator.io/angular/ngx-translate/)
* 4.[safely use translate.instant() #517](https://github.com/ngx-translate/core/issues/517#issuecomment-299637956)
* 5.[safely use translate.instant()](https://github.com/ngx-translate/core/issues/517)

## 2.小工具<a name="translate_tools"/>
* 1.[BabelEdit(要收费)](https://www.codeandweb.com/babeledit) -> 'BabelEdit is the translation editor for your ngx-translate project.'
* 2.[poedit(免费)(不能编辑 json 文件)](https://poedit.net) -> Powerful and intuitive translation editor
* 3.[在线多语言 json 编辑工具](https://translation-manager-86c3d.firebaseapp.com/)
* 4.[ngx-translate-extract](https://github.com/biesbjerg/ngx-translate-extract) -> 可以把在 `Html` 和 `Typescript` 文件上用到的 翻译 自动转换到 `xx.json` 文件中.
* 5.[localize-router](https://github.com/Greentube/localize-router) -> 语言改变时 URL 也会跟着变化.

## 3.翻译<a name="translate"/>

* 1.翻译文件为:

```
// en.json
{

  "welcomeMessage": "Thanks for joining, {{ firstName }}! It's great to have you!",
  "loginPrompt": "Please login",
  "login": {
    "username": "Enter your user name",
    "password": "Password here"
  },
  "buttonTx": "click this button",
  "dialog.logout-prompt": "{{ device }} Will logout in {{ time }} seconds later.",
}

```

* 1.在 html 中的翻译

```
<button (click)="buttonClickAction()" >{{buttonText}}</button>

<!-- 元素 文字的翻译 -->
<p>{{ 'welcomeMessage' | translate:user }}</p>
<!-- 元素 的属性 -->
<input title="{{ 'login.password' | translate }}" type="password" placeholder="{{ 'login.password' | translate }}">

<hr>
<br>
<!-- '元素 文字的翻译' 的另外两种做法 -->
<label translate='login.username'></label>
<p translate [translateParams]="{ firstName: user.firstName }">welcomeMessage</p>

<!-- 带参数 -->
<div class="label1-size">{{ 'dialog.logout-prompt' | translate:{device: dialogConfig.message, time: timeLeft} }}</div>
```



* 2.在代码中的翻译

```
import {Component, OnInit} from '@angular/core';
import {TranslateService} from '@ngx-translate/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit{

  user: { firstName: string, lastName: string };
  welcome: string;
  loginPrompt: string;
  usernameLabel: string;
  passwordLabel: string;

  buttonText = "点击666";

  constructor(private translate: TranslateService) {
    console.log('constructor');

    translate.addLangs(['en', 'klingon'])
    translate.setDefaultLang('en');
    translate.use('en');
  }

  ngOnInit() {
    this.user = {firstName: "Tian1", lastName: "Zeng1"};

    // synchronous. Also interpolate the 'firstName' parameter with a value
    // 这里只会分别得到 'welcomeMessage','loginPrompt','buttonTx', 不会得到真正翻译的结果.
    this.welcome = this.translate.instant('welcomeMessage', {firstName: this.user.firstName});
    this.loginPrompt = this.translate.instant('loginPrompt');
    this.buttonText = this.translate.instant('buttonTx');

    // asynchronous - gets translations then completes.
    // 这里可以顺利得到结果
    this.translate.get(['login.username', 'login.password', 'buttonTx'])
      .subscribe(translations => {
        this.buttonText = translations['buttonTx'];
        this.usernameLabel = translations['login.username'];
        this.passwordLabel = translations['login.password'];
      });
  }

  buttonClickAction() {
    this.user = {firstName: "Tian2", lastName: "Zeng2"};

    // 这里可以得到想要的翻译结果
    this.welcome = this.translate.instant('welcomeMessage', {firstName: this.user.firstName});
    this.loginPrompt = this.translate.instant('loginPrompt');
  }
}

```

效果显示:

![](http://pic.pgyjz.cn/blog/Angular/multiLanguages/Snip20200215_2.png)

## 4.错误处理<a name="error_process"/>
#### 1.翻译文件中没有这个翻译 key 的情况.
* 1.Html: 翻译的文本只会显示这个 key.
* 2.Typescript: 翻译的文本为: undefine
#### 2.翻译文件没有的情况.
* 会报 	`GET http://localhost:4200/assets/i18n/cn.json 404 (Not Found)` 错误.

#### 3.`this.translate.instant ` 一直返回 key 的问题.
* 1.原因: 这是因为 翻译文件还没有加载完毕，所以用 同步的方法 `translate.instant` 不能获取到 value.

* 2.解决方法
	* 1: 用 异步方法 get 去实现

```
translate.get("lessons_title").subscribe((result: string) => {
        lesson.title = result;
    });
```

* 2.在项目加载时就加载全部的翻译文本,具体做法是在 AppModule 中添加以下代码:

```
@NgModule({

  providers: [  {
    provide: APP_INITIALIZER,
    useFactory: appInitializerFactory,
    deps: [TranslateService, Injector],
    multi: true
  }],
  bootstrap: [AppComponent]
})


export function appInitializerFactory(translate: TranslateService, injector: Injector) {
  return () => new Promise<any>((resolve: any) => {
    const locationInitialized = injector.get(LOCATION_INITIALIZED, Promise.resolve(null));
    locationInitialized.then(() => {

      translate.addLangs(['en', 'de', 'zh'])
      translate.setDefaultLang('en');
      const browerLang = translate.getBrowserLang();
      const langToSet = browerLang.match(/en|de|zh/)? browerLang: 'en';
      translate.use(langToSet).subscribe(() => {
        console.info(`Successfully initialized '${langToSet}' language.'`);
      }, err => {
        console.error(`Problem with '${langToSet}' language initialization.'`);
      }, () => {
        resolve(null);
      });
    });
  });
}
```
	
#### 4.首次加载时项目时,加载不出特定语言的文本
* 1.情况描述: 如在 AppModule 中设置了默认语言 en: `setDefaultLang('en')`, 然后使用语言 fr: `use('fr')`, 然后在 Component 中用 `get('translate-key').subscribe(()=>{}) ` 或 `instant('translate-key')` 都只能得到 en 的文本, 但是 html 的翻译文本却都已经正确地翻译为 fr 了.(通过查 log 发现是 view 已经更新完毕了，但是 `fr.json` 文本还没下载完.)
* 2.解决方法: 在 constructor 中监听 lang 的变化，然后改变文本. 

```
constructor(private translateService: TranslateService) {
	translateService.onLangChange.subscribe(() => {
      this.viewText = translateService.instanct('translate-key');
    });
}
```
	
	
## 5.多语言切换的实现<a name="multi_lang_imprements"/>
#### 1.下拉选择框切换语言

* 1.代码如下所示

```
// AppComponent

  currentLang : string;
  constructor(private translate: TranslateService) {
    translate.addLangs(['en', 'de'])
    translate.setDefaultLang('en');
    
    this.currentLang = "en";
  }
  ngOnInit() {
   this.translate.onLangChange.subscribe((event: LangChangeEvent) => {
        console.log('onLangChange event:'+ event.lang);
        this.currentLang = event.lang;
    });
    this.translate.onTranslationChange.subscribe((event: TranslationChangeEvent) => {
      console.log('onTranslationChange event:'+ event.translations);
    });
    this.translate.onDefaultLangChange.subscribe((event: DefaultLangChangeEvent) => {
      console.log('onDefaultLangChange event:'+ event.lang);
    });
  }
  languageChangeEvent(lang: string) {
    console.log("lang select:"+lang);
    this.translate.use(lang);
  }
```

```
// Html file

<h3>当前的语言为: {{currentLang}}</h3>
<select (change)="languageChangeEvent($event.target.value)" >
  <option *ngFor="let lang of this.translate.getLangs()" [value]="lang">{{lang}}</option>
</select>

```

效果显示:
![](http://pic.pgyjz.cn/blog/Angular/multiLanguages/lang-change.gif)

* 2.但是如果程序有很多个 Module, 在子 Module 下设置 `translate.use(lang)` 的话其它的 module 会得不到响应，有个暴力的方法就是重新刷新整个页面就可以解决这个问题:

```
      this.currentSelectLang = currentLang;
      this.translate.use(currentLang);

      // window.location.pathname:  '/en/dashboard/mower' , '/'

      //  window.location.href:  'http://localhost:4200/en/dashboard/mower' , 'http://localhost:4200/'
      var currentPathName = window.location.pathname;
      // 1.Check pathname contain en\zh\fr
      const urlLang = this.getLangFromPath(currentPathName);
      const urlHasLang = !urlLang.match(/en|zh|fr/) ;

      if( !urlHasLang ) {
        // 2.If contain, then replace the old language to new language

        currentPathName = currentPathName.replace(urlLang,currentLang);
      } else {
        // 3.Else , add the new language to the path name
        currentPathName = `/${currentLang}${currentPathName}`;
      }

      this.route.navigate([currentPathName]) .then(() => {
        window.location.reload();
      });
```

#### 2.URL 切换语言
##### 1.`this.translate.currentLang` 不生效 !!!
	* 需要设置 `translate.use('en');` 后， `this.translate.currentLang ` 才生效.
##### 2.所实现的效果有:
	* 1.访问 http://localhost:4200/de/login 会显示德语.
	* 2.下拉选择了语言后， URL 会变化，如从 login 页面的 en 选择了 de 后,  URL 从 http://localhost:4200/login 变为 http://localhost:4200/de/login .

##### 3.实现步骤:
* 1.在 `AppModule` 添加一个 `providers`,如下面代码所示(逻辑就是为一开始加载程序时，就拿到 URL 上的所显示的语言,然后跟据这个语言去设置程序的语言):

```
	providers: [
        {
            provide: APP_INITIALIZER,
            useFactory: appInitializerFactory,
            deps: [TranslateService, Injector],
            multi: true
        }
    ],
    
    
    export function appInitializerFactory(translate: TranslateService, injector: Injector) {
    return () => new Promise<any>((resolve: any) => {
        const locationInitialized = injector.get(LOCATION_INITIALIZED, Promise.resolve(null));
        locationInitialized.then(() => {


            translate.addLangs(['en','zh']);
            // Set defaultLang to en
            translate.setDefaultLang('en');

          const pathName = window.location.pathname;
          const urlLang = getLangFromPath(pathName);

          // Get default browser language
            // const browserLang = translate.getBrowserLang();

            const langToSet = urlLang.match(/en|zh/)? urlLang: 'en';
            translate.use(langToSet).subscribe(() => {
                console.info(`Successfully initialized '${langToSet}' language.'`);
            }, err => {
                console.error(`Problem with '${langToSet}' language initialization.'`);
            }, () => {
                resolve(null);
            });
        });
    });
}

/**
 *
 * @param pathName, '/' or '/zh' or '/zh/tools/mower/rlm1-repair-tool'
 */
function getLangFromPath(pathName: string): string {
  const slashIndex = pathName.indexOf("/",1);
  if (slashIndex == -1) {
    return pathName.substring(1, pathName.length)
  }
  return pathName.substring(1, slashIndex);
}
    
```

#### 3.跟随浏览器的语言去改变 Web APP 的语言

```
  constructor(private translate: TranslateService, private router: Router) {
 
	    translate.addLangs(['en', 'de', 'zh'])
	    translate.setDefaultLang('en');
	    const browerCurLang = translate.getBrowserCultureLang();
	    const browerLang = translate.getBrowserLang();
	    translate.use(browerLang.match(/en|de|zh/)? browerLang: 'en');
    }
```

## 6.复数形式的处理<a name="one_more"/>