# HTTP

HTTPとは Hyper Text Transfer Protocol の略でブラウザ等のクライアントとWebサーバとの通信を行う為のプロトコル（通信手段です。）

この資料ではWebエンジニアには必須であるHTTPの基本的な事を学びます。

# HTTP通信を実際に行ってみる

まずはHTTPサーバとコマンドで通信してみましょう。

ターミナルを開いて以下のコマンドを実行して下さい。

`curl -v https://www.yahoo.co.jp`

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

※ ブラウザのほうはHTMLを解釈しレンダリングする能力があるので、人間が見て分かりやすいグラフィカルな表現が出来ているだけです。

これがHTTPによる通信になります。

## HTTPの基本構成

先程の `curl` コマンドを元に解説していきます。
