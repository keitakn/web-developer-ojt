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

ただし、この動画は早く作成する事だけを考えて作られているので、コードの書き方が古かったりします。

ES2015 以降の新しい書き方で書くように頑張ってみましょう。

それから、変数名や関数名を付ける時は分かりやすい名前を心がけましょう。

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
