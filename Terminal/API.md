# linux
FileZilla xshell Workbench

查看80端口是否占用:
```
sudo lsof -i:80
```


# node

后台开启node服务器:
```
nohup node bin/www &
```

查看错误日志:
```
cat nohup.out
```

移除错误日志:
```
rm nohup.out
```

持续输出日志-f最后200行:
```
tail -200f nohup.out  
```

查看当前运行的，包含"node"的进程：
```
ps -ef | grep node
```

杀死进程:
```
kill <pid>
```

以80端口运行Node:
```
PORT=80 nohup node bin/www &
```

查看80端口是否被占用:
```
netstat -anp | grep :80
```


# Mysql
链接Mysql databases:  
```
mysql -h 10.10.100.2 -uweixin -p weixin
```

导入table:
```
mysql -h 10.10.100.2 -P 3306 -uweixin -p weixin < /apps/wx-server/backup/qingsu_wx_tbl_question.sql
```


# pm2多进程
所有进程重新启动:
```
pm2 restart all
```

查看进程日志:
```
pm2 logs
```
