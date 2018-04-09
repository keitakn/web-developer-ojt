# 課題1（PHPの実行環境を作成する）

## アウトプット

[ojt-linux-vagrant](https://github.com/keitakn/ojt-linux-vagrant) 上にPHPの実行環境を作成して下さい。

また作成の手順をQiitaの記事として公開して下さい。（最初は限定共有公開でメンターレビュー後に全公開でOK）

## 完了条件

以下の条件を満たしていた時に課題は完了となります。

- [ojt-php](https://github.com/keitakn/ojt-php) のクイックスタートを参考に `http://192.168.33.100:8080` へアクセスして正常にHTML形式のレスポンスが確認出来る事
- `/etc/php.ini` にタイムゾーンの設定と日本語を扱う為の設定が行われている事
- `/etc/php.ini` でエラーログが出力されるように設定されている事
- 作成したQiitaの記事の手順を踏む事で環境構築を再現出来る事

Node.jsの時と同じく [ojt-php](https://github.com/keitakn/ojt-php) は [ojt-linux-vagrant](https://github.com/keitakn/ojt-linux-vagrant) の共有ディレクトリに設定されています。

もしもvagrant上で [ojt-php](https://github.com/keitakn/ojt-php) を確認出来ない時は最新のリポジトリを取り込んで下さい。

## ヒント

### インストール方法

[公式ドキュメント](http://php.net/manual/ja/install.php) を見るとインストール方法にも様々な方法があります。

一番簡単な `yum` によるインストールを行いましょう。

ただしPHP7.2のような新しいパッケージはyumリポジトリの追加を行う必要があります。

必要なリポジトリは `epel` と `remi` の2つです。

それぞれ下記のような手順でインストールが可能です。

```
yum install epel-release
yum install http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
```

PHPはいつくかのパッケージに依存しています。

AmazonLinuxの場合、以下のパッケージが入っていないので、インストールを行っておきましょう。

```
yum install httpd libmcrypt libtool-ltdl zlib-devel autoconf automake libevent libevent-devel
```

おそらくそのままインストールすると、`yum` のエラーが発生します。

これは元々存在するAmazonLinuxのyumリポジトリと先ほど入れた `remi` リポジトリの間でパッケージの衝突が起こっているからです。

インストールを完了させるには、`remi` , `remi-php72` のリポジトリを有効にして、`amzn-main` というリポジトリを無効化します。

`yum` のコマンドに引数を渡す事でリポジトリの有効・無効化が出来ます。

`--enablerepo` , `--disablerepo` というオプションを調査してみましょう。

またその後のレッスンで必要になるので、最低限以下のパッケージを入れておきましょう。

- php
- php-devel
- php-opcache
- php-mbstring
- php-pdo
- php-mysqlnd
- php-pecl-xdebug
- php-fpm
- php-xml

`php -v` と実行した結果が以下のようになれば成功です。（実行時期によって多少バージョンが異なる事はあります）

```
PHP 7.2.1 (cli) (built: Jan  3 2018 09:08:32) ( NTS )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.2.0, Copyright (c) 1998-2017 Zend Technologies
    with Zend OPcache v7.2.1, Copyright (c) 1999-2017, by Zend Technologies
    with Xdebug v2.6.0beta1, Copyright (c) 2002-2017, by Derick Rethans
```

少々バージョンが古いですが過去に [私が書いた記事](https://qiita.com/keitakn/items/7d0a8d6e24c1861d799f#php%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB) が参考になるでしょう。

### `/etc/php.ini` の設定

様々な設定がありますが、最低限、完了の定義に記載してある設定は行っておきましょう。

特にエラーログが出ないと何が起こったのか本当に分からなくなってしまうので忘れずに設定を行いましょう。

`/etc/php.ini` の設定に関しては以下の記事も参考になります。

- [CentOSにPHP7をインストールしたらやっておくべき初期設定](http://affiwork.net/php-settings/)
- [【PHP】PHPをインストールしたらやっておきたい設定](https://qiita.com/knife0125/items/0e1af52255e9879f9332)
