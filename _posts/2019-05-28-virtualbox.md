---
title: VirtualBox 上安装 Linux 系统
---

利用 VirtualBox 安装 Linux 系统，方便管理，但是安装过程有些复杂，下面介绍如何用 VirtualBox 安装 Centos 和 Ubuntu 系统。

- [Centos Linux DVD ISO](https://www.centos.org/download/) 下载
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads) 下载
- [Ubuntu 18.04 LTS](https://ubuntu.com/download/desktop) 下载

## 1. 安装 VirtualBox

默认默认，下一步。。。完成安装。

## 2. 安装 Centos

1. 点击新建按钮。
2. 选择虚拟磁盘大小时，最好选 32 G 或以上，以后不用扩张。
3. 点击设置按钮。
4. 选择系统→启动顺序→光驱调至第一位。
5. 点击存储→没有盘片→右侧光盘图标→选择本地 ISO 镜像文件。
6. 点击启动，进去 Centos 安装界面。
7. 安装 Centos 时，软件选择选带 GUI 的，安装开发工具。

## 3. 安装 Ubuntu

和安装 Centos 的前 5 步一样，然后进入 Ubuntu 安装界面。

## 4. 常见问题

1. 安装增强功能的时候，需要将桌面的磁盘弹出。

2. 共享文件无权限访问。

   `sudo usermod -aG vboxsf $(whoami)`

   其中，`usermod -aG <group> <user>` 表示将用户`<user>`加入到（追加到）组`<group>`中，选项`[-aG]`是追加到组的意思。

3. 常在水上漂，难免会湿鞋，为了以防你搞崩系统，最好先备份，virtualbox 提供了快照功能。点击控制按钮，生成备份即可。