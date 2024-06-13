---
title: "Hugo | 新主题驾到！"
date: 2023-01-07T15:42:37+08:00
description: "新年新气象换个新主题！" #描述
tags: 
    - Hugo
slug: "hugo2"
showToc: true

---
建站之后第一次换主题，挑了好久才选了Papermod，简洁风超级好看。还有现成作业可以抄，懒人狂喜！

## 待办列表
Papermod主题以来更新和装修内容的List，方便之后统计【最后更新日期：240613】
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
    - Neodb卡片
- [x]favicon
- [x]全站字体
    - 添加字体
    - 修改字号
- [x]封面图片尺寸
- 评论区
    - [x]取消本地加载
    - [x]头像随机小怪兽
    - [ ]添加新的表情系列
- [x]修改站点语言
    - ```defaultContentLanguage```
- [x]文本样式
    - 高斯模糊
    - 黑幕效果
- [x] 书影游墙
- [x]博客目录显示层级修改
- [x]友链界面样式修改

## 抄抄作业
几乎没有自己动脑全程到处抄抄，很幸福的体验。列一下以供下次换主题参考（你……！
1. [Papermod主题配置](https://www.sulvblog.cn/posts/blog/build_hugo)
2. [文章修改meta信息](https://www.sulvblog.cn/posts/blog/hugo_postmeta/)
3. [给博客添加友链](https://www.sulvblog.cn/posts/blog/hugo_link/)
4. [博客目录放在侧边](https://www.sulvblog.cn/posts/blog/hugo_toc_side/)
5. 短代码参考：[来写一些好玩的 Hugo 短代码](https://irithys.com/p/hugo-shortcode-list/)
6. [文章封面图片](https://www.sulvblog.cn/posts/blog/img_right/)
7. [Blog | 主题重新施工，和书影游展示墙](https://mantyke.icu/posts/2022/a-flower-upon-your-return/)

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

### 博客目录显示层级修改
标题叠太多层时都显示在目录里太乱了不好看，希望目录里只显示到三级标题。于是把```toc.html```里第一行的两个```h[1-6]```改成了```h[1-3]```。

### 友链界面修改
看到[小鱼](https://gregueria.icu/)的友链界面觉得好好看于是冲去毛象求教！小鱼很热情地把代码直接发给我了ww，只要在友链界面文章前放CSS样式就可以，在小鱼的版本上自行进行了一些修改，具体代码如下：

```
<style>
.post-content ul {
    padding-inline-start: 40px;
    /*修改列表缩进量*/
}

.post-content a{
    box-shadow: none;
    /*去掉链接下方横线*/
    color: rgb(114,62,136); 
    /*修改链接颜色*/
}

.post-content li::marker {
  content: "❀  "; 
  /* 让无序列表前的圆点变成小花 */
  color:  rgb(152,101,175); 
  /*修改小花颜色*/

}

.post-content a::after {
    content: " | " ;
    /* 让每个超链接后都有个分隔号 */
    padding: 0 0.2em; /* 调整间距大小 */
}

.post-content a:last-child::after {
    content: none;
    /* 最后一个链接不需要分隔号 */
}
</style>
```

效果：
![1](1.png#center)
<style>
  img[alt="1"]{
    width:500px;
  }
</style>