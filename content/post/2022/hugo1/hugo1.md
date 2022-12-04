---
title: "Hugo | 建设全纪录"
description: 摸索、抄作业、踩坑与雄心壮志！
date: 2022-12-04T17:49:24+08:00
tags:
  - Hugo
slug: hugo1
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---
## 写在最前
博客能搭起来总的来说跟本人的聪明才智没有半毛钱关系，完全是照抄各路大佬的作业得来，自己最常做的就是灵光一现把网站搞崩溃……在这里超级感谢[塔塔](https://mantyke.icu/)，可以说是我的博客入门导师✊🏻，90%以上都是完全照抄她的作业才得以完成的。所以只是记录搭建过程中一些由于过于基础或者系统差异而教程里没有收录、但是又让我搜得好苦的小操作，还有一些未来装修计划！

## 基本信息
**搭建环境：** macOS Monterey 12.6.1 <br>

**建站参考：** 
[Hugo | 一起动手搭建个人博客吧](https://mantyke.icu/posts/2021/hugo-build-blog/)（1）、
[macOS使用hugo、github搭建个人博客](https://blog.csdn.net/qq_39618959/article/details/118443054)（2）<br>
塔塔的建站教程简直就是保姆级，可以完全跟着做下来，不过因为win和mac系统的差别有一些需要照着第二个教程操作，例如hugo安装和测试等。   

**博客主题：** [Hugo | Hugo-stack-theme 主题魔改版](https://mantyke.icu/posts/2022/stack-theme-mod/)

## 东跑西颠的小问题
### Mac操作：在根目录下
教程1的“在E:\Hugo 右键执行Git bash Here”和教程2的“在根目录下执行命令”后的操作直接在**终端**里输入是没有用的，要先执行前置命令进入目录才可以。  
假如博客文件存放路径在/Documents/Hugo，则需要先执行以下命令。
```
    cd ./Documents
    cd ./Hugo
```
不能直接`cd ./Documents/Hugo`，会报错。真的是很简单的一个操作，但是折磨了我很久才发现……

### Vercel
不知道为什么我通过github注册Vercel的时候一定要电话号码认证，且内地手机号没法用。最后是在google上找了个专门提供国外电话号接受验证码服务的网站解决的问题。如果遇上的同样的问题要做好很多号码都已经注册过的心理准备，多试几次，在注册之后截止目前还没有遇到过再次验证的情况。

### 制作头像链接
是为了交换友链用的！在路径下的static文件夹（没有的话可以新建一个）再放一份头像文件然后改成固定的名字，例如avatar.png。之后换头像的时候再把文件替换进去，这样就可以有一个固定的头像链接[clear0804.vercel.app/avatar.png](http://clear0804.vercel.app/avatar.png)


## 装修计划
- [ ] 修改网站背景颜色
- [ ] 学会增加头图
- [ ] 增加网站评论区
- [ ] 个人书架
- [ ] 一旦又看到好看主题呢（你怎么回事？？？
<br>
<br>

---
