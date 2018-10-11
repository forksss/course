# Redis 安装及使用操作


## Redis 在Docker中的使用

### Docker安装

```
docker pull Redis # 安装最新版本的redis
```

### Redis运行

```
docker run -d \
-v ~/data/redis/conf/redis.conf:/usr/local/etc/redis/redus.conf \
--name redis \
-p 6379:6379
```