---
title: virtualbox
---

利用 virtualbox 安装虚拟机，很方便，但是安装过程有些复杂，下面介绍 centos 安装。

官方下载 centos 镜像（DVD ISO）

官方下载 virtualbox （或者去腾讯软件中心）

安装 virtualbox，一切默认

## 安装 centos

1. 选择虚拟磁盘大小时，最好选 32 G 以上，以后不用扩张。

2. 安装 centos 时，软件选择选带 GUI 的，安装开发工具。

3. 安装增强功能的时候，需要将桌面的磁盘弹出。

4. 共享文件无权限访问

   `sudo usermod -aG vboxsf $(whoami)`

   `usermod -aG <group> <user>`

   将用户`<user>`加入到（追加到）组`<group>`中，其中选项`[-aG]`是追加到组的意思。

## 快照

常在水上漂，难免会湿鞋，为了以防你搞崩系统，最好先备份，virtualbox 提供了快照功能。

需要你你的虚拟机上控制那里，生成备份即可。