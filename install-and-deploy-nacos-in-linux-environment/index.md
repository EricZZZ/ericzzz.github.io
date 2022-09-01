# Linux 环境下安装、部署Nacos


## 1.环境准备

64位Linux操作系统
64位JDK1.8+

## 2.下载源码或安装包

https://github.com/alibaba/nacos/releases
## 3.启动、关闭命令

```shell
sh startup.sh -m standalone # 单机模式启动命令
sh shutdown.sh # 关闭命令
```
## 出现的问题

运行脚本报错：**syntax error near unexpected token `$'{\r'**
这是由于项目是在Windows系统下编译，文件格式是DOC格式，与UNIX格式不符。需要把脚本文件格式改为UNIX格式。
打开脚本文件，用vi打开
```shell
vi startup.sh
```
输入
```shell
:set ff=unix
```
