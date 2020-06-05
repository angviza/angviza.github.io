---
title: Python 安装使用
categories: 工具
toc: true
tags:
  - jenkins
  - ops
---
# Python

**前言**

Python是世界上最受欢迎的编程语言之一，凭借其简单易学的语法，Python是初学者和经验丰富的开发人员的流行选择。

当前有两个Python版本正在积极开发中，尽管Python 2得到了良好的支持和活跃，但Python 3被应用得更多，也更为高效，参考[Python 3.8.0发布，已提供Python-3.8.0.tgz下载，附更新功能](https://ywnz.com/linuxrj/5990.html)。

默认情况下，RHEL/CentOS 8没有未版本控制的全系统python命令，以避免将用户锁定到特定版本的Python。相反，它使用户可以选择安装、配置和运行特定的Python版本，系统工具（例如yum）使用内部Python二进制文件和库。

本文将引导你逐步在CentOS 8操作系统上安装Python 3和Python 2。

 

**在CentOS 8上安装Python 3**

要在CentOS 8上安装Python 3，请以root或sudo用户身份在终端中运行以下命令：
```shell
$ sudo dnf install python3
```
要验证安装，请输入以下命令检查Python版本：
```shell
$ python3 --version
```
参考：[在Linux系统中检查Python版本（Python Version）的方法](https://ywnz.com/linuxjc/5833.html)。

在选写本文时，CentOS存储库中可用的Python 3的最新版本是“3.6.x”：

Python 3.6.8

注：该命令还会安装pip。

要运行Python，你需要明确输入python3并运行pip输入pip3。

你应该始终喜欢使用yum或dnf安装发行版提供的python模块，因为它们受支持并经过测试可以在CentOS 8上正常工作，仅在虚拟环境中使用pip，Python虚拟环境允许你将Python模块安装在特定项目的隔离位置，而不必全局安装，这样，你不必担心会影响其他Python项目。

Python 3模块软件包的名称以“python3”为前缀，例如，要安装paramiko模块，应运行：
```shell
$ sudo dnf install python3-paramiko
```


**在CentOS 8上安装Python 2**

Python 2软件包也包含在默认的CentOS 8存储库中。

要安装Python 2，请输入以下命令：
```shell
$ sudo dnf install python2
```
通过键入以下命令验证安装：
```shell
$ python2 --version
```
输出应如下所示内容：

Python 2.7.15

要执行Python 2，请输入python2，然后运行pip并输入pip2。

 

**设置默认Python版本（Unversioned Python命令）**

如果你有希望在系统路径中找到python命令的应用程序，则需要创建unversioned python命令并设置默认版本。

要将Python 3设置为系统范围内的非版本化python命令，请使用Alternatives实用程序：
```shell
$ sudo alternatives --set python /usr/bin/python3
```
对于Python 2，输入：
```shell
$ sudo alternatives --set python /usr/bin/python2
```
Alternatives命令创建一个指向指定python版本的symlink python。

在终端中键入python --version，你应该看到默认的Python版本。

要更改默认版本，请使用上面的命令之一，如果要删除未版本控制的python命令，请输入：
```shell
$ sudo alternatives --auto python
```


**结论**

在CentOS 8操作系统中，默认情况下未安装Python，如果有需要，按照以上方法操作即可。

如果做一个总结的话，那就是：要安装Python 3，请键入dnf install python3，如果是安装python 2，请键入dnf install python2。

https://ywnz.com/linuxjc/6033.html





