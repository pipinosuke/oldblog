---
layout: post
title: GithubPagesのサイトで、Twitterカードの画像だけが反映されない
tags: ["技術"]
description: "TitleやDescriptionはうまくいっているのに画像だけ反映されないということがありました。本来のドメインと異なるドメインのパスを指定していると起こるようです。"
image: programming.jpeg
date: 2018-12-30T09:03:48+09:00
---

----------

適当に調べたところ、このような記事を発見。参考にしたら治りました。
[Twitterカードの設定は正しいのに画像が表示されない原因](https://blog.apar.jp/web/9844/#_CDN)
## やったこと
``` html
  <meta name="twitter:image" content="https://raw.githubusercontent.com/myusername/blog/master/images/thumbs/{{ page.image }}" />
```
のようにカスタムドメインではなく、githubcontent.comのURLで指定するとうまくいきました。

## おまけ
参考にしてくださいmm
- ツイッターカードのメタデータ一覧
[![Image from Gyazo](https://i.gyazo.com/5ddc2411c01aa686ca3a60ed48ff7e08.png)](https://gyazo.com/5ddc2411c01aa686ca3a60ed48ff7e08)
引用: [ツイートにページ情報を表示する「Twitterカード（Twitter Cards）」を設定してみた](https://gyazo.com/5ddc2411c01aa686ca3a60ed48ff7e08)
