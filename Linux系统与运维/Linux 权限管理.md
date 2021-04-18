# Linux 权限管理

所谓权限管理，其实就是指对不同的用户，设置不同的文件访问权限，包括对文件的读、写、删除等，在 Linux 系统中，每个用户都具有不同的权限，拿非 root 用户来说，它们只能在自己的主目录下才具有写权限，而在主目录之外，只具有访问和读权限。

## 用户/用户组的权限设置
Linux 系统，最常见的文件权限有 3 种，即对文件的读（用 r 表示）、写（用 w 表示）和执行（用 x 表示，针对可执行文件或目录）权限。在 Linux 系统中，每个文件都明确规定了不同身份用户的访问权限，通过 ls 命令即可看到。
``` 
[root@localhost ~]# ls -al
total 156
drwxr-x---.   4    root   root     4096   Sep  8 14:06 .
drwxr-xr-x.  23    root   root     4096   Sep  8 14:21 ..
-rw-------.   1    root   root     1474   Sep  4 18:27 anaconda-ks.cfg
-rw-------.   1    root   root      199   Sep  8 17:14 .bash_history
-rw-r--r--.   1    root   root       24   Jan  6  2007 .bash_logout
```

我们可以通过两种方式修改用户/用户组的文件(rwx)权限：
- 使用数字进行修改
``` 
chmod -R 744 .bash_history
```

- 使用字母进行修改

``` 
chmod u=rwx,go=rx .bashrc
chmod a+w .bashrc
```
既然文件的基本权限就是 3 种用户身份（所有者、所属组和其他人）搭配 3 种权限（rwx）。

u、g、o 分别代表 3 种身份：所有者、所属组、其他人；
+、-、= 分别代表 3 种操作：加入、删除、设定；

## 默认权限的设置，[umask 命令](http://c.biancheng.net/view/764.html)
Linux 是注重安全性的操作系统，而安全的基础在于对权限的设定，不仅所有已存在的文件和目录要设定必要的访问权限，创建新的文件和目录时，也要设定必要的初始权限。

Windows 系统中，新建的文件和目录时通过继承上级目录的权限获得的初始权限，而 Linux 不同，它是通过使用 umask 默认权限来给所有新建的文件和目录赋予初始权限的。



## ACL 的权限设置 , [getfacl/setfacl 命令](http://c.biancheng.net/view/3132.html)

ACL，是 Access Control List（访问控制列表）的缩写，在 Linux 系统中， ACL 可实现对单一用户设定访问文件的权限。也可以这么说，设定文件的访问权限，除了用传统方式（3 种身份搭配 3 种权限），还可以使用 ACL 进行设定。拿本例中的 st 学员来说，既然赋予它传统的 3 种身份，无法解决问题，就可以考虑使用 ACL 权限控制的方式，直接对 st 用户设定访问文件的 r-x 权限。

- `getfacl` 查看文件或目录当前设定的 ACL 权限信息
``` 
➜  /www/release getfacl qdfk-bus/bootstrap/cache 
# file: qdfk-bus/bootstrap/cache  当前文件/目录
# owner: root  文件的所有者
# group: root  文件的所属组
user::rwx       # 所有者的权限
user:33:rwx     # uid=33用户的权限
user:82:rwx     # uid=82用户的权限
group::rwx      # 所属组的权限
mask::rwx       # mask权限 mask 权限，指的是用户或群组能拥有的最大 ACL 权限，也就是说，给用户或群组设定的 ACL 权限不能超过 mask 规定的权限范围，超出部分做无效处理。
other::---      # 其他人的权限
```

- `setfacl` 设定用户或群组对指定文件的访问权限
``` 
# 给用户组 tgroup2 对文件 project ，分配 rwx 的ACL权限
setfacl -m g:tgroup2:rwx project
# 给用户 luhj 对目录 src，分配 r-x 的ACL权限 
setfacl -R -m u:luhj:r-x src/
```

某个文件/目录设置了ACL权限后，`ls`展示的信息会不一样：
``` 
➜  /www/release/face-platform git:(release) ✗ ls -al bootstrap/cache
total 344
drwxrwxrwx  2 root root   4096 Apr  6 17:04 .
drwxr-xr-x  3 root root   4096 Apr  6 15:29 ..
-rwxr-xr-x  1 root root   1645 Apr  6 16:31 packages.php
-rwxrwxrwx+ 1 root root 189595 Oct 16 14:32 routes.php
-rwxrwxrwx+ 1 root root  95075 Dec  9 12:09 routes-v7.php
-rwxr-xr-x  1 root root  16330 Apr  6 16:31 services.php
```
会发现文件`routes.php`、`routes-v7.php`的权限位最后面带"+"的符号

## 文件特殊权限 [SetUID - SUID](http://c.biancheng.net/view/868.html) 

## 文件特殊权限 [SetGID - SGID](http://c.biancheng.net/view/870.html)

## 文件特殊权限 [Stick BIT - SBIT](http://c.biancheng.net/view/872.html)


