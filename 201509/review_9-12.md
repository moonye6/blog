### 重现一个问题 

周末过来，想做一下数据库迁移的事情。然后搜索数据库迁移的方案，找到两种方式可以实现
+ 移动数据库到指定目录，修改数据库对应的配置文件
+ 移动数据库到指定目录，建立一个软连接链接过去

第一种方式涉及到修改配置文件，第二种建立一个软链接就好了，论操作第二个比较简单，于是选择了第二种方式来操作。

```
mv /var/lib/mysql /data/mysql
ln -s /data/mysql/ /var/lib/mysql/
```
软连接建立成功 ，验证
```
cd /var/lib/mysql
```
发现没有切换到mysql目录下去，原来软连接建立到mysql目录下面了，其实应该建在`/var/lib`目录下面的

于是删除软连接
```
rm -rf /var/lib/mysql/mysql
rm -rf /var/lib/mysql
```

建好了，验证ok

启动数据库，发现启动失败。不知道原因，于是到mysql目录下`ls`一下，发现数据文件没了。。没了。。

 于是周末在恢复数据中度过。。。。。

### 回顾

再来回顾一下上面的步骤，重现问题。
第一反应是因为删除了软连接，导致源文件被一起删掉了。
后面自己找文件测试的时候，只删软连接或软连接的父目录，根本不会影响到源文件。

另外数据库迁移后，如果重新启动，是否会因为路径变化导致覆写了源文件。
这个在windows下验证是不会的，可以正常启动（原谅我没有linux环境可以用）

那这里是什么问题呢
`只能是直接删除了源文件`,软连接建立好了，如果切换到软连接目录下，pwd会发现当前路径是软连接的路径，并不会显示源文件的路径。但如果以为能在这里随意操作就错了，一旦切换到软连接下面，所有的操作针对的都是同一个文件（源文件）

### 后续验证 
1 在linux上使用ln命令创建硬链接，发现不能给一个目录创建硬链接（验证这个是因为硬链接真的很危险）
2  在windows下做了一个测试（实在没有linux机器做测试了）
建立软连接`mklink link src`，数据库配置文件指向软连接，启动数据库，启动失败，报1067错误，错误原因是配置的数据文件路径出错。
而查资料发现，windows下建立软连接的方式是需要带参数`mklink /D link src`，再试发现数据库可以正常启动ok

### 总结

1 重要数据一定要备份
+ cp 方式拷贝备份
+ git、svn管理（小文件）
+ 多端多台机器备份
+ 定时任务来做自动备份，人力还是有覆盖不到的地方,linux下可以使用contrab
	
2 迁移数据库，还是改配置方案比较安全靠谱

3 数据库相关的操作命令
```
//启动|停止数据
service mysql start|stop
//增加数据库用户|修改数据库用户权限
grant all on *.* to new_user@localhost identified by 'new password'; 
//使用数据库备份还原
source ***.sql
//备份数据库
mysqldump -u用户名 -p密码 数据库名 > xxx.sql

```

4 linux下相关命令[总结](https://github.com/moonye6/blog/blob/master/201509/linux_cmd.md)，持续补充

5 从以下方面着手输出相关规范

流程优化：服务器安全规范、操作规范、危险操作总结等
自动化：备份自动化，操作日志自动化


