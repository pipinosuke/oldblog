+++
title = "AtomからVSCodeに乗り換えた所感と行なった初期設定"
date = 2019-04-04T12:30:46+09:00
description = "最近VSCodeの新バージョンがリリースされたことを受け試してみた結果、AtomからVSCodeに乗り換えました。少し使っててAtomと比較しての所感と行なった初期設定のメモ。マジで軽くて少し感動しながら書いた。"
tags = ["VSCode","開発環境"]
category = ["技術"]
image = "vscode.png"
draft = false
+++

#### メモリ使用量1/4の衝撃
4/3日にVSCodeの新バージョンがリリースされました。TLを眺めていたところ、このようなツイートを発見。
<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">Gighubを買収した成果がこんなところにも。 VSやはり良いですね <a href="https://t.co/aJkeMwjOk3">https://t.co/aJkeMwjOk3</a></p>&mdash; Kurose (@jkurose777) <a href="https://twitter.com/jkurose777/status/1113315511486504960?ref_src=twsrc%5Etfw">2019年4月3日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
    
#### 乗るしかない、このビックウェーブに・・・！！

### 結論
> Atomより**圧倒的に**VSCode

Vim職人などの例外を除けば、無料で使えるエディタの中で一番ではないのかなと思います。立ち上がりのはやいAtomという感じ。（強い）

#### Atomと比較しての所感
- とにかく**立ち上がりが早い**！！・**サクサク**  
Sublimeほどまではいかないが、軽快。
- Atomと同等もしくはそれ以上に充実したプラグイン  
自分が知らないだけという可能性もありますが、プラグインもatomより優秀な気がする。
- 洗練されたUI  
画面分割やファイル検索など自在にできる。特にGitがみやすい。

### 初期設定など
#### UI設定
- フォントサイズの設定  
デフォルトは12で少しみにくいので15
- テーマ  
[Visual Studio Codeで見やすいテーマファイルのまとめ](https://coliss.com/articles/build-websites/operation/work/best-of-visual-studio-code-themes.html)から自分はAtomOneDarkを選びました。

#### ファイルオートセーブ
デフォルトではオフになっていて使いにくいので、onFocusChange(フォーカスを外した際に保存される)に変更。
[![onFocusChange](https://i.gyazo.com/ddfa389faee5fd4d97b86b706545ab68.png)](https://gyazo.com/ddfa389faee5fd4d97b86b706545ab68)

#### 標準ターミナルではなくiTermを使えるように設定
VSCode内でiTermを使いたいので以下のように設定します。 「UserSettings」→「Features」→「Extension Viewlet」
[![use-iterm](https://i.gyazo.com/3d57e01e4e5326b67564e4a848259b7f.png)](https://gyazo.com/3d57e01e4e5326b67564e4a848259b7f)

ちなみにMacでは`Control+Shift+@`でterminalを起動できます。

#### プラグイン
[VSCodeのオススメ拡張機能 24 選 (とTipsをいくつか)](https://t.co/2o7zVxPKwX)から気に入ったものをインストールしました。他にもあれば教えて欲しいです。

#### デバッグ
Webの開発行う人は入れておくとよいかと思います。
- [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)
- [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)

参考: [Visual Studio Code でフロントエンドの開発環境を構築してデバッグする](https://qiita.com/C3REVE/items/273646ad028e98758e70)

#### ターミナルからファイルやプロジェクトをVSCodeで開く
```bash
code .
code filename
```
で開けるように設定。[ターミナルからVisual Studio Codeを起動する方法【公式の方法】](https://qiita.com/naru0504/items/c2ed8869ffbf7682cf5c)の通りです。


##### 最後に
さようならAtom。