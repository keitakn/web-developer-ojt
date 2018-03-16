# RDBMSの基礎知識

ここではRDBMSの基礎を学びます。

## RDBMSの概要

RDBMSはRelational DataBase Management Systemの略です。

日本語にすると「関係データベース管理システム」となります。

リレーショナルデータベースを管理する為の専用ソフトウエアを指します。

リレーショナルデータベースは1件のデータを表形式で管理します。

ざっくり言うとExcelの表をイメージしてもらうと分かりやすいです。

下記は商品テーブルのイメージです。

| 商品ID | 商品名 | 価格 |カテゴリ|
|:---|:---:|---:|---:|
|1 |コーラ |100 |10|
|2 |USB |2000 |20|
|3 |傘 |500 |30|
|4 |お茶 |100 |10|

`商品ID` や `商品名` 等の項目を「カラム（列）」、1件のデータを「レコード（行）」、レコードの集まりをテーブル（表）と呼びます。

リレーショナルデータベースの名の通り復数のテーブル（表）を結合（リレーション）して利用する事が出来ます。

下記の画像のようなイメージです。

![rdbimages](https://user-images.githubusercontent.com/11032365/37476473-9bdfb96e-28b8-11e8-91f2-e9e504f719be.png)

## RDBMSの特徴

テーブル形式でデータの出し入れが出来る以外にも様々な特徴があります。

例えば以下のような特徴です。

- 不正なデータ記録を防ぐ仕組みがありデータ不整合を起こしにくい（もちろん正しく利用すればという前提条件はつきます）
- 権限を持たない不正なユーザー操作を防ぐ仕組みがある
- トランザクション処理（後で解説します）が利用出来るのでデータ不整合が起きにくい

## RDBMSの種類

以下のような製品があります。

- [Oracle](https://ja.wikipedia.org/wiki/Oracle_Database)
- [Microsoft SQL Server](https://ja.wikipedia.org/wiki/Microsoft_SQL_Server)
- [PostgreSQL](https://ja.wikipedia.org/wiki/PostgreSQL)
- [MySQL](https://ja.wikipedia.org/wiki/MySQL)

月額数千万円する値段が高い物からオープンソースで無料提供されている物まで様々です。

本カリキュラムではWeb開発でもっとも多く利用されている [MySQL](https://ja.wikipedia.org/wiki/MySQL) を利用します。

## MySQL公式サイト

世界で最も普及しているオープンソースのRDBMSです。

[こちら](https://dev.mysql.com/) が公式サイトになります。

## MySQLのインストール

`yum` を使ってインストールするのが最も簡単です。

https://dev.mysql.com/downloads/repo/yum/ より公式提供されているyumリポジトリを利用しましょう。

※ なお、本カリキュラムでは [Amazon Linux AMI 2017.09](https://aws.amazon.com/jp/blogs/news/now-available-amazon-linux-ami-2017-09/) を利用している前提で話を進めさせて頂きます。

他のLinuxディストリビューションを利用している場合はインストール方法が多少変わってきます。

インストールするMySQLのバージョンは5.7系です。

以下のコマンドを実行します。

```bash
sudo yum install https://dev.mysql.com/get/mysql57-community-release-el6-11.noarch.rpm
```

これで公式のyumリポジトリがインストールされました。

続いてMySQLの本体をインストールします。

```bash
sudo yum install mysql-community-client mysql-community-common mysql-community-devel mysql-community-libs mysql-community-server
```

多少時間がかかりますがインストールが完了するまで待ちましょう。

エラーが出なければインストールが完了しています。

## MySQLの起動

以下のコマンドで起動を行います。

```bash
sudo service mysqld start
```

以下のように表示されれば起動は成功です。

```
Initializing MySQL database:                               [  OK  ]
Starting mysqld:                                           [  OK  ]
```
