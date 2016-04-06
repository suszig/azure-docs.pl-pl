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

##概要
このトピックでは、Azure Mobile Services を使用して Xamarin Android アプリにプッシュ通知を送信する方法について説明します。 このチュートリアルでは、Google Cloud Messaging (GCM) サービスを使用してプッシュ通知を追加、 [Get started with Mobile Services] プロジェクトです。 完了すると、モバイル サービスは、レコードが挿入されるたびにプッシュ通知を送信します。

このチュートリアルには、次のものが必要です。

+ アクティブな Google アカウント。
+ [Google Cloud Messaging Client Component]. このコンポーネントは、チュートリアル中に追加します。

いれば、 [Xamarin.Android] と [Azure Mobile Services Component] からいずれかを完了すると、プロジェクトにインストールされている [Get started with Mobile Services]します。

##<a id="register"></a>Google Cloud Messaging を有効にする

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a id="configure"></a>プッシュ要求を送信するように Mobile Services を構成する

[AZURE.INCLUDE [mobile-services-android-configure-push](../../includes/mobile-services-android-configure-push.md)]

##<a id="update-scripts"></a>通知を送信するように登録済み挿入スクリプトを更新する

>[AZURE.TIP] 次の手順では、Azure 旧ポータルで、TodoItem テーブルに対する挿入操作に登録されているスクリプトを更新する方法を説明します。 このモバイル サービス スクリプトは、Visual Studio のサーバー エクスプローラーの Azure ノードで、直接アクセスして編集することもできます。

[AZURE.INCLUDE [mobile-services-javascript-backend-android-push-insert-script](../../includes/mobile-services-javascript-backend-android-push-insert-script.md)]


##<a id="configure-app"></a>プッシュ通知の既存のプロジェクトを構成する

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>アプリケーションにプッシュ通知コードを追加する

[AZURE.INCLUDE [mobile-services-xamarin-android-push-add-to-app](../../includes/mobile-services-xamarin-android-push-add-to-app.md)]

##<a id="test"></a>アプリケーションでプッシュ通知をテストする

Android フォンを USB ケーブルで直接接続するか、エミュレーターで仮想デバイスを使用する方法により、アプリケーションをテストできます。

[AZURE.INCLUDE [mobile-services-android-push-notifications-test](../../includes/mobile-services-android-push-notifications-test.md)]

これで、このチュートリアルは終了です。

## <a name="next-steps"></a>次のステップ

Mobile Services と Notification Hubs については次のトピックを参照してください。

* [認証の使用](mobile-services-android-get-started-users.md)
  <br/>Mobile Services を使用して別のアカウントの種類のアプリケーションのユーザーを認証する方法について説明します。

* [Microsoft Azure 通知ハブとは](../notification-hubs-overview.md)
  <br/>Notification Hubs がすべての主要なクライアント プラットフォーム全体のアプリケーションに通知を配信するための動作を説明します。

* [通知ハブのデバッグ](http://go.microsoft.com/fwlink/p/?linkid=386630)
  </br>Notification Hubs ソリューションのトラブルシューティングとデバッグについて説明します。

* [Mobile Services 向け .NET クライアント ライブラリを使用する方法](mobile-services-windows-dotnet-how-to-use-client-library.md)
  <br/>Xamarin c# コードと共に Mobile Services を使用する方法について説明します。

* [モバイル サービスのサーバー スクリプト リファレンス](mobile-services-how-to-use-server-scripts.md)
  <br/>Mobile Services にビジネス ロジックを実装する方法について説明します。

<!-- URLs. -->
[Get started with Mobile Services]: mobile-services-ios-get-started.md

[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Xamarin.Android]: http://xamarin.com/download/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/


