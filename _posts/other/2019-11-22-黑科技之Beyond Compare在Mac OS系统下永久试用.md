---
layout: default
title: 黑科技之Beyond Compare在Mac OS系统下永久试用
---

亲测可用

一、原理
Beyond Compare每次启动后会先检查注册信息，试用期到期后就不能继续使用。解决方法是在启动前，先删除注册信息，然后再启动，这样就可以永久免费试用了。

二、下载
首先下载Beyond Compare最新版本，链接如下：https://www.scootersoftware.com/download.php


三、安装
下载完成后，直接安装。

四、创建BCompare文件
1.进入Mac应用程序目录下，找到刚刚安装好的Beyond Compare，路径如下/Applications/Beyond Compare.app/Contents/MacOS。
2.修改启动程序文件BCompare为BCompare.real。
3.在当前目录下新建一个文件BCompare，文件内容如下：

    ```
    #!/bin/bash
    rm "/Users/$(whoami)/Library/Application Support/Beyond Compare/registry.dat"
    "`dirname "$0"`"/BCompare.real $@
    ```
4.保存BCompare文件。
5.修改文件的权限：

chmod a+x /Applications/Beyond\ Compare.app/Contents/MacOS/BCompare
以上步骤完成后，再次打开Beyond Compare就可以正常使用了，enjoy it。
转自：https://blog.csdn.net/wu__di/article/details/82390196

作者：Looking_life
链接：https://www.jianshu.com/p/596b4463eacd
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。