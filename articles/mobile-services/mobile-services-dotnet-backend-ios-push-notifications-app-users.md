<properties
    pageTitle="認証されたユーザーへのプッシュ通知の送信 (.NET バックエンド)"
    description="特定の場所にプッシュ通知を送信する方法について説明します。"
    services="mobile-services,notification-hubs"
    documentationCenter="ios"
    authors="krisragh"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2015"
    ms.author="krisragh"/>


# 認証されたユーザーへのプッシュ通知の送信

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-push-users](../../includes/mobile-services-selector-push-users.md)]

このトピックでは、iOS 上の認証されたユーザーにプッシュ通知を送信する方法を説明します。 このチュートリアルを開始する前に完了 [を使ってみる認証] と [プッシュ通知の使用開始を取得] 最初です。

このチュートリアルでは、まずユーザーに認証を受けるように求める必要があります。次にプッシュ通知用の通知ハブに登録します。さらに、認証されたユーザーのみに通知が送信されるようにサーバー スクリプトを更新します。

## <a name="register"></a>登録に認証を必要とするサービスを更新します。

[AZURE.INCLUDE [mobile-services-dotnet-backend-push-notifications-app-users](../../includes/mobile-services-dotnet-backend-push-notifications-app-users.md)]

## <a name="update-app"></a>登録の前に、のログインにアプリケーションを更新します。

[AZURE.INCLUDE [mobile-services-ios-push-notifications-app-users-login](../../includes/mobile-services-ios-push-notifications-app-users-login.md)]

## <a name="test"></a>アプリケーションをテストします。

[AZURE.INCLUDE [mobile-services-ios-push-notifications-app-users-test-app](../../includes/mobile-services-ios-push-notifications-app-users-test-app.md)]




[updating the service to require authentication for registration]: #register 
[updating the app to log in before registration]: #update-app 
[testing the app]: #test 
[next steps]: #next-steps 
[get started with authentication]: mobile-services-dotnet-backend-ios-get-started-users.md 
[get started with push notifications]: mobile-services-dotnet-backend-ios-get-started-push.md 
[mobile services .net how-to conceptual reference]: /develop/mobile/how-to-guides/work-with-net-client-library 

