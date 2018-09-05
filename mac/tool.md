
#### 允许任何开发者的程序运行

```
sudo spctl --master-disable
```

#### Bash_profile 文件损坏修复办法

```
export PATH=/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
```

#### 修改终商名称
```
sudo scutil --set HostName NewName 
```
#### 命令数据拷贝 
```
# 从远程拷贝到本地 -r 可以不用填写
scp -r remote user@remote ip:remote file or directory local directory 
scp root@192.168.4.136:/opt/soft/nginx-0.5.38.tar.gz /opt/soft/
 # 从本地复制到远程
scp /opt/soft/nginx-0.5.38.tar.gz root@192.168.120.204:/opt/soft/scptest  
```
#### 查询磁盘使用情况

```
du -k subdir 
选项
-a或-all 显示目录中个别文件的大小。 
-b或-bytes 显示目录或文件大小时，以byte为单位。 
-c或--total 除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和。 
-k或--kilobytes 以KB(1024bytes)为单位输出。 
-m或--megabytes 以MB为单位输出。 
-s或--summarize 仅显示总计，只列出最后加总的值。 
-h或--human-readable 以K，M，G为单位，提高信息的可读性。 
-x或--one-file-xystem 以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过。 
-L<符号链接>或--dereference<符号链接> 显示选项中所指定符号链接的源文件大小。 
-S或--separate-dirs 显示个别目录的大小时，并不含其子目录的大小。 
-X<文件>或--exclude-from=<文件> 在<文件>指定目录或文件。 
--exclude=<目录或文件> 略过指定的目录或文件。 
-D或--dereference-args 显示指定符号链接的源文件大小。 
-H或--si 与-h参数相同，但是K，M，G是以1000为换算单位。 
-l或--count-links 重复计算硬件链接的文件。
```
[参考网站](http://man.linuxde.net/du)


#### 自动备份处理

```
#关闭自动备份
sudo tmutil disablelocal

#开启自动备份
sudo tmutil enablelocal
```

#### 文件查找

```
#查找所有包含.xx的文件
find . -name ".git"
```



#### Homebrew使用

```
Homebrew使用没啥好说的了，常用的
搜索软件：
brew search 软件名，如brew search wget
安装软件：brew install 软件名，如brew install wget

卸载软件：brew remove 软件名，如brew remove wget


centlingdeMacBook-Pro:openssl-1.0.2g centling$ ruby -e "$(curl -fsSLhttps://raw.githubusercontent.com/Homebrew/install/master/install)"
==> This script will install:
/usr/local/bin/brew
/usr/local/Library/...
/usr/local/share/doc/homebrew
/usr/local/share/man/man1/brew.1
/usr/local/share/zsh/site-functions/_brew
/usr/local/etc/bash_completion.d/brew

Press RETURN to continue or any other key to abort
==> /usr/bin/sudo /bin/mkdir /usr/local
==> /usr/bin/sudo /bin/chmod g+rwx /usr/local
==> /usr/bin/sudo /usr/sbin/chown centling:admin /usr/local
==> /usr/bin/sudo /bin/mkdir /Library/Caches/Homebrew
==> /usr/bin/sudo /bin/chmod g+rwx /Library/Caches/Homebrew
==> /usr/bin/sudo /usr/sbin/chown centling /Library/Caches/Homebrew
==> Downloading and installing Homebrew...
remote: Counting objects: 458, done.
remote: Compressing objects: 100% (416/416), done.
remote: Total 458 (delta 28), reused 277 (delta 17), pack-reused 0
Receiving objects: 100% (458/458), 701.87 KiB | 116.00 KiB/s, done.
Resolving deltas: 100% (28/28), done.
From https://github.com/Homebrew/brew
 * [new branch]      master     -> origin/master
HEAD is now at 496fff6 doco: more updates for core/formula separation
==> Tapping homebrew/core
Cloning into '/usr/local/Library/Taps/homebrew/homebrew-core'...
remote: Counting objects: 3660, done.
remote: Compressing objects: 100% (3545/3545), done.
remote: Total 3660 (delta 17), reused 425 (delta 3), pack-reused 0
Receiving objects: 100% (3660/3660), 2.75 MiB | 375.00 KiB/s, done.
Resolving deltas: 100% (17/17), done.
Checking connectivity... done.
Tapped 3535 formulae (3,686 files, 8.6M)
==> Installation successful!
==> Next steps
Run `brew help` to get started
centlingdeMacBook-Pro:openssl-1.0.2g centling$
```


#### brew安装node
  
```
1. 首先更新brew，使其在最新版本，代码如下：
$ brew update
  2. 确保brew是安全可靠的，代码如下：
$ brew doctor
将可能导致如下情况，可针对性逐条处理，处理完成放可完成下一步：
Warning: Some directories in /usr/local/share/man aren't writable.
This can happen if you "sudo make install" software that isn't managed
by Homebrew. If a brew tries to add locale information to one of these
directories, then the install will fail during the link step.
You should probably sudo chown -R $(whoami) them:
/usr/local/share/man/man5
/usr/local/share/man/man7
  1. 将brew的位置添加到$PATH环境变量中，并保存bash或者profile文件；
export PATH="/usr/local/bin:$PATH"
  2. 当处理完上述问题后，来处理brew和node关系
若在上文中出现，如下错误信息：
Warning: You have unlinked kegs in your Cellar
Leaving kegs unlinked can lead to build-trouble and cause brews that depend on
those kegs to fail to run properly once built. Run brew link on these:
node
则需要如下操作：
  ● 清理brew的link
$ brew cleanup
  ● 删除node文件，完全卸载node和npm
sudo rm -rf /usr/local/{lib/node{,/.npm,_modules},bin,share/man}/{npm*,node*,man1/node*}

或者是
sudo rm -rf /usr/local/bin/npm /usr/local/share/man/man1/node* /usr/local/lib/dtrace/node.d ~/.npm ~/.node-gyp /opt/local/bin/node opt/local/include/node /opt/local/lib/node_modules

或者是下面这样：
  1.在/usr/local/lib目录下，删除任何与node和 node_modules有关的目录；
  2.在/usr/local/include 目录下，删除任何与node 和 node_modules有关的目录；
  3.如果你是通过**brew install node**安装的node，则在终端执行**brew uninstall node** ，并在home目录下查找 **local** 或**lib** 或 **include**文件夹，删除任何与**node** 和 **node_modules**有关的目录；
  4.在**/usr/local/bin**目录下，删除任何与 **node** 执行文件；
  5.最后下载 **nvm** ，跟随它的介绍安装node。当然，你也可以通过**npm**来安装最新版本的Node。

  1. 通过brew安装node和npm
brew link node
brew uninstall node
brew install node

  1. 测试Node和npm安装是否成功，安装Grunt
npm install -g grunt-cli
如果安装成功，那么恭喜你node，npm，grunt均安装成功。若出现问题，请回顾前面内容
```
。