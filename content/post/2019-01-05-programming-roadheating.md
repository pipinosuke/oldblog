---
layout: post
title:  ロードヒーティングのスイッチをスマホから遠隔操作するプロジェクト
tags: ["技術"]
image: programming.jpeg
description: "このプロジェクトは夜の寒い中外に出てロードヒーティング の電源を手動で切り替えなければならないことへの怒りから生まれました"
date: 2019-01-05T09:03:48+09:00
---
## 趣旨
> 下記の問題を、ロードヒーティングのOn/Offをスマホから操作できるようにすることで解決したい。

## 経緯
僕が住む北海道などの積雪地方ではロードヒーティングという設備が利用されている。本州在住の方には馴染みがないかもしれないがこの装置は、文字通り床を温めて雪を溶かしてくれるものだ。

このロードヒーティングを稼働させるには「自動」と「手動」の2タイプが存在するのだが、「自動」の場合稼働させるとアイドリングで結構電気代を持っていかれるし雨と雪を勘違いする誤動作も多いためコスパが悪い。そのため我が家では「手動」で行っている。
「手動」で稼働させた場合スイッチは物理的に切り替えなければならないため、外出時に突然雪が降ってきて家のロードヒーティングのスイッチを入れておくみたいなことはできない。
これが自分の家だけではまだいいのだが、両親が他に所有している近くのアパートも同じ仕組みで動いておりわざわざアパートまで出向きスイッチを入れなければならず、この点が非常にネックになっている。というか非常に面倒臭い。この問題をRaspberryPiを用いてスマホから操作できるようにすることで解決できないかと思い、考えてみた。

## 必要なもの
注）商品のリンクはAMAZONですが、金額は実際に購入した金額です。買えるものはメルカリで買っているので、リンク先の値段と表の値段が異なる場合があります。

|品目|説明|値段|
|---|---|---|
|[soracom airカード](https://www.switch-science.com/catalog/2878/)|ネットとの通信に必要。IoT用のものを使用。|1200円|
|usbドングル|RasPiとSimを接続するためのもの。[DOCOMO L-02C](https://amzn.to/2R8teQC)|1680円|
|[Raspberry Pi WH](https://www.switch-science.com/catalog/3646/)|基盤（PCで言う所のマザーボードのようなもの）&CPU|1800円|
|SwitchBot|RaspberryPiと繋いでスイッチングできるモジュール|---|
|sdカード|Raspiに刺してOSをインストールするもの|すでに持っている|
|RasPi用電源コード|スマホのような充電方法だと、電力不足になるらしい|---|

### Rasberry Piの種類について簡単なメモ
##### 標準サイズ
- Raspberry Pi 3
CPU 64bit。WifiとBluetoothが標準搭載。
- Raspberry Pi 2 Model B
RasPi 3に次ぐスペック。
##### 小型版
- Raspberry Pi Zero
初心者用のシンプルなモデル。5$と非常に安価。
- Raspberry Pi Zero W
RasPi ZeroにWifiとBluetoothが標準搭載されたモデル。
- Raspberry Pi Zero WH
Raspberry Pi Zero Wにピンヘッダを実装したもの。今回はこれを実験用として使う。
##### エクステンション
- 3GPi
simスロットが搭載されている。RasPi3と組み合わせて使用する。2万円代と少し値段が高い・・・


参考: [購入可能なラズパイ比較(2018年5月)Pi 3 Model B/Pi Zero/Pi Zero W/Pi2 Model B/Pi Model B+](https://littlewing.hatenablog.com/entry/2016/01/26/135043)

## 要素分解
- 通信
	- スマホ
サーバーにリクエストを送るアプリを作る
	- サーバー
スマホからのリクエストを受けて、soracomAPIを叩く。とりあえずrailsで実装する。
	- ロードヒーティング
soracomAPiを受けてなんやかんや
- スイッチング
ロードヒーティングスイッチの仕組みがわからないのでまだなんとも言えない。

## 最後に
親から聞いた話なのだが驚いたことに、、テクノロジーが発達した今もなおロードヒーティングのスイッチ切り替えはもなお人力で行なうのが主流だそうだ。これは予想だが、業界内がソフトウェアの知識に疎い、かつ市場規模的な問題とかで事業化したところでペイしないため依然としてアナログ的なやり方のままここまで来たのだろうと思われる。
複数の物件を持つ不動産オーナーはわざわざ業者を雇って人力でスイッチの切り替えを行なっているらしい。
仮にスマホからの操作が実現可能ならば、我が家の他にも一定数いるであろう上記のような不動産オーナーの役立つものと考えられる。

## 参考URL
### Raspiについて
- [Raspberry Pi Zeroに必要な物リスト](https://www.g104robo.com/entry/shopping-list-for-raspberry-pi-zero#USB%E3%83%8F%E3%83%96)
### 通信
- [SORACOM公式 各種デバイスで SORACOM Air を使用する ](https://dev.soracom.io/jp/start/device_setting/)
- [RASPBERRY PIをSORACOM SIMでインターネットに繋ぐ](https://ermine.co.jp/raspberry-pi-soracom-sim/)
### スイッチング
- [Raspberry Pi 3でpythonを使いスイッチ制御でLEDを光らせる！](https://qiita.com/RyosukeKamei/items/5c19819a2153b0a997b6)
- [Switch Botについて](https://yuki-no-yabo.com/switch-bot/)
- [物理的にスイッチ操作するIoTデバイス「SwitchBot」をラズパイ経由でIFTTT連携し全自動化してみた](https://gigazine.net/news/20170907-switchbot-raspberry-pi/)
- [RaspberryPiにスイッチを付けよう](https://deathmarch.net/archives/193)
### 類似サービス
- [ゆりもっと](https://www.ecomott.co.jp/service/yurimott.html)
