# vue项目配置文档

## 创建项目
```
vue create {项目名称}
```
选择默认配置

## 运行项目
```
npm run serve
```

## 安装项目常用模块
主要模块
```
npm install axios vue-router vuex node-sass sass-loader vue-axios vue-cookie --save
```
可能有用的模块
```
npm install vue-lazyload vue-awesome-swiper --save
```

## 解决跨域问题
这里使用配置代理服务器的方式来转发请求,首先在项目根目录新建`vue.config.js`，然后输入如下样例配置:
```
module.exports = {
    devServer: {
        host: 'localhost',
        port: 8080,
        proxy: {
            '/api': {
                target: 'http://localhost:9000',
                changeOrigin: true,
                pathRewrite: {
                    '/api': ''
                }
            }
        }
    }
}
```
以上会将项目中如`/api/login`的链接转换为`http://localhost:9000/login`，即`/api -> http://localhost:9000/`。