+++
title = "GASで任意のTwitterアカウントにDMを送る"
date = 2019-05-31T01:15:00+09:00
description = "TwitterBotの開発で必要だったので、実際のスクリプトを紹介します。"
tags = ["GoogleAppsScript","Twitter"]
category = ["技術"]
image = "gas.png"
draft = false
+++
### 前準備
GASからTwitterのアカウントを操作するためには開発者用アカウントの取得などの前準備が必要です。[こちらの記事](https://blog.nosugi.tech/tech/gas-twitter/)を参考にしてください。

## DMする
DMのAPIを叩くためには任意のアカウントに紐づくuserIDが必要です。因みに**screenNameとuserIdは違います**。screen_nameはメンションするときなどにおなじみの「@」から始まる文字列です。それに対してuserIdは数字だけの文字列です。詳しくは[TwitterAPIの公式ドキュメントを](https://developer.twitter.com/en/docs/accounts-and-users/follow-search-get-users/api-reference/get-users-lookup)参照してください。  
今回紹介するgasのスクリプトはscreen_nameからuser_idを取得し、そのuser_id宛にDMを送信するという挙動となっています。

### 1. screen_nameからuser_idを取得する
``` javascript
function getUserId() {
  var service = twitter.getService();
  var screenName = "screen_name" //@screen_nameさんにDMを送る場合
  var requestURL = "https://api.twitter.com/1.1/users/lookup.json?screen_name=" + screenName
  var response = service.fetch(requestURL, {
    method: "get",
    contentType: 'application/json'
  });
  var o = JSON.parse(response.getContentText());
  var user_id = o[0].id_str //配列で帰ってくるのでそのうちの最初の要素のuser_idを取得する
  
  newDirectMessage(user_id) //後述します
}
```

### 2. user_idに紐づくTwitterアカウント対してDMを送る
```javascript
function newDirectMessage(user_id){
  try{
    var service = twitter.getService();
    var payload = JSON.stringify({
      event: {
        type: 'message_create',
        message_create: {
          target: {
            recipient_id: String(user_id)  //先ほど取得したuserId
          },
          message_data: { text: "こんにちは" }  //メッセージ内容を定義
        }
      }
    });
    var response = service.fetch('https://api.twitter.com/1.1/direct_messages/events/new.json',{
      method: 'POST',
      contentType: 'application/json',
      payload: payload
    });
    return response;
  } catch(e) {
    Logger.log('Exception:'+e);
  }
}
```
