---
title: Use Hexo + Github Page build Blog Website! // 使用Hexo + Github Page搭建个人博客
date: 2023-11-30 11:00:00
tags:
---

## Install and Enviornment // 安装及环境搭建
准备工作：
- Node.js
- Git
- hexo-cli
```
npm install -g hexo-cli
```
Refer: https://hexo.io/zh-cn/docs/
## Initialize Project // 初始化
1. Create an new folder at local: // 本地创建并初始化项目
hexo init [your_folder]
2. Build static page // 构建静态文件
hexo generate
3. Run server // 本地启动
hexo server
默认情况下，项目运行在http://localhost:4000/
## Config // 配置
### Github
1. 创建项目
![image](https://github.com/ShellyDeng08/my-blog/assets/149735476/a5dc9830-695c-4501-a23e-601707a77196)

项目名称会是你的博客路由，最后默认的博客地址是${user_name}.github.io/{repo_name}
2. 关联本地项目并推送到远程分支
记住你的分支名称
3. 打开项目Settings -> Pages -> Build and deployment, 将source改为Github Actions

![image](https://github.com/ShellyDeng08/my-blog/assets/149735476/1f517135-9fde-4d7f-a24a-e135cb147919)

### Project
项目根目录下创建.github/workflows/pages.yml
```
name: Pages

on:
  push:
    branches:
      - master # default branch

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          # If your repository depends on submodule, please see: https://github.com/actions/checkout
          submodules: recursive
      - name: Use Node.js 18.12.1
        uses: actions/setup-node@v2
        with:
          node-version: '18'
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
      - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```
注意修改：
分支名称
```
branches:
      - master # default branch
node版本：
      - name: Use Node.js 18.12.1
        uses: actions/setup-node@v2
        with:
          node-version: '18'
```
可以使用`node -v`查看你当前版本号

修改_config.yml的url配置
```
# URL
## Set your site url here. For example, if you use GitHub Page, set url as 'https://username.github.io/project'
url: https://shellydeng08.github.io/my-blog
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks
```
把url改为 https://${user_name}.github.io/{repo_name}的格式
提交更新到远程，它就会自动开始部署
再回到项目Settings -> Pages，看到Your site is live at xxx，打开这个地址，就能看到你的博客啦
![image](https://github.com/ShellyDeng08/my-blog/assets/149735476/d0058a69-c827-4afd-9480-b599534c2451)

比如我的：https://shellydeng08.github.io/my-blog/

## Hexo基本使用
### 创建新文章

```
hexo new [post|page|draft]"Article_title"
```
布局（Layout）
Hexo 有三种默认布局：post、page 和 draft。在创建这三种不同类型的文件时，它们将会被保存到不同的路径；而您自定义的其他布局和 post 相同，都将储存到 source/_posts 文件夹。

post:
```
hexo new post "Post_Article_title"
```
创建的post会放在source/_posts下面，并且post没有独立的访问路径，在你的博客首页就能看到所有的post

page:
```
hexo new page "Page_Article_title"
```
创建的page会放在source下面，每一篇page都有独立的访问路径，路径地址是blog_home_link/page_article_file_name

文章的模板在scaffolds/下面，你也可以自定义模板名称和格式，在创建时使用你的自定义模板。

备注：使用vscode编辑器，可以用Control+Shift+V预览markdown格式的文章