## 安装ES

https://www.elastic.co/guide/en/elasticsearch/reference/7.9/docker.html



```yml
version: '2.2'
services:
  es01:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.9.3
    container_name: es01
    environment:
      - discovery.type=single-node
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - ./data:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
```

```shell
$  apt-get update &&    apt-get -y install
$  #apt-get install python python-pip -y (Python3)
```

安装python3

https://blog.csdn.net/bz0446/article/details/105500585



## 安装fdw

[GitHub](https://github.com/matthewfranglen/postgres-elasticsearch-fdw)

```sql
select pg_terminate_backend(pid) from pg_stat_activity where DATNAME = 'gis';
```

