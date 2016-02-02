<properties
    pageTitle="プッシュの使用 (Android) | Microsoft Azure"
    description="Azure Mobile Services を使用して Android .Net アプリにプッシュ通知を送信する方法について説明します。"
    services="mobile-services, notification-hubs"
    documentationCenter="android"
    authors="RickSaling"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="12/06/2015"
    ms.author="ricksal"/>


# Mobile Services アプリへのプッシュ通知の追加

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

このトピックでは、Azure モバイル サービスを使用して Android アプリにプッシュ通知を送信する方法について説明します。 このチュートリアルでは、Google Cloud Messaging (GCM) を使用したプッシュ通知をクイック スタート プロジェクトに追加します。 完了すると、モバイル サービスは、レコードが挿入されるたびにプッシュ通知を送信します。

このチュートリアルは、Mobile Services のクイック スタートに基づいています。 このチュートリアルを開始する前に完了しておく必要 [を使ってみるモバイル サービス]、プロジェクトをモバイル サービスに接続します。 そのため、このチュートリアルには、Visual Studio 2013 も必要です。

## サンプル コード

移動する完全なソース コードを確認する [ここ](https://github.com/RickSaling/mobile-services-samples/tree/push/GettingStartedWithPush)します。

## Google Cloud Messaging を有効にする

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## プッシュ要求を送信するように Mobile Services を構成する

[AZURE.INCLUDE [mobile-services-android-configure-push](../../includes/mobile-services-android-configure-push.md)]

## Mobile Services を更新してプッシュ通知を送信する

[AZURE.INCLUDE [mobile-services-dotnet-backend-android-push-update-service](../../includes/mobile-services-dotnet-backend-android-push-update-service.md)]

## アプリケーションにプッシュ通知を追加する

### Android SDK バージョンの検証

[AZURE.INCLUDE [mobile-services-verify-android-sdk-version](../../includes/mobile-services-verify-android-sdk-version.md)]


次の手順は、Google Play サービスをインストールすることです。 Google Cloud Messaging には、マニフェストの **minSdkVersion** プロパティが準拠する必要がある、開発およびテストに関する最小 API レベル要件があります。

古いデバイスを使用している場合は、Google Play Services SDK のセットアップに関するページを参考に、どれだけ小さな値を設定できるか判断し、適切に設定してください。

### プロジェクトへの Google Play Services の追加

[AZURE.INCLUDE [Add Play Services](../../includes/mobile-services-add-google-play-services.md)]

### コードの追加

[AZURE.INCLUDE [mobile-services-android-getting-started-with-push](../../includes/mobile-services-android-getting-started-with-push.md)]

## 発行されたモバイル サービスに対してアプリケーションをテストする

Android フォンを USB ケーブルで直接接続するか、エミュレーターで仮想デバイスを使用する方法により、アプリケーションをテストできます。

### ローカル テストのためにプッシュ通知を有効にする

[AZURE.INCLUDE [mobile-services-dotnet-backend-configure-local-push](../../includes/mobile-services-dotnet-backend-configure-local-push.md)]

[AZURE.INCLUDE [mobile-services-android-push-notifications-test](../../includes/mobile-services-android-push-notifications-test.md)]

これで、このチュートリアルは終了です。

## 次のステップ

このチュートリアルでは、Android アプリケーションで Mobile Services と Notification Hubs を使用してプッシュ通知を送信できるようにする方法の基本について説明しました。 次は、タグを使用して、プッシュ通知を Mobile Services から認証ユーザーにのみ送信する方法を説明した、次のチュートリアル「プッシュ通知を認証ユーザーに送信する」を行うことをお勧めします。

+ [サブスクライバーにブロードキャスト通知の送信]
    <br/>ユーザーの登録およびに興味のあるカテゴリに関してプッシュ通知を受信する方法について説明します。

+ [登録者へのテンプレート ベース通知を送信]
    <br/>テンプレートを使用して、バックエンドでプラットフォームに固有のペイロードを作成することがなく、モバイル サービスからプッシュ通知を送信する方法について説明します。

Mobile Services と Notification Hubs については次のトピックを参照してください。

* [通知ハブは何ですか?]
  <br/>通知を配信するアプリケーションにすべての主要なクライアントのプラットフォームの通知ハブの動作について説明します。

* [Notification Hubs アプリケーションをデバッグします。](http://go.microsoft.com/fwlink/p/?linkid=386630)
  </br>ガイドのトラブルシューティングと Notification Hubs ソリューションのデバッグを取得します。

* [モバイル サービス向け Android クライアント ライブラリの使用方法]
  <br/>Android と共に Mobile Services を使用する方法について説明します。







[create a new mobile service]: #create-service 
[download the service locally]: #download-the-service-locally 
[test the mobile service]: #test-the-service 
[download the getstartedwithdata project]: #download-app 
[update the app to use the mobile service for data access]: #update-app 
[test the android app against the service hosted locally]: #test-locally-hosted 
[publish the mobile service to azure]: #publish-mobile-service 
[test the android app against the service hosted in azure]: #test-azure-hosted 
[test the app against the published mobile service]: #test-app 
[next steps]: #next-steps 
[get started with push notifications (eclipse)]: mobile-services-dotnet-backend-android-get-started-push-ec.md 
[get started with mobile services]: mobile-services-dotnet-backend-android-get-started.md 
[mobile services sdk]: http://go.microsoft.com/fwlink/p/?LinkId=257545 
[how to use the android client library for mobile services]: mobile-services-android-how-to-use-client-library.md 
[what are notification hubs?]: ../notification-hubs-overview.md 
[send broadcast notifications to subscribers]: ../notification-hubs-windows-store-dotnet-send-breaking-news.md 
[send template-based notifications to subscribers]: ../notification-hubs-windows-store-dotnet-send-localized-breaking-news.md 

