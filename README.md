# web-developer-ojt

## Web開発者向けのOJT（仮）

## 目標

未経験者が0からWebサービスを開発しリリース、運用まで出来るだけの技術を身につける為、必要な学習内容をまとめます。

## 事前準備

### 事前準備（各種アカウント）

以下のアカウントの準備が必要です。

- [GitHub](https://github.com/)
- [Qiita](https://qiita.com/about)
- [Google](https://accounts.google.com/SignUp?hl=ja)
- [AWS](https://aws.amazon.com/jp/account/)

[Qiita](https://qiita.com/about) はGoogleアカウントやGitHubアカウントがあれば簡単に作る事が出来ます。

今後、様々なサービスのアカウントが出てきますが、なるべくGoogleアカウントやGitHubアカウントで登録するようにするとアカウントの管理が楽になるのでオススメです。

反面不正利用された時のダメージが大きいので、各アカウントには二段階認証の設定を行っておきましょう。

特にAWSが不正アクセスされると、見に覚えのない金額を請求されたりするリスクがあるので必ず設定を行っておく事をオススメします。

参考までに役に立ちそうなリンクを載せておきます。

- [GitHubで二段階認証に変更する](https://qiita.com/non0311/items/a67a7e7c5599c7ace0c4)
- [2 段階認証プロセスを有効にする](https://support.google.com/accounts/answer/185839?hl=ja)
- [AWSアカウントを取得したら速攻でやっておくべき初期設定まとめ](https://qiita.com/tmknom/items/303db2d1d928db720888)

### 事前準備（開発用のPC）

個人的にはMacをオススメします。
理由としては、以下の点が挙げられます。

1. 持ち運びが楽
1. マウスがなくても操作に困らない（個人の主観による）
1. iOSアプリの開発（本件では触れませんが将来的にiOSエンジニアになる可能性もある為）
1. [UNIX](https://ja.wikipedia.org/wiki/UNIX) ベースなのでサーバでよく使うコマンドがほぼそのまま使えるしローカルに開発環境を作る敷居が低い

4に関してはWindows 10からBashシェルが使えるようになったらしいので、ともかくとしても、3に関しては今後もサポートされる可能性は低いでしょう。

これは個人の主観ですが、Web開発においてはまだまだ `Windows < Mac` だと筆者は感じています。

新たに購入する際のスペックの選び方ですが、CPU、SSDは低めに抑えても、メモリは最低でも16GB積んでおく事をオススメします。

## 具体的な学習内容

下記の通りです。

全ての項目に言えますが座学よりも手を動かす事を重視します。

よって無理に暗記したりする必要はありません。記憶する事は最小限に、手を動かしながら自然と身につくそんなカリキュラムの作成を目指します。

各学習内容はディレクトリごとに分かれてマークダウン形式にまとめてあります。

`task.md` というファイルがその項目における課題でここに載っている成果物を提出し、メンターのレビューを終えたら、その項目は終了となります。

### 1. HTML CSS基礎

以下の資料を見て、HTML、CSSの概要を理解します。

- [HTMLの基礎知識](https://github.com/keitakn/web-developer-ojt/blob/master/docs/html-css/html.md)
- [CSSの基礎知識](https://github.com/keitakn/web-developer-ojt/blob/master/docs/html-css/css.md)

その後 [Progate](https://prog-8.com/) や [ドットインストール](https://dotinstall.com/) で実際に手を動かします。

最終的に自身の自己紹介ページを作成して完了とします。

この時点ではあまり拘らずに概要を理解したら速やかに次のステップに進みましょう。

### 2. Linuxの基礎

[linux](https://github.com/keitakn/web-developer-ojt/tree/master/docs/linux) 配下の資料を見てLinuxの概要とコマンドに慣れます。

ここには載っていませんが、[Git](https://git-scm.com/) と呼ばれるバージョン管理システムに対する理解も重要です。

以下のサイトを参考にGitやGitHubの使い方を理解しましょう。

- [サルでもわかるGit入門](https://backlog.com/ja/git-tutorial/)
- [GitHub超初心者入門](https://qiita.com/nnahito/items/565f8755e70c51532459)

### 3. ネットワークの基礎

### 4. サーバサイド向け言語（環境構築）

環境構築したサーバサイド言語を使って基本的なアルゴリズムとデータ構造を理解します。

### 5. アルゴリズムとデータ構造

### 6. TCP/IPの基礎

### 7. HTTPの基礎

### 8. RDBMSの基礎

MySQLを使う予定です。
インストール方法やコマンド操作の方法、index等の用語を理解する、サーバサイド向け言語からMySQLを利用する方法。

### 9. デザインパターン & DB設計

簡単なアプリケーションを作りながら設計全般について学びます。

### 10. セキュリティ

### 11. テストの自動化

### 12. アプリケーションフレームワーク

### 13. 最終課題

実際に使えるWebサービスとして公開します。

## 間違った内容を見つけたら

PR大歓迎です。（誤字、脱字が結構多いので修正大歓迎です）

もしくは [こちら](https://github.com/keita-nishimoto/web-developer-ojt/issues) からissueを作成して頂いても問題ありません。

issueの内容は「こういう観点も必要」等の内容も大歓迎です。（ただし筆者の力量不足でそれが叶えられない可能性はあります…）

## PRの際の注意点

PRを作成する前に、`textlint` による文書校正を行って下さい。

以下のコマンドで実行が可能です。

- 校正のみを行う場合

```
yarn run lint [実行対象のファイル名、もしくはディレクトリ名]
```

- 校正ルールに従って修正を行う場合（ただし自動で修正されるのは、一部のルールだけです）

```
yarn run lint:fix [実行対象のファイル名、もしくはディレクトリ名]
```

`yarn run lint:all:fix` を実行すると全てのファイルに対して自動修正が行われます。
最低限こちらのコマンドを実行してからコミットをお願いします。

校正ルールに関しては以下の内容を利用しております。

- [textlint-rule-max-ten](https://github.com/textlint-ja/textlint-rule-max-ten)
- [textlint-rule-no-mix-dearu-desumasu](https://github.com/textlint-ja/textlint-rule-no-mix-dearu-desumasu)
- [textlint-rule-preset-ja-technical-writing](https://github.com/textlint-ja/textlint-rule-preset-ja-technical-writing)
- [textlint-rule-preset-jtf-style](https://github.com/textlint-ja/textlint-rule-preset-JTF-style)

Atomを使っている場合、[linter-textlint](https://atom.io/packages/linter-textlint) をインストールするとリアルタイムで校正を行ってくれるのでオススメです。

既存文書でもtextlintに違反しまくっている文書が多々あります。

この辺は時間があれば徐々に修正していきますのでご了承下さい。（それよりもカリキュラムを最後まで作り上げる事を優先したいので）

※ textlintの導入に関しては [快適なMarkdown編集環境](https://qiita.com/daichiii/items/d36f52c45c744177eb7c) という記事を参考にさせて頂きました。
