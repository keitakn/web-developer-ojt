# データベース設計

この資料ではデータベース設計で押さえるべきポイントを記載します。

基本的にMySQLベースで話を進めさせて頂きますので [RDBMSの基礎](https://github.com/keitakn/web-developer-ojt/tree/master/docs/mysql) を読んでMySQLの基礎を理解してから読み進めるようにお願いします。

## データベース設計で押さえるべきポイント

RDBMS設計には既に様々なノウハウが確立されています。

特に参考になりそうな資料を下記に記載します。

- [SQLアンチパターン](https://www.amazon.co.jp/dp/4873115892)
- [達人に学ぶDB設計 徹底指南書 初級者で終わりたくないあなたへ](https://www.amazon.co.jp/dp/4798124702)
- [現場で役立つシステム設計の原則](https://www.amazon.co.jp/dp/477419087X)

[現場で役立つシステム設計の原則](https://www.amazon.co.jp/dp/477419087X) はデータベースの専門書ではありませんが、押さえるべきポイントが簡潔に守られており、非常に良い教材となっています。

ここでは [現場で役立つシステム設計の原則](https://www.amazon.co.jp/dp/477419087X)「第6章データベースの設計とドメインオブジェクト」をベースに話を進めていきます。

この書籍を保持している場合はこの書籍を読んでもらってから本ドキュメントを読むと、より理解を深める事が出来ます。

### 名前を省略しない

メーカー品番を表すカラム名を例に説明します。

`mk_cd` 等のような意味不明な略称は使わないようにしましょう。

`maker_code` のような誰が見ても分かる命名を行う事を徹底しましょう。

略名が使用されているのはかつてデータベースびテーブル名やカラム名に厳しい文字制限があった時代の名残です。

現在では文字列制限は緩和されており、略名を使う理由はありません。

「テーブル名、カラム名」で何を表わしているか分かるのがベストです。

良い名前を付けるというのは、データベース設計でも重要になります。

[データベースオブジェクトの命名規約](https://qiita.com/genzouw/items/35022fa96c120e67c637) という記事を参考にすると良いでしょう。

また良く使われる項目には既に世の中に標準的な仕組みがあったりします。

例えば、性別は [ISO 5218](https://en.wikipedia.org/wiki/ISO/IEC_5218) で仕様が決められています。

以下が正しい仕様です。

```
0 = not known
1 = male
2 = female
9 = not applicable
```

man、womanではなくmale、femaleです。

良く使うユーザー属性名も [OpenIDConnect 5.1.Standard Claims](http://openid-foundation-japan.github.io/openid-connect-core-1_0.ja.html#StandardClaims) で物理名が定められています。

日本国内のみで利用する想定であれば [データベース列名の名前付け（英単語での）採用例を集めてみた](https://qiita.com/otagaisama-1/items/4d7e2eb5c274e9fce664) も参考になります。

世の中の流れはグローバル化なので今後サービス開発を行う時は多言語に対応する事がマストになってくると考えられます。

[OpenIDConnect 5.1.Standard Claims](http://openid-foundation-japan.github.io/openid-connect-core-1_0.ja.html#StandardClaims) のような国際的に利用されているものがあれば日本独自の慣例よりも国際基準を優先したほうが良いでしょう。

### 適切なデータ型を利用する

たまに全てを文字列型で保存しているような設計を見かけますが、数値を扱うのであれば数値型を入れるべきだし、日付を扱うなら日付型を使うべきです。

細かい事を言うと数値型だけでも以下のようにたくさんの種類があります。

- INTEGER
- INT
- SMALLINT
- TINYINT
- MEDIUMINT
- BIGINT

これらは扱える数値の範囲が異なります。（詳しくは [公式ドキュメント](https://dev.mysql.com/doc/refman/5.6/ja/integer-types.html) を参照）

これらも踏まえて適切なデータ型を利用しましょう。

データ型については [公式ドキュメント](https://dev.mysql.com/doc/refman/5.6/ja/data-types.html) を参考にして下さい。

ただし [ENUM型](https://dev.mysql.com/doc/refman/5.6/ja/enum.html) は注意して利用しましょう。

テーブル定義は後で変更する事も可能ではありますが、プログラムと比較して後で変更するコストが大きいです。

ENUMを使うとそのカラムに対する拡張性は完全に失われるので注意して利用しましょう。

ちなみに先程紹介した性別コードのようにコードが決まっている物は数値型で管理したほうがデータベースの容量は少なくて済みます。

数値型のほうが文字列よりもデータを検索する効率も良い事を覚えておきましょう。

```
0 = not known
1 = male
2 = female
9 = not applicable
```

### 制約を適切に利用する

制約は必ず利用しましょう。

データベースが持つ3つの制約は下記の通りです。

- NOT NULL制約
- ユニーク制約
- 外部キー制約

これらを利用する事の重要性について説明します。

#### NOT NULL制約

ユーザー情報を管理するテーブルを例に説明します。

ユーザー情報には以下の3つがあります。

- ユーザーID
- メールアドレス
- 電話番号

この中でユーザーIDとメールアドレスは必須ですが、電話番号の登録が必須ではありません。

さらに以下の条件を満たしている必要があると仮定します。

- メールアドレスはユーザー1人につき、1つまで登録可能
- メールアドレスは全ユーザーでユニークになる必要がある
- 電話番号は1人のユーザーが何個でも登録出来る

普通に考えると下記のようなテーブル構造になります。

```mysql
CREATE TABLE `users` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `email` varchar(128) COLLATE utf8mb4_bin NOT NULL,
  `phone_number` varchar(32) COLLATE utf8mb4_bin,
  `lock_version` int(10) unsigned NOT NULL DEFAULT '0',
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  UNIQUE KEY `uq_users_01` (`email`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin ROW_FORMAT=DYNAMIC;
```

結論から言うとこのテーブル構造は良い構造ではありません。

電話番号登録がないユーザー数だけ `phone_number` カラムに `NULL` が登録されます。

`NULL` は未知を表すデータ型です。

データベースは既知の事実を記録する仕組みです。

また `NULL` を許可するとプログラム側で常に `NULL` かどうかをチェックするコードが必要になり、複雑化の原因になります。

このようなケースでは下記のようなテーブル構造にするのが良いでしょう。

```mysql
CREATE TABLE `users` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `lock_version` int(10) unsigned NOT NULL DEFAULT '0',
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin ROW_FORMAT=DYNAMIC;

CREATE TABLE `users_emails` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `user_id` int(10) unsigned NOT NULL,
  `email` varchar(128) COLLATE utf8mb4_bin NOT NULL,
  `lock_version` int(10) unsigned NOT NULL DEFAULT '0',
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  UNIQUE KEY `uq_users_emails_01` (`user_id`),
  UNIQUE KEY `uq_users_emails_02` (`email`),
  CONSTRAINT `fk_users_emails_01` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin ROW_FORMAT=DYNAMIC;

CREATE TABLE `users_phone_numbers` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `user_id` int(10) unsigned NOT NULL,
  `phone_number` varchar(32) COLLATE utf8mb4_bin,
  `lock_version` int(10) unsigned NOT NULL DEFAULT '0',
  `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `idx_users_phone_numbers_01` (`user_id`),
  KEY `idx_users_phone_numbers_02` (`phone_number`),
  CONSTRAINT `fk_users_phone_numbers_01` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_bin ROW_FORMAT=DYNAMIC;
```

テーブルを3つに分けました。

これで電話番号を登録しないユーザーがいたとしても、`users_phone_numbers` テーブルにデータが登録されないだけで `NULL` データを回避する事が出来ます。

さらにこの構造にした事で後で要件が変化してメールアドレスの登録が必須でなくなったとしても対応が可能になっています。

データベース設計には正規化と呼ばれるテクニックがあります。

詳しくは [4ステップで作成する、DB論理設計の手順とチェックポイントまとめ](https://qiita.com/nishina555/items/a79ece1b54faf7240fac) を見て下さい。

しかし正規化の知識がない状態であったとしても、`NOT NULL` 制約の利用を徹底する事で自然と正規化されたテーブル設計になります。

#### ユニーク制約

テーブル内で一意な値になる事を担保する機能です。

先程例に上げた `users_emails` テーブルの下記に注目しましょう。

```sql
UNIQUE KEY `uq_users_emails_01` (`user_id`),
UNIQUE KEY `uq_users_emails_02` (`email`),
```

`user_id` と `email` は重複登録されるとシステムとして成立しなくなります。

特に `email` をログインIDとして利用しているケースでは致命的です。

ログイン時にユーザーを特定する際にどちらのユーザーがログインするか分からなくなってしまいます。

このように重複する事が許されないデータを管理する時は必ずユニーク制約を利用しましょう。

ユニーク制約を使わずにプログラムで重複チェックを行う方法もあります。

- データベース登録前にSELECTでデータベースを検索する
  - 検索結果が見つからない場合はユーザー情報をデータベースに登録する
  - 検索結果が1件以上存在する場合は登録エラーを出す

結論から言うとこの方法はアンチパターンです。

ほぼ同時刻に同じメールアドレスからリクエストを受けた場合、重複登録を完全に防ぐ事は出来ません。

データベースが出来る事を無理にプログラムでやろうとするよりもデータベースの仕組みを適切に利用する事を考えましょう。

#### 外部キー制約

`users_emails` テーブルの以下に注目しましょう。

```sql
CONSTRAINT `fk_users_emails_01` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`)
```

これがMySQLにおける外部キー制約を表す記述です。

`users_emails` テーブルはユーザーのメールアドレスを管理するテーブルです。

`users` テーブルに存在しないユーザーIDが `users_emails` テーブルに登録されているという事はあってはいけません。

このような状況を防ぐ為の仕組みが外部キー制約です。

外部キー制約に関しては反対派意見もあります。

主に以下のような反対意見です。

- テストデータを用意するのが面倒くさい
- パフォーマンスが劣化する
- プログラムでも十分に担保出来る

>テストデータを用意するのが面倒くさい

面倒くさいと言われてしまってはそれまでですが、言い方を変えると常に正しい状態のデータでテストが実施されるので、後から問題が起きるよりは全然良いと考えます。

データ仕様に関してはテスト用データベースにデータを自動登録する仕組みがあるのでそれを利用すればさほど手間ではないでしょう。

>パフォーマンスが劣化する

多少のオーバーヘッドがある事は事実です。

しかし経験上、それが問題になった事は一度もありません。

少なくとも人間に感じるレベルのパフォーマンス劣化が外部キー制約を付けただけで起こるという事はないと考えられます。

>プログラムでも十分に担保出来る

完璧に書かれているプログラムなら確かにそうですが、人間とはミスをする物です。

これはバグのないプログラムしか書かないという前提の考えの元にのみ成り立つ考えで非常に危険な考え方です。

人間が書くプログラムにミスがあってもデータベース側でエラーを出してくれたほうがミスに早い段階で気がつけるので良いと考えます。

### indexを適切に利用する

RDMBSにはindexという検索を効率的に行う為の仕組みがあります。

`users_phone_numbers` テーブルの以下の記述に注目しましょう。

```sql
KEY `idx_users_phone_numbers_01` (`user_id`),
KEY `idx_users_phone_numbers_02` (`phone_number`),
```

ここでは詳しい説明は割愛しますが内部では「B木」と呼ばれるアルゴリズムが利用されています。

（参考）[MySQL with InnoDB のインデックスの基礎知識とありがちな間違い](http://techlife.cookpad.com/entry/2017/04/18/092524)

簡潔に言うとテーブルの特定カラムに対してindex（索引）を追加する事で効率良くデータを見つけるように出来る仕組みです。

`users_phone_numbers` テーブルの場合、ユーザーIDと電話番号でSELECTする事が想定されます。
よって `user_id` と `phone_number` にindexを付けています。

SQLを作る際はindexが適切に利用されているかを確認する必要があります。

確認方法ですが、下記のようにSQLの前に `EXPLAIN ` を付けます。

```sql
mysql> EXPLAIN SELECT * FROM users_phone_numbers WHERE user_id = 3;
+----+-------------+---------------------+------------+------+----------------------------+----------------------------+---------+-------+------+----------+-------+
| id | select_type | table               | partitions | type | possible_keys              | key                        | key_len | ref   | rows | filtered | Extra |
+----+-------------+---------------------+------------+------+----------------------------+----------------------------+---------+-------+------+----------+-------+
|  1 | SIMPLE      | users_phone_numbers | NULL       | ref  | idx_users_phone_numbers_01 | idx_users_phone_numbers_01 | 4       | const |    1 |   100.00 | NULL  |
+----+-------------+---------------------+------------+------+----------------------------+----------------------------+---------+-------+------+----------+-------+
1 row in set, 1 warning (0.00 sec)
```

見方ですが、まず `type` という部分に注目しましょう。

以下の値のどれかが入ります。

- const
- eq_ref
- ref
- range
- index
- ALL

上から効率の良い順番に並んでいます。

`const` が最も効率が良く、 `ALL` が最も効率が悪いという事です。

（参考1）[8.8.2 EXPLAIN 出力フォーマット](https://dev.mysql.com/doc/refman/5.6/ja/explain-output.html)
（参考2）[EXPLAINを見て気をつけること（自分なりの忘付録）](https://qiita.com/Tsuji_Taku50/items/43eb2a41915d03173773)
（参考3）[MySQL EXPLAINのそれぞれの項目についての覚書](https://qiita.com/kzbandai/items/ea02727f4bb539fcedb5)

数百件程度のデータなら `const` でも `ALL` でもほとんど変わりません。

しかし件数が多くなると人間でも感じる程にSQLの性能は劣化していきます。

SQLを作った際は `EXPLAIN` で効率が悪いSQLを書いていないか確認を行いましょう。

ちなみに `UNIQUE KEY` や `PRIMARY KEY` はindexが付いている状態と同様の効果（それ以上の効果）があるので、indexを付ける必要はありません。

#### 全てのカラムにindexを付けるのはダメ

SELECTのパフォーマンスが良くなるからと言って、全てのカラムにindexを付けてはダメです。

indexを増やすとINSERTやUPDATEのSQLにオーバーヘッドが生じてパフォーマンスが劣化します。

またメモリ使用量が増えるのでデータベース全体のメモリ容量が不足する要因になります。

ビジネス要件に対して適切なindexを付けるようにしましょう。

#### 無意味なindexを作らない

良く言われる事は「カーディナリティが低いカラムには作るべきではない」という事です。

カーディナリティとは「あるカラムにおける、取りうる値の種類」のことです。

性別を表すカラムがあり、「男性」「女性」の2種類があったとします。

男女比がほぼ1対1になる場合、性別カラムにindexを作ってもほとんど効果がありません。

また日本の都道府県と当道府県コードを管理するようなレコード数が極端に少ない（最大で47件）テーブルもindexを作る意味はないでしょう。

このようなテーブルは `ALL` で検索する前提のテーブル構造だからです。

- [MySQL with InnoDB のインデックスの基礎知識とありがちな間違い](http://techlife.cookpad.com/entry/2017/04/18/092524)
- [MySQL(InnoDB)でカーディナリティの低いカラムにINDEXを張る](https://qiita.com/hmatsu47/items/2d44c173a9114fd06853)

ちなみにindexはテーブル作成時だけでなく、後で追加する事も可能です。

`ALTER TABLE テーブル名 ADD INDEX インデックス名(カラム名);`

詳しくは下記の記事を参考にして下さい。

（参考）[14.11.1 オンライン DDL の概要](https://dev.mysql.com/doc/refman/5.6/ja/innodb-create-index-overview.html)
（参考）[MySQLのスキーマ変更をオンラインでやるための選択肢比較](https://qiita.com/masamasa/items/0dc0c7f35f2bffc02db1)

単純にindexを追加するだけなら問題ないですが、大きな変更をシステム稼働中に行うのはリスクがあります。

可能ならば一度システムをメンテナンス状態にして書き込みが全く起こらない状況を作ってから、テーブル構造を変更したほうがより安全です。

### 必ず作るカラム

私の場合以下のカラムを必ず全てのテーブルに作ります。

```
`lock_version` int(10) unsigned NOT NULL DEFAULT '0',
`created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
`updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
```

これらはビジネス上必要なデータというよりはデータを管理する上で必要なカラムです。

- lock_version
  - [楽観的ロック](https://qiita.com/qsona/items/01feada22c8f9fe17c7b) を実装する際に利用
  - [Ruby on Rails](http://rubyonrails.org/) ではこのカラムがあると楽観ロックを自動で行ってくれる
- created_at
  - いつレコードが登録されたかを調査する際に利用する
- updated_at
  - いつレコードが更新されたかを調査する際に利用する

場合によってはこれらのカラムがなくても成り立つケースもあるでしょう。

その場合はこれらのカラムを無理に作成する必要はありません。

例えば仕様的にデータを貯めるだけのテーブルで更新が発生しないケースの場合は `lock_version` と `updated_at` は意味のないデータです。

また全てのテーブルの主キーに `id` カラムを利用しています。

これも [Ruby on Rails](http://rubyonrails.org/) を意識した作りです。

本カリキュラムでは [Ruby on Rails](http://rubyonrails.org/) を強く意識している為、このような構造になっています。

全テーブルの主キーを `id` にする手法は [SQLアンチパターン](https://www.amazon.co.jp/dp/4873115892) で [IDリクワイアド](https://qiita.com/fatomy/items/cf048fc0461224ddfbd7) と呼ばれるアンチパターンです。

[Ruby on Rails](http://rubyonrails.org/) 等を利用しない場合はこれも避けたほうが良い設計です。

このあたりは利用するフレームワークの規約等を見て最適な手法を取って頂ければと考えます。

## まとめ

長くなりましたが、要点は以下の3つになります。

- 名前を省略しない
- 適切なデータ型を利用する
- 制約を適切に利用する

この3点を徹底するだけでもかなり良いテーブル設計にする事が出来ます。

データベースはプログラムよりも寿命が長く簡単に構造を変更する事は出来ません。

上達への近道はたくさん動くシステムを設計して慣れる事です。

良い設計が出来るようになるように常に意識しておきましょう。
