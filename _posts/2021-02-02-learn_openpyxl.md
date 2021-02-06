---
layout: post
title: openpyxl 学习笔记
date: 2021-02-02
Author: karinlee
categories: 
tags: [python,openpyxl]
comments: true
---
### 导入openpyxl库
`import openpyxl`
### 将xlsx文件打开，并将该文件对象赋予wb变量  
实例：
```
wb = openpyxl.load_workbook("测试.xlsx")
print(wb)
```
输出：
```
<openpyxl.workbook.workbook.Workbook object at 
0x00000219ED2D23C8>
```
此时，wb是一个`openpyxl.workbook`对象。
也可以用
```
wb=openpyxl.load_workbook("测试.xlsx",read_only=True)
```
采用只读形式打开xlsx文件。
如果在打开xlsx文件时不希望单元格的公式被读取出来，就要
```
wb=openpyxl.load_workbook("测试.xlsx",data_only=True)
```
此时，单元格的公式将不会被读取，只读取公式计算后的值。
### 获得工作薄对象wb后，可以通过以下方式进行操作
`wb.sheetnames `返回工作薄对象中所有工作表的名称`（:rtype  list）`
实例：
```
wb_sheetnames = wb.sheetnames
print(wb_sheetnames)
```
输出：
```
['Sheet1', 'Sheet2', 'Sheet3']
```
或者
```
for sheet in wb:   
    print(sheet.title)
```
输出： 
```
Sheet1

Sheet2

Sheet3
```
#### 通过sheet页的名称，获取wb工作薄对象里的一个工作表对象
实例：
```
ws= wb['Sheet1']
print(ws)
```
输出：
```
<Worksheet "Sheet1">
```
也可以通过`wb.sheetnames`获取所有sheet的名称，返回sheet名称的list后，通过索引获取指定工作表名称，从而获取worksheet对象
实例：
```
ws = wb[wb.sheetnames[0]]  
print(ws)
```
输出：
```
<Worksheet "Sheet1">
```
#### 创建和删除sheet页
实例：
创建名称为First sheet的sheet页，index=0 代表放在第1个位置，如果不加index参数，默认加在最后
```
wb.create_sheet(index=0,title='First sheet')
print(wb.sheetnames)
```
输出：
```
['First sheet', 'Sheet1', 'Sheet2', 'Sheet3']
```
删除名称为First sheet的sheet页
```
wb.remove_sheet(wb['First sheet'])
print(wb.sheetnames)
```
输出：
```
['Sheet1', 'Sheet2', 'Sheet3']
```
### 获取工作表对象ws后，可进行如下操作
#### 获取ws工作表对象中的单个单元格对象
实例：
```
cell = ws['A1']
print(cell)
```
输出：
```
<Cell 'Sheet1'.A1>
```
此时，cell变量是一个ws工作表对象中的A1单元格对象
#### 也可以用另外一种方式来获取单个单元格对象
实例：
```
cell = ws.cell(row=1,column=1)
print(cell)
```
输出：
```
<Cell 'Sheet1'.A1>
```
#### 对于一个单元格对象cell，可以通过cell.value获取或者修改该单元格里的数值
实例：
```
cell = ws['A1']
print(cell.value)
```
输出：
```
测试A1
```
该内容即为A1单元格里的数值
#### 可以通过赋值的形式来修改数据
实例：
```
cell = ws['A1']
print(cell.value)

cell.value = "测试修改"
print(cell.value)
```
输出：
```
测试A1

测试修改
```
此时 A1单元格的值已经从  测试A1   修改为    
测试修改
#### 获取单行，单列
实例：
```
colC = ws['C']
row4 = ws[4]

print(colC)
print(row4)
```
输出：
```
(<Cell 'Sheet1'.C1>, <Cell 'Sheet1'.C2>, <Cell 
'Sheet1'.C3>, <Cell 'Sheet1'.C4>, <Cell 
'Sheet1'.C5>, <Cell 'Sheet1'.C6>, <Cell 
'Sheet1'.C7>, <Cell 'Sheet1'.C8>)

(<Cell 'Sheet1'.A4>, <Cell 'Sheet1'.B4>, <Cell 
'Sheet1'.C4>, <Cell 'Sheet1'.D4>, <Cell 
'Sheet1'.E4>)
```
返回一个整行或者整列所有单元格对象组成的元组
#### 获取多行，多列
实例：
```
col_range = ws['B:C'] 

row_range = ws[2:3]

print(col_range)

print(row_range)
```
输出：
```
((<Cell 'Sheet1'.B1>, <Cell 'Sheet1'.B2>, <Cell 
'Sheet1'.B3>, <Cell 'Sheet1'.B4>, <Cell 
'Sheet1'.B5>, <Cell 'Sheet1'.B6>, <Cell 
'Sheet1'.B7>, <Cell 'Sheet1'.B8>), (<Cell 
'Sheet1'.C1>, <Cell 'Sheet1'.C2>, <Cell 
'Sheet1'.C3>, <Cell 'Sheet1'.C4>, <Cell 
'Sheet1'.C5>, <Cell 'Sheet1'.C6>, <Cell 
'Sheet1'.C7>, <Cell 'Sheet1'.C8>))



((<Cell 'Sheet1'.A2>, <Cell 'Sheet1'.B2>, <Cell 
'Sheet1'.C2>, <Cell 'Sheet1'.D2>, <Cell 
'Sheet1'.E2>), (<Cell 'Sheet1'.A3>, <Cell 
'Sheet1'.B3>, <Cell 'Sheet1'.C3>, <Cell 
'Sheet1'.D3>, <Cell 'Sheet1'.E3>))
```
可以看出，col_range和 row_range都是二维元组，col_range每个元素是一列单元格的元组，row_range每个元素是一行单元格的元组
#### 获取一个区域
实例：
```
cell_range = ws["B2:D4"]
print(cell_range)

```
输出：
```
((<Cell 'Sheet1'.B2>, <Cell 'Sheet1'.C2>, <Cell 
'Sheet1'.D2>), (<Cell 'Sheet1'.B3>, <Cell 
'Sheet1'.C3>, <Cell 'Sheet1'.D3>), (<Cell 
'Sheet1'.B4>, <Cell 'Sheet1'.C4>, <Cell 
'Sheet1'.D4>))

```
可以看出，`cell_range`是一个二维元组，`cell_range`每个元素是一行单元格的元组
**附：表格示例 测试.xlsx Sheet1**
| \ | A | B | C | D | E |
| --- | --- | --- | --- | --- | --- |
| 1 | 测试A1 |    |  |  |  |
| 2 |  |  |  |  |  |
| 3 |  |  |  |  |  |
| 4 |  |  |  |  |  |
| 5 |  |  |  |  |  |
| 6 |  |  |  |  |  |
| 7 |  |  |  |  |  |
|8  |  |  |  |  |  |

