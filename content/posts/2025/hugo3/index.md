---
title: "Hugo | Papermod装修日志（2）"
date: 2025-03-14T12:11:37+01:00
description: "这东西我竟然也能写出第二期" #描述
tags: 
    - Hugo
slug: "hugo3"
cover:
    image: 
showToc: true
---
其实很早以前就在想Papermod用的时间太久了，要不换一个新主题吧，但是用得越久花费的装修精力就越多，投入的时间精力越多就越难换，这就是沉没成本的陷阱啊陷阱！上一篇已是2023年，因此干脆开了一篇新的装修记录。

## 修改博客字体
将博客字体修改为[汇文正楷](https://www.maoken.com/freefonts/24506.html)和[Esteba](https://www.maoken.com/english/24291.html)。装修的时候发现好像没有霞鹜文楷那样的引用链接，于是让GPT老师帮我写了代码。
1. 在static文件夹下新建fonts文件夹，将字体文件放入其中。
2. 在博客的自定义css文件中添加以下代码：

```
@font-face {
    font-family: 'HuiwenZhengkai'; /* 自定义字体名称 */
    src: url('/fonts/Huiwen-Zhengkai.ttf') format('truetype'); /* 字体路径 */
    font-weight: normal; /* 设置权重，视字体文件支持的权重而定 */
    font-style: normal; /* 设置风格，例如 normal、italic */
    unicode-range: U+4E00-9FFF;/* 只适用于中文字符 */
}

@font-face {
    font-family: 'Esteban'; 
    src: url('/fonts/Esteban.ttf') format('truetype'); 
    font-weight: normal; 
    font-style: normal; 
}
```
3. 在reset.rss中body模块下的font-family添加```'HuiwenZhengkai','Esteban'```

## 取消博客RSS输出

1. 修改config.yaml
```
outputs: /* 禁用 RSS 输出 */
  home: ["HTML"]
  section: ["HTML"]
  taxonomy: ["HTML"]
  taxonomyTerm: ["HTML"]
```
2. 删除```public/index.xml```文件

## 更改高斯模糊样式

之前的样式是照抄的[塔塔教程](https://mantyke.icu/posts/2021/a08f1963/)，最近发现夜间模式下文字颜色不会自动适配，于是修改了一下css样式。

1. 在```/assets/css/core/theme-vars.css```（其他主题文件可能会有不同）中添加：
```
:root {
    --blur-text-shadow: 0px 0px 8px rgba(0, 0, 0, 0.5); /* 亮色模式阴影 */
}

.dark {
    --blur-text-shadow: 0px 0px 8px rgba(255, 255, 255, 0.5); /* 暗色模式阴影 */
}
```
2. 修改一下之前写的高斯模糊样式
```
/* 高斯模糊 */
.blur {
    color: transparent;
    text-shadow: var(--blur-text-shadow);
}

.blur:hover {
    color: var(--primary);
    text-shadow: none;
}
```
效果展示时间：<span class="blur">也可以切换日间/夜间模式看看！</span>

## NeoDB短代码
也参与了轰轰烈烈的折腾Neodb短代码的活动，参考教程为白石京的[Neodb自动化短评卡片](https://thirdshire.com/hugo-stack-renovation-part-three/)和塔塔的[用NeoDB短代码展示书影游短评](https://mantyke.icu/posts/2025/blogglowup/)，除了rss之外基本没做什么大修改，因为现在人在墙外也不太存在API加载不出来的问题于是也就先没管可能存在的本地加载失败的情况（……）真是能省事一点是一点的典范。

1. 在```layouts/shortcodes```中加入```neodb.html```文件

- 个人不需要太多功能，因此大刀阔斧删减了很多，如有需要可以根据上面提到的两篇教程自行添加

- Neodb音乐条目的url中显示的种类虽然是“album”，但API提取出的分类实际上是“music”，如果按照“album”编写规则会无法显示“听”字，这里要注意修改。很让人挠头的差异……

```
{{ $dbUrl := .Get 0 }}
{{ $apiUrl := "https://neodb.social/api/me/shelf/item/" }}
{{ $itemUuid := "" }}
{{ $authToken := "Your own API" }} <!-- Replace with your actual API token -->

{{ $comment := trim .Inner " \n\r" }}

<!-- Extract item_uuid from the URL -->
{{ if (findRE `.*neodb\.social\/.*\/(.*)` $dbUrl) }}
{{ $itemUuid = replaceRE `.*neodb\.social\/.*\/(.*)` "$1" $dbUrl }}
{{ else }}
<p style="text-align: center;"><small>Invalid URL format.</small></p>
{{ return }}
{{ end }}

<!-- Construct the API URL -->
{{ $dbApiUrl := print $apiUrl $itemUuid }}

<!-- Set up the Authorization header -->
{{ $headers := dict "Authorization" (print "Bearer " $authToken) }}

<!-- Fetch JSON from the API -->
{{ $dbFetch := getJSON $dbApiUrl $headers }}

<!-- Determine shelf status -->
{{ $shelfType := $dbFetch.shelf_type }}
{{ $category := $dbFetch.item.category }}
{{ $action := "" }}
{{ $prefix := "" }}
{{ $suffix := "" }}
{{ $displayText := "" }}

<!-- Determine the action based on category -->
{{ if eq $category "book" }}
    {{ $action = "读" }}
{{ else if or (eq $category "tv") (eq $category "movie") }}
    {{ $action = "看" }}
{{ else if or (eq $category "music") (eq $category "podcast")}}
    {{ $action = "听" }}
{{ else if eq $category "game" }}
    {{ $action = "玩" }}
{{ end }}

<!-- Determine the prefix and suffix based on shelf type -->
{{ if eq $shelfType "wishlist" }}
    {{ $prefix = "想" }}
{{ else if eq $shelfType "complete" }}
    {{ $prefix = "" }}
    {{ $suffix = "过" }}
{{ else if eq $shelfType "progress" }}
    {{ $prefix = "在" }}
{{ else if eq $shelfType "dropped" }}
    {{ $prefix = "不" }}
    {{ $suffix = "了" }}
{{ end }}

<!-- Combine prefix, action, and suffix -->
{{ $displayText = print $prefix $action $suffix }}

<!-- Check if data is retrieved -->
{{ if $dbFetch }}
<div class="db-card">
    <div class="db-card-subject">
        <div class="db-card-post">
            <img src="{{ $dbFetch.item.cover_image_url }}" alt="Cover Image"
                style="max-width: 100%; height: auto;">
        </div>
        <div class="db-card-content">
            <div class="db-card-title">
                <a href="{{ $dbUrl }}" class="cute" target="_blank" rel="noreferrer">
                    {{ $dbFetch.item.title }}
                </a>
            </div>
            <div class="db-card-rating">
                {{ $dbFetch.created_time | time.Format "2006-01-02T15:04:05Z" | time.Format "2006年01月02日" }} {{ $displayText }}
            </div>
            <!-- 这里显示手动输入的评论 -->
            <div class="db-card-comment">
                {{ if $comment }}
                <p>{{ $comment | safeHTML }}</p>
                {{ else }}
                <p>{{ $dbFetch.comment_text | safeHTML }}</p>
                {{ end }}
            </div>
        </div>
    </div>
</div>
{{ else }}
<p style="text-align: center;"><small>Failed to fetch content, please check the API validity.</small></p>
{{ end }}
```

2. 按照喜好调整rss样式
```
/*Neodb card style*/
.db-card {
    margin: 0.5rem 0.5rem;
    background: var(--color-codebg);
    border-radius: 7px;
}

.db-card-subject {
    display: flex;
    align-items: flex-start;
    line-height: 1.6;
    padding: 12px 0px 12px 5px;
    position: relative;
}

.dark .db-card {
    background: var(--color-codebg);
}

.db-card-content {
    flex: 1 1 auto;
    overflow: auto;
    margin-top: 8px;
}

.db-card-post {
    width: 20%;
    margin-right: 15px;
    margin-top: 10px;
    display: flex;
    flex: 0 0 auto;
}

.db-card-title {
    margin-bottom: 3px;
    font-size: 19px;
    color: var(--card-text-color-main);
    font-weight: bold;
}

.db-card-title a {
    text-decoration: none!important;
}

.db-card-rating {
    font-size: 0.9em;
}

.db-card-comment {
    font-size: var(--body-font-size);
    margin-top: 10px;
    margin-bottom: 15px;
    color: var(--card-text-color-main);
}

.db-card-cate {
    position: absolute;
    top: 0;
    right: 0;
    background: #8aa2d3;
    padding: 1px 8px;
    font-size: small;
    font-style: italic;
    border-radius: 0 8px 0 8px;
    text-transform: capitalize;
}

 .db-card-post img {
    width: 120px!important;
    height: flex;
    border-radius: 4px;
    -o-object-fit: cover;
    object-fit: cover;
}

@media (max-width: 600px) {
    .db-card {
        margin: 0.8rem 0.5rem;
    }
    .db-card-title {
        font-size: calc(var(--body-font-size) * 0.75);
    }
    .db-card-rating {
      font-size: calc(var(--body-font-size) * 0.7);
    }
    .db-card-comment {
      font-size: calc(var(--body-font-size) * 0.7);
    }
}
```

3. 上面的版本是只能调用私有API，如果没有标记过的条目会报错。因为有时候想在博客里引用一些没有标记过的条目（例如上篇的人狼村之谜），所以想达成 **“优先调用私有API，如果失败则切换到Neodb的公共API”** 的效果。然后和GPT老师折腾了三天也没搞好，条件语句就是写不出了……最后决定放过自己，先简单粗暴把内容拆成两个短代码文件分开使用。<br><br>使用公共API的短代码部分：

```{{ $dbUrl := .Get 0 }}
{{ $dbApiUrl := "https://neodb.social/api/" }}
{{ $dbType := "" }}

{{ $customComment := .Inner }}

{{ if ( findRE `^.*neodb\.social\/.*` $dbUrl ) }}
    {{ $dbType = replaceRE `.*neodb.social\/(.*\/.*)` "$1" $dbUrl }}
{{ else }}
    {{ $dbType = $dbUrl }}
    {{ $dbApiUrl = "https://neodb.social/api/catalog/fetch?url=" }}
{{ end }}

{{ $dbFetch := getJSON $dbApiUrl $dbType }}

{{ if $dbFetch }}
    {{ $itemRating := 0 }}{{ with $dbFetch.rating }}{{ $itemRating = . }}{{ end }}
    <div class="db-card">
        <div class="db-card-subject">
            <div class="db-card-post"><img loading="lazy" decoding="async" referrerpolicy="no-referrer" src="{{ $dbFetch.cover_image_url }}"></div>
            <div class="db-card-content">
                <div class="db-card-title"><a href="{{ $dbUrl }}" class="cute" target="_blank" rel="noreferrer">{{ $dbFetch.title }}</a></div>
                <div class="db-card-comment"><p>{{ $customComment | safeHTML }}</p></div>
            </div>
        </div>
    </div>
{{else}}
    <p style="text-align: center;"><small>远程获取内容失败，请检查 API 有效性。</small></p>
{{end}}
```
除了不会显示标记时间和状态之外剩下基本一致。