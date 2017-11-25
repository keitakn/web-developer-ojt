# 課題1 パスワードジェネレータ機能の作成

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
- `http://localhost:3000/password` で対象機能にアクセス出来るようにする事（localhostの部分は実際はVM環境のIPになると思います。）
- トップページに `/password` へのリンクが貼られている事
- レビュワーのレビューを通過しており、masterブランチにマージが完了している事

## ポイント

基本的に [JavaScriptでパスワードジェネレータを作ろう](https://dotinstall.com/lessons/pwd_generator_js_v3) で紹介されているコードを参考にしても構いません。

ただし、この動画は初心者向けに作られている事もあり、ES2015以降の新しい構文やscriptの外部ファイル化等が考慮されていません。

このレベルの小さい機能であれば全てhtmlファイル内に書いても同じかもしれませんが、HTMLとJavaScriptは分離して書きましょう。（実戦では同じところに書くというケースのほうが圧倒的に少ないです。）

それから以下の2点も意識して頂きたいです。

- ES2015 以降の新しい書き方で書くように頑張ってみましょう。
- 変数名や関数名を付ける時は分かりやすい名前を心がけましょう。

良い名前をつけるには以下の記事等が参考になります。

- [よりよいネーミングを目指して](https://qiita.com/takasek/items/693c57dc9ddc6c1eb1ba)
- [うまくメソッド名を付けるための参考情報](https://qiita.com/KeithYokoma/items/2193cf79ba76563e3db6)
- [プログラミングでよく使う英単語のまとめ【随時更新】](https://qiita.com/Ted-HM/items/7dde25dcffae4cdc7923)

※ このあたりの方法はかなり重要なので別途 Wikiあたりにまとめます。。。

逆にHTMLの見た目やデザイン等は手を多少手を抜いて良いです。（今回の課題の本質ではないので）

[ojt-node](https://github.com/keita-nishimoto/ojt-node) のsampleのページでは bootstrap というCSSフレームワークを使っています。

[こちら](http://getbootstrap.com/docs/4.0/components/alerts/) がドキュメントになります。

# 課題2 バイナリサーチを行う関数の実装

[ojt-node](https://github.com/keita-nishimoto/ojt-node) 上に [バイナリサーチ](https://ja.wikipedia.org/wiki/%E4%BA%8C%E5%88%86%E6%8E%A2%E7%B4%A2) を行う関数の実装をお願いします。

- src/algorithm/search.js 内に `binarySearch` を追加
- src/tests/algorithm/search.test.js 内に作った関数のテストコードを追加

上記の2点の対応が必要になります。

## アウトプット

プルリクエストのレビュワーにメンターを追加して下さい。

## 完了条件

- 数値の検索が出来る事
- 関数のインプットとして検索対象の配列と検索対象の数値の2つを受取る事
- 関数のアプトプットとしては、検索対象となる配列のindexとループ回数の2つを返す事
- 検索対象の数値が見つからなかった場合は Error を返す事
- レビュワーのレビューを通過しており、masterブランチにマージが完了している事

## ポイント

今回は比較的簡単なアルゴリズムを実装しながら、テストコードの書き方と コマンドでのNode.js の使い方を覚えてもらう事が目的です。

課題1 と同じようにES2015以降の書き方や、良い名前を考える点を意識しましょう。

# 課題3 Qiita APIを使ってユーザー検索フォームを作成する

まずは完成品を見たほうが早いと思うので、[こちら](http://ec2-13-115-160-223.ap-northeast-1.compute.amazonaws.com:3000/qiita/users) をご覧下さい。

例えば Qiitaの公式アカウントの https://qiita.com/qiita だったら "qiita" がユーザーIDになります。

フォームからユーザーIDを受け取り、Qiita APIにそれを渡し、Qiitaのユーザー名とプロフィール画像を画面に出力します。

## アウトプット

完了の条件を満たしているプルリクエストを送り、メンターのレビューを通過する事。

※ [ojt-node](https://github.com/keita-nishimoto/ojt-node) 上に作成して下さい。

## 完了条件

- URLは `/qiita/users` として下さい。 （[こちら](http://ec2-13-115-160-223.ap-northeast-1.compute.amazonaws.com:3000/) のように「Qiitaユーザー検索」へのリンクも忘れずに追加しましょう。）
- htmlファイルを直接利用するのではなく [ejs](https://github.com/mde/ejs) のテンプレートエンジンを利用して下さい。
- Qiita APIへの通信は [axios](https://github.com/axios/axios) というライブラリを利用して下さい。
- フォームにはValidation処理を実装し、ユーザーIDのValidationを行う事（半角英数字と記号で1文字以上、30文字以内の文字列のみ許可する事）
- 見つからない場合はエラーメッセージをフォームの下に表示して下さい。
- Qiita APIに通信を行う処理とユーザーIDのValidationは `src/server/domain/Qiita.js` というファイルを作成し Qiita クラスを宣言しその中に実装して下さい。
- Qiita クラスにはテストコードを実装して下さい。
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
- 3回目のPRはQiita ユーザーIDのValidation処理を行うメソッドのテストコードを書くところまで。
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
- 複雑な問題を分割し、小さな課題に分離出来る能力（開発を進める上でかなり重要です。）

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

通常は プロジェクトルートに `views` というディレクトリを作成するのですが、今回は `/src/views` の下に作るので以下のような設定を `app.js` で行っておきます。

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

まずは GET, POSTでそれぞれURLルーティングが出来るような設定を追加する必要があります。

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

それから個人的には下記のQiitaの記事がとても分かりやすいと思います。

- [JavaScriptは如何にしてAsync/Awaitを獲得したのか Qiita版](https://qiita.com/gaogao_9/items/5417d01b4641357900c7)
- [Promiseとasync-awaitの例外処理を完全に理解しよう](https://qiita.com/gaogao_9/items/40babdf63c73a491acbb#_reference-7848204ae40de27d0fc0)

JavaScriptでの非同期処理の書き方の変化が載っていて分かりやすいです。

また `async` `await` というのはPromiseをより便利にした書き方で非同期処理の書き方がシンプルになります。

内部で利用されているのは結局PromiseなのでPromiseの基本的な動きを理解する事が大事です。

このあたりは最初はかなり難しく感じるかと思いますが、JavaScript開発をやる上で非同期プログラミングは避けては通れないので、この課題を通じて理解するようにしましょう。
