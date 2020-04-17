---
title: Excel 导入 Json 文件数据
tags: [Office]
categories: [Office]
date: 2020-02-23 17:46:19
---


> 用 Excel 导入 Json 文件的数据， 并把 Key 和 value 的数据分成不同的列去显示.

<!-- more -->

## 1.从 JSON 文件中导入数据到 Excel(Mac excel 2016)
## 2.从 Excel 中导出数据到 JSON 文件.

***
***
***

## 1.从 JSON 文件中导入数据到 Excel(Mac excel 2016)
#### 1.目标:
* 1.源 json 文件

```
{
  "product.rlm1": "RLM 1",
  "product.zeroturn": "Zeroturn",
  "product.battery": "Batteries",
  "product.rlm2": "RLM 2",

  "search": "Search",
  "set": "Set",
  "update-now": "Update now",
  "enable": "Enable",
  "disable": "Disable",
  "length-unit-m": "m"
}
```

* 2.导入 json 数据后,目标 Excel 文件的样子(Key 跟 value 分开不同的列)

![](http://pic.pgyjz.cn/blog/Office/Snip20200224_4.png)

#### 2.步骤:

* 1.在 Excel 中选择 Data -> Get External Data -> Import Text File

![](http://pic.pgyjz.cn/blog/Office/Snip20200224_2.png)
![](http://pic.pgyjz.cn/blog/Office/Snip20200224_5.png)

* 2.选择 Delimited

![](http://pic.pgyjz.cn/blog/Office/Snip20200224_6.png)

* 3.根据自己的需求去选择不同的配置，我这里选择了 Common,Space, 自添加了 colon :.

![](http://pic.pgyjz.cn/blog/Office/screenshot_2020-02-24_at_14.52.54.png)

* 4.选择 Do not import column(Skip)

![](http://pic.pgyjz.cn/blog/Office/screenshot_2020-02-24_at_14.53.09.png)

* 5.选择 new sheet, 及可以在新的 Tab 中看到 json 的 key -> value 已经成功被导入.



#### 3.注意：
* 1.JSON 文件的 value 值中不要有 双引号(""), 否则 Excel 中解析出来会错误.
![](http://pic.pgyjz.cn/blog/Office/Snip20200224_22.png)
![](http://pic.pgyjz.cn/blog/Office/Snip20200224_21.png)

* 2.如果有双引号，请把它变为单引号 (''), 这样输出的 Excel 文件就会正常了.

![](http://pic.pgyjz.cn/blog/Office/Snip20200224_23.png)
![](http://pic.pgyjz.cn/blog/Office/Snip20200224_24.png)

***

## 2.从 Excel 中导出数据到 JSON 文件.

* 1.参考: [ CSV -> JSON ](http://www.convertcsv.com/csv-to-json.htm)

#### 1.把 xlsx 格式转换为 CSV 格式
* 1.File -> `Save as ...` -> 选择 CSV 格式，然后保存.

![](http://pic.pgyjz.cn/blog/Office/Snip20200224_18.png)
![](http://pic.pgyjz.cn/blog/Office/Snip20200224_19.png)

* 2.进入网站 [http://www.convertcsv.com/csv-to-json.htm](http://www.convertcsv.com/csv-to-json.htm) , 在 Step1 处选择 刚刚生成的 CSV 文件.

* 3.Step2:选择 `Skip # of lines:0`, Tab 为选中的状态.

![](http://pic.pgyjz.cn/blog/Office/Snip20200224_29.png)

* 4.Step3 的选择如图所示, Step4 输入自己的 JSON 模板，然后点击 `Convert CSV to Json via template`

![](http://pic.pgyjz.cn/blog/Office/Snip20200224_30.png)
![](http://pic.pgyjz.cn/blog/Office/Snip20200224_31.png)

* 5.Step5,直接点击 `Download result` 即可，无需做其它的操作.

![](http://pic.pgyjz.cn/blog/Office/Snip20200224_32.png)
 
* 6.因为我们生成的 json 文件会包括空字符键值对: `"":""`, 所以我们把它全部替换成空格即可.

![](http://pic.pgyjz.cn/blog/Office/Snip20200224_33.png)

* 7.用 JSON 对比工具，可以看到生成的文件与目标文件现在完全一致了.

![](http://pic.pgyjz.cn/blog/Office/Snip20200224_34.png)

