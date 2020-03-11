---
title: Linux 权限位详解
---

因为 Linux 是多用户操作系统，所以需要引入权限管理。你想想，资源是你的，被其他用户盗用了，你气不气！

## 1. 权限位解释

在 Linux 中，有 5 种权限，分别是 r、w、x、s、t。

1. 可读权限 ：r
2. 可写权限 ：w
3. 可执行权限 ：x
4. Setuid ：s（Set User ID）
5. Setgid ：s（Set Group ID）
6. 粘滞位 ：t

### 1.1 可读权限

文件和目录的可读权限设置一样：

1. 用字符表示：r
2. 用八进制表示：4
3. 可以对读取文件里的内容

### 1.2 可写权限

文件和目录的可写权限设置一样：

1. 用字符表示：w
2. 用八进制表示：2
3. 可以对文件进行更改

### 1.3 可执行权限

对于文件，可写权限：

1. 用字符表示：x
2. 用八进制表示：1
3. 可以执行该文件（脚本或命令）

对于目录，可写权限：

1. 用字符表示：x
2. 用八进制表示：1
3. 可以cd进入该目录

### 1.4 Setuid

这是一个特殊的权限位，对于文件：

1. 用字符表示：s
2. 用八进制表示：4000

`Setuid` 最常用的是配合执行权限 `x` 使用，例如，系统中内置命令 `passwd`，它默认是带有 `s` 权限位，`passwd` 命令的主要功能是修改用户的密码，而修改密码的流程是：

1. 将加密后的哈希值写入到 `/etc/passwd` 文件对应的用户条目中。
2. 使用 `pwconv` 工具转换到 `/etc/shadow` 文件中。
3. 而普通用户是没有权限修改`/etc/passwd` 和 `/etc/shadow` 文件的

在普通用户尝试执行 `passwd`，该 `passwd` 的所有者是 `root` 并且设置了 `Suid`，因此 `passwd` 以 `root` 身份执行。

### 1.5 Setgid

这是一个特殊的权限位，对于目录：

1. 用字符表示：s
2. 用八进制表示：2000

当一个目录拥有 `sgid` 权限时，其他用户在该目录下创建文件或目录后，它会继承目录的 `id`，即创建的文件或目录的属组为父目录的属组。

```bash
root# mkdir project
root# chmod 2777 project/
root# ls -lh
total 8.0K
drwxrwsrwx 2 root   root   4.0K Mar 11 16:29 project
drwxrwxr-x 3 ubuntu ubuntu 4.0K Mar 11 16:00 xv6-expansion
root# su ubuntu
ubuntu# mkdir project/test_for_ubuntu
ubuntu# ls -lh project 
total 4.0K
drwxrwsr-x 2 ubuntu root 4.0K Mar 11 16:30 test_for_ubuntu
```

### 1.6 粘滞位

这是一个特殊的权限位，对于目录：

1. 用字符表示：t
2. 用八进制表示：1000

`/tmp` 目录就是使用了粘滞位 `t`，其作用是，在该目录下创建文件或目录后，仅允许其作者（所有者）进行删除操作。其他用户无法删除。

## 2. 样例演示

如下例子：

```bash
# ll 
total 4.0K
drwxrwxr-x 3 ubuntu ubuntu 4.0K Mar 11 16:00 xv6-expansion
```

第一个字符常用的含义如下：

1. `-`：常规文件
2. `b`：块特殊文件
3. `c`：高性能（“连续数据”）文件
4. `d`：目录
5. `l`：符号链接
6. `P`：FIFO（命名管道）
7. `s`：套接字
8. `?`：其他文件

2~4 个字符分别表示属主的读、写、执行权限。

5~7 个字符分别表示属组的读、写、执行权限。

8~10 个字符分别表示其他人的读、写、执行权限。

接下来后面的信息分别是属主、属组、文件大小、最后一次修改的时间、文件或目录的名称。

## 3. 掩码

`umask` 是一个内置命令，其作用是指定创建的文件或目录的默认权限。

使用方法：`umask [-S|-p] [mode]`

- `[-s]` ：打印出字符权限位
- `[-p]` ：打印出八进制数权限位（默认）

使用不加任何参数的 `umask` 会打印出八进制的权限，有 4 位。

- 第 1 个数字表示：特殊权限位的八进制数
- 第 2 个数字表示：属主的八进制数的反掩码
- 第 3 个数字表示：属组的八进制数的反掩码
- 第 4 个数字表示：其他人的八进制数的反掩码

手动更改时，可使用八进制，也可以使用字符表示：

```bash
# mkdir test_dir
# touch test_txt
# ls -lh        
total 4.0K
drwxrwxr-x 2 ubuntu ubuntu 4.0K Mar 11 16:50 test_dir
-rw-rw-r-- 1 ubuntu ubuntu    0 Mar 11 16:50 test_txt
# rm -rf *      
# umask 777
# umask    
0777
# mkdir test_dir
# touch test_txt
# ls -lh
total 4.0K
d--------- 2 ubuntu ubuntu 4.0K Mar 11 16:52 test_dir
---------- 1 ubuntu ubuntu    0 Mar 11 16:52 test_txt
```

注意：若你的 `umask` 设置为 0000，后续创建文件的权限依然是 666。出于安全着想，执行权限必须手动添加。所以你会看到，目录权限为 777，而文件权限为 666。