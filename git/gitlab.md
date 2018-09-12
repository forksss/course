#### gitlab安装

参照

#### 执行git数据备份

```
sudo gitlab-rake gitlab:backup:create
# 默认备份文件储存在
/var/opt/gitlab/backups/140623891_gitlab_backup.tar
```


#### gitlab从备份中还原
 
```
1. sudo cp 140623891_gitlab_backup.tar  /var/opt/gitlab/backups/   
2. sudo gitlab-ctl stop unicorn  
3. sudo gitlab-ctl stop sidekiq  
4. sudo gitlab-rake gitlab:backup:restore BACKUP=140623891   -- 备份文件名的时间戳前缀  
5. sudo gitlab-ctl start  
6. sudo gitlab-rake gitlab:check SANITIZE=true
```

#### gitlab定时自动备份

```
/opt/gitlab/bin/gitlab-rake gitlab:backup:create
```

 
#### gitlab修改备份路径

```
修改/etc/gitlab/gitlab.rb文件
gitlab_rails['backup_path'] = '/mnt/backups'
```

 
#### gitlab恢复
1. ##### 停止相关数据连接服务

```
gitlab-ctl stop unicorn
gitlab-ctl stop sidekiq
```

 
2. ##### 从1393513186编号备份中恢复

```
gitlab-rake gitlab:backup:restore BACKUP=1393513186
```

 
3. ##### 启动Gitlab

```
sudo gitlab-ctl start
sudo gitlab-ctl reconfigure
```

 
#### Gitlab迁移
把备份文件拷贝到gitlab的备份目录下，根据上面gitlab恢复步骤即可。

从本地拷远程文件到本地目录:
```
scp root@192.168.4.136:/var/opt/gitlab/backups/1531286755_2018_07_11_11.0.2_gitlab_backup.tar /users/shaipe/
```

#### 