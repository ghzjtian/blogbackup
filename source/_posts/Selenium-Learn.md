---
title: Selenium 自动化测试(python3)
tags: [auto-test]
categories: [auto-test]
date: 2020-05-10 17:46:19
---

> Selenium 环境的搭建及简单的例子

<!-- more -->

## 0.参考
## 1. Install Python3
## 2. Install PyCharm (Python IDE/Editor)
## 3. First WebDriver program in Python
## 4. Run it on Chrome and Firefox Browsers
## 5.输出测试报告
## 6.示例代码
## 7.其它插件


***

## 0.参考
* 1.[https://www.selenium.dev/](https://www.selenium.dev/)
* 2.[https://selenium-python.readthedocs.io/getting-started.html](https://selenium-python.readthedocs.io/getting-started.html)

## 1.Install Python3

* 1.Mac 自带了 Python2, 我们安装 Python3 即可(Python3 与 Python2 可以共存，现在我们用 Python3 去开发). [下载地址](https://www.python.org/downloads/)

* 2.设置 python3 的 PATH 到环境变量(`sudo vi ~/.bash_profile`)中.

* 3.打开 Mac terminal, 输入 `python3 --version` ，如果正确显示了版本号(如下所示)，证明 Python3 已安装成功.(如想查看默认的 Python 版本号, 输入 `python --version` 即可)
 
```
$ python3 --version
Python 3.7.3
```

* 4.在 terminal 中输入 `pip3 --version`, 可以看到 pip3 的版本号.

```
$ pip3 --version
pip 19.0.3 from /usr/local/lib/python3.7/site-packages/pip (python 3.7)
```

## 2. Install PyCharm (Python IDE/Editor)
* 1.下载地址: [https://www.jetbrains.com/pycharm/download/#section=mac](https://www.jetbrains.com/pycharm/download/#section=mac)

## 3. First WebDriver program in Python
### 1.用 Pycharm 创建一个新项目.
* 1.选择 `New env using Virtualenv`.
* 2.Base Interpreter: `python 3.7` 
* 3.Inherit global site-packages.

### 2.安装 selenium
* 1.在项目根目录运行命令 `$ pip3 install selenium`

```
$ pip3 install selenium
Collecting selenium 
  Downloading https://files.pythonhosted.org/packages/80/d6/4294f0b4bce4de0abf13e17190289f9d0613b0a44e5dd6a7f5ca98459853/selenium-3.141.0-py2.py3-none-any.whl (904kB)
    100% |████████████████████████████████| 911kB 304kB/s 
Collecting urllib3 (from selenium)
  Downloading https://files.pythonhosted.org/packages/e1/e5/df302e8017440f111c11cc41a6b432838672f5a70aa29227bf58149dc72f/urllib3-1.25.9-py2.py3-none-any.whl (126kB)
    100% |████████████████████████████████| 133kB 66kB/s 
Installing collected packages: urllib3, selenium
Successfully installed selenium-3.141.0 urllib3-1.25.9
```

* 2.或者可以下载安装 [https://www.selenium.dev/downloads/](https://www.selenium.dev/downloads/)

### 3.下载及安装 `selenium Third Party browser driver`
##### 1.下载链接 [selenium Third Party browser driver download](https://www.selenium.dev/downloads/)
* 1.[ChromeDriver - WebDriver for Chrome](https://sites.google.com/a/chromium.org/chromedriver/)
* 2.[Firefox Driver](https://firefox-source-docs.mozilla.org/testing/geckodriver/Support.html)



## 4. Run it on Chrome and Firefox Browsers
#### 1.PyCharm 运行.
* 1.右键 `Run` 指定的文件
#### 2.命令行运行. 
* 1.terminal 到指定的目录
* 2.`python3 文件名`

## 5.输出测试报告
* 1.安装 `html-testRunner`

```
$ pip install html-testRunner
```

* 2.在文件中导入并使用 testRunner

> 如下代码要在 terminal 中运行才会有测试报告生成 !!

```
import HtmlTestRunner


if __name__ == '__main__':
    test_report_path = '/Users/glb_gz/Documents/python_workplace/PythonSeleniumSessions/reports/'
    unittest.main(testRunner=HtmlTestRunner.HTMLTestRunner(output=test_report_path))

```

## 6.示例代码

```
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
import unittest
import HtmlTestRunner
import time

# Selenium Python: Sample Project
# 1.   Create a test for Baidu Search
# 2.   Add implicit wait of 10 sec
# 3.   Maximize window
# 4.   Create Unit Tests
# 5.   Add HTML reporting library
# 6.   Run from command line


class GoogleSearch(unittest.TestCase):

    @classmethod
    def setUpClass(cls):
        chrome_exec_path = "/Users/glb_gz/Documents/python_workplace/PythonSeleniumSessions/drivers/chromedriver"
        cls.driver = webdriver.Chrome(executable_path=chrome_exec_path)
        cls.driver.implicitly_wait(10)
        cls.driver.maximize_window()

    def test_search_automationstepbystep(self):
        self.driver.get("https://www.baidu.com/")
        self.driver.find_element(By.NAME, "wd").send_keys("cheese" + Keys.RETURN)
        time.sleep(5)

    def test_search_raghav(self):
        self.driver.get("https://www.baidu.com/")
        self.driver.find_element(By.NAME, "wd").send_keys("Raghav Pal" + Keys.RETURN)
        time.sleep(5)

    @classmethod
    def tearDownClass(cls):
        cls.driver.close()
        cls.driver.quit()
        print("Test completed.")


if __name__ == '__main__':
    test_report_path = '/Users/glb_gz/Documents/python_workplace/PythonSeleniumSessions/reports/'
    unittest.main(testRunner=HtmlTestRunner.HTMLTestRunner(output=test_report_path))


```


## 7.其它插件
#### 1.pytest

#### 2. Katalon recorder(Chrome Extensions)
* 1.自动记录在浏览器上做的操作，然后生成代码.
