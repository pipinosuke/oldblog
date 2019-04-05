+++
title = "Firebaseでサーバーレス開発  ログイン編(iOS版)"
date = 2019-03-31T09:48:36+09:00
description = "名前がイケてる感あるので遅ればせながら使ってみました。「サーバーレス開発」という言葉をこの流行らせたのって間違いなくFireBaseでしょう。というぐらい機能的には至れり尽くせりな感じでした。今回は自前での実装だと面倒なログイン機能とUIを提供してくれるFirebaseAuthUIを利用してみた際の覚書です。"
tags = ["iOS","Firebase","サーバーレス開発"]
category = ["技術"]
image = "firebase-auth-ios.png"
draft = false
+++

##### 参考URL
基本的には[公式ドキュメント](https://firebase.google.com/docs/auth/ios/password-auth?hl=ja)を参考にしましたが端折られている部分もあったので、こちらの[Qiitaの記事](https://qiita.com/matsuei/items/4f56c0f8d9a1b96cd9f0)も合わせて参考にしました。
### 導入
cocoapodsでインストールします。cocoapodsの使い方については割愛します。そのあとは下の画像のような感じでかなり親切にやってくれます、簡単に導入できました。
[![FireBase installed image](https://i.gyazo.com/9f67804569f2d2d629136c98c3d7afd7.png)](https://gyazo.com/9f67804569f2d2d629136c98c3d7afd7)

#### 共通部分
AppDelegateに追記。公式ドキュメントに従って**didFinishLaunchingWithOptionsに書くとエラーになるので注意**。尚、このことは公式ドキュメントでは言及されていない。

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
#### 2. TwitterのコールバックURLを設定
- twitterkit-CONSUMERKEY(CONSUMERKEYの部分は[developer.twitter.com](https://developer.twitter.com/en/)から各自参照)
- 上の画像のURL
以上二つを[developer.twitter.com](https://developer.twitter.com/en/apps)のアプリのコールバックURLに設定します。
[![Image from Gyazo](https://i.gyazo.com/b0939fdc9278a664f9372f560ae5bdab.png)](https://gyazo.com/b0939fdc9278a664f9372f560ae5bdab)
##### 3. URLスキームの登録
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
続いてAppDelegateのapplicationに記述。ここも公式に書いてなくてわからんかったです。
```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any]) -> Bool {
        let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as! String?
        if FUIAuth.defaultAuthUI()?.handleOpen(url, sourceApplication: sourceApplication) ?? false {
            return true
        }
    //ツイッターログイン
        if TWTRTwitter.sharedInstance().application(app, open: url, options: options) {
            return true
        }

        // other URL handling goes here.
        return false
    }

```

#### Googleログイン
こちらは簡単Firebase コンソールで [Authentication] セクションを開き、Google ログインを有効にします。
XCodeのURLスキームに`GoogleService-Info.plist`のREVERSED_CLIENT_IDを登録します。ツイッターと同じ要領で追加すればおk。

### ログイン画面の実装
FirebaseAuthUIというライブラリがUIのテンプレを用意してくれているのでこれを使う。
[![Image from Gyazo](https://i.gyazo.com/2e6e254eb084c4863c24f726ef7634d7.png)](https://gyazo.com/2e6e254eb084c4863c24f726ef7634d7)
#### 1. FirebaseUIの導入
[FirebaseUI](https://firebase.google.com/docs/auth/ios/firebaseui?hl=ja)を導入する。Podfileに以下を記述。
``` vim
pod 'Firebase/Auth'
//以下追記部分
pod 'FirebaseUI/Auth'
pod 'FirebaseUI/Google'
pod 'FirebaseUI/Twitter'
```

#### 2. ログイン画面の実装と遷移
``` swift
import FirebaseAuthUI
import FirebaseTwitterAuthUI
import FirebaseGoogleAuthUI
~~~~~
class ViewController: UIViewController {
  private var authUI = FUIAuth.defaultAuthUI() //authUIの初期化
  private let providers: [FUIAuthProvider] = [
        //ここでプロバイダの追加を行う
        FUITwitterAuth(),
        FUIGoogleAuth()
    ]
    override func viewDidLoad() {
        super.viewDidLoad()
        authUI.providers = providers //プロバイダをセット
    }

```
といった感じでログインできるプロバイダを追加してあげて、後はお決まりのやつで遷移できます。
``` swift
let authVC = authUI.authViewController() //ログイン画面のインスタンス化
present(vc, animated: true, completion: nil)
```

### ログイン状態か否かの判定について
`viewWillAppear`でaddStateChangeListenerを呼ぶことで判定します。ログイン状態でないときはuserがnilになります。
```swift
    override func viewWillAppear(_ animated: Bool) {
        Auth.auth().addStateDidChangeListener { (auth, user) in
            if let user = user {
                print(user)
            } else {
                self.login() //nilなのでログインさせる
            }
        }
    }
```

### ログイン後の処理について
FUIAuthのDelegateメソッドが用意されています。こんな感じで実装できます。
```swift
extension ViewController: FUIAuthDelegate {
    func authUI(_ authUI: FUIAuth, didSignInWith authDataResult: AuthDataResult?, error: Error?) {
        if let error = error {
            //エラー処理
            print(error)
        }
        //ログイン後の処理
    }
}
```
`viewDidLoad`とかでdelegateの宣言も忘れずに。
```swift
   authUI.delegate = self
```

#### カスタマイズ
FUIAuthDelegateメソッドでUIのカスタマイズが可能です。詳しく書く。
