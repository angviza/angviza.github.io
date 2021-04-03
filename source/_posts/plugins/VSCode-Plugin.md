---
title: VSCode 插件开发
categories: 插件开发
toc: true
tags:
  - VSCode 
  - plugin
---







## VSCode自定义插件开发

### VSCode插件开发有什么用

每个公司、项目组、各人都有自己特殊的定制化、可公用的需求，vscode的已有插件显然不能满足我们各种各样的要求，所以开发一个适用于我们自己的定制化插件，无疑能大大提高我们的开发效率以及团队的语法规范

### VSCode插件开发、发布主要流程

1. 插件开发前的准备：vscode、nodejs、vscode插件生产工具、git、微软账号
2. 插件开发：插件构思、官方文档查阅
3. 插件发布：打包、上传、插件市场操作
4. 插件维护：更新迭代后打包、上传、插件市场操作

### 插件开发前的准备：

vscode、nodejs、git、微软账号
vscode插件生产工具：官方推荐使用Yeoman 和 VS Code Extension Generator。用如下命令安装：

```shell
npm install -g yo generator-code
```

#### 使用生产工具初始化代码

```shell
yo code
```

- 目录如下

```
├── .vscode      // 资源配置文件
├── CHANGELOG.md // 更改记录文件，会展示到vscode插件市场
├── extension.js // 拓展源代码文件
├── jsconfig.json
├── package.json // 资源配置文件
├── README.md    // 插件介绍文件，会展示到vscode插件市场
├── test         // 测试文件
└── vsc-extension-quickstart.md
```

在js拓展项目下，最重要的就是extension.js和package.json。

### 开发的插件功能

1. 初始化项目的组件模块
2. 初始化项目里面能共用的模块



## 打包

[官方文档](https://code.visualstudio.com/api/working-with-extensions/publishing-extension)   [Bugs](https://www.yuque.com/chengkuan/web/yr5r7u?language=en-us)

### Installation

Make sure you have [Node.js](https://nodejs.org/) installed. Then run:

```
npm install -g vsce
```

### Usage

You can use `vsce` to easily [package](https://code.visualstudio.com/api/working-with-extensions/publishing-extension#packaging-extensions) and [publish](https://code.visualstudio.com/api/working-with-extensions/publishing-extension#publishing-extensions) your extensions:

```
$ cd myExtension
$ vsce package
# myExtension.vsix generated
$ vsce publish
# <publisherID>.myExtension published to VS Code MarketPlace
```