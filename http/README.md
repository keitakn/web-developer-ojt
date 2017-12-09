# HTTP

HTTPとは Hyper Text Transfer Protocol の略でブラウザ等のクライアントとWebサーバとの通信を行う為のプロトコル（通信手段です。）

この資料ではWebエンジニアには必須であるHTTPの基本的な事を学びます。

## HTTP通信を実際に行ってみる

まずはHTTPサーバとコマンドで通信してみましょう。

ターミナルを開いて以下のコマンドを実行して下さい。

`curl -v http://laravel.jp`

するとめちゃくちゃ長いですが、下記のような結果が表示されているかと思います。

```
* Rebuilt URL to: http://laravel.jp/
*   Trying 133.242.20.234...
* TCP_NODELAY set
* Connected to laravel.jp (133.242.20.234) port 80 (#0)
> GET / HTTP/1.1
> Host: laravel.jp
> User-Agent: curl/7.54.0
> Accept: */*
>
< HTTP/1.1 200 OK
< Date: Mon, 04 Dec 2017 15:28:21 GMT
< Server: Apache/2.2.22 (Ubuntu)
< X-Powered-By: PHP/5.3.10-1ubuntu3.25
< Set-Cookie: laravel_session=tskf6tvimo2unifp6jb6tj3j01; expires=Mon, 04-Dec-2017 17:28:21 GMT; path=/; HttpOnly
< Set-Cookie: laravel_session=tskf6tvimo2unifp6jb6tj3j01; expires=Mon, 04-Dec-2017 17:28:21 GMT; path=/; httponly
< Cache-Control: no-cache
< Set-Cookie: docs_versions=eyJpdiI6InhoQ1NoMTZDN2lJMHJBcTNyaXZIWGdET21PZXR1TTVrQ3F5cG5GZjAwQ1E9IiwidmFsdWUiOiJuSUtLbmdVSEJSK1N3WlVqZ3pncnRMTGxLTzZUUVBYQk1RRXBnaThMRzFBPSIsIm1hYyI6ImIwYWI2M2RjMDk3MjRiZGI4NGJmNWRkMTBjOWUzMGVkYjQwZjkwNDBhZDE2MzJhZTM3ZjE0ZjViOTAzZThkZGYifQ%3D%3D; path=/; httponly
< Vary: Accept-Encoding
< Transfer-Encoding: chunked
< Content-Type: text/html; charset=UTF-8
<
<!doctype html>

<html lang="ja">

<head>
    <title>Laravel - ウェブ職人のためのPHPフレームワーク</title>
-------長いのでこれより下は省略--------------------------------
```

これは http://laravel.jp/ というWebサーバに対して、`curl` というHTTPクライアントを使って接続した結果となります。

次にこのURLをブラウザ（GoogleChrome）で開いてみましょう。

![laravel](https://user-images.githubusercontent.com/11032365/33560680-c66c4918-d953-11e7-8927-5d6ae7d9d38c.png)

DeveloperToolを開いてみると先に実行した `curl` と似たような `200 OK` とか `GET` みたいな単語が表示されているかと思います。

ブラウザと `curl` では表示されている結果が全く異なりますが、実はこの2つはどちらもHTTP通信を行ってその結果としてHTMLのテキストを受信しているという点では同じです。

ブラウザのほうはHTMLを解釈しレンダリングする能力があるので、人間が見て分かりやすいグラフィカルな表現が出来ているだけです。

これがHTTPによる通信になります。

## HTTPの通信の流れ

先程の `curl` コマンドを元に解説していきます。

まずはじめの `*` で始まっている部分に注目してみましょう。

```
* Rebuilt URL to: http://laravel.jp/
*   Trying 133.242.20.234...
* TCP_NODELAY set
* Connected to laravel.jp (133.242.20.234) port 80 (#0)
```

こんな感じの記述ですね。

裏側では以下のような動きが行われています。

### DNSによる名前解決

まず初めに `laravel.jp` のIPアドレスをDNSサーバに問い合わせします。

DNSサーバは IPアドレスとドメイン名を紐付ける為のサーバです。DNSサーバは限られた組織内で限定公開のものもあれば、インターネット上で公開されているものもあります。

有名なのは Googleが公開している `8.8.8.8` ですね。

下記図の1の部分、これをDNS Queryといいます。

DNSサーバは応答（DNS Response）を返します。

![http-dns](https://user-images.githubusercontent.com/11032365/33562227-1b803f78-d958-11e7-90a9-e084c16e07a4.png)

ちなみにこの時点でIPアドレスが得られなかった場合は下記のようなエラーが返ります。

```
* Rebuilt URL to: http://aaa/
* Could not resolve host: aaa
* Closing connection 0
curl: (6) Could not resolve host: aaa
```

`Could not resolve host` つまり ホスト名をDNSサーバに問い合わせたけど、IPアドレスが返って来なかったという内容です。名前解決が出来なかった等と言ったりします。

### TCPコネクションの確立

HTTPはTCPというプロトコルを使っています。

HTTPは80番ポートを使って通信を行うので、クライアントからWebサーバ（この場合 `laravel.jp`）の80ポートに接続します。

その際の手順は下記の図のようになります。

![http-tcp](https://user-images.githubusercontent.com/11032365/33609863-16fafc3a-da0d-11e7-9ff3-7bd00891330e.png)

1. クライアントからWebサーバに `TCP syn` パケットを送信します。

1. Webサーバからクライアントに `TCP syn/ack` が送信されます。

1. クライアントからWebサーバに `TCP ack` を送ります。

これはTCP通信を始める際のルールで [3ウェイ・ハンドシェイク](https://ja.wikipedia.org/wiki/3%E3%82%A6%E3%82%A7%E3%82%A4%E3%83%BB%E3%83%8F%E3%83%B3%E3%83%89%E3%82%B7%E3%82%A7%E3%82%A4%E3%82%AF) と呼ばれます。

友人とLINE等でやり取りする時に例えると分かりやすいです。（AとBの会話）

1. A（クライアント）「今LINE大丈夫？（`TCP syn`）」
1. B（Webサーバ）「大丈夫だよ！（`TCP syn/ack`）」
1. A（クライアント）「よかった！今日さー〜〜 （`TCP ack` 通信開始）」

ざっくりとしたイメージはこんな感じです。

※ TCP通信に関してはかなり重要なので、TCP/IPのlessonでさらに詳しく説明します。

### HTTP Method "GET" によるファイル要求

クライアントは `TCP ack` を送信後は相手からの反応を待たずに Webサーバに対して `GET` リクエストを送信します。

先程の `curl` コマンドの結果を再度見てみましょう。

```
> GET / HTTP/1.1
```

この `GET / HTTP/1.1` という部分がそれに該当します。

`GET` というのは HTTPMethodの1種でサーバからファイルをダウンロードするという意味です。

つまり `GET / HTTP/1.1` は HTTPのバージョン1.1を使って `Host: laravel.jp` から ファイルをダウンロードする、という意味になります。

これをWebサーバに送信します。

### HTTPステータスコードと要求したファイルの送信

Webサーバは要求を受けると、`HTTP/1.1 200 OK` という文字列と共に要求したファイルが送られてきます。

※ `HTTP/1.1` の部分はWebサーバが利用しているHTTPのバージョンによって変わる可能性があります。

この時、ファイルサイズが例えば1460Byte以上の MSS値（TCPが格納するユーザデータの受信可能なセグメントサイズの最大値） に収まりきらない量の通信が必要な場合は、サーバのOSがTCPセグメンテーションにより分割して複数パケットにして送ります。

下記の図のようなイメージです。

![http-response](https://user-images.githubusercontent.com/11032365/33614880-e2c03e4e-da1b-11e7-94f6-80256ffa2312.png)

以上がHTTP通信の基本的な流れです。

`curl` コマンドやブラウザの結果が表示されるまでは一瞬ですが、その間にこれだけのやり取りが行われている訳です。

## HTTPリクエストの構成

先程の `curl` コマンドの結果からHTTPのリクエストの構成について説明します。

HTTPリクエストは主に3つのパーツに分類されます。

- リクエストライン
- Header
- Body

の3つです。

### リクエストライン

`GET / HTTP/1.1` と記載されている部分がリクエストラインです。

ここでは HTTPメソッド、URLパス、HTTPバージョンの3つが指定されます。

#### HTTPメソッド

クライアントが行いたい処理をサーバに伝える為の決まり事です。

HTTPメソッドの種類は9つあります。

|メソッド|意味|
|:--|--:|:--:|
|GET|リソースの取得|
|POST|新しいリソースを作成する|
|PUT|リソースを更新する|
|PATCH|リソースを部分的に更新する|
|DELETE|リソースの削除|
|HEAD|リソースの削除|
|OPTIONS|リソースがサポートしているメソッドの取得|
|TRACE|プロキシ動作の確認|
|CONNECT|プロキシ動作のトンネル接続への変更|

※ [リソース](https://ja.wikipedia.org/wiki/%E3%83%AA%E3%82%BD%E3%83%BC%E3%82%B9_(WWW)) というのはWebで使われる言葉で意味が若干曖昧な部分がありますが、Webサーバ上に存在するデータの事だと思って下さい。

各メソッドの解説に関しては説明すると長くなるので、参考になりそうな記事を載せておきます。

- [HTTP リクエストメソッド](https://developer.mozilla.org/ja/docs/Web/HTTP/Methods)
- [HTTPメソッド(CRUD)についてまとめた](https://qiita.com/Ryutaro/items/a9e8d18467fe3e04068e)

`curl` コマンドでHTTPメソッドを指定するには次のようにします。

```
curl -v \
-X POST \
https://qiita.com/api/v2/items
```

`POST` の部分に利用したいHTTPメソッドを指定すれば、OKです。

ちなみに `curl` の場合、何も指定しなければ `GET` を使う事になります。

### Header

クライアントから追加情報をWebサーバに渡す時に利用されます。

ユーザーの入力値を渡すという使い方よりは、補足情報的な情報を渡す事が多いです。

HTTP Headerは キー名と値を `: ` で繋いで設定します。

下記のようにリクエストに `-H` オプションを付けて実行します。

`curl` でリクエストHeaderを設定するには以下のように `-H` オプションを付けて実行します。

※ 例として Qiita APIへのアクセスを行います。

```
curl -v \
-X GET \
-H "Authorization: Bearer rgnbfncaidaih4umxfn9yf7znfkx788ax7k55ttg" \
https://qiita.com/api/v2/users/qiita
```

こちらは [Qiita API](https://qiita.com/api/v2/docs#アクセストークン) を `Authorization` に設定している例です。

※ アクセストークンの値はデタラメですので上記のリクエストをコピーして実行しても403エラーが返ってくると思います。実際にご自身でアクセストークンを発行して試してみましょう。 ※ [Qiita API 認証認可](https://qiita.com/api/v2/docs#認証認可) を参照。

Qiitaでは `Authorization` に有効なアクセストークンが入っているとAPIが利用出来る範囲が広まったり、1時間あたりに呼び出せる回数が増えたりします。

以下 Qiita公式より抜粋。

>利用制限
認証している状態ではユーザごとに1時間に1000回まで、認証していない状態ではIPアドレスごとに1時間に60回までリクエストを受け付けます。

このように `Authorization` HeaderはクライアントとWebサーバ間の認証に利用されます。

他にもいくつか標準で定められているHeaderがあるので、詳しくは [MDN HTTP ヘッダー](https://developer.mozilla.org/ja/docs/Web/HTTP/Headers) を参照しましょう。

後でセキュリティのところでやりますが `Set-Cookie` や `Cookie` 等は利用頻度が高いので自然と覚えると思います。

### Body

リクエストの本文です。

先程に例に挙げた `https://qiita.com/api/v2/users/qiita` へのリクエスト等はパラメータであるユーザーID（"qiita" の部分がユーザーID）をURLのパスとして指定しましたが、URLにくっ付ける方法だとデータが大きい場合に非常に長いURLになってしまい分かりにくいという問題や一部のブラウザではURLの長さに制限があったりします。

よって大きなデータを送信する際はリクエストボデイにデータを入れるという方法で対応を行います。

例えば、こんなWebフォームを作ってsubmitボタンをクリックすると↓

![qiita-form](https://user-images.githubusercontent.com/11032365/33724500-e88b0d20-dbb2-11e7-854e-c8bac2a469df.png)

```html
<form method="post" id="qiita-user-form">
  <div class="form-group ">
    <label class="form-control-label" for="userId">ユーザーID</label>
    <input
      type="text"
      class="form-control form-control-danger"
      id="userId"
      name="userId"
      placeholder="QiitaのユーザーIDを入れて下さい"
      value=""
    >
</div>
  <button type="submit" class="btn btn-primary">送信</button>
</form>
```

formが存在するURLに対してPOSTリクエストを送ります。（form の `action` にはURLパスを設定出来るので他のURLに対しても送信出来ます。）

この時に `value` にユーザーが入力した値が送信されますので、Webサーバ側は `name` に指定されている `userId` というキー名でユーザーの入力値を受取る事が出来ます。

同じ事を `curl` で行う為には以下のようにオプションを設定すれば実現出来ます。

URLは  `https://sample.com/qiita/users` だと仮定（実際には存在しません）

```
curl -v \
-X POST \
-H "Content-Type: application/x-www-form-urlencoded" \
-d "userId=qiita" \
https://sample.com/qiita/users
```

このフォームデータの送信に関しては、かなりよく利用するので [MDN フォームデータを送信する](https://developer.mozilla.org/ja/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data) を確認しておくと良いでしょう。

また、階層構造がある複雑なデータを送信したい際は `Content-Type: application/json` を利用するのが有効です。

※ JSONに関してはデータ形式の1種です。詳しくは [こちら](https://dev.classmethod.jp/etc/concrete-example-of-json/) を参照して下さい。

```
curl -k \
-X POST \
-H "Content-Type: application/json" \
-d \
'
{
  "userId": "qiita",
  "list": [10, 100, 200]
}
' \
https://XXXX.execute-api.ap-northeast-1.amazonaws.com/dev/auth/authentication
```

このくらいのデータだと対した差はないですが、Bodyの構造が複雑であれば、JSONを利用したほうが分かりやすくてオススメです。

### Query String

単に `query` と呼ばれる事も多いです。

これはURLの後ろにパラメータをくっつけて送信する方法です。

`curl -v https://qiita.com/api/v2/schema?locale=ja` のようにURLの後ろに `?locale=ja` とします。

こうするとWebサーバ側では `locale` というキー名で `ja` という値が取得出来ます。

複数繋げる場合は `?page=1&per_page=20` のように `&` で繋げます。

基本的には `GET` でオプションパラメータ的な値を送信する際に利用される事が多いです。

## HTTPレスポンスの構成

[HTTP メッセージ](https://developer.mozilla.org/ja/docs/Web/HTTP/Messages) にあるように基本的に3つの要素で構成されます。

### ステータスコード

[HTTP レスポンスステータスコード](https://developer.mozilla.org/ja/docs/Web/HTTP/Status) を確認しながら以降の説明を読んで下さい。

Webサーバからクライアントに レスポンスを返す時にそのリクエストが成功したのか、失敗したのかを示す数値です。

詳しくは [HTTP レスポンスステータスコード](https://developer.mozilla.org/ja/docs/Web/HTTP/Status) を参照するのが良いですが、ざっくりと以下の点を覚えておきましょう。

### 200番台のステータスコード

200番台から始まるステータスコードはリクエストが成功した事を表します。

`GET` なのであれば、クライアントが要求するリソースが見つかったという意味で `200 OK` を返しますし、`POST` の場合は `201 Created` を返しクライアントがリクエストした新しいリソースが作成された事を伝えます。

### 400番台のステータスコード

これはクライアントのリクエストが正しくない等、クライアントに原因がある場合に返されるコードです。

例えば `404 Not Found` は クライアントが要求するリソースが存在しない事を示し、`403 Forbidden` はクライアントにリソースへのアクセス権がない事を表します。

このステータスコードが返却された場合、クライアントはリクエストの方法を見直す必要があります。

### 500番台のステータスコード

Webサーバ側が原因で何らかの問題が発生している場合に返すステータスコードです。

これが返ってきた場合は、400番台と違いクライアント側に出来る事は基本的にありません。

Webサーバの管理対象が自分ではない場合はWebサーバの管理者に問い合わせる等を行うしかないでしょう。

その他、リダイレクト（URLが変更になった際にWebサーバ側で新URLに転送する仕組み）を表す 300番台があります。

`301 Moved Permanently` は自分がWebサーバの開発者でURLを変更した時等に良く使います。

### Header

レスポンスに含まれるHTTPHeaderです。

一番初めに例に上げた `curl -v http://laravel.jp` の結果で言うと下記の部分になります。

```
< Date: Mon, 04 Dec 2017 15:28:21 GMT
< Server: Apache/2.2.22 (Ubuntu)
< X-Powered-By: PHP/5.3.10-1ubuntu3.25
< Set-Cookie: laravel_session=tskf6tvimo2unifp6jb6tj3j01; expires=Mon, 04-Dec-2017 17:28:21 GMT; path=/; HttpOnly
< Set-Cookie: laravel_session=tskf6tvimo2unifp6jb6tj3j01; expires=Mon, 04-Dec-2017 17:28:21 GMT; path=/; httponly
< Cache-Control: no-cache
< Set-Cookie: docs_versions=eyJpdiI6InhoQ1NoMTZDN2lJMHJBcTNyaXZIWGdET21PZXR1TTVrQ3F5cG5GZjAwQ1E9IiwidmFsdWUiOiJuSUtLbmdVSEJSK1N3WlVqZ3pncnRMTGxLTzZUUVBYQk1RRXBnaThMRzFBPSIsIm1hYyI6ImIwYWI2M2RjMDk3MjRiZGI4NGJmNWRkMTBjOWUzMGVkYjQwZjkwNDBhZDE2MzJhZTM3ZjE0ZjViOTAzZThkZGYifQ%3D%3D; path=/; httponly
< Vary: Accept-Encoding
< Transfer-Encoding: chunked
< Content-Type: text/html; charset=UTF-8
```

たくさんの種類が多いので詳しくは [こちら](https://developer.mozilla.org/ja/docs/Web/HTTP/Headers) を参照して下さい。

[HTTPレスポンスヘッダによるセキュリティ対策](https://qiita.com/mint__/items/a4039d3cc659959d9231) に書いてあるようにセキュリティ対策目的で付与されるケースも多いです。

RFCで定義されている物以外にも、Webサーバの実装者が独自のHeaderを定義する事が出来ます。MDNに以下の記述があります。

>'X-' 接頭辞を使用して独自のヘッダーを追加できますが、この慣習は 2012 年 6 月に非推奨になりました。これは、RFC 6648 で非標準のフィールドが標準になったときに発生した不便さのためです。

ただし現行でも `X` から始まる独自のResponseHeaderは結構使われています。（AWSのサービスとか特に独自のHeaderが結構あります。）

例を1つ挙げると `X-Request-Id` というRequestの識別を行う事を目的とした物があります。

これは [Ruby on Rails](http://rubyonrails.org/) という有名なWebアプリケーションを利用した際に自動的にResponseHeaderに含まれます。

### Body

Responseのデータそのものです。

返ってくるデータ形式はHTMLやJSONをはじめとして様々な物があります。

ResponseHeaderの `Content-Type` を見ると、どのような形式のデータかが分かります。

## Webサーバ

Webサービスを作成する為にはWebサーバが必要になります。

Node.js のような単体でWebサーバとしての機能を備えている物もありますが、基本的には全面にWebサーバを置いてWebサービスを配信するケースが多いです。

ここでは代表的な2つを紹介します。

### [Apache](https://ja.wikipedia.org/wiki/Apache_HTTP_Server)

おそらく世界で最も使われているWebサーバです。

[日本語の公式ドキュメント](https://httpd.apache.org/docs/2.4/ja/) はこちらになります。

利用用途としてはApache経由でPHP等のプログラムを実行してそれをWebサービスとして配信する形が最も多いです。

特徴としてはプロセス駆動アーキテクチャでマルチプロセス。

各リクエストをプロセスに割り当てて処理を行う方法です。

この為、各プロセスは独立しており、サーバ側のプログラミングはシンプルになりますが、一度に大量のリクエストが来るとメモリ不足になりやすい等の欠点を抱えています。

### [nginx](https://ja.wikipedia.org/wiki/Nginx)

こちらも有名なWebサーバです。

- [公式ドキュメント](http://nginx.org/en/)
- [日本語ドキュメントLink](http://nginx-ug.jp/link/)

イベント駆動アーキテクチャ、シングルスレッドモデル。

イベントループ方式で起動します。

詳しくは下記の記事を参照して下さい。

https://blog.mosuke.tech/entry/2016/06/04/180122/

同時接続数が増えても消費メモリは少なく、非同期処理なので高速に動作しますが、リクエストの処理時間が長いとキューにリクエストが溜まってしまい、性能が劣化します。

よって一度のリクエストで時間がかかるような処理が多いサービスには向いていません。

## [Apache](https://ja.wikipedia.org/wiki/Apache_HTTP_Server) と [nginx](https://ja.wikipedia.org/wiki/Nginx) どちらを採用するべきか？

どちらも長所と短所があり、どちらが優れているという話ではありません。

下記のような記事を参考にどちらを採用するか考えると良いでしょう。

- [Apacheとnginxどちらを採用すべきかメリット・デメリット比較](https://qiita.com/pink/items/7709218310b5cf11eabe)
- [NginxとApache HTTP Serverの違いメモ](https://qiita.com/tomoyamachi/items/06b2eca14987a30b8fda)
- [ApacheとNginxについての比較](https://www.marineroad.com/staff-blog/8095.html)

## HTTPの学習をさらに進めるには？

HTTPは単純なように見えてかなり奥の深い技術です。

しかし、Webエンジニアにとっては基本となる非常に大切な技術なので、本格的に学習しておく事をオススメします。

※ 一度習得してしまえば、長く使える技術なので非常にオススメです。

学習を進める上で最もオススメなのは [Real World HTTP](https://github.com/oreilly-japan/real-world-http) という書籍です。

HTTPの歴史から、最新仕様であるHTTP2まで対応しているので非常にオススメです。

また [Webを支える技術](https://www.amazon.co.jp/dp/4774142042) という書籍もオススメです。

こちらはHTTPの事だけでなく、良いURLの設計やJSON等のデータフォーマットに関しても記述があるのでWebエンジニアの最初の一冊としても最適だと思います。

ただし内容が少し古い点があるので、以下の点に注意しましょう。

- HTMLは最新仕様の5ではなく古い4系の記述なのでこれをそのまま開発に活かす事は難しいです。開発ではHTMLの5系の記述でマークアップしましょう。
- XMLの説明に第11章〜第13章までのかなり長いページ数を割いていますが、現在ではデータフォーマットに使われるのはJSONが主流なので、このあたりは軽く読んでおけば問題ありません。（逆にJSONについての知識は現在のWeb開発において非常に重要です。）
