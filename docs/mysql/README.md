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

## MySQLサーバの設定

MySQLを使う上で事実上必須になっている設定を行っていきます。

### 文字コードの設定

MySQLサーバにログインを行い `SHOW VARIABLES LIKE 'char%';` というSQLを実行してみて下さい。

初期状態では下記のような状態になっている事が確認出来ます。

```
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | latin1                     |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | latin1                     |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)
```

`latin1` という文字コードはMySQLのデフォルト設定なのですが、これだと日本語を正しく扱う事が出来ません。

MySQL5.7系で日本語を含めたマルチバイト文字を正しく扱えるのは `utf8mb4` だけです。

`utf8mb4` は 👌や🐱等の4バイト文字も扱う事が可能です。

最も多くの言語にも対応しているので文字コードは `utf8mb4` の一択で問題ありません。

他にもいくつか変更すべき設定項目があるので、設定ファイルを修正し設定を変更します。

rootユーザーで `/etc/my.cnf` というファイルを `vim` 等で開いて下さい。

初期状態は下記のようになっています。

```
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html

[mysqld]
#
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M
#
# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin
#
# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock

# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
```

以下の記述を追記して下さい。

```
character-set-server=utf8mb4
collation-server=utf8mb4_bin
skip-character-set-client-handshake
default-storage-engine=InnoDB
innodb_file_per_table=1
innodb_large_prefix=1
innodb_file_format=Barracuda
innodb_default_row_format=DYNAMIC
default_password_lifetime = 0
slow_query_log = 1
long_query_time = 0.1
slow_query_log_file = /var/log/mysql-slow-query.log

[mysql]
auto-rehash
default-character-set = utf8mb4

[mysqldump]
default-character-set = utf8mb4
```

### 文字コードに関する設定

追記した内容のうち文字コードに関する設定は以下になります。

```
character-set-server=utf8mb4
collation-server=utf8mb4_bin
skip-character-set-client-handshake

[mysql]
default-character-set = utf8mb4

[mysqldump]
default-character-set = utf8mb4
```

`[mysql]` 、`[mysqldump]` セクションに記載してあるのはそのまま文字コードを `utf8mb4` に設定するという意味です。

- `skip-character-set-client-handshake`

MySQLのクライアント側（接続側）が設定する文字コード設定を無効化する事が出来ます。

常にMySQLサーバ側の設定（ここでは `utf8mb4`）が設定されて欲しいのでこの設定を有効にします。

https://dev.mysql.com/doc/refman/5.6/ja/faqs-cjk.html

- `character-set-server`

MySQLサーバ側の文字コード設定です。

- `collation-server`

文字の照合規則・照合順序（並べ替えルールみたいなもの）を表す項目です。

MySQLでマルチバイト文字を扱うには考慮しておくべき問題があります。

- 絵文字の 🍣と🍺が同様のデータとして扱われてしまう「🍣🍺問題」
- カタカナの「ハ」と「パ」が同じものとして扱われてしまう「ハハパパ問題」

現状、この2つを解決する設定は `collation-server=utf8mb4_bin` のみです。

これを設定する事によりデフォルト状態とは少し違う挙動になります。

MySQLは通常 `a` と `A` を同じ物として認識する仕様です。

`utf8mb4_bin` を設定すると明確に別の文字として扱われるようになりますので注意が必要です。

普通にプログラムを組んでいればこの点が問題になる事は少ないハズです。

MySQLの文字コードに関しては [こちらのスライド](https://www.slideshare.net/tmtm/mysql-62004569) が参考になります。

### ストレージエンジンに関する設定

ストレージエンジンとはRDBMSの核になる部分で主にデータの格納形式等を決定している部分です。

詳しくは下記の記事を参考にして下さい。

- [徹底比較!! MySQLエンジン](https://thinkit.co.jp/free/article/0608/1/1/)
- [公式](https://dev.mysql.com/doc/refman/5.6/ja/storage-engines.html) を

結論から言うとよほど特殊な状況化を除き `InnoDB` の一択で良いです。

理由はRDBMSで重要な機能であるトランザクションを扱えるのはこのエンジンだけだからです。

ちなみにテーブルを作成する時もストレージエンジンを設定する事は可能です。

万が一特定のテーブルだけ別のストレージエンジンを利用したい場合はテーブル作成時に指定する方法を取れば良いでしょう。

### innodb_file_per_table

ストレージエンジン `InnoDB` に関する設定の1つです。

`innodb_file_per_table` はテーブルごとに専用のテーブルスペースを作成する為の設定です。

これを設定しておかないと古いデータを消してディスク容量を確保したい時にかなり面倒です。

詳しくは下記の記事を参考にして下さい。

- [【AWS】RDS for MySQLで共有テーブルスペースに構成変更する際の所要時間を実測してみた](https://dev.classmethod.jp/server-side/db/rds-mysql-innodb_file_per_table/)
- [【MySQL】肥大化したInnoDBテーブルを圧縮機能で縮小する方法！](https://engineers.weddingpark.co.jp/?p=622)

### innodb_file_format

ストレージエンジン `InnoDB` に関する設定の1つです。

圧縮機能が利用可能になる `Barracuda` へ変更しておきます。

### innodb_default_row_format

ストレージエンジン `InnoDB` に関する設定の1つです。

ここでは `DYNAMIC` を指定しています。

ちなみに圧縮機能が有効なのは `COMPRESSED` です。

圧縮機能を有効にすれば確かにテーブルのデータ容量を節約する事は出来ますが、パフォーマンスが低下するという弱点があります。

その為、普段は `DYNAMIC`を指定しておくのが無難だと私は考えます。

テーブル作成時に `ROW_FORMAT=COMPRESSED` を指定すれば圧縮機能を有効に出来ます。

詳しくは [こちらの記事](https://engineers.weddingpark.co.jp/?p=622) を参考にして下さい。

### default_password_lifetime

デフォルト設定だと365日でパスワードを強制変更するようにMySQLからエラーが出るようになっています。

運用中にこのエラーが出るとサービスが止まってしまうのでこの設定を明示的に無効化しています。

※ MySQL 5.7.11からはデフォルトで0に設定が変更されたようなのでこの設定は不要ですが念の為記載しておきます。

### auto-rehash

LinuxのようにTABキーでテーブル名やSQLの補完が出来るようになります。

よって設定しておきましょう。

### slow_query_log

スロークエリを出力する設定です。

名前の通り時間がかかっているSQLをログとして出力する為の設定です。

### long_query_time

どの程度時間がかかったらスロークエリと見なすかの設定です。

個人的には厳し目の設定にしておくのが良いと考えます。

ここでは0.1秒を設定しています。

遅いSQLはそれだけサーバのリソースを消費し、ユーザー体験にも悪影響を与えるので厳し目に設定を行っておき常に監視しておくのが良いでしょう。

### slow_query_log_file

スロークエリが出力されるファイルパスです。

ここでは `/var/log/mysql-slow-query.log` を設定しています。

スロークエリは `mysqldumpslow` というコマンドで集計する事が可能です。

詳しくは [こちらの記事](https://webmake.info/mysql%E3%81%AE%E3%82%B9%E3%83%AD%E3%83%BC%E3%82%AF%E3%82%A8%E3%83%AA%E3%83%AD%E3%82%B0%E3%82%92mysqldumpslow%E3%81%A7%E5%88%86%E6%9E%90%E3%81%99%E3%82%8B/) を参考にして下さい。

## 設定を反映する

説明が長くなってしまいましたが、以上がMySQLで最低限設定しておくべき項目です。

設定内容を反映した `/etc/my.cnf` を載せておきます。

```
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html

[mysqld]
#
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.
# innodb_buffer_pool_size = 128M
#
# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin
#
# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock

# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0

log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid

character-set-server=utf8mb4
collation-server=utf8mb4_bin
skip-character-set-client-handshake
default-storage-engine=InnoDB
innodb_file_per_table=1
innodb_large_prefix=1
innodb_file_format=Barracuda
innodb_default_row_format=DYNAMIC
default_password_lifetime = 0
slow_query_log = 1
long_query_time = 0.1
slow_query_log_file = /var/log/mysql-slow-query.log

[mysql]
auto-rehash
default-character-set = utf8mb4

[mysqldump]
default-character-set = utf8mb4
```

スロークエリログの設定に関しては `slow_query_log_file` で指定したファイルを作成し書き込み権限を与えておく必要があります。

以下のコマンドを実行しておきましょう。

```bash
sudo touch /var/log/mysql-slow-query.log
sudo chown mysql:mysql /var/log/mysql-slow-query.log

```

準備が終わったら、MySQLサーバの再起動を行いましょう。

```bash
sudo service mysqld restart
```

続いてMySQLサーバにログインを行い、設定が反映されているか確認を行います。

`SHOW VARIABLES LIKE 'char%';` で文字コードの確認を行います。

以下のように表示されていればOKです。

```
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8mb4                    |
| character_set_connection | utf8mb4                    |
| character_set_database   | utf8mb4                    |
| character_set_filesystem | binary                     |
| character_set_results    | utf8mb4                    |
| character_set_server     | utf8mb4                    |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.01 sec)
```

※ `character_set_system` と `character_set_system` に関しては `utf8mb4` になっていなくてもOKです。

続いて `SHOW VARIABLES LIKE 'slow%';` でスロークエリの設定を確認します。

以下のように `slow_query_log` が `ON` になっていればOKです。

```
+---------------------+-------------------------------+
| Variable_name       | Value                         |
+---------------------+-------------------------------+
| slow_launch_time    | 2                             |
| slow_query_log      | ON                            |
| slow_query_log_file | /var/log/mysql-slow-query.log |
+---------------------+-------------------------------+
3 rows in set (0.00 sec)
```

`SHOW VARIABLES LIKE 'long_query%';` でスロークエリの秒数設定を確認します。

以下のように表示されていればOKです。

```
+-----------------+----------+
| Variable_name   | Value    |
+-----------------+----------+
| long_query_time | 0.100000 |
+-----------------+----------+
1 row in set (0.00 sec)
```

ここで紹介した物以外にも様々な設定があります。

近年では [Amazon Aurora](https://aws.amazon.com/jp/rds/aurora/) のような便利なサービスが出てきたので昔ほど設定内容に詳しい必要はなくなりました。

しかしSQLの性能はアプリケーションの応答速度やサーバリソースに大きな影響を与えるので知っておいて損はない知識です。

興味がある方はより詳しく調べてみると良いでしょう。

特に `innodb_buffer_pool` は重要な設定項目の1つです。

（参考）[MySQL 5.7 インストールと設定メモ](https://blog.apar.jp/linux/6769/)

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

## テーブルの作成

ますは簡単なテーブルを作成してみましょう。

この資料の最初で例に上げた商品テーブルを作成するSQLは下記のようになります。

mysqlサーバにログインを行い以下のSQLを実行しましょう。

`USE ojt_db;` を実行してデータベースの選択をするのを忘れないようにしましょう。

```mysql
CREATE TABLE `products_categories` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(255) COLLATE utf8mb4_bin NOT NULL,
  `lock_version` int(10) unsigned NOT NULL DEFAULT '0',
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin ROW_FORMAT=DYNAMIC;
```

```mysql
CREATE TABLE `products` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `category_id` int(10) unsigned NOT NULL,
  `name` varchar(255) COLLATE utf8mb4_bin NOT NULL,
  `price` int(10) unsigned NOT NULL DEFAULT '0',
  `lock_version` int(10) unsigned NOT NULL DEFAULT '0',
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  CONSTRAINT `fk_products_01` FOREIGN KEY (`category_id`) REFERENCES `products_categories` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin ROW_FORMAT=DYNAMIC;
```

`SHOW TABLES;` を実行してみましょう。

テーブルが作成されている事が確認出来ます。

```sql
mysql> SHOW TABLES;
+---------------------+
| Tables_in_ojt_db    |
+---------------------+
| products            |
| products_categories |
+---------------------+
2 rows in set (0.00 sec)
```

`SHOW TABLES;` で確認出来るのはテーブルの一覧だけです。

テーブルの構造を再度確認したい場合は `SHOW CREATE TABLE [テーブル名]` を実行します。

```sql
mysql> SHOW CREATE TABLE products;
+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table    | Create Table                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| products | CREATE TABLE `products` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `category_id` int(10) unsigned NOT NULL,
  `name` varchar(255) COLLATE utf8mb4_bin NOT NULL,
  `price` int(10) unsigned NOT NULL DEFAULT '0',
  `lock_version` int(10) unsigned NOT NULL DEFAULT '0',
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `fk_products_01` (`category_id`),
  CONSTRAINT `fk_products_01` FOREIGN KEY (`category_id`) REFERENCES `products_categories` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin ROW_FORMAT=DYNAMIC |
+----------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

## データの登録

次にデータの登録を行ってみましょう。

以下のSQLを実行して下さい。

```mysql
INSERT INTO products_categories (`name`) VALUES('ドリンク');
INSERT INTO products_categories (`name`) VALUES('お菓子');
```

```mysql
INSERT INTO products (`category_id`, `name`, `price`) VALUES(1, 'コーラ', 150);
INSERT INTO products (`category_id`, `name`, `price`) VALUES(2, 'ポテチ', 100);
```

`Query OK, 1 row affected (0.00 sec)` と表示されればデータ登録に成功しています。

これがデータ登録を行う為のINSERT文の基本になります。

書式をまとめると下記のようになります。

プログラミング言語と比べると簡単に感じるのではないでしょうか。

```sql
INSERT INTO [テーブル名] (`カラム名`, `カラム名`, ...) VALUES ([値], [値], ...);
```

