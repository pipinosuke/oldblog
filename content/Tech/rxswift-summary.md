+++
draft = true
image = "rxswift.jpg"
tags = ["iOS","RxSwift","リアクティブプログラミング"]
category = ["技術"]
date = "2019-04-08T03:06:17+09:00"
title = "RxSwift入門 Observableの概要まとめ"
description = ""
+++
### 入門
- [リアクティブプログラミングとRxJavaの概要](https://codezine.jp/article/detail/9570)
- [オブザーバーパターンから始めるRxSwift入門](https://qiita.com/k5n/items/17f845a75cce6b737d1e)

まずは上の記事でリアクティブプログラミングのノリを理解します。特に6pのマーブルダイヤグラムの図が理解に役立ちます。下の記事は実際のコードを交えた解説です。具体的にどのような実装を行えば良いのかがわかります。

> ありとあらゆるものをストリームとして扱うことが可能です

データのbindや、非同期処理を簡単に描けるのがメリットです。若干学習コストは高いかなと思いますが、マスターできれば文字通りなんでもできます。

### Observable
Observableはストリームを流しsubscribe(購読)することが可能になります。名前だけでも覚えて帰ってください。一口にObservableと言っても様々なTraitが用意されています。以下にざっくりとまとめます。
<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">Observableが通知するイベントは以下の3種類。subscribe(購読)することで、それぞれのタイミングでクロージャ内のメソッドを呼ぶことが出来る。<br><br>･onNext: 通常のイベントが発生した時<br>･onError: エラーが発生になった時<br>･onComplete: イベントが完了した時</p>&mdash; nosugi (@nosugi1) <a href="https://twitter.com/nosugi1/status/1114825476126662656?ref_src=twsrc%5Etfw">2019年4月7日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


言い換えると何かが起こった時の挙動をクロージャの中にに記述しておけば良いということです。
```swift
//Observable<T>で定義します

```

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

### 具体的な実装
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

### 補足 Variableについて
RxSwift独自に提供されていたTraitであるVariableは非推奨(deprecated)になりました。ObserveすることができるObservableで、非常に紛らわしいです。ObserverとObservableの関係がこんがらがってしまう人も多いかと思います。 前述のTraitがあればVariableを使わずとも事足りるので、わざわざ使わないのが良いようです。
