---
title: "日本語版 AvatarDynamics オープンベータリリース告知"
date: 2022-03-18
categories: ["翻訳", "VRChat"]
tag: ["VRChat"]
lead: "元ページ: https://docs.vrchat.com/v2022.1.2/docs/latest-release 及び 公式 Discord #open-beta-announcements"
description: "AvatarDynamics リリース告知の日本語化"
---

# from Discord

オープンベータテスターの皆さん、こんにちは！  
さて、「OSC」「IK Beta」ではまだまだ満足できないだろうなぁと思っています。  
もうちょっと「アバター」が「ダイナミック」に動いてくれたらな、と思いませんか？

**VRChat 2022.1.2, Build 1181 がオープンベータとして公開されました -- Avatar Dynamic オープンベータ公開！！！！**

今回のアップデートは盛りだくさんなので、先に気になる疑問を解決してからパッチノートに移りましょう。

- はい、**PhysicsBone は Quest で動作します！** やったぜ。
- **ブログの記事を読んでおく**と良い「スタートダッシュ」が切れるでしょう: https://hello.vrchat.com/blog/avatar-dynamics-open-beta
- パッチノートや技術的な詳細に関しては、**ドキュメントをご覧ください**: https://docs.vrchat.com/v2022.1.2/docs/latest-release
- **VRChat に飛び込んで、Avatar Dynamics Hub をチェックしよう！** クローズド β テスターの皆様が制作したテストアバターで Avatar Dynamics の挙動を確かめることができます。  
  PhysicsBone や Contacts のシンプルな実装から、Avatar Dynamics システムで出来る事の限界に挑んだ複雑なアバターまで、様々な物があります。  
  [こちらのワールドリンク](https://vrchat.com/home/world/wrld_f995a2eb-7ddc-4558-aef1-815c3b23df6c)を使うか、VRChat Home のポータルを経由して移動する事が出来ます。
- **Live 版との互換性はありません。** 申し訳ありません...

更新された SDK はこちらです:  
アバター用 SDK : https://files.vrchat.cloud/sdk/VRCSDK3-AVATAR-2022.03.04.12.28_Public.unitypackage  
ワールド用 SDK : 更新はありません。

**このオープンベータはネットワーク互換性が確保されない事に注意してください。**  
**つまり、ベータビルドを使用している際は Live ビルドのプレイヤーに参加する事が出来ず、相手もあなたに参加する事が出来ません。**  
カスタムステータスを使用して、あなたがベータビルドを使用している事を友達に知らせた方が良いかもしれません。

もしバグを見つけたり、フィードバックがあれば、オープンベータフィードバックボードに投稿してください: https://feedback.vrchat.com/open-beta

もし**バグを見つけたりしてもフィードバックボードに投稿されない場合は「報告されなかった」とみなし、修正や対処はされないためご注意ください。**

このリリースは Steam、Oculus PC、Oculus Quest で利用可能です。  
リリースノートの全文は[こちら](https://docs.vrchat.com/v2022.1.2/docs/latest-release)で確認できます！

# from Patchnote

## クライアント

**[Avatar Dynamics](https://docs.vrchat.com/v2022.1.2/docs/avatar-dynamics) が実装されました！** これは、VRChat のアバターに「直感的な」機能を追加する、全く新しいカテゴリーのシステムです。  
まずはブログの記事をご覧ください。

### 機能

- **[PhysicsBone](https://docs.vrchat.com/v2022.1.2/docs/physbones) を実装しました！** PhysicsBone は、髪の毛やしっぽ等をよりリアルに描写する為の二次的な動きを可能にするコンポーネントです。  
  技術的な詳細と実装上の重要な注意点は[ドキュメント](https://docs.vrchat.com/v2022.1.2/docs/physbones)をご覧ください。

  - このシステムは、**全プラットフォームで利用可能です -- もちろん Quest もです！**
  - PhysicsBone をセットアップして、掴んだり、引っ張ったり、伸ばしたり、ポーズを取ったりする事が出来るようになります。
  - 他の人の PhysicsBone を触る事ができ、影響を受けたボーンを押したり、ポーズを取ったりする事が出来るようになります。友達の髪をセットしたい？簡単に出来ます！
  - **カスタム [PhysicsBone](https://docs.vrchat.com/v2022.1.2/docs/physbones) コライダーをセット** して、様々な効果を得る事が出来ます。
  - [PhysicsBone のドキュメント](https://docs.vrchat.com/v2022.1.2/docs/physbones)に記載されている、 **全てのアバター** 用のデフォルトとの指と手のコライダーを定義したため、いつでも PhysicsBone を触る事が出来ます。
  - これらの定義済みコライダーをカスタマイズする為の UI を AvatarDescriptor に統合したため、アバター作成時にいつでも不要な物を無効化出来ます。
  - [PhysicsBone](https://docs.vrchat.com/v2022.1.2/docs/physbones) を使用してアバターの[アニメーターパラメーター](https://docs.vrchat.com/v2022.1.2/docs/animator-parameters)に影響を与える事が出来るため、誰かが PhysicsBone を掴んだり、引っ張ったり、ポーズを取らせたりしたのを検知する事が出来ます！
  - PhysicsBone はパフォーマンスがとても高く、DynamicBone に比べて **数桁高いパフォーマンス** を提供する事が出来ます。

- **[Contacts](https://docs.vrchat.com/v2022.1.2/docs/contacts) を実装しました！** Contacts はシグナルを送受信する空間のボリュームを定義する事が出来ます。  
  これらのシグナルは、アバターの[アニメーターパラメーター](https://docs.vrchat.com/v2022.1.2/docs/animator-parameters)に使用する事が出来ます。

  - このシステムももちろん **全プラットフォームで利用可能です -- 当然 Quest も！**
  - Contact Senders はシグナルを **送信** します。送信側と受信側の両方で、少なくとも 1 組の文字列が一致しないと衝突が発生しません。衝突タグは大文字と小文字を区別します。
  - Contact Receivers はシグナルを **受信** します。上記のように、送信側と受信側の両方で、少なくとも 1 組の文字列が一致する必要があります。

- **Avatar Dynamics Permission を実装しました！** [Permission と設定](https://docs.vrchat.com/v2022.1.2/docs/permissions-and-settings)のインターフェースでは、アバター上で Avatar Dynamics システムとインタラクトできる人を、「無効」「フレンドのみ」「誰でも」選択する事が出来ます。  
  インタラクションを有効化するには、 **両方のユーザーから許可されている必要があります** 。

  - ユーザーのネームプレートに新しいアイコンが表示され、許可の状態が示されます。
  - 左手だけが点灯している場合、自分には触る事が出来ますが、相手に触る事は出来ません。
  - 右手だけが点灯している場合、相手に触る事は出来ますが、自分に触る事は出来ません。
  - 握手アイコンになっている場合、Avatar Dynamics インタラクションが有効です。
  - どちらの手も点灯していない場合、両者共に許可をしていません。
    - 訳注: 合っているか不明。相手に触るには当然 VR 環境必須っぽさそう。

- **DynamicBone から PhysicsBone へのランタイムオートコンバーター機能を実装しました！**
  - Bone と Collider コンポーネントを含む DynamicBone を PhysicsBone に変換するための最善の試みがなされています。
  - 2 つのシステム間でメソッド、変数、計算式が異なる為、全てのアバターで変換が完璧で行われるとは限りません。  
    複雑な設定が行われているアバターでは PhysicsBone の設定を手動で行い、再アップロードする必要があるかもしれません。
  - PhysicsBone に存在しない幾つかの DynamicBone の機能（「Keep-Inside」コライダー等）は変換されず、削除されます。
  - このシステムはデフォルトで有効になっています。  
    VRChat の[Performance Options](https://docs.vrchat.com/v2022.1.2/docs/vrchat-configuration-window)で無効化する事が出来ます。  
    無効化すると、ローカルで PB→DB コンバーターが実行されなくなり、通常通り DynamicBone が動作します。  
    言い換えれば、このコンバーターはローカルです。

### 変更、改良、修正

- シミュレーション時間の改善により、ネットワーク動作が全体的に改善され、ネットワーク関連のエッジケースのバグがいくつか修正されました。
- 入力がフレームレート依存ではなくなりました。

### クリエイター向け情報

- PhysicsBone は DynamicBone とは少し異なる動作をします。  
  そのため、PhysicsBone の[ドキュメント](https://docs.vrchat.com/v2022.1.2/docs/physbones)に使用上のヒントを記載しました。ぜひご覧ください。
- Avatar Dynamics を使用するには SDK3 にアップデートする必要があります。
- Avatar Dynamics は SDK3 専用です。

## アバター SDK

この SDK から、Burst と Mathematics が必須パッケージになりました。  
もしインストールされていない場合は、SDK インストール時に一緒にインストールされるようになります。

### 機能

- Avatar Dynamics Component を実装しました。
  - VRC_PhysBone とコライダー、Contact Senders と Receivers。
- Avatar Dynamics の Avatar Descriptor にいくつかの追加コントロールと設定を実装しました。
- DynamicBone から PhysicsBone への自動変換を実装しました。  
  アバターから DynamicBone が検出された場合、Build 画面で自動的に PhysicsBone に変換されます。

## ドキュメンテーション

### 変更

- アバターパフォーマンスランキングシステムのページが更新され、PhysicsBone と Contacts の制限が追加されました。
- VRChat が提供する全ての Prefab は SDK Prefabs ページで公開されるようになり、Prefabs カテゴリは削除されました。
- 多くの未使用、未発表、非推奨のドキュメントページが削除されました。
- 2017, 2018, 2019, 2020 年のアーカイブされたパッチノートは、1 つのページにまとめられました。
