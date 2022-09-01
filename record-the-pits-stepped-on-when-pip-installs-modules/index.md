# 记录pip安装模块时踩过的坑


在网上下了个python脚本，第一个问题如何安装包， 在使用pip 命令行，出现的问题记录下（**针对Windows用户**）
## 1.pip源更换为国内镜像

国内pip镜像源,**全部使用https**
```
  阿里云 https://mirrors.aliyun.com/pypi/simple
  中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple
  豆瓣(douban) https://pypi.douban.com/simple
  清华大学 https://pypi.tuna.tsinghua.edu.cn/simple
  中国科学技术大学 https://pypi.mirrors.ustc.edu.cn/simple
```
更换方法，本人使用windows操作系统
在C:\Users\你的用户名 ，在这个目录下创建pip文件夹，然后在pip目录下创建pip.ini文件，文件内容如下

```
[global]
index-url = https://pypi.mirrors.ustc.edu.cn/simple

[install]
trusted-host = pypi.mirrors.ustc.edu.cn
```
按照此上述操作不会再报以下错误
```
Could not fetch URL https://mirrors.aliyun.com/pypi/simple/pip/: There was a problem confirming the ssl certificate: HTTPSConnectionPool(host='mirrors.aliyun.com', port=443): Max retries exceeded with url: /pypi/simple/pip/ (Caused by SSLError(SSLEOFError(8, 'EOF occurred in violation of protocol (_ssl.c:997)'))) - skipping
```
**如果还出错请关闭翻墙代理软件！！！**
## 2.安装 Microsoft Visual C++ Build Tools

问题报错如下
```
distutils.errors.DistutilsPlatformError: Microsoft Visual C++ 14.0 or greater is required. Get it with "Microsoft C++ Build Tools": https://visualstudio.microsoft.com/visual-cpp-build-tools/ Testing support for clang
```
就是提示你需要安装 Microsoft Visual C++ Build Tools 了。
这是网址 https://visualstudio.microsoft.com/visual-cpp-build-tools/
