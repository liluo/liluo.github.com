---
layout: post
title: "Python 使用 xlrd/xlwt 操作 Excel"
date: 2011-01-07 21:16
comments: true
categories: [Python]
---

Python 处理 Excel，可以使用 xlrd/xlwt 2个模块，使用简单特好上手。

<!-- more -->

## xlrd
安装
``` python
sudo easy_install xlrd # windows 参考http://pypi.python.org/pypi/xlrd
```
简单使用
``` python
import xlrd
data = xlrd.open_workbook('demo.xls') # 打开demo.xls
data.sheet_names()        # 获取xls文件中所有sheet的名称
table = data.sheets()[0]  # 获取xls文件第一个工作表
table = data.sheet_by_index(0)        # 通过索引获取xls文件第0个sheet
table = data.sheet_by_name(u'Sheet1') # 通过工作表名获取 sheet
# 获取行数和列数
nrows = table.nrows
ncols = table.ncols
# 获取整行和整列的值（数组）
table.row_values(i)
table.col_values(i)
# 循环行,得到索引的列表
for rownum in range(table.nrows):
    print table.row_values(rownum)
# 获取单元格
cell_A1 = table.cell(0,0).value
cell_C4 = table.cell(2,3).value
# 分别使用行列索引
cell_A1 = table.row(0)[0].value
cell_A2 = table.col(1)[0].value
# 简单的写入
row = 0
col = 0
ctype = 1 # 类型 0 empty,1 string, 2 number, 3 date, 4 boolean, 5 error
value = 'liluo'
xf = 0 # 扩展的格式化 (默认是0)
table.put_cell(row, col, ctype, value, xf)
table.cell(0,0) # 文本:u'lixiaoluo'
table.cell(0,0).value # 'lixiaoluo'
```

## xlwt
安装
``` python
sudo easy_install xlwt  # windows 参考http://pypi.python.org/pypi/xlwt
```
简单使用
``` python
import xlwt
file = xlwt.Workbook()                # 注意这里的Workbook首字母是大写
table = file.add_sheet('sheet name')  # 新建一个sheet

table.write(0,0,'test')               # 写入数据table.write(行,列,value)

# 如果对一个单元格重复操作，会引发
# returns error:
# Exception: Attempt to overwrite cell:
# sheetname=u'sheet 1' rowx=0 colx=0
# 所以在打开时加cell_overwrite_ok=True解决

table = file.add_sheet('sheet name',cell_overwrite_ok=True)
file.save('demo.xls')     # 保存文件

# 另外，使用style
style = xlwt.XFStyle()    # 初始化样式
font = xlwt.Font()        # 为样式创建字体
font.name = 'Times New Roman'
font.bold = True
style.font = font         #为样式设置字体
table.write(0, 0, 'some bold Times text', style) # 使用样式
```
xlwt 允许单元格或者整行地设置格式。还可以添加链接以及公式。
例子： <http://scienceoss.com/write-excel-files-with-python-using-xlwt/>
