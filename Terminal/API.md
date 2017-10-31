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

## nginx.conf http反向代理配置:
```
#运行用户
#user somebody;

#启动进程,通常设置成和cpu的数量相等
worker_processes  1;

#全局错误日志
error_log  D:/Tools/nginx-1.10.1/logs/error.log;
error_log  D:/Tools/nginx-1.10.1/logs/notice.log  notice;
error_log  D:/Tools/nginx-1.10.1/logs/info.log  info;

#PID文件，记录当前启动的nginx的进程ID
pid        D:/Tools/nginx-1.10.1/logs/nginx.pid;

#工作模式及连接数上限
events {
    worker_connections 1024;    #单个后台worker process进程的最大并发链接数
}

#设定http服务器，利用它的反向代理功能提供负载均衡支持
http {
    #设定mime类型(邮件支持类型),类型由mime.types文件定义
    include       D:/Tools/nginx-1.10.1/conf/mime.types;
    default_type  application/octet-stream;

    #设定日志
    log_format  main  '[$remote_addr] - [$remote_user] [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log    D:/Tools/nginx-1.10.1/logs/access.log main;
    rewrite_log     on;

    #sendfile 指令指定 nginx 是否调用 sendfile 函数（zero copy 方式）来输出文件，对于普通应用，
    #必须设为 on,如果用来进行下载等应用磁盘IO重负载应用，可设置为 off，以平衡磁盘与网络I/O处理速度，降低系统的uptime.
    sendfile        on;
    #tcp_nopush     on;

    #连接超时时间
    keepalive_timeout  120;
    tcp_nodelay        on;

    #gzip压缩开关
    #gzip  on;

    #设定实际的服务器列表
    upstream zp_server1{
        server 127.0.0.1:8089;
    }

    #HTTP服务器
    server {
        #监听80端口，80端口是知名端口号，用于HTTP协议
        listen       80;

        #定义使用www.xx.com访问
        server_name  www.helloworld.com;

        #首页
        index index.html

        #指向webapp的目录
        root D:\01_Workspace\Project\github\zp\SpringNotes\spring-security\spring-shiro\src\main\webapp;

        #编码格式
        charset utf-8;

        #代理配置参数
        proxy_connect_timeout 180;
        proxy_send_timeout 180;
        proxy_read_timeout 180;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarder-For $remote_addr;

        #反向代理的路径（和upstream绑定），location 后面设置映射的路径
        location / {
            proxy_pass http://zp_server1;
        }

        #静态文件，nginx自己处理
        location ~ ^/(images|javascript|js|css|flash|media|static)/ {
            root D:\01_Workspace\Project\github\zp\SpringNotes\spring-security\spring-shiro\src\main\webapp\views;
            #过期30天，静态文件不怎么更新，过期可以设大一点，如果频繁更新，则可以设置得小一点。
            expires 30d;
        }

        #设定查看Nginx状态的地址
        location /NginxStatus {
            stub_status           on;
            access_log            on;
            auth_basic            "NginxStatus";
            auth_basic_user_file  conf/htpasswd;
        }

        #禁止访问 .htxxx 文件
        location ~ /\.ht {
            deny all;
        }

        #错误处理页面（可选择性配置）
        #error_page   404              /404.html;
        #error_page   500 502 503 504  /50x.html;
        #location = /50x.html {
        #    root   html;
        #}
    }
}
```

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

不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件:
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
