# Linux 安装、部署 Redis（单机）

## 1. 下载 Redis

http://www.redis.cn/download.html
下载相应 redis 版本，并上传到服务器上

## 2. 安装

解压，编译

```shell
tar -xzf redis-5.0.5.tar.gz
cd redis-5.0.5
make
```

编译完成后，通过如下命令开启 Redis

```shell
src/redis-server
```

## 3. 设置 Redis 远程连接，后台启动，设置密码

打开配置文件

```shell
vi redis.conf
```

设置后台启动

```shell
daemonize yes # no 修改为 yes 
```

设置远程连接

```shell
bind 135.224.85.5 # 修改为主机 ip
protected-mode no # yes 修改为 no
```

设置密码

```shell
requirepass 123456 # 设置密码为 123456
```

## 4. 重新开启 Redis

关闭 Redis 命令

```shell
lsof -i :6379 # 找到对应的 pid
kill -9 XXXX # 杀掉对应的 pid 进程
```

重启 Redis 命令

```shell
src/redis-server redis.conf # 进入 redis 目录下
```
