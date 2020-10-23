---
layout: post
title: pandas 学习笔记
date: 2020-10-08
Author: karinlee
categories: 
tags: [python,pandas]
comments: true
---

### 导入pandas库及常用类、函数

```
import pandas as pd
from pandas.core.frame import DataFrame
```

### pandas对excel表格进行读取、写入 
####  pandas使用read_excel()读取excel表格，返回DataFrame对象

实例：读取test.xlsx第一个sheet页的数据

```
dataframe = pd.read_excel("test.xlsx",sheet_name=0,header=None)
```

**pandas中定义read_excel函数如下：**


```
......
@deprecate_nonkeyword_arguments(allowed_args=2, version="2.0")
@Appender(_read_excel_doc)
def read_excel(
    # io参数为传入文件的路径
    io,
     # sheet_name参数默认为0，代表第1个sheet页，（从0开始索引）
     # 可根据自己的需要传入
     # sheet_name=None时返回一个以sheet名称（str）为key，对应sheet表格内容（dataframe）为value的字典
    sheet_name=0, 
     # header参数默认为0，默认excel表第1行为列名（表头），dataframe的数据部分不会包含第1行。
     # header=None 时，读入excel的第1行不再为列名（表头），而是作为数据部分。
    header=0,   
    names=None,
     # index_col默认为None。index_col=0时，第1列为行名，此时第1列不再是dataframe的数据部分。
    index_col=None,
    usecols=None,
    squeeze=False,
    # 可以设置参数dtype=str 强制读取excel时所有数据为str格式
    dtype=None,  
    engine=None,
    #converters参数指定某列类型。
    # 如 converters={'names':str,'ages':str} 
    # 将names列和ages列的数据转换为str
    # 也可以用列索引号 如converters={2:str}
    # 将第2列数据转为str格式（记住是从第0列开始的第2列！ ）
    converters=None,
    true_values=None,
    false_values=None,
    skiprows=None,
    nrows=None,
    na_values=None,
    keep_default_na=True,
    na_filter=True,
    verbose=False,
    parse_dates=False,
    date_parser=None,
    thousands=None,
    comment=None,
    skipfooter=0,
    convert_float=True,
    mangle_dupe_cols=True,
):
    ......
```

#### pandas使用to_excel()将DataFrame保存为excel表

```
# 保存为一个新的excel文件
# 假设result.xlsx不存在
dataframe.to_excel(r"result.xlsx",header=False,index=False)
```

```
# 向已有的excel文件写入，需要用到ExcelWriter()
# 假设result.xlsx已存在
writer = pd.ExcelWriter(r"result.xlsx")
dataframe.to_excel(writer,header=False,index=False)
writer.save()
```

**pandas中定义to_excel函数如下：**
```python
......
@doc(klass="object")
    def to_excel(
        self,
        # excel_writer参数一般为文件保存的路径，
        excel_writer,
        # 默认写入“Sheet1”工作表
        sheet_name="Sheet1",
        na_rep="",
        float_format=None,
        columns=None,
        # header和index默认为写入有列名和行索引（导致写入的第1行和A列有黑色框框，可选择为False。）
        header=True,
        index=True,
        
        index_label=None,
        startrow=0,
        startcol=0,
        engine=None,
        merge_cells=True,
        encoding=None,
        inf_rep="inf",
        verbose=True,
        freeze_panes=None,
    ) -> None:
       ......
```


- 测试总结1：read_excel和 to_excel都支持.xls和.xlsx格式（需要其他库支持） 

- 测试总结2：貌似公式计算的值可以被读取。读取为dataframe后再写入，只写入值，不保留公式  

- pd.ExcelWriter保存结果到已存在的excel文件中，并支持多个sheet表格写入excel。（注：pandas读出、写入excel数据时依赖通过read_excel、to_excel读出或写入excel时需要xlrd、xlwt库，调用ExcelWriter方法则需要openpyxl库。） 


### DataFrame处理

#### DataFrame转二维列表

`list = dataframe.values.tolist()`将dataframe转为二维列表list

#### 二维列表转DataFrame

`dataframe = DataFrame(list)`将二维列表list转换为dataframe
需要`from pandas.core.frame import DataFrame`

#### 提取行

**可以直接进行切片提取连续行**  

`new_dataframe = dataframe[0：2]` 提取第1-2行生成新DataFrame对象  

`new_dataframe2 = dataframe[2：]` 提取第2行到最后1行生成新DataFrame对象  

**也可以用.iloc方法提取行**  

`new_dataframe = dataframe.iloc[0：2,：] `提取第1-2行生成新DataFrame对象  

`new_dataframe2 = dataframe.iloc[2：,：]`提取第2行到最后1行生成新DataFrame对象  


#### 提取列

可以用dataframe.iloc[行索引,列索引]方法通过列索引提取列（索引均从0开始）  

`newdataframe1 = dataframe.iloc[:,[1,2]]`提取所有行，2-3列（":"表示选择所有）  

`newdataframe2 = dataframe.iloc[0:1,:]` 提取第1行，所有列  

`newdataframe3 = dataframe.iloc[[1,3,4],:]` 提取2，4，5行，所有列  

#### 复制DataFrame

anotherdataframe = dataframe.copy(deep=True) # 深拷贝


#### 获取DataFrame行列数

行数`len(df)`
列数 `df.shape[1]`


#### 遍历DataFrame

笨办法：通过`dataframe.iloc[行索引,列索引]`及用`len(df)`获取行数，`df.shape[1]`获取列数来遍历。

#### 拼接DataFrame

`df1.append(df2)`,此时df1在上方，df2在下方  
注意!：尽量不要在循环里使用append方法对DataFrame进行操作。经测试DataFrame不会追加内容。最好先将df1，df2转为二维列表list1，list2后，再用list1.extend(list2)循环拼接。拼接好后list1再转为DataFrame。


### ExcelFile()

#### 获取工作薄所有sheet页名称


```
# 返回sheet_list ，一个包含所有sheet页名称的列表
demo_excel = pd.ExcelFile(r'demo.xlsx')
sheet_list = demo_excel.sheet_names
```

