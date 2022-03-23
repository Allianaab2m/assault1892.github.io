---
title: "DepotDownloaderの使い方"
date: 2022-03-23
categories: ["雑多", "Steam"]
tag: ["雑多"]
lead: "Steamのサーバーから任意のバージョンのゲームを落とせるツールのつかいかた"
description: "Steam非公認ツール「DepotDownloader」を用いたゲームのダウングレードのやり方"
---

# ダウングレード欲

皆さん、Steam のゲームを遊んでいて「**古いバージョンのゲーム遊んでみたいなぁ**」とか思った事、ありませんか？私は常にあります。  
**親切なゲーム**では、ベータリリースの所に**古いバージョンを置いてくれてたりする**のですが、そんな親切なことしてくれるゲームはそうそうありません。

{{<figure src="./image/1.png" width="100%">}}

実際、今まで遊んできた Steam のゲームの中でベータの所に古いバージョンが置いてあるゲームは Undertale しか見た事無いです。

## じゃあどうやってダウングレードすんの？

「**DepotDownloader**」というツールを使います。  
結構昔からあるツールらしく、「Rainbow Six Siege」の古いバージョンをダウンロードするツールなんかはこれをフォークして開発されてるらしいです。

### 注意点

**ツールの性質上、Steam アカウントのクレデンシャルを入力する必要があります。**  
ソースコードは GitHub にて公開されていますが、どうしても不安な方は使用しない事を推奨します。

また、このツールは当然 Steam の認証を受けていない、所謂「非公認」ツールです。  
最悪の場合、自身の Steam アカウントが停止されてしまう可能性もあります。  
「自分が何をしようとしているのか」正確に理解している場合にのみこのツールを使用してください。

とは言いましたがぶっちゃけ普通に古いバージョンをダウンロードするぐらいじゃ止まらないと思います。  
~~ただそれだとしても非公認のツールを使っているのは事実なのでなんかあっても文句は言えません。~~

## 使い方

以下雑に導入方法や使い方を解説します。

### 導入

前提として「.NET 5.0 Runtime」が必要です。  
[公式サイト](https://dotnet.microsoft.com/en-us/download) から引っ張ってきてください。.NET Runtime のインストール方法に関しては割愛します。

次に GitHub から DepotDownloader 本体をダウンロードします。  
[リリースページ](https://github.com/SteamRE/DepotDownloader/releases)から最新の zip をダウンロードしてください。

zip を解凍し、任意のディレクトリに配置します。

{{<figure src="./image/2.png" width="100%">}}

中に `depotdownloader.bat` を作成し、好きなエディタで開いてください。

{{<figure src="./image/3.png" width="100%">}}

`depotdownloader.bat` に以下を**全て**コピペしてください。

```bat
@echo off

:: user を自身のユーザーネーム（ログイン時に使用する方）に置換してください
set Username=user

:: pass を自身のパスワードに置換してください
set Password=pass

:: 1234 を AppID に置換してください
set AppID=1234

:: 5678 を DepotID に置換してください
set DepotID=5678

:: 1234567890 を ManifestID に置換してください
set ManifestID=1234567890

:: 以下編集しないでください
dotnet %~dp0DepotDownloader.dll -app %AppID% -depot %DepotID% -manifest %ManifestID% -username %Username% -password %Password%

pause
```

AppID とか DepotID とかなんか変なの出てきましたが後々解説します。

### 各種 ID を揃える

ここで揃える ID は

- ゲーム本体を指定する「AppID」
- ダウンロードするファイルを指定する「DepotID」
- バージョンを指定する「ManifestID」

の 3 つです。

全て [SteamDB](https://steamdb.info/) で入手できます。

#### 入手方法

以降、「VRChat」の古いビルドを入手すると仮定します。  
ゲーム名等を欲しいゲームの物に適宜置換してください。

SteamDB にアクセスし、上の検索窓に欲しいゲームの名前を入力します（ここでは `VRChat` と入力）

{{<figure src="./image/4.png" width="100%">}}

サジェストに出てきた「VRChat」をクリックし、ゲームの詳細に飛びます。  
なんかいっぱい出てきてますが、「AppID」をメモしてください（ここでは `438100` ）

{{<figure src="./image/5.png" width="100%">}}

次に左サイドバーの「Depots」をクリックし、目的の Depot を見つけたら「DepotID」をメモしてください（ここでは `438101` 多くの場合 AppID に +1 した値が DepotID になりやすいです）  
目的の Depot がどれか分からない際、「MAX SIZE」や「EXTRA INFO」を見ると多少探しやすくなります。~~最悪勘で行きましょう~~

{{<figure src="./image/6.png" width="100%">}}

次に先程メモした DepotID をクリックし、サイドバーの `Manifests` をクリックし、目的の Manifest を見つけたら「ManifestID」をメモしてください。

{{<figure src="./image/7.png" width="100%">}}

見つけ方は「日付」しかないです。(◞‸◟)。  
ゲームによってはバージョンと ManifestID がセットになったスプレッドシート的な物が公開されている事があります（[PAYDAY 2](https://steamcommunity.com/sharedfiles/filedetails/?id=2185438382)の場合は存在している）

目的の ID を全て揃えたらいよいよ bat ファイルの改変です。

### bat を書き換え

先程コピペした bat を改変します。  
といっても `Username`, `Password`, `AppID`, `DepotID`, `ManifestID` をそれぞれ自身の ID とパスワード及び先程集めた ID に置換するだけです。  
上の VRChat の例に合わせて改変するとこんな感じになります:

```bat
set Username=user

set Password=pass

set AppID=438100

set DepotID=438101

:: ちなみにこのManifestIDは僕がVRChatを始めた際のビルド(2021/3/23リリース)です
set ManifestID=3949554128768240596
```

### Manifest をダウンロードする

bat ファイルの書き換えが終わったら実行します。  
コマンドプロンプトや Powershell 等から bat を叩いてください。

初回起動の際、Steam ガードによるメールや SMS 等の認証が求められる事があります。  
認証コードを入力して認証してください。  
後は放置してると勝手にダウンロードが終わります。  
時折エラーを吐いてる事がありますが、まぁ止まってなければ問題ないでしょう（止まってても一回落として再実行させれば途中からダウンロードされるかも？）

Manifest のダウンロードが終わると `depots/<AppID>` にファイルが格納されています。  
別のフォルダに退避させるなりそのまま exe 実行するなりご自由にどうぞ。

## 自動アップデート対策

Steam のおせっかいクソウンチ機能のせいで勝手に最新版に戻されてしまう事があります。  
防ぐには Steam をオフラインモードで起動するのが確実ですが、ゲームによってはオンラインプレイをする際 Steam がオフラインモードだと動かない場合があります。  
その際は現行バージョンと違うディレクトリに格納しておくと自動アップデートされなくなります。

## どうでもいい話

DepotDownloader の引数を見ていると、特定のベータブランチのダウンロードはもちろん、パスワード付きのブランチのダウンロードも可能です（当然パスワード必須です）  
また、（対応していれば）別の OS や OS アーキテクチャのビルドをダウンロードする事も可能ですし、特定のファイルだけダウンロードする事も可能です。
