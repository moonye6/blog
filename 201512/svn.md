
### 切换账号

1.临时切换，在命令下强制加上 --username 和--password选项，例如：svn up --username zhangsan --password 123456
2.永久切换
删除目录 ~/.subversion/auth/  下的所有文件。下一次操作svn时会提示你重新输入用户名和密码的。换成你想用的就可以了。然后系统默认会记录下来的。
