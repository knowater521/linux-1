---
title: UEFI 启动模式 win10 + ubuntu18 双系统安装
---



1. 原理

   系统部署 + 系统引导。iso 镜像文件是系统的封装，UEFI 是引导模式，引导文件要找到部署好的系统才能正常启动。

2. 安装环境

   X64 硬件，win 10 + ubuntu18 + UEFI 引导 + GPT 分区格式

## 安装 win 10

如果没有安装 win 10 的可以安装下面顺序安装 win 10。

win 10 镜像文件下载：<https://msdn.itellyou.cn/>

利用迅雷下载，下载后文件需要和网站的文件哈希校对确保是文件无误。

利用 [rufus](<http://rufus.ie/>) 制作镜像文件。这里注意，分区类型选择 **GPT**，引导类型选择你的 iso 系统镜像，目标系统类型选择 UEFI 格式。其他默认安装即可。

1. 利用 U 盘启动，可以通过按键（联想是 F12）

2. 分区

   win 10 安装唯一要注意的是分区，如果你的电脑没有重要文件，先删除全部分区，然后创建第一个盘作为 C 盘，300 G 足矣，然后对其格式化（一般这时候会出现其他几个小分区给系统用，你可以看见有UEFI、系统恢复盘等等，还有一个290 多 G 的已经分配的空间）。其他空间显示未分配状态，先不管，点击 290 多G 的空间，下一步。

3. 剩下的都很简单了，自己设置。

## 安装 ubuntu 18

进入 UEFI 设置将 Secure Boot 设置为 disabled。

下载 ubuntu18： <https://www.ubuntu.com/download/desktop>

利用 [rufus](<http://rufus.ie/>) 制作 U 盘，利用 U 盘启动。这里特别注意，分区类型是 **GPT**，保证和 win 10 一致。

1. 选择直接安装

   选择语言

2. 选择键盘布局

3. 选择最小化安装，其他不用选，浪费时间。

4. 选择安装类型，一定要选择第三项，something else。

5. 分区挂载

   这不，我们有很多未分配的磁盘空间，ubuntu 盘符命名特点是 **sdxy** 。x 表示物理磁盘号，y 表示逻辑分区号。可以看见有 free space 的一行，我们将分出来一些安装 ubuntu 。

6. 挂载 **/** 

   选中 free space，点击 + ，size 我选 102400 M，也就是 100G，挂载点选 **/** ，其他默认，点击 OK。

7. 挂载 **/home**

   同 6 ，挂载点不一样而已

8. 最关键一步！！！ 将 Boot 安装在  Windows boot Manager 中。

   Boot 占用量很少，是 ubuntu 的启动引导，和 win 10 安装在同个启动盘，这样就可以在电脑启动的时候选择进入哪一个系统了。

   操作：在 device for boot loader installation 下面选择 Windows boot Manager 。OK ，点击下一步

9. 时区选择 shanghai

10. 账号设置，秘密设置。

11. 进入安装界面，等待完成。

