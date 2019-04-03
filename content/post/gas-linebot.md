+++
title = "GAS(GoogleAppsScript)とTypeScriptを使って簡単なLinebotを作る"
date = 2019-04-04T04:08:49+09:00
description = "TypeScriptとGASを使ってLineBotを作ります。今までカオスなJavascriptを敬遠していましたがTypeScriptによって型による平穏がもたらされたかつ、GASがTypeScriptに対応したと聞きつけたと言うことで手を出してみました。"
tags = ["GoogleAppsScript","LineBot"]
category = ["技術"]
image = "linebot-gas.png"
draft = false
+++
個人的にはWebhook辺りでハマりどころが結構ありました。Qiitaで30分でできたとか書いてる人いるけど絶対嘘！！

##### 使うもの
- GAS
- clasp
- TypeScript

### ローカル開発環境の構築
補完とか使えるので個人的にはローカルで開発することをお勧めします。もちろんGASのエディタを使っても良いです。まずは[https://script.google.com/home/usersettings](https://script.google.com/home/usersettings)からGoogleAppsScriptAPIをオンにしておきます。
[![GASAPIの有効化](https://i.gyazo.com/03f659b5a167d80852448fdcec0deca9.png)](https://gyazo.com/03f659b5a167d80852448fdcec0deca9)

#### プロジェクト作成
``` bash
mkdir gas-project //プロジェクトファイル作成
cd gas-project
npm init -y
npm i -S @google/clasp //claspについては後述
npm i -S @types/google-apps-script @types/node
clasp create --title "GcarenderBot" --rootDir ./src
clasp pull
```

説明することが2つあるので軽く触れます。
##### 1. clasp
以下のようにGoogle製のCIライブラリであるclaspを使うことでプロジェクトファイルをgitみたいな感じで扱うことができます。`clasp create`でGASにプロジェクトが登録されます。その際--rootDirオプションでディレクトリを指定しましょう。
```bash
clasp create --title "gas-project" --rootDir ./src
clasp pull //GASからpull
clasp push //GASにpush
```

##### 2. Typescriptへの対応
```bash
npm i -S @types/google-apps-script @types/node
```
地味に嬉しい人多いのでは??`clasp push`すれば`*.ts`から`*.js`へコンパイルはGASが勝手にやってくれます。強い。

### 実装
ローカル開発環境を整えたところで、簡単な実装します。試しにおうむ返しするBotを作ります。LineBotでできることと仕様については[LineMessagingAPIリファレンス](https://developers.line.biz/ja/docs/messaging-api/overview/)をチェケラ
#### コード
コードは下記のリンクから拝借したしたものです。ファイル名は適当に`code.ts`とかで良いと思います。

- [【LINE Botの作り方】Messaging API × GAS(Google Apps Script)でおうむ返しボットを作成する](https://www.takeiho.com/messaging-api-gas)

```javascript
var channel_access_token = "各自取得"

function doPost(e) {
  var events = JSON.parse(e.postData.contents).events;
  events.forEach(function(event) {
    if(event.type == "message") {
      reply(event);
    } else if(event.type == "follow") {
      follow(event);
    } else if(event.type == "unfollow") {
      unFollow(event);
    }
 });
}

// 入力されたメッセージをおうむ返し
function reply(e) {
  var message = {
    "replyToken" : e.replyToken,
    "messages" : [
      {
        "type" : "text",
        "text" : ((e.message.type=="text") ? e.message.text : "Text以外は返せません・・・")
      }
    ]
  };
  var replyData = {
    "method" : "post",
    "headers" : {
      "Content-Type" : "application/json",
      "Authorization" : "Bearer " + channel_access_token
    },
    "payload" : JSON.stringify(message)
  };
  UrlFetchApp.fetch("https://api.line.me/v2/bot/message/reply", replyData);
}

/* フォローされた時の処理 */
function follow(e) {

}

/* アンフォローされた時の処理 */
function unFollow(e){

}
```

アクセストークンは[LineDeveloppers](https://developers.line.biz/console/)の設定から各自取得しましょう。
[![Image from Gyazo](https://i.gyazo.com/0c59246484f7269d20d3d3504c367825.png)](https://gyazo.com/0c59246484f7269d20d3d3504c367825)


#### GASアプリケーションの公開
プロジェクトを`clasp push`したらGASアプリケーションとして公開します。公開した際のURLはWebhook設定の時に使います。
```bash
clasp open
```

でGASを開いて「公開」→「ウェブアプリケーションとして公開」を選択します。以下の点に注意してください。

- 初回の公開のみ「プロジェクトバージョン」→「New」にする
- 「アプリケーションにアクセスできるユーザー」→「全員(匿名ユーザーを含む)」

#### Webhookの設定
続いて[LineDeveloppers](https://developers.line.biz/console/)からBot設定画面を開きます。

- 「Line@機能の自動応答」→「オフ」にします
[![Image from Gyazo](https://i.gyazo.com/f1a7c955bd235f4bc9f786f0011d4027.png)](https://gyazo.com/f1a7c955bd235f4bc9f786f0011d4027)
- 「Webhook送信」→利用する
- 「WebhookUrl」に公開したGASアプリケーションのURLを登録

接続確認ができれば実装完了。おうむ返ししてくれましたかと思います。次回はもう少し発展させたものを作ります。

### 参考URL
毎回言ってる気がするけど先人達に感謝。

- [GAS ビギナーが GAS を使いこなすために知るべきこと 10 選](https://qiita.com/tanabee/items/2c51681396fe12b6a0e4)
- [Google Apps ScriptをTypeScriptで実装する(clasp/TSLint/Prettier)](https://budougumi0617.github.io/2019/01/16/develop-google-apps-script-by-typescript/)
- [【LINE Botの作り方】Messaging API × GAS(Google Apps Script)でおうむ返しボットを作成する](https://www.takeiho.com/messaging-api-gas)
- [clasp が Typescript をサポートした！](https://qiita.com/HeRo/items/f2ce057c6b1456e896ad)
