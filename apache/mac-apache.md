### Apache

服务器信息

名称  | 详情
-- | --
Version |2.2.31 
use port| 80
program dir| /usr/local/var/apache2/
config path| /etc/apache2/httpd.conf
wwwroot|     /library/webserver/documents/
current Root | /users/shaipe/workspace/ui/eui

#### 启动操作

```
sudo apachectl start      // 启动Apache服务
sudo apachectl stop       // 停止Apache服务
sudo apachectl restart    // 重启Apache服务
```
#### 关闭apache随系统启动

```
sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist
```

#### 常用命令

```
apachectl -v       //查看版本信息
cd /etc/apache2   //切换工作目录
sudo cp httpd.conf httpd.conf.bak     //备份文件，以防不测，只需要执行一次就可以了(可以使用ls命令查看是否新增了httpd.conf.bak文件）
sudo cp httpd.conf.bak httpd.conf    //提示：如果后续操作出现错误！可以使用以下命令，恢复备份过的 httpd.conf 文件（此步骤不需执行）
sudo vim httpd.conf    //用vim编辑httpd.conf（vim里面只能用键盘，不能用鼠标）
/DocumentRoot   //查找DocumentRoot（搜索完后会出现下图界面）
```

#### 删除Apache
1. 停止apache服务

```
sudo apachectl stop
```

2. 删除apache目录

```
sudo rm -rf /etc/apache2
sudo rm -rf /usr/include/apache2
sudo rm -rf /usr/libexec/apache2
```
