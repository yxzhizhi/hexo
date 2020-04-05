---
title: hexo 
date: 2020-01-01
categories:
- linux
- hexo
tags:
- hexo
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).
<!--more-->
## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

``` bash
###  hexo install
npm install hexo /yarn add hexo
//npx hexo <command> to hexo 
echo 'PATH="$PATH:./node_modules/.bin"' >> ~/.profile 

hexo init ./yxzhizhi     # 初始化
yarn install    # 安装组件
hexo g   # 生成页面
hexo s   # 启动预览
yarn add hexo-deployer-git

然后修改 _config.yml 文件末尾的 Deployment 部分，修改成如下：
deploy:
  type: git
  repository: git@github.com:用户名/用户名.github.io.git
  branch: master
完成后运行 hexo d 将网站上传部署到 GitHub Pages。

git config --global user.email "xx"
git config --global user.name "xx"

hexo new "name"       # 新建文章
hexo new page "name"  # 新建页面
hexo g                # 生成页面
hexo d                # 部署
hexo g -d             # 生成页面并部署
hexo s                # 本地预览
hexo clean            # 清除缓存和已生成的静态文件
hexo help             # 帮助
https://yxzhizhi.github.io/
```
```
hexo new "name"       # 新建文章
hexo new page "name"  # 新建页面
hexo g
hexo d

```