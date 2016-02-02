<properties
    pageTitle="既存の Azure Mobile Services アプリへの認証の追加 (iOS) | .NET バックエンド | Microsoft Azure"
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


# 既存の Azure Mobile Services アプリへの認証の追加

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

このチュートリアルでは、サポートされている ID プロバイダーを使用して、クイック スタート プロジェクトに認証を追加します。 このチュートリアルは、[Mobile Services クイック スタート チュートリアル] に基づいて最初に完了する必要があります。

## <a name="register"></a>アプリケーションを認証に登録し、モバイル サービスを構成します。

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

[AZURE.INCLUDE [mobile-services-dotnet-backend-aad-server-extension](../../includes/mobile-services-dotnet-backend-aad-server-extension.md)]

## <a name="permissions"></a>認証されたユーザーへのアクセス許可を制限します。

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)]

Xcode で、プロジェクトを開きます。 キーを押して、 **実行** アプリを起動するボタンをクリックします。 アプリケーションの起動後に、状態コード 401 (許可されていません) の例外が発生することを確認します。 この問題は、認証されないユーザーとしてアプリケーションがモバイル サービスにアクセスしようとしても、_TodoItem_ テーブルでは認証が要求されるために発生します。

## <a name="add-authentication"></a>アプリに認証を追加します。

[AZURE.INCLUDE [mobile-services-ios-authenticate-app](../../includes/mobile-services-ios-authenticate-app.md)]

## <a name="store-authentication"></a>アプリケーションに認証トークンを保存します。

[AZURE.INCLUDE [mobile-services-ios-authenticate-app-with-token](../../includes/mobile-services-ios-authenticate-app-with-token.md)]

## <a name="next-steps"></a>次のステップ

次のチュートリアルでは、[モバイル サービス ユーザーのサービス側承認] ではユーザー、ユーザー ID 値を返されたデータをフィルター処理します。




[register your app for authentication and configure mobile services]: #register 
[restrict table permissions to authenticated users]: #permissions 
[add authentication to the app]: #add-authentication 
[next steps]: #next-steps 
[storing authentication tokens in your app]: #store-authentication 
[service-side authorization of mobile services users]: mobile-services-dotnet-backend-service-side-authorization.md 
[mobile services quick start tutorial]: mobile-services-dotnet-backend-ios-get-started.md 
[get started with authentication]: mobile-services-dotnet-backend-ios-get-started-users.md 
[get started with push notifications]: mobile-services-dotnet-backend-ios-get-started-push.md 
[authorize users with scripts]: mobile-services-dotnet-backend-service-side-authorization.md 
[mobile services .net how-to conceptual reference]: /develop/mobile/how-to-guides/work-with-net-client-library 
[register your windows store app package for microsoft authentication]: ../mobile-services-how-to-register-store-app-package-microsoft-authentication.md 

