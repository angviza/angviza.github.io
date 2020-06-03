---
title: Ansiable安装使用
categories: 工具
toc: true
tags:
  - Ansiable
  - tools
---

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

