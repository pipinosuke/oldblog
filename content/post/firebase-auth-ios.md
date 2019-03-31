+++
title = "FirebaseAuthenticationでSNSログイン機能の実装 覚書 (iOS版)"
date = 2019-03-31T09:48:36+09:00
description = "名前がイケてる感あるので遅ればせながら使ってみました。機能的には「サーバーレス開発」という言葉をこの流行らせたのって間違いなくFireBaseでしょう。というぐらい至れり尽くせりな感じでした。今回は自前での実装だと面倒なログイン機能を提供してくれるFireBaseAuth及びFirebaseAuthUIを利用してみた際の覚書です。"
tags = ["ios","firebase"]
category = ["技術"]
image = ""
draft = false
+++
##### 参考URL
[Qiitaの記事](https://qiita.com/matsuei/items/4f56c0f8d9a1b96cd9f0)を参考にしました。[公式ドキュメント](https://firebase.google.com/docs/auth/ios/password-auth?hl=ja)を参考に導入するはずでしたが、重要なところを端折っていたりしたりしてわかりにくかった。

### 導入
cocoapodsで導入。詳しくは割愛します。下の画像のような感じでかなり親切にやってくれます、簡単に導入できました。
[![FireBase installed image](https://i.gyazo.com/9f67804569f2d2d629136c98c3d7afd7.png)](https://gyazo.com/9f67804569f2d2d629136c98c3d7afd7)

### 実装
#### 共通
AppDelegateに追記。公式ドキュメントに従って**didFinishLaunchingWithOptionsに書くとエラーになるので注意**（ふざけんな）

``` swift
import Firebase
import FirebaseAuthUI
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    //以下追記
    override init() {
        super.init()
        FirebaseApp.configure()
    }

    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any]) -> Bool {
        let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as! String?
        if FUIAuth.defaultAuthUI()?.handleOpen(url, sourceApplication: sourceApplication) ?? false {
            return true
        }

        // other URL handling goes here.
        return false
    }

```
#### ツイッターログイン
##### 1. Twitterログインの有効化
[developer.twitter.com](https://developer.twitter.com/en/apps)からapiKeyとTokenを取得する必要があります。使用用途などについて300文字くらい書かされてクッソめんどかったです。まぁ無料で使わせてもらってるので大人しく感謝します。先ほどのリンクからアプリを登録したあと、FirebaseAuthのコンソールからそれぞれのキーを登録・有効化しましょう。
[![TwitterLoginConfig](https://i.gyazo.com/3b90e9e386df85be75e637e25d1c6c54.png)](https://gyazo.com/3b90e9e386df85be75e637e25d1c6c54)
##### 2. URLスキームの登録
Xcodeのworkspace→target→Infoの「URL Types」からURLスキームを登録します。
[![Image from Gyazo](https://i.gyazo.com/fbf18c8ad97472731e3a8c1b6fa8abef.png)](https://gyazo.com/fbf18c8ad97472731e3a8c1b6fa8abef)
`twitterkit-CONSUMERKEY`という形で入力します。ConsumerKeyも[developer.twitter.com](https://developer.twitter.com/en/apps)から参照できます。
##### 3.  実装
AppDelegateのdidFinishLaunchingWithOptionsに以下のように記述。引数は各自入力しましょう。
``` swift
import TwitterKit
~~~~~~~~~~~~~~~

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.

        //Twitterログイン
        TWTRTwitter.sharedInstance().start(withConsumerKey: "CONSUMERKEY", consumerSecret: "SECRET")
        return true
    }
```
4.
#### Googleログイン
Firebase コンソールで [Authentication] セクションを開き、Google ログインを有効にします。
XCodeのURLスキームに`GoogleService-Info.plist`のREVERSED_CLIENT_IDを登録します。ツイッターと同じ要領で追加すればおk。

### ログイン画面の実装
FirebaseAuthUIというライブラリがUIのテンプレを用意してくれているのでこれを使う。
[![Image from Gyazo](https://i.gyazo.com/2e6e254eb084c4863c24f726ef7634d7.png)](https://gyazo.com/2e6e254eb084c4863c24f726ef7634d7)
##### 1. FirebaseUIの導入
[FirebaseUI](https://firebase.google.com/docs/auth/ios/firebaseui?hl=ja)を導入する。Podfileに以下を記述。
``` vim
pod 'Firebase/Auth'
//以下追記部分
pod 'FirebaseUI/Auth'
pod 'FirebaseUI/Google'
pod 'FirebaseUI/Twitter'
```

#### 2. ログイン画面への遷移
``` swift
import FirebaseAuthUI
import FirebaseTwitterAuthUI
import FirebaseGoogleAuthUI
~~~~~
class ViewController: UIViewController {
private var authUI = FUIAuth.defaultAuthUI()
private let providers: [FUIAuthProvider] = [
        FUITwitterAuth(),
        FUIGoogleAuth()
    ]
    override func viewDidLoad() {
        super.viewDidLoad()
        authUI.providers = providers
    }

```
といった感じでログインできるプロバイダを追加してあげて、後はお決まりのやつで遷移できます。
``` swift
let authVC = authUI.authViewController() //ログイン画面のインスタンス化
navigationController?.present(vc, animated: true, completion: nil)
```

#### カスタマイズ
FUIAuthDelegateメソッドでUIのカスタマイズが可能です。
