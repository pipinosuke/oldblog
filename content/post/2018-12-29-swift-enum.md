---
layout: post
title: SwiftでEnumのおさらい
tags: ["技術"]
image: swift.png
description: "Enum自体についての解説は割愛します。
SwiftのEnum型の要諦を適当にまとめます。"
date: 2018-12-29T09:03:48+09:00
---


----------
## 要素数の取得
- [CaseIterable](https://developer.apple.com/documentation/swift/caseiterable)プロトコルの準拠(Swift4.2~)
``` swift
Enum.allCases.count
```

## 値の取得・逆引き
値型Enumの時はrawValueを用いて取得可能
- 取得
``` swift
Enum.case.rawValue
```
- 値から逆引き
``` swift
Enum(rawValue: value)
```
ちなみに値を指定しなかった時はかたによって以下のように振る舞いが変わります
- String型のとき
caseの名前がそのまま値になる
- Int型のとき
0から順に番号が振られる
