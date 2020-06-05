---
title: Frp安装使用
categories: 工具
toc: true
tags:
  - frp
  - tools
---
## FRP
frp 是一个可用于内网穿透的高性能的反向代理应用，支持 tcp, udp 协议，为 http 和 https 应用协议提供了额外的能力，且尝试性支持了点对点穿透。

[[GitHub]](https://github.com/fatedier/frp)  [[官方中文文档]](https://github.com/fatedier/frp/blob/master/README_zh.md)  

[frps 完整配置文件](https://github.com/fatedier/frp/blob/master/conf/frps_full.ini)

[frpc 完整配置文件](https://github.com/fatedier/frp/blob/master/conf/frpc_full.ini)

## 安装

docker-compose.yml

https://hub.docker.com/r/snowdreamtech/frps

start frps
```shell
docker run --restart=always --network host -d -v /etc/frp/frps.ini:/etc/frp/frps.ini --name frps snowdreamtech/frps
```
start frpc
```shell
docker run --restart=always --network host -d -v /etc/frp/frpc.ini:/etc/frp/frpc.ini --name frpc snowdreamtech/frpc
```

frps.ini

```ini
[common]
#服务器端端口
bind_port=7000
#客户端连接凭证
privilege_token=foo
#最大连接数
max_pool_count=5

#客户端映射的端口
vhost_http_port = 8080

#服务器看板的访问端口
dashboard_port=7500
#服务器看板账户
dashboard_user=root
dashboard_pwd=rot12

frpc.ini
​```ini
[common]
server_addr = 111.230.248.124 
server_port = 7000
# protocol = kcp
privilege_token=foo

[proxycn]
type = http
local_port = 80
local_ip = 172.17.0.1
custom_domains = *.domain.cn

[ws]
type = tcp
local_port = 80
local_ip = 172.17.0.1
custom_domains = proxy.domain.cn
remote_port = 45635

[proxycom]
type = http
local_port = 80
local_ip = 172.17.0.1
custom_domains = *.domain.com

[ssh]
type = tcp
local_ip = 172.17.0.1
local_port = 22
remote_port = 14222
```