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
