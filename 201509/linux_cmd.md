## linux cmd

[c](https://github.com/moonye6/blog/blob/master/201509/linux_cmd.md#c)
[c](https://github.com/moonye6/blog/blob/master/201509/linux_cmd.md#c)
[c](https://github.com/moonye6/blog/blob/master/201509/linux_cmd.md#c)
[c](https://github.com/moonye6/blog/blob/master/201509/linux_cmd.md#c)
[c](https://github.com/moonye6/blog/blob/master/201509/linux_cmd.md#c)
[c](https://github.com/moonye6/blog/blob/master/201509/linux_cmd.md#c)[c](https://github.com/moonye6/blog/blob/master/201509/linux_cmd.md#c)


### c
#####cp
使用：cp filename /newpath/
      cp [-r] dir /newpath/    

作用：拷贝文件或文件夹到指定目录

### d
##### df
使用：df -h

作用：查看磁盘使用情况（主要是挂载盘的使用情况）

##### du
使用：du -h --max-depth=1

作用：查看磁盘使用情况（查看目录的使用情况）

### f
##### free
使用：free

作用：查看内存、buffer、cache的使用量



### l
##### ll
使用：ll -h
作用：列出当前目录列表以及每个文件的详情（文件大小以kb展示）

##### ln
使用：ln -s /home/mysql/ /var/lib/mysql/
作用：创建软连接，可以理解成是一个快捷方式

##### ls
使用：ls
作用：列出当前目录列表

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

### zip
##### zip
使用：zip -r temp.zip temp

作用：压缩文件

##### unzip
使用：unzi  temp.zip

作用：压缩文件


## 案例
### 移动数据库到其他目录

```
//停止数据库
servide mysqld stop
//备份数据库，一定一定要备份
cp -r /var/lib/mysql /data/mysql_bak
//移动数据库
mv /var/lib/mysql /data/
//创建软连接(软连接名：mysql，在var/lib目录下面，指向/data/mysql):
ln -s /data/mysql/ /var/lib/mysql
//启动mysql:
servide mysqld stop
```

