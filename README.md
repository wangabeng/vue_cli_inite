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
```
vue-router对mode的说明：
mode

类型: string

默认值: "hash" (浏览器环境) | "abstract" (Node.js 环境)

可选值: "hash" | "history" | "abstract"

配置路由模式:

hash: 使用 URL hash 值来作路由。支持所有浏览器，包括不支持 HTML5 History Api 的浏览器。

history: 依赖 HTML5 History API 和服务器配置。附上链接查看 HTML5 History 模式配置.

abstract: 支持所有 JavaScript 运行环境，如 Node.js 服务器端。如果发现没有浏览器的 API，路由会自动强制进入这个模式。

解决办法
终于弄明白了，如果使用history模式上线，必须要服务端在服务器上有对应的模式才能使用history（看上面链接），如果服务器上没有配置，可以先使用默认的hash；
当然个人建议还是使用history模式，因为这样链接看起来要美观些
```
# 在vue-cli中使用vuex
```
 import store from '@/store/index'

 // 获取state的某个属性
 store.getters.token

 // 调用actions中的方法
 store.dispatch('setToken', 'abc');
```
# 解决点击相同路由地址报错的问题
```
vue-router.esm.js?8c4f:2007 Uncaught (in promise) NavigationDuplicated {_name: "NavigationDuplicated", name: "NavigationDuplicated"}
```
解决方方法 在定义router的时候 增加一段
```
import Router from 'vue-router'

const originalPush = Router.prototype.push
Router.prototype.push = function push(location) {
  return originalPush.call(this, location).catch(err => err)
}
```
# axios跨域问题
Vue使用Axios实现http请求以及解决跨域问题
https://www.jianshu.com/p/0fbaf45340e4

# 请问我用vue做的分页，点击当前页跳转路由，然后返回如何记住当前页？
https://segmentfault.com/q/1010000010215881
```
<keep-alive>
  <router-view v-if="$route.meta.keepAlive"></router-view>
</keep-alive>
<router-view v-if="!$route.meta.keepAlive"></router-view>
然后在路由上,需要keep-alive的地方加一个
meta: {
keepAlive: true
}
```
# 路由变化页面数据不刷新问题
https://www.jianshu.com/p/a0ee62659201
```
 watch: {
// 方法1      
'$route'(to, from) { //监听路由是否变化        
   if(this.$route.params.articleId){// 判断条件1  判断传递值的变化         
//获取文章数据
    }
 }      
//方法2     
'$route'(to, from) {        
  if(to.path == "/page") {    /// 判断条件2  监听路由名 监听你从什么路由跳转过来的           
    this.message = this.$route.query.msg     
   }
 }      
}
```
# 监听路由变化后出现执行2次的情况 
原因在于 在路由上添加了keep-alive 需要用排除或添加的方法 把不需要路由缓存的地方 排除掉
https://www.jianshu.com/p/4b55d312d297

# vue项目安装汇总
```
// jquery
npm install popper.js --save
npm install jquery --save

// vue-router
npm install vue-router --save

// vue-x
npm install vuex --save

// 安装sass
npm install sass-loader --save-dev 
npm install node-sass --save-dev
```

# vue2修改浏览器显示title
一、配置路由器的时候添加如下项 meta
```
routes: [
    {
      path: '/',
      name: 'login',
      component: Login,
      meta: {
        title: '登录'
      }
    },
    {
      path: '/home',
      name: 'home',
      component: Home,
      meta: {
        title: '首页'
      }
    }
  ]  
```
二、在main.js中写全局路由钩子
// 路由钩子
```
router.beforeEach((to, from, next) => {
  if (to.meta.title) {
    document.title = to.meta.title
  }
  next()
})
```

# 使用vue+typescript构建项目，引入weixin-js-sdk后，不能用里面的方法？
1 安装
```
1、npm install --save-dev weixin-js-sdk

2、import wx from 'weixin-js-sdk';
```
2 console.log(wx) 值为undefined
解决
```
const wx = window['wx']
```

# 内网穿透访问Vue项目的时候出现Invalid Host header解决办法
vue-cli3中:  
```
// vue.config.js文件中
module.exports = {
  devServer: {
    disableHostCheck: true
  }
}
```
# swiper刷新页面异常
在Swiper的配置项里加上这2个就可以  
observer: true,  
observeParents: true,  

我也是参考别人的，但大家都是转载来转载去，出处不详了 = = 具体可以看下这2个api的官方文档

# 每次用axios发送请求 就失败，但是用ajax发送 就成功
原因 https://www.cnblogs.com/yiyi17/p/9409249.html
axios发送请求 默认会把参数转成json的格式，ajax就以fomdata的形式发的
解决办法  
网上有很多方案说使用 
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded'; 
或者 
{headers:{'Content-Type':'application/x-www-form-urlencoded'}} 
我试了一下，其实这样还是不行的 
【还需要额外的操作，（我们要将参数转换为query参数）】 
引入 qs ，这个库是 axios 里面包含的，不需要再下载了。
```
import Qs from 'qs'
let data = {
    "username": "admin",
    "pwd": "admin"
}

axios({
    headers: {
        'deviceCode': 'A95ZEF1-47B5-AC90BF3'
    },
    method: 'post',
    url: '/api/lockServer/search',
    data: Qs.stringify(data)
})
```
# vue-axios中post和get携带参数和token
```
this.$axios({
    method:'GET',
    url:`/scihua/webapp/designs/delete?id=${id}`,　　　　　　　　//利用了字符串模板来携带id
    headers:{
            'token':window.localStorage.getItem('token')　　　　//由于是多页面应用所以token存储在本地localStorage中
        }
},

).then(res=>{
    console.log(res)
    this.inlist()
}).catch(req=>{
    console.log(req)
})
```
# vue axios拦截器 配合elementUI loading 讲解 很详细
https://www.cnblogs.com/zhoulifeng/p/9858605.html

# Vue项目中实现用户登录及token验证
https://www.cnblogs.com/web-record/p/9876916.html

# 前后端分离的，不可避免的就产生了跨域问题，导致Authorization始终无法添加到请求头中去
https://blog.csdn.net/why15732625998/article/details/79348718

# 设置headers 报错Request header field Content-Type is not allowed by Access-Control-Allow-Headers
我要自定义一些参数放在headers里面，试了好多方法都不行
比如：http://ask.dcloud.net.cn/question/8596这里的很多个方式
http://ask.dcloud.net.cn/article/13026的方式
后来比较明确的报错Request header field “我自定义的key” is not allowed by Access-Control-Allow-Headers in preflight response.
又去搜了一下找到https://www.cnblogs.com/caimuqing/p/6733405.html，结果发现是后端需要把要传到后端的headers里面的key加入到response
```
// TODO 支持跨域访问  
        response.setHeader("Access-Control-Allow-Origin", "*");  
        response.setHeader("Access-Control-Allow-Credentials", "true");  
        response.setHeader("Access-Control-Allow-Methods", "*");  
        response.setHeader("Access-Control-Allow-Headers", "Content-Type,Access-Token");//这里“Access-Token”是我要传到后台的内容key  
        response.setHeader("Access-Control-Expose-Headers", "*");  

        if (request.getMethod().equals("OPTIONS")) {  
            HttpUtil.setResponse(response, HttpStatus.OK.value(), null);  
            return;  
        }
```
# localStorage - 本地存储（添加、读取、修改、删除、JSON格式存储）
https://blog.csdn.net/Janus_lian/article/details/83412964
```
window.localStorage.setItem('a','123456')
//存入的是number类型，实际存储在localStorage的是string类型，因为localStorage只支持string类型存储


window.localStorage.getItem("a");
console.log(typeof(window.localStorage.getItem("a"))); //String

window.localStorage.setItem('a','新的值')

//清除所有localStorage
window.localStorage.clear();
 
//清除某个键值对
window.localStorage.removeItem("a");

let storage = window.localStorage;
for(let i = 0 ; i < storage.length; i++){
    console.log(storage.key(i));
}


//json格式存取

// 存储 json -》 字符串
var storage=window.localStorage;
var data={
    name:'tom',
    hobby:'program'
};
var nameData = JSON.stringify(data);
storage.setItem("nameData",nameData);

// 读取 string -> json
let getNameData = window.localStorage.getItem("nameData");
console.log(typeof(getNameData)); //sting类型
let getNameDataJSON = JSON.parse(getNameData);
console.log(typeof(getNameDataJSON));  //object类型

 
```

# vue打包的时候报错
Vue-cli3 WARNING in asset size limit: The following asset(s) exceed the recommended size limit (244 KiB)
https://www.cnblogs.com/tonnytong/p/10764454.html

# vue main.js设置全局变量
https://www.cnblogs.com/kewenxin/p/8619240.html

# 理解 vue-router的beforeEach无限循环的问题
调用 next()方法才能进入下一个路由，否则的如果写了beforeEach方法，但是没有调用next()方法的话，页面会空白，不会跳转到任何页面的。
如果已经手动跳转页面，就无需调用next了 否则 就出现重复刷新页面的问题
https://www.cnblogs.com/tugenhua0707/p/10125535.html
