# vscode打包问题

## 打包过程报错



1. npm list错误



```
npm run package

Error: Command failed: npm list --production --parseable --depth=99999`

peer dep missing: webpack@^2.0.0 || ^3.0.0 || ^4.0.0, required by less-loader@4.1.0
```



1. max buffer问题



```
npm run package

Error: stdout maxBuffer exceeded
```



## 解决方案

1. 选用yarn执行
```shell
vsce package --yarn
```



2. 解决npm list错误



https://github.com/Microsoft/vscode-vsce/issues/246 vscode内的方案



https://github.com/npm/npm/issues/19393 npmlist的错误issue



```
yarn install --ignore-engines --registry=http://registry.npm.alibaba-inc.com
npm run postinstall
npm run package -> // vsce package --yarn 在list时，会选用yarn执行
```



3. max buffer问题



因为依赖的npm包太多了，使用exec执行时，超过了stdout的buffer



```
// vsce out/npm.js
// fn getYarnProductionDependencies

cp.exec('yarn list --json', { cwd: cwd,  maxBuffer: 5000 * 1024 , encoding: 'utf8', env: __assign({}, process.env, { NODE_ENV: 'production' }) 

// 加上了一句，maxBuffer: 5000 * 1024的配置
```



## 为什么执行npm list



为什么执行，以及npm list为什么这么多报错



运行时是没有任何问题的



## npm list报错



`npm list --production --parseable --depth=99999` 这个命令，导致各种奇葩的报错



1. missing



这两个是未带版本号的



```
Error: Command failed: npm list --production --parseable --depth=99999
npm ERR! peer dep missing: webpack@^2.0.0 || ^3.0.0 || ^4.0.0, required by less-loader@4.1.0
npm ERR! missing: mkdirp@, required by kissy-xtemplate@0.4.3
npm ERR! missing: lodash@, required by kissy-xtemplate@0.4.3
npm ERR! missing: commander@, required by kissy-xtemplate@0.4.3
npm ERR! missing: async@, required by wmic@0.0.7
```



1. externous



怎么理解？安装但是为写入 `package.json` ，但是我是全删除了，重新 `tnpm install` 的。



1. 



## yarn打包没有版本号



```
kissy-xtemplate@~0.4.3:
  version "0.4.3"
  resolved "http://registry.npm.alibaba-inc.com/kissy-xtemplate/download/kissy-xtemplate-0.4.3.tgz#f6f682b1531e8a66002eea265458330ad3dc92ab"
  integrity sha1-9vaCsVMeimYALuomVFgzCtPckqs=
  dependencies:
    colors ">=0.6.0-1"
    commander ""
    escodegen "~1.3.2"
    esprima "~1.1.1"
    event-stream "~3.1.1"
    iconv-lite ">=0.1.0"
    js-beautify "~1.4.0"
    lodash ""
    logger-manager "*"
    mkdirp ""
    node.extend "~1.0.10"
```



这是包写的不规范吧？



```
"dependencies": {

    "mkdirp": "",
    "lodash": "",
    "commander": "",

}
```



## yarn和npm打包目录结构



- yarn是树型，非扁平的结构



- npm是扁平的结构，通过软链接的形式



- tnpm呢？又一种新的





## 过程分析



### 一 这个错误从哪里来的？





### 二 这个错误的含义是什么？



### 三 为什么会报错，问题时什么



### 四 如何解决这个问题？





```
vsce package
```



```
vscode:prepublish: tsc -p ./
```



Npm需要深入理解了，这玩意做的太崩溃了



不要着急，各种乱试，注意力集中在发声错误，以及错误的原因上。



- `npm list --production --parseable --depth=99999` 执行这句，为什么会报错呢？这句命令作用是什么？怎么解决？



[Have NPM as a runtime dependency · Issue #246 · Microsoft/vscode-vsce · GitHub](https://github.com/Microsoft/vscode-vsce/issues/246)



https://github.com/npm/npm/issues/19393 这个完全解释了错误的原因`optional dependencies`



https://github.com/Microsoft/vscode-vsce/issues/52 npm list的问题



- vscode打包时报错



```
修改以下两部分
// yarn install --ignore-engines --registry=http://registry.npm.alibaba-inc.com
// vsce package --yarn
```



- extraneous是什么，为什么`npm list`执行上面的报extraneous错误什么意思？

- `optional dependencies` 这是什么？

- 为什么yarn安装的，很多都没有package.json文件呢？



Yarn安装的文件结构怎么那么乖？



- tnpm安装的文件夹结构