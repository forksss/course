## Nginx 反向代理配置

### nginx 启动，重启，停止 


```
nginx # 启动

nginx -s reload # 重启
nginx reload  #重启

# 通过进程结束的方式停止
```


### 站点配置
```
/conf/nginx.conf

Server #中进行配置

./nginx  #打开 nginx
nginx -s reload|reopen|stop|quit  #重新加载配置|重启|停止|退出 nginx
nginx -t   #测试配置是否有语法错误

nginx [-?hvVtq] [-s signal] [-c filename] [-p prefix] [-g directives]

-?,-h           : 打开帮助信息
-v              : 显示版本信息并退出
-V              : 显示版本和配置选项信息，然后退出
-t              : 检测配置文件是否有语法错误，然后退出
-q              : 在检测配置文件期间屏蔽非错误信息
-s signal       : 给一个 nginx 主进程发送信号：stop（停止）, quit（退出）, reopen（重启）, reload（重新加载配置文件）
-p prefix       : 设置前缀路径（默认是：/usr/local/nginx/）
-c filename     : 设置配置文件（默认是：/usr/local/nginx/conf/nginx.conf）
-g directives   : 设置配置文件外的全局指令

```

### 80端口被占用不能启动处理方式
```
看到80端口果真被占用。发现占用的pid是4，名字是System。怎么禁用呢？
 
1、打开注册表：regedit
 
2、找到：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\HTTP
 
3、找到一个REG_DWORD类型的项Start，将其改为0
 
4、重启系统，System进程不会占用80端口
 
重启之后，start nginx.exe 。在浏览器中，输入127.0.01，即可看到亲爱的“Welcome to nginx!” 了。
```

nginx配置gzip压缩

      默认情况下，Nginx的gzip压缩是关闭的，也只对只对text/html进行压缩，需要在编辑nginx.conf文件，在http段加入一下配置，常用配置片段如下：

      gzip    on;
      gzip_comp_level  6;    # 压缩比例，比例越大，压缩时间越长。默认是1
      gzip_types    text/xml text/plain text/css application/javascript application/x-javascript application/rss+xml;     # 哪些文件可以被压缩
      gzip_disable    "MSIE [1-6]\.";     # IE6无效

http的vary头在nginx中的配置方法

gzip_vary on