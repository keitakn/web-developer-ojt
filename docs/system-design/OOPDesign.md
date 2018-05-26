# オブジェクト指向入門

この資料ではオブジェクト指向で設計する為の技術を学びます。

内容に関してはQiitaで話題になっていた [オブジェクト指向と10年戦ってわかったこと](https://qiita.com/tutinoco/items/6952b01e5fc38914ec4e) という記事をベースに記載しています。（非常に分かりやすい記事です）

# オブジェクト指向とは

まずはオブジェクト指向をざっくり理解する事から始めましょう。

[初心者向けに徹底解説！オブジェクト指向とは？](https://eng-entrance.com/what-oop) という記事をご覧下さい。

簡単に言うと、システムに登場する人やモノをオブジェクトとして扱う設計手法です。

私も昔独学で学習していた時、最初はさっぱり理解出来ませんでした。

人によっても解釈が異なるし、ネット上の記事を見ても様々な意見があるので初心者にとってはより混乱を加速させる原因になっていると考えます。

よって最初はざっくりとした理解で大丈夫です。

この資料ではサンプルコードを交えてなるべく分かりやすく説明させて頂きます。

# なぜオブジェクト指向で書くのか

[オブジェクト指向と10年戦ってわかったこと](https://qiita.com/tutinoco/items/6952b01e5fc38914ec4e) にもありますが、一言で言うと「変更に強い構造」を作る為です。

ちなみにこれはオブジェクト指向に限らず全ての設計手法に言える事です。

対象的な設計手法に関数型プログラミングがあります。

方法論は違いますが、目指しているのはオブジェクト指向と同じく「変更に強い構造」を作る事です。

※関数型プログラミングに関してはこれとは別に資料を作成します。

# サンプルプログラムの仕様説明

[design-pattern-tips](https://github.com/keitakn/design-pattern-tips/tree/master/src/oop/basic) というサンプルプロジェクトを用意しました。

ここでは書籍の管理をする為のプログラムを例にオブジェクト指向の設計手法を見ていきましょう。

サンプルプログラムの要件は下記の通りです。

- 書籍にはISBNが設定されている
- ISBNから言語コード（英語、日本語等）を求める事が出来る
- 書籍にはタイトルが設定されている
- タイトルはメインタイトルとサブタイトルに分かれている
- 書籍には価格が設定されている
- 価格は税込み価格と税抜き価格の両方を表示する事が出来る

ISBNとは書籍に必ず設定されている識別子のようなモノです。

厳密にはISBNから出版社の情報や書名記号等が分かるようになっているのですが、サンプルコードを単純化する為、そのあたりの仕様は考慮に入れない事とします。

クラス図で表すと次のようになります。

![web-developer-ojt-oop](https://user-images.githubusercontent.com/11032365/40579336-34cca5ee-6160-11e8-806d-674989893140.jpg)

クラス名が日本語で書いてありますが実際の実装は [こちら](https://github.com/keitakn/design-pattern-tips/tree/master/src/oop/basic) を参照して下さい。

クラス図の見方ですが厳密に覚える必要はありません。

[こちらのサイト](http://www.itsenka.com/contents/development/uml/class.html) に載っている事をざっくりと理解しているレベルで十分です。

# オブジェクト指向の原則

オブジェクト指向で設計出来ているかを判断する為には自分が書いたコードを見て下記の原則に当てはまっているかをチェックすると良いでしょう。

[design-pattern-tips](https://github.com/keitakn/design-pattern-tips/tree/master/src/oop/basic) を例に説明します。

## 継承

プログラミング言語には継承の機能があります。

`extends` という構文を利用する事で継承元の親クラスの機能を引き継ぐ事が出来ます。

この機能を引き継ぐという部分に着目しがちですが、継承の本質はインターフェースになります。

書籍（Book）の定義は下記のようになっています。

```typescript
import Isbn from "./Isbn";
import Price from "./Price";
import Title from "./Title";

export default interface Book {
  isbn: Isbn;
  title: Title;
  price: Price;
}
```

このインターフェースを定義する事で「サンプルプログラムの仕様説明」にあった「書籍はISBN、タイトル、価格を持つ」という部分を表しています。

例として雑誌を表すクラスの実装例を見てみましょう。

```typescript
import Book from "./Book";
import Isbn from "./Isbn";
import Title from "./Title";
import Price from "./Price";

export default class Magazine implements Book {
  private readonly _isbn: Isbn;
  private readonly _title: Title;
  private readonly _price: Price;

  constructor(isbn: Isbn, title: Title, price: Price) {
    this._isbn = isbn;
    this._title = title;
    this._price = price;
  }

  get isbn(): Isbn {
    return this._isbn;
  }

  get title(): Title {
    return this._title;
  }

  get price(): Price {
    return this._price;
  }
}
```

`implements Book` という部分が `Book` インターフェースを利用しますよという意味です。

もしも `Book` がインターフェースではなくクラスであった場合は `extends` を使う事になります。

どちらの場合も本質的には書籍（book）という抽象的なインターフェースを用意して漫画とか雑誌等のより具体的なオブジェクトを生成出来るようにしています。

継承を理解する上で大切な概念があります。

[is-a](https://ja.wikipedia.org/wiki/Is-a) という概念です。

この場合だと以下の2つが成り立っています。

- 書籍は漫画である
- 漫画は書籍である

こうして言葉にしてみると当たり前の事を言っているように聞こえます。

しかし私はこの基本原則が守られていない現場を多く見た事があります。

そのような現場では書籍クラスを継承して「ファイナルファンタジー」というゲームソフトクラスを作る等の [is-a](https://ja.wikipedia.org/wiki/Is-a) 原則を完全に無視した実装が平気で行われています。

このような実装を行ってしまうと後で見た人（将来の自分も含む）が理解しにくいコードになってしまうので避けるようにしましょう。

## ポリモーフィズム

継承の本質はインターフェイスという事を先程説明しました。

ポリモーフィズムとはインターフェースに依存するようにプログラムを書きましょう。という事です。

サンプルプログラムで説明します。

先程はBookインターフェースを具象化した「雑誌クラス」を例に出しましたが今度は同じように作られている「漫画クラス」を例に説明します。

漫画クラスの実装は下記になります。

```typescript
import Book from "./Book";
import Isbn from "./Isbn";
import Price from "./Price";
import Title from "./Title";
import Author from "./Author";

/**
 * 漫画クラス
 */
export default class Comic implements Book {
  private readonly _isbn: Isbn;
  private readonly _title: Title;
  private readonly _price: Price;
  private readonly _author: Author;

  constructor(isbn: Isbn, title: Title, price: Price, author: Author) {
    this._isbn = isbn;
    this._title = title;
    this._price = price;
    this._author = author;
  }

  get isbn(): Isbn {
    return this._isbn;
  }

  get title(): Title {
    return this._title;
  }

  get price(): Price {
    return this._price;
  }

  get author(): Author {
    return this._author;
  }

  /**
   * 作者のフルネームを取得する
   *
   * @returns {string}
   */
  extractAuthorFullName(): string {
    return this.author.fullName(this.isbn);
  }
}
```

雑誌クラスとは違い「Author」という作者を表すオブジェクトを保持しています。

作者クラスの実装は下記のようになります。

```typescript
import Isbn from "./Isbn";

export default class Author {
  private readonly _givenName: string;
  private readonly _familyName: string;

  constructor(givenName: string, familyName: string) {
    this._givenName = givenName;
    this._familyName = familyName;
  }

  get givenName(): string {
    return this._givenName;
  }

  get familyName(): string {
    return this._familyName;
  }

  /**
   * 作者のフルネームを取得する
   *
   * @param {Isbn} isbn
   * @returns {string}
   */
  fullName(isbn?: Isbn): string {
    // 書籍の言語が日本語の場合だけ姓、名の順番で返す
    if (isbn != null && isbn.extractLanguage() === "ja") {
      return `${this.familyName} ${this.givenName}`;
    }

    return `${this.givenName} ${this.familyName}`;
  }
}
```

雑誌は特定の作者がいないので作者オブジェクトを保持していません。

しかし漫画は通常、作者がいるので漫画クラスだけが作者オブジェクトを保持しているような実装になっています。

前置きが長くなりましたがここからが本題です。

例えば書籍を購入する処理を作るとします。

その際、Bookインターフェースに定義されている情報のみに依存して作るのがポリモーフィズムです。

「書籍を購入する」行為は「雑誌」と「漫画」両方で同じハズです。

よって雑誌オブジェクト、漫画オブジェクトどちらでも同じように購入出来るのがベストです。

もっと言うなら将来「参考書オブジェクト」が出てきても購入処理を修正しなくても良いのがベストです。

「雑誌」や「漫画」等の具象化したモノではなく「書籍」という抽象（インターフェース）に依存するのが、ポリモーフィズムです。

## カプセル化

[オブジェクト指向と10年戦ってわかったこと](https://qiita.com/tutinoco/items/6952b01e5fc38914ec4e) の説明ではカプセル化は最も難しく最も理解が難しいとされています。

カプセル化とは複雑なロジックを閉じ込めて、分かりやすいインターフェースだけを公開するという事です。

プログラミング言語だけで説明しようと難しいのですが、現実世界を見渡すとカプセル化で溢れている事に気が付きます。

例えば洗濯機を例に説明します。

洗濯機を使う人間にとっては選択物を中に入れてボタンを押すだけで洗濯が完了します。

洗濯機は中で色々複雑な動作を行っていますが、それを利用者である人間が知っている必要はありません。

人間が意識しなければいけないのは以下の3点くらいです。

- 洗濯物を入れる
- 洗剤や柔軟剤を入れる
- ボタンを押す

人間は洗濯機の使い方だけ知っていれば良くて洗濯の方法は知りません。

洗濯の方法は洗濯機だけが知っています。

これがカプセル化です。

サンプルコードを例に説明します。

書籍オブジェクトが保持しているオブジェクトの1つに価格オブジェクトがあります。

以下のような実装になっています。

```typescript
export default class Price {
  private readonly _excludingTaxPrice: number;
  private readonly _taxRate: number;

  constructor(excludingTaxPrice: number, taxRate: number) {
    this._excludingTaxPrice = excludingTaxPrice;
    this._taxRate = taxRate;
  }

  get excludingTaxPrice(): number {
    return this._excludingTaxPrice;
  }

  get taxRate(): number {
    return this._taxRate;
  }

  /**
   * 税込み価格を計算する
   *
   * @returns {number}
   */
  calculateIncludingTaxPrice(): number {
    // taxRateを1.08のように設定すると誤差が出るので108のように渡す事
    // （参考）https://qiita.com/jkr_2255/items/0ca7bc536d930f83a901
    return Math.round(this.excludingTaxPrice * this.taxRate / 100);
  }
}
```

税込み価格を計算するには下記のようにします。

```typescript
const isbn = new Isbn("978-4-06-319239-1");
const title = new Title("ちはやふる", "第1巻");
const price = new Price(463, 108);
const author = new Author("由紀", "末次");

const comic = new Comic(isbn, title, price, author);

// 税込み価格を表示する
console.log(comic.price.calculateIncludingTaxPrice());
```

`Comic` クラスは税込み価格の計算方法を知りません。

ただ、価格オブジェクトの `calculateIncludingTaxPrice` を呼び出したら税込み価格が計算されるという事は分かります。

これが分かるのは `calculateIncludingTaxPrice` が公開（public）メソッドになっているからです。

一方、価格クラスは税込み価格の計算方法を知っています。

他のオブジェクトにその方法を教えるために `calculateIncludingTaxPrice` をpublicメソッドにしています。

こうする事で他のオブジェクトは消費税の計算方法を知らなくても税込み価格が分かるようになります。

カプセル化を意識する為には以下の2点を意識すると良いです。

- クラスの役割は1つにする事（[単一責任の原則](https://qiita.com/gomi_ningen/items/02c42e2487d035f9c3c8)）
- [良い名前をつける](https://qiita.com/tutinoco/items/85641c0819d813186f9d)

クラスの役割が2つ以上あると他オブジェクトに公開するpublicメソッドが複雑になりがちです。

操作が簡単で分かりやすいのは重要です。

洗濯機の操作が複雑で熟練者しか扱えない物だとしたら誰も洗濯機を買わないでしょう。

[オブジェクト指向と10年戦ってわかったこと](https://qiita.com/tutinoco/items/6952b01e5fc38914ec4e) に良い例えがあったので抜粋します。

>十得ナイフは便利ですが、コンピュータの世界において十得ナイフは必要ありません。もし現実世界においても四次元ポケットが存在したら十得ナイフはいらなくなるでしょう。栓抜きを必要とした時、四次元ポケットから何を取り出すでしょうか？わざわざ十得ナイフを取り出して十得ナイフの栓抜きを使うようなことをするでしょうか？答えはNOです。四次元ポケットからは栓抜きを取り出して使います。

>このようにコンピュータの世界は四次元ポケットが存在する世界なので「なんでもできる便利な道具」より「何ができるか明確な道具」の方が利便性が高まることになります。プログラミングの世界では欲しい時に欲しいものを手に入れることができるため、十得ナイフのような複数の異なる役割を持ったクラスやモジュールは必要ないのです。

「良い名前をつける」事は使いやすいオブジェクトを作る上で重要です。

`Price` という名前を見れば、価格に関する情報や振る舞いがある事を推測出来ます。

もしも `Price` というクラスに作者の情報が入っていたら意味不明です。

# オブジェクト指向で使えるデザインパターン

# 参考資料
- [オブジェクト指向と10年戦ってわかったこと](https://qiita.com/tutinoco/items/6952b01e5fc38914ec4e)
- [ISBNとは？ 「978」から始まるISBNコードの意味](http://pro.bookoffonline.co.jp/book-enjoy/books-trivia/20160408-isbn-mean.html)
- [なぜオブジェクト指向は難しいのか](https://qiita.com/tutinoco/items/7f7568cc7dbf7e2276c8)
