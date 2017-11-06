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

/usr/local/Cellar/nginx/1.12.0 建立一个文件logs好让日志进行打印;

sudo nginx -s stop

- nginx 安装之后的目录地址

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

# supervisor 后台nodeJs热加载
安装
```
sudo npm install -g supervisor
```

运行
```
//cd 项目目录
cd /usr/Desktop/xxxx/

//node启动脚本文件
supervisor app.js
```

# 部署阿里云
远程链接
```
ssh root@123.57.143.189
```

ubuntu 的linux管理器
```
apt-get update
```

安装nodeJs
[https://nodejs.org/en/download/package-manager](https://nodejs.org/en/download/package-manager)
```
apt-get install -y curl

curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -

apt-get install nodejs
```

安装mysql
```
apt-get install mysql-server

apt-get install mysql-client

apt-get install libmysqlclient-dev
```

修改mysql配置允许远程链接
```
cd /etc/mysql/
ls
vi my.cnf
```

修改bind-address
```
# bind-address    = 127.0.0.1
```

重新启动mysql
```
service mysql restart
```

链接mysql授权
```
grant all privileges on DBName.* to admin@localhost identified by 'password' with grant option
grant all privileges on *.* to "root"@"%" identified by '123456' with grant option
```

刷新一下权限
```
flush privileges
```

mysql数据库可视化工具： Navicat

**云服务部署项目**
递归创建目录
```
mkdir -p /data/work_run

cd /data/work_run
```

安装git
```
apt-get install git
```

然后clone项目仓库
```
git clone https://github.com/Akesure/xxxxxx

npm install

npm install -g bower

bower install

bower install --allow-root
```

启动云服务项目并且访问
```
npm start
```

公网ip:服务port
```
123.57.143.189:3000
```
