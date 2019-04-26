---
layout: default
title: mac 挂载ntfs外置硬盘并读写
---

如何将NTFS格式的移动硬盘挂接到Mac OS上进行读写（Read/Write）操作
现在硬盘便宜，很多同学都有移动硬盘，如果你同时使用Windows与Mac OS的话，移动硬盘最好不要使用NTFS文件系统，否则在Mac OS上，你只能读你的移动硬盘，不能写。

但是实际上的情况是，移动硬盘上有很多东西了，且最初是格式化为了NTFS格式，这时候重新格式化是很麻烦的，要做数据移动。

这个问题有两种办法解决：

使用第三方软件。
使用mac os自带的mount_ntfs工具。
在Mac OS上mount NTFS文件系统的第三方软件，常用的是NTFS3G，这是一个开源软件，免费使用，不过官方只提供源代码，需要自行编译。

我们重点介绍第二种方法，使用mac os自带的mount_ntfs工具。

操作步骤如下：
1、打开命令行终端。

2、插上移动硬盘，这时候你在Finder里面看到此卷是只读的。

 

3、执行 diskutil info /Volumes/YOUR_V_NAME ，找出 Device Node 这个字段值，记录下来
        如，我的移动硬盘，是东芝的，那么执行 diskutil info /Volumes/Toshiba\ Portable\ Hard\ Drive/ ，有如下输出：
Device Identifier: disk1s1
Device Node: /dev/disk1s1
Part of Whole: disk1
Device / Media Name: Untitled 1

Volume Name: Toshiba Portable Hard Drive
Escaped with Unicode: Toshiba%FF%FE%20%00Portable%FF%FE%20%00Hard%FF%FE%20%00Drive

Mounted: Yes
Mount Point: /Volumes/Toshiba Portable Hard Drive
Escaped with Unicode: /Volumes/Toshiba%FF%FE%20%00Portable%FF%FE%20%00Hard%FF%FE%20%00Drive

File System Personality: NTFS
Type (Bundle): ntfs
Name (User Visible): Windows NT File System (NTFS)

Partition Type: Windows_NTFS
OS Can Be Installed: No
Media Type: Generic
Protocol: USB
SMART Status: Not Supported
Volume UUID: 0FDB1418-8175-4D31-A06E-09A5EBB35CF5

Total Size: 500.1 GB (500105217024 Bytes) (exactly 976768002 512-Byte-Blocks)
Volume Free Space: 430.2 GB (430243385344 Bytes) (exactly 840319112 512-Byte-Blocks)
Device Block Size: 512 Bytes

Read-Only Media: No
Read-Only Volume: Yes
Ejectable: Yes

Whole: No
Internal: No

4、弹出移动硬盘
执行 hdiutil eject /Volumes/Toshiba\ Portable\ Hard\ Drive/，如下输出
“disk1″ unmounted.
“disk1″ ejected.

5、创建一个目录，稍后将mount到这个目录
sodu mkdir /Volumes/MYHD

6、将移动硬盘以NTFS格式mount到上面的目录
sudo mount_ntfs -o rw,nobrowse /dev/disk1s1 /Volumes/MYHD/
执行完上面命令后，你可以看到你的移动硬盘灯又两起来了，没有任何输出，表示成功。

7、此时，你的移动硬盘可写了。 不过你不能在Finder里面操作，因为上面加了nobrowse选项。但是这个选项是必须的，否则你无法写。

最后，大家可能会问，为什么这么麻烦，要通过命令行来做，而不直接在Finder里面支持？
我的看法是mount_ntfs不是Mac OS的核心部分，可能是一些插件形式近来的，因此并没有将Finder与其绑定死