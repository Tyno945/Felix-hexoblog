---
title: 基于Hexo和Github建立自己的博客
tags:
  - hexo
  - github
categories: 原创博文
toc: true
date: 2018-10-24 16:49:02
updated: 2018-10-27 16:49:02
---

欢迎来到风和博客！这是我的第一篇博客，在本篇博客中，我首先向大家介绍下如何建立一个自己个人的博客。我的博客网站是基于Hexo和Github Page搭建的，使用VScode编辑器软件和Markdown语法写博客。

<!-- ![hexo&github](/images/heox&github.jpg) -->

{% asset_img hexo-github.jpg hexo-github %}

**环境和工具准备**
- Windows10系统
- VS code编辑器
- [Node.js](http://nodejs.org/)
- [Git](http://git-scm.com/)
- Github 账号及ssh key配置

## 什么是 Hexo？

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

### 安装使用Hexo

```bash
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
$ hexo server
```

## 写博客

### 更换主题

首先换一个好看的主题，以[cofess/hexo-theme-pure](https://github.com/cofess/hexo-theme-pure)为例，在博客主目录下安装主题及相关插件。

```bash
$ git clone https://github.com/cofess/hexo-theme-pure.git themes/pure
$ npm install hexo-wordcount --save
$ npm install hexo-generator-json-content --save
$ npm install hexo-generator-feed --save
$ npm install hexo-generator-sitemap --save
$ npm install hexo-generator-baidu-sitemap --save
```

主题安装完成后，将`pure/_source`下的文件都复制到博客根目录下的`source`下，否则相关的页面无法获取。主题配置参考[pure github主题文档](https://github.com/cofess/hexo-theme-pure/blob/master/README.cn.md#%E4%B8%BB%E9%A2%98%E9%85%8D%E7%BD%AE)

打开站点配置文件`_config.yml`，找到theme字段，将其值更改为 pure，然后重启服务即可。

更换主题后如遇到问题，可以先删除原有静态文件缓存并重新生成。

    $ hexo clean && hexo generate

### 创建草稿并发布

    $ hexo new draft <title>
    $ hexo publish [layout] <title>
    $ hexo g

### 创建文章

    $ hexo new [post] <title>

### 高级技巧

（1）**模板设置**

当我们使用命令 `hexo new <title>` 去创建我们的文章时，Hexo会根据/scaffolds/post.md模板文件对新建文件进行初始化，所以我们可以修改它来适应自己的写作习惯，一个简单的示例如下：

```yaml
---
title: {{ title }}
date: {{ date }}
updated: {{ date }}
tags:
categories:
---
```

（2）**Front-matter设置**

[Front-matter](https://hexo.io/docs/front-matter) 是文件最上方以 `---` 分隔的区域，用于指定个别文件的变量，包括标题，日期，标签，分类等，示例如下：

```yaml
title: Title
date: YYYY-MM-DD HH:MM:SS
tags: [tag1, tag2, ...]
categories: category
```

（3）**首页显示**

在Hexo框架博客的首页会显示文章的内容（默认显示文章的全部内容），如果当文章太长的时候就会显得十分冗余，所以我们有必要对其进行精简，只需在文章中使用 `<!--more-->` 标志，表示只会显示标志前面的内容。

（4）**在博客中插入图片**

方法一：如果你的Hexo项目中只有少量图片，那最简单的方法就是将它们放在 `source/images` 文件夹中。然后通过类似于 `![](/images/image.jpg)` 的方法访问它们。

方法二：利用标签插件，Hexo也提供了更组织化的方式来管理资源。将 `config.yml` 文件中的 `post_asset_folder` 选项设为 `true` 来打开。

当资源文件管理功能打开后，Hexo将会在你每一次创建新文章时自动创建一个同名文件夹。将所有与你的文章有关的资源放在这个关联文件夹中之后，你可以通过相对路径来引用它们。

在文章中使用如下方式插入图片：

    {% asset_img example.jpg This is an example image %}

方法三: 在打开资源文件管理功能的前提下，安装插件`npm install hexo-asset-image --save`,在资源文件夹（就是那个与title同名的文件夹）中添加了图片example.PNG，则可以在对应的文章中使用语句 `![示例图片](title/example.PNG "示例图片")` 添加图片。

（5）**支持评论**

我使用的是[Gitalk](https://github.com/gitalk/gitalk)评论插件，它是一个基于 GitHub Issue 和 Preact 开发的评论插件。

首先，注册一个[Github OAuth application](https://github.com/settings/applications/new)，填写如下：
```yaml
Application name: # 应用名, 如Felix Blog,或写仓库名
Homepage URL: # 博客的域名URL，如https://tyno945.github.io
Application description: # 应用描述，选填
Authorization callback URL: # 填写当前使用插件页面的域名,和博客URL相同
```
注册成功后，记录下你的`clientID`和`clientSecret`。

其次，安装gitalk并在主题配置文件里启用gitalk评论系统

```yaml
comment:
  type: gitalk  # 启用哪种评论系统
  gitalk: # gitalk. https://gitalk.github.io/
    owner:  # GitHub用户名，必须. GitHub repository 所有者，可以是个人或者组织。
    admin:  # GitHub用户名，必须. GitHub repository 的所有者和合作者 (对这个 repository 有写权限的用户)。
    repo:  # 填写仓库名称，不是地址。必须. GitHub repository.
    ClientID:  #必须. GitHub Application Client ID.
    ClientSecret:  #必须. GitHub Application Client Secret.
```

最后，发布到线上后登陆下进行授权即可。

## 部署发布到github page

### 创建github page

创建repository,必须命名为username.github.io，点击 Setting-->GitHub Pages，你会看到那边有个网址，访问它，你将会惊奇的发现该项目已经被部署到网络上，能够通过外网来访问它。 

### hexo配置部署

1.安装 hexo-deployer-git

    $ npm install hexo-deployer-git --save

2.修改 `_config.yml`配置

```yaml
deploy:
  type: git   
  repo: <repository url>  #git@github.com:Tyno945/Tyno945.github.io.git
  branch: [branch] #master
  message: [message]  #leave this blank
```

3.更新网站并发布

    $ hexo clean && hexo deploy -g

## 博客优化

### 代码压缩

生成静态文件时，自动压缩html,css,js

    $ npm install hexo-neat --save

在博客配置文件`_config.yml`中添加

```yaml
# hexo-neat
neat_enable: true
neat_html:
  enable: true
  exclude:  
neat_css:
  enable: true
  exclude:
    - '*.min.css'
neat_js:
  enable: true
  mangle: true
  output:
  compress:
  exclude:
    - '*.min.js' 
```