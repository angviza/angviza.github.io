---
title: typescript 提示
---





【转】https://github.com/coppyC/blog/issues/6

## 前言

为什么vue中有的文件中的函数不能提示，转到定义提示没有任何定义，而有的文件又正常工作

- 从第三方库增强提示 （例如 `this.$axios`）

- 从已写的代码增强提示 （例如 `this.$store`）

## 新手教程

  先来个官网教程小改版本。

  在根目录新建 `global.d.ts` 文件，然后加入下面代码

  ```
  import Vue from 'vue'
  
  declare module 'vue/types/vue' {
    // 声明原型链属性: `this.x`
    interface Vue {
      xx: string
    }
    // 声明全局属性: `Vue.xx`
    interface VueConstructor {
      oo: string
    }
  }
  
  // 声明选项属性: `export default { xxx: '' }`
  declare module 'vue/types/options' {
    interface ComponentOptions<V extends Vue> {
      www?: string
    }
  }
  ```

对的，你没有看错，我没有在 data 声明 xx，提示里面却有 xx，
`Vue.oo` 也有提示，Vue选项的 `ww` 也能得到提示。

（如果没有提示，需要重启一下 vscode，用js的话可能还需要添加 jsconfig.json 文件）

在 vscode 中增强 Vue 的提示无非就是这三种途径。

## [番外] typescript 声明文件了解一下

typescript 作为 vscode 的亲儿子，只要掌握之后，在 vscode 中写 js、ts 确实是能提升幸福度。

而本文也不可能让大家马上精通 typescript，只能说说上面的代码。

其实通过图片效果，大家也都了解得差不多了，而且我也打了注释，应该都能看懂，需要注意的可能就是，声明vue选项(即`interface ComponentOptions`)的时候，要记得加上 `?` 来表示这是一个可选属性，不然会变成必填属性，然后提示可能会泡汤。

然后注释使用 `/** */` 这种注释，不要使用这种 `//` ，这样的话提示会显示注释

然后就是typescript 剩下的就要自己去学了，我只能带到这里了。

如果是解决 mpvue 提示的话，我这里写了一个包放在 github 上开源了。

可以参考一下。 https://github.com/coppyC/types/tree/mpvue

也欢迎安装使用 `yarn add -D coppyc/types#mpvue`

效果还是很 nice 的

# 增强 `this.$store 提示`

但是你上面那样加一些无意义的提示是没用的，我们先来解决最常见的一个问题`this.$store`提示

`$store` 是 访问 Vuex 的一个常用途径，虽然我们可以通过导入 store 文件来引用，而不是`$store` 来引用来获得提示，但这似乎不是什么最佳选择，毕竟官方给了这个渠道，不用感觉就亏了。

好，现在打个比方，我们有这么个 store.js 文件

```
import Vue from 'vue'
import Vuex, { Store } from 'vuex'

Vue.use(Vuex)

export default  new Store({
  state: {
    user: {},     //用户
    sign: {},     //签到
    wallet: {}    //积分
  }
})
```

然后在 global.d.ts 中这么搞

```
import Vue from 'vue'
import 'vuex/types/vue'
import store from './src/store'

declare module 'vue/types/vue' {
  interface Vue {
    $store: typeof store
  }
}
```

注意是 `typeof store` 不是 `store`

看一下效果，打开，还是和原来一样，重启，还是一样，没变化

原因是 vuex 中已经定义过 $store 了，而且还不能覆盖，官网是这么说的

> 接口的非函数的成员应该是唯一的。如果它们不是唯一的，那么它们必须是相同的类型。如果两个接口中同时声明了同名的非函数成员且它们的类型不同，则编译器会报错。

这个就很难搞了，我目前只有一个临时解决方案，按住 control (mac:command)，然后点击

[![Screen Shot 2019-06-22 at 4 41 29 PM](https://user-images.githubusercontent.com/25004510/59961624-c9d77e80-950c-11e9-9fc5-f0db9468d83c.png)](https://user-images.githubusercontent.com/25004510/59961624-c9d77e80-950c-11e9-9fc5-f0db9468d83c.png)

然后跳转到 vuex 的 vue.d.ts 声明文件 把 16 行 （`$store: Store<any>;`）这一行注释掉

重启一下 vscode，然后看下效果

[![image](https://user-images.githubusercontent.com/25004510/59961709-f5a73400-950d-11e9-9b2f-30fa9076abea.png)](https://user-images.githubusercontent.com/25004510/59961709-f5a73400-950d-11e9-9b2f-30fa9076abea.png)

还不错，就是有个缺点，npm 的包重新装的话，我们刚才在 vuex 的声明文件改的东西就会被改回去，这个也是没有办法，毕竟是临时工方案，如果你有更好的方案，可以告诉我

或者你用 ts 而不是 js 的话，就可以用 as 大法来断言，不用这么折腾。

# 从第三方库中 增强提示

上面说的都是自己写的代码，如果是别人写的呢? 从 npm 安装的呢？别急。

不知什么时候，`this.$http` 这种网络请求方式就开始流行起来（可能是 angular 中开始的吧）

然后 vue 本身是没有提供 ajax 的封闭的，比较流行的就是 axios，但是每次都 import 进来的话，还是比较麻烦的，很多人喜欢通过 `Vue.prototype.$http = axios` 来修改原型链，然后用`this.$http`这样一个便捷方式来访问，这样一个人还好，不同人维护的话，在原型链中挂了各种这样不同的东西，还是很难维护的，想一下，你在原型上挂点什么，下一个人不知道，然后也挂点什么，然后可能冲突出 bug。这也一直被 react 用户吐嘈，还是 vue 做大型项目难以维护的一个地方。

但是现在，我们可以用维护性十分强的 typescript 声明文件来避免这个问题，增加提示

就用 axios 来做例子吧

先挂上原型中 `Vue.prototype.$http = axios`

然后在 global.d.ts 中写点什么。

```
import Vue from 'vue'
import axios from 'axios'

declare module 'vue/types/vue' {
  interface Vue {
    $http: typeof axios
  }
}
```

嗯 ，就是这么简单，记得是 `typeof axios` ，不是 `axios`

当然了，这个库是要支持 typescript 的才行

看一下效果

[![image](https://user-images.githubusercontent.com/25004510/59961497-1de16380-950b-11e9-8a73-9e389fe79d3a.png)](https://user-images.githubusercontent.com/25004510/59961497-1de16380-950b-11e9-8a73-9e389fe79d3a.png)

嗯，还不错

# 完

差不多就这样了，有缘再见吧。