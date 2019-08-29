# vue_cli 安装卸载等
卸载旧版本  
```
 npm uninstall vue-cli -g

```
  
查看vue_cli版本号   
```
vue --version
或
vue -V
```  
  
vue cli 的包名称由 vue-cli 改成了 @vue/cli。 如果你已经全局安装了旧版本的 vue-cli (1.x 或 2.x)，你需要先通过 npm uninstall vue-cli -g 或 yarn global remove vue-cli 卸载它。
```
npm install -g @vue/cli
```
# 创建项目
```
vue create vue-webpack-demo
```
# 启动项目
```
npm run serve
```
# webpack相关配置
https://cli.vuejs.org/zh/guide/webpack.html#%E7%AE%80%E5%8D%95%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%B9%E5%BC%8F
默认vue-cli3没有配置文件，在根目录下新建一个
vue.config.js
## 遇到的问题 打包后 文件为空 js css 加载路径不正确 解决方法
vue.config.js
```
module.exports = {
    baseUrl: './',//输出的根路径  默认是/ 如果你的网站是app.com/vue 这更改此配置项
    /*outputDir:'dist',//构建输出目录
    assetsDir:'assets',//静态资源目录(js,css,img,fonts)
    lintOnSave:false,//是否开启eslint保存检测,有效值: true || false || 'error'
    devServer:{
        open:true,//是否自动弹出
        host:'localhost',
        port:8080,//端口
        https:false,
        hotOnly:false,//热更新
    }*/
}
```
# vue-cli3设置文件夹别名 + 配置jquery为全局
```
const webpack = require('webpack');

const path = require('path');
function resolve(dir) {
  return path.join(__dirname, dir)
}

module.exports = {
    "publicPath": './',//输出的根路径  默认是/ 如果你的网站是app.com/vue 这更改此配置项
    /*outputDir:'dist',//构建输出目录
    assetsDir:'assets',//静态资源目录(js,css,img,fonts)
    lintOnSave:false,//是否开启eslint保存检测,有效值: true || false || 'error'
    devServer:{
        open:true,//是否自动弹出
        host:'localhost',
        port:8080,//端口
        https:false,
        hotOnly:false,//热更新
    }*/
    configureWebpack: {
      plugins: [
        new webpack.ProvidePlugin({
          $:"jquery",
          jQuery:"jquery",
          "windows.jQuery":"jquery"
        })
      ]
    },
    /*chainWebpack: config => {
        config.resolve.alias
            .set('@', resolve('src'))
            .set('assets', resolve('src/assets'));
    },*/
    chainWebpack: (config) => {
        config.resolve.alias
            .set('@$', resolve('src'))
            .set('components',resolve('src/components'))
            .set('common',resolve('src/common'))
            .set('base',resolve('src/base'))
            .set('static',resolve('src/static'))
    }
}
```
# 配置好后 在测试的时候 用console.log 报错error: Unexpected console statement (no-console)
解决办法
https://www.jianshu.com/p/bfc7e7329cff
关闭eslint 在package.json中添加设置  "no-console": "off"(json中部可以有注释 否则编译不通过)
```
  "eslintConfig": {
    "root": true,
    "env": {
      "node": true
    },
    "extends": [
      "plugin:vue/essential",
      "eslint:recommended"
    ],
    "rules": {
      "no-console": "off"
    },
    "parserOptions": {
      "parser": "babel-eslint"
    }
  },
```
# 安装sass/scss（是sass的升级版本）
```
npm install node-sass --save-dev //安装node-sass 
npm install sass-loader --save-dev //安装sass-loade
```

# 初始化git本地项目并推送到远程
1 远程建立仓库 readme为空 并复制地址  
2 进入项目目录 
```
// 初始化
git init
// 关联地址
git remote add origin git@github.com:XXXX/XXXXX.git
// 重新设置地址
git remote set-url origin https://gitee.com/name/project.git 

//
git add .
git commit -m "msg"
// 首次推送到远程仓库 必须指明推送的分支
git push --set-upstream origin master

````

# 当路由开启history模式 vue-router 配置404报错 如果只配置一级路由的404 不会报错 如果输入二级路由 浏览器就会报错
https://forum.vuejs.org/t/uncaught-syntaxerror-unexpected-token/32862
开始history模式 需要后端配合
```
刷新页面出现空白 估计是后端路由识别不了 譬如https://www.youproject.com/main/1 刷新时后端没找到这个路径，配置一下后端 找不到的时候重定向到入口的index.html 前端路由就能正常使用了

参考官方文档https://router.vuejs.org/zh-cn/essentials/history-mode.html
```
# vue-cli3 中在index.html中引入js css文件方法
把所有css js文件放入public文件夹下 引入方式 
```
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <script src='<%= BASE_URL %>selection.js'></script>
```

# vue-cli3中如何引入bootstrap和jquery (也可以一次性安装 npm install jquery bootstrap@3 popper.js --save)
## 1 安装jQuery  
bootstrap是基于jQuery的，在使用之前我们先安装一下jQuery包 又因为jquery以来popper.js，所以在安装jquery之前先安装popper
```
npm install popper.js --save
npm install jquery -S
```
然后在main.js中引入jquery作为全局变量
```
import $ from 'jquery'
```

```
/*
*Vue-CLI项目的核心配置文件
*/
const webpack = require("webpack");

module.exports = {
 configureWebpack: {
   plugins: [
     new webpack.ProvidePlugin({
       $: "jquery",
       jQuery: "jquery",
       "window.jQuery": "jquery",
       Popper: ["popper.js", "default"]
     })
   ]
 }
};
```

## 2 安装bootstrap
接下来我们安装bootstrap依赖，这里的@3是安装bootstrap3.x版本，不喜欢这个版本的小伙伴也可以安装最新版。
```
npm install bootstrap@4 -S
```
在main.js中导入bootstrap
```
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
// 导入bootstrap
import "bootstrap"
import "bootstrap/dist/css/bootstrap.css"


Vue.config.productionTip = false

new Vue({
router,
store,
render: h => h(App)
}).$mount('#app')
```
# 解决vue-cli3打包代码后，上线服务器后白屏问题及路由跳转错误问题
路由模式为mode: 'history',  打包后路由 /helloworld  变为http://localhost:3000/helloworld 如果直接从D盘打开 会出现路径错误。所以，如果用history模式，必须要配合服务器设置。否则，就不要用history模式。
