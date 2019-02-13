---
layout: post
title:  Hugo覚書とその所感
category: ["技術"]
image: programming.jpeg
description: "ruby製の静的webサイトジェネレータ「jekyll」が非常によかったので、同じWEBサイトジェネレーターであるGo製の「Hugo」も試してみた。Hugo、結果的にはかなりイケてた。"
date: 2019-01-20T09:03:48+09:00
---
### ざっくりHugoについて
Go製の静的webジェネレータ。新しいライブラリというかフレームワーク？？なので、コミュニティもJekyllに比べて盛んな印象です。
### インストール
``` bash
$ brew install hugo
$ hugo version
```
brew以外のインストール方法もあるようです。
参考: [install Hugo](https://gohugo.io/getting-started/installing/)

## 使い方
### コマンド
- プロジェクト作成

``` bash
$ hugo new site yoursite
```
- ローカルサーバーを立てる

``` bash
$ hugo server
```
- post

``` bash
$ hugo new post/first-post.md
```
/content以下に作成される仕様になっております。
- テーマの適用

``` bash
$ git clone hoge@hoge.git themes/hoge #テーマ「hoge」をclone
$ echo 'theme = "hoge"' >> config.toml #config.tomlへ記述
```

設定などはconfig.tomlへ記述するようです。どうでもいいけど、toml形式って初めて見ました。
こちらの公式ドキュメントが参考になると思います [quickstart](https://gohugo.io/getting-started/quick-start/)

## HugoでGithubPagesにホスティング
``` bash
$ hugo
```
でpublicディレクトリが追加される。
このpublicディレクトリ以下をGithubPagesでホスティングします。**プロジェクトディレクトリではありません**、注意！
#### やっておくと良いこと
- 絶対パス化

``` bash
$ echo 'canonifyurls = true' >> config.toml
```
- gitignore

``` bash
$ vi .gitignore
```
で以下をgitignoreしておくと良いと思います
``` vim
themes
public
*.swp
```
参考:  [Host on GitHub](https://gohugo.io/hosting-and-deployment/hosting-on-github/)  
おすすめ: [HugoとGitHub Pagesで静的サイトを公開する](https://qiita.com/satzz/items/e24bd703fc04fb45f7ef)

### 最後に: Hugoの所感およびJekyllとの比較
**シンプルにイケてる！！**
go製ということで速さはもちろん、コミュニティが活発なせいかテーマのクオリティが高いように感じる。総合的にHugoの方がイケてる。コマンドもJekyllでは届かなかった痒い所に手が届く感じがする。触ってみるまで「いうてJekyllと同じやろw」とか思ってたが、githubpagesへのデプロイの方法などが若干異なる部分があった。  
追記: ホームページをHugoでリニューアルしましたリンクは[こちら](https://nosugi.tech)
