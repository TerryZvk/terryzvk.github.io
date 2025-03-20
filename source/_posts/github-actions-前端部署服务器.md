---
title: github actions 前端部署服务器
date: 2021-07-04 01:32:22
tags: ['CI', '部署', '持续集成']
---

前端每次部署都要手动打包感觉好麻烦，浪费了大量时间不说还容易出错。所以就研究了一下github 的github actions。github自己推出的持续集成工具。
<!-- more -->
```
name: Blog CI/CD

on:
  push:
    branches:
      - master  # 只在master上push触发部署

jobs:
  build-production:

    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12.x] # 配置所需node版本
    steps:  # 自动化步骤
    - uses: actions/checkout@v2   # 第一步，检出仓库副本

    - name: Use Node.js ${{ matrix.node-version }} #规定node.js版本(可不配置)
      uses: actions/setup-node@v1
      with:
        node-version: ${{ matrix.node-version }}

    - name: Install dependencies  # 第二步，安装依赖
      run: npm install

    - name: Build                 # 第三步，打包代码
      run: npm run build --if-present

    # Deploy
    - name: Deploy
      uses: easingthemes/ssh-deploy@v2.0.7
      env:
        SSH_PRIVATE_KEY: ${{ secrets.ACCESS_TOKEN }} # 服务端私钥
        ARGS: "-avz --delete"
        SOURCE: "dist/"
        REMOTE_HOST: ${{ secrets.SSH_HOST }}
        REMOTE_USER: ${{ secrets.SSH_USERNAME }}
        TARGET: ${{ secrets.TARGET }}


```
