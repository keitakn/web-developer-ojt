# Macで開発環境を構築する

## この記事の目的

本カリキュラムを進める上ではMacOSを推奨していますが、場合によってはMacを触るのが初めての人もいると思うのでここに最低限開発において用意しておいたほうが良いツール等を記載しておきます。

なおここにある内容は強制ではなく、慣れてきたら自分好みの開発PCにカスタマイズして行くのが良いと思います。

## Command Line Tools

後述のパッケージマネージャー「Homebrew」の動作にも必要なため、必ずインストールします。

ターミナルを開いて以下のコマンドを実行して下さい。

`xcode-select --install`

## Homebrew

Mac用のパッケージマネージャーです。

開発ツールをインストールする時に必須です。

インストール方法は [公式サイト](https://brew.sh/index_ja.html) を見れば簡単に分かるのでここでは割愛させて頂きます。

※ Homebrewに関して詳しい解説が載っている記事がありますのでこちらに記載させて頂きました。

https://qiita.com/omega999/items/6f65217b81ad3fffe7e6

## テキストエディタ

私の個人的なオススメは [Atom](https://atom.io/) です。

github社が開発したエディタで各種Pluginを駆使すれば、最新の言語仕様にも対応しています。

欠点を上げれば少々起動が重い事ですが最近のPCは性能も上がっているのでそれほど気になりません。

[こちらの記事](https://eng-entrance.com/free-texteditor-mac-2) にMacでのオススメエディタが載っていますので参考にするのも良いでしょう。

## Gitクライアント

コマンドでのgit操作に十分に慣れてしまえば不要な気もしますが、初心者のうちはGitクライアントを入れておくと良いでしょう。

私のオススメは [Sourcetree](https://ja.atlassian.com/software/sourcetree) です。

Atlassianという会社が作成していて同社のbitbucketやgithubとの連携も出来るので使いやすいです。

ただ [Sourcetree](https://ja.atlassian.com/software/sourcetree) のようなツールを使う場合でも裏でどのようなGitコマンドが使われているかは理解するようにしておいたほうが良いです。

gitは現在の開発においては必須なので、少しづつで良いのでgitその物を理解するように知識を蓄積していく事を強く推奨します。

初心者が学習する為に役に立ちそうなURLをいくつか載せておきます。

- https://www.backlog.jp/git-guide/
- https://qiita.com/amymd/items/056864438b839855b6d7

例によってIT技術というのは手を動かす事が一番重要だったりするので最初から書籍でがっつり学習するというよりは、使いながら少しづつ覚えていく事をオススメします。

## ターミナルソフト

Macに標準搭載されているターミナルでも十分ですがより高機能な [iTerm2](https://www.iterm2.com/) を推奨します。

参考になりそうな記事をいくつか載せさせて頂きます。

- [MacのターミナルアプリにはiTermがオススメ](https://qiita.com/k_saito/items/ab73032a82632af3cd3c)
- [Macのターミナルよりおすすめ！分割と移動がイケてるiTerms2が素敵！](https://laugh-raku.com/archives/3127)

## IDE

統合開発環境と呼ばれる物で、有名どころだと [Eclipse](https://ja.wikipedia.org/wiki/Eclipse_(%E7%B5%B1%E5%90%88%E9%96%8B%E7%99%BA%E7%92%B0%E5%A2%83)) があります。

しかし私のオススメは、[IntelliJ IDEA](https://www.jetbrains.com/idea/) です。

様々な言語での開発をサポートしており、これ1つあれば大抵の状況に対応が可能です。

私は今まで様々なIDEやエディタを試しましたがこれに勝る物はなかったです。

唯一の欠点を言えば有料だと言う事です。（IntelliJ IDEA Ultimateを購入した場合、初年度は$149 , 2年目は以降は更新量が安くなっていきます）
詳しくは公式サイトをご覧下さい。

https://www.jetbrains.com/idea/buy/#edition=personal

エディタで十分とか（[達人プログラマー](https://www.amazon.co.jp/dp/427421933X) というめちゃくちゃ有名な書籍がありますがこの中ではエディタ推してます）有料ソフト等買いたくないとか、様々な意見を言う人がいます。

エディタを十分に使える事は確かに良いプログラマの条件の1つだと言う事は分かります、しかし初心者のうちはなるべく早いうちに全体像を把握して開発全体の流れを把握したほうが良いため、 エディタを極めるとかは後でやれば良いと思います。（IDEは高機能なのでプログラミング自体の負荷はかなり下がります）

有料である点も [IntelliJ IDEA](https://www.jetbrains.com/idea/) は費用に見合う価値は十分にあると思いますので、購入をオススメします。

参考までに [IntelliJ IDEA](https://www.jetbrains.com/idea/) の使い方を書いた記事をいくつか紹介させて頂きます。（中には私が書いた物もあります）

- [IntelliJ IDEAのインストールと日本語化](https://qiita.com/keitakn/items/31e8af9ccb4b3bdd74d0)
- [IntelliJ IDEA初期設定（主にエディタ）](https://qiita.com/keitakn/items/5968b9eee4177c302481)
- [IntelliJ IDEA ショートカットリンク集（随時更新）](https://qiita.com/keitakn/items/b9a77efbcc17f8844081)
- [IntelliJ IDEA Rubyの開発環境を作成する](https://qiita.com/keitakn/items/76d6707db7d23fe4ca85)
- [IntelliJ IDEA PHP7の開発環境を作成する](https://qiita.com/keitakn/items/638b080a1420b401c315)

※ 一番最初に日本語化の方法が書いてありますが日本語化は不具合が発生する可能性があるのでオススメしません。
プログラマにとって英語はかなり重要なので英語に慣れる意味でも英語のまま利用する事を推奨します。

## dnsmasq

自身のPC上に構築した環境に接続する際にIPアドレスではなく利用予定のドメイン名を用いて接続したいケースが良くあります。

DNS登録されていないドメインに接続する為には `/etc/hosts` にIPアドレスとドメイン名を記載して名前解決出来るようにする必要があります。

その際にこのdnsmasqがあると便利なのでインストール ＆ 初期設定しておきましょう。

※ DNSが何なのかはネットワークの基礎のことろで説明します。

インストール方法ですが [こちらの記事](https://laccie.blogspot.jp/2017/04/mac-osxdnsmasq.html) が分かりやすかったので記載させて頂きます。

## Alfred（ランチャーアプリ）

Macはなるべくキーボードで操作出来るようになったほうが効率が良いので、ランチャーアプリを使う事を推奨します。

Alfredは無料で使える高機能なランチャーアプリです。

参考になりそうな記事を載せておきますので参考にインストールしておく事をオススメします。

- [Alfredの 設定 & Tips メモ](https://qiita.com/pawjun/items/f45f8980ac9941c893c5)

## GoogleChrome（Webブラウザ）

[Chrome DevTools](https://saruwakakun.com/html-css/basic/chrome-dev-tool) の使い勝手やPluginの豊富さ、情報量等あらゆる面で他のブラウザを上回っているのでWeb開発ではGoogleChromeを使う事を推奨します。
