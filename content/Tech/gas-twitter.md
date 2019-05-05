+++
title = "GASでGoogleカレンダーの週ごとの予定を定期的にツイートする"
date = 2019-05-05T15:41:50+09:00
description = "今回はGカレンダーの予定を取得し、週一でツイートするBotを開発します。"
tags = ["GoogleAppsScript","Twitter"]
category = ["技術"]
image = "gas.png"
draft = false
+++
### 参考
- [GASで複数のGoogleカレンダーの予定を1週間取得する](https://cyuraharuto.com/gas-googlecalender-week-get/)
- [Google Apps Script (GAS) でTwitterへ投稿するだけの機能を実装してみる](https://qiita.com/akkey2475/items/ad190a507b4a7b7dc17c)
- [ Google Apps ScriptでTwitter botを作ってみる](https://www.npca.jp/magazine/2019/sashiming.html)

## 手順
### 1. アクセストークンとシークレットキーを取得する
[ Google Apps ScriptでTwitter botを作ってみる](https://www.npca.jp/magazine/2019/sashiming.html)が圧倒的に詳しく解説されています。丸投げだと身もふたもないので、一応説明します。笑
[https://developer.twitter.com/en/apps](https://developer.twitter.com/en/apps)から`create an app`を選択。
![enter image description here](https://www.npca.jp/magazine/2019/images/sashiming/17.png)  
![enter image description here](https://www.npca.jp/magazine/2019/images/sashiming/18.png)  
![enter image description here](https://www.npca.jp/magazine/2019/images/sashiming/19.png)  
画像引用: [ Google Apps ScriptでTwitter botを作ってみる](https://www.npca.jp/magazine/2019/sashiming.html)

#### Callback URLs
`https://script.google.com/macros/d/[GASのスクリプトID]/usercallback`とします。GASのScriptIDはGASエディタのプロパティから調べられます。

#### 登録後
- APIKey
- APISecretKey

上記の二つの値を使用します。「Keys&Tokens」の「ConsumerKey」の欄にあります。

### 2. GASプロジェクトの作成とTweet用のライブラリを導入する
GASプロジェクトの作成方法は割愛します。作成後Twitter用のライブラリをインポートします。「リソース」→「ライブラリ」から以下のプロジェクトキーで追加できます。  
`1rgo8rXsxi1DxI_5Xgo_t3irTw1Y5cxl2mGSkbozKsSXf2E_KBBPC3xTF`

### 3. Gカレンダーの一週間の予定を取得する
ほぼ[GASで複数のGoogleカレンダーの予定を1週間取得する](https://cyuraharuto.com/gas-googlecalender-week-get/)の通り。for文を用いて一週間それぞれの日付と終日イベントのタイトルの二つを配列として取得し、eventsForWeekにappendしています（最終的には二次配列の形で保存されます）。ちなみにgetDateやgetMonthした値に数字を足して処理しているのは、javascriptのDate型の仕様のため。  
参考: [JavaScript の Date は罠が多すぎる](https://qiita.com/labocho/items/5fbaa0491b67221419b4)

```javascript
function getEventsForWeek() {
    var calender = CalendarApp.getDefaultCalendar;
    var dateNow = new Date();
    var date = new Date();
    var eventsForWeek = [];
    
    for (var j = 2; j < 9 ; j++ ){
        date.setDate(dateNow.getDate() + j);
        date.setMonth(dateNow.getMonth());
        var dateStr = (date.getMonth() + 1).toString() + "/" + date.getDate().toString()
        var events = calender.getEventsForDay(date);
        if (events.length > 0 && events.slice(0, 1)[0].isAllDayEvent()) {
            eventsForWeek.push([dateStr,events.slice(0, 1)[0].getTitle()])
        } else {
            eventsForWeek.push([dateStr,"空き"])//終日イベントが登録されていない時
        }
    }
    Logger.log(eventsForWeek);
}
```

### 4. Tweetさせる
ちなみにTwitterAPIを用いてできることは、ツイート以外にもそこそこあります。
#### 認証
初回のみツイッタアカウントへの認証作業が必要です。それ以降は認証する必要はありません。以下のように記述し、authorizeメソッドを走らせたあと、CMD + Returnでログを確認します。urlが記述されているはずなので、そのURLにアクセスします。
```javascript
var twitter = TwitterWebService.getInstance(
  "手順1で取得したAPIKey",
  "手順1で取得したAPISecretKey "
);

// 認証
function authorize() {
  twitter.authorize();
}

// 認証解除
function reset() {
  twitter.reset();
}

// 認証後のコールバック
function authCallback(request) {
  return twitter.authCallback(request);
}
```

#### ツイート
以下のように実装しました。`tweetStr`をツイートするようになっています。
```javascript
function Tweet(tweetStr) {
  var service  = twitter.getService();
  var response = service.fetch("https://api.twitter.com/1.1/statuses/update.json", {
    method: "post",
    payload: { status: tweetStr }
  });
}
```

### 5. cron処理
cron的なことをさせる方法です。超簡単。
[![Image from Gyazo](https://i.gyazo.com/8e18e415fda911d1f2df10548cd01549.png)](https://gyazo.com/8e18e415fda911d1f2df10548cd01549)

こんな感じで選べます。
[![Image from Gyazo](https://i.gyazo.com/1615b9141fa0d9ec65df41550855a737.png)](https://gyazo.com/1615b9141fa0d9ec65df41550855a737)
