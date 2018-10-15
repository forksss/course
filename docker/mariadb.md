# Mariadb 在docker中配置

## Mariadb 安装

```
docker pull Mariadb
```

## Mariadb 配置

### 第一步创建一个mariadb容器

```
docker run  --name mariadb3306 \
-p 3306:3306 \
-v ~/docker/mariadb/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=mysql.root \
-d mariadb
```

### 第二步把mariadb自动生成的my.cnf拷贝到映射目录

```bash
# 拷贝文件
docker cp mariadb3306:/etc/mysql/my.cnf ~/docker mariadb/conf/ 
# 停止刚创建的容器
docker stop mariadb3306
# 删除开始创建的容器
docker rm mariadb3306

```

### 第三步修改my.cnf

```bash
vim ~/docker/mariadb/conf/my.cnf

# [mysqld] 下添加配置项
# 忽略数据库表名的大小写区分
lower_case_table_names = 1
# 解决时区与中国时区不至问题
default-time_zone=+8:00
```

### 最后生新创建容器

```bash
# 重新创建容器并挂载外部配置文件
docker run  --name mariadb3306 \
-p 3306:3306 \
-v ~/docker/mariadb/conf:/etc/mysql/conf.d \
-v ~/docker/mariadb/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=mysql.root \
-d mariadb
```
