## Nginx Centos

### 1. install

```
    yum install openssl # 安装ssl模块
    yum install zlib    # zlib模块
    yum install nginx   # 安装 nginx
```

### 2. start

```
    systemctl start nginx.service   # 启动
    systemctl stop nginx.service    # 停止
    systemctl restart nginx.service # 重启
```

### 3. directory

modules dir:
    /usr/share/nignx/
    


##定义nginx运行的用户各用户组
user nginx nginx;
##nginx进程数，建议设置与cpu核心数一致
worker_processes 1;
worker_cpu_affinity 00000001 00000010 00000100 00001000 00010000 00100000 01000000 10000000;


##全局错误日志定义类型[ debug | info | notice | warn | error | crit ]
#error_log logs/error.log;
#error_log logs/error.log notice;
#error_log logs/error.log info;


##一个nginx进程打开的最多文件描述符数目，理论值应该是最多打开文件数（系统的值ulimit -n）与nginx进程数相除，但是nginx分配请求并不均匀，所以建议与ulimit -n的值保持一致。
worker_rlimit_nofile 65535;


##进程文件
#pid logs/nginx.pid;


##工作模式与连接数上限
events {
　　##参考事件模型，use [ kqueue | rtsig | epoll | /dev/poll | select | poll ]; epoll模型是Linux 2.6以上版本内核中的高性能网络I/O模型，如果跑在FreeBSD上面，就用kqueue模型。
　　use epoll;
　　##单个进程的最大连接数
　　worker_connections 65535;
}


##设置http服务器
http {
　　##引入外置配置文件
　　include /etc/nginx/conf.d/*.conf;


　　##文件扩展名与文件类型映射表
　　include mime.types;


　　##默认文件类型
　　default_type application/octet-stream;


　　##默认编码
　　#charset utf-8;
　　##服务器名字的hash表大小
　　#server_name_hash_bucket_size 128;


　　##上传文件大小限制   建议打开
　　client_header_buffer_size 32K;


　　##设定请求缓存 建议打开
　　large_client_header_buffers 4 64K;


　　##最大缓存
　　client_max_body_size 20M;


client_header_timeout        20;


　　##日志格式设定
　　#log_format main '$remote_addr - $remote_user [$time_local] "$request" '
　　# '$status $body_bytes_sent "$http_referer" '
　　# '"$http_user_agent" "$http_x_forwarded_for"';


　　##访问日志
　　#access_log logs/access.log main;


　　##开启高效文件传输模式sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如　　　　果图片显示不正常把这个改成off。
　　sendfile on;
　　##开启目录列表访问，合适下载服务器，默认关闭
　　#autoindex on;


　　##防止网络阻塞  建议打开
　　tcp_nopush on;
　　##防止网络阻塞  建议打开
　　tcp_nodelay on;


　　##长链接超时时间，单位是秒，为0，无超时　　
　　keepalive_timeout 65;


　　##gzip模块设置
　　##开启gzip压缩输出     建议打开
　　gzip on;
　　##最小压缩文件大小    建议打开
　　gzip_min_length 1k;
　　##压缩缓冲区   建议打开
　　gzip_buffers 4 16k;
　　##压缩版本（默认1.1，前端如果squid2.5请使用1.0）   建议打开
　　gzip_http_version 1.0;
　　##压缩等级
　　gzip_comp_level 2;     建议打开
　　##压缩类型，默认就已经包含了textxml,默认不用写，写上去也没有问题，会有一个warn    建议打开
　　gzip_types text/plain application/x-javascript text/css application/xml;
　　gzip_vary on;
　　##开启连接限制ip连接数使用
　　#limit_zone crawler $binary_remote_addr 10m;


##反向代理缓存Proxy Cache配置
proxy_temp_path  /opt/cache/nginx/temp;
proxy_cache_path /opt/cache/nginx/cache levels=1:2 keys_zone=infcache:600m inactive=1d max_size=2g;
proxy_cache_path  /opt/cache/nginx/proxy_cache_image  levels=1:2   keys_zone=cache_image:3000m inactive=1d max_size=10g;
proxy_connect_timeout 30; 
proxy_read_timeout        60; 
proxy_send_timeout        20; 
proxy_buffer_size        96k; 
proxy_buffers        8 256k; 
proxy_busy_buffers_size        512k; 
proxy_temp_file_write_size        512k;


#proxy_cache_path配置
#keys_zone=infcache:600m 表示这个zone名称为infcache，分配的内存大小为600MB
#/opt/cache/nginx/cache 表示cache这个zone的文件要存放的目录
#levels=1:2 表示缓存目录的第一级目录是1个字符，第二级目录是2个字符，即/data/ngx_cache/cache1/a/1b这种形式
#inactive=1d 表示这个zone中的缓存文件如果在1天内都没有被访问，那么文件会被cache manager进程删除掉
#max_size=10g 表示这个zone的硬盘容量为10GB
 
　　##FastCGI相关参数是为了改善网站的性能：减少资源占用，提高访问速度。
　　fastcgi_connect_timeout 300;
　　fastcgi_send_timeout 300;
　　fastcgi_read_timeout 300;
　　fastcgi_buffer_size 64k;
　　fastcgi_buffers 4 64k;
　　fastcgi_busy_buffers_size 128k;
　　fastcgi_temp_file_write_size 128k;
 
　　##负载均衡,weight权重，权值越高被分配到的几率越大
　　upstream myserver{
　　　　server 192.168.1.10:8080 weight=3;
　　　　server 192.168.1.11:8080 weight=4;
　　　　server 192.168.1.12:8080 weight=1;
　　}


　　##虚拟主机配置
　　server {
　　　　##监听端口
　　　　listen 80;


　　　　##域名可以有多个，用空格隔开
　　　　server_name localhost;


　　　　#charset koi8-r;
　　　　##定义本虚拟主机的访问日志
　　　　#access_log logs/host.access.log main;


　　　　location / {
　　　　　　root html;
　　　　　　index index.html index.htm;
　　　　}
　　　　##图片缓存时间设置
　　　　location ~.*.(gif|jpg|jpeg|png|bmp|swf)${
　　　　　　expires 10d;
　　　　}　　
　　　　##js和CSS缓存时间设置
　　　　location ~.*.(js|css)?${
　　　　　　expires 1h;
　　　　}
　　　　#error_page 404 /404.html;
　　　　# redirect server error pages to the static page /50x.html
　　　　#error_page 500 502 503 504 /50x.html;
　　　　location = /50x.html {
　　　　　　root html;
　　　　}
　　　　# proxy the PHP scripts to Apache listening on 127.0.0.1:80
　　　　#
　　　　#location ~ \.php$ {
　　　　　　# proxy_pass http://127.0.0.1;
　　　　#}
　　　　# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
　　　　#
　　　　#location ~ \.php$ {
　　　　　　# root html;
　　　　　　# fastcgi_pass 127.0.0.1:9000;
　　　　　　# fastcgi_index index.php;
　　　　　# fastcgi_param SCRIPT_FILENAME /scripts$fastcgi_script_name;
　　　　　　# include fastcgi_params;
　　　　#}
　　　　# deny access to .htaccess files, if Apache's document root
　　　　# concurs with nginx's one
　　　　#
　　　　#location ~ /\.ht {
　　　　　　# deny all;
　　　　#}
　　　　
　　　　##对 "/" 启用反向代理
　　　　location / {
　　　　　　##或者使用
　　　　　　#proxy_pass http://myserver;
　　　　　　proxy_pass http://127.0.0.1:88;
　　　　　　proxy_redirect off;
　　　　　　proxy_set_header X-Real-IP $remote_addr;#后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
　　　　　　proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
　　　　　　#以下是一些反向代理的配置，可选。
　　　　　　proxy_set_header Host $host;
　　　　　　client_max_body_size 10m; 　　　　　　#允许客户端请求的最大单文件字节数
　　　　　　client_body_buffer_size 128k; 　　　　　#缓冲区代理缓冲用户端请求的最大字节数，
　　　　　　proxy_connect_timeout 90; 　　　　　　#nginx跟后端服务器连接超时时间(代理连接超时)
　　　　　　proxy_send_timeout 90;　　　　　　　　 #后端服务器数据回传时间(代理发送超时)
　　　　　　proxy_read_timeout 90;　　　　　　　　 #连接成功后，后端服务器响应时间(代理接收超时)
　　　　　　proxy_buffer_size 4k; 　　　　　　　　　　#设置代理服务器（nginx）保存用户头信息的缓冲区大小
　　　　　　proxy_buffers 4 32k;　　　　　　　　　　 #proxy_buffers缓冲区，网页平均在32k以下的设置
　　　　　　proxy_busy_buffers_size 64k; 　　　　　　#高负荷下缓冲大小（proxy_buffers*2）
　　　　　　proxy_temp_file_write_size 64k;　　　　　　#设定缓存文件夹大小，大于这个值，将从upstream服务器传
　　　　}
　　　　##设定查看Nginx状态的地址
　　　　location /NginxStatus {
　　　　　　stub_status on;
　　　　　　access_log on;
　　　　　　auth_basic "NginxStatus";
　　　　　　auth_basic_user_file confpasswd;
　　　　　　#htpasswd文件的内容可以用apache提供的htpasswd工具来产生。
　　　　}
　　　　##本地动静分离反向代理配置
　　　　#所有jsp的页面均交由tomcat或resin处理
　　　　location ~ .(jsp|jspx|do)?$ {
　　　　　　proxy_set_header Host $host;
　　　　　　proxy_set_header X-Real-IP $remote_addr;
　　　　　　proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
　　　　　　proxy_pass http://127.0.0.1:8080;
　　　　}
　　　　##所有静态文件由nginx直接读取不经过tomcat或resin
　　　　location ~ .*.(htm|html|gif|jpg|jpeg|png|bmp|swf|ioc|rar|zip|txt|flv|mid|doc|ppt|pdf|xls|mp3|wma)$
　　　　　　{ expires 15d; }
　　　　location ~ .*.(js|css)?$
　　　　　　{ expires 1h; }
　　}
}