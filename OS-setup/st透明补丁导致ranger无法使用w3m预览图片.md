#! https://zhuanlan.zhihu.com/p/266553673
# ranger无法预览图面
- [问题详细描述](#问题详细描述)
- [解决方案](#解决方案)
  - [ueberzug安装方法](#ueberzug安装方法)
- [相关链接](#相关链接)
## 问题详细描述
st使用透明补丁之后，会导致ranger无法使用w3m预览图片。
## 解决方案
使用`ueberzug`代替w3m预览图片。
### ueberzug安装方法
卸载当前`ranger`：
```
sudo apt remove ranger
```
使用pip3安装`ranger`:
```
pip3 install ranger-fm
```
安装`ueberzug`:
```
pip3 install ueberzug
```
修改`ranger`的rc.conf:
```
set preview_images_method ueberzug
```

## 相关链接
[ueberzug](https://github.com/seebye/ueberzug)
[ranger](https://github.com/ranger/ranger)