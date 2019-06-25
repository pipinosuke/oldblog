+++
title = "StripeのCheckoutで決済機能を簡単に導入する"
date = 2019-06-23T21:49:39+09:00
description = "Stripeといえば決済APIを提供しているということは知っていたが、詳しく調べてみるとそれだけではなかった。StripeCheckoutという機能を使うとコードを貼り付けるだけで簡単に決済機能が導入できた。その手順と所感についてのメモ"
tags = ["決済","Stripe"]
category = ["技術"]
image = "stripe.png"
draft = false
+++
### 経緯
今までは個人で運営しているスクールなどで発生する毎月の集金には[PaymoBiz](https://biz.paymo.life/)を利用していました。そんな中PaymoBizサービス終了が発表され、別の集金方法を模索することを余儀なくされてしまいました。「決済APIを利用するという方法が手数料安く済みそうだけど、自前でシステム作るしかないのか・・・手間がかかりそうだなぁ。。」などと思いながら詳しく調べていたところ、StripeではCheckoutという機能が提供されており、その機能を使うと自動生成されたコードを貼り付けるだけで決済機能が導入できるようでした。このシンプルさが自分にはドンピシャだったのですぐに試してみました。

### 所感
試してみて複雑にデータを管理しなければならないものでなければCheckoutで十分だと感じました。またStripeはただオンライン決済のAPIを提供しているだけの印象でしたが実はそうではなく、ダッシュボードが充実していたりサードパーティ製の決済アプリのマーケトットプレイスとしての側面も持ち合わせていていわゆる「プラットフォーム」という方がしっくりきました。書いた後気付いたのですが[PAY.JP](https://pay.jp)が同様の機能を提供していてかつ手数料も1%ほど安いです。

## Stripe Checkoutの導入手順メモ
[公式ドキュメント](https://stripe.com/docs/payments/checkout/client)

#### できること
- WEBページにJSのスクリプトを貼り付けることで決済機能を導入できます
- 月額課金にも対応しています
- カスタマイズ次第で顧客データの収集なども可能

#### 仕様について
- 実際に使用する際はテストモードと本番モードを切り替える必要があります。最初はテストモードで挙動を確認しておくことをお勧めします。
- 決済成功時・失敗時の遷移先を用意しておく必要があります。
- サイトはhttps化されている必要があります
- 手数料は一律3.6%(同じく決済APIを提供しているBASE社の[PAY.JP](https://pay.jp)は2.6%でした。こちらでも良いかもしれない)

### 1. アカウントの有効化
導かれるまま本人情報などを入力すればおkです。
### 2. Checkoutの設定
公式ドキュメント、[Checkout Client Quickstart](https://stripe.com/docs/payments/checkout/client)を参考に導入します。主にやることは以下の二つです。

- ドメインの設定
- 商品の作成とCheckoutの有効化

### 3. LIVEモードに切り替え
[Going Live with Checkout](https://stripe.com/docs/payments/checkout/live)を参考にLIVEモードでチェックアウトできるように設定します。本番モードで稼働する際はドメインが設定されていないとボタンを押しても正しく動作しないので注意。

### 4. コードの貼り付けとカスタマイズ
<!-- Load Stripe.js on your website. -->
<script src="https://js.stripe.com/v3"></script>

<!-- Create a button that your customers click to complete their purchase. Customize the styling to suit your branding. -->
<button
  style="background-color:#6772E5;color:#FFF;padding:8px 12px;border:0;border-radius:4px;font-size:1em"
  id="checkout-button-plan_FIfczSYPCPZLe1"
  role="link"
>
  テスト
</button>

<div id="error-message"></div>

<script>
  var stripe = Stripe('pk_test_IYYRFH7Xe3g9nygvZkfKsT4I006VEBDP5U');

  var checkoutButton = document.getElementById('checkout-button-plan_FIfczSYPCPZLe1');
  checkoutButton.addEventListener('click', function () {
    // When the customer clicks on the button, redirect
    // them to Checkout.
    stripe.redirectToCheckout({
      items: [{plan: 'plan_FIfczSYPCPZLe1', quantity: 1}],

      // Do not rely on the redirect to the successUrl for fulfilling
      // purchases, customers may not always reach the success_url after
      // a successful payment.
      // Instead use one of the strategies described in
      // https://stripe.com/docs/payments/checkout/fulfillment
      successUrl: window.location.protocol + '//school.nosugi.tech/success',
      cancelUrl: window.location.protocol + '//school.nosugi.tech/canceled',
    })
    .then(function (result) {
      if (result.error) {
        // If `redirectToCheckout` fails due to a browser or network
        // error, display the localized error message to your customer.
        var displayError = document.getElementById('error-message');
        displayError.textContent = result.error.message;
      }
    });
  });
</script>

上のボタンを押すとクレジットカード情報の入力フォームにリダイレクトされるはずです。中身のコードは以下に掲載しています。見た目や顧客データの処理についてのカスタマイズも可能です。詳しくは[公式ドキュメント](https://stripe.com/docs/payments/checkout/client)を参照してください。

```html
<script src="https://js.stripe.com/v3"></script>

<!-- Create a button that your customers click to complete their purchase. Customize the styling to suit your branding. -->
<button
  style="background-color:#6772E5;color:#FFF;padding:8px 12px;border:0;border-radius:4px;font-size:1em"
  id="checkout-button-plan_FIfczSYPCPZLe1"
  role="link"
>
  テスト 
</button>

<div id="error-message"></div>

<script>
    // 本番はLIVEモードのAPIキーを入力してください
  var stripe = Stripe('pk_test_IYYRFH7Xe3g9nygvZkfKsT4I006VEBDP5U');

  var checkoutButton = document.getElementById('checkout-button-plan_FIfczSYPCPZLe1');
  checkoutButton.addEventListener('click', function () {
    // When the customer clicks on the button, redirect
    // them to Checkout.
    stripe.redirectToCheckout({
      items: [{plan: 'plan_FIfczSYPCPZLe1', quantity: 1}],

      // Do not rely on the redirect to the successUrl for fulfilling
      // purchases, customers may not always reach the success_url after
      // a successful payment.
      // Instead use one of the strategies described in
      // https://stripe.com/docs/payments/checkout/fulfillment
      successUrl: window.location.protocol + '//success',
      cancelUrl: window.location.protocol + '//canceled',
    })
    .then(function (result) {
      if (result.error) {
        // If `redirectToCheckout` fails due to a browser or network
        // error, display the localized error message to your customer.
        var displayError = document.getElementById('error-message');
        displayError.textContent = result.error.message;
      }
    });
  });
</script>
```

## まとめ
ダッシュボードのUIが非常にわかりやすかったおかげで特に詰まることなく導入できてしまいました。。。労力で考えると自前で用意するのとCheckoutを使うのでは雲泥の差だったと思うので非常に助かりました！！