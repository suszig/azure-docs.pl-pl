<properties
    pageTitle="認証の使用 (Android) | Microsoft Azure"
    description="Mobile Services を使用して、Google、Facebook、Twitter、Microsoft などのさまざまな ID プロバイダーを通じて Windows ストア アプリのユーザーを認証する方法について説明します。"
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

# Mobile Services Android アプリへの認証の追加

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

## 概要

このトピックでは、アプリケーションから Azure Mobile Services のユーザーを認証する方法について説明します。 このチュートリアルでは、Mobile Services でサポートされている ID プロバイダーを使用して、クイック スタート プロジェクトに認証を追加します。 Mobile Services によって正常に認証および承認されると、ユーザー ID 値が表示されます。

このチュートリアルでは、アプリケーションでの認証を有効にするための、次の基本的な手順について説明します。


## 前提条件

[AZURE.INCLUDE [mobile-services-android-prerequisites](../../includes/mobile-services-android-prerequisites.md)]

##<a name="register"></a>アプリケーションを認証に登録し、Mobile Services を構成する

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

[AZURE.INCLUDE [mobile-services-dotnet-backend-aad-server-extension](../../includes/mobile-services-dotnet-backend-aad-server-extension.md)]

##<a name="permissions"></a>アクセス許可を、認証されたユーザーだけに制限する

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)]

3. チュートリアルを完了したときに作成したプロジェクトを開く [を使ってみるモバイル サービス] です。

4.  **実行** ] メニューのをクリックし、 **アプリケーションを実行する** にアプリを起動します。 アプリケーションの開始後に、状態コード 401 (許可されていません) のハンドルされない例外が発生することを確認します。

     これは、問題は、アプリケーションは、認証されないユーザーとしてモバイル サービスにアクセスしようとしていますが、 _TodoItem_ テーブルには、今すぐ認証が必要です。

次に、モバイル サービスのリソースを要求する前にユーザーを認証するようにアプリケーションを更新します。

##<a name="add-authentication"></a>アプリケーションに認証を追加する

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>クライアントに認証トークンをキャッシュする

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

## <a name="refresh-tokens"></a>トークン キャッシュを更新する

[AZURE.INCLUDE [mobile-android-authenticate-app-refresh-token](../../includes/mobile-android-authenticate-app-refresh-token.md)]

##<a name="next-steps"></a>次のステップ

次のチュートリアルでは、[モバイル サービス ユーザーのサービス側の承認] で [スクリプトを持つユーザーを承認する] を認証されたユーザーに基づいてモバイル サービスによって提供されるユーザー ID 値を受け取りしてモバイル サービスから返されたデータをフィルター処理に使用します。


<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps

<!-- URLs. -->
[Get started with Mobile Services]: mobile-services-dotnet-backend-android-get-started.md
[Get started with authentication]: mobile-services-dotnet-backend-android-get-started-users.md
[Get started with push notifications]: mobile-services-dotnet-backend-android-get-started-push.md
[Authorize users with scripts]: ../mobile-services-dotnet-backend-android-authorize-users-in-scripts.md

[Mobile Services .NET How-to Conceptual Reference]: /develop/mobile/how-to-guides/work-with-net-client-library
[Register your Windows Store app package for Microsoft authentication]: ../mobile-services-how-to-register-store-app-package-microsoft-authentication.md

