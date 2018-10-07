# docker

## 安装

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