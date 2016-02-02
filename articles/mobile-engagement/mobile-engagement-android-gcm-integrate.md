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


# GCM を Mobile Engagement に統合する方法

> [AZURE.IMPORTANT] このガイドの手順を実行する前に、「Engagement を Android に統合する方法」のドキュメントの統合手順を実行する必要があります。
>
> このドキュメントは、いつでも実施できるキャンペーンをサポートする Reach モジュールを統合した場合にのみ役立ちます。 アプリケーションに Reach キャンペーンを統合するには、Engagement Reach を Android に統合する方法に関するドキュメントを先にお読みください。

## はじめに

GCM を統合すると、アプリケーションをプッシュできます。

常に、SDK にプッシュされた GCM ペイロードを含む、 `azme` データ オブジェクトのキー。 そのため、アプリケーションで別の目的で GCM を使用する場合、そのキーに基づいてプッシュをフィルター処理できます。
> [AZURE.IMPORTANT] Android 2.2 以降を実行中のデバイスで、Google Play がインストールされ、Google バックグラウンド接続が有効になっているデバイスのみを GCM でプッシュできます。ただし、このコードはサポートされていないデバイスで安全に統合できます (インテントのみを使用します)。

## GCM にサインアップして GCM Service を有効にする

まだ実行してない場合は、GCM Service を Google アカウントで有効にする必要があります。

(2014 年 2 月 5 日)、このドキュメントの作成時にある手順を行うことができる: [<http://developer.android.com/guide/google/gcm/gs.html>] です。

自分のアカウントで GCM を有効にするためにのみ、その手順に従ってください。 「**API キーの取得**」セクションまで進んだら、Google の次の手順を実行するのではなく、このページに戻ってください。

この手順の説明では、**プロジェクト番号**が **GCM 送信者 ID** として使われています。この情報はこの手順の後半で必要になります。
> [AZURE.IMPORTANT] **プロジェクト番号**と**プロジェクト ID** を混同しないでください。 プロジェクト ID は異なるものにすることができるようになりました (それは新しいプロジェクトの名前です)。 エンゲージメント SDK に統合する必要があります、 **プロジェクト番号** に表示されると、 **概要** ] メニューの [Google Developers Console] にします。

## SDK の統合

### デバイス登録の管理

各デバイスは Google サーバーに登録コマンドを送信する必要があります。そうしないと、デバイスに接続できません。

デバイスは、GCM 通知の登録を解除することもできます (アプリケーションをアンインストールすると、デバイスは自動的に登録解除されます)。

[GCM クライアント ライブラリ] を使用する場合は、android sdk-gcm-受信直接読み取ることができます。

登録インテントをまだ自分で送信していない場合は、Engagement にデバイスの自動登録を実行させることができます。

これを有効にするには、次の追加、 `AndroidManifest.xml` ファイル内、 `< application/>` タグ。

            
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### 登録 ID を Engagement Push サービスに伝達し、通知を受信する

Engagement Push サービスにデバイスの登録 id を伝達し、その通知を受信するために、次を追加して、 `AndroidManifest.xml` ファイル内、 `< アプリケーション/>` (デバイスの登録自分で管理する) 場合でもタグを付けます。

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

次のアクセス許可があることを確認、 `AndroidManifest.xml` (後、 `</アプリケーション >` タグ)。

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

## サーバー API キーへの Engagement のアクセス権限を与える

まだ行われていない場合は、作成、 **サーバー API キー** [Google Developers Console] にします。

サーバー キーには **IP 制限を設定することはできません**。

このドキュメントの作成時点 (2014 年 2 月 5 日) では、手順は次のようになります。

-   これを行うには、[Google Developers Console] を開きます。
-   上記の手順で、同じプロジェクトを選択 (その 1 つを **プロジェクト番号** に統合した `AndroidManifest.xml`)。
-   [APIs & auth]、[Credentials] の順に移動し、[Public API access] セクションの [CREATE NEW KEY] をクリックします。
-   [Server key] を選びます。
-   次の画面は空白のままにし **(IP 制限なし)**、[Create] をクリックします。
-   生成された **API キー**をコピーします。
-   $/\#application/YOUR\_ENGAGEMENT\_APPID/native-push に移動します。
-   GCM セクションで、生成してコピーした API キーを貼り付けて編集します。

これで、Reach 通知とポーリングを作成するときに [いつでも] を選択することができます。
> [AZURE.IMPORTANT] Engagement では**サーバー キー**が必要です。Android キーは Engagement サーバーでは使えません。

## テスト

「Android での Engagement の統合をテストする方法」を読んで、統合を検証してください。



[<http://developer.android.com/guide/google/gcm/gs.html>]: http://developer.android.com/guide/google/gcm/gs.html 
[google developers console]: https://cloud.google.com/console 
[gcm client library]: http://developer.android.com/guide/google/gcm/gs.html#libs 
[google developers console]: https://cloud.google.com/console 

