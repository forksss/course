# centos java环境

### 1. 下载Jdk:

```
wget  --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/10.0.2+13/19aef61b38124481863b1413dce1855f/jdk-10.0.2_linux-x64_bin.rpm

#或者:
curl -LO -H "Cookie: oraclelicense=accept-securebackup-cookie" \
"http://download.oracle.com/otn-pub/java/jdk/10.0.2+13/19aef61b38124481863b1413dce1855f/jdk-10.0.2_linux-x64_bin.rpm"

```
    由于oracle页面下载是需要cookie，所以wget下载是需要加上--no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie"

### 2. 安装jdk

```shell
rpm -Uvh jdk-10.0.2_linux-x64_bin.rpm 
```

### 3. 设置环境变量

```
# 编辑环境变量配置文件
vi /etc/profile

# add follows to the end
export JAVA_HOME=/usr/java/default
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/jre/lib:$JAVA_HOME/lib:$JAVA_HOME/lib/tools.jar
```

### 重新执行刚修改的初始化文档
source /etc/profile

### 查看java注册信息
alternatives --config java 

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

### JDK 

```shell
cd jdk1.7.0_79
vi /etc/profile
#错误：-bash: /mnt/pgmfiles/jdk1.7.0_79/bin/javac: Permission denied
# 找到对应文件进行赋权
 /usr/local/jdk/bin/java看看这个文件的权限
cd /mnt/pgmfiles/jdk1.7.0_79/bin
# 没有执行的权限

ls -l
 
chmod +x java
chmod +x javac
#环境变量里的JDK已经给你卸了，找不到了就到系统默认的/usr/local/j dk/bin/这个路径找
执行：javac  java -version

===修改配置======
vi /etc/profile

export JAVA_HOME=/mnt/pgmfiles/jdk1.7.0_79
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH
===最后执行======
source /etc/profile
```