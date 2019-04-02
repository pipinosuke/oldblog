+++
title = "AVFoundation覚書 iOSアプリでバーコードリーダーを実装する"
date = 2019-04-03T01:50:01+09:00
description = "iOSアプリで画像や動画の加工を行う際に必要なAVFoundationというライブラリを使ってバーコード読み取り機能を実装した際のメモ。カメラアプリは作ったことがなく色々面倒くさそうな印象でしたが、ブログのおかげでなんとかできました。先人達に感謝。"
tags = ["iOS","AVFoundation","Swift"]
category = ["技術"]
image = "swift.jpg"
draft = false
+++

### 挙動
> 読み取ったバーコードの値でGoogleBooksAPIを叩いて本のタイトル、作者などの情報を取得する。

##### 参考URL
- [iOSでバーコードを読み取る](https://swiswiswift.com/barcode/)
- [AVFoundation(AVCaptureMetadataOutput)でバーコードリーダーを作ってみた](https://dev.classmethod.jp/smartphone/ios-avfoundation-avcapturemetadataoutput-ean13-ean8/)

### 実装
#### 事前準備
**この手順を踏まずにカメラを起動しようとすると強制終了するので注意！**。Xcodeの`Info.plist`に`Privacy - Camera Usage Description`を追加します。カメラを使用する目的を値として入力します。
[![Info.plist](https://i.gyazo.com/10cd0c5bf9a1a2fe89002096ee637495.png)](https://gyazo.com/10cd0c5bf9a1a2fe89002096ee637495)

- 参考: [iOS10ではカメラアクセスなどの目的を明示しないと強制終了する](https://qiita.com/Takumi_Mori/items/f53c6eec1676d3df59dc)



### AVCaptureMetadataOutputObjectsDelegate
