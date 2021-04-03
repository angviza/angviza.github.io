minIO





[Docker 安装 minio](https://www.cnblogs.com/eason-d/p/12711041.html)

参考地址: https://www.jianshu.com/p/52dbc679094a

 

注意的地方:

echo 命令设置 ACCESS_KEY 和 SECRET_KEY

```sh
echo "admin" | docker secret create access_key -
echo "12345678" | docker secret create secret_key 
-此处secret_key长度至少8位, 若已经设置密码不足8位，可通过命令
docker secret --help
查看相关命令中
docker secret rm secret_key
来移除 若执行出现 Error response from daemon: This node is not a swarm manager. Use "docker swarm init" or "docker swarm join" to connect this node to swarm and try again.错误, 需执行
docker swarm init
```

 

启动命令

```sh
docker run -itd -p 9000:9000 --name minio1   -e "MINIO_ACCESS_KEY=admin"   -e "MINIO_SECRET_KEY=admin"   -v /mnt/data:/data   -v /mnt/config:/root/.minio   minio/minio server /data
```

 

打开网址: http://x.x.x.x:9000/ 发现启动失败, 但是改回原来的用户密钥 正确 估计是用户名有问题， 最后使用默认的方式启动,

```sh
docker run -itd -p 9000:9000 --name minio1   -e "MINIO_ACCESS_KEY=AKIAIOSFODNN7EXAMPLE"   -e "MINIO_SECRET_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"   -v /mnt/data:/data   -v /mnt/config:/root/.minio   minio/minio server /data
```

docker-compose.yml
```yml
version: '2'

services:
  minio:
    image: minio/minio
    command: server /data
    environment:
      - MINIO_ACCESS_KEY=admin
      - MINIO_SECRET_KEY=admin123456
    ports:
      - '9000:9000'
    volumes:
      - 'minio_data:/data'
      - 'config:/root/.minio'

volumes:
  minio_data:
    driver: local
```



```sh
docker run -p 9000:9000 --name minio \
  -v /mnt/data1:/data1 \
  -v /mnt/data2:/data2 \
  -v /mnt/data3:/data3 \
  -v /mnt/data4:/data4 \
  -v /mnt/data5:/data5 \
  -v /mnt/data6:/data6 \
  -v /mnt/data7:/data7 \
  -v /mnt/data8:/data8 \
  minio/minio server /data1 /data2 /data3 /data4 /data5 /data6 /data7 /data8
```