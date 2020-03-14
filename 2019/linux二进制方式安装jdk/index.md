# Linux二进制方式安装jdk

## 检查当前系统中的java
```bash
$ java -version
openjdk version "1.8.0_201"
OpenJDK Runtime Environment (build 1.8.0_201-b09)
OpenJDK 64-Bit Server VM (build 25.201-b09, mixed mode)

$ javac
zsh: javac: 未找到命令...
相似命令是： 'java'
```

## 卸载系统中已安装的openjdk
```bash
$ rpm -qa | grep jdk
copy-jdk-configs-3.7-2.fc29.noarch
java-1.8.0-openjdk-headless-1.8.0.201.b09-6.fc29.x86_64

$ sudo rpm -e java-1.8.0-openjdk-headless-1.8.0.201.b09-6.fc29.x86_64 
错误：依赖检测失败：
	java-headless >= 1:1.6 被 (已安裝) libreoffice-core-1:6.1.5.2-4.fc29.x86_64 需要
	libjvm.so()(64bit) 被 (已安裝) libreoffice-ure-1:6.1.5.2-4.fc29.x86_64 需要

$ sudo rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.201.b09-6.fc29.x86_64

$ rpm -qa | grep jdk                                                     
copy-jdk-configs-3.7-2.fc29.noarch

```

## 下载安装jdk
去oracle[下载jdk](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)，然后解压，将解压内容移动到合适的目录
```bash
$ sudo mv jdk1.8.0_202 /usr/lib/java 
```

## 配置
编辑`/etc/profile`文件
```bash
$ sudo vim /etc/profile
```
在文件末尾增加以下内容
```bash
export JAVA_HOME=/usr/lib/java/jdk1.8.0_202
export PATH=$PATH:$JAVA_HOME/bin
```
保存后使配置生效
```bash
$ source /etc/profile
```
## 验证安装成功
```bash
$ java -version
java version "1.8.0_202"
Java(TM) SE Runtime Environment (build 1.8.0_202-b08)
Java HotSpot(TM) 64-Bit Server VM (build 25.202-b08, mixed mode)

$ javac -version
javac 1.8.0_202
```

