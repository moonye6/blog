## linux cmd


### d
##### df
使用：df -h

作用：查看磁盘使用情况

### l
##### ln
使用：ln -s /home/mysql/ /var/lib/mysql/
作用：创建软连接，可以理解成是一个快捷方式

### m
##### mv
使用： mv /a /b

### n
##### netstat
使用： netstat -anop

作用： 查看端口使用情况


### r
##### rz
使用：rz

作用：接受上传文件

### s
##### sz
使用：sz filename

作用：下载文件


## 案例
### 移动数据库到其他目录

```
//停止数据库
/etc/init.d/mysqld stop
//移动数据:
mv /var/lib/mysql/* /home/mysql/
//创建软连接:
ln -s /home/mysql/ /var/lib/mysql/
//启动mysql:
/etc/init.d/mysqld start
```

