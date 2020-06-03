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
    ports:
      - 80:80
      - 443:443
      
    volumes:
      - /data/services/nginx/conf:/etc/nginx            #映射nginx的配置文件到容器里
      - /data/services/nginx/logs:/wwwlogs
      - /data/static/:/www                    #映射nginx的网页目录到容器里
```

```conf

```



个人开发机外网地址规则
https://proxy.idflc.cn/dev_${ip}_${port}_${path}/

如： 内网访问地址为： http://192.168.1.117:8080/logistics/
对应的外网访问地址 :    https://proxy.idflc.cn/dev_117_8080_logistics/