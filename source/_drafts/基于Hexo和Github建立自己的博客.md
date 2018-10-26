---
title: 基于Hexo和Github建立自己的博客
date: 2018-10-24 16:49:02
updated: 2018-10-25 16:49:02
tags: [hexo, github]
categories: 原创博文
---
欢迎来到风和博客！这是我的第一篇博客，在本篇博客中，我首先向大家介绍下如何建立一个自己个人的博客。我的博客网站是基于Hexo和Github Page搭建的，使用VScode编辑器软件和Markdown语法写博客。

**环境和工具准备**
- Windows10系统
- VS code编辑器
- [Node.js](http://nodejs.org/)
- [Git](http://git-scm.com/)

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

### 写博客

#### 更换主题

首先换一个好看的主题，以[cofess/hexo-theme-pure](https://github.com/cofess/hexo-theme-pure)为例，在博客主目录下安装主题及相关插件。

```bash
$ git clone https://github.com/cofess/hexo-theme-pure.git themes/pure
$ npm install hexo-wordcount --save
$ npm install hexo-generator-json-content --save
$ npm install hexo-generator-feed --save
$ npm install hexo-generator-sitemap --save
$ npm install hexo-generator-baidu-sitemap --save
```

主题安装完成后，将`pure/_source`下的文件都复制到博客根目录下的`source`下，否则相关的页面无法获取。

打开站点配置文件`_config.yml`，找到theme字段，将其值更改为 pure，然后重启服务即可。

更换主题后如遇到问题，可以先删除原有静态文件缓存并重新生成。

    $ hexo clean && hexo generate

#### 创建草稿并发布

    $ hexo new draft <title>
    $ hexo publish [layout] <title>
    $ hexo g

#### 创建文章

    $ hexo new [post] <title>

#### 高级技巧

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