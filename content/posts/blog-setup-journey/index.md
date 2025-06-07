---
title: "从零搭建个人博客：Hugo + GitHub + Vercel 的踩坑之旅"
date: 2025-06-07T14:00:00+08:00
draft: false
author: "GoldExperience"
description: "记录一下搭建这个博客的完整过程，包括技术选型、部署流程和各种踩坑经历"
tags: ["Hugo", "博客搭建", "Vercel", "技术分享"]
categories: ["技术"]
---

## 前言

最近终于把个人博客搭建好了，整个过程虽然不算复杂，但也踩了不少坑。想着把这个经历记录下来，一方面给自己做个备忘，另一方面也许能帮到其他想搭建博客的朋友。

## 技术选型

### 为什么选择 Hugo？

一开始其实纠结了挺久，主要在 Jekyll、Hexo 和 Hugo 之间选择。最终选择 Hugo 主要是因为：

- **速度快**：Go 语言编写，构建速度确实很快
- **主题丰富**：社区活跃，主题选择多
- **配置简单**：相比 Jekyll 不需要 Ruby 环境
- **文档完善**：官方文档写得很清楚

### 主题选择：PaperMod

在 Hugo 的主题库里逛了好久，最终选择了 PaperMod。这个主题的优点：

- 界面简洁，符合我的审美
- 响应式设计，手机端体验不错
- 功能齐全：搜索、标签、分类、深色模式都有
- 更新活跃，bug 修复及时

### 部署方案：GitHub + Vercel

- **GitHub**：代码托管，免费且稳定
- **Vercel**：自动部署，每次 push 都会自动构建
- **域名**：暂时用 Vercel 提供的免费域名

## 搭建过程

### 1. 本地环境搭建

首先安装 Hugo：

```bash
# Windows 用户可以用 Chocolatey
choco install hugo-extended

# 或者直接下载二进制文件
```

创建新站点：

```bash
hugo new site GoldExperience
cd GoldExperience
```

### 2. 添加主题

使用 Git Submodule 的方式添加主题：

```bash
git init
git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

这里有个小坑：一开始我直接 clone 了主题到 themes 目录，后来发现用 submodule 更好管理。

### 3. 配置文件

这是最费时间的部分。Hugo 的配置文件支持 YAML、TOML 和 JSON 格式，我选择了 YAML，因为看起来比较清爽。

主要配置包括：
- 基础信息（标题、描述、作者等）
- 菜单配置
- 主题参数
- 搜索功能
- 社交链接

### 4. 创建内容

```bash
hugo new posts/welcome-post/index.md
```

Hugo 支持两种内容组织方式：
- Page Bundles（推荐）：每篇文章一个文件夹
- 单文件：直接在 posts 目录下放 .md 文件

我选择了 Page Bundles，这样可以把文章相关的图片等资源放在同一个文件夹里。

### 5. 本地测试

```bash
hugo server -D
```

这个命令会启动本地服务器，并且支持热重载，修改文件后浏览器会自动刷新。

## 部署到 Vercel

### GitHub 仓库

先把代码推送到 GitHub：

```bash
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/username/GoldExperience.git
git push -u origin main
```

### Vercel 配置

1. 在 Vercel 官网注册账号
2. 连接 GitHub 账号
3. 导入仓库
4. 配置构建设置：
   - Framework Preset: Hugo
   - Build Command: `hugo --minify`
   - Output Directory: `public`

## 踩坑记录

### 坑一：中文文件名问题

最开始创建了一个叫「新文章」的文件夹，结果在 Vercel 部署时报错：

```
expected comma character or an array or object ending
```

查了半天才发现是中文文件名导致的编码问题。解决方案：统一使用英文文件名。

### 坑二：vercel.json 配置错误

为了配置一些自定义的重定向和头部信息，我添加了 `vercel.json` 文件。结果正则表达式的转义搞错了：

```json
// 错误的写法
"source": "/(.*\\.(html|xml|json))"

// 正确的写法
"source": "/(.*\\\\.(html|xml|json))"
```

最后干脆删掉了这个文件，让 Vercel 使用默认配置。

### 坑三：Hugo 版本问题

Vercel 默认使用的 Hugo 版本比较新（v0.147.6），但是主题可能不兼容。如果遇到构建错误，可以在环境变量中指定 Hugo 版本：

```
HUGO_VERSION=0.125.7
```

### 坑四：文章列表不显示

一开始文章列表页面是空的，后来发现是因为在 `content/posts/` 目录下放了一个 `index.md` 文件，这会覆盖 Hugo 的默认列表页面。删掉这个文件就好了。

## 优化和改进

### 性能优化

- 启用了 `minify` 选项压缩输出文件
- 配置了适当的缓存策略
- 图片使用 WebP 格式（计划中）

### SEO 优化

- 配置了 `robots.txt`
- 添加了 sitemap
- 设置了合适的 meta 标签

### 用户体验

- 支持深色模式
- 添加了搜索功能
- 配置了代码高亮
- 启用了数学公式渲染

## 总结

整个搭建过程大概花了两天时间，其中大部分时间都在调试配置和解决各种小问题。不过最终效果还是很满意的：

- 网站加载速度快
- 界面简洁美观
- 功能齐全
- 维护成本低

对于想搭建个人博客的朋友，我的建议是：

1. **选择合适的技术栈**：根据自己的需求和技术背景选择
2. **先跑起来再优化**：不要一开始就追求完美
3. **多看文档**：遇到问题先查官方文档
4. **备份很重要**：记得定期备份配置和内容

希望这篇文章能对大家有所帮助。如果有问题欢迎留言讨论！

---

*技术栈：Hugo v0.125.7 + PaperMod 主题 + GitHub + Vercel*