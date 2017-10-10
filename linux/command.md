# Linux commandの基礎知識

## はじめに

コマンドは膨大な数がありますので都度使い方を調べればOKです。

※ よく使うコマンドは嫌でも覚えますので間違っても暗記をしないように。

自分用のチートシート（カンペ的なモノ）を作るのも良いでしょう。

代表的なコマンドは以下のサイト等に記載されています。（これでも全部ではない）

https://webkaru.net/linux/command-reference/

## シェルについて

Linuxコマンドを実行する時はシェルというモノを通して行います。

シェルとは UNIX,Linux等のOSで使用されるコマンドラインインターフェースの事です。

シェル（殻）という名前は、カーネル（OS）とアプリケーションの中間に位置し、カーネルを包み込むことに由来しています。

### シェルの種類について

シェルにはたくさんの種類があります。

代表的なモノをいくつか挙げておきます。

#### sh(Bシェル)

sh は、Bourne シェル(ボーンシェル)のことです。

AT&Tのベル研のスティーブン・ボーン(Stephen R. Bourne)によって開発されました。

初期のUNIXの標準シェルでした。

#### csh(Cシェル)

csh は、サン・マイクロシステムズの共同創業者の一人である、ビル・ジョイによって開発されたシェルです。

C言語に似たシェルスクリプトを実行させることが出来ることからCシェルと呼ばれるようになりました。

BSD系のUNIXの標準シェルです。

#### tcsh(TCシェル)

tcsh は、Cシェルの上位互換のあるシェルです。

FreeBSD 4.1以降の標準シェルであり、FreeBSDの流れを組むMacOSでもMac OS X v10.2までは標準のシェルでした。

MacOS でもv10.3以降は、Linuxの標準シェルであるBashがデフォルトのシェルとなりました。

#### ksh(Kシェル)

ksh は、AT&Tのディビット・コーン(David Korn)よって開発された、SVR系のUNIXの標準シェルです。

IBMのUNIXであるAIXのデフォルトのシェルも ksh です。

#### Bash(バッシュ)

Bash は、GNUプロジェクトによるシェルで、Bourne Again の頭文字です。

初期のUNIXのシェルである Bourne Shell の生まれ変わり (born again) にひっかけた名前です。

Bash は1987年、ブライアン・フォックスによって開発されました。

Bashは、Linuxの多くのディストリビューションの標準のシェルであり、Mac OS X v10.3以降の標準シェルです。

ちなみに、Windows 10でもAnniversary Update後、Ubuntu の Bash が使えるようになりました。

#### その他のシェル

高機能なシェルとしては [zsh](https://qiita.com/ryutoyasugi/items/cb895814d4149ca44f12) や [fish](https://intheweb.io/lets-use-fish-shell/) があります。

特にzshは使っている人はそれなりに多いです。

#### 使えるシェルの種類を確認する

以下のコマンドを実行すると現在利用しているシェルの種類を確認出来ます。

```bash
echo $SHELL
```

以下のコマンドで現在利用出来るシェルの種類を確認出来ます。

```bash
cat /etc/shells
```

ちなみに多くのLinuxディストリビューションでは `bash` が標準で使われています。

本カリキュラムでも基本的に `bash` を使っているという前提で進めさせて頂きます。

## 最低限知っておくべきコマンド

最低限これを知らないと学習を進められないコマンドがいくつかあるので記載します。

無論ここに載っている以外にも重要なコマンドはありますが、それは使う都度調べましょう。

ちなみにキーボードの `tab` を押すとコマンドの補完が出来るので少々長いコマンドは補完を使う事をオススメします。（タイプミスが防げる）

### pwd

現在の自分の位置を表示します。

コマンドを実行する前にこれで自分の位置を確認すると事故を防げたりするので、筆者はこれを実行するのが癖になっています。

```bash
pwd
```

↓実行結果の例です。

```text
/home/vagrant
```

### cd

移動する際に利用します。

```bash
cd /usr/local/etc/
```

`/usr/local/etc/` の部分には移動したいディレクトリを入れます。（tabキーで補完出来ます）

絶対パスで指定すると、自分がどのディレクトリにいても必ず同じディレクトリに移動できます。
頭に `/` をつける事がポイントです。

```bash
$ pwd
/home/vagrant
$ cd tmp
$ pwd
/home/vagrant/tmp
```

相対パスで指定すると現在のディレクトリを起点に移動します。

```bash
$ pwd
/home/vagrant/tmp
$ cd ~
$ pwd
/home/vagrant
```

`~` は特殊な文字列で自分のホームに戻ります。

※ ログインユーザーのホームディレクトリである `/home/vagrant` に戻る。

```bash
cd ~
```

### ls

めちゃくちゃ利用しますので使い方は嫌でも覚えると思います。

現在のディレクトリに存在するディレクトリやファイルを表示します。

```bash
ls
```

```text
index.html  sample.txt
```

もう少し詳細に表示させたい場合は以下のようにオプションを追加します。

```bash
ls -la
```

すると実行結果は下記のようになります。

```text
total 36
drwx------ 3 vagrant vagrant 4096 Oct  9 15:45 .
drwxr-xr-x 3 root    root    4096 Apr  5  2017 ..
-rw-r--r-- 1 vagrant vagrant   18 Aug 15  2016 .bash_logout
-rw-r--r-- 1 vagrant vagrant  193 Aug 15  2016 .bash_profile
-rw-r--r-- 1 vagrant vagrant  124 Aug 15  2016 .bashrc
-rw-rw-r-- 1 vagrant vagrant    7 Oct  9 15:43 index.html
-rw-rw-r-- 1 vagrant vagrant    7 Oct  9 15:42 sample.txt
drwx------ 2 vagrant vagrant 4096 Apr  5  2017 .ssh
-rw------- 1 vagrant vagrant  966 Oct  9 15:43 .viminfo
```

これらは同じディレクトリで実行したにも関わらず実行結果が大きく変わっています。

`l` はリスト形式で表示させる為のオプションです。ファイルの作成日時 や [パーミッション](https://eng-entrance.com/linux-permission-basic) 等が表示されています。

`a` は隠しファイル（.から始まるファイルやディレクトリ）も全て表示させるというオプションです。

このようにオプションには様々な種類があります。

都度ネットで調べても良いですが、一番確実なのは `man` コマンドでオプションを見る事です。

例えば `ls` コマンドのオプションは `man ls` で確認出来ます。

↓めちゃくちゃ長いので途中までしか載せてませんがこんな感じでオプションの一覧と説明が記載されます。

```text
LS(1)                                               User Commands                                               LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List  information  about the FILEs (the current directory by default).  Sort entries alphabetically if none of
       -cftuvSUX nor --sort is specified.

       Mandatory arguments to long options are mandatory for short options too.

       -a, --all
              do not ignore entries starting with .

       -A, --almost-all
              do not list implied . and ..

       --author
              with -l, print the author of each file

       -b, --escape
              print C-style escapes for nongraphic characters

       --block-size=SIZE
              scale sizes by SIZE before printing them; e.g., '--block-size=M' prints sizes  in  units  of  1,048,576
              bytes; see SIZE format below

       -B, --ignore-backups
              do not list implied entries ending with ~

       -c     with  -lt:  sort  by,  and show, ctime (time of last modification of file status information); with -l:
              show ctime and sort by name; otherwise: sort by ctime, newest first

       -C     list entries by columns

       --color[=WHEN]
              colorize the output; WHEN can be 'never', 'auto', or 'always' (the default); more info below
```

### vi(vim)

長いので別のファイルにまとめました。

[こちら](https://github.com/keita-nishimoto/web-developer-ojt/blob/master/linux/vim.md) を参照して下さい。

