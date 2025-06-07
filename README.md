# GoldExperience Blog

基于 Hugo 和 Paper 主题的个人博客网站。

## 特性

- 使用 Hugo 静态网站生成器
- Paper 主题，简洁美观
- 支持多语言（中文、英文、阿拉伯语）
- 自动化部署到 GitHub Pages 和 Vercel
- 自定义域名支持

## 本地开发

### 环境要求

- Hugo Extended v0.147.6+
- Git

### 安装和运行

1. 克隆仓库：
```bash
git clone --recursive https://github.com/你的用户名/仓库名.git
cd GoldExperience
```

2. 启动开发服务器：
```bash
hugo server -D
```

3. 访问 http://localhost:1313 查看网站

## 创建新文章

```bash
# 创建新文章
hugo new post/文章标题.md

# 或创建文章目录（推荐）
hugo new post/文章标题/index.md
```

## 部署

### GitHub Pages

1. 推送代码到 GitHub 仓库
2. GitHub Actions 会自动构建和部署
3. 在仓库设置中配置 Pages 使用 GitHub Actions

### Vercel

1. 在 Vercel 中导入 GitHub 仓库
2. 构建命令：`hugo --gc --minify`
3. 输出目录：`public`
4. 配置自定义域名

## 域名配置

网站使用自定义域名 `gldexp.cn`，需要在域名提供商处配置 DNS：

### GitHub Pages
- A 记录指向 GitHub Pages IP
- CNAME 记录 www 指向 你的用户名.github.io

### Vercel
- CNAME 记录指向 Vercel 提供的域名

## 主题

使用 [Hugo Paper](https://github.com/nanxiaobei/hugo-paper) 主题，已作为 Git 子模块添加。

## 许可证

本项目采用 MIT 许可证。