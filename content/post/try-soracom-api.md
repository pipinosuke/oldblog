+++
title = "SoracomAPIを試してみる"
date = 2019-02-17T15:06:08+09:00
description = "タイトル通りですが、SoracomAPIを試してみました。"
tags = ["SoracomAir","API"]
category = ["技術"]
image = "soracom.jpg"
draft = false
+++

### SoracomAirのSimを触ってみる
#### SoracomSimの登録
[soracomコンソール](https://console.soracom.io/)から登録できます
[![Image from Gyazo](https://i.gyazo.com/042d9df4778b10b6a2679b70534194a5.png)](https://gyazo.com/042d9df4778b10b6a2679b70534194a5)

#### 料金体系
[公式サイト](https://soracom.jp/pricing/)
[![Image from Gyazo](https://i.gyazo.com/7f4cca4f19fb379db83ede56063940ac.png)](https://gyazo.com/7f4cca4f19fb379db83ede56063940ac)

#### APIを叩いてみる
試しに登録したSimカードの一覧を取得するAPIを叩いてみます。

1. APIキーとトークンの取得
[こちらのリンク](https://dev.soracom.io/jp/docs/api/)から二種類あるので好きなやり方で認証・発行してください。
[![Image from Gyazo](https://i.gyazo.com/ed8209b0509d5c1177e442ce4c288e64.png)](https://gyazo.com/ed8209b0509d5c1177e442ce4c288e64)
2. APIを叩いてみる
  - リクエストURL
	   - https://api.soracom.io/v1/subscribers
  - リクエストヘッダ
		 - X-Soracom-Token=TOKEN
     - X-Soracom-API-Key=APIKEY
3. レスポンス

``` json
[
  {
    "imsi": "440103198207751",
    "msisdn": "812016084496",
    "ipAddress": "10.245.237.68",
    "operatorId": "OP0061660828",
    "apn": "soracom.io",
    "type": "s1.standard",
    "groupId": null,
    "createdAt": 1545836470247,
    "lastModifiedAt": 1550380631731,
    "expiredAt": null,
    "registeredTime": 1549618116968,
    "expiryAction": null,
    "terminationEnabled": false,
    "status": "active",
    "tags": {
      "name": "mySim"
    },
    "sessionStatus": {
      "lastUpdatedAt": 1550380631731,
      "imei": "353168044272803",
      "location": null,
      "cell": {
        "radioType": "lte",
        "mcc": 440,
        "mnc": 10,
        "tac": 12390,
        "eci": 185881861
      },
      "ueIpAddress": "10.245.237.68",
      "dnsServers": [
        "100.127.0.53",
        "100.127.1.53"
      ],
      "online": true
    },
    "imeiLock": null,
    "speedClass": "s1.standard",
    "moduleType": "mini",
    "plan": 0,
    "iccid": "8981100005601355721",
    "serialNumber": "DN0605601355720",
    "subscription": "plan-D",
    "createdTime": 1545836470247,
    "expiryTime": null,
    "lastModifiedTime": 1550380631731
  }
]
```
