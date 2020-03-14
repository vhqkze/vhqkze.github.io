# Jmeter安装

## 下载
去[jmeter下载页面](https://jmeter.apache.org/download_jmeter.cgi)，下载Binaries文件。
## 安装
将下载文件解压，移动到`/opt`文件夹下，然后执行`bin/jmeter`文件
```bash
$ x apache-jmeter-5.1.1.tgz

$ sudo mv apache-jmeter-5.1.1 /opt

$ /opt/apache-jmeter-5.1.1/bin/jmeter           
================================================================================
Don't use GUI mode for load testing !, only for Test creation and Test debugging.
For load testing, use CLI Mode (was NON GUI):
   jmeter -n -t [jmx file] -l [results file] -e -o [Path to web report folder]
& increase Java Heap to meet your test requirements:
   Modify current env variable HEAP="-Xms1g -Xmx1g -XX:MaxMetaspaceSize=256m" in the jmeter batch file
Check : https://jmeter.apache.org/usermanual/best-practices.html
================================================================================
An error occurred: null
```
有错误发生，jmeter无法运行，检查java环境
```bash
$ java -version
openjdk version "1.8.0_201"
OpenJDK Runtime Environment (build 1.8.0_201-b09)
OpenJDK 64-Bit Server VM (build 25.201-b09, mixed mode)

$ javac
zsh: javac: 未找到命令...
相似命令是： 'java'
```
可知是java环境的问题，具体可以参考[linux二进制方式安装jdk](/2019/04/05/linux二进制方式安装jdk/)配置java环境，然后再次运行jmeter，运行成功。
```bash
$ /opt/apache-jmeter-5.1.1/bin/jmeter
================================================================================
Don't use GUI mode for load testing !, only for Test creation and Test debugging.
For load testing, use CLI Mode (was NON GUI):
   jmeter -n -t [jmx file] -l [results file] -e -o [Path to web report folder]
& increase Java Heap to meet your test requirements:
   Modify current env variable HEAP="-Xms1g -Xmx1g -XX:MaxMetaspaceSize=256m" in the jmeter batch file
Check : https://jmeter.apache.org/usermanual/best-practices.html
================================================================================
```

