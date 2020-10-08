---
title:利用 Github Actions 来部署静态博客
date:2020-10-08
---

利用 Github actions ，在每次 push 到 master 之后都自动发布博客内容。

## workflow file

```yaml
# This is a basic workflow to help you get started with Actions

name: Deploy

# Controls when the action will run. Triggers the workflow on push or pull request
# events but only for the master branch
on:
  push:
    branches: [ master ]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2
        with:
          ref: 'master'

      - name: Setup node
        uses: actions/setup-node@v1
        with:
          node-version: '12'

      - name: cache node_modules
        uses: actions/cache@v2
        with:
          path: '**/node_modules'
          key: ${{ runner.os }}-modules-${{ hashFiles('**/yarn.lock') }}

      - name: install dependencies
        run: yarn

      # build
      - name: build
        run: npm run build

      - name: rsync deployments
        uses: burnett01/rsync-deployments@4.1
        with:
          switches: -avzP --delete
          path: public/
          remote_path: ${{ secrets.DEPLOY_PATH }}
          remote_host: ${{ secrets.DEPLOY_HOST }}
          remote_user: ${{ secrets.DEPLOY_USER }}
          remote_key: ${{ secrets.DEPLOY_KEY }}

```



## 大致流程

1. checkout master 分支的代码；
2. 设置 node 环境
3. 安装 node_modules。这里把 node_modules 缓存起来了；
4. 构建。实际上是执行了 Gatsby 的 build 命令；
5. 用 rsync 把构建产品同步到服务器。由于是使用 nginx 直接 serve 静态文件，所以不需要重启服务