# centos java环境

### 1. 下载Jdk:

```
wget  --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/10.0.2+13/19aef61b38124481863b1413dce1855f/jdk-10.0.2_linux-x64_bin.rpm
```
    由于oracle页面下载是需要cookie，所以wget下载是需要加上--no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie"

### 2. 安装

```
rpm -ivh *.rpm
```

### 3. 设置环境变量

```
JAVA_HOME=/usr/java/jdk1.8.0_91
CLASSPATH=$JAVA_HOME/lib:$JAVA_HOME/jre/lib
PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin
export PATH CLASSPATH JAVA_HOME
```

## Centos Tomcat

### 1.tomcat下载
tomcat各版本下载地址
http://archive.apache.org/dist/tomcat/
下载
wget http://archive.apache.org/dist/tomcat/tomcat-7/v7.0.65/bin/apache-tomcat-7.0.65.tar.gz
### 2.解压
tar -xvf apache-tomcat-7.0.65.tar.gz
### 3.修改文件名
mv apache-tomcat-7.0.65 tomcat_xxx


