---
title: "困ったらとりあえず再起動"
date: 2022-03-22
categories: ["雑多", "Raspberry Pi"]
tag: ["雑多"]
lead: "「何もしてないのにおかしくなった？じゃあ再起動しろ！なんとかなる！」"
description: "割と珍しく何もしてないのに list ファイルが壊れて apt install も upgrade も完了できなくなっちゃったので再起動したら直った話"
---

# TL: DR;

困ったら再起動すればなんとかなるっぽい？  
書いてて気持ち悪くなったクソ文章なのでそんな長ったらしいゴミ見たくない方は上の 1 行だけ見て Twitter でも見てください。

## 経緯

事の発端は先日 3 月 21 日、[友人](https://twitter.com/kazukazu1231234)[ 2 人](https://twitter.com/hiraginoyuki)とグループ通話しながら各々が作業していた際、ふと「Raspberry Pi のアプデしてみるか」と思って SSH 接続し、`sudo apt update && apt upgrade`を実行した所

```
dpkg: unrecoverable fatal error, aborting:
E: Sub-process /usr/bin/dpkg returned an error code (2)
```

を吐いて止まっていました。

{{<figure src="./image/1.png" alt="apt update && upgrade エラー" width="100%">}}

これを見た時、「**あ～なんか死んでんな～まぁほっときゃ直るだろ**」とかいうクソにクソを練り混ぜたかのような考えをして SSH 接続を切り、その後[テトリスをしたり](https://twitter.com/SzlyNe_/status/1505840803314552832)していました。

### インストール出来んが？

んでもって今日(3 月 22 日)の朝、あまりの寒さに勝手に目が覚め、**Raspberry Pi に負荷を掛けてホッカイロにしよう**とイカれた事を考え stress パッケージをインストールしようとしました。  
普通ならなんかいっぱい色々出てきてインストール終わるんですが、そんな簡単に行くはずもなく

```
dpkg: unrecoverable fatal error, aborting:
 パッケージ 'mariadb-common' のファイル一覧ファイルに最後の改行がありません
E: Sub-process /usr/bin/dpkg returned an error code (2)
```

とかいうエラーを吐いてインストールが止まってしまいました。

{{<figure src="./image/3.png" alt="apt install エラー" width="100%">}}

流石に何のパッケージも入れる事が出来ないのは色々厳しいので対処法をググってみた所、[こんな記事](https://totech.hateblo.jp/entry/2016/08/05/112814)を見つけました。  
記事内に従って `file /var/lib/dpkg/info/mariadb-common.list` を実行してみた所、通常 `/var/lib/dpkg/info/acl.list: ASCII text` みたいになってる所、 `GNU message catalog` となっており、どう見ても通常ではないファイルになっていました。

{{<figure src="./image/2.png" alt="file 破損ファイル" width="100%">}}

どうしようかなぁと思いつつ記事を読み進め、記事内と同じ手段で list ファイルを復元出来ないかと思って[張られていたリポジトリ](https://github.com/volumio/RootFS-RaspberryPI/tree/master/var/lib/dpkg/info)を見てみましたが、 `mariadb-common.list` が見つからなかった為この方法は断念。  
WSL に Debian 環境を建て mariaDB を入れるのもアリだと思いましたが、そこそこ時間がなかった為これも断念。

### おぼろげながら浮かんできたんです、「再起動」という単語が

どうしようかなーと思いつつ Twitter 見たりしていた所、ふと「困ったら再起動」というワードが脳に浮かんできた為、 `sudo reboot now` で急いで再起動しました。  
とりあえず再起動して再度 SSH 接続してもう一回 `sudo apt update && apt upgrade` を実行してみた所、先程までは `パッケージを事前設定しています` の後に `dpkg: unrecoverable fatal error, aborting` が返ってきて止まっていたのが進み、パッケージアップデート完了。

{{<figure src="./image/4.png" alt="apt upgrade 進行" width="100%">}}

その後一応 `file /var/lib/dpkg/info/mariadb-common.list` を実行してみた所、他のファイル一覧ファイルと同様 `ASCII text` になっており、解決したっぽいです。

{{<figure src="./image/5.png" alt="file ファイル修正？" width="75%">}}

## 原因

割とマジで不明。Uptime が 30days 程になっていたのは確認していたんですが...そんな 1 ヶ月ほど連続起動してるだけでファイル読み込みおかしくなる OS なんか...？  
OS を入れているストレージが Raspberry Pi を購入した時に付いてきた microSD なのでまぁもしかしたら壊れてるのかも...？それにしては再起動でまた正常に読み出せたのが不明。

何も分からん...。
