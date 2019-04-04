+++
title = "AtomからVSCodeに乗り換えた所感と行なった初期設定"
date = 2019-04-04T12:30:46+09:00
description = "最近VSCodeの新バージョンがリリースされたことを受け試してみた結果、AtomからVSCodeに乗り換えました。少し使っててAtomと比較しての所感と行なった初期設定のメモ。マジで軽くて少し感動しながら書いた。"
tags = ["VSCode","開発環境"]
category = ["その他"]
image = "vscode.png"
draft = false
+++

#### VSCode新バージョンがリリース、試してみる
4/3日にVSCodeの新バージョンがリリースされました。TLを眺めていたところ、このようなツイートを発見。
<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">Gighubを買収した成果がこんなところにも。 VSやはり良いですね <a href="https://t.co/aJkeMwjOk3">https://t.co/aJkeMwjOk3</a></p>&mdash; Kurose (@jkurose777) <a href="https://twitter.com/jkurose777/status/1113315511486504960?ref_src=twsrc%5Etfw">2019年4月3日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

なんとメモリ使用量1/4・・・。乗るしかないこのビックウェーブに・・・ということで試してみた。

### 結論
> AtomよりVSCode

Vim職人などの例外を除けば、無料で使えるエディタの中で一番ではないのかなと思います。立ち上がりのはやいAtomという感じ。（強い）

#### Atomと比較しての所感
- とにかく**立ち上がりが早い**！！・**サクサク**  
Sublimeほどまではいかないが、軽快。
- Atomと同等もしくはそれ以上に充実したプラグイン  
自分が知らないだけという可能性もありますが、プラグインもatomより優秀な気がする。
- 洗練されたUI  
画面分割やファイル検索など自在にできる。特にGitがみやすい。


<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">インストールしてみた結果Atomより余裕でサクサク動くし、プラグインも充実してるな。乗り換えることにした。 <a href="https://twitter.com/hashtag/VSCode?src=hash&amp;ref_src=twsrc%5Etfw">#VSCode</a><a href="https://t.co/2o7zVxPKwX">https://t.co/2o7zVxPKwX</a></p>&mdash; nosugi (@nosugi1) <a href="https://twitter.com/nosugi1/status/1113628550181965824?ref_src=twsrc%5Etfw">2019年4月4日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>


### 初期設定など
#### プラグイン
[VSCodeのオススメ拡張機能 24 選 (とTipsをいくつか)](https://t.co/2o7zVxPKwX)から気に入ったものをインストールしました。他にもあれば教えて欲しいです。

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

#### ターミナルからファイルやプロジェクトをVSCodeで開く
```bash
code .
code filename
```
で開けるように設定。[ターミナルからVisual Studio Codeを起動する方法【公式の方法】](https://qiita.com/naru0504/items/c2ed8869ffbf7682cf5c)の通りです。


##### 最後に
さようならAtom。