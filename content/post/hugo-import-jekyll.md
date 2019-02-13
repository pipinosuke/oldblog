+++
title = "ブログをJekyllからHugoへ移行した"
date = 2019-02-12T22:01:43+09:00
description = "Hugoが予想以上に良かったのと、どうせ移行するなら早い方が良いなと思い移行しました。移行手順のメモです。参考になれば幸いです。"
category = ["技術"]
tags = ["Hugo","jekyll"]
draft = false
+++

### データをJekyllからインポート
なんとHugoがコマンドを用意してくれています。一発で綺麗に引越し完了！とはなりませんが、楽チン。
``` bash
$ hugo imoprt jekyll path_to_jekyll_root_project path_to_hugo_root_project
```
日付のデータは、移行できないようです。なぜか記事ファイルのメタデータが無視されていたので、手動でコピーし直しました。
終了すると、Hugoから

``` bash
$ git clone https://github.com/spf13/herring-cove.git HOGEHOGE/themes/herring-cove
$ hugo server --theme=herring-cove
```
「このコマンド打ってな〜」と言われると思いますが、なんとその通りにやってもうまくいきません！笑
git cloneしたherring-coveテーマが原因です。おそらくアプデされていないのでしょう。以下のように直します。変更箇所は複数のファイルで存在するので、エディタの機能使うなどして一括変換するといいと思います。

- {{ template "theme/herring-cove/footer.html" . }} を {{ partial "footer.html" }}に
- {{ template "theme/herring-cove/header.html" . }} を {{ partial "header.html" }}に

これで再度hugo serverすると動いてくれるかなと思います。正常に動くのを確認したら、あとは適当に良さげなテーマ選んでやればおkです！

### 終わりに
日付のデータを移行できなかったのが残念ですね。。それ以外は簡単に移行できたので、まぁ良しとします！  
「ブログを書く→git commit→git push」という手順を踏むのが面倒になってきたので
次回は流行り？のCIサービス、[CircleCI](https://circleci.com/)の無料枠を使ってデプロイの自動化を行いたいと思います。こちらもこのブログで手順や所感を[メモ](https://blog.nosugi.tech/post/circleci-hugo/)しておこうとお思います。
