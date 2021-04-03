# Superset

https://blog.csdn.net/whq12789/article/details/89640453

准备环境docker

然后执行命令

docker pull amancevice/superset

docker run --detach --name superset -p 8088:8088 -v ./superset:/usr/local/lib/python3.6/site-packages/superset/ amancevice/superset

docker exec -it superset  superset db upgrade

docker exec -it superset superset-init

回车回车

输入密码

——安装完成

 

输入IP:8088访问

[http://192.168.91.146:8088](http://192.168.91.146:8088/)

用户名admin

密码是刚才的密码

 

 

加数据源

sources——databases

右上角加号：add a new record

Database中填写个名称

SQLAlchemy URI中填写

postgresql://scott:tiger@localhost/mydatabase

 

 

加表

sources——tables

右上角加号：add a new record

选择数据库

填写schema和table name即可

 

点击表进入进行统计处理

点击Datasource & Chart Type——Visualization Type 下的Table字样可以选择Chart形式

 

 

需要改源码的话，到docker中的

/usr/local/lib/python3.6/site-packages/superset/

目录



汉化

https://blog.csdn.net/u014589856/article/details/103582824