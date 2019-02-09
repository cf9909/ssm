Ubuntu查看端口使用情况，使用netstat命令：


查看已经连接的服务端口（ESTABLISHED）


netstat -a


查看所有的服务端口（LISTEN，ESTABLISHED）


netstat -ap


查看指定端口，可以结合grep命令：


netstat -ap | grep 8080


若要关闭使用这个端口的程序，使用kill + 对应的pid


kill -9 PID号

ls -a
