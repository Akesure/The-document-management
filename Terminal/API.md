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
