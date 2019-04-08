+++
title = "RxSwiftでUITableViewの実装"
date = 2019-04-03T00:46:56+09:00
description = "RxSwiftでUITableViewの実装がよくわからなかった。というかRxSwift自体よくわからなかった。ということでこの際ちゃんとまとめることにしました"
tags = ["RxSwift","iOS"]
category = ["技術"]
image = "rxswift.jpg"
draft = false
+++
鬼門でした。

## RxSwiftを使ったUITableViewの実装
大きく分けてふたパターンあります。UITableViewDataSourceで実装する部分をRxを用いて実装します。1のやり方はシンプルです。2は自由度が高い実装が可能です。

> 1. ObservableをUITableViewにbindして実装
> 2. DataSourceを自分で定義して実装

### 1. ObservableをUITableViewにbindして実装
#### A.  デフォルトのセルを使う場合
indexPath.rowとitems[n]、cellを引数に取ります。
```swift
let items: Observable<[Item]> = ...

items
    .bind(to: tableView.rx.items(cellIdentifier: "Cell")) { row, element, cell in
        // row: Int …… アイテムのインデックス
        // element: Item …… アイテムのインスタンス
        // cell: UITableViewCell …… セルのインスタンス

        // ここでセルの中身を設定する
        cell.textLabel?.text = element.name
    }
    .disposed(by: disposeBag)
```
#### B.  カスタムセルを使う場合
Aパターンの実装に加えてcelltypeを指定できるようになりました。カスタムセルを使う場合はこちらを使います。
```swift
let items: Observable<[Item]> = ...

items
    .bind(to: tableView.rx.items(cellIdentifier: "Cell", cellType: MyTableViewCell.self)) { row, element, cell in
        // row: Int …… アイテムのインデックス
        // element: Item …… アイテムのインスタンス
        // cell: MyTableViewCell …… セルのインスタンス

        // ここでセルの中身を設定する
        cell.nameLabel.text = element.name
        cell.ageLabel.text = "\(element.age)"
    }
    .disposed(by: disposeBag)
```
#### C. 複数のカスタムセルを使いわける場合
tableViewとindexPath.rowとarticle[n]を引数に取ります。Cellの定義はクロージャーの中で行うので、複数のCellを使い分けることができます。3つの中では一番自由度が高いやり方。
```swift
let items: Observable<[Item]> = ...

items
    .bind(to: tableView.rx.items) { tableView, row, element in
        // tableView: UITableView …… テーブルビュー
        // row: Int …… アイテムのインデックス
        // element: Item …… アイテムのインスタンス

        // ここでセルを作って、中身を設定する
        let cell = tableView.dequeueReusableCell(withIdentifier: "Cell")!
        cell.textLabel?.text = element.name
        return cell
    }
    .disposed(by: disposeBag)

```
### 2. DataSourceを自分で定義して実装する
UITableViewにおける通常の実装ではよく用いられる`UITableViewDataSource`を継承するクラスを定義し、そのクラスごとBindしてしまうやり方です。通常の実装で使うメソッドは全て使うことができるので一番自由度の高い実装が可能です。さらに補足として次の2点を挙げておきます

- 定義するDataSorceは通常の`UITableViewDataSource`に加えて`RxTableViewDataSourceType`を継承する必要があります。
- `UITableViewDelegate`は通常通りViewController内で実装できます。

個人的には必要性を感じませんが[RxDataSources](https://github.com/RxSwiftCommunity/RxDataSources)というライブラリを使って同様の実装ができるようです。参考URL: [RxSwiftでの実装練習の記録ノート（前編：Observerパターンの例とUITableViewの例）](https://qiita.com/fumiyasac@github/items/90d1ebaa0cd8c4558d96#4-chapter1%E3%83%A9%E3%83%BC%E3%83%A1%E3%83%B3%E3%81%AE%E4%B8%80%E8%A6%A7%E3%82%92rxdatasources%E3%82%92%E5%88%A9%E7%94%A8%E3%81%97%E3%81%A6uitableview%E3%81%AB%E4%B8%80%E8%A6%A7%E8%A1%A8%E7%A4%BA%E3%82%92%E3%81%99%E3%82%8B%E3%83%97%E3%83%A9%E3%82%AF%E3%83%86%E3%82%A3%E3%82%B9)

## データ自体をストリームで流す
例としてニュースアプリなどでよく見る、通信で受け取った記事データをTabaleViewで一覧表示する機能を考えてみましょう。いきなり最終形を書くと意味がわからなくなると思うので、理解しやすいよう2つの段階に分け説明します。

#### 第一段階: 受け取った時にイベントを流す
記事データを受け取ったタイミングでデータをviewModelに保存し、Observable<Bool>を流すやり方です。当たり前ですがこの時ObservableはUIViewControllerが購読しており、viewModel内の記事データをbindします。やっていること自体はMVVMにおける通常の実装とそこまで変わらないのでイメージしやすいかなと思います。

#### 第二段階: 受け取った時に記事データそのものを流す
さらに発展させてこのような実装もできます。


#### 参考URL
- [RxSwiftのUITableViewとのバインディングまとめ](https://qiita.com/hironytic/items/71bc729abe73ab9f0879)
- [RxDataSourcesライブラリ＋UITableViewを使ってMVVMサンプルiOSアプリを作る](https://qiita.com/k0uhashi/items/d4a6bdcd6fca014a37dd)
- [SimpleTableviewExample](https://github.com/ReactiveX/RxSwift/blob/master/RxExample/RxExample/Examples/SimpleTableViewExample/SimpleTableViewExampleViewController.swift)
- [SimpletableviewExampleSectioned](https://github.com/ReactiveX/RxSwift/blob/master/RxExample/RxExample/Examples/SimpleTableViewExampleSectioned/SimpleTableViewExampleSectionedViewController.swift)
- [RxのHotとColdについて](https://qiita.com/toRisouP/items/f6088963037bfda658d3)
- [RxCocoa 4 の Signal と Relay のまとめ](https://tech.mercari.com/entry/2017/12/04/103247)