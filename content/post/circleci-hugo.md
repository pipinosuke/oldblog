+++
title = "CircleCIで毎回面倒なデプロイ作業を自動化する"
date = 2019-02-12T22:03:25+09:00
description = "Hugoでブログを書いているのだが、毎回行わなければならないgit commit→git pushの作業が段々と面倒になってきた。その辺を自動でやってくれるらしいCIサービス「CircleCi」を試してみた。"
tags = ["技術"]
draft = false
+++

#### CIサービスとは
CIサービスって言われてもよくわからんので、なんぞやということをメモっておきます。

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
1. CircleCIからレポジトリを登録
2. Circle.ymlの記述

#### 1. レポジトリとデプロイキーを登録する
- レポジトリの登録  
最初のブランチ一覧が表示される画面で登録したいレポジトリにチェックマークをつけてFollowを押せば登録できます。
- デプロイキーの登録  
今回はgithubへのsshキーを登録します。サイドメニューのcheck out SSH Key からadd User KeyでGithubアカウントにログインすること楽に登録ができます。手動で登録も可能ですが、書き込み権限の付与がされてなくてエラーになりました。解決方法はわかってません。
#### 2.  Circle.ymlの記述
.circleci/config.ymlに記述します。
```yaml

```
