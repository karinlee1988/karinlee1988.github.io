---
layout: post
title: python selenium 学习笔记
date: 2021-01-28
Author: karinlee
tags: [python,selenium]
toc: true
---
Selenium 是一个用于Web应用程序测试的工具。Selenium测试直接运行在浏览器中，就像真正的用户在操作一样。



## 开始使用

**在python3中使用selenium，首先要cmd -> pip3 install selenium 安装selenium库，然后下载相应浏览器（ie，chrome等）的webdrive文件（例：chromedriver.exe），放在某个文件夹里（如D:\webdrive），再将该文件夹的路径加入到环境变量的path里。**

**如果不添加webdriver的文件路径到环境变量，也可以在实例化浏览器时传入webdriver文件路径的参数。**

~~通过网站的url，我们可以获取一个浏览器实例（旧方法，建议不要使用这种方法）：~~

```python
from selenium import webdriver
#  某个网址
url = "http://www.xxxxxx.com"
browser = webdriver.Chrome(r'D:\chromedriver.exe')# 没有添加path时
browser1 = webdriver.Chrome()# 已经添加path时
browser.get(url)
```

通过网站的url，我们可以获取一个浏览器实例（20211103新方法，建议使用）：
```python
from selenium import webdriver
from selenium.webdriver.common.by import By  # 选择元素时使用
from selenium.webdriver.chrome.service import Service

#  某个网址
url = "http://www.xxxxxx.com"
s= Service(r'C:\chromedriver.exe')
browser = webdriver.Chrome(service=s)# 没有添加path时
browser.get(url)


```


通过`browser.maximize_window()`可以最大化窗口
此时，我们可以对browser实例进行各种操作。

## 使用xpath，name，id等方式定位元素（旧）

```
browser.find_element_by_xpath()
browser.find_element_by_name()
browser.find_element_by_id()
```

实例：输入用户名，密码后点击登录过程
```
# name定位

browser.find_element_by_name("S_TLR_ID").send_keys("账号")
browser.find_element_by_name("S_TLR_PWD_CD").send_keys("密码")
browser.find_element_by_name("imageField").click()

# xpath 定位 （可以按F12在浏览器开发者工具element 使用鼠标点选元素，在元素代码里右键 copy xpath）

browser.find_element_by_xpath('/html/body/table/tbody/tr[3]/td[2]/table[1]/tbody/tr[1]/td[2]/div/input').send_keys("账号")
browser.find_element_by_xpath('/html/body/table/tbody/tr[3]/td[2]/table[1]/tbody/tr[2]/td[2]/div/input').send_keys("密码")
browser.find_element_by_xpath('/html/body/table/tbody/tr[3]/td[2]/table[1]/tbody/tr[3]/td/input').click()

```
某些输入框可能自带有信息了，可以通过`browser.find_element_by_xpath('xpath').clear()`清除掉里面的信息，再`send_keys("str")`
```
实例：

browser.find_element_by_xpath('//*[@id="S_BEGIN_DT"]').clear()
```
`.sendkeys("str")`可以在输入框对象里输入信息，`.click()`等同于单击该对象。

##  使用class,tag等方式定位元素（旧）
```
browser.find_elements_by_class_name()
browser.find_element_by_class_name()
browser.find_elements_by_tag_name()
```

使用find_elements_by_class_name 获取元素,返回包含多个webelement对象的列表。如果没有获取到，返回空列表
```
elements = browser.find_elements_by_class_name('animal')
for e in elements:
	print(e.text)
```
使用find_element_by_class_name 获取元素，返回class第1个元素。如果没有获取到，抛出异常
```
element = browser.find_element_by_class_name('animal')
print(element.text)
```

使用find_elements_by_tag_name标签获取（标签就是尖括号里的内容，如`<div>`,`<span>`这种
```
elements = browser.find_elements_by_tag_name('span')
for e in elements:
	print(e.text)
```


## 统一使用find_element()函数定位元素


20211103新方法：统一使用find_element()定位。find_element()函数定义如下：



```python
def find_element(self, by=By.ID, value=None) -> WebElement:
    """
    Find an element given a By strategy and locator.

    :Usage:
    ::

    element = driver.find_element(By.ID, 'foo')

    :rtype: WebElement
    """
```
因此，我们可以统一使用find_element()函数，并传入参数确定使用xpath，name，id或其他方式定位，不需要使用不同函数定位。实例如下：

```python
from selenium.webdriver.common.by import By  # 导入By模块

browser.find_element(by=By.XPATH,value='xpath表达式')

```

By模块结构如下：

```
class By(object):
    """
    Set of supported locator strategies.
    """

    ID = "id"
    XPATH = "xpath"
    LINK_TEXT = "link text"
    PARTIAL_LINK_TEXT = "partial link text"
    NAME = "name"
    TAG_NAME = "tag name"
    CLASS_NAME = "class name"
    CSS_SELECTOR = "css selector"

```
从上可知，By包含了XPATH在内的各种定位方式。

## 切换frame

要定位元素，需要切换到相应的frame里才能定位。

### 通过名称切换frame
```
browser.switch_to.frame("leftFrame")
```
frame的名称在F12在浏览器开发者工具 console处选取确定，也可以查看html代码确定元素在哪个frame中。
### 切换到最顶层frame
```
browser.switch_to.default_content()
```
一般来说，切换frame都是从上向下一级级来进行切换的，这样就能尽量避免报错。

## onclick按钮

某些按钮是通过绑定js事件，点击后执行js代码实现的
实例：元素代码为` <a href="javascript:void(0)" onclick="a.Click(46,this)">`
这种按钮一般是切换到相应的frame后，直接用selenium执行js代码 模拟点击，`browser.execute_script('a.Click(46,this)')`执行onclick= 的语句。
要执行js代码，在浏览器中，可以在F12开发人员工具console控制台，选取好对应的frame后，直接粘贴代码在控制台运行。在selenium中，可以通过`browser.execute_script('js')`执行

## 截图
使用`browser.get_screenshot_as_file(path)`可以进行截图，截图默认为当前浏览器界面
实例：
```
browser.get_screenshot_as_file(f"C:\\Users\\karinlee\\Desktop\\screenshot_yd\\picturename.png")
```
## 延时加载

在我们进行网页操作的时候， 有的元素内容不是可以立即出现的， 可能会等待一段时间。
Selenium 的 Webdriver 对象 提供` implicitly_wait`方法，接受一个参数， 用来指定 最大等待时长。

如果我们 加入如下代码`wd.implicitly_wait(10)`
那么后续所有的 find_element 或者 find_elements 之类的方法调用 都会采用上面的策略：
如果找不到元素， 每隔 半秒钟 再去界面上查看一次， 直到找到该元素， 或者 过了10秒 最大时长。当发现元素没有找到的时候， 并不 立即返回 找不到元素的错误。而是周期性（每隔半秒钟）重新寻找该元素，直到该元素找到，或者超出指定最大等待时长，这时才 抛出异常（如果是 find_elements 之类的方法， 则是返回空列表）。

