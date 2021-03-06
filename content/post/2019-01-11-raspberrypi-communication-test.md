---
layout: post
title:  RaspberryPIとSoracomAirのSimを使って通信してみる
category: ["技術"]
image: programming.jpeg
description: "必要最低限かつコストをなるべく抑えた構成で行いました。"
date: 2019-01-14T09:03:48+09:00
tags: ["RaspberryPi","SoracomAir"]
---

[ロードヒーティングのOn/Off切り替えが面倒なので、スマホから操作したいプロジェクト](https://blog.nosugi.tech/programming-roadheating/)の実現に向けて、まずはRasPiとSoracomAirで通信できるか試してみる。

## 目標
> RasPiで通信する

## 用意したもの
- [RaspberryPI ZERO WH](https://www.switch-science.com/catalog/3646/)
<a href="https://www.amazon.co.jp/Raspberry-Pi-ZERO-Wi-Fi-Bluetooth%E3%80%81GPIO%E3%83%94%E3%83%B3%E3%83%98%E3%83%83%E3%83%80%E3%83%BC%E5%AE%9F%E8%A3%85%E6%B8%88%E3%81%BF%E3%83%A2%E3%83%87%E3%83%AB%EF%BC%89/dp/B07L1GFNH6/ref=as_li_ss_il?ie=UTF8&qid=1547126823&sr=8-3-spons&keywords=raspberry+pi+zero+wh&psc=1&linkCode=li2&tag=pipinosuke04-22&linkId=64301b909de7ceeb6e02de9fbfecceb3&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B07L1GFNH6&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=pipinosuke04-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=pipinosuke04-22&language=ja_JP&l=li2&o=9&a=B07L1GFNH6" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />
- [Amazon Basis miniHDMI-HDMIケーブル](https://amzn.to/2D39KEB)
<a href="https://www.amazon.co.jp/Amazon%E3%83%99%E3%83%BC%E3%82%B7%E3%83%83%E3%82%AF-%E3%83%8F%E3%82%A4%E3%82%B9%E3%83%94%E3%83%BC%E3%83%89HDMI%E3%82%B1%E3%83%BC%E3%83%96%E3%83%AB-1-8m-%E3%82%BF%E3%82%A4%E3%83%97A%E3%82%AA%E3%82%B9-%E3%83%9F%E3%83%8B%E3%82%BF%E3%82%A4%E3%83%97C%E3%82%AA%E3%82%B9/dp/B014I8UEGY/ref=as_li_ss_il?ie=UTF8&qid=1547126678&sr=8-1-fkmr0&keywords=basis+hdmi+minihdmi&linkCode=li2&tag=pipinosuke04-22&linkId=f90fd5faead2a0aafdb41371c54d0b24&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B014I8UEGY&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=pipinosuke04-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=pipinosuke04-22&language=ja_JP&l=li2&o=9&a=B014I8UEGY" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />
- [USB変換アダプタ](https://amzn.to/2FmL37v)
<a href="https://www.amazon.co.jp/UGREEN-OTG%E3%82%B1%E3%83%BC%E3%83%96%E3%83%AB-USB%E3%83%9B%E3%82%B9%E3%83%88%E5%A4%89%E6%8F%9B%E3%82%A2%E3%83%80%E3%83%97%E3%82%BF-micro-%E3%82%AA%E3%82%B9-USB/dp/B00LN3LQKQ/ref=as_li_ss_il?ie=UTF8&qid=1547126723&sr=8-9&keywords=usb+b+%E5%A4%89%E6%8F%9B&linkCode=li2&tag=pipinosuke04-22&linkId=3040ed4adbf4c7ab732eba75a297ce46&language=ja_JP" target="_blank"><img border="0" src="//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=B00LN3LQKQ&Format=_SL160_&ID=AsinImage&MarketPlace=JP&ServiceVersion=20070822&WS=1&tag=pipinosuke04-22&language=ja_JP" ></a><img src="https://ir-jp.amazon-adsystem.com/e/ir?t=pipinosuke04-22&language=ja_JP&l=li2&o=9&a=B00LN3LQKQ" width="1" height="1" border="0" alt="" style="border:none !important; margin:0px !important;" />
- SDカード(8GB以上必要)

### やること
- OSインストール
- wvdialで通信

### OSインストール
#### フォーマット
まずはじめにSDカードのフォーマットを行なっておきましょう。RaspberryPiが読み込めるフォーマット(FAT32)にしておく必要があります。よくわからないという方は[SDCardFormatter](https://www.sdcard.org/jp/downloads/formatter_4/)というソフトでフォーマットしておけばおkです。Raspbianのインストールには4GB以上必要なので注意です。(一敗)
#### インストール
RaspberryPiに[raspbian](https://www.raspberrypi.org/downloads/raspbian/)というOSをインストールします。
1. [こちらのリンク](https://www.raspberrypi.org/downloads/noobs/)からダウンロードして解凍したファイル群をSDカードのルートにコピー
[![Image from Gyazo](https://i.gyazo.com/6d74100e15cd47efd7742e8cfb4483d0.png)](https://gyazo.com/6d74100e15cd47efd7742e8cfb4483d0)  
2. SDカードをRaspberryPiに挿入し、電源コードを接続すると起動するはずです  
3. メニューが表示されるのでRasbianをenter選択し、「i」を押すとインストールが始まります。  

#### 通信してみる
##### 1.USBデバイスの確認
``` bash
$ lsusb
としてUSB機器として認識しているかをまず確認します。「LG Electronics, Inc. Ally/Optimus One」という名称でした。
```
##### 2.確認できたら、ttyデバイスとして認識させます。
``` bash
sudo modprobe usbserial vendor=VENDORID product=PRODUCTID
```
##### 3.必要なソフトのインストールと設定
``` bash
sudo apt-get install wvdial
sudo vi /etc/wvdial.conf
```
内容は以下です。コピペでおk。
``` vim
[Dialer Defaults]
Init1 = ATZ
Init2 = ATQ0 V1 E1 S0=0 &C1 &D2 +FCLASS=0
Init3 = AT+CGDCONT=1,"IP","soracom.io"
Dial Attempts = 3
Modem Type = Analog Modem
Dial Command = ATD
Stupid Mode = yes
Baud = 460800
New PPPD = yes
Modem = /dev/ttyUSB2 <---他のUSBデバイスがあると"USB3"などに変える必要があるので注意。
ISDN = 0
APN = soracom.io
Phone = *99***1#
Username = sora
Password = sora
Carrier Check = no
Auto DNS = 1
Check Def Route = 1
```

##### 4.通信してみる
``` bash
$ sudo wvdial
~~~~~~~
connect
```

と出ればおk！！

## 最後に
なんか大吉でした。笑 今年はいい年になりそうです。
<blockquote class="twitter-tweet" data-lang="ja"><p lang="ja" dir="ltr">スイッチサイエンスで買ったラズパイ届いた。相当小さくてびっくり<br>あとなんか大吉だったよ。笑 <a href="https://t.co/fwYDQnljma">pic.twitter.com/fwYDQnljma</a></p>&mdash; とり (@nosugi1) <a href="https://twitter.com/nosugi1/status/1083327478662217729?ref_src=twsrc%5Etfw">2019年1月10日</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
