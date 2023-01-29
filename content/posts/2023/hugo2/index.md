---
title: "Hugo | 新主题驾到！"
date: 2023-01-07T15:42:37+08:00
description: "新年新气象换个新主题！" #描述
tags: 
    - Hugo
slug: "hugo2"
showToc: true
draft: false
hidden: false
---
建站之后第一次换主题，挑了好久才选了Papermod，简洁风超级好看。还有现成作业可以抄，懒人狂喜！

## 待办列表
其实主要是列一个list，记录一下换主题之后需要更新和装修的内容，方便之后统计。
- [x] 页脚信息
    - 站点运行时间、文章数目&总字数统计
    - ```footer.html```
- [x] 友链信息
- [x] 文章meta信息修改
    - 首页显示问题
- [x] 短代码迁移
    - 图片轮播
    - 引用
    - bilibili
    - 网易云
- [x]favicon
- [x]全站字体
    - 添加字体
    - 修改字号
- [ ]封面图片尺寸
- 评论区
    - [x]取消本地加载
    - [ ]头像随机小怪兽
    - [ ]添加新的表情系列


## 抄抄作业
几乎没有自己动脑全程到处抄抄，很幸福的体验。列一下以供下次换主题参考（你……！
1. [Papermod主题配置](https://www.sulvblog.cn/posts/blog/build_hugo)
2. [文章修改meta信息](https://www.sulvblog.cn/posts/blog/hugo_postmeta/)
3. [给博客添加友链](https://www.sulvblog.cn/posts/blog/hugo_link/)
4. [博客目录放在侧边](https://www.sulvblog.cn/posts/blog/hugo_toc_side/)
5. 短代码参考：[来写一些好玩的 Hugo 短代码](https://irithys.com/p/hugo-shortcode-list/)

## 灵机一动
### 首页显示
修改首页博文显示标题下的小字内容，默认显示summary，否则就会直接显示正文，但是之前博客已经有description再额外写summary感觉太麻烦。于是把这里改成直接显示description。方式是在```list.html```里修改下面这段代码：

```
<div class="entry-content">
      <p>{{ .Summary | plainify | htmlUnescape }}{{ if .Truncated }}...{{ end }}</p>
</div>
```

改成：

```
<div class="entry-content">
    <p>{{ .Description | plainify | htmlUnescape }}{{ if .Truncated }}{{ end }}</p>
</div>
```

改完之后显示效果就是这样的啦！
![ ](18.46.17.png#center)

### waline评论区
换了个主题之后终于建好了……！感觉这一日需要载入本博客史册。参考了[官方中文文档](https://waline.js.org/guide/get-started/)与塔塔这篇[Hugo | 为 Blog 增加评论区](https://mantyke.icu/posts/2021/comment/)。

按照官方文档进行到vercel部署之后，html引入到```layouts/partials/comments.html```里。找在哪引入找了半天最后发现直接放到comments里就行…照抄的塔塔的评论区格式代码也是放在这里。

#### 评论配置
在```config```和```comments.html```里配置都没反应，后来发现是上面放的那串代码直接引入了init。于是走下下策把```waline.mjs```文件下载到本地进行修改。我把这个文件直接放到static里了，引用的时候比较方便。

具体操作：将上文html引入的代码中引```'https://unpkg.com/@waline/client@v2/dist/waline.mjs'```修改为```'/waline.mjs'```，需要修改的地方就直接用开发者模式查看是文件里哪段代码管理的，需要哪段改哪段就好了。

#### 邮件通知
参照文档里的[邮件通知](https://waline.js.org/guide/features/notification.html#%E9%82%AE%E4%BB%B6%E9%80%9A%E7%9F%A5)，在vercel里配置环境变量即可。配置完记得要重新部署。