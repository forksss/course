## 清理mac

#### 1. 禁用休眠模式 --能节省4G左右 
下面的命令可以关闭OS X原生的休眠功能，也就是SafeSleep。这种休眠模式当Mac休眠或者没电池时会将内存中的内容储存在硬盘上的sleepimage文件上。sleepimage文件与Mac电脑的内存一样大，这意味着如果你的内存是4GB，该文件就有4GB，如果是16GB，该文件就有16GB。关闭SafeSleep可以不让系统自动创建该文件，缺点就是当Mac电脑没电池时，你不能恢复到之前的状态。不过我们可以使用OS X的自动保存功能在电池将要耗尽的时候保存自己的工作。
禁用休眠模式的命令

```
sudo pmset -a hibernatemode 0
```
然后定位到/private/var/vm/删除已经存在的sleepimage文件

#### 关闭自动备份


```
sudo tmutil disablelocal
```

   
```
cd /private/var/vm/
```

使用下面的命令删除该文件

    
```
sudo rm sleepimage
```


最后我们要防止OS X继续创建该文件，所以我们需要下面的命令生成一个无法被替换的空文件

    
```
touch sleepimage
    chmod 000 /private/var/vm/sleepimage
```


当然，如果你想要重新开启SafeSleep功能，只需下面的命令即可。


```
sudo pmset -a hibernatemode 3
    sudo rm /private/var/vm/sleepimage
```


#### 2.  移除系统嗓音文件——可以节省出500MB-3GB+硬盘空间

如果你不适用文字转语音功能，那么你肯定不会使用到OS X内置的嗓音文件。你可以删除这些文件重新获得硬盘空间。在终端应用中，使用下面的命令即可，首先定位到文件所在文件夹：

    cd /System/Library/Speech/

然后执行删除命令，将所有嗓音文件删除

    sudo rm -rf Voices/*

#### 3. 删除所有系统日志——可以节省出100MB-2GB硬盘空间

随着你使用Mac的时间越来越长，系统日志文件也会越来越多，根据电脑的用量、错误和服务，这些文件会越来越多。这些系统日志文件是用来调试和排除故障的，如果你感觉没有用，可以使用下面的命令删除：

    sudo rm -rf /private/var/log/*
    
#### 4. 删除快速查看生成的缓存文件——可以节省出100MB-300MB硬盘空间

快速查看功能是OS X系统内置的文件预览功能，在Finder中选择任何文件后都可以点击空格来查看文件的详情。不过快速查看功能依靠缓存功能才能更流畅，而且这些缓存文件会一直增加，通过下面的命令移除缓存：

    sudo rm -rf /private/var/folders/
    
#### 5. 删除Emacs——可以节省出60MB+的硬盘空间

如果你都不知道什么是Emacs，那么你可以放心的将其移除。Emacs是终端中的文本编辑器，如果你使用的固态硬盘空间实在太小，那么删除它就是不错的选择，况且你还可以使用vi和nano在终端中编辑文本。下面是删除Emacs的命令：

    sudo rm -rf /usr/share/emacs/
    
#### 6. 删除临时文件——可以节省500MB-5GB硬盘空间

/private/var/tmp/是存放系统缓存的文件夹，通常情况下会在系统重启时清楚，不过有时确不会。而且如果你长时间不关闭Mac，也不重启的话，缓存文件会越来越多。使用下面的命令清楚这些临时文件：

    cd /private/var/tmp/
    rm -rf TM*
    
#### 7. 清除缓存文件——可以节省1GB-10GB硬盘空间

缓存文件有很多种，比如网页浏览记录，应用meta数据等等。这些缓存文件的容量究竟多大跟用户使用的应用有关，也与Mac重启的频率有关。此外，很多在线音乐播放app也会产生大量的缓存文件，我们可以通过下面的命令删除这些缓存文件：

    cd ~/Library/Caches/
    rm -rf ~/Library/Caches/*