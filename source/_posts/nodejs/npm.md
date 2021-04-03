
cnpm

```shell
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

```shell
npm config set registry https://registry.npm.taobao.org
```

```shell
npm adduser --registry http://nexus.domain.cn/repository/npm-hosted/
user: user
password: dev123
email :  dev@domain.com
```

```shell
npm publish
```

### node-sass 修改地址

```sh
SASS_BINARY_SITE=https://npm.taobao.org/mirrors/node-sass/ npm install node-sass
```

Linux 执行命令权限问题，加上参数：

```sh
--unsafe-perm
```

