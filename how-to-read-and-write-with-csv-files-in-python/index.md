# 如何使用 Python 操作 cvs 文件


使用数据库导出电子表格的格式是 cvs，有时还需要操作表格中的数据，用 Python 语言就是最好的选择。

``` python
import csv

# 定义一个临时数组，用于存放处理后的数据
new_list = []

# 读取 csv 文件内容
with open ('read_xxx.csv', newline='', encoding='utf-8') as csvfile:
    reader = csv.DictReader (csvfile)
    for row in reader:
        # 拿到表格中每一行数据，进行处理
        # TODO
        # 数据处理完成后，存入临时数组中
        new_list.append (row)


# 写入 csv 文件
with open ('write_xxx.csv', 'w', newline='') as csvfile:
    # 定义好表头，与读取时的电子表格保持一致
    fieldnames = [' 甲 ', ' 乙 ',' 丙 ',' 丁 ']
    writer = csv.DictWriter (csvfile, fieldnames=fieldnames)

    writer.writeheader ()
    for index in range (len (new_list)):
        # 逐条写入
        writer.writerow (new_list [index])
        print ("总共：% d, 处理：% d" % (len (new_list), index))
```

#### 小技巧💡

在使用 wps 打开 cvs 文件时，数字字符串会显示成为科学计数方式，很难受，解决此问题可以在字符串后加个 '\t' 转义字符就行了。

两种方式解决：
##### 1. 通过数据库
在导出 sql 语句中，使用 concat () 函数，例：concat (zoi.no,'\t')

##### 2. 使用 Python
在读取表格后处理，例：row [' 甲 '] = row [' 甲 '] + '\t'
