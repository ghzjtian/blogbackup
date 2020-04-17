---
title: Angular 图表的使用
tags: [Angular]
categories: [Angular]
date: 2019-08-19 17:46:19
---

> Angular 中对图表使用的记录

<!-- more -->

## [1.JavaScript Chart](#javascript_chart)
## [2.Angular Charts](#angular_chart)
## [3.使用中出现的问题](#issues)

***
***
***

## 1.JavaScript Chart<a name="javascript_chart"/>

#### 1.[Google Charts](https://github.com/FERNman/angular-google-charts)
#### 2.Jvectormap(区域性地图)
##### 1.参考:
* 1.[https://github.com/StephanWagner/svgMap](https://github.com/StephanWagner/svgMap)
* 2.[bjornd/jvectormap](https://github.com/bjornd/jvectormap)
* 3.[教你用 jVectorMap 制作属于自己的旅行足迹](https://github.com/HelloWuJiaYi/jVectorMap-Footprint)

##### 2.在 Angular 中导入 Jvectormap
* 1.在 `index.html` 中导入必要的文件

```
<script type="text/javascript" src="./assets/js/jquery-1.9.1.min.js"></script>
<script type="text/javascript" src="./assets/js/jquery-jvectormap.min.js"></script>
<link href="./assets/js/jquery-jvectormap.min.css" rel="stylesheet" media="screen">
<script src="./assets/js/jquery-jvectormap-world-mill.js"></script>
```

* 2.在需要导入的 html 写入以下代码

```

<div id="world-map" style="width: 100%; height: 100%"  ></div>

```

* 3.然后在 component 中导入 Jvectormap 即可

```
declare var jQuery: any;
export class WorldMapComponent implements OnInit {
	private  readonly MAP_VIEW_ID = '#world-map';

 public initVectorChart(regionsData: any, vectorMapConfig: IVectorMapConfigBean) {

    jQuery(this.MAP_VIEW_ID).vectorMap({
      map: 'world_mill',
      series: {
        regions: [{
          values: regionsData, // Map's data
          scale: ['#ffffff', '#4CA858'], // color
          normalizeFunction: 'polynomial'

        }]
      },
      // show the tip when mouse hover
      onRegionTipShow: function(e, el, code) {
        // [jvectormap-tip](https://stackoverflow.com/questions/10664969/jvectormap-label-is-not-visible-why)
        // CSS: https://stackoverflow.com/questions/38656092/jvector-map-how-to-change-the-size-of-tooltip-which-are-shown-when-mouse-cursor
        if (regionsData[code]) {
          el.html(`${el.html()}: ${regionsData[code] }`).css("fontSize","16px").css("border-radius","1em").css("background","white").css("color","black");
        } else {
          el.html(`${el.html()}`).css("fontSize","16px").css("border-radius","1em").css("background","white").css("color","black");
        }
      },
      // When the region is click, move the camera to the region.
      onRegionClick: function(e,  code,  isSelected,  selectedRegions) {
        jQuery('#world-map').vectorMap('get','mapObject').setFocus({region: code, animate: true});
        vectorMapConfig.regionCode = code;
      },
      // Get the scale innformation when zoom
      onViewportChange: function( e,  scale) {
        vectorMapConfig.zoomScale = scale;
      },

      backgroundColor: '#B1C8EF', // Ocean color
      zoomMin: 1,
      zoomMax: 10,
      focusOn: {region: vectorMapConfig.regionCode, animate: vectorMapConfig.isAnimate}, // zoom to the region when init
      regionStyle: {
        initial: {
          fill: '#F5F9FB',   // sea default color
          "fill-opacity": 1, //  Province should be hidden? 1:display, 2:hidden
          stroke: 'none',
          "stroke-width": 0,
          "stroke-opacity": 1,
        // },
        // hover: {
          // fill: '#ffffff85',
          // "fill-opacity": 0.8
          // "fill-opacity": 0.8,
          cursor: 'pointer'
        },
        selected: {
          // fill: 'yellow'
        },
        selectedHover: {}
      },
      regionLabelStyle: {
        initial: {
          fill: '#B90E32'
        },
        hover: {
          fill: 'black'
        }
      },
      // show the text which is still show on the region.
      labels: {
        regions: {
          render: function(code) {
            return regionsData[code];
          },
          offsets: function(code) {
            return {
              'FR': [60, -50],
              'AT': [3, 0],
              'NO': [-20, 30],
              'SE': [-5, 0],
              'DK': [-3, 0],
              'US': [-280, 30],
            }[code];
          }
        }
      },


    });
  }

}
```
	
#### 3.在 Angular 中导入 javascript 文件
* 1.[How to use external JS files and JavaScript code in Angular 6/7](https://www.truecodex.com/course/angular-6/how-to-use-external-js-files-and-javascript-code-in-angular)

#### 4.[Leaflet Map](https://leafletjs.com/)

***

## 2.Angular Charts<a name="angular_chart"/>
* 1.[Angular 4 Chart library](https://medium.com/@alphacoder/angular-4-chart-library-8571c2ef50e4)
* 2.[https://github.com/swimlane/ngx-charts](https://github.com/swimlane/ngx-charts)

* 3.[ng2-charts](https://valor-software.com/ng2-charts/#LineChart)
	* 1.[Github:ng2-charts](https://github.com/valor-software/ng2-charts)
	* 2.[Chart.js docs](https://www.chartjs.org/docs/latest/general/colors.html)
	* 3.[Line demo](https://stackblitz.com/edit/ng2-charts-line-template-alysmu?file=src/app/app.component.ts)
	
* 4.[Google Chart](https://google-developers.appspot.com/chart/interactive/docs/)
	* 1.[Using Google Charts in Angular 4 project, part 2](http://anthonygiretti.com/2017/10/12/using-google-charts-in-angular-4-project-part-2/)	* 2.[GEO Charts(账号需要信用卡去做绑定)](https://developers.google.com/chart/interactive/docs/gallery/geochart)
	* 3.[GitHub:angular-google-charts](https://github.com/FERNman/angular-google-charts)
	
* 5.[Google Map](https://developers.google.com/maps/documentation)
	* 1.[Integrating Google Maps in Angular 6](https://medium.com/@balramchavan/integrating-google-maps-in-angular-5-ca5f68009f29)
	* 2.[Angular 包:SebastianM/angular-google-maps](https://github.com/SebastianM/angular-google-maps)

## 3.使用中出现的问题<a name="issues"/>
* 1.[出现 `Can't bind to 'datasets' since it isn't a known property of 'base-chart'.` 问题](https://github.com/valor-software/ng2-charts/issues/535)
	* 1.如果是在子 Module 中使用，就需要在子 Module 中导入，如我的子 Module 为 `HomeModule` , 根 Module 为 `AppModule` ,则在 `HomeModule` 中写入 `  imports: [ ChartsModule, ]`

	






