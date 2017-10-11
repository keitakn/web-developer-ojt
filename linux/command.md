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

都度ネットで調べても良いですが、一番確実なのはコマンドでマニュアルやヘルプを見る事です。

例えば `ls` コマンドのオプションは `man ls` や `ls --help` で確認出来ます。

↓以下は `ls --help` の結果です。長いので途中までしか載せてませんがこんな感じでオプションの一覧と説明が記載されます。

```text
Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      scale sizes by SIZE before printing them; e.g.,
                               '--block-size=M' prints sizes in units of
                               1,048,576 bytes; see SIZE format below
  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                               modification of file status information);
                               with -l: show ctime and sort by name;
                               otherwise: sort by ctime, newest first
  -C                         list entries by columns
      --color[=WHEN]         colorize the output; WHEN can be 'never', 'auto',
                               or 'always' (the default); more info below
  -d, --directory            list directories themselves, not their contents
  -D, --dired                generate output designed for Emacs' dired mode
  -f                         do not sort, enable -aU, disable -ls --color
  -F, --classify             append indicator (one of */=>@|) to entries
      --file-type            likewise, except do not append '*'
      --format=WORD          across -x, commas -m, horizontal -x, long -l,
                               single-column -1, verbose -l, vertical -C
      --full-time            like -l --time-style=full-iso
  -g                         like -l, but do not list owner
      --group-directories-first
                             group directories before files;
                               can be augmented with a --sort option, but any
                               use of --sort=none (-U) disables grouping
  -G, --no-group             in a long listing, don't print group names
  -h, --human-readable       with -l, print sizes in human readable format
```

`man` と `--help` ですが、基本的な使い方を分かっている前提であれば、 `--help` のほうが見やすい事が多いです。

`man` に関しては マニュアルを表示させるコマンドで 下記に詳しい解説があったので載せておきます。

https://eng-entrance.com/linux-command-man

### vi(vim)

長いので別のファイルにまとめました。

[こちら](https://github.com/keita-nishimoto/web-developer-ojt/blob/master/linux/vim.md) を参照して下さい。

### which

コマンドの実態がどこにあるか調べる為のコマンドです。

以下は `ruby` の位置を調べる為の例です。

```bash
which ruby
```

実行結果は下記のようになります。

```text
/usr/bin/ruby
```

地味によく利用します。

### watch

簡易的に定周期で何かのコマンドを実行したい場合に使用すると便利です。

下記の例は `date` コマンドを1秒間隔で実行する例です。

```bash
watch -n 1 date
```

※ 抜ける時は `control + C` を実行して下さい。

### su

ユーザーへの切り替えコマンドです。

`su ユーザー名` と入力しパスワードでの認証が通ればそのユーザーでログイン出来ます。

下記の例はスーパーユーザーであるrootになる為の例です。

```bash
su -
```

`vagrant` でLinuxサーバを作った場合は 初期パスワードが `vagrant` なので入力するとrootに切り替わります。

※ちなみに余談ですが `vagrant` の場合は `sudo su` の入力でパスワード認証なしでrootになれるのでそっちのほうが便利だったりします。

### tail

ファイルの最後を表示してくれます。

```bash
tail ファイル名
```

ログなどをコンソールに出しておきたい場合は

```bash
tail -f ファイル名
```

と打つと、ファイルに書き込まれた情報が表示され続けます。
リアルタイムでログの監視が出来ます。

抜ける時は `control + C` を実行して下さい。

### head

ファイルの上から10行を表示する機能です。 `tail` の逆ですね。

`head student.txt -n 2` のように利用します。

これは上から2行目まで表示させるという意味になります。

### cat

ファイルの内容を表示したり、複数のファイルを連結します。

```bash
# 基本書式
cat [option] filename
```

| オプション | 意味 |
|:----------|----:|
| -n | すべての行に行番号をつける |
| -s | 連続した空行を圧縮する |
| -b | 空行以外に行番号をつける |

例えば、以下のような.txtファイル（sample.txt）があると仮定します。

```text
hello

world
linux
ubuntu
command
```

`cat sample.txt` を実行するとファイルの内容をそのまま表示させます。

行数も出したい場合は `cat -n sample.txt` と実行します。

以下のように出力されます。

```text
     1	hello
     2
     3	world
     4	linux
     5	ubuntu
     6	command
```

空行に行番号をつけたくないならbオプションを付けて `cat -b sample.txt` と実行します。

```text
     1	hello

     2	world
     3	linux
     4	ubuntu
     5	command
```

複数のファイルを連結する場合は以下のように実行します。

```bash
cat filename1 filename2
```

### less

ファイルの内容をコンソールに出力します。

かなり高頻度で利用します。

個人的には `vim` を参照モードで開くよりこっちを使う派です。

`q` で終了します。

地味に便利で例えば [こちらの記事](https://qiita.com/yusabana/items/b5cc2a706be8c031043e) に載っているように リアルタイムでファイルの更新を追えたりします。

`tail -f` と同様の事がより便利に出来る。

### mkdir

ディレクトリを作成します。

`mkdir [ディレクトリ名]` という書式になります。

例えば app というディレクトリを作るには `mkdir app` とします。

appの配下にさらにmodelsというディレクトリを作りたい場合は `mkdir -p app/models` のように -p オプションを付けて実行します。

そうすると下記のようなディレクトリが出来上がります。

```text
app/
  ├ models/
```

### rm

ファイルやディレクトリを削除します。

`rm sample.txt` とすると "sample.txt" が消えます。

ディレクトリを消す場合は `rm -r test/` を実行します。（test/ 配下の物は全て削除されます）

結構危険なコマンドなので `pwd` や `ls` 等で現在の場所と状況を確認してから使ったほうが良いです。

### cp

ファイルやディレクトリのコピーを行います。

基本的な書式は下記の通りです。

```bash
cp -p コピー元ファイル コピー先ファイル名
```

（例1）sample.txtを元にしてsample2.txtを作る

```bash
cp sample.txt sample2.txt
```

（例2）ディレクトリをコピー。

```bash
# ディレクトリとその配下のファイルの属性（所有者や更新時刻）を保ってコピー
cp -rp test/ test2/
```

### mv

ファイルを移動させるコマンドです。

```bash
# 基本書式
mv 移動元ファイル 移動先
```

（例1）sample2.txt を test/ に移動

```bash
mv sample2.txt test/sample2.txt
```

ちなみに移動先の名前を別のファイル名にするとファイル名がリネームされます。

よってリネームコマンドとしての役割も持ちます。

(例3)sample.txt を sample99.txt という名前にリネーム

```bash
mv sample.txt sample99.txt
```

### find

ファイルの検索を行います。

```bash
# 今いるディレクトリの直下から拡張子が .txt のファイルを探す
find . -name *.txt
```

### grep

ログを漁る時に良く使います。

```text
grep 検索文字 ファイル名
```

```bash
# sample99.txt から 'sample' という文字列を抜き出す
grep sample sample99.txt
```

```bash
# test/ ディレクトリ配下から sample という文字列ある箇所を抽出
grep -n -r "sample" test/
```

### top

システム全体のメモリ使用料や負荷状況などが見れます。

Windowsで言うとタスクマネージャーみたいなモノですね。

サーバ監視を行う際に非常に重要なので後で専用のページを作ります。

### ps

プロセスの実行状態を確認します。

サーバ監視を行う際に非常に重要なので後で専用のページを作ります。

ここでは簡単な使い方だけ、例えば下記のように入力すると `sshd` のプロセス一覧が表示されます。

```bash
ps aux | grep sshd
```
`aux` の部分はオプションです。

それぞれ下記のような意味になります。

- a (自分以外のユーザーのプロセスについても表示させる)
- u (デーモンプロセスを表示)
- x (ユーザー名と開始時刻を表示)

`|` という記号と共に `grep` が使われているかと思います。

これはパイプと呼ばれる物で重要なので、後で記載します。

### pgrep

プロセス名からプロセスIDを出力します。

プロセス名が確実に分かっている場合はこちらのほうが楽な場合もあります。

```bash
# sshdのプロセスIDを全て表示
$ pgrep sshd

4529
4531
4619
4621
4741
```

### kill

プロセスを終了させるコマンドです。

`ps` コマンド等で予めプロセスIDを割り出しておく必要があります。

`kill 4741` のように使用します。

これでも止まらない場合は最終手段として `kill -9 4741` のように `-9` を付けると強制終了します。

`kill` はプロセスを終了させるコマンドなのですが `USR1` という特殊なオプションもあります。 `kill -s USR1 4741` のように利用します。

`USR1` は特殊なシグナルで これを受けたプロセスの挙動は、アプリケーションが自由に設定する事ができます。

例えば [Node.js](https://nodejs.org/ja/) では `USR1` を受けるとDebugモードに移行します。

プロセス名が確実に分かっているなら `killall` というコマンドも便利です。

`killall ssh-agent` 等のように使います。これは `ssh-agent` の全プロセスを停止させるという意味になります。

### sort

入力ファイルの並び替えを行います。

これもログ漁りの時に役に立ちます。

| オプション | 意味 |
|:----------|----:|
| -r | 逆順に並べる |
| -n | 数値を昇順にして並べる |
| -f | 大文字小文字区別なく並べる |
| -k | 何列目かを指定する |

以下の内容のテキストファイル（student.txt）があったと仮定します。

```text
80 John
19 Alex
65 Max
```

点数順に降順ソートするなら下記のようにします。

```bash
sort -nr student.txt
```

表示は下記のようになります。

```text
80 John
65 Max
19 Alex
```

2列目の名前順に並べ替える場合は `sort -fk 2 student.txt` のようにします。

以下が出力結果です。

```text
19 Alex
80 John
65 Max
```

### chmod

パーミッションを操作するコマンドです。

これを利用する為には Linuxのパーミッションについて理解しておく必要があります。

パーミッションは [こちらの資料](https://eng-entrance.com/linux-permission-basic) に詳しい解説が載っています。

ざっくりと言うとファイルやディレクトリに付ける権限の事です。

下記のように利用します。

```bash
# 全ての権限を与える。
$ chmod 777 ファイル名/ディレクトリ名
# 全てのユーザーに実行権限だけ与える
$ chmod +x ファイル名/ディレクトリ名
```

数値かアルファベットどちらの記法でも大丈夫ですが、私は数値を使う派です。

さらに詳しい使い方に関しては下記の資料に載っています。

https://eng-entrance.com/linux-command-chmod

### chown

こちらも権限周りのコマンドでファイルやディレクトリの所有者を変更します。

基本的に root で使わないと権限なしで操作できない場合があるので root（または sudo）で使うモノと認識して良いと思います。

```bash
# 基本書式
$ chown 所有者名 ファイル名またはディレクトリ名
```

下記にキレイにまとまった情報があるので記載しておきます。

https://eng-entrance.com/linux-command-chown

### パイプ

記号の `|` の事です。

これを利用する事で前のコマンドの出力結果をパイプの後ろに来るコマンドの入力として利用する事が出来ます。

例えば `ps` コマンドの時に例に出した `ps aux | grep sshd` という記述。

これは `ps aux` コマンドの出力結果（めちゃめちゃ長いので少し省略しています）↓

```text
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root      2275  0.0  0.0  19136   168 ?        Ss   14:54   0:00 /usr/sbin/atd
root      2290  0.0  0.1   4308  1444 tty1     Ss+  14:54   0:00 /sbin/mingetty /dev/tty1
root      2293  0.0  0.1   4308  1400 tty2     Ss+  14:54   0:00 /sbin/mingetty /dev/tty2
root      2297  0.0  0.1   4308  1440 tty3     Ss+  14:54   0:00 /sbin/mingetty /dev/tty3
root      2299  0.0  0.1   4308  1440 tty4     Ss+  14:54   0:00 /sbin/mingetty /dev/tty4
root      2301  0.0  0.1   4308  1496 tty5     Ss+  14:54   0:00 /sbin/mingetty /dev/tty5
root      2303  0.0  0.1   4308  1464 tty6     Ss+  14:54   0:00 /sbin/mingetty /dev/tty6
root      4220  0.0  0.1   9360  1428 ?        Ss   14:54   0:00 /sbin/dhclient -nw -q -lf /var/lib/dhclient/dhclient-et
root      4380  0.0  0.1   9360  1980 ?        Ss   14:54   0:00 /sbin/dhclient -6 -nw -lf /var/lib/dhclient/dhclient6-e
root      4529  0.0  0.6 119904  6648 ?        Ss   14:54   0:00 sshd: vagrant [priv]
vagrant   4531  0.0  0.5 119904  5172 ?        S    14:54   0:00 sshd: vagrant@pts/0
vagrant   4532  0.0  0.3 115352  3352 pts/0    Ss   14:54   0:00 -bash
root      4619  0.0  0.6 119904  6708 ?        Ss   14:55   0:00 sshd: vagrant [priv]
vagrant   4621  0.0  0.5 119904  5308 ?        S    14:55   0:00 sshd: vagrant@pts/1
vagrant   4622  0.0  0.3 115352  3376 pts/1    Ss+  14:55   0:00 -bash
root      4741  0.0  0.2  79952  2616 ?        Ss   14:55   0:00 /usr/sbin/sshd
rpc      15858  0.0  0.2  35324  2040 ?        Ss   14:56   0:00 rpcbind -w
vagrant  25360  0.0  0.2 117220  2576 pts/0    R+   15:46   0:00 ps aux
```

を元にして `grep` で "sshd" という文字列が含まれる箇所だけを抽出しています。

結果は下記のようになります。

```text
root      4529  0.0  0.6 119904  6648 ?        Ss   14:54   0:00 sshd: vagrant [priv]
vagrant   4531  0.0  0.5 119904  5172 ?        S    14:54   0:00 sshd: vagrant@pts/0
root      4619  0.0  0.6 119904  6708 ?        Ss   14:55   0:00 sshd: vagrant [priv]
vagrant   4621  0.0  0.5 119904  5308 ?        S    14:55   0:00 sshd: vagrant@pts/1
root      4741  0.0  0.2  79952  2616 ?        Ss   14:55   0:00 /usr/sbin/sshd
vagrant  25362  0.0  0.2 110472  2156 pts/0    S+   15:48   0:00 grep --color=auto sshd
```

パイプは非常に便利で、元々強力なLinuxコマンドの利便性をさらに高い物にしています。

### 標準入出力のリダイレクション

標準入出力とは以下の事を指します。

- 標準入力(キーボードからの命令)
- 標準出力(画面に表示される結果)

#### `<`

記号の `<` はキーボードからの入力ではなくファイルの内容を利用する時に使います。

以下の内容のテキストファイル（student.txt）があったと仮定します。

```text
80 John
19 Alex
65 Max
```

`grep 80 < student.txt` とすると スコアが80の行を取得出来ます。

以下は出力結果です。

```text
80 John
```

#### `>`

コマンドの結果をテキストファイルに書き出す時に利用します。

結果はファイルに出力されるので画面には表示されません。

`ls -la > ls_result.txt` と実行すると `ls` の結果がファイルに出力されます。

`vi` 等で開くと下記のようなテキストファイルが出来ています。

```text
total 64
drwx------ 5 vagrant vagrant 4096 Oct 11 16:06 .
drwxr-xr-x 3 root    root    4096 Apr  5  2017 ..
-rw------- 1 vagrant vagrant    0 Oct 10 16:09 .bash_history
-rw-r--r-- 1 vagrant vagrant   18 Aug 15  2016 .bash_logout
-rw-r--r-- 1 vagrant vagrant  193 Aug 15  2016 .bash_profile
-rw-r--r-- 1 vagrant vagrant  124 Aug 15  2016 .bashrc
-rw-rw-r-- 1 vagrant vagrant   22 Oct 10 15:36 index.html
-rw------- 1 vagrant vagrant   41 Oct 10 15:28 .lesshst
-rw-rw-r-- 1 vagrant vagrant    0 Oct 11 16:06 ls_result.txt
-rw-rw-r-- 1 vagrant vagrant  349 Oct 10 14:24 sample99.txt
-rw-rw-r-- 1 vagrant vagrant  849 Oct 11 16:05 sample.txt
drwx------ 2 vagrant vagrant 4096 Apr  5  2017 .ssh
-rw-rw-r-- 1 vagrant vagrant   23 Oct 11 15:26 student.txt
drwxrwxr-x 3 vagrant vagrant 4096 Oct 10 15:42 test
drwxrwxr-x 3 vagrant vagrant 4096 Oct 10 15:40 test2
-rw------- 1 vagrant vagrant 9127 Oct 11 16:05 .viminfo
```

ちなみに リダイレクションを両方使い標準入出力を一行で切り替えることも可能です。

下記のような書式になります。

```bash
command < input_datafile > output_file
```
