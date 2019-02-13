---
layout: post
title: Fish Shell覚書
category: ["技術"]
description: "デフォルトのシェルではなく、fishシェルを愛用している。その際の設定などをメモ。"
image: programming.jpeg
date: 2019-01-12T09:03:48+09:00
---
## 導入&設定
### 導入
``` bash
$brew install fish
```

デフォルトシェルに設定
``` bash
$ sudo vi /etc/shells

#以下を記述
/usr/local/bin/fish
```
``` bash
$chsh -s /usr/local/bin/fish
```
### 設定
設定やpathはconfig.fishに記述
``` bash
$ vi ~/.config/fish/config.fish
```
読み込み
``` bash
$ source ~/.config/fish/config.fish
```

## その他
### rbenv
rubyのバージョン管理のライブラリで使用している方も多いと思われる。fishだとそのままでは動かないので以下のように記述する
``` bash
rbenv init - | source
```
### GOPath
``` bash
set -x GOPATH /users/username/go
set -x PATH $PATH /usr/local/go/bin $GOPATH/bin
```

## 参考
- [【fish shellでモダンなターミナル環境を構築】＠2017年版](https://qiita.com/k-waragai/items/396acd783ed03511d57c)
- [fish で rbenv の初期処理を実行する方法](https://qiita.com/raccy/items/61bd4780b2bd6de49deb)
