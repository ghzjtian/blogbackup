---
title: Angular 中 Google Map 地图开发
tags: [Angular]
categories: [Angular]
date: 2020-04-24 17:46:19
---

在 Angular 中导入地图组件.

<!-- more -->


# Angular 中 Google Map 地图开发

## 1.价格配置信息

#### 1.全球可选地图服务列表

> [List of online map services](https://en.wikipedia.org/wiki/List_of_online_map_services)

![](http://pic.pgyjz.cn/blog/Angular/global-map-services.png)


#### 2.Google-map 价格资料


[* 1.PRICING FOR MAPS, ROUTES, AND PLACES]( https://cloud.google.com/maps-platform/pricing/sheet )

```
每个 google map 用户每月均可获得价值 $200 的免费额度, 因为 A3S 系统使用的是 `Dynamic Maps`, 所以一个月有 28,000 次(即每天大概 933次)的免费使用额度，已足够使用.

```

![](http://pic.pgyjz.cn/blog/Angular/API-Prices.png)

#### 2.关于 API-Key 对网站的限制

> Restrictions help prevent unauthorized use and quota theft.

* 1.为了防止别人恶意的使用 API-KEY ， 需要对 API-KEY  进行限制，具体是在 CENDENTIALS 中配置那个网站的 URL .

![](http://pic.pgyjz.cn/blog/Angular/API-Restrictions.png)

* 2.配置 API-KEY 支持以下 IP 的 map 使用

![](http://pic.pgyjz.cn/blog/Angular/API-Restrictions-supports.png)





#### 3.功能开发

##### 1.相关参考链接
* 1.[Maps JavaScript API](https://developers.google.com/maps/documentation/javascript/tutorial?hl=zh_CN)
* 2.[SebastianM/angular-google-maps](https://github.com/SebastianM/angular-google-maps)
	* 1.定制化很高，所以很难自定义化
* 3.[Angular official google-map component](https://github.com/angular/components/tree/master/src/google-maps)
	* 1.只支持 Angular CLI 9.0 及以上.

* 4.[Setup Google Map in Angular app (The pro way) pt1](https://dev.to/devpato/setup-google-map-in-angular-app-the-pro-way-3m9p)
	* 1.手动自己导入 google-map ,亲手证实可行.
	* 2.下面的记录主要针对这个教程.
	* 2.[How to load google maps api asynchronously in Angular2](https://stackoverflow.com/questions/34931771/how-to-load-google-maps-api-asynchronously-in-angular2/40462864#40462864)
		* 设置 google-map 加载完成后再显示页面.

##### 2.简单效果

* 1.添加一个 `Control` 的按钮.(有 icon, 并添加点击事件)
[Control Positioning](https://developers.google.com/maps/documentation/javascript/examples/control-positioning?hl=zh_CN)
[Custom Controls](https://developers.google.com/maps/documentation/javascript/examples/control-custom?hl=zh_CN) 的功能.

```

addSubComponentToElement(element: HTMLElement, customElementInfo: CustomElementBean) {
    const controlUI = document.createElement('div');
    this.setDefaultControlUiStyle(controlUI.style, customElementInfo);
    controlUI.title = customElementInfo.promptMessage;

    const controlImg = document.createElement('img');
    controlImg.src = customElementInfo.imageSrc;
    this.setDefaultControlImgStyle(controlImg.style);
    controlUI.appendChild(controlImg);

    element.appendChild(controlUI);
  }

  setDefaultControlUiStyle(controlUiStyle: CSSStyleDeclaration, customElementInfo: CustomElementBean) {
    if (customElementInfo.backgroundColor !== null ) {
      controlUiStyle.backgroundColor = customElementInfo.backgroundColor;
      controlUiStyle.borderRadius = '3px';
      controlUiStyle.boxShadow = '0 2px 6px rgba(0,0,0,0.3)';
    }

    controlUiStyle.cursor = 'pointer';
    controlUiStyle.marginRight = customElementInfo.marginRight;
    controlUiStyle.marginTop = '12px';
    controlUiStyle.textAlign = 'center';
    controlUiStyle.minHeight = '40px';
    controlUiStyle.minWidth = '40px';
    controlUiStyle.display = 'flex';
  }
  setDefaultControlImgStyle(controlUiStyle: CSSStyleDeclaration) {
    controlUiStyle.height = '25px';
    controlUiStyle.width = '25px';
    controlUiStyle.margin = 'auto';
  }

```

* 3.添加 Marker 
[Markers](https://developers.google.com/maps/documentation/javascript/markers?hl=zh_CN)
[Customizing a Google Map: Custom Markers](https://developers.google.com/maps/documentation/javascript/custom-markers?hl=zh_CN)

```
 const DEFAULT_SCALED_SIZE = 20;
    const DEFAULT_ANCHOR_POINT = 10;
    // Mower Location Marker and Circle
    // [Set the icon align center](https://stackoverflow.com/a/12093539/5237440)
    const markerIcon = {
      url: iconUrl,
      scaledSize: new google.maps.Size(DEFAULT_SCALED_SIZE, DEFAULT_SCALED_SIZE),
      anchor: new google.maps.Point(DEFAULT_ANCHOR_POINT, DEFAULT_ANCHOR_POINT),
    };
    const marker = new google.maps.Marker({
      position: markerPosition,
      map: markerMap,
      title: markerTitle,
      icon: markerIcon,
      draggable: canDrag,
      zIndex: curzIndex
    });
    return marker;
```


* 4.添加 `Circle`
[Circles Shape](https://developers.google.com/maps/documentation/javascript/shapes?hl=zh_CN#circles)

```

    const mapCircle = new google.maps.Circle({
      strokeColor: '#fff',
      strokeOpacity: 0,
      strokeWeight: 0,
      fillColor: circleFillColor,
      fillOpacity: 0.42,
      map: curMap,
      center: centerPosition,
      radius: circleRadius
    });
    return mapCircle;
```