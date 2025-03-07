<p align="center">
  <a href="https://micro-zoe.github.io/micro-app/">
    <img src="https://zeroing.jd.com/micro-app/media/logo.png" alt="logo" width="200"/>
  </a>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@micro-zoe/micro-app">
    <img src="https://img.shields.io/npm/v/@micro-zoe/micro-app.svg" alt="version"/>
  </a>
  <a href="https://www.npmjs.com/package/@micro-zoe/micro-app">
    <img src="https://img.shields.io/npm/dt/@micro-zoe/micro-app.svg" alt="downloads"/>
  </a>
  <a href="https://github.com/micro-zoe/micro-app/blob/master/LICENSE">
    <img src="https://img.shields.io/npm/l/@micro-zoe/micro-app.svg" alt="license"/>
  </a>
  <a href="https://gitter.im/microzoe/micro-app">
    <img src="https://badges.gitter.im/microzoe/micro-app.svg" alt="gitter">
  </a>
  <a href="https://travis-ci.com/github/micro-zoe/micro-app">
    <img src="https://api.travis-ci.com/micro-zoe/micro-app.svg?branch=master" alt="travis"/>
  </a>
  <a href="https://coveralls.io/github/micro-zoe/micro-app?branch=master">
    <img src="https://coveralls.io/repos/github/micro-zoe/micro-app/badge.svg?branch=master" alt="coveralls"/>
  </a>
</p>

[English](https://github.com/micro-zoe/micro-app)｜简体中文｜[官网文档](https://micro-zoe.github.io/micro-app/)｜[讨论组](https://github.com/micro-zoe/micro-app/discussions)｜[Gitter群聊](https://gitter.im/microzoe/micro-app)

# 📖简介
micro-app是京东零售推出的一款微前端框架，它基于类WebComponent进行渲染，从组件化的思维实现微前端，旨在降低上手难度、提升工作效率。它是目前接入微前端成本最低的框架，并且提供了JS沙箱、样式隔离、元素隔离、预加载、资源地址补全、插件系统、数据通信等一系列完善的功能。

micro-app与技术栈无关，也不和业务绑定，可以用于任何前端框架。

# 🔧开始使用
微前端分为基座应用和子应用，我们分别列出基座应用和子应用需要进行的修改，具体介绍micro-app的使用方式。

## 基座应用
> 基座应用以vue框架为例

1、安装依赖
```bash
yarn add @micro-zoe/micro-app
```

2、在入口处引入依赖
```js
// main.js
import microApp from '@micro-zoe/micro-app'

microApp.start()
```

3、分配一个路由给子应用
```js
// router.js
import Vue from 'vue'
import VueRouter from 'vue-router' // vue-router@3.x
import MyPage from './my-page.vue'

Vue.use(VueRouter)

const routes = [
  {
    // 👇 非严格匹配，/my-page/xxx 都将匹配到 MyPage 页面
    path: '/my-page/*', 
    name: 'my-page',
    component: MyPage,
  },
]

export default routes
```

4、在`MyPage`页面中嵌入微前端应用
```html
<!-- my-page.vue -->
<template>
  <div>
    <h1>子应用</h1>
    <!-- 👇 name为应用名称，url为html地址 -->
    <micro-app name='app1' url='http://localhost:3000/' baseroute='/my-page'></micro-app>
  </div>
</template>
```

> url和子应用路由的关系请查看[路由一章](https://micro-zoe.github.io/micro-app/docs.html#/zh-cn/route)

## 子应用
> 子应用以react框架为例

1、设置基础路由(如果基座应用是history路由，子应用是hash路由，不需要设置基础路由，这一步可以省略)

```js
// router.js
import { BrowserRouter, Switch, Route } from 'react-router-dom'

export default function AppRoute () {
  return (
    // 👇 设置基础路由，子应用可以通过window.__MICRO_APP_BASE_ROUTE__获取基座应用下发的baseroute
    <BrowserRouter basename={window.__MICRO_APP_BASE_ROUTE__ || '/'}>
      <Switch>
        ...
      </Switch>
    </BrowserRouter>
  )
}
```

2、在webpack-dev-server的headers中设置跨域支持。
```js
devServer: {
  headers: {
    'Access-Control-Allow-Origin': '*',
  },
},
```

以上微前端就可以正常渲染，实现了在vue基座应用中嵌入react子应用，效果如下：

<img src="https://img12.360buyimg.com/imagetools/jfs/t1/196940/34/1541/38365/610a14fcE46c21374/c321b9f8fa50a8fc.png" alt="result" width='900'/>

上面列出了react和vue框架的使用方式，它们是可以自由组合的，比如基座应用是react，子应用是vue，或者基座应用是vue，子应用是react，或者基座应用和子应用都是react、vue。 micro-app对前端框架没有限制，任何框架都可以作为基座应用嵌入任何类型框架的子应用。

更多详细配置可以查看[官网文档](https://micro-zoe.github.io/micro-app/docs.html#/zh-cn/start)

# 🤝 参与共建
如果您对这个项目感兴趣，欢迎提 pull request，也欢迎 "Star" 支持一下 ^_^

### 本地运行
1、克隆项目
```
git clone https://github.com/micro-zoe/micro-app.git
```

2、安装依赖
```
yarn bootstrap
```

3、运行项目
```
yarn start # 访问 http://localhost:3000
```

更多命令请查看[DEVELP](https://github.com/micro-zoe/micro-app/blob/master/DEVELOP.zh-cn.md)

# FAQ
[问题汇总](https://micro-zoe.github.io/micro-app/docs.html#/zh-cn/questions)
<details>

  <summary>micro-app的优势在哪里？</summary>
  上手简单、侵入性低，只需改动少量的代码即可接入微前端，同时提供丰富的功能。

  具体细节请参考文章：[micro-app介绍](https://github.com/micro-zoe/micro-app/issues/8)

</details>
<details>
  <summary>兼容性如何？</summary>
  micro-app依赖于CustomElements和Proxy两个较新的API。

  对于不支持CustomElements的浏览器，可以通过引入polyfill进行兼容，详情可参考：[webcomponents/polyfills](https://github.com/webcomponents/polyfills/tree/master/packages/custom-elements)。

  但是Proxy暂时没有做兼容，所以对于不支持Proxy的浏览器无法运行micro-app。

  浏览器兼容性可以查看：[Can I Use](https://caniuse.com/?search=Proxy)

  总体如下：
  - PC端：除了IE浏览器，其它浏览器基本兼容。
  - 移动端：ios10+、android5+
</details>

<details>
  <summary>子应用一定要支持跨域吗？</summary>
  是的！

  如果是开发环境，可以在webpack-dev-server中设置headers支持跨域。
  ```js
  devServer: {
    headers: {
      'Access-Control-Allow-Origin': '*',
    },
  }
  ```

  如果是线上环境，可以通过[配置nginx](https://segmentfault.com/a/1190000012550346)支持跨域。
</details>

<details>
  <summary>支持vite吗?</summary>
  
  支持，详情请查看[适配vite](https://micro-zoe.github.io/micro-app/docs.html#/zh-cn/advanced?id=_2%e3%80%81%e9%80%82%e9%85%8dvite)
</details>

# 贡献者们
<a href="https://github.com/micro-zoe/micro-app/graphs/contributors"><img src="./.github/contributors.svg" /></a>
<!-- opencollective is inaccurate  -->
<!-- <a href="https://github.com/micro-zoe/micro-app/graphs/contributors"><img src="https://opencollective.com/micro-app/contributors.svg?width=890&button=false" /></a> -->

# License
[MIT License](https://github.com/micro-zoe/micro-app/blob/master/LICENSE)
