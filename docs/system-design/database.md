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
  UNIQUE KEY `uq_users_phone_numbers_01` (`user_id`),
  UNIQUE KEY `uq_users_phone_numbers_02` (`phone_number`),
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

#### 外部キー制約s
