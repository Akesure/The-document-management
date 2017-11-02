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

# nginx
- 403 Forbidden 时候的权限配置
sudo chmod -R 777 /Users/xuyang/Desktop/The-document-management

/usr/local/etc/nginx/nginx.conf 里面还要设置 user  root owner;

在此记住三个目录；/usr/local/cellar           /usr/local/etc/nginx           /usr/local/var

          /usr/local/etc/nginx/nginx.conf （配置文件路径）

          /usr/local/var/www （服务器默认路径）

          /usr/local/Cellar/nginx/1.12.0 （安装路径）（我安装的是1.12.0，具体参照自己安装的版本）

配置文件目录   /usr/local/etc/nginx/nginx.conf

## nginx.conf配置:
```
server{
    listen [你要监听的端口号];
    server_name [你要监听的域名/IP];
    index index.html index.htm index.php;
    root /usr/local/webserver/nginx/html [站点目录];
    location / {
        proxy_pass [代理的目标地址];

        add_header Access-Control-Allow-Origin * [允许所有http跨域请求访问];
     }
}
```
conf文件中有示例的server，可以添加多个server。

启动命令:
```
sudo nginx
```

快速关闭Nginx，可能不保存相关信息，并迅速终止web服务:
```
sudo nginx -s stop
```

平稳关闭Nginx，保存相关信息，有安排的结束web服务:
```
sudo nginx -s quit
```

重新加载配置文件命令:
```
sudo nginx -s reload   (当配置文件修改后，可执行此命令)
```

重新打开日志文件命令:
```
sudo nginx -s reopen
```

为 Nginx 指定一个配置文件，来代替缺省的:
```
nginx -c filename   
```

不运行，而仅仅测试配置文件。
nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件:
```
nginx -t
```

显示 nginx 的版本
```
nginx -v
```

显示 nginx 的版本，编译器版本和配置参数
```
nginx -V
```
