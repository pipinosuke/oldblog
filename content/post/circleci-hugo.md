+++
title = "CircleCIで毎回面倒なHugoのデプロイ作業を自動化する"
date = 2019-02-12T22:03:25+09:00
description = "Hugoでブログを書いているのだが、毎回行わなければならないgit commit→git pushの作業が段々と面倒になってきた。その辺を自動でやってくれるらしいCIサービス「CircleCi」を試してみた。"
category = ["技術"]
tags = ["Hugo","CircleCI"]
draft = false
+++

#### CIサービスとは

> ビルドとテストを自動化するためのサービス

ということで理解しておけばいいと思います。一応直訳すると「Continuous Integration（継続的インテグレーションサービス）」、略してCIサービスということらしいです。

参考: [なぜCIが必要なのか](https://dev.classmethod.jp/devenv/why-ci-is-needed/)
### 目標
> GithubPagesで公開しているHugoプロジェクトのデプロイ作業を自動化する

Hugoの場合はプロジェクト本体ではなく、publicディレクトリ以下をgithubにホスティングする必要があるので、デプロイするときは基本的に以下のような手順を踏まなければならない。

``` bash
$ git commit -m "hoge"
$ hugo #public以下にサイトを生成するコマンド
$ cd public
$ git commit -m "hogehoge"
$ git push
```

このルーティンが段々だるくなってくきたので、[CircleCi](https://circleci.com/)でこれを自動化する。

### 手順
1. レポジトリとデプロイキーを登録する
2. .circleci/config.ymlの記述

参考: [CircleCIでHugoを実行してGitHub Pagesにデプロイ](https://t32k.me/mol/log/hugo-circleci-ghpages-2018/)  
[ブログ(HUGO)のビルドとデプロイをCircleCIで自動化した](https://koirand.github.io/blog/2018/blog-auto-deploy/)

#### 1. レポジトリとデプロイキーを登録する
- レポジトリの登録  
最初のブランチ一覧が表示される画面で登録したいレポジトリにチェックマークをつけてFollowを押せば登録できます。
- デプロイキーの登録  
今回はgithubへのsshキーを登録します。サイドメニューのcheck out SSH Key からadd User KeyでGithubアカウントにログインすること楽に登録ができます。手動で登録も可能ですが、pushのための書き込み権限の付与がされてなくてエラーになりました、何やかんややりましたが解決できなかったので、check out ssh keyからの登録をお勧めします。

#### 2. .circleci/config.ymlの記述
CircleCiにやってほしいことを.circleci/config.ymlに記述します。githubのレポジトリにpushされた時発火します。
僕の場合masterブランチでhugoプロジェクト本体、gh-pagesブランチで公開用のpublic以下のファイル群を管理しています。内容はこんな感じです参考までに

``` yaml
version: 2
jobs:
  build:
    docker:
      - image: cibuilds/hugo:latest
        environment:
          TZ: Asia/Tokyo  #ここら辺はコピペでいいと思う

    branches:
      ignore:
        - gh-pages #gh-pagesにプッシュされた際に反応しないようにignoreを設定している

    steps:
      - checkout

      - run:
          name: "Setting for Git"
          command: |
            git config --global user.name "username"
            git config --global user.email "adress@adress"

      - run:
          name: "Get GitHub repository"
          command: git clone git@github.com:username/blog.git

      - run:
          name: "Build & Push"
          command: |
            git clone -b gh-pages git@github.com:pipinosuke/blog.git public
            hugo
            cd public
            git add .
            git commit -m "rebuilding site `date '+%Y-%m-%d'`"
            git push origin gh-pages
```
