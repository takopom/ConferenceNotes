# スライド
https://speakerdeck.com/syarihu/droidkaigi-2017-function-introduction-of-google-play-services

# Google Sign-In API

## 以前のもの
アカウント選択画面、Intentが帰ってきたら色々自分でやって…Auth周りを自分で色々やって…と手間だった。

## 新しいもの
[Google Play Services 8.3で新しいAPI](https://developers.google.com/android/guides/releases#november_2015_-_v83)になり、簡単に実装できるようになった

- アカウントを取得してサインインするのにPermissionが必要ない
- 新しいデザインのサインインボタン
- Silent Sign-In
 - 別のプラットフォームでサインインを要求されることがなくなる
 - Silent Sign-Inを満たす要件が必要（スライド参照）
- 多言語対応

## 実装

（詳しい実装はスライドを参照）

動作にはAndroid2.3以上が必要

- ユーザの基本的な情報を取得するには、`DEFAULT_SIGN_IN`
- メールアドレスの取得もできる`requestEmail()`
- サインインボタン：xmlに記述すればよい
- 権限の取り消し：`revoke` ←サインアウトする前に権限を取り消すこと
- Silent signin：`onResume`に実装する

# Google Plus One Button

Google+で共有したりできる機能。（Facebookのいいね！みたいな機能）

Google Playストア上にはこのボタンは存在しない。

## ASOに影響があるらしい？

「ストアの検索順位に影響があるらしい」という情報があり、検証されたそう。

- 実装方法についてはスライド参照

## +1数の変化

- 1ヶ月後で2倍
- 1年で6倍

- +1数の増加に伴い、無料トップ総合順位も上がり始めた
- カテゴリ順位は上がった
 - 一時的に上昇するが、すぐ下がる
- 検索順位も上がった
 - が、変動の値は小さかった
 - 上がりにくいが下がりにくい

## まとめ

- 長期的に見て順位が上がる可能性はあるが、+1以外の要素がやはり必要
- 手軽に実装できるところがいいところ
- 余裕がある時に実装するとよい

# App Invites

- メールやSNSで友人知人をアプリに簡単に招待できるしくみ
- メールの招待リンクからインストールする

- Firebase Invitesという名前になっている


## 例：Google santa tracker

- 招待する側：ゲームで遊ぶ
- シェアでAppInvitesの招待画面を開く
- シェアすると、メールorSNSで招待メッセージが送られる
- アプリをインストールして、アプリを起動すると、ディープリンクが発動、すぐ遊べる

## 実装

（正確な情報はスライド参照）

Firebase Dynamic Linksをあらかじめ有効にしておく必要がある。  
 - Firebase consoleから有効にする

ディープリンク用のActivityを作成する  
 - onStartの中で、AppInvitesの情報が含まれているかを調べる。  
 - 招待IDの取得、ディープリンク用のURLを取得、などする

招待状送信画面の、タイトルやメール本文の一番上は、自分の好きな文言を設定できる。  
 - ディープリンクのパラメータの後ろに、任意のパラメータを付与できる。

GoogleAnalyticsのトラッキングもできる（analyticsのIDが必要）

AppInvitesの受け入れ  
 - AppInviteApi.getInvitation

## 効果検証
GoogleAnalyticsと連携すれば検証できる。

GoogleAnalyticsでカスタムディメンションを追加する

- sent:メール送信数
- accpted:インストールリンククリック数
- completed:インストール数

## 導入結果

メールアプリが立ち上がっていたところをAppInvitesに変更した。

- 送信数、承諾数も増えた
- インストール数に大きな変化はなかった

アプリの紹介メールではなく、もう少しディープリンクを活用するような内容なら結果が変わるかもしれない。

もしメール送信で紹介の機能がある場合は、それをAppInvitesに置き換えるのはありかもしれない。（共有のステップが通常のメールより簡単なため）

# その他メモ

別のSNSに共有する場合は、Firebaseの方のディープリンクを使うとよい。
（詳細を忘れてしまったが、LINEとかに共有するときは、firebaseのdeeplinkのurlを使うとよい、というような話…？？）
