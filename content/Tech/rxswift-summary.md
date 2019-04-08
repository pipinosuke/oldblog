+++
draft = false
image = "rxswift.jpg"
tags = ["iOS","RxSwift","リアクティブプログラミング"]
category = ["技術"]
date = "2019-04-08T03:06:17+09:00"
title = "RxSwift入門とObservableの概要 まとめ"
description = "Rxが注目されてからややしばらく経ちましたがわからず使っている部分も多々あり、また「Variable」が非推奨になるなど仕様が変更された部分もあるので、RxSwift4について改めて基本的な概念についてまとめました。。"
+++
# RxSwift入門 Observableの概要 まとめ
### 入門
- [リアクティブプログラミングとRxJavaの概要](https://codezine.jp/article/detail/9570)
- [オブザーバーパターンから始めるRxSwift入門](https://qiita.com/k5n/items/17f845a75cce6b737d1e)

まずは上の記事でリアクティブプログラミングのノリを理解します。特に6pのマーブルダイヤグラムの図が理解に役立ちます。下の記事は実際のコードを交えた解説です。具体的にどのような実装を行えば良いのかがわかります。

### Rxができることとそのメリット
Rxを使いこなせれば**ありとあらゆるものをストリームとして扱うことが可能です**

![everything is stream](https://camo.qiitausercontent.com/644aff983f0cd1e6db4faf8a6bdc28e8fffd9787/687474703a2f2f626c6f672e616d61793037372e6e65742f6173736574732f696d616765732f706f7374732f6576657279746869735f69735f615f73747265616d5f776861745f69735f766965776d6f64656c5f30312e6a7067)

例えばボタンをタップされた時の挙動はこう書けます。（以下のコードは厳密には`RxSwift`ではなく`RxCocoa`というUIKitをObservableとして扱うことのできるライブラリを用いています。）

```swift
import RxCocoa
import RxSwift

weak var button: UIButton!

~~~~~~~~~~~~~~~~~~

override func viewDidLoad(){
  button.rx.tap.subscribe(onNext: { _ in
    print("buttonがtapされたよー")  
  }).disposed(by: DisposeBag())
}
```


例えばデータのbindや、面倒だった非同期処理を簡単に書けるけるのがメリットです。若干学習コストは高いかなと思いますが、マスターできれば文字通りなんでもできます。

## Rxで登場する基本概念 2種
### Observable
名前だけでも覚えて帰ってください！！Observableはストリームを流し、そのストーリムをsubscribe(購読)することが可能になります。
<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">Observableが通知するイベントは以下の3種類。subscribe(購読)することで、それぞれのタイミングでクロージャ内のメソッドを呼ぶことが出来る。<br><br>･onNext: 通常のイベントが発生した時<br>･onError: エラーが発生になった時<br>･onComplete: イベントが完了した時</p>&mdash; nosugi (@nosugi1) <a href="https://twitter.com/nosugi1/status/1114825476126662656?ref_src=twsrc%5Etfw">2019年4月7日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


言い換えると、何かが起こった時の挙動をクロージャの中に記述しておけば何もせんでも勝手に呼んでくれるということです。下のコードは[新卒エンジニアの開発日記](https://fukatsu.tech/rxswift)さんの記事から引用したものになります。趣旨とは外れますが、中の人とは実は学生時代の知り合いだったりします。笑
```swift
import RxSwift
import RxCococa

let observableContentOffset = tableView.rx.contentOffset

observableContentOffset
    .subscribe(onNext: { _ in
        print("next")      //スクロールするたびに呼ばれる
    }, onError: { _ in
        print("error")
    }, onCompleted: { _ in 
        print("completed")
    }).disposed(by: DisposeBag())
```

### Disposable
```swift
}.disposed(by: DisposeBag())
```

について疑問を持った方も多いと思います。これについて説明します。こう書くことによって購読を解除できます。購読し続けるとその分メモリを圧迫し、アプリのパフォーマンスに影響するので基本的にはメソッドが走った最後に購読を解除します。今まで特に理解せずにこのメソッドを使っていましたが、[オブザーバーパターンから始めるRxSwift入門
](https://qiita.com/k5n/items/17f845a75cce6b737d1e#disposable)から引用するとどうやら

> 管理下にある全ての Disposable の dispose が呼び出されます。

ということのようです。ちなみにDisposableを直訳すると「使い捨て」になります。何と無くイメージできるかなと思います。

### 様々なObservable
一口にObservableと言っても様々なTraitが用意されています。以下にざっくりとまとめます。
#### Subject
SubjectはObserver(購読者)から購読されるだけでなく、**外部に対し任意のストリームイベントを発行できるObservable**です。
<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">RxSwfitのSubjectについての概要<br>・Subjectはイベントを発行できるObservable<br>・PublishSubjectは初期値を持たないかつ中身を取り出せない<br>・BehaviorSubjectは初期値を持ち購読時に流れる中身を取り出せる<br>・RelayはonNextしか流さないSubject</p>&mdash; nosugi (@nosugi1) <a href="https://twitter.com/nosugi1/status/1113862312119525376?ref_src=twsrc%5Etfw">2019年4月4日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

#### Relay
**onNextしか流さないSubject**。uiのbindなど成功が保証されている場合はsubjectよりもこちらを使うのがベターです。
[BehaviorRelayとPublishRelayについてまとめてみた](https://egg-is-world.com/2018/08/11/rxswift-behaviorrelay-publishrelay/)

### single
一回しか流れない
[enter link description here](https://qiita.com/monoqlo/items/7bcec98432389b3b8909)

### driverとsignal
サブスクライブされる時にDriverは一回replayしますがSignalはreplayしないです。
ちなみにRxSwift独自で提供されていたVariableはRxSwift4で廃止になりました。代わりにはBehaviorRelyを使います。

### 補足 Variableについて
RxSwift独自に提供されていたTraitであるVariableは非推奨(deprecated)になりました。ViewModelのプロパティとして使っている方が多くいらっしゃるかと思いますが、代わりにBehaviorSubject、もしくはBehaviorRelayを使うと良いかと思います。