---
title: First Blog
date: 2021-08-07 08:00:08
---

**法外狂徒张三：“未经省察的人生是没有意义的”，共勉**

> 本站是使用Hexo+travis+github pages搭建，意义在于记录自己的学历过程，可能几年过后回首还能勾起些许回忆；同时也是一种激励，因为学习是反人性的，需要及时的正反馈。

## 1.搭建过程

> 参考 https://hexo.io/zh-cn/docs/
### 1.1 安装Hexo脚手架
```shell
$ npm install -g hexo-cli
```
<!--more-->
### 1.2 初始化项目
```shell
$ hexo init <folder>
$ cd <folder>
$ npm install
```
初始化完成之后项目结构
```text
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
### 1.3 安装Hexo-server

```shell
$ npm install hexo-server --save
```
至此就可以本地启动项目了
```shell
$ hexo server
```
### 1.4 创建github仓库
travis对开源项目是免费的，对私有项目是收费的；同时github pages 想要设置域名为 http://username.github.io 则repository name 必须设置同域名，否则域名会变成 https://username.github.io/repository name，即创建repository的配置基本固定，repository name = username.github.io ，repository type = public

![github repository](/images/github.png)

### 1.5 上传代码
> 如果repository没有创建README.md，不会创建分支，创建之后会生成默认**main**分支，不知道从什么时候开始不是**master**分支了。

```shell
$ cd <folder>
$ git init
$ git remote add origin git@github.com:username/username.github.io.git
$ git branch -M main
$ git push -u origin main
```

### 1.6 引入travis

> 参考 https://hexo.io/zh-cn/docs/github-pages

写的很详细，需要注意的是.travis.yml文件中有两处需要修改
1. nodejs版本 v10测试对代码块的支持有限，可以考虑升级到最新版
2. branches 监听分支改为main。

> 将 .travis.yml 推送到 repository 中，Travis CI 应该会自动开始运行，并将生成的文件推送到同一 repository 下的 gh-pages 分支下
```yml
sudo: false
language: node_js
node_js:
  - 14.17.1 # use nodejs v10 LTS -> 14.17.1
cache: npm
branches:
  only:
    - main # build master branch only
script:
  - hexo generate # generate static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  on:
    branch: main
  local-dir: public
```
至此可以看到travis控制台，每次代码提交会自动部署

![travis dashboard](/images/travis.png)

**不得不说，travis是真的厉害，使用人性化，界面简洁**

