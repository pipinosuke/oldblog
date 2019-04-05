+++
title = "RxSwiftでUITableViewの実装"
date = 2019-04-03T00:46:56+09:00
description = "RxSwiftでUITableViewの実装がよくわからなかった。というかRxSwift自体よくわからなかった。ということでこの際ちゃんとまとめることにしました"
tags = ["RxSwift","iOS"]
category = ["技術"]
image = "rxswift.jpg"
draft = false
+++
### RxSwift入門
- [リアクティブプログラミングとRxJavaの概要](https://codezine.jp/article/detail/9570)
- [オブザーバーパターンから始めるRxSwift入門](https://qiita.com/k5n/items/17f845a75cce6b737d1e)

まずは上の記事でリアクティブプログラミングのノリを理解します。特に6pのマーブルダイヤグラムの図が理解に役立ちます。下の記事は実際のコードを交えた解説です。具体的にどのような実装を行えば良いのかがわかります。

## RxSwiftを使ったUITableViewの実装
大きく分けてふたパターンあります。1のやり方はシンプルです。2は自由度が高い実装が可能です。

> 1. ObservableをUITableViewにbindして実装
> 2. DataSourceを自分で定義して実装

### 1. ObservableをUITableViewにbindして実装
#### A.
#### B.
#### C.
### 2. DataSourceを自分で定義して実装
[RxDataSources](https://github.com/RxSwiftCommunity/RxDataSources)というライブラリを使って実装できます。

#### 参考URL
- [RxSwiftのUITableViewとのバインディングまとめ](https://qiita.com/hironytic/items/71bc729abe73ab9f0879)
- [RxDataSourcesライブラリ＋UITableViewを使ってMVVMサンプルiOSアプリを作る](https://qiita.com/k0uhashi/items/d4a6bdcd6fca014a37dd)
- [SimpleTableviewExample](https://github.com/ReactiveX/RxSwift/blob/master/RxExample/RxExample/Examples/SimpleTableViewExample/SimpleTableViewExampleViewController.swift)
- [SimpletableviewExampleSectioned](https://github.com/ReactiveX/RxSwift/blob/master/RxExample/RxExample/Examples/SimpleTableViewExampleSectioned/SimpleTableViewExampleSectionedViewController.swift)
- [RxのHotとColdについて](https://qiita.com/toRisouP/items/f6088963037bfda658d3)


### 通信について
通信し値を受け取った時の挙動もRxSwiftでかけます。subject
あとで追記します