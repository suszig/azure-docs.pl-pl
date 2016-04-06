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

このチュートリアルは、モバイル サービスのクイック スタートに基づいています。 このチュートリアルを開始する前に完了しておく必要 [Get started with Mobile Services] 、プロジェクトをモバイル サービスに接続します。 そのため、このチュートリアルには、Visual Studio 2013 も必要です。

## サンプル コード
移動する完全なソース コードを確認する [ここ](https://github.com/RickSaling/mobile-services-samples/tree/push/GettingStartedWithPush)します。

## Google Cloud Messaging を有効にする

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## プッシュ要求を送信するように Mobile Services を構成する

[AZURE.INCLUDE [mobile-services-android-configure-push](../../includes/mobile-services-android-configure-push.md)]

## Mobile Services を更新してプッシュ通知を送信する

[AZURE.INCLUDE [mobile-services-dotnet-backend-android-push-update-service](../../includes/mobile-services-dotnet-backend-android-push-update-service.md)]

## アプリケーションにプッシュ通知を追加する

###Android SDK バージョンの検証

[AZURE.INCLUDE [mobile-services-verify-android-sdk-version](../../includes/mobile-services-verify-android-sdk-version.md)]


次の手順は、Google Play サービスをインストールすることです。 開発、およびテストに関する最小 API レベル要件を持つ Google Cloud Messaging、 **minSdkVersion** マニフェスト内のプロパティが準拠する必要があります。

古いデバイスを使用している場合は、Google Play Services SDK のセットアップに関するページを参考に、どれだけ小さな値を設定できるか判断し、適切に設定してください。

###プロジェクトへの Google Play Services の追加

[AZURE.INCLUDE [Add Play Services](../../includes/mobile-services-add-google-play-services.md)]

###コードの追加

[AZURE.INCLUDE [mobile-services-android-getting-started-with-push](../../includes/mobile-services-android-getting-started-with-push.md)]

## 発行されたモバイル サービスに対してアプリケーションをテストする

Android フォンを USB ケーブルで直接接続するか、エミュレーターで仮想デバイスを使用する方法により、アプリケーションをテストできます。

### ローカル テストのためにプッシュ通知を有効にする

[AZURE.INCLUDE [mobile-services-dotnet-backend-configure-local-push](../../includes/mobile-services-dotnet-backend-configure-local-push.md)]

[AZURE.INCLUDE [mobile-services-android-push-notifications-test](../../includes/mobile-services-android-push-notifications-test.md)]

これで、このチュートリアルは終了です。

## 次のステップ

このチュートリアルでは、Android アプリケーションで Mobile Services と Notification Hubs を使用してプッシュ通知を送信できるようにする方法の基本について説明しました。 次は、タグを使用して、プッシュ通知を Mobile Services から認証ユーザーにのみ送信する方法を説明した、次のチュートリアル「プッシュ通知を認証ユーザーに送信する」を行うことをお勧めします。

+ [Send broadcast notifications to subscribers]
    <br/>ユーザーが興味のあるカテゴリに関してプッシュ通知を登録して、プッシュ通知を受信できるようにする方法について説明します。

+ [Send template-based notifications to subscribers]
    <br/>テンプレートを使用して、バックエンドでプラットフォームに固有のペイロードを作成する必要なくモバイル サービスからプッシュ通知を送信する方法について説明します。

Mobile Services と Notification Hubs については次のトピックを参照してください。

* [What are Notification Hubs?]
  <br/>通知ハブがすべての主要なクライアント プラットフォーム全体のアプリケーションに通知を配信するための動作を説明します。

* [通知ハブのデバッグ](http://go.microsoft.com/fwlink/p/?linkid=386630)
  </br>Notification Hubs ソリューションのトラブルシューティングとデバッグについて説明します。

* [How to use the Android client library for Mobile Services]
  <br/>Android で Mobile Services を使用する方法について説明します。

<!-- Anchors. -->

[Create a new mobile service]: #create-service
[Download the service locally]: #download-the-service-locally
[Test the mobile service]: #test-the-service
[Download the GetStartedWithData project]: #download-app
[Update the app to use the mobile service for data access]: #update-app
[Test the Android App against the service hosted locally]: #test-locally-hosted
[Publish the mobile service to Azure]: #publish-mobile-service
[Test the Android App against the service hosted in Azure]: #test-azure-hosted
[Test the app against the published mobile service]: #test-app
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Get started with push notifications (Eclipse)]: mobile-services-dotnet-backend-android-get-started-push-ec.md
[Get started with Mobile Services]: mobile-services-dotnet-backend-android-get-started.md
[Mobile Services SDK]: http://go.microsoft.com/fwlink/p/?LinkId=257545

[How to use the Android client library for Mobile Services]: mobile-services-android-how-to-use-client-library.md

[What are Notification Hubs?]: ../notification-hubs-overview.md
[Send broadcast notifications to subscribers]: ../notification-hubs-windows-store-dotnet-send-breaking-news.md
[Send template-based notifications to subscribers]: ../notification-hubs-windows-store-dotnet-send-localized-breaking-news.md


