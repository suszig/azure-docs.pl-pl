<properties
    pageTitle="iOS の認証されたユーザーへのプッシュ通知の送信 (JavaScript バックエンド)"
    description="特定のユーザーにプッシュ通知を送信する方法について説明します。"
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

[AZURE.INCLUDE [mobile-services-selector-push-users](../../includes/mobile-services-selector-push-users.md)]

このトピックでは、iOS 上の認証されたユーザーにプッシュ通知を送信する方法を説明します。 このチュートリアルを開始する前に完了 [を使ってみる認証] と [プッシュ通知の使用開始を取得] 最初です。

このチュートリアルでは、まずユーザーに認証を受けるように求める必要があります。次にプッシュ通知用の通知ハブに登録します。さらに、認証されたユーザーのみに通知が送信されるようにサーバー スクリプトを更新します。


##<a name="register"></a>登録に認証を必要とするサービスを更新します。

[AZURE.INCLUDE [mobile-services-javascript-backend-push-notifications-app-users](../../includes/mobile-services-javascript-backend-push-notifications-app-users.md)]

置き換える、 `insert` 関数を次のコードでは、クリック **保存**します。 次の insert スクリプトでは、ユーザー ID タグを使用して、ログインしたユーザーからのすべての iOS アプリケーション登録にプッシュ通知を送信します。

```
// Get the ID of the logged-in user.
var userId = user.userId;

function insert(item, user, request) {
    request.execute();
    setTimeout(function() {
        push.apns.send(userId, {
            alert: "Alert: " + item.text,
            payload: {
                "Hey, a new item arrived: '" + item.text + "'"
            }
        });
    }, 2500);
}
```

##<a name="update-app"></a>登録の前に、のログインにアプリケーションを更新します。

[AZURE.INCLUDE [mobile-services-ios-push-notifications-app-users-login](../../includes/mobile-services-ios-push-notifications-app-users-login.md)]

##<a name="test"></a>アプリケーションをテストします。

[AZURE.INCLUDE [mobile-services-ios-push-notifications-app-users-test-app](../../includes/mobile-services-ios-push-notifications-app-users-test-app.md)]



<!-- Anchors. -->
[Updating the service to require authentication for registration]: #register
[Updating the app to log in before registration]: #update-app
[Testing the app]: #test
[Next Steps]:#next-steps


<!-- URLs. -->
[Get started with authentication]: mobile-services-ios-get-started-users.md
[Get started with push notifications]: mobile-services-javascript-backend-ios-get-started-push.md
[Mobile Services .NET How-to Conceptual Reference]: mobile-services-ios-how-to-use-client-library.md

