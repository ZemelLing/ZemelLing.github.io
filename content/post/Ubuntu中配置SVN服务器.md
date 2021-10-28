---
title: "Ubuntu中配置SVN服务器"
date: 2021-10-28T21:42:14+08:00
draft: false
tags: ["ubuntu", "linux"]
categories: ["linux", ]
---

## 安装SVN服务器及创建仓库

```shell
sudo apt-get install subversion
svnadmin create /home/pi/MY_SVN_REPOS
```

## SVN配置文件修改，进入到新建仓库下conf文件夹中，我们需要配置修改svnserve.conf、passwd以及authz三个文件。

```shell
pi@raspberrypi:~$ cd /home/pi/MY_SVN_REPOS/conf/
pi@raspberrypi:~/MY_SVN_REPOS/conf$ ls
authz  hooks-env.tmpl  passwd  svnserve.conf
```

svnserve.conf文件修改，主要将一下三点前的#号去掉即可

```shell
[general]
#1.匿名用户没有读写权限，认证用户有读写权限
anon-access = none
auth-access = write
#2.使用本配置文件目录下的passwd文件作为密码数据来源文件
password-db = passwd
#3.使用本配置文件目录下的authz文件作为读写权限管理认证文件
authz-db = authz
```
passwd文件修改，添加用户名和密码

```shell
[users]
# harry = harryssecret
# sally = sallyssecret
dengfu = dengfu123
```
authz文件修改，指定目录及用户权限

```shell
#根目录，用户dengfu具有读写权限
[/]
dengfu = rw
```
##  启动SVN服务，为了是以后树莓派启动的时候都能启动SVN服务，我们需要将启动SVN服务的指令添加到/etc/rc.local文件中。

```shell
pi@raspberrypi:~/MY_SVN_REPOS/conf$ sudo vi /etc/rc.local 

# rc.local
svnserve -d -r /home/pi/MY_SVN_REPOS

exit 0
```

## 启动服务

```shell
启动服务
svnserve -d -r /home/pi/svn
然后检查是否启动
ps -ef |grep svnserve
```