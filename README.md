# 【iOS Swift】ニフティクラウドmobile backend を体験しよう！
![画像1](/readme-img/001.png)

## 概要
* Xcode(Swift)で作成したiOSアプリから、[ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)へデータ登録を行うサンプアプリです
 * 「START DEMO」ボタンをタップするとクラウドにデータが上がります★
* 簡単な操作ですぐに [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)を体験いただけます

## ニフティクラウドmobile backendって何？？
スマートフォンアプリのバックエンド機能（プッシュ通知・データストア・会員管理・ファイルストア・SNS連携・位置情報検索・スクリプト）が**開発不要**、しかも基本**無料**(注1)使えるクラウドサービス！今回はデータストアを体験します

注1：詳しくは[こちら](http://mb.cloud.nifty.com/price.htm)をご覧ください

![画像2](/readme-img/002.png)

## 動作環境
* Mac OS X 10.10(Yosemite)
* Xcode ver. 7.2.1
* Simulator ver. 9.2
 * iPhone6(iOS 9.2)

※上記内容で動作確認をしています。


## 手順
### 1. [ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)の会員登録とログイン→アプリ作成

* 上記リンクから会員登録（無料）をします。登録ができたらログインをすると下図のように「アプリの新規作成」画面出るのでアプリを作成します

![画像3](/readme-img/003.png)

* アプリ作成されると下図のような画面になります
* この２種類のAPIキー（アプリケーションキーとクライアントキー）はXcodeで作成するiOSアプリに[ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)の紐付けるため、あとで使います

![画像4](/readme-img/004.png)

* この後動作確認でデータが保存される場所も確認しておきましょう

![画像5](/readme-img/005.png)

### 2. [GitHub](https://github.com/natsumo/iOS-Swift_DB_DEMO.git)からサンプルプロジェクトのダウンロード

* この画面([GitHub](https://github.com/natsumo/iOS-Swift_DB_DEMO.git))の![画像10](/readme-img/010.png)ボタンをクリックしてサンプルプロジェクトをMacにダウンロードします

### 3. Xcodeでアプリを起動

* ダウンロードしたフォルダを開き、![画像09](/readme-img/009.png)をダブルクリックしてXcode開きます　![画像08](/readme-img/008.png)

![画像6](/readme-img/006.png)

### 4. APIキーの設定

* `AppDelegate.swift`を編集します
* 先程[ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)のダッシュボード上で確認したAPIキーを貼り付けます

![画像07](/readme-img/007.png)

* それぞれ`YOUR_NCMB_APPLICATIONKEY`と`YOUR_NCMB_CLIENTKEY`の部分を書き換えます
 * このとき、ダブルクォーテーション（`"`）を消さないように注意してください！
* 書き換え終わったら`command + s`キーで保存をします

### 5. 動作確認
* 左上の実行ボタン（さんかくの再生マーク）をクリックします

![画像12](/readme-img/012.png)

* シミュレーターが起動したら![画像13](/readme-img/013.png)ボタンをタップします
* 動作結果により画面に表示がされます
 * 保存に成功した場合：「`保存に成功しました。objectId:******`」
 * 保存に失敗した場合：「`エラーが発生しました。エラーコード:******`」
* objectIdはデータを保存したときに自動で割り振られるIDです
* 万が一エラーが発生した場合は、[こちら](http://mb.cloud.nifty.com/doc/current/rest/common/error.html)よりエラーコードを確認いただけます
![画像1](/readme-img/001.png)

* 保存に成功したら、[ニフティクラウドmobile backend](http://mb.cloud.nifty.com/)のダッシュボードから「データストア」を確認してみましょう！
* `TestClass`という保存用クラスが作成され、その中にデータが確認できます

## 解説
サンプルプロジェクトに実装済みの内容のご紹介

#### SDKのインポートと初期設定
* ニフティクラウドmobile backend の[ドキュメント（クイックスタート）](http://mb.cloud.nifty.com/doc/current/introduction/quickstart_ios.html)をSwift版に書き換えたドキュメントをご用意していますので、ご活用ください
 * [＜CocoaPods＞SwiftでmBaaSを始めよう！](http://qiita.com/natsumo/items/b2d18d87d57300c8d81c)

#### ロジック
 * `Main.storyboard`でデザインを作成し、`ViewController.swift`にロジックを書いています
 * `testClass`オブジェクトに対してkey, value形式で値をセット（`setObject`）し、`saveInBackgroundWithBlock`メソッドを実行すると、非同期にてデータが保存されます

```swift
//
//  ViewController.swift
//  DB_Swift
//
//  Created by Natsumo Ikeda on 2016/04/12.
//  Copyright © 2016年 NIFTY Corporation. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    // Main.storyboardで作成した「START DEMO」ボタン押下時の処理
    @IBAction func startBtn(sender: UIButton) {
        // 保存先クラスを作成
        let obj = NCMBObject(className: "TestClass")
        // 値を設定
        obj.setObject("Hello,NCMB!", forKey: "message")
        // 保存を実施
        obj.saveInBackgroundWithBlock{(error: NSError!) in
            if (error != nil) {
                // 保存に失敗した場合の処理
                print("エラーが発生しました。エラーコード:\(error.code)")
                self.alertText.text = "エラーが発生しました。エラーコード:\(error.code)"
            }else{
                // 保存に成功した場合の処理
                print("保存に成功しました。objectId:\(obj.objectId)")
                self.alertText.text = "保存に成功しました。objectId:\(obj.objectId)"
            }
        }
    }
    
    // Main.storyboardで作成した動作確認結果表示用のテキストフィールド
    @IBOutlet weak var alertText: UITextField!
}
```

## 参考
* 編集中
