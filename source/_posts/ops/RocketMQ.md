---
title: RocketMQ
categories: MQ
toc: true
tags:
  - MQ
  - services
---

生产者发送消息：
export NAMESRV_ADDR=127.0.0.1:9876
sh tools.sh org.apache.rocketmq.example.quickstart.Producer

消费者接受消息：
sh tools.sh org.apache.rocketmq.example.quickstart.Consumer
————————————————
版权声明：本文为CSDN博主「森雨海巷」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_33767353/java/article/details/103082730