
### Python in centos

```
#!/bin/bash
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
 
wget https://www.python.org/ftp/python/3.5.3/Python-3.5.3.tgz
 
mkdir /usr/local/python3 
tar -zxvf Python-3.5.3.tgz
cd Python-3.5.3

./configure --prefix=/usr/local/python3
make && make install
 
ln -s /usr/local/python3/bin/python3 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3
pip3 -V
python3 -V
```

1. python 中pip更新
```
    python -m pip install --upgrade pip
```

主要两步操作，查看需要升级库，升级库。如下：
```
pip list # 列出安装的库
pip list --outdated # 列出有更新的库
pip install --upgrade library_name # 升级库library_name
```