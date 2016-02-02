<properties
    pageTitle="Android での認証の使用 (JavaScript バックエンド) | Microsoft Azure"
    description="モバイル サービスを使用して、さまざまな Google、Facebook、Twitter、および Microsoft (JavaScript バックエンド) を含む、id プロバイダーを通じて Android アプリのユーザーを認証する方法について説明します。"
    services="mobile-services"
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
    ms.date="10/05/2015"
    ms.author="ricksal"/>


# Mobile Services Android アプリへの認証の追加 (JavaScript バックエンド)

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

## 概要

このトピックでは、アプリケーションから Azure Mobile Services のユーザーを認証する方法について説明します。 このチュートリアルでは、Mobile Services でサポートされている ID プロバイダーを使用して、クイック スタート プロジェクトに認証を追加します。 Mobile Services によって正常に認証および承認されると、ユーザー ID 値が表示されます。

> [AZURE.VIDEO android-getting-started-with-authentication-in-windows-azure-mobile-services]

このチュートリアルでは、アプリケーションでの認証を有効にするための、次の基本的な手順について説明します。


## 前提条件

[AZURE.INCLUDE [mobile-services-android-prerequisites](../../includes/mobile-services-android-prerequisites.md)]

## アプリケーションを認証に登録し、Mobile Services を構成する

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

## アクセス許可を、認証されたユーザーだけに制限する

[AZURE.INCLUDE [mobile-services-restrict-permissions-javascript-backend](../../includes/mobile-services-restrict-permissions-javascript-backend.md)]

1. Android Studio で、チュートリアルを完了したときに作成したプロジェクトを開く [を使ってみるモバイル サービス] です。

2. **[Run]** メニューの **[Run app]** をクリックし、アプリケーションの開始後に、状態コード 401 (許可されていません) のハンドルされない例外が発生することを確認します。

     この問題は、認証されないユーザーとしてアプリケーションがモバイル サービスにアクセスしようとしても、_TodoItem_ テーブルでは認証が要求されるために発生します。

次に、モバイル サービスのリソースを要求する前にユーザーを認証するようにアプリケーションを更新します。

## アプリケーションに認証を追加する

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>クライアントで認証トークンをキャッシュします。

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="refresh-tokens"></a>トークンのキャッシュを更新します。

[AZURE.INCLUDE [mobile-android-authenticate-app-refresh-token](../../includes/mobile-android-authenticate-app-refresh-token.md)]



## <a name="next-steps"></a>次のステップ

次は、チュートリアルでは、[ユーザーの認証のスクリプト] で、ユーザー ID 値になりますが、認証されたユーザーに基づいてモバイル サービスによって提供され、モバイル サービスから返されたデータをフィルター処理に使用します。










[register your app for authentication and configure mobile services]: #register 
[restrict table permissions to authenticated users]: #permissions 
[add authentication to the app]: #add-authentication 
[store authentication tokens on the client]: #cache-tokens 
[refresh expired tokens]: #refresh-tokens 
[next steps]: #next-steps 
[4]: ./media/mobile-services-android-get-started-users/mobile-services-selection.png 
[5]: ./media/mobile-services-android-get-started-users/mobile-service-uri.png 
[13]: ./media/mobile-services-android-get-started-users/mobile-identity-tab.png 
[14]: ./media/mobile-services-android-get-started-users/mobile-portal-data-tables.png 
[15]: ./media/mobile-services-android-get-started-users/mobile-portal-change-table-perms.png 
[submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582 
[my applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039 
[live sdk for windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253 
[get started with mobile services]: mobile-services-android-get-started.md 
[authorize users with scripts]: mobile-services-javascript-backend-service-side-authorization.md 

