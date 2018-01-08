# JavaScript

JavaScriptの基礎を学びます。

最初はこのJavaScriptを使って、基礎的な構文やアルゴリズムのトレーニングをして行きましょう。

※ アルゴリズムというのは問題解決の手順の事でアルゴリズムの学習の際に詳しく解説します。

## JavaScriptの特徴

プロトタイプベースのオブジェクト指向言語で主にWebブラウザで動作します。

昔は癖があり使いくいという印象を持たれる事も多かった言語ですが、最近はモダンな機能等も取り入れられているので、そのような印象は大分薄くなったのではないでしょうか。

詳しくは [wikipedia](https://ja.wikipedia.org/wiki/JavaScript) を参照してみて下さい。

## なぜJavaScriptを最初に学習するのか？

私が最初に学ぶべき言語としてJavaScriptをオススメする理由は下記の通りです。

### 理由1フロントエンド（ブラウザ）でもサーバでも利用出来るから

JavaScriptはブラウザでしか動かないというイメージは過去の物です。

[Node.js](https://nodejs.org/ja/) の登場でサーバでも実行させる事が可能だという点が選定理由の1つ目です。

### 理由2多くのプロジェクトで使われているので学習した内容がながく使える可能性が高い

[Node.js](https://nodejs.org/ja/) の恩恵はサーバ側でJavaScriptが実行出来るようになった事だけではありません。

[npm](https://www.npmjs.com/) という強力なパッケージ管理システムの存在により、ブラウザでもサーバでも実行出来る様々なパッケージが利用出来ます。

これらを利用する事で自分で1からプログラムを作るよりも開発速度を大幅に短縮出来ます。

※ 他の言語にもパッケージ管理システムはあります。Rubyの[RubyGems](https://rubygems.org/) やPHPの[composer](https://getcomposer.org/) 等ですね。

このパッケージ管理システムとブラウザ、サーバ両方で動くという性質上、ほとんどのプロジェクトで [Node.js](https://nodejs.org/ja/) が利用されています。

サーバサイドの言語に関してはNode.jsを使わずにRubyやPHPを利用しているケースも多いですが、Web開発だとフロントエンド（ブラウザ）では、事実上JavaScriptの一択です。

参加するプロジェクトによってサーバサイドの言語は様々な物（PHP, Ruby, Java等）がありましたが、JavaScriptが全く登場しないプロジェクトは1つもありませんでした。

よって習得した事を活かせる可能性が高いという点がメリットになります。

### 理由3ブラウザで動かす事だけを考えるなら開発環境の用意が非常に簡単

[Node.js](https://nodejs.org/ja/) を使わなくてもシンプルに動かすだけならWebブラウザとテキストエディタだけあれば開発を進められます。

Webエンジニアを目指すのであれば開発環境構築のスキルは必須ではあるのですが、初心者はここで挫折してしまう可能性が結構高いので、この点は結構なメリットだったりします。

### 理由4言語仕様がモダン（現代的）になってきている

JavaScriptは [ECMAScript](https://ja.wikipedia.org/wiki/ECMAScript) という標準仕様によって仕様が策定されています。

このバージョン5.1までは、正直なところを言うと、他の言語と比較して独自仕様や罠が多く、考慮しなければいけない点が多く存在していました。

しかし2015年に最新の言語仕様である、 ECMAScript 6(2015) が登場してから状況が大きく変わりました。

オーソドックスなオブジェクト指向プログラミングと、関数型言語のエッセンスなどを備えたモダンな言語に生まれ変わりました。

もしこの仕様がリリースされていなかったら私は最初に学ぶべき言語にJavaScriptをオススメしなかったと思います。

※ 補足ですが、ECMAScript 6以降はECMAScript 2015のような呼び方で呼ぶのが正しいそうです。

ネット上ではECMAScript 6とかES7（2017の事）みたいな呼び方をしている場合もありますがECMAScript 20◯◯ が正しい呼び方です。

## 学習の進め方

[ojt-node](https://github.com/keita-nishimoto/ojt-node) というリポジトリを作りました。

こちらを利用して学習を進めて行きましょう。

まずはREADMEの手順の通りに動かしてみて下さい。

※ 動作させるには [こちらの課題](https://github.com/keita-nishimoto/web-developer-ojt/blob/master/docs/linux/task.md) が終わっている必要があります。

## 学習の際に参考にするリンク

学習の際に参考になるリンクをいくつか紹介させて頂きます。

### [MDN JavaScript リファレンス](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference)

mozillaの公式リファレンスです。

ネット上に公開されている記事等はあくまでも2次情報なので、ここで正しい仕様等を確認しましょう。

### [JavaScript入門](https://dotinstall.com/lessons/basic_javascript_v2)

動画形式で基礎的な構文がある程度身につきます。

本格的な学習に入るまでに一度やっておくといいでしょう。

## JavaScriptの学習の注意点

ネット上の記事は古い可能性があります。

特にJavaScriptはECMAScript 2015以降とそれ以前のバージョンで全く書き方が異なる部分があるので注意しましょう。

※ 今から習得するのであればECMAScript 2015以降の構文を覚えるのが良いでしょう。

見分け方の1つとして下記のように `var` を使って変数宣言を行っているケースは古い情報の可能性があります。

```javascript 1.8
var myName = 'K';
```

ECMAScript 2015以降では下記のように `const` を使っての変数宣言が一般的です。

```javascript 1.8
const myName = 'K';
```

このあたりは下記の記事に詳しく記載されていたので、こちらに載せさせて頂きます。

- [旧石器時代のJavaScriptを書いてる各位に告ぐ、現代的なJavaScript超入門 Section1 ～すぐにでも現代っぽく出来るワンポイントまとめ～](https://qiita.com/gaogao_9/items/ec2b867d6941173fd0b1)
- [旧石器時代のJavaScriptを書いてる各位に告ぐ、現代的なJavaScript超入門 Section5 ～ES2015文法を覚えよう](https://qiita.com/gaogao_9/items/18b20ad9b76c9c81b5fa)

この旧石器時代のJavaScriptシリーズは今までのJavaScriptの歴史からどのような背景で進化していったのかの仮定が書かれており、読み物としても非常に面白い内容になっています。

興味がある方は読んでみて下さい。（今の時点で意味が全て分からなくても大丈夫です。後のlessonで理解出来ます）

## コーディング規約について

様々なコーディング規約がありますが、私のオススメは [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript) です。

オープンな議論を通じてスタイルが決定していること、スタイルについてパフォーマンスに関するエビデンスがあることがオススメの理由です。

github上でも多くのスターを獲得しています。

本カリキュラムではこのAirbnbのスタイルで学習を進めていきます。

[日本語版](http://mitsuruog.github.io/javascript-style-guide/README.md) もあります。
