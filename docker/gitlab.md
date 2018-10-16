# Docker 中使用gitlab

## 手动备份

```
# 第一种进行入容器执行命令的方法进行手工备份
docker exec -it 容器名或容器id bash # 进入容器
gitlab-rake gitlab:backup:create    # 执行gitlab备份命令

# 第二种直接使用外部命令执行,一次完成
docker exec 容器名或容器id gitlab-rake gitlab:backup:create
```

## 自动备份

需要使用linux的crontab的自动执行命令来进行备份
**注意这里是在宿主机上设置,在gitlab的docker中是没有crontab的**
1. 创建手动备份执行脚本

```
#！ /bin/bash
case "$1" in 
    start)
            docker exec gitlab-ce11.2.3 gitlab-rake gitlab:backup:create
            ;;
esac
```
2. 创建定时执行计划
```
# 使用crontab -e 进入定时任务编辑界面，新增如下内容
crontab -e
# 编辑定时任务
0 2 * * * /root/gitlab_backup.sh start


```

## Crontab 命令说明
crontab -e 进入
*  *  *  *  *  command
分  时  日  月  周  命令

其中，
第1列表示分钟，1~59，每分钟用*表示
第2列表示小时，1~23，（0表示0点）
第3列表示日期，1~31
第4列表示月份，1~12
第5列表示星期，0~6（0表示星期天）
第六列表示要运行的命令。


1.  /root/bin/autoback.sh # 添加自动备份执行文件
2.  /root/bin/autorun.sh    # 添加自动运行扫行文件
3. 使用crontab -e 添加了第天2点执行自动备份gitlab文件
4. 在docker中 pull redis 并添加了名为docker-ridis 的redis容器
5. 在/ect/rc.d/rc.local中添加了开机自动执行的/root/bin/autorun.sh脚本