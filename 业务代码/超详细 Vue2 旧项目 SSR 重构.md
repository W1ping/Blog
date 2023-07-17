# Vue2 旧项目 SSR 重构
业务需要，需要把Vue2 旧项目从以前的静态项目重构成 SSR 项目，不想看过程的可以直接拉到最后，源码贴在最后面了，还有用vite 搭建的Vue3/Vue3 SSR / Nuxt3 项目模板。

本次重构使用 express 做为服务端

## 前置准备工作

### 安装库包 

npm i vue-server-renderer  express -S

注意点： vue-server-renderer 版本必须和 vue 版本匹配（和 vue 版本保持一致）node-sass 替换成 sass ，sass-loader 不要用8以上的版本，webpack 不要升到 5 ，webpack5 对 Vue2 SSR 兼容不行

如果只做 SSR 改造使用 vue-server-renderer express 就足够了，下面的库包更多的工作是对打包进行优化，讲究一个重构打包优化一步到位👍。

其它库包
```
npm i body-parser@1.15.2 cookie-parser@1.4.3 es6-promise@4.1.1 express-fileupload@1.3.1 lru-cache@4.1.5 memory-fs@0.4.1 route-cache@0.4.3 serve-favicon@2.4.5 vuex@3.1.3 vuex-router-sync@5.0.0 -S
```

```
npm i chokidar@3.4.0 css-loader@3.6.0  friendly-errors-webpack-plugin@1.6.1 optimize-css-assets-webpack-plugin@6.0.1 postcss@8.2.2 postcss-loader@3.0.0 sass@1.3.0 sass-loader@7.3.1 uglifyjs-webpack-plugin@2.2.0 vue-loader@15.3.0 vue-style-loader@4.1.2   vue-template-loader@4.1.2 webpack@4.8.1 webpack-cli@3.3.7  webpack-dev-middleware@1.12.0 webpack-dev-server@3.1.4 webpack-hot-middleware@2.20.0 webpack-merge@4.2.1 webpack-node-externals@1.7.2 -D
```

## 重构
### 重写 router 
找到 router 入口文件，一般是 `router/index.js`，以前是返回一个 route 对象，现在改成返回一个 createRouter 函数，（这个函数名不要修改，下面的重构也是很多函数名是固定的不能自己修改），把路由的模式改成 `history` 模式，把懒加载的 `import` 形式改成 `require('xxxx').default` 的写法
```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
const router = new VueRouter({
    mode: 'history',
    routes: [
        {
            path: '/',
            name: 'home',
            component: require('@/views/index').default,
            meta: {
                title: '首页'
            }
        },
        {
            path: '/demo/:id',
            name: 'demo',
            component: require('@/views/demo').default,
            meta: {
                title: 'demo页面'
            }
        }
    ]
})
export function createRouter () {
    return router
}
```
### 重写 Vuex
和 router 一样，Vuex 的导出方式也需要从导出一个对象，改成导出一个函数的形式，Vuex 是 SSR 重构数据请求的中转，详细的请求看源码中的 `src/store` 文件夹
```js
import Vue from 'vue'
import Vuex from 'vuex'

import state from './state'
import actions from './actions'
import mutations from './mutations'

Vue.use(Vuex)
export function createStore () {
    return new Vuex.Store({
        namespaced: true,
        state,
        actions,
        mutations
    })
}
```

### 重写入口文件
找到我们的入口文件，一般是 `src` 目录下的 `main.js` 或者是 `app.js`，把 `new Vue` 的初始化写法改写成 `createApp` 函数，刚刚重写的 `router` 和 `vuex` 就在这里派上用场了，需要注意的是，如果使用一些组件库需要多做一步判断，很多组件库会使用到一些浏览器特用的 api，如 `document` 等，所以我要在浏览器环境才加载这些组件库，详情看下面代码。
```js
import Vue from 'vue'
import App from './App.vue'
import { createStore } from './store'
import { createRouter } from './router'
import { sync } from 'vuex-router-sync'
import ssrMixin from './mixins/ssr.mixin.js'

// 服务端、客户端渲染差异mixin
Vue.mixin(ssrMixin)

Vue.config.productionTip = false

// 仅客户端时才渲染
if (typeof window !== 'undefined') {
    const ElementUI = require('element-ui')
    require('element-ui/lib/theme-chalk/index.css')
    Vue.use(ElementUI)
}

const store = createStore()
const router = createRouter()

export function createApp () {
    // sync the router with the vuex store. this registers `store.state.route`
    sync(store, router)

    const app = new Vue({
        router,
        store,
        render: h => h(App)
    })

    return { app, store, router }
}
```


### 新建文件
新建的文件夹直接参考源码这里不做复制说明

**根目录**

- `index.template.html` 作为应用程序 HTML 标记注入的地方
- `entry-client.js` 客户端渲染处理入口
- `entry-server.js` 服务端渲染处理入口
- `manifest.json` 缓存文件
- `postcss.config.js` postcss 配置文件
- `server.js` 项目启动入口文件

**build 文件夹**  

- `setup-dev-server.js` 打包入口文件
- `webpack.base.config.js` 打包共同配置文件
- `webpack.client.config.js` 客户端打包配置文件
- `webpack.server.config.js` 服务端打包配置文件

**在 src 目录下**

- `ssr.mixin.js` 文件 位置自己决定，例子是放在 src/mixin/ssr/mixin.js 里，做动态TDK 的 mixin 

- `sitemap.html` 站点地图文件，相当于给爬虫一张地图（蛛网）告诉爬虫引擎有哪些页面和不同页面的权重优先级

### 删除文件

- `vue.config.js`

- `main.js`

- `各种不再用到的库包`

### 修改运行和打包脚本

部署脚本运行步骤：
```sh
先通过 npm run build/ npm run build:dev 打包，
然后再 npm run start/ npm run start:dev
```

具体参考代码例子 package.json


### 注意点

1. 生命周期函数调整，没有 `beforeMounted 和 mounted` 函数，使用 `beforeCreate 和 created` 函数代替
2. 不要使用浏览器环境的特有 api ，如 `window document localStorage` ，如使用换个逻辑实现
3. 数据请求不要直接发送，通过 `asyncData Vuex` 转发再在 `computed` 中拿取渲染，具体看例子代码里的实现（内容类通过 `asyncData` 获取比如 列表列或者详情页，如果是权限类不做要求，如获取 `token 验证码`等可以不通过 `Vuex` 转发）
4. `asyncData` 应该放在页面级组件，不要放在组件级组件里，会触发不了路由跳转前逻辑（可以做到，但是会失去SEO效果）
5. 路由层级不要超过 三级 
6. 图片命名不要出现相同，不同文件夹下相同的命名也不行，多加前缀以区分 如 `xxx1-banner.png  xxx2-banner.png`

### 验证

右键点击查看源代码，搜索内容是否在源代码中，在则重构成功

## 总结
SSR 服务端渲染，服务器一次性把所有的页面内容返回到浏览器，包括接口请求后的数据也是已经渲染到 HTML 了才返回到浏览器，而 CSR 客户端渲染则是把所有渲染放在浏览器。爬虫在爬取页面的时候并不会去执行页面的 JS 代码，所以这就是 CSR 对于 SEO 不友好的原因，而 SSR 完美解决了这个缺陷，所有的数据都已经在访问页面的时候已经渲染完成了，爬虫也能爬取到所有的数据。

SSR 说的再直白一点就是我们访问一个地址  `https:xxx/home.com` 的时候其实相当于请求了一个接口，后端把接口的数据返回到浏览器，这个数据是 html 格式的，浏览器进行渲染时只是把 html 代码渲染到页面，减少了接口请求的时间和其它资源的加载时间，对于 SEO 和 首屏时间是很友好的。

### 源码
[Vue2 SSR](https://github.com/W1ping/Vue2-SSR-default)
