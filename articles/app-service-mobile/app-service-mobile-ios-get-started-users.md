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

このチュートリアルでは、認証を追加、 [iOS quick start] プロジェクトのサポートされている id プロバイダーを使用します。 このチュートリアルに基づいて、 [iOS quick start] チュートリアルで、先に完了する必要があります。 

##<a name="register"></a>アプリを認証に登録し、App Service を構成する

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>アクセス許可を、認証されたユーザーだけに制限する

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Xcode で、キーを押して **実行** 、アプリケーションを開始します。 アプリケーションが、認証されないユーザーとしてバックエンドにアクセスしようとしているため、例外が発生するが、 _TodoItem_ テーブルには、今すぐ認証が必要です。

##<a name="add-authentication"></a>アプリケーションに認証を追加する

[AZURE.INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]


<!-- URLs. -->

[iOS quick start]: app-service-mobile-ios-get-started.md

[Azure portal]: https://portal.azure.com
 

