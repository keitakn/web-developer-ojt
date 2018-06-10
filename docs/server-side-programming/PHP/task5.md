# 課題5（ソーシャルログイン）

他の課題と同様に [ojt-php](https://github.com/keitakn/ojt-php) をベースに開発を進めて行きましょう。

## アウトプット

Googleアカウントでユーザー登録、ログインが出来るようにシステムを改修して下さい。

## 完了条件

以下の条件を満たすようにして下さい。

- TwitterやLINE等他のSNSアカウントでの改修要件が来ても耐えられる設計になっている事

## ヒント

この課題を行う為には https://console.developers.google.com/ に登録を行う必要があります。（Googleアカウントを持っていれば可能です）

流れを理解するだけなら [GoogleでログインするWebサービスを作ろう](https://dotinstall.com/lessons/google_connect_php_v2) を見ると良いでしょう。

ただし実装方法は実戦で使うには不十分なのであくまでも参考程度に捉えて下さい。

ここで利用するGoogleAPIはOAuthやOpenID Connectの仕組みに準拠しています。

下記の記事等を見てこの仕組について理解しておきましょう。

- [一番分かりやすい OAuth の説明](https://qiita.com/TakahikoKawasaki/items/e37caf50776e00e733be)
- [OpenID Connect](https://qiita.com/TakahikoKawasaki/items/498ca08bbfcc341691fe)

OAuthやOpenID Connectは世界的に広く使われているID連携方法です。

ここでしっかりと理解しておきましょう。

## このレッスンで身につける事

- OAuth、OpenID Connectに対する理解

## メンター向け

OAuthクライアントとして必要な実装が全て行われているか重点的にレビューして下さい。
