#! https://zhuanlan.zhihu.com/p/301350500
# scala使用playframwork开发webApp系列三——CSRF过滤器配置

- [系列目录](#系列目录)
- [配置`application.conf`文件](#配置applicationconf文件)
- [关于前端请求](#关于前端请求)

## 系列目录
[scala使用playframwork开发webApp系列壹——项目搭建](https://zhuanlan.zhihu.com/p/265746455)

[scala使用playframwork开发webApp系列二——slick使用](https://zhuanlan.zhihu.com/p/286630566)

[**scala使用playframwork开发webApp系列三——CSRF过滤器配置**](https://zhuanlan.zhihu.com/p/301350500)

## 配置`application.conf`文件
加入如下行
```
play.filters.csrf.header.bypassHeaders {
    X-Requested-With = "*"
}
```

## 关于前端请求
此配置需要在前端请求头中加入`X-Requested-With`请求头，该请求头的值随意。(有的框架执行ajax请求会自动添加此header，如jquery)