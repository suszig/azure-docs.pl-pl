<properties
    pageTitle="Azure Mobile Apps を使用して iOS アプリにプッシュ通知を追加します。"
    description="Azure Mobile Apps を使用して iOS アプリにプッシュ通知を送信する方法について説明します。"
    services="app-service\mobile"
    documentationCenter="ios"
    manager="dwrede"
    editor=""
    authors="krisragh"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="11/25/2015"
    ms.author="krisragh"/>


# iOS アプリへのプッシュ通知の追加

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

## 概要
このチュートリアルで追加するプッシュ通知 [iOS のクイック スタート] プロジェクトをレコードが挿入されるたびにプッシュ通知が送信できるようにします。 このチュートリアルは、先に完了する必要があります [iOS のクイック スタート] のチュートリアルに基づきます。 ダウンロードしたクイック スタートのサーバー プロジェクトを使用しない場合は、プッシュ通知拡張機能パッケージをプロジェクトに追加する必要があります。 サーバーの拡張機能パッケージの詳細については、次を参照してください。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)します。 

 [IOS シミュレーターはプッシュ通知をサポートしない](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html), 、ので、このチュートリアルでは、物理 iOS デバイスを必要と [Apple Developer Program メンバーシップ](https://developer.apple.com/programs/ios/)します。 

##<a name="create-hub"></a>通知ハブの作成

[AZURE.INCLUDE [app-service-mobile-create-notification-hub](../../includes/app-service-mobile-create-notification-hub.md)]

## <a id="register"></a>プッシュ通知用アプリケーションを登録します。

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## プッシュ通知を送信するように Azure を構成する

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a id="update-server"></a>プッシュ通知を送信するサーバー プロジェクトの更新

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a name="publish-the-service"></a>サーバー プロジェクトを Azure にデプロイする

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service](../../includes/app-service-mobile-dotnet-backend-publish-service.md)]

## <a id="add-push"></a>アプリにプッシュ通知を追加します。

[AZURE.INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>アプリでプッシュ通知をテストします。

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

##<a id="more"></a>詳細

* テンプレートを利用すれば、プラットフォーム間のプッシュやローカライズされたプッシュを柔軟に送信できます。 [IOS 用 Azure Mobile Apps のクライアント ライブラリの使用方法](app-service-mobile-ios-how-to-use-client-library.md#templates) テンプレートを登録する方法を示します。
* タグを利用すれば、特定の区分に属する顧客にプッシュで的を絞ることができます。 [Azure モバイル アプリの .NET バックエンド サーバー SDK と連携](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) デバイスのインストールにタグを追加する方法を示します。

<!-- Anchors.  -->
[Generate iOS certificate signing request]: #certificates
[Register your app and enable push notifications]: #register
[Create a provisioning profile for the app]: #profile
[Add push notifications to the app]: #add-push
[Configure your mobile backend to send push requests]: #configure
[Update the server to send push notifications]: #update-server
[Publish the mobile backend to Azure]: #publish-mobile-service
[Test your app]: #test-the-service

<!-- Images. -->

<!-- URLs. -->
[iOS quick start]: app-service-mobile-ios-get-started.md

