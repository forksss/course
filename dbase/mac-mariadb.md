# MySql Mariadb
## 1. Mac
#### 1.1 install mariadb 

```
brew install mariadb --安装mariadb

cd /usr/local/cellar/mariadb/10.2.10/bin  --进入安装目录

./mysql.server start --启动mysql
```

#### 1.2 inital mariadb

```
mysql_secure_installation --mariadb 默认root账号是没有密码的,可以根据提示让输入当前密码时直接回车,然后一步一步进行初始化设置
```

#### create database

```
GBK: create database test2 DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;  

  

UTF8: CREATE DATABASE `test2` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;  
```

#### restore databae

```
source file path
```


首先进入数据库，使用系统数据库mysql。
```
 mysql -u root -p mysql #回车，然后输入则使用了系统数据库
```
接着对系统数据库的root账户设置远程访问的密码，与本地的root访问密码并不冲突。
```
grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option; #123456为你需要设置的密码
```
防火墙设置一下，不然3306端口还是无法访问。
```
 iptables -I INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
```
设置完之后，查看一下是否能通过。
```
 iptables -L -n
```

## CentOS7

### 安装Mariadb
```
yum -y install mariadb mariadb-server # 安装命令

systemctl start mariadb # 安装完成MariaDB，首先启动MariaDB

systemctl enable mariadb # 设置开机启动

mysql_secure_installation # 接下来进行MariaDB的相关简单配置

Enter current password for root (enter for none): # <–初次运行直接回车,首先是设置密码，会提示先输入密码

# 设置密码

Set root password? [Y/n] # <– 是否设置root用户密码，输入y并回车或直接回车
New password: #<– 设置root用户的密码
Re-enter new password: # <– 再输入一次你设置的密码

# 其他配置

Remove anonymous users? [Y/n] #<– 是否删除匿名用户，回车

Disallow root login remotely? [Y/n] # <–是否禁止root远程登录,回车,

Remove test database and access to it? [Y/n] # <– 是否删除test数据库，回车

Reload privilege tables now? [Y/n] # <– 是否重新加载权限表，回车

# 初始化MariaDB完成，接下来测试登录
mysql -uroot -ppassword 

```

2、配置MariaDB的字符集

文件/etc/my.cnf

vi /etc/my.cnf
在[mysqld]标签下添加

init_connect='SET collation_connection = utf8_unicode_ci' 
init_connect='SET NAMES utf8' 
character-set-server=utf8 
collation-server=utf8_unicode_ci 
skip-character-set-client-handshake
文件/etc/my.cnf.d/client.cnf

vi /etc/my.cnf.d/client.cnf
在[client]中添加

default-character-set=utf8
文件/etc/my.cnf.d/mysql-clients.cnf

vi /etc/my.cnf.d/mysql-clients.cnf
在[mysql]中添加

default-character-set=utf8
 全部配置完成，重启mariadb

systemctl restart mariadb
之后进入MariaDB查看字符集

mysql> show variables like "%character%";show variables like "%collation%";
显示为


+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client    | utf8                      |
| character_set_connection | utf8                      |
| character_set_database  | utf8                      |
| character_set_filesystem | binary                    |
| character_set_results    | utf8                      |
| character_set_server    | utf8                      |
| character_set_system    | utf8                      |
| character_sets_dir      | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)

+----------------------+-----------------+
| Variable_name        | Value          |
+----------------------+-----------------+
| collation_connection | utf8_unicode_ci |
| collation_database  | utf8_unicode_ci |
| collation_server    | utf8_unicode_ci |
+----------------------+-----------------+
3 rows in set (0.00 sec)

字符集配置完成。

 

3、添加用户，设置权限

创建用户命令

mysql>create user username@localhost identified by 'password';
直接创建用户并授权的命令

mysql>grant all on *.* to username@localhost indentified by 'password';
授予外网登陆权限 

mysql>grant all privileges on *.* to username@'%' identified by 'password';
授予权限并且可以授权

mysql>grant all privileges on *.* to username@'hostname' identified by 'password' with grant option;
简单的用户和权限配置基本就这样了。
