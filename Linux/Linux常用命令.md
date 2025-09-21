# Linux命令及其相关概念

详细内容参考网站：[Linux命令大全(手册) – 真正好用的Linux命令在线查询网站](https://www.linuxcool.com/)

## Linux命令语法

- 一般情况下，**参数**是可选的，一些情况下**文件或路径**同样可选
- 参数，同一个命令，跟上不同的参数执行不同的功能
- 执行Linux命令，添加参数的目的是让命令更贴合工作
- Linux命令，参数之间，普遍应该用一个或者多个空格分隔

```bash
使用 --help可以查看一个命令的参数
[warghost@10 ~]$ ls --help
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first

其中，-a等参数是简写参数，--all等则是完整参数
```

参数之间可以组合，命令支持完整参数和组合参数

```bash
[warghost@10 ~]$ ls -a -l
total 40
drwx------. 16 warghost warghost 4096 Jul  7 22:35 .
[warghost@10 ~]$ ls -lh /var/log
total 2.9M
drwxr-xr-x. 2 root   root    204 Jun 29 21:52 anaconda
drwx------. 2 root   root     23 Jun 29 21:53 audit
[warghost@10 ~]$ ls -l /var/log
total 2952
drwxr-xr-x. 2 root   root       204 Jun 29 21:52 anaconda
drwx------. 2 root   root        23 Jun 29 21:53 audit
```

命令提示符，提示当前使用的用户名

其中 `warghost` 表示用户名，`@`是占位符，`10`表示当前的主机名，`~`表示当前所在文件夹下，`$` 是普通用户的标志字符，`# `则表示root用户的标识符

```bash
[warghost@10 ~]$ 
[root@10 warghost]#
```

> 为确保笔记中的命令不会被当成注释来使用，后续的root标识符都被替换成 $

## 基本命令

```bash
pwd  显示当前目录
[root@10 warghost]$ pwd
/home/warghost

cd    切换到指定目录，直接使用则切换到~目录
cd .. 切换到上级目录
cd -  切换到上一次的工作目录
[root@10 warghost]$ cd Desktop/
[root@10 Desktop]$ cd ..
[root@10 warghost]$ cd Desktop/
[root@10 Desktop]$ cd
[root@10 ~]$ 

mkdir   在当前目录下创建新目录，-p 一次性创建多个有嵌套关系的目录
[root@10 Desktop]$ mkdir test2
[root@10 Desktop]$ ls
test  test2
[root@10 Desktop]$ mkdir -p test2/test3

rmdir   删除当前目录下的空文件夹
[root@10 Desktop]$ rmdir test test2

cp     创建文件或目录的新副本，-r 递归复制目录，-i 覆盖旧文件时提示
[root@10 Desktop]$ cp test test2/
[root@10 Desktop]$ tree test2
test2
├── test
└── test3
[root@10 Desktop]$ cp -r test2 test4
[root@10 Desktop]$ tree test4
test4
└── test2
    ├── test
    └── test3

mv     移动或重命名文件，-i 覆盖旧文件时提示
[root@10 Desktop]$ mv test4 test5
[root@10 Desktop]$ ls
test  test2  test3  test5
[root@10 Desktop]$ tree test5
test5
└── test2
    ├── test
    └── test3
    
echo   打印一段字符串，配合''输出纯字符，搭配""会输出字符串中的特殊字符
[warghost@war ~]$ name=test
[warghost@war ~]$ echo $name
test
[warghost@war ~]$ echo '这是一段测试$name'
这是一段测试$name
[warghost@war ~]$ echo "这是一段测试$name"

    
hostname   能够查看当前的主机名
hostnamectl  能够查看当前主机的详细信息
[warghost@war ~]$ hostnamectl
   Static hostname: war
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 78fc72962b40471ab5ea0791a68bfcc6
           Boot ID: 27558d377c71436482ecf505c47519e4
    Virtualization: vmware
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-1160.el7.x86_64
      Architecture: x86-64
      
hostnamectl set-hostname  使用该命令可以更改主机名称，需要重新登录后才能看到变化
本质上是修改了/etc/hostname文件
[root@10 ~]$ hostnamectl set-hostname war
[warghost@war ~]$ 

uname   能够查看当前内核的相关内容，可以搭配 -a
[warghost@war ~]$ uname -a
Linux war 3.10.0-1160.el7.x86_64 #1 SMP Mon Oct 19 16:18:59 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux

man   Linux中提供的针对命令的使用手册
[warghost@war ~]$ man ls
LS(1)                                      User Commands                                      LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

shutdown  关机命令

clear  清空当前终端

history  查看使用的命令，默认3000条
[warghost@war ~]$ history 
    1  ifconfig
    2  ip address show
    3  ip a

```

## 用户相关命令

```bash
useradd  表示创建一个新用户，仅在root权限下能够使用
passwd   表示为用户设置一个密码
[root@10 warghost]$ useradd yxz
[root@10 warghost]$ passwd yxz
Changing password for user yxz.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.

su  能够切换到对应的用户（用户应该存在），没有对应用户名则切换到root
[yxz@10 ~]$ su -
Password: 
Last login: Mon Jul  7 22:56:43 CST 2025 on pts/0
[root@10 ~]$ su - yxz
Last login: Mon Jul  7 22:52:25 CST 2025 on pts/0

logout  能够返回上一个登录的用户，或者直接退出ssh远程控制
[yxz@10 ~]$ su -
Password: 
Last login: Mon Jul  7 22:58:39 CST 2025 on pts/0
Last failed login: Mon Jul  7 23:02:53 CST 2025 on pts/0
There was 1 failed login attempt since the last successful login.
[root@10 ~]$ logout
[yxz@10 ~]$

id  能够查看系统中的用户信息，查询用户是否存在，直接输入查看当前用户身份
[warghost@war ~]$ id yxz
uid=1001(yxz) gid=1001(yxz) groups=1001(yxz)

bash  能够再一次加载用户的环境变量，更新用户信息
[warghost@war ~]$ hostnamectl set-hostname test
[warghost@war ~]$ bash
[warghost@test ~]$ 
```

> `su`和`su -`之间完全不同，使用`su`命令只能获得root的执行权限，不能获得环境变量`PATH`，而使用`su -`能够切换到对应用户的环境变量并获得权限
>
> 如果使用`su`命令后无法使用`logout`命令，会提示`bash: logout: not login shell: use 'exit'`

## 远程连接相关命令

```bash
ip address show    可以简写成 ip a，查看当前ip地址
ip a
[root@10 Desktop]$ ip a
...
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:42:f8:e7 brd ff:ff:ff:ff:ff:ff
    inet 10.96.0.129/24 brd 10.96.0.255 scope global noprefixroute dynamic ens33
       valid_lft 1421sec preferred_lft 1421sec
    inet6 fe80::efa3:413f:146e:5e2/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
       
exit    登出当前用户，如是root用户则返回到基本用户
logout  同理
[warghost@10 ~]$ exit
logout
Connection closing...Socket close.

ssh     使用ssh命令远程连接
Windows中的帮助文档：
[C:\~]$ ssh
NAME
	ssh - connects to a host using the SSH protocol.

SYNOPSYS
	ssh [-pa ][-a ][-A ][-i user_key ][-J jump_host ][user:pass@]host[ port][;host[ port]]
		jump_host: [user:pass@]host[:port][,[user:pass@]host[:port]]

OPTIONS
	-pa    Use Password authentication. Ignore other auth parameters(-A -i).
	-a     Do not use Xagent for user authentication.
	-A     Use Xagent for user authentication.
	-i user_key     User key authentication.
	-J jump_host    Use jump host proxy.*
	user   Indicates the user's login name.
	pass   If pass is definded, use password authentication.
	host   Indicates the name, alias, or Internet address of the
	       remote host.
	port   Indicates a port number (address of an application).
	       If the port is not specified, the default ssh port
	       (22) is used.

使用ssh命令远程连接  命令中会默认指定22端口号，其他端口号需要指定
ssh root@10.96.0.129 22
[C:\~]$ ssh root@10.96.0.129 22
Connecting to 10.96.0.129:22...
Connection established.

Linux文档下的ssh帮助文档
[root@10 ~]$ ssh
usage: ssh [-1246AaCfGgKkMNnqsTtVvXxYy] [-b bind_address] [-c cipher_spec]
           [-D [bind_address:]port] [-E log_file] [-e escape_char]
           [-F configfile] [-I pkcs11] [-i identity_file]
           [-J [user@]host[:port]] [-L address] [-l login_name] [-m mac_spec]
           [-O ctl_cmd] [-o option] [-p port] [-Q query_option] [-R address]
           [-S ctl_path] [-W host:port] [-w local_tun[:remote_tun]]
           [user@]hostname [command]
           
不同于Windows，linux中的指令中使用-p命令添加端口参数
ssh root@10.96.0.129 -p 22
[root@10 ~]$ ssh root@10.96.0.129 -p 22
root@10.96.0.129's password: 
Last login: Sun Jun 29 22:50:34 2025 from 10.96.0.1
```

## 文件相关指令

```bash
touch  创建空文件
[root@10 Desktop]$ touch test
[root@10 Desktop]$ ls
test
如果当前目录存在该文件，则表示修改文件的访问时间属性

rm     删除文件，-r 参数递归删除目录
[root@10 Desktop]$ rm -r test2
rm: descend into directory ‘test2’? yes
rm: remove directory ‘test2/test3’? yes
rm: remove directory ‘test2’? yes

ls  查看当前目录下，-l 详细列表，-a 显示隐藏文件
[root@10 warghost]$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
[root@10 Desktop]$ ls -a
.  ..  test
[root@10 Desktop]$ ls -l
total 8
-rw-r--r--. 1 root root  6 Jun 30 16:01 test
drwxr-xr-x. 3 root root 19 Jun 30 15:58 test2
-rw-r--r--. 1 root root 17 Jun 30 20:32 test3

stat  可以详细查看文件状态
[yxz@war test]$ stat test3\ test 
  File: ‘test3 test’
  Size: 0         	Blocks: 0          IO Block: 4096   regular empty file
Device: fd00h/64768d	Inode: 4213042     Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1001/     yxz)   Gid: ( 1001/     yxz)
Context: unconfined_u:object_r:user_home_t:s0
Access: 2025-07-08 12:00:38.427212484 +0800
Modify: 2025-07-08 12:00:38.427212484 +0800
Change: 2025-07-08 12:00:38.427212484 +0800
 Birth: -
```

> `-rw-r--r--.`便是所谓的**权限位**，以`-`开头的是文件，以`d`开头的是目录，具体内容后续会补充

即便linux中后缀没有意义，我们还是选择人为的通过拓展名来区分文件的用途，需要记忆的文件后缀：

- 压缩文件：`.gz  .bz2  .zip  .tar.gz  .tar.bz2  .tgz`
- 软件安装包：windows系统下`.exe`，Linux中也有其他的软件包格式，比如redhat系列的`.rmp`
- 脚本文件：`shell脚本.sh  python脚本.py   .java`
- 网页相关：`.jpg  .png  .html  .js  .css`

## 进程相关命令

