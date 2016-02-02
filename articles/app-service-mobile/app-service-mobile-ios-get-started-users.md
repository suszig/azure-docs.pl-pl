<properties
    pageTitle="Azure Mobile Apps を使用した iOS での認証の追加"
    description="Azure Mobile Apps を使用して、AAD、Google、Facebook、Twitter、Microsoft などのさまざまな ID プロバイダーを通じて iOS アプリのユーザーを認証する方法について説明します。"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="krisragh" 
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/30/2015"
    ms.author="krisragh"/>


# iOS アプリに認証を追加する

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

このチュートリアルでは、サポートされている id プロバイダーを使用して、[iOS のクイック スタート] プロジェクトに認証を追加します。 このチュートリアルは、先に完了する必要があります [iOS のクイック スタート] のチュートリアルに基づきます。

## <a name="register"></a>アプリケーションを認証に登録し、App Service を構成します。

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="permissions"></a>認証されたユーザーへのアクセス許可を制限します。

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Xcode で、キーを押して **実行** 、アプリケーションを開始します。 認証されないユーザーとしてアプリがバックエンドにアクセスしようとしても、_TodoItem_ テーブルで認証が要求されるために例外が発生します。

## <a name="add-authentication"></a>アプリに認証を追加します。

[AZURE.INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]





[ios quick start]: app-service-mobile-ios-get-started.md 
[azure portal]: https://portal.azure.com 

