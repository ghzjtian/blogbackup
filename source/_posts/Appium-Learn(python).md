---
title: Appium 自动化测试(python3)
tags: [auto-test]
categories: [auto-test]
date: 2020-07-11 17:46:19
---

> Appium 环境的搭建及简单的例子(Python3)

<!-- more -->


# 1.参考
* 1.[Appium Github](https://github.com/appium/appium)
* 2.[Appium.io 移动应用自动化](http://appium.io/)
* 3.[Appium Python Testing: The Complete Guide](https://experitest.com/appium-testing/the-complete-guide-appium-testing-using-python/)
* 4.[Appium的常用定位方法](https://www.cnblogs.com/tiechui2015/p/10310738.html)
* 5.[Appium Scroll](https://www.youtube.com/watch?v=Ot0N0tw54Hg)
* 6.[Python 修改 pip 源为国内源](https://zhuanlan.zhihu.com/p/109939711)
* 7.[Python test report(HtmlTestRunner)](https://github.com/oldani/HtmlTestRunner)

# 2.步骤
## 1.下载 Pycharm CE, 然后配置 `Project Interpreter` 信息.


# 3.效果
## 1.滚动屏幕

* 1.通过坐标, 如从 `(x=60, y=1000)` 到 `(x=50, y=500)`
```
# // 向下滑动效果
for i in range(5):
    TouchAction(driver).press(x=60, y=1000).move_to(x=50, y=500).release().perform()
    sleepSomeTime(2)
```

* 2.通过特定的控件的位置.如从 `country` 到 `addressLine2 `
```
for i in range(5):
    country = driver.find_element_by_xpath("//android.widget.TextView[contains(@text,'Country')]")
    addressLine2 = driver.find_element_by_xpath("//android.widget.TextView[contains(@text,'Address line 2')]")
    TouchAction(driver).press(country).move_to(addressLine2).release().perform()
```

## 2.保存截图

```
# 保存到 Html
# screenshotBase64 = driver.get_screenshot_as_base64()

# 保存到文件
directory = '%s/reports/' % os.getcwd()
now = datetime.now()
file_name = 'screenshot_{0}.png'.format(now.strftime('%Y-%m-%d_%H:%M:%S'))

driver.save_screenshot(directory + file_name)

```

## 3.查看页面的代码结构

* 1.执行
```
source = driver.page_source
print("source:{0}".format(source))
```

* 2.输出效果

```
source:<?xml version='1.0' encoding='UTF-8' standalone='yes' ?>
<hierarchy index="0" class="hierarchy" rotation="0" width="1080" height="1812">
  <android.widget.FrameLayout index="0" package="com.test.main" class="android.widget.FrameLayout" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,0][1080,1812]" displayed="true">
    <android.widget.LinearLayout index="0" package="com.test.main" class="android.widget.LinearLayout" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,0][1080,1812]" displayed="true">
      <android.widget.FrameLayout index="0" package="com.test.main" class="android.widget.FrameLayout" text="" resource-id="android:id/content" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,72][1080,1812]" displayed="true">
        <android.support.v4.widget.SlidingPaneLayout index="0" package="com.test.main" class="android.support.v4.widget.SlidingPaneLayout" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,72][1080,1812]" displayed="true">
          <android.widget.FrameLayout index="0" package="com.test.main" class="android.widget.FrameLayout" text="" resource-id="com.test.main:id/ccb_base_drawer_container" checkable="false" checked="false" clickable="false" enabled="true" focusable="true" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,72][1080,1812]" displayed="true">
            <android.widget.RelativeLayout index="0" package="com.test.main" class="android.widget.RelativeLayout" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,72][1080,1812]" displayed="true">
              <android.widget.FrameLayout index="0" package="com.test.main" class="android.widget.FrameLayout" text="" resource-id="com.test.main:id/layout_container" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,72][1080,1812]" displayed="true">
                <android.widget.FrameLayout index="0" package="com.test.main" class="android.widget.FrameLayout" text="" resource-id="com.test.main:id/fragment_container" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,72][1080,1812]" displayed="true">
                  <android.widget.RelativeLayout index="0" package="com.test.main" class="android.widget.RelativeLayout" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,72][1080,1812]" displayed="true">
                    <android.widget.ImageView index="0" package="com.test.main" class="android.widget.ImageView" text="" content-desc="关闭" resource-id="com.test.main:id/iv_close" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[65,150][137,222]" displayed="true" />
                    <android.widget.ImageView index="1" package="com.test.main" class="android.widget.ImageView" text="" resource-id="com.test.main:id/iv_user_photo" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[417,290][662,535]" displayed="true" />
                    <android.widget.LinearLayout index="2" package="com.test.main" class="android.widget.LinearLayout" text="" resource-id="com.test.main:id/ll_username_reserveinfo" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,603][1080,723]" displayed="true">
                      <android.widget.LinearLayout index="0" package="com.test.main" class="android.widget.LinearLayout" text="" resource-id="com.test.main:id/ll_showUserInfo" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[65,603][1015,723]" displayed="true">
                        <android.widget.TextView index="0" package="com.test.main" class="android.widget.TextView" text="ID" resource-id="com.test.main:id/label_userInfo" checkable="false" checked="false" clickable="false" enabled="true" focusable="true" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[65,630][257,695]" displayed="true" />
                        <android.view.View index="1" package="com.test.main" class="android.view.View" text="" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[257,603][693,723]" displayed="true" />
                        <android.widget.TextView index="2" package="com.test.main" class="android.widget.TextView" text="4453***4990" resource-id="com.test.main:id/tv_userInfo" checkable="false" checked="false" clickable="false" enabled="true" focusable="true" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[736,630][1015,695]" displayed="true" />
                      </android.widget.LinearLayout>
                    </android.widget.LinearLayout>
                    <android.widget.FrameLayout index="3" package="com.test.main" class="android.widget.FrameLayout" text="" resource-id="com.test.main:id/ll_password_finger_container" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,723][1080,1703]" displayed="true">
                      <android.widget.LinearLayout index="0" package="com.test.main" class="android.widget.LinearLayout" text="" resource-id="com.test.main:id/ll_password_login_container" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,723][1080,1703]" displayed="true">
                        <android.widget.LinearLayout index="0" package="com.test.main" class="android.widget.LinearLayout" text="" resource-id="com.test.main:id/ll_password" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[65,723][1015,843]" displayed="true">
                          <android.widget.TextView index="0" package="com.test.main" class="android.widget.TextView" text="Password "heckable="false" checked="false" clickable="false" enabled="true" focusable="true" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[65,750][257,815]" displayed="true" />
                          <android.widget.EditText index="1" package="com.test.main" class="android.widget.EditText" text="•••" resource-id="com.test.main:id/et_password" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="true" long-clickable="true" password="true" scrollable="false" selected="false" bounds="[300,723][1015,843]" displayed="true" />
                        </android.widget.LinearLayout>
                        <android.widget.LinearLayout index="1" package="com.test.main" class="android.widget.LinearLayout" text="" resource-id="com.test.main:id/ll_login_btn" checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[0,924][1080,1005]" displayed="true">
                          <android.widget.Button index="0" package="com.test.main" class="android.widget.Button" text="登录" resource-id="com.test.main:id/btn_confirm" checkable="false" checked="false" clickable="true" enabled="true" focusable="true" focused="false" long-clickable="false" password="false" scrollable="false" selected="false" bounds="[259,924][821,1005]" displayed="true" />
                        </android.widget.LinearLayout>
                      </android.widget.LinearLayout>
                    </android.widget.FrameLayout>
                  </android.widget.RelativeLayout>
                </android.widget.FrameLayout>
              </android.widget.FrameLayout>
            </android.widget.RelativeLayout>
          </android.widget.FrameLayout>
        </android.support.v4.widget.SlidingPaneLayout>
      </android.widget.FrameLayout>
    </android.widget.LinearLayout>
  </android.widget.FrameLayout>
</hierarchy>
```