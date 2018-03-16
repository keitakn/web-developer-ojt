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

## MySQLの自動起動設定

OSの再起動を行ってもMySQLサーバが自動起動する設定を行っておきましょう。

`sudo su` でrootユーザーになり、以下のコマンドを実行しましょう。

```bash
chkconfig mysqld on
```

## MySQLサーバにログインを行う

MySQLサーバにログインを行って操作を行ってみましょう。

初期状態だと `root` ユーザーのパスワードが自動的に設定されたものになっていますので以下のコマンドで確認を行います。

```bash
sudo cat /var/log/mysqld.log | grep 'temporary password'
```

実行すると下記のように出力されます。

```
2018-03-16T15:20:39.532915Z 1 [Note] A temporary password is generated for root@localhost: Qzon6<:YgfeX
```

この場合 `Qzon6<:YgfeX` がrootパスワードになりますのでどこかにメモしておきます。

以下のコマンドでログインを行います。

```bash
mysql -u root -h localhost -p
```

`-u` はユーザーを表すオプションです。最初はrootしかユーザーがいないのでrootを入力します。

`-h` はMySQLサーバが存在するIPアドレスやホスト名を指定します。

今回のケースだとログインしているLinuxサーバ上にMySQLサーバが存在するので `localhost` を指定します。

`-p` はパスワードを入力するという意味です。

先程メモしたrootパスワードを入力しましょう。

下記のように表示されればログイン成功です。

```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.21

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

## セキュリティ設定を行う

ログインは出来ましたが、今の状態だと正しいコマンドを打っても以下のエラーが表示されてしまいます。

```
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
```

これはセキュリティ上の理由から初期設定を行わないとMySQLサーバが利用出来ない状態になっている為です。

初期設定を行う為に一度MySQLサーバからログアウトを行います。

コンソールに `exit` を入力して下さい。

```
mysql> exit
Bye
[root@localhost vagrant]#
```

これでMySQLサーバからログアウトが出来ます。

ここから初回セキュリティ設定を行っていきます。

以下のコマンドを実行しましょう。（rootユーザーで実行する必要があります）

```
mysql_secure_installation
```

ここからは対話式インターフェースになりますので画面の指示に従い情報を入力します。

`Enter password for user root:` と聞かれるのでrootパスワードを入力します。

新しいrootパスワードの入力を求められるので入力します。

```root
The existing password for the user account root has expired. Please set a new password.

New password:
```

ここで注意点が1点あります。

パスワードは「英数字、記号、混在の8文字以上」である必要があります。

条件に合うパスワードを入力しましょう。

例えば `(MySQLRootPassword1234)` とかです。

`Re-enter new password:` と聞かれるので再度同じパスワードを入力します。

`Estimated strength of the password: 100` みたいなメッセージが表示されます。

パスワードの推定強度を数値で表しています。

`Do you wish to continue with the password provided?` と聞かれるので `y` を入力しEnterを押します。

`Remove anonymous users?` と聞かれます。

MySQLには匿名ユーザーという特殊なユーザーが存在しており、匿名ユーザーはパスワードなしでMySQLにログインする事が可能です。

セキュリティ的に大きな問題があるので `y` Enterで匿名ユーザーを削除します。

`Disallow root login remotely?` と聞かれます。

意味は「他のサーバからこのMySQLサーバへrootユーザーでログインする事を拒否しますか」です。

rootユーザーはMySQLの操作が全て実行可能なので他サーバからログインする事を許可すべきではありません。

よって `y` Enterを押します。

`Remove test database and access to it?` と聞かれます。

「テスト用に作られたデータベースを削除してアクセスしますか」なので `y` Enterを押します。

`Reload privilege tables now?` と聞かれます。

これは「今までの設定を今すぐ反映しますか」という意味になりますので `y` Enterを押します。

下記のように表示されれば初期設定は完了です。

```
Success.

All done!
```

再度 `mysql -u root -p` を行い先程設定したパスワードでログインが出来る事を確認しましょう。

rootパスワードを忘れると結構面倒な作業をしなければならないので忘れないようにしましょう。

一応忘れた時用の参考記事を載せておきます。

- [MySQL の root パスワード忘れた時](https://qiita.com/y1row/items/994ecf8b478b7aac4c7d)
- [MySQL5.7でrootパスワード初期化変更メモ / 2017年10月](https://qiita.com/ononoy/items/7732a2e97b3901eb9d57)

## データベースとユーザーの作成

次にデータベースとユーザーを作成します。

MySQLサーバにrootユーザーでログインを行います。

最初にテーブルを格納する為のデータベースを作成します。

データベース名は `ojt_db` とします。

MySQLを操作する為には [SQL](https://ja.wikipedia.org/wiki/SQL) という言語を利用します。

SQLはStructured English Query Languageの略でリレーショナルデータベースと対話する為の言語です。

データベースを作るには以下のSQLを実行します。

```sql
CREATE DATABASE ojt_db;
```

以下のように表示されれば成功です。

```
mysql> create database ojt_db;
Query OK, 1 row affected (0.00 sec)
```

SQLは小文字でも大文字でも動きますが、SQLで使われている文法と利用者が決めたデータベース名、テーブル名等と区別する為、大文字で記載する事が多いです。

同様の理由からテーブル名やデータベース名には小文字と `_` のみを使って命名するケースが多いです。

次にユーザーの作成を行います。

以下のSQLを実行します。

```sql
GRANT CREATE, SELECT, UPDATE, INSERT, DELETE ON ojt_db.* TO `ojt_user`@`localhost` IDENTIFIED BY '(YourPassword999)';
```

意味を簡単に説明すると以下のようになります。

- ユーザー名： ojt_user
- パスワード： (YourPassword999)
- 操作権限があるデータベースとテーブル： ojt_dbに存在する全てのテーブル
- 実行可能なSQL： CREATE、SELECT、UPDATE、INSERT、DELETE
- 有効な接続元： localhost

`CREATE、SELECT` 等は実行出来るSQLの種類を表します。

例えば `CREATE` はテーブルの新規作成、`SELECT` はテーブルからデータを閲覧出来る権限です。

[こちら](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html) を参照して下さい。

`ALL` とすると全てのSQLが実行可能になりますが基本的には必要な権限だけを追加するのがセキュリティ的に推奨されます。

ユーザー名は `ユーザー名@接続元ホスト` でユニークになります。

その為、ホスト名が異なれば同じユーザー名を使う事が出来ます。

例えば下記のようなSQLを実行すると、IPアドレスが `192.168.` から始まる全てのサーバから `ojt_db` にアクセス出来る `ojt_user` が作成されます。

```sql
GRANT CREATE, SELECT, UPDATE, INSERT, DELETE ON ojt_db.* TO `ojt_user`@`192.168.%` IDENTIFIED BY '(YourPassword999)';
```

一度MySQLサーバからログアウトして今度は作成した `ojt_user` でログイン出来るか確認しましょう。

`mysql -u ojt_user -p` でパスワードを入力します。

ログインが出来たら `SHOW DATABASES;` というSQLを実行します。

```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| ojt_db             |
+--------------------+
2 rows in set (0.00 sec)
```

`SHOW DATABASES;` は存在するデータベースの一覧を表示させる命令ですが、この時に接続権限がないデータベースは表示されません。

このようにMySQLではユーザーに適切な権限を与える事で不正な操作を予め防ぐ事が出来ます。
