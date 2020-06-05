---
title: Ansiable安装使用
categories: 工具
toc: true
tags:
  - Ansiable
  - tools
---
![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1591264607708&di=34d399d4cc0f9bd92ae0644723ce34c7&imgtype=0&src=http%3A%2F%2Fimg3.imgtn.bdimg.com%2Fit%2Fu%3D118371233%2C1275544235%26fm%3D214%26gp%3D0.jpg)

Ansible is Simple IT Automation



## 前言

Ansible 是给 Linux 系统管理员使用的出色自动化工具。它是一种开源配置工具，能让系统管理员可以从一个中心节点（即 Ansible 服务器）管理数百台服务器。将 Ansible 与 Puppet、Chef 和 Salt 等类似工具进行比较时，它是首选的配置工具，因为它不需要任何代理，并且可以工作在 SSH 和 python 上。

[Centos8] https://blog.csdn.net/F8qG7f9YD02Pe/article/details/103379794

## 安装

```shell
yum install epel-release -y #安装epel源
yum install ansible -y
ansible --version #查看ansible版本
 
```


## 配置

```shell
 vim /etc/ansible/hosts
```



## 3.设置SSH无密码登录

```shell
 ssh-keygen -t rsa
 ssh-agent bash
 ssh-add xx
 ssh-copy-id root@xx.xx.xx.xx
```

