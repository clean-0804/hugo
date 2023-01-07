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
- [x]favicon
- [x]全站字体
- [ ]封面图片尺寸
- [ ]评论区
    - 评论区……我总也搞不好的评论区………


## 抄抄作业
抄了一位大佬的全套作业，很幸福的体验。列一下以供下次换主题参考（你……！
1. [Papermod主题配置](https://www.sulvblog.cn/posts/blog/build_hugo)
2. [文章修改meta信息](https://www.sulvblog.cn/posts/blog/hugo_postmeta/)
3. [给博客添加友链](https://www.sulvblog.cn/posts/blog/hugo_link/)
4. [博客目录放在侧边](https://www.sulvblog.cn/posts/blog/hugo_toc_side/)

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