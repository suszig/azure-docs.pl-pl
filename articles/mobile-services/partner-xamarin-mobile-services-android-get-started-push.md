<properties
    pageTitle="Xamarin Android アプリへのプッシュ通知の追加 | Microsoft Azure"
    description="Azure Mobile Services と Azure Notification Hubs を使用している Xamarin.Android アプリ向けに、Google Cloud Messaging を使用したプッシュ通知を構成する方法について説明します。"
    documentationCenter="xamarin"
    authors="ggailey777"
    manager="dwrede"
    services="mobile-services"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="12/07/2015"
    ms.author="glenga"/>


# Mobile Services アプリへのプッシュ通知の追加

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started-push](../../includes/mobile-services-selector-get-started-push.md)]

## 概要

このトピックでは、Azure Mobile Services を使用して Xamarin Android アプリにプッシュ通知を送信する方法について説明します。 このチュートリアルでは、Google Cloud Messaging (GCM) サービスを使用してプッシュ通知を追加、[を使ってみるモバイル サービス] プロジェクトです。 完了すると、モバイル サービスは、レコードが挿入されるたびにプッシュ通知を送信します。

このチュートリアルには、次のものが必要です。

+ アクティブな Google アカウント。
+ [Google Cloud Messaging クライアント コンポーネント] です。 このコンポーネントは、チュートリアル中に追加します。

[Xamarin.Android] が既にあり、[Azure Mobile Services コンポーネント] は、いずれかを完了すると、プロジェクトにインストールされている [モバイル サービスの開始を取得] です。

## <a id="register"></a>Google Cloud Messaging を有効にします。

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a id="configure"></a>プッシュ要求を送信する、モバイル サービスを構成します。

[AZURE.INCLUDE [mobile-services-android-configure-push](../../includes/mobile-services-android-configure-push.md)]

## <a id="update-scripts"></a>登録の更新通知を送信するスクリプトを挿入します。

>[AZURE.TIP] 次の手順では、Azure クラシック ポータルの TodoItem テーブルに対する挿入操作に登録されているスクリプトを更新する方法を示します。 このモバイル サービス スクリプトは、Visual Studio のサーバー エクスプローラーの Azure ノードで、直接アクセスして編集することもできます。

[AZURE.INCLUDE [mobile-services-javascript-backend-android-push-insert-script](../../includes/mobile-services-javascript-backend-android-push-insert-script.md)]


## <a id="configure-app"></a>プッシュ通知の既存のプロジェクトを構成します。

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

## <a id="add-push"></a>アプリにプッシュ通知コードを追加します。

[AZURE.INCLUDE [mobile-services-xamarin-android-push-add-to-app](../../includes/mobile-services-xamarin-android-push-add-to-app.md)]

## <a id="test"></a>アプリでプッシュ通知をテストします。

Android フォンを USB ケーブルで直接接続するか、エミュレーターで仮想デバイスを使用する方法により、アプリケーションをテストできます。

[AZURE.INCLUDE [mobile-services-android-push-notifications-test](../../includes/mobile-services-android-push-notifications-test.md)]

これで、このチュートリアルは終了です。

## <a name="next-steps"></a>次のステップ

Mobile Services と Notification Hubs については次のトピックを参照してください。

* [認証を使用します。](mobile-services-android-get-started-users.md)
  <br/>別のアカウントの種類のモバイル サービスを使用して、アプリのユーザーを認証する方法について説明します。

* [Notification Hubs とは](../notification-hubs-overview.md)
  <br/>通知を配信するアプリケーションにすべての主要なクライアントのプラットフォームの通知ハブの動作について説明します。

* [Notification Hubs アプリケーションをデバッグします。](http://go.microsoft.com/fwlink/p/?linkid=386630)
  </br>ガイドのトラブルシューティングと Notification Hubs ソリューションのデバッグを取得します。

* [Mobile Services 向け .NET クライアント ライブラリを使用する方法](mobile-services-windows-dotnet-how-to-use-client-library.md)
  <br/>Xamarin c# コードと共に Mobile Services を使用する方法について説明します。

* [モバイル サービスのサーバー スクリプト リファレンス](mobile-services-how-to-use-server-scripts.md)
  <br/>モバイル サービスにビジネス ロジックを実装する方法について説明します。



[get started with mobile services]: mobile-services-ios-get-started.md 
[google cloud messaging client component]: http://components.xamarin.com/view/GCMClient/ 
[xamarin.android]: http://xamarin.com/download/ 
[azure mobile services component]: http://components.xamarin.com/view/azure-mobile-services/ 

