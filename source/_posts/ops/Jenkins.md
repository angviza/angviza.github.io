---
title: Jenkins 安装使用
categories: 工具
toc: true
tags:
  - jenkins
  - ops
---


## 安装

```shell
wget -O /etc/yum.repos.d/jenkins.repo https:*//pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
yum install jenkins -y
systemctl start jenkins
systemctl enable jenkins
```

**配置文件位置 /etc/sysconfig/jenkins**

```shell
cat /root/.jenkins/secrets/initialAdminPassword #查看密码
```





# 配置

## Groovy postbuild 插件

### 使用 unirest

到jenkins 工作目录，plugins/随便找一个包，如jdk-tools/WEB-INF/lib/放入依赖jar

```groovy
def result = manager.getResult()
def jobName = manager.getEnvVariable("JOB_NAME")
def prod = manager.getEnvVariable("PROD")


        LinkedHashMap textcard = new LinkedHashMap()
        textcard.put("title", "构建通知")

        textcard.put("description", "<div class='gray'>" + jobName + "</div> <br/>" +
                "<div class='normal'> 渠　　道：" + prod + "</div> " +
                "<div class='highlight'> 结　　果：" + result + "</div>")
        textcard.put("url", "https://ops.idflc.cn/ci/job/logistics-android/")
        textcard.put("btntxt", "前往")

        LinkedHashMap body = new LinkedHashMap()

        body.put("msgtype", "textcard")
        body.put("enable_id_trans", 0)
        body.put("enable_duplicate_check", 0)
        body.put("duplicate_check_interval", 1800)
        body.put("textcard", textcard)

        Object res = kong.unirest.Unirest.post("http://push.domain.cn/wechat/push/cp001").
                header("token", "test").
                header("accept", "*/*").header("content-type", "application/json").body(body).
                asJson().getBody().
                getObject()
manager.listener.logger.println(res)
```





