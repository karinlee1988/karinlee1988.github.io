---
layout: post
title: python selenium 学习笔记
date: 2021-01-28
Author: karinlee
categories: 
tags: [python,selenium]
comments: true
---
**Selenium 是一个用于Web应用程序测试的工具。Selenium测试直接运行在浏览器中，就像真正的用户在操作一样。**
**在python3中使用selenium，首先要cmd -> pip3 install selenium 安装selenium库，然后下载相应浏览器（ie，chrome等）的webdrive文件（例：chromedriver.exe），放在某个文件夹里（如D:\webdrive），再将该文件夹的路径加入到环境变量的path里。**
### 开始使用
```
from selenium import webdriver
```
通过网站的url，我们可以获取一个浏览器实例：
```
#  某个网址
url = "http://www.xxxxxx.com"
browser = webdriver.Chrome()
browser.get(url)
```
通过`browser.maximize_window()`可以最大化窗口
此时，我们可以对browser实例进行各种操作。
### 使用xpath，name，id等方式定位元素
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
### 切换frame
要定位元素，需要切换到相应的frame里才能定位。
#### 通过名称切换frame
```
browser.switch_to.frame("leftFrame")
```
frame的名称在F12在浏览器开发者工具 console处选取确定，也可以查看html代码确定元素在哪个frame中。
#### 切换到最顶层frame
```
browser.switch_to.default_content()
```
一般来说，切换frame都是从上向下一级级来进行切换的，这样就能尽量避免报错。
### onclick按钮
某些按钮是通过绑定js事件，点击后执行js代码实现的
实例：元素代码为` <a href="javascript:void(0)" onclick="a.Click(46,this)">`
这种按钮一般是切换到相应的frame后，直接用selenium执行js代码 模拟点击，`browser.execute_script('a.Click(46,this)')`执行onclick= 的语句。
要执行js代码，在浏览器中，可以在F12开发人员工具console控制台，选取好对应的frame后，直接粘贴代码在控制台运行。在selenium中，可以通过`browser.execute_script('js')`执行
### 截图
使用`browser.get_screenshot_as_file(path)`可以进行截图，截图默认为当前浏览器界面
实例：
```
browser.get_screenshot_as_file(f"C:\\Users\\karinlee\\Desktop\\screenshot_yd\\picturename.png")
```
