---
title: "KDEとi3wmを共存させる"
date: 2022-03-21
categories: ["雑多"]
tag: ["雑多"]
lead: "Zennに投げるつもりだったボツ記事 | Author: Alliana"
description: "Zennに投げるつもりだったボツ記事です。KDEのウィンドウマネージャをi3wmに置き換えるついでにある程度設定をします。"
---

**友人のブログにボツ記事を投げる男，~~スパイダーマ！~~**

ということであさるとの承諾を得て Zenn に投げるつもりだった記事を投げます。
初めまして，[ありあな](https://twitter.com/Alliana_ab2m)です。

突然ですがタイル型ウィンドウマネージャってかっこいいですよね。しかしそのシンプルさ故に初期設定が結構ハードです。
最初は壁紙はおろか，バッテリーの状況もロクに教えてくれません。

そこでデスクトップ環境である KDE を先に導入し，ウィンドウマネージャだけを i3wm に置き換えることで KDE と i3wm を共存させます。

メリットは言わずもがな，KDE のリッチな GUI ユーティリティ群を使用しながらも i3wm を使用できることや，通知などのインターフェースが粗方組まれている，そして何よりキーボードで全てが操作できるという点です。
デメリットは特に思いつきませんが，強いて言うなら依存パッケージが増えてシステムのシンプルさが損なわれることでしょうか。3 日に 1 回ぐらい`yay -Syyu`したときに鬼のようにアプデがやって来ることがあります。それなりに恐ろしいです。
とはいえ，私はなんだかんだ KDE もそれなりの頻度で使うので，そのままにしています。[^1]
２つもデスクトップ環境を抱えるのが嫌な人は，i3wm 環境が煮詰まってから KDE を削除しても良いと思います。

[^1]: というより，Manjaro の i3 エディションは私の環境ではなぜか不安定だったので，公式フレーバーの KDE を選ぶ他無かった，という結果でもあります。

## 参考にした記事

というかここに書いてある通りに進めればできます。
[heckelson/i3-and-kde-plasma](https://github.com/heckelson/i3-and-kde-plasma)

## 環境

Manjaro KDE Edition を使用します。Debian 系であれば，適宜 pacman を apt に置き換えるなどしてください。
他のディストリビューションでも KDE であればおそらく問題ありません。

## 1. i3wm 環境に必要なパッケージをインストールする

```
$ sudo pacman -Syyu
$ sudo pacman -S i3-gaps feh i3status
```

`i3-gaps`: i3wm 本体です。ウィンドウ間に隙間を生ませるために i3 からフォークされたものです。それ以外の機能は i3 と全く変わりません。
`feh`: 壁紙を設定するためのパッケージ
`i3status`: ステータスバーを表示するためのパッケージ

## 2. 初期設定をする

まずは XSession ファイルを作ります。su 環境で`/usr/share/xsessions`配下に`plasma-i3.desktop`ファイルを作ってください。
以下のように記述します。

```txt:plasma-i3.desktop
[Desktop Entry]
Type=XSession
Exec=env KDEWM=/usr/bin/i3 /usr/bin/startplasma-x11
DesktopNames=KDE
Name=i3Plasma
Comment=Plasma with i3
```

次に i3wm 環境の設定を行います。`i3-config-wizard`を実行してください。
ちなみに実行するターミナルは KDE 環境でも i3 環境でも問題ありません。
ウィザードに沿って設定を行い，`~/.config/i3/config`が作成されれば OK です。

最後に i3wm 上での Plasma アプリ群の利便性を高めるため，i3wm の設定をします。
`~/.config/i3/config`を開いて以下の内容をコピペしてください。

```
# Plasmaアプリ群の利便性向上
for_window [window_role="pop-up"] floating enable
for_window [window_role="task_dialog"] floating enable

for_window [class="yakuake"] floating enable
for_window [class="systemsettings"] floating enable
for_window [class="plasmashell"] floating enable;
for_window [class="Plasma"] floating enable; border none
for_window [title="plasma-desktop"] floating enable; border none
for_window [title="win7"] floating enable; border none
for_window [class="krunner"] floating enable; border none
for_window [class="Kmix"] floating enable; border none
for_window [class="Klipper"] floating enable; border none
for_window [class="Plasmoidviewer"] floating enable; border none
for_window [class="(?i)*nextcloud*"] floating disable
for_window [class="plasmashell" window_type="notification"] border none, move right 700px, move down 450px
no_focus [class="plasmashell" window_type="notification"]
```

ここまで終わったら，一度ログアウトしてログイン画面(SDDM)に戻り，左下の Plasma(X11)をクリックし，i3Plasma を選択してログインしてください。

## 3. i3wm を設定する

### ターミナルを Konsole に変更する

~~共存感を出すために~~KDE に入っているコンソールに置き換えます。

```
# bindsym $mod+Return exec i3-sensible-terminal
bindsym $mod+return exec konsole
```

config ファイルの編集を適用するには Win+Shift+c を入力します。
Win キー+Return で Konsole が起動すれば成功です。

簡単に bindsym に関して説明すると，`bindsym [設定したいキーバインド] [コマンド]`で設定したキーバインドによってコマンドが実行されます。
例えば，`bindsym $mod+F2 exec firefox`と記述した場合，Win+F2 で Firefox が起動します。

### 壁紙を設定する

feh を使います。

```
$ feh /path/to/image
```

すると壁紙にしたい画像が別ウィンドウで表示されると思います。
別ウィンドウ上で右クリックし，File>Background>Set Scaled を選択します。背景が変更されます。
config に

```
exec --no-startup-id ~/.fehbg
```

を書き込むと，起動時に背景が設定されます。

### ウィンドウ間にゆとりを持たせる

今のままでは複数ウィンドウを開いたときにかなり窮屈だと思います。
config に以下の内容を追記します。

```
gaps inner 14
gaps outer -2

smart_gaps on

smart_borders on
```

いい感じに余白が出来ると思います。

## 最後に

i3wm の良さを残しつつも，フォント設定やディスプレイ設定など，CUI から設定が少し難しい部分を KDE システム設定アプリを使用して GUI から設定できるのはやはり快適です。
**i3wm の世界は沼です。**
どれぐらい沼かと言うと，[日々世界中の人がカスタマイズした自分のデスクトップ環境を貼る reddit](https://www.reddit.com/r/unixporn)があるぐらいには沼です。
ぜひ自分好みのデスクトップ環境に仕上げてみてください。
参考に私の[クソ雑な config](https://github.com/Allianaab2m/dotfiles/blob/main/dot_i3/config)と画面構成の一例を置いておきます。
![画像](image/Untitled30.jpg)
