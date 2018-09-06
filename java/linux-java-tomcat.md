## 安装Nginx

### 第一步：准备ngnix包

nginx 官网下载地址: https://nginx.org/en/download.html

```
wget https://nginx.org/download/nginx-1.15.3.tar.gz

# 执行：tar zxvf nginx-1.12.2.tar.gz
cd nginx-1.12.2
```

### 安装编译环境
```
# yum -y install pcre-devel openssl openssl-devel
yum install gcc gcc-c++
http: ./configure
https: ./configure --with-http_ssl_module
```
###  安装和配置nginx
```shell
make
make install

# 把原来nginx备份
cp /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.bak

# 把新的nginx覆盖旧的
cp objs/nginx /usr/local/nginx/sbin/nginx
```

### 问题解决
```shell
# 出现错误时cp: cannot create regular file ‘/usr/local/nginx/sbin/nginx’: Text file busy

# 用cp -rfp objs/nginx /usr/local/nginx/sbin/nginx解决
# 测试nginx是否正确
/usr/local/nginx/sbin/nginx -t
（nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful）
# 重启nginx
/usr/local/nginx/sbin/nginx -s reload

# 安装Nginx时报错

./configure:  error: the HTTP rewrite module requires the PCRE library.

# 安装pcre-devel解决问题
yum -y install pcre-devel

 

# 错误提示：./configure: error: the HTTP cache module requires md5 functions
from OpenSSL library.   You can either disable the module by using
--without-http-cache option, or install the OpenSSL library into the system,
or build the OpenSSL library statically from the source with nginx by using
--with-http_ssl_module --with-openssl=<path> options.

# 解决办法：

yum  -y install openssl openssl-devel
yum -y install openssl openssl-deve
```

### 总结：

```shell
yum -y install pcre-devel openssl openssl-devel

./configure --prefix=/usr/local/nginx

make

make install

nginx在reload时候报错invalid PID number

cd /usr/local/nginx/conf
../sbin/nginx -c /usr/local/nginx/conf/nginx.conf
../sbin/nginx -s reload

===============nginx:[emerg]unknown directive "ssl"=============

在centos中，配置nginx的https时，出现如下错误。
nginx: [emerg] unknown directive "ssl" in /usr/local/nginx/conf/nginx.conf:102

到解压的nginx目录下
./configure --with-http_ssl_module

当执行上面语句，出现./configure: error: SSL modules require the OpenSSL library.
用 yum -y install openssl openssl-devel
再执行./configure

重新执行./configure --with-http_ssl_module
make ，切记不能make install 会覆盖。

把原来nginx备份
cp /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.bak

把新的nginx覆盖旧的
cp objs/nginx /usr/local/nginx/sbin/nginx
出现错误时cp: cannot create regular file ‘/usr/local/nginx/sbin/nginx’: Text file busy

用cp -rfp objs/nginx /usr/local/nginx/sbin/nginx解决
测试nginx是否正确
/usr/local/nginx/sbin/nginx -t
（nginx: the configuration file /usr/local/nginx/conf/nginx.conf syntax is ok
nginx: configuration file /usr/local/nginx/conf/nginx.conf test is successful）
重启nginx
/usr/local/nginx/sbin/nginx -s reload

```

=