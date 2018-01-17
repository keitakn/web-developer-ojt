# 課題1パスワードジェネレータ機能の作成

[ojt-node](https://github.com/keita-nishimoto/ojt-node) 上にパスワードジェネレータと呼ばれるランダムでパスワードを生成するWebフォームを追加して頂きたいです。

## アウトプット

[ojt-node](https://github.com/keita-nishimoto/ojt-node) をcloneして自身のgithubに取り込み、プルリクエストのレビュワーにメンターを追加して下さい。

※ フォークすると私のRepositoryに対してプルリクエストを出す事になるので、あくまで自身のRepositoryに対してマージされるようになってしまうので面倒ですが、cloneしてから自身のgithub上に上げる事をオススメします。

レビューのやり取りはpublicリポジトリだと全世界に公開されてしまいます。

もし抵抗がある場合はprivateリポジトリを作る事をオススメします。（月7$かかりますが・・・）

このあたりの手順は後でWikiにでもまとめます。

## 完了条件

以下の条件を満たして頂きたいです。

- [JavaScriptでパスワードジェネレータを作ろう](https://dotinstall.com/lessons/pwd_generator_js_v3) と同等の仕様が実装されている事
- `http://localhost:3000/password` で対象機能にアクセス出来るようにする事（localhostの部分は実際はVM環境のIPになると思います）
- トップページに `/password` へのリンクが貼られている事
- レビュワーのレビューを通過しており、masterブランチにマージが完了している事

## ポイント

基本的に [JavaScriptでパスワードジェネレータを作ろう](https://dotinstall.com/lessons/pwd_generator_js_v3) で紹介されているコードを参考にしても構いません。

ただし、この動画は初心者向けに作られている事もあり、ES2015以降の新しい構文やscriptの外部ファイル化等が考慮されていません。

このレベルの小さい機能であれば全てhtmlファイル内に書いても同じかもしれませんが、HTMLとJavaScriptは分離して書きましょう。（実戦では同じところに書くというケースのほうが圧倒的に少ないです）

それから以下の2点も意識して頂きたいです。

- ES2015以降の新しい書き方で書くように頑張ってみましょう。
- 変数名や関数名を付ける時は分かりやすい名前を心がけましょう。

良い名前をつけるには以下の記事等が参考になります。

- [よりよいネーミングを目指して](https://qiita.com/takasek/items/693c57dc9ddc6c1eb1ba)
- [うまくメソッド名を付けるための参考情報](https://qiita.com/KeithYokoma/items/2193cf79ba76563e3db6)
- [プログラミングでよく使う英単語のまとめ【随時更新】](https://qiita.com/Ted-HM/items/7dde25dcffae4cdc7923)

※ このあたりの方法はかなり重要なので別途Wikiあたりにまとめます。。。

逆にHTMLの見た目やデザイン等は手を多少手を抜いて良いです。（今回の課題の本質ではないので）

[ojt-node](https://github.com/keita-nishimoto/ojt-node) のsampleのページではbootstrapというCSSフレームワークを使っています。

[こちら](http://getbootstrap.com/docs/4.0/components/alerts/) がドキュメントになります。

# 課題2バイナリサーチを行う関数の実装

[ojt-node](https://github.com/keita-nishimoto/ojt-node) 上に [バイナリサーチ](https://ja.wikipedia.org/wiki/%E4%BA%8C%E5%88%86%E6%8E%A2%E7%B4%A2) を行う関数の実装をお願いします。

- src/algorithm/search.js内に `binarySearch` を追加
- src/tests/algorithm/search.test.js内に作った関数のテストコードを追加

上記の2点の対応が必要になります。

## アウトプット

プルリクエストのレビュワーにメンターを追加して下さい。

## 完了条件

- 数値の検索が出来る事
- 関数のインプットとして検索対象の配列と検索対象の数値の2つを受取る事
- 関数のアプトプットとしては、検索対象となる配列のindexとループ回数の2つを返す事
- 検索対象の数値が見つからなかった場合はErrorを返す事
- レビュワーのレビューを通過しており、masterブランチにマージが完了している事

## ポイント

今回は比較的簡単なアルゴリズムを実装しながら、テストコードの書き方とコマンドでのNode.jsの使い方を覚えてもらう事が目的です。

課題1と同じようにES2015以降の書き方や、良い名前を考える点を意識しましょう。

# 課題3 Qiita APIを使ってユーザー検索フォームを作成する

まずは完成品を見たほうが早いと思うので、[こちら](http://ec2-13-115-160-223.ap-northeast-1.compute.amazonaws.com:3000/qiita/users) をご覧下さい。

例えばQiitaの公式アカウントの https://qiita.com/qiita だったら "qiita" がユーザーIDになります。

フォームからユーザーIDを受け取り、Qiita APIにそれを渡し、Qiitaのユーザー名とプロフィール画像を画面に出力します。

## アウトプット

完了の条件を満たしているプルリクエストを送り、メンターのレビューを通過する事。

※ [ojt-node](https://github.com/keita-nishimoto/ojt-node) 上に作成して下さい。

## 完了条件

- URLは `/qiita/users` として下さい。（[こちら](http://ec2-13-115-160-223.ap-northeast-1.compute.amazonaws.com:3000/) のように「Qiitaユーザー検索」へのリンクも忘れずに追加しましょう）
- htmlファイルを直接利用するのではなく [ejs](https://github.com/mde/ejs) のテンプレートエンジンを利用して下さい。
- Qiita APIへの通信は [axios](https://github.com/axios/axios) というライブラリを利用して下さい。
- フォームにはValidation処理を実装し、ユーザーIDのValidationを行う事（半角英数字と記号で1文字以上、30文字以内の文字列のみ許可する事）
- 見つからない場合はエラーメッセージをフォームの下に表示して下さい。
- Qiita APIに通信を行う処理とユーザーIDのValidationは `src/server/domain/Qiita.js` というファイルを作成しQiitaクラスを宣言しその中に実装して下さい。
- Qiitaクラスにはテストコードを実装して下さい。
- フォームの送信には [CSRF攻撃対策](https://www.ipa.go.jp/security/awareness/vendor/programmingv2/contents/301.html) を実施して下さい。
- 各ページには適切なHTTPステータスコードを設定するようにお願いします。（正常時は200, 結果が見つからない時は404, ValidationError時は422、それ以外の異常は500）

## ポイント

今までの課題とは違い難易度が格段に高いと思います。

さらに作業量も多いので、プルリクエストを複数回に分けて進める事を推奨します。

PRを段階的に送り、1ステップをクリアする毎に次のPRの作成に取り掛かります。

例えば以下のような形で進めると全部いっきにやるより、問題解決が楽になると思います。

※ こうするとレビューするメンターも楽だったりします。

- 最初のPRはフォームを作成し、フォームからPOSTパラメータを送信しユーザーIDを受け取れるところまで。
- 2回目のPRはQiita APIからデータを取得するテストコードを書くところまで。
- 3回目のPRはQiitaユーザーIDのValidation処理を行うメソッドのテストコードを書くところまで。
- 4回目のPRは作成したQiitaクラスを利用し、Webフォームから正常に検索出来るパターンのみを実装する。
- 5回目のPRはQiita APIの結果がエラーの場合のエラーメッセージを表示させるところまで。
- 6回目のPRはフォームに入力した値がValidationエラーだった場合にフォームのテキストボックスを赤く表示させる。
- 7回目のPRはCSRF対策を行われた事を想定してセキュリティ対策を実装する。

## この課題で身につける事

- セキュリティも考慮したWebフォームの実装方法
- テンプレートエンジンの使い方
- パッケージの追加インストール方法
- JavaScriptをやる上で避けては通れない非同期プログラミングの実装方法
- WebAPIの使い方
- 実戦的なテストコードの書き方の練習
- POST、GET等のHTTPメソッドの扱い方
- HTTPステータスコードの扱い方
- 複雑な問題を分割し、小さな課題に分離出来る能力（開発を進める上でかなり重要です）

一部、Node.js固有の知識もありますが、基本的にどの言語であっても必要な技術がこの課題を行う事で身につくかと思います。

## ヒント

今回の課題は今までと比較してかなり難しいので、ハマりそうなところを予め記載しておきます。

### テンプレートエンジンについて

`src/views/qiita.ejs` というファイルを定義しましょう。

ファイルの内容は下記のようになります。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  <title lang="ja">OJT JavaScript Qiitaユーザー検索</title>
  <link
    rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.6/css/bootstrap.min.css"
    integrity="sha384-rwoIResjU2yc3z8GV/NPeZWAv56rSmLldC3R/AZzGRnGxQQKnKkoFVhFQhNUwEyJ"
    crossorigin="anonymous"
  >
</head>
<body>
<article>
  <div class="container">
  <h1 lang="ja">Qiitaユーザー検索</h1>
    <% const hasDangerClass = isValidationError ? 'has-danger' : '' %>

    <form method="post" id="qiita-user-form">
      <div class="form-group <%= hasDangerClass %>">
        <label class="form-control-label" for="userId">ユーザーID</label>
        <input
          type="text"
          class="form-control form-control-danger"
          id="userId"
          name="userId"
          placeholder="QiitaのユーザーIDを入れて下さい"
          value="<%= userId %>"
        >
        <% if (isValidationError === true) { %>
          <div class="form-control-feedback">
              <%= errorResponse.message %>
          </div>
        <% } %>
    </div>
      <button type="submit" class="btn btn-primary">送信</button>
    </form>

    <!-- ユーザー情報表示 -->
    <% if (Object.keys(userResponse).length !== 0) { %>
      <div class="card" style="width: 20rem;">
        <img class="card-img-top" src=<%= userResponse.profile_image_url %> alt="QiitaUserImage">
        <div class="card-body">
          <p class="card-text">Qiitaユーザー名:  <%= userResponse.name %></p>
        </div>
      </div>
    <% } %>
    <!-- ユーザー情報表示 -->

    <!-- QiitaAPIのエラー表示 -->
    <% if (Object.keys(errorResponse).length !== 0 && isValidationError === false) { %>
      <div class="alert alert-danger" role="alert">
        <strong><%= errorResponse.message %></strong>
      </div>
    <% } %>
    <!-- QiitaAPIのエラー表示 -->
  </div>
</article>
<script src="https://code.jquery.com/jquery-3.1.1.slim.min.js" integrity="sha384-A7FZj7v+d/sdmMqp/nOQwliLvUsJfDHW+k9Omg/a/EheAdgtzNs3hpfag6Ed950n" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/tether/1.4.0/js/tether.min.js" integrity="sha384-DztdAPBWPRXSA/3eYEEUWrWCy7G5KFbe8fFjk5JAIxUYHKkDx6Qin1DkWx51bBrb" crossorigin="anonymous"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.6/js/bootstrap.min.js" integrity="sha384-vBWWzlZJ8ea9aCX4pEW3rVHjgjt7zpkNpZk+02D9phzyeVkE+jo0ieGizqPLForn" crossorigin="anonymous"></script>
</body>
</html>
```

一見普通のHTMLのように見えますが、この中で変数を利用したり、別のテンプレートファイルを呼び出したりする事が出来ます。

こうする事でHTMLを部品化し共通化する事が容易になります。

下記の記事が参考になりますので、載せておきます。

https://qiita.com/y_hokkey/items/31f1daa6cecb5f4ea4c9

通常はプロジェクトルートに `views` というディレクトリを作成するのですが、今回は `/src/views` の下に作るので以下のような設定を `app.js` で行っておきます。

```javascript
const express = require('express');
const ejs = require('ejs');

const app = express();
const port = 3000;

app.set('views', __dirname + '/src/views');
app.engine('ejs', ejs.renderFile);

// 利用する時はこんな感じで利用する（renderParamsには好きな値を定義出来る）
const renderParams = {
  userId: '',
};

res.status(200).render('qiita.ejs', renderParams);
```

### Qiitaクラスを外部から利用可能にする

以下のように宣言します。

```javascript
class Qiita {
  // この中に必要な処理を書く
}

module.exports = Qiita;
```

`module.exports = Qiita;` がポイントです、これを行う事で外部から利用出来る状態になります。

下記は `app.js` から呼び出す際の例です。

```javascript
const Qiita = require('./src/server/domain/Qiita.js');

const qiita = new Qiita();
```

### フォームの値を受取るには

サンプルで用意したHTMLはsubmitボタンが押された時に `/qiita/users` に対してPOSTリクエストが送信されます。

つまり、POST（検索する処理）とGET（検索フォームを表示させる）で同じURLだけど行うべき処理が全く異なるという事です。

まずはGET, POSTでそれぞれURLルーティングが出来るような設定を追加する必要があります。

POSTデータを受取る為には、HTTPリクエストのBodyを解析する必要があります。

`body-parser` というライブラリを調べてみましょう。

### Qiita APIについて

[公式ドキュメント](https://qiita.com/api/v2/docs) を見るのが最も良いと思います。

今回の課題で利用するのは、`https://qiita.com/api/v2/users/{user_id}` のみで認証も不要です。

よってアクセストークンが・・・みたいな記述はとりあえず読まなくても大丈夫です。

※ 本カリキュラムの後半ではこのあたりの事も学習します。

Macのターミナルで `curl -v https://qiita.com/api/v2/users/qiita` と実行してみて下さい。

JSON形式でレスポンスが返却されるかと思います。

これをNode.jsのプログラム上から [axios](https://github.com/axios/axios) を使って呼び出し、結果を表示するという訳です。

>認証している状態ではユーザごとに1時間に1000回まで、認証していない状態ではIPアドレスごとに1時間に60回までリクエストを受け付けます。

とQiitaの公式に書いてあるので、もし予期しないエラーが返ってきたらこの件を疑ってみて下さい。

### 非同期処理について

Qiita APIを使う時に非同期プログラミングについての知識が必要になります。

[axios](https://github.com/axios/axios) はPromiseと呼ばれる非同期処理を行う為のオブジェクトを返してきます。

かなり長いですが [JavaScript Promiseの本](http://azu.github.io/promises-book/) に詳しく仕様が書いてあります。

[MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Using_promises) も確認しておいたほうが良いでしょう。

参考になりそうなQiitaの記事を何個か載せておきます。

- [JavaScriptのPromise](https://qiita.com/ysk_1031/items/888a84cb259cec4e0625)
- [JavaScriptの同期、非同期、コールバック、プロミス辺りを整理してみる](https://qiita.com/YoshikiNakamura/items/732ded26c85a7f771a27)

また `async` `await` というのはPromiseをより便利にした書き方で非同期処理の書き方がシンプルになります。

下記の記事が参考になります。

- [async/await 入門（JavaScript）](https://qiita.com/soarflat/items/1a9613e023200bbebcb3)

内部で利用されているのは結局PromiseなのでPromiseの基本的な動きを理解する事が大事です。

このあたりは最初はかなり難しく感じるかと思いますが、JavaScript開発をやる上で非同期プログラミングは避けては通れないので、この課題を通じて理解するようにしましょう。

# 課題4 Qiita APIを使って記事の一覧ページを作成する

これがJavaScript基礎の最終課題になります。

まずは [完成品](http://ec2-13-115-160-223.ap-northeast-1.compute.amazonaws.com:3000/qiita/items) を見てみましょう。

こんな感じでQiitaの記事の一覧を取得し、表示させます。

初期状態で表示させるのは5件で「次の5件を表示する」をクリックされたら、次の5件を表示して下さい。

Qiita APIとの通信中はプログレスバーを表示させて下さい。

HTMLは [完成品](http://ec2-13-115-160-223.ap-northeast-1.compute.amazonaws.com:3000/qiita/items) を参考にして頂ければ問題ありません。

また [トップページ](http://ec2-13-115-160-223.ap-northeast-1.compute.amazonaws.com:3000/) へのリンクの追加も忘れずにお願いします。（Qiita投稿一覧）

## 最初に `/qiita/items` に遷移した時の挙動

この課題はサーバ側の実装とクライアント側（ブラウザで動くJavaScript）の両方を実装する必要があります。

最初に `/qiita/items` にページ遷移した際は課題3で作ったQiitaクラスを使って、Qiitaから最新記事の一覧を取得します。

この時に利用するAPIは `https://qiita.com/api/v2/items` です。

QiitaクラスにこのAPIを使って、記事の一覧を取得するメソッドを実装しましょう。

このAPIの結果を画面に表示させます。

`src/views/qiita_items.ejs` というファイルを新規で作成しこのテンプレートに結果を渡して表示して下さい。

なお、記事の取得に失敗した場合（APIの結果が200以外だったら）以下のようにエラー画面を表示させます。

![qiita-items-sync-error](https://user-images.githubusercontent.com/11032365/34081525-2a1adc58-e391-11e7-9665-c36308e92c7e.png)

エラーの表示用に `src/views/qiita_error.ejs` というエラー用のテンプレートファイルを別途用意して下さい。

ここまでの実装はサーバ側のプログラムだけで実現出来るハズです。

## ブラウザから次の5件を取得するリクエストが来た時の挙動

ここからが結構複雑なので、以下のシーケンス図を用いて説明します。

![javascript-task4](https://user-images.githubusercontent.com/11032365/34081565-bc3cba2a-e391-11e7-8f3c-8adfbadf290a.jpg)

### 1. POST /api/qiita/itemsへリクエストを送信する

ブラウザからサーバ側に対してリクエストを送信します。

このリクエストを送信する処理は `/public/js/qiita.js` というファイルを新規で作成しそこで行いましょう。

要求する際にページパラメータを渡してあげる必要があります。

リクエストBodyに `page` を含めて下さい。

ここで `page` に渡す値は初期値が1なので、最初は2、2回目にButtonがクリックされた際は3と1づつ増えていきます。

直接Qiita APIにリクエストを行うのではなく、Node.jsのサーバを通じて行うのがポイントです。

`/api/qiita/items` というルーティングを定義しましょう。受け付けるリクエストメソッドはPOSTです。

何故直接、Qiita APIに通信するのが駄目なのか？という理由ですが、このあたりはセキュリティの学習の際に説明します。

ともかくこうする事で、Qiitaクラスに作成した記事の一覧を取得するメソッドを使いまわす事が出来るハズです。

### Qiita APIから次のページの記事一覧を取得

シーケンス図で言うところの2, 3の実装です。

`/api/qiita/items` でQiitaクラスを使って記事の一覧を取得しましょう。

結果をJSON形式で返却しましょう。

下記のようなJSONデータが返すように実装出来ればOKです。

```json
{
  "items": [
    { "id": "545da3485f05598461b9", "title": "記事1", "url": "https://qiita.com/t-mochizuki/items/562a758e5e23f25f7899"},
    { "id": "545da3485f05598461b1", "title": "記事2", "url": "https://qiita.com/t-mochizuki/items/545da3485f05598461b1"},
    { "id": "545da3485f05598461b2", "title": "記事3", "url": "https://qiita.com/t-mochizuki/items/545da3485f05598461b2"},
    { "id": "545da3485f05598461b3", "title": "記事4", "url": "https://qiita.com/t-mochizuki/items/545da3485f05598461b3"},
    { "id": "545da3485f05598461b4", "title": "記事5", "url": "https://qiita.com/t-mochizuki/items/545da3485f05598461b4"}
  ]
}
```

`page` パラメータにはバリデーションが必要です。

課題3の時と同じくQiitaクラスにバリデーションを用意しましょう。

バリデーションエラーになった場合は以下のようにエラーを示すJSON形式のデータを返しましょう。

```json
{ "code": 422, "message": "Unprocessable Entity" }
```

`page` は1から100の整数型を受け付けるようにしましょう。

文字列の "1" 等は受け付けても良いですが、 "abc" のような文字列や -1等の範囲外の数値は受け付けないようにしましょう。

### Qiita APIからの結果を元にJSONを解析してHTMLを生成する

シーケンス図で言うところの4, 5の実装です。

`/public/js/qiita.js` で `/api/qiita/items` からの通信結果を受け取り、HTMLを組み立てます。

[insertAdjacentHTML](https://developer.mozilla.org/ja/docs/Web/API/Element/insertAdjacentHTML) という機能を利用します。

似たような機能に `innerHTML` がありますが、こちらよりもinsertAdjacentHTMLが高速に動作するのでこちらを利用します。

なお通信結果がエラーだった場合は以下のようにエラーメッセージを表示させましょう。

![qiita-async-error](https://user-images.githubusercontent.com/11032365/34082328-3af58f38-e39f-11e7-9146-6b962409f195.png)

## 課題4で作成するファイルまとめ

新規で作成するファイルは以下の通りです。

- `public/js/qiita.js`（クライアント側のメインロジック）
- `src/views/qiita_items.ejs`（記事を表示させる為のテンプレート）
- `src/views/qiita_error.ejs`（記事取得でエラーが発生した際に利用するテンプレート）

他にもQiitaクラスが実装されている `src/server/domain/Qiita.js` やそれに対応するテストコード `src/tests/server/domain/Qiita.test.js` や `app.js` の修正対象になるかと思います。

## 注意点

以下のような点に注意して実装を行って下さい。

- 課題3と同じくQiitaクラスのメソッドには単体テストを用意しましょう。

- 「次の5件を表示する」を連続で押されると、Qiita APIにリクエストが連続で行われてしまいます。
よって通信終了まではButtonを非表示にする等の対策を実施しましょう。

- `/public/js/qiita.js`（ブラウザ）から `/api/qiita/items`（サーバ）に通信する際は [Fetch](https://developer.mozilla.org/ja/docs/Web/API/Fetch_API/Using_Fetch) を利用しましょう。
まだ実装されていないブラウザもありますが今後こちらのAPIがブラウザからHTTP通信を行う為の標準APIになりますので使い方を覚えておきましょう。

- クライアントからサーバに通信する際にもCSRF対策が必要です。課題3と同じく実装を行いましょう。

## この課題で身につける事

- 高機能なUIを作成する為に必要不可欠なサーバ通信を行うJavaScriptアプリケーションの開発の基礎
- ブラウザからHTTP通信を行う基礎的な技術

近年では [SPA(Single Page Application)](https://qiita.com/takanorip/items/82f0c70ebc81e9246c7a) と呼ばれる手法で開発を行う事が多くなってきました。

この課題の設計・実装方法は少々古いやり方ではありますが、モダンなSPAを開発する為にはこの基礎が重要となります。

完成後、課題3で作成したアプリケーションの性質と何が違うかを見比べてみましょう。

## ヒント

この課題は課題3よりさらに難易度が高い為、つまりそうな箇所を予め記載しておきます。

### Qiita APIの利用制限について

本課題は課題3以上にQiita APIの利用制限に引っかかる可能性が高いです。（下記ドキュメントの利用制限を参照）

https://qiita.com/api/v2/docs

もし利用制限に引っかかった場合は以下の3つが解決策となります。

- 1時間経過するのを待つ（実際はそれより早く利用可能になる）
- 接続元のIPアドレスを変更する（Proxyサーバを用意すれば可能）
- アクセストークンによる認証認可を行う

オススメは [アクセストークンによる認証認可を行う](https://qiita.com/api/v2/docs#%E8%AA%8D%E8%A8%BC%E8%AA%8D%E5%8F%AF) です。

ユーザーの管理画面からトークンを発行出来るので一時的にこのトークンをプログラムに埋め込む事で認証付きのリクエストを行うことが可能になり、利用制限に引っかかる事はまずなくなります。

ただしアクセストークンは外に漏れると、自信のQiitaアカウントが悪用されてしまうリスクがあるので、GitRepositoryには登録しないように注意しましょう。

※ 特にpublicなRepositoryで作業している場合、アクセストークンをGitRepositoryに登録する行為は絶対に避けるべきです。

### Ajaxについて

クライアント側からサーバ側に通信を行い、通信結果を元にJavaScriptでHTMLを組み立てるような手法を [Ajax](https://ja.wikipedia.org/wiki/Ajax) と呼びます。

HTML全体を書き換えるのではなく、必要な部分だけを書き換えるので、ページ全体を書き換えるより時間を短縮出来るのと、通信中もユーザー操作が可能な点がメリットです。

Ajaxでググると、おそらく [jQuery.ajax](http://api.jquery.com/jquery.ajax/) が引っかかる事が多いと思います。

jQueryは昔流行ったJavaScriptのライブラリで一時期はこれなしでは、Web開発は出来ないとまで言われた程でしたが、今では標準のJavaScriptが十分に高機能になったので、このライブラリを利用しない流れになっています。

AjaxでCSRF対策を行う際に `csurf` を利用している場合は `X-CSRF-Token` というHTTPHeaderを送信する必要があります。

`curl` で言うと下記のように送信する必要があります。

```bash
curl -v http://localhost:3000/api/qiita/items \
-X POST \
-H "Content-Type: application/json" \
-H "X-CSRF-Token: 0s4oYoa0-85zSh6fJUr3Qkv8-WQ9ljc56ilo" \
-d '{"page": "1"}'
```

このあたりを理解する為にはHTTPの基礎的な知識が必須となります。

本カリキュラムの [HTTP](https://github.com/keita-nishimoto/web-developer-ojt/blob/docs/master/http/README.md) 等を読み、HTTP通信の基礎を理解しておきましょう。

また `csurf` を利用している場合は `csurf` のドキュメントにも目を通しておきましょう。

### 見本で読み込まれているJavaScriptファイルについて

[完成品](http://ec2-13-115-160-223.ap-northeast-1.compute.amazonaws.com:3000/qiita/items) のJavaScriptをGoogleDeveloperTool等で見てみると、非常に読みにくいJavaScriptFileが読み込まれているかと思います。

このJavaScriptファイルは難読化の処理が施してある為です。

課題を行う皆さんはこのような難読化の処理を行う必要はありませんので、普通に `/public/js/qiita.js` を開発して頂いて問題はありません。

### 大きな課題を分離する能力の重要性

課題3と同じく、プルリクエスト（PR）は複数回に分けて送信しましょう。

今回はこちらからPRの分離方法を指定しませんので、課題をやる皆さんがPRをどのような単位で分けるかを考えてみましょう。

課題を分ける際には以下のような事を意識すると良いです。

- まず正常に動く状態を一刻も早く作る事、色々な条件はありますが、バリデーションエラーやCSRFエラー等のエラー発生時の挙動は後回しにする事で問題が単純化出来ます。
- 正常に動くという状態も色々あります、例えばこの課題であれば、最初の段階（初期表示の5件が表示されている段階）で次の5件を表示するButtonが存在しなかったとしても、最初に5件表示されているという機能は満たしていますので、これでも十分PRを出す事が出来ます。
- 単独でテストが出来る物は分離しやすいので、ユニットテストが可能な範囲は分離しやすいです。

上記の点から言えるのは、一度に全ての機能をマージする必要はないという事です。
PRを細かい単位に分割し、細かい単位でレビューを受ける事は大幅な手戻りを軽減し、品質を向上させます。

これは [アジャイル開発](https://ja.wikipedia.org/wiki/%E3%82%A2%E3%82%B8%E3%83%A3%E3%82%A4%E3%83%AB%E3%82%BD%E3%83%95%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A2%E9%96%8B%E7%99%BA) の基本的な考え方でもあります。

「複雑な課題をいくつかの単純な課題に分離する能力」は開発者にとって非常に重要なスキルです。

身につけようと思っても一朝一夕で身につく物ではありません。

よって普段から意識しておく事が大事です。

[アジャイルな見積りと計画づくり](https://www.amazon.co.jp/dp/B00IR1HYGW/ref=dp_kinw_strp_1) の「ユーザーストーリーの分割」がもっとも参考になりました。

下記のスライドも参考になるでしょう。

https://www.slideshare.net/aratafuji/ss-38057768

課題の分離方法に悩む場合はメンターに相談しましょう。
