---
title: Nginx安装使用
categories: 工具
toc: true
tags:
  - nginx
  - tools
---
# Nginx安装使用
## 安装

docker-compose.yml
```yaml
version: "3"
services:
  nginx:
    container_name: nginx
    restart: always
    image: nginx
    network_mode: "host"
    ports:
      - 80:80
      - 443:443
      
    volumes:
      - /data/services/nginx/conf:/etc/nginx            #映射nginx的配置文件到容器里
      - /data/services/nginx/logs:/wwwlogs
      - /data/static/:/www                    #映射nginx的网页目录到容器里
```

## 配置

nginx.conf

```nginx
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  102400; #设置单个工作进程最大连接数102400
    accept_mutex on; #优化同一时刻只有一个请求而避免多个睡眠进程被唤醒的设置，on为防止被同时唤醒，默认为off，因此nginx刚安装完以后要进行适当的优化。
    multi_accept on; #打开同时接受多个新网络连接请求的功能。
    use epoll; #使用epoll事件驱动，因为epoll的性能相比其他事件驱动要好很多
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    default_type text/html;
    charset UTF-8;
	server_tokens off; #隐藏ngxin版本号
	
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  /var/log/nginx/access.log  main;

    sendfile        on; #配置允许sendfile方式传输文件
    sendfile_max_chunk 512k;   #Nginx工作进程每次调用sendfile()传输的数据最大不能超出这个值，默认值为0表示无限制，可以设置在http/server/location模块中。
   
    tcp_nopush on; #告诉nginx在一个数据包里发送所有头文件，而不一个接一个的发送。
    tcp_nodelay on; #告诉nginx不要缓存数据，而是一段一段的发送--当需要及时发送数据时，就应该给应用设置这个属性，这样发送一小块数据信息时就不能立即得到返回值。

    keepalive_timeout  65;

    proxy_cache_path  /www/cache/ levels=1:2 keys_zone=STATIC:10m inactive=365d  max_size=1g;

    root /www;
    include /etc/nginx/conf.d/*.conf;
}
```

`include       /etc/nginx/gzip;`

```nginx
	gzip  on; # 开启gzip压缩
	gzip_min_length 1k; # 表示当请求的资源超过1k时，才开启压缩
	gzip_buffers 4 16k; 	# 设置压缩所需要的缓冲区大小
	gzip_http_version 1.1; 	# 针对的http版本
	gzip_comp_level 2; 	# 压缩级别，级别越底压缩速度越快文件压缩比越小，反之速度越慢文件压缩比越大
	# 支持压缩的资源类型，对于前后分离的项目而言，注意下json的压缩支持
	gzip_types text/plain application/x-javascript text/css application/xml application/json text/javascript application/x-httpd-php image/jpeg image/gif image/png;
	gzip_vary on; 	# 是否在http header中添加Vary: Accept-Encoding，建议开启
	gzip_disable "MSIE [1-6]\."; 	# 禁用IE 6 gzip
	gzip_proxied any;#nginx 做前端代理时启用该选项，表示无论后端服务器的headers头返回什么信息，都无条件启用压缩

```



**结合frp内网穿透统一代理配置**

```nginx
    # 匹配到指定主机，最后要加$ 不然get参数丢失
    location ~ ^/dev_(\d+)_(\d+)/(.*)$ {
        proxy_pass http://192.168.1.$1:$2/$3?$args;
    }
```



个人开发机外网地址规则:https://angviza.com/dev_${ip}_${port}/

ex： 
内网访问地址为： http://192.168.1.117:8080/logistics/
对外网访问地址 :   https://angviza.com/dev_117_8080/logistics