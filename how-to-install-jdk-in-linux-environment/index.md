# Linux环境下如何安装JDK


## 1.下载JDK

根据需求下载不同版本的JDK

openJDK http://hg.openjdk.java.net/

OracleJDK https://www.oracle.com/technetwork/java/archive-139210.html

## 2. 上传至服务器并解压

上传至服务器相应的目录下

使用 tar zxvf 命令解压

## 3. 配置环境变量

配置当前用户下的环境变量

```bash
vi ~/.bash_profile
```

```bash
#在末尾加上
JAVA_HOME=/home/jenkinsadmin/jdk1.8.0_144 #此处应为jdk的解压目录
CLASSPATH=.:$JAVA_HOME/bin/tools.jar
PATH=$JAVA_HOME/bin:$PATH

export JAVA_HOME CLASSPATH PATH
```

## 4.使环境变量文件生效

```bash
java -version
```

返回结果如下，说明修改成功

```bash
openjdk version "1.8.0_40"
OpenJDK Runtime Environment (build 1.8.0_40-b25)
OpenJDK 64-Bit Server VM (build 25.40-b25, mixed mode)
```


