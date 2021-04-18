# Linux 权限管理

知识点来自[C 语言中文网-Linux权限管理](http://c.biancheng.net/linux_tutorial/70/)

## Linux 普通文件/目录权限
- `chgrp` 修改文件/目录所属的组
``` 
chgrp [-R] 所属组 文件名（目录名）
```

- `chown` 修改文件和目录的所有者和所属组
``` 
chown [-R] 所有者 文件或目录
chown [-R] 所有者:所属组 文件或目录
```

- `chmod` 修改文件权限
``` 
//使用数字修改文件权限
chmod -R 777 文件或目录
//使用字母修改文件权限
// u用户 g群组 o其他 a所有人
// +添加 -删减 =设定
// w写权限 r读权限 x执行权限
chmod -R u+w \webser
```

## [Linux acl权限](http://c.biancheng.net/view/3132.html)

ACL，是 Access Control List（访问控制列表）的缩写，在 Linux 系统中， ACL 可实现对单一用户设定访问文件的权限。也可以这么说，设定文件的访问权限，除了用传统方式（3 种身份搭配 3 种权限），还可以使用 ACL 进行设定。拿本例中的 st 学员来说，既然赋予它传统的 3 种身份，无法解决问题，就可以考虑使用 ACL 权限控制的方式，直接对 st 用户设定访问文件的 r-x 权限。

- `getfacl` 查看文件或目录当前设定的 ACL 权限信息
```
root@SZ-PC-00517:/webser/golang-project/src/git.myscrm.cn/aiot# getfacl id-card
# file: id-card
# owner: root
# group: root
user::rwx
user:luhj:rwx
group::rwx
mask::rwx
other::rwx
```

- `setfacl`

``` 
//设定 st 用户对 project 目录及其子目录（递归）具有 rx 权限
setfacl -R -m u:st:rx /project
```