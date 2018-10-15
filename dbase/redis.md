# Redis 安装及使用操作


## Redis 在Docker中的使用

### Docker安装

```
docker pull Redis # 安装最新版本的redis
```

### Redis运行

```
# mac
docker run -d \
-v ~/docker/redis/conf/redis.conf:/etc/redis/conf/redis.conf \
-v ~/docker/redis/data:/data \
--name docker-redis \
-p 6379:6379 \
redis

# centos
docker run -d \
-v /srv/redis/conf/redis.conf:/etc/redis/conf/redis.conf \
-v /srv/redis/data:/data \
--name docker-redis \
-p 6379:6379 \
redis

docker run –name redis6700 \
–privileged=true \
-p 6700:6379 \
-v $PWD/data:/data \
-v $PWD/redis.log:/var/log/redis/redis.log \
-d redis redis-server /data/redis.conf
```

docker run -d \
-v /srv/mariadb/data:/data \
-p 3306:3306 \
--name docker-mariadb \
mariadb


sudo docker run  --name MariaDB \
    -p 3306:3306 \
    -v /srv/mariadb/data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=mysql.root \
    -d mariadb