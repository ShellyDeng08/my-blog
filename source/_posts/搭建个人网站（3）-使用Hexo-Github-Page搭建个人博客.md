---
title: Use Hexo + Github Page build Blog Website! // 使用Hexo + Github Page搭建个人博客
date: 2023-11-30 11:00:00
categories: Others
tags: [Hexo, Github Page, 个人博客, 前端开发, Frontend]
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
```
hexo init [your_folder]
```
2. Run server // 本地启动
```
hexo server
```
默认情况下，项目运行在http://localhost:4000/，如果还有子目录，参见后面的url配置
<img width="1440" alt="截屏2023-12-01 上午9 22 42" src="https://github.com/ShellyDeng08/my-blog/assets/149735476/25c01752-402a-419f-8862-b66051a8f3e7">

## Config // 配置
### Github Config
1. 创建项目
![image](https://github.com/ShellyDeng08/my-blog/assets/149735476/a5dc9830-695c-4501-a23e-601707a77196)

项目名称会是你的博客路由，最后默认的博客地址是${user_name}.github.io/{repo_name}
2. 关联本地项目并推送到远程分支，记住你的分支名称，后面配置的时候要用到，通常是`master`分支
3. 打开项目Settings -> Pages -> Build and deployment, 将source改为Github Actions

![image](https://github.com/ShellyDeng08/my-blog/assets/149735476/1f517135-9fde-4d7f-a24a-e135cb147919)

### Project Config
1. 项目根目录下创建`.github/workflows/pages.yml`
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
这个文件中你需要修改两个地方：
1. 分支名称, 替换成你自己的分支
```
branches:
      - master # default branch
```
2. node版本：可以使用`node -v`查看你当前版本号
```
      - name: Use Node.js 18.12.1
        uses: actions/setup-node@v2
        with:
          node-version: '18'
```


2. 修改_config.yml的url配置
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

再回到Github项目Settings -> Pages，看到Your site is live at xxx，打开这个地址，就能看到你的博客啦
![image](https://github.com/ShellyDeng08/my-blog/assets/149735476/d0058a69-c827-4afd-9480-b599534c2451)

比如我的：https://shellydeng08.github.io/my-blog/

## 本地运行
创建新post:
```
hexo new "Article_title"
```
就能在source/_posts下看到你新建的文章了，在这里使用markdown语法开始写文章，启动服务`hexo server`就可以随时查看效果

## 结语
到这里就完成了个人博客搭建了，如何发布文章、配置主题可以看Hexo的官方文档：https://hexo.io/docs/