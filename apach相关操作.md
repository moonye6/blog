apache安装目录为/usr/local/apache2

apache启动命令：
推荐/usr/local/apache2/bin/apachectl start apache启动

apache停止命令
/usr/local/apache2/bin/apachectl stop   停止

apache重新启动命令：
/usr/local/apache2/bin/apachectl restart 重启

要在重启 Apache 服务器时不中断当前的连接，则应运行：

/usr/local/sbin/apachectl graceful

如果apache安装成为linux的服务的话，可以用以下命令操作：

service httpd start 启动

service httpd restart 重新启动

service httpd stop 停止服务
