# Linux如何安装、部署RocketMQ


## 1.环境配置

64位 Linux操作系统
64位 JDK1.8+
RocketMQ二进制版本 http://rocketmq.apache.org/dowloading/releases/
## 2.修改RocketMQ内存配置

修改runserver.sh、runbbroker.sh
```shell
JAVA_OPT="${JAVA_OPT} -server -Xms8g -Xmx8g -Xmn4g"
```
修改为
```shell
JAVA_OPT="${JAVA_OPT} -server -Xms256m -Xmx256m -Xmn128m -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=320m"
```
## 3.启动服务

Start Name Server
```shell
nohup sh bin/mqnamesrv &
tail -f ~/logs/rocketmqlogs/namesrv.log
```
启动成功
```shell
The Name Server boot success...
```
Start Broker
```shell
nohup sh bin/mqbroker -n localhost:9876 &
tail -f ~/logs/rocketmqlogs/broker.log
```
启动成功
```shell
The broker[%s, 172.30.30.233:10911] boot success...
```
## 4.部署可视化服务

```shell
java -jar rocketmq-console-ng-1.0.1.jar --rocketmq.config.namesrvAddr='135.224.85.5:9876'
```
## 5.停止服务

关闭namesrv服务
```shell
sh bin/mqshutdown namesrv
```
关闭broker服务
```shell
sh bin/mqshutdown broker
```
