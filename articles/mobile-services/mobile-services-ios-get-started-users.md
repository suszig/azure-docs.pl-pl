<properties
    pageTitle="既存の Azure Mobile Services アプリへの認証の追加 (iOS) | JavaScript バックエンド | Microsoft Azure"
    description="Mobile Services を使用して、Google、Facebook、Twitter、Microsoft などのさまざまな ID プロバイダーを通じて iOS アプリのユーザーを認証する方法について説明します。"
    services="mobile-services"
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


# 既存の Azure Mobile Services アプリに認証を追加する

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

[Mobile Services クイック スタート チュートリアル] に認証を追加するこのチュートリアルではサポートされている id プロバイダーを使用します。

まず、[Mobile Services クイック スタート チュートリアル]」を完了することをお勧めします。 または、[Azure クラシック ポータル] をクリックしてから iOS のクイック スタート プロジェクトをダウンロード  **モバイル サービス** >、モバイル サービス > クラウドの左上隅にサインイン > **iOS** > **新しい iOS アプリを作成** > **をダウンロードし、アプリの実行** > **Objective C** > **ダウンロード**します。 テーブルをまだ作成していない場合は、**[TodoItem テーブルを作成する]** をクリックしてから、**[ダウンロード]** をクリックしてください。

## <a name="register"></a>認証用のアプリを登録します。

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

## <a name="permissions"></a>認証されたユーザーへのデータのアクセス許可を制限します。

[AZURE.INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)]

## <a name="add-authentication"></a>アプリに認証を追加します。

[AZURE.INCLUDE [mobile-services-ios-authenticate-app](../../includes/mobile-services-ios-authenticate-app.md)]

## <a name="store-authentication"></a>アプリケーションに認証トークンを保存します。

[AZURE.INCLUDE [mobile-services-ios-authenticate-app-with-token](../../includes/mobile-services-ios-authenticate-app-with-token.md)]

## <a name="next-steps"></a>次のステップ

次に、説明 [、ユーザー ID 値を使用して、返されたデータをフィルター処理する方法](mobile-services-javascript-backend-service-side-authorization.md)します。









[register your app for authentication and configure mobile services]: #register 
[restrict table permissions to authenticated users]: #permissions 
[add authentication to the app]: #add-authentication 
[next steps]: #next-steps 
[storing authentication tokens in your app]: #store-authentication 
[4]: ./media/mobile-services-ios-get-started-users/mobile-services-selection.png 
[5]: ./media/mobile-services-ios-get-started-users/mobile-service-uri.png 
[13]: ./media/mobile-services-ios-get-started-users/mobile-identity-tab.png 
[14]: ./media/mobile-services-ios-get-started-users/mobile-portal-data-tables.png 
[15]: ./media/mobile-services-ios-get-started-users/mobile-portal-change-table-perms.png 
[service-side authorization of mobile services users]: mobile-services-javascript-backend-service-side-authorization.md 
[submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582 
[my applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039 
[live sdk for windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253 
[single sign-on for windows store apps by using live connect]: /develop/mobile/tutorials/single-sign-on-windows-8-dotnet 
[mobile services quick start tutorial]: /develop/mobile/tutorials/get-started-ios 
[get started with data]: /develop/mobile/tutorials/get-started-with-data-ios 
[get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios 
[get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios 
[authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios 
[azure classic portal]: https://manage.windowsazure.com/ 

