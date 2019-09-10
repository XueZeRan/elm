# 项目介绍
### 一个Vue.js实战项目
# 项目技术栈
## vue全家桶
### vuejs---
### Vue-cli---
### Axios---
### vue-router
## 项目构建工具
### webpack--
### npm--
## 基础"基石"
### javascript+html+css
## css预编译
### stylus
### stylus-loader
## 移动端适配
### width=device-width,initial-scale=1.0,maximum-scale=1.0,minimum-scale=1.0,user-scalable=no
## 依赖第三方css库
### reset.css
### border.css 解决移动端1px边框问题
### better-scroll  移动端滑动组件功能
### fastclick 300毫秒延残的点击事件问题


# 项目功能
### 商品滚动，商品滚轮滚动--
### 商品联动--
### 加入购物车，移除购物车--
### 显示评论评论筛选--
### 图片左右滑动--
### 商品详情父子组件的通信--
### 商品购物金额获取显示--结算金额
### 动画的效果，淡入淡入，3D效果转动
### 路由跳转，组件替换，实现跳转页面不刷新
### loaclStorage 缓存商家信息

# 打包部署GitHub所遇到的问题
### 相信同学也希望将自己完成的项目分享出来
### 本人在一番资料查找并结合自己的想法后得到的解决方案-结果的确能实现在GitHub页面成功显示
## 第一个问题：打包生成的dist文件夹上传到GitHub后，打开项目页面网址，js，css文件不成功显示
### 此解决方案：在搜索引擎搜索后得到，需要打开webpack.dev.conf.js文件，Ctrl+f查找到assetsPublicPath并按住Ctrl点击进入index.js文件夹，再次查找assetsPublicPath 将assetsPublicPath:'/'修改为assetsPublicPath: './',即“/”前加 .  ，注意有两个assetsPublicPath:'/'时应该都修改（我是这么做的，结果可行）在修改后即可打包，运行：npm run build   得到dist文件夹，注意修改后在本地运行：npm run dev 时打开页面时js，和css等显示不出来，原因是我们修改了文件路径，如果需要在本地运行，需要重新修改，将.去掉即可
## 第二个问题：（依赖于Axios）同学们做项目时一般用的是本地保存的json数据，用axios获取数据，可当我们打包上线到GitHub后，本地使用的接口路径已经失效，所以出现打开页面数据，图片不显示，
### 解决方案：本人在查找资料，得到的解决方法均是搭建后台服务器来解决并上线，将json数据在服务器上变成真实数据，用axios编写得到的网络接口地址，  而我们则想在GitHub上线，那么问题就变成如何不自己搭建服务器，得到自己需要的网络地址接口呢？本人在打包上传时发现，本地json数据在GitHub上已经有了可以打开的网络地址：比如项目地址为 https://项目地址.github.io/存储库名/dist  我们的json文件地址就在dist文件内，所以也是可以访问到的，假如dist文件内有：data.json 文件，我们则可以通过 https://项目地址.github.io/存储库名/dist/data.json 得到数据接口，打开链接得到json数据即获取成功。
### 我们需要将axios的接口地址该为这个真实的网络地址即可
### 比如
###     methods: {
###       getHomeInfo() {
###         axios.get('https://项目地址.github.io/存储库名/dist/data.json')
###	        .then(this.getHomeInfoSucc)
###       },
###        getHomeInfoSucc(res) {
###         res=res.data
###         if (res.ret && res.data) {
###         }   
###         this.data=res   //使用data变量来保存数据
###         console.log(res) //控制台打印出来有想要数据，即获取成功
###       }
###     }

###     mounted () {
###       this.getHomeInfo() //让页面挂载好后执行该函数
###       }
### 
### 在将所有接口修改后，我们可以先在本地运行，测试是否成功显示数据，页面是否成功加载（要将第一步的 . 去掉），成功后即可再次修改为"./"，并打包上传到GitHub，输入得到的项目地址，加入/dist ，打开，结果成功显示，即json数据成功获取，js，css文件路径也正确配置成功。

# vue-cli少了很多文件，要修改路径方法如下
## 立即联想到需要改输入路径的地址。却尴尬的发现之前的build和config文件夹不见了。查阅后发现如果需要自定义配置，需要在项目的 根目录添加一个## Vue.config.js。在这个文件中，我们可以进行一些个性化定制

module.exports = {
    // 基本路径
    baseUrl: './',
    // 生产环境是否生成 sourceMap 文件
    productionSourceMap: false,
    // 服务器端口号
    devServer: {
        port: 1234,
    }
}
# ----------------
## Build Setup
``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).



