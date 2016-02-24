<properties
    pageTitle="Azure Mobile Engagement Android SDK の統合"
    description="Android SDK for Azure Mobile Engagement の最新の更新情報と更新手順について"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/10/2015"
    ms.author="piyushjo" />

#GCM を Mobile Engagement に統合する方法

> [AZURE.IMPORTANT] このガイドの手順の前に、Android ドキュメントのエンゲージメントを統合する方法で説明する統合手順を従う必要があります。
>
> このドキュメントは、いつでも実施できるキャンペーンをサポートする Reach モジュールを統合した場合にのみ役立ちます。 アプリケーションに Reach キャンペーンを統合するには、Engagement Reach を Android に統合する方法に関するドキュメントを先にお読みください。

##はじめに

GCM を統合すると、アプリケーションをプッシュできます。

SDK にプッシュされた GCM ペイロードのデータ オブジェクトには常に `azme` キーが含まれています。 そのため、アプリケーションで別の目的で GCM を使用する場合、そのキーに基づいてプッシュをフィルター処理できます。

> [AZURE.IMPORTANT] Android 2.2 を実行しているデバイスのみ、インストールし、Google を Google Play にあるバック グラウンド接続が有効になっているプッシュされる GCM; を使用してまたはただし、このコードは (インテントのみを使用します)、サポートされていないデバイスでも安全を統合することができます。

##GCM にサインアップして GCM Service を有効にする

まだ実行してない場合は、GCM Service を Google アカウントで有効にする必要があります。

(2014 年 2 月 5 日)、このドキュメントの作成時にある手順を行うことができる: [<http://developer.android.com/guide/google/gcm/gs.html>]します。

自分のアカウントで GCM を有効にするためにのみ、その手順に従ってください。 達すると、 **API キーの取得** セクションをしないして読み込んだ、Google 手順を実行する代わりにこのページに戻ってください。

手順の説明で、 **プロジェクト番号** として使用される、 **GCM 送信者 ID**, 、この手順の後半で必要があります。

> [AZURE.IMPORTANT] **プロジェクト番号** を混同しないは **プロジェクト ID**します。 プロジェクト ID は異なるものにすることができるようになりました (それは新しいプロジェクトの名前です)。 Engagement SDK に統合する必要がありますが、 **プロジェクト番号** に表示されます、 **概要** ] メニューに、 [Google Developers Console]します。

##SDK の統合

### デバイス登録の管理

各デバイスは Google サーバーに登録コマンドを送信する必要があります。そうしないと、デバイスに接続できません。

デバイスは、GCM 通知の登録を解除することもできます (アプリケーションをアンインストールすると、デバイスは自動的に登録解除されます)。

使用する場合、 [GCM client library], 、android sdk-gcm の受信を直接読み取ることができます。

登録インテントをまだ自分で送信していない場合は、Engagement にデバイスの自動登録を実行させることができます。

これを有効にするには、次を `AndroidManifest.xml` ファイルの `<application/>` タグ内に追加します。

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### 登録 ID を Engagement Push サービスに伝達し、通知を受信する

デバイスの登録 ID を Engagement Push サービスに伝達し、その通知を受信するためには、以下を `AndroidManifest.xml` ファイルの `<application/>` タグ内に追加します (これはデバイス登録を自分で管理する場合にも実行します)。

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

次のアクセス権限が `AndroidManifest.xml` 内に設定されていることを確認します (`</application>` タグの後ろ)。

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

##サーバー API キーへの Engagement のアクセス権限を与える

まだ行われていない場合は、作成、 **サーバー API キー** に [Google Developers Console]します。

サーバー キー **は IP 制限がある**です。

このドキュメントの作成時点 (2014 年 2 月 5 日) では、手順は次のようになります。

-   これを行うには、開く [Google Developers Console]します。
-   上記の手順で、同じプロジェクトを選択 (その 1 つを **プロジェクト番号** に統合した `AndroidManifest.xml`)。
-   [APIs & auth]、[Credentials] の順に移動し、[Public API access] セクションの [CREATE NEW KEY] をクリックします。
-   [Server key] を選びます。
-   次の画面で空白のままに **(IP 制限なし)**, 、[作成] をクリックします。
-   生成された **API キー**します。
-   $/\#application/YOUR\_ENGAGEMENT\_APPID/native-push に移動します。
-   GCM セクションで、生成してコピーした API キーを貼り付けて編集します。

これで、Reach 通知とポーリングを作成するときに [いつでも] を選択することができます。

> [AZURE.IMPORTANT] エンゲージメントは実際に必要な **サーバー キー**, 、Android キーは Engagement サーバーで使用することはできません。

##テスト

「Android での Engagement の統合をテストする方法」を読んで、統合を検証してください。


[<http://developer.android.com/guide/google/gcm/gs.html>]:http://developer.android.com/guide/google/gcm/gs.html
[Google Developers Console]:https://cloud.google.com/console
[GCM client library]:http://developer.android.com/guide/google/gcm/gs.html#libs
[Google Developers Console]:https://cloud.google.com/console

