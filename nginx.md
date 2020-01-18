

#### nginx

```
nginx能做什么？
正向代理（代理的是客户端），反向代理（代理的是服务端）负载均衡就是一个网站的内容被部署在若干服务器上，可以把这些机子看成一个集群那Nginx可以将接收到的客户端请求“均匀地”分配到这个集群中所有的服务器上，从而实现服务器压力的平均分配，也叫负载均衡。Nginx适配PC或移动设备（$http_user_agent）
```

##### IO多路复用

​	多个描述符的IO操作都能在一个线程里面并发交替完成

1. select，线性遍历文件描述符列表。但效率低下，最多有1024个线程
2. epoll模型，每当fd就绪，采用系统回调函数将fd放入。效率高，没有1024限制

##### CPU亲和

吧CPU核心和nginx工作进程绑定方式，每个进程固定在一个CPU上执行，减少切换CPU、提高缓存命中率



查看配置文件和目录

```nginx
rpm -ql nginx
```

基本命令：

1. rpm -ql nginx  查看安装位置
2. 启动：nginx，systemctl start nginx.service
3. 停止： nginx -s quit， nignx -s stop， killall nginx
4. 重启： systemctl restart nignx.service，
5. 重载配置文件： nignx -s reload
6. netstat -tlnp 查看开启的端口 ![1571815559073](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1571815559073.png)

![1571816333278](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1571816333278.png)

|              |                          |
| :----------: | :----------------------: |
| server_name  |  监听的主机（ip或域名）  |
|  ~  \.php$   | 正则匹配以.php结尾的路径 |
|  access_log  | 访问成功后的记录日志格式 |
| ~ ^/download |  以/download开头的路径   |
|     2304     |                          |
|              |                          |
|              |                          |

![1571803540468](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1571803540468.png)

#定义Nginx运行的用户和用户组
#user  nobody; 

#nginx进程数，建议设置为等于CPU总核心数。
worker_processes  1; 

#全局错误日志定义类型，[ debug | info | notice | warn | error | crit ]
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#进程文件
#pid        logs/nginx.pid;

#工作模式与连接数上限
events {
    #单个进程最大连接数（最大连接数=连接数*进程数）
    worker_connections  1024;
}

#设定http服务器
http {
    #文件扩展名与文件类型映射表
    include       mime.types;
    #默认文件类型
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
    
    #access_log  logs/access.log  main;
    
    #开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如果图片显示不正常把这个改 成off。
    sendfile        on;
    
    #防止网络阻塞
    #tcp_nopush     on;


    #长连接超时时间，单位是秒
    #keepalive_timeout  0;
    keepalive_timeout  65;
    
    #开启gzip压缩输出
    #gzip  on;
    
    #虚拟主机的配置
    server {
        #监听端口
        listen       80;
    
        #域名可以有多个，用空格隔开
        server_name  localhost;
    
        #默认编码
        #charset utf-8;
    
        #定义本虚拟主机的访问日志
        #access_log  logs/host.access.log  main;
    
        location / {
            root   html;   网站的根目录
            index  index.html index.htm;
        }
    
        #error_page  404              /404.html;
    
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    
        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}
    
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}
    
        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;
    
    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;
    
    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;
    
    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;
    
    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;
    
    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}