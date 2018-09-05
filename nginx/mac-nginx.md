###  Mac Nginx

#### Nginx信息

名称 | 说明
---|---
Version | 1.12.1
use prot | 8010
program dir | /usr/local/cellar/nginx
config path | /usr/local/etc/nginx/nginx.conf

#### Nginx 操作命令
```
sudo nginx -t #测试配置文件是否有错
sudo nginx  # 启动Nginx服务
sudo nginx -s stop # 停止Nginx服务
sudo nginx -s reload   # 重启nignx服务
```

#### 设置随系统启动
通过brew install nginx后设置开机启动项


```
sudo cp /usr/local/opt/nginx/*.plist /Library/LaunchDaemons #拷贝启动文件到系统启动目录
sudo launchctl load -w /Library/LaunchDaemons/homebrew.mxcl.nginx.plist #注册为系统服务,添加随系统启动
sudo launchctl unload -w /Library/LaunchDaemons/com.nginx.plist  #卸载系统服务
normanygtekimbp:LaunchDaemons normanyang$ ps -ef | grep nginx   #查看nginx是否启动
```


