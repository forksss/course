# docker

## 安装

```
curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
```

### Docker 在Windows中使用

1. 在windows中启动好之后 docker pull xxx 报错如下错误解决办法

no matching manifest for unknown in the manifest list enties;

是因为docker的配置文件中的: experimental: false导致

docker在windows下的配置目录为: c:\programdata\docker\config\dadmon.json

修改: experimental: true

通过 docker info 查看是否配置成功


## 安装镜像

```
docker pull 镜像名
# 例如:
# 搜索镜像
docker search keywords // 例如搜索jumpserver
docker pull nginx
```

## 启动Docker

```
# 将镜像启动为容器
# -i 表示容器的标准输入打开 
# -t 表示分配的伪终端
# -d 表示后台启动
docker run ...

# 查看已启动的docker列表
docker ps 
# 删除镜像
docker rmi aming_centos:235123
# 进入docker
docker attach 实例名或id
# 将镜像导出为tar包
docker save -o /opt/docker-centos-httpd.tar centos:httpd
# 将tar包导入为本地镜像
docker load -i /opt/docker-centos-httpd.tar
# 将镜像centos:httpd运行为容器，将80端口映射到本机的9000，启动httpd
docker run -d -p 9000:80 centos:httpd /bin/sh -c /usr/local/bin/start.sh

# 镜像启动为容器之后，进入一个启动的容器
docker exec [OPTIONS] CONTAINER COMMAND [ARG...] [flags]

# 支持systemctl
docker run --privileged -d -p 10080:80 centos /sbin/init 


```