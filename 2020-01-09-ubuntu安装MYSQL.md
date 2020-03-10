---
title: mysql数据库安装
categories: 一阶段模块三
date: 2020-01-09
---





# MySQL For Ubuntu

## ubuntu安装mysql

ubuntu上安装mysql非常简单只需要几条命令就可以完成。

```
sudo apt-get install mysql-server          //服务端
sudo apt-get install mysql-client          //客户端
sudo apt-get install libmysqlclient-dev    //程序编译时链接的库
```

安装完成后使用`mysql -V`查看数据库版本

> 资料来源：https://www.cnblogs.com/lfri/p/10437694.html

## mysql服务器基本命令

**启动MySQL数据库服务**
`sudo service mysql start`
或
`sudo systemctl start mysql.service`
**重启MySQL数据库服务**
`sudo service mysql restart`
或
`sudo systemctl restart mysql.service`

**停止MySQL数据库服务**
`sudo service mysql stop`
或
`sudo systemctl stop mysql.service`

**查看MySQL运行状态**
sudo service mysql status
或
`sudo systemctl status mysql.service`

**设置MySQL服务开机自启动**
`sudo service mysql enable`
或
`sudo systemctl enable mysql.service`

**停止MySQL服务开机自启动**
`sudo service mysql disable`
或
`sudo systemctl disable mysql.service`

**MySQL的配置文件**
`sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf`

————————————————
版权声明：本文为CSDN博主「奉君逍遥」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/hwx865/article/details/90287715

## 权限问题

使用上述安装方法之后，是进行过全局配置的，不过在ubuntu18.04中会有权限问题，登录mysql必须要sudo

**就是报错1698**

`sudo mysql -u root`

为了解决以上问题需要删掉原有root用户，在创建新的root用户并赋予权限，具体操作如下：

首先使用sudo 进入mysql

```
～$ sudo mysql -u root
```

然后查询mysql.user表

```
mysql> select user, host, plugin FROM mysql.user;
+------------------+-----------+-----------------------+
| user             | host      | plugin                |
+------------------+-----------+-----------------------+
| mysql.session    | localhost | mysql_native_password |
| mysql.sys        | localhost | mysql_native_password |
| debian-sys-maint | localhost | mysql_native_password |
| root             | localhost | auth_socket           |
+------------------+-----------+-----------------------+
4 rows in set (0.00 sec)
```

在此处你将 auth_socket 更新成 mysql_native_password其实并不能解决问题。

这个时候我们需要将root用户删除并重新建立个root 用户并授权，具体操作步骤如下：

```
mysql> DROP USER 'root'@'localhost';
mysql> CREATE USER 'root'@'%' IDENTIFIED BY 'passwd';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';
```

然后再刷新权限

```
mysql> FLUSH PRIVILEGES;
```

最后

```
mysql> exit
```

然后就可以用普通账户登录了。

## 完全卸载mysql数据库

对于某一款软件必须学会卸载才是完全掌握，以下是卸载步骤。

首先在终端中查看MySQL的依赖项：dpkg --list|grep mysql



卸载： sudo apt-get remove mysql-common



卸载：sudo apt-get autoremove --purge mysql-server-5.7



清除残留数据：dpkg -l|grep ^rc|awk '{print$2}'|sudo xargs dpkg -P



再次查看MySQL的剩余依赖项：dpkg --list|grep mysql



继续删除剩余依赖项，如：sudo apt-get autoremove --purge mysql-apt-config


> 原文链接：https://blog.csdn.net/iehadoop/article/details/82961264 

## 关于给其他用户授予权限报错1044

主要原因是当前用户（一般是root）没有   授予其他人权限  的权限，因为安装的时候是删掉原本的root用户在新建的用户

所以我们需要给这个root用户赋予权限

1.查询一下当前用户有哪些权限，看到grant_priv下root用户的权限是N，这代表root用户没有权限，我们对此进行修改即可。

```
mysql> select host,user,grant_priv,Super_priv from mysql.user;
+-----------+------------------+------------+------------+
| host      | user             | grant_priv | Super_priv |
+-----------+------------------+------------+------------+
| localhost | root             				| N          | Y          |
| localhost | mysql.session     | N          | Y          |
| localhost | mysql.sys       		 | N          | N          |
| localhost | debian-sys-maint | Y          | Y          |
+-----------+------------------+------------+------------+

4 rows in set (0.00 sec)
```

2.修改权限，使用update

```
mysql> update mysql.user 
    -> set grant_priv='Y'
    -> where user='root';
Query OK, 1 row affected (0.00 sec)

Rows matched: 1  Changed: 1  Warnings: 0
```

3.重新查看用户权限可以看到，root用户grant_priv下编程了Y

```
mysql> select host,user,grant_priv,Super_priv from mysql.user;
+-----------+------------------+------------+------------+
| host      | user             | grant_priv | Super_priv |
+-----------+------------------+------------+------------+
| localhost | root             					| Y          | Y          |
| localhost | mysql.session  	  | N          | Y          |
| localhost | mysql.sys        			| N          | N          |
| localhost | debian-sys-maint | Y          | Y          |
+-----------+------------------+------------+------------+

4 rows in set (0.00 sec)
```

4.重新启动服务

> 资料来源：https://www.cnblogs.com/biaopei/p/8376941.html

## 关于远程连接报错2003

1.在控制台输入,进入mysql目录下，

sudo su  //进入root权限
cd /etc/mysql
1
2
2.打开my.cnf文件，找到 bind-address = 127.0.0.1 在前面加上#注释掉，如下：
#bind-address = 127.0.0.1
3.然后在添加如下代码；OK。

`skip-external-locking`
`skip-name-resolve`

> 原文链接：https://blog.csdn.net/qq_32144341/article/details/52403388

## 相关资料

> 文章：https://blog.csdn.net/hwx865/article/details/90287715
>
> 本文中安装等方法均未测试可能有错误，但是其中一些配置命令值得学习。