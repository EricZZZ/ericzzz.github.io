# Linux安装部署Nginx

## 1.下载Nginx

http://nginx.org/en/download.html
## 2.上传到服务器上

解压
```shell
tar -xzf nginx-1.16.1.tar.gz
```
## 3.安装

进入nginx 解压后的目录并执行
```shell
./configure
```
nginx会自动检查运行nginx所需要的库
其中这些库必须要安装
- gcc++：nginx 编译需要这个库
- PCRE：nginx 的http模块使用pcre库来解析正则表达式
- zlib：zlib库提供很多解压和压缩的方式
- OpenSSL：nginx支持http, https协议，OpenSSL库提供了常用的密钥和证书封装管理功能及SSL协议

安装 gcc++
```shell
yum install gcc-c++
```
安装 PCRE
```shell
yum install pcre pcre-devel
```
安装 zlib
```shell
yum install zlib zlib-devel
```
安装 OpenSSL
```shell
yum install openssl openssl-devel
```
编译nginx
```shell
make
make install
```
## 4.启动、停止Nginx

```shell
cd /usr/local/nginx/sbin/
./nginx # 启动
./nginx -s stop # 强制停止
./nginx -s quit # 停止 （任务处理完停止）
./nginx -s reload # 重新加载配置
```
查询nginx进程
```shell
ps -ef|grep nginx
```
## 5.遇到的问题

### 1.启动完成后，访问80端口被拒绝

检查本地80端口服务是否正常
```shell
curl 127.0.0.1
```
通过curl 命令检查是否有返回，有返回说明服务正常
检查防火墙端口是否打开
```shell
firewall-cmd --zone=public --query-port=80/tcp
```
返回 yes 说明打开，no说明关闭
打开防火墙 80端口
```shell
firewall-cmd --zone=public --add-port=80/tcp --permanent
```
--permanent 永久生效，没有此参数重启后失效
重新载入
```shell
firewall-cmd --reload
```


