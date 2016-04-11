<properties 
    pageTitle="ユニバーサル Windows 8.1 アプリへの認証の追加 | Azure Mobile Services"
    description="Mobile Services を使用して、Google、Facebook、Twitter、Microsoft などのさまざまな ID プロバイダーを通じて Windows ストア アプリのユーザーを認証する方法について説明します。" 
    services="mobile-services" 
    documentationCenter="windows" 
    authors="ggailey777" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-windows-store" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="11/22/2015" 
    ms.author="glenga"/>

# ユニバーサル Windows 8.1 アプリへの認証の追加

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]      

このトピックでは、ユニバーサル Windows 8.1 アプリから Azure モバイル サービスのユーザーを認証する方法を示します。 このチュートリアルでは、モバイル サービスでサポートされている ID プロバイダーを使用して、クイック スタート プロジェクトに認証を追加します。 モバイル サービスによって正常に認証および承認されると、ユーザー ID 値が表示されます。

このチュートリアルは、モバイル サービスのクイック スタートに基づいています。 また、最初のチュートリアルを完了する必要があります [Get started with Mobile Services]します。 

>[AZURE.NOTE]このチュートリアルでは、Windows ストア アプリおよび Windows Phone ストア 8.1 アプリでユーザーを認証する方法を示します。 Windows Phone 8.0 または Windows Phone Silverlight 8.1 アプリでは、このバージョンを参照してください。 [モバイル サービスで認証を使ってみる](mobile-services-windows-phone-get-started-users.md)します。

##<a name="register"></a> アプリケーションを認証に登録し、Mobile Services を構成する

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)] 

##<a name="permissions"></a> アクセス許可を、認証されたユーザーだけに制限する

[AZURE.INCLUDE [mobile-services-restrict-permissions-windows](../../includes/mobile-services-restrict-permissions-windows.md)] 
 
>[AZURE.NOTE] Visual Studio のツールを使用して、アプリをモバイル サービスに接続する場合、ツールは、2 つのセットを生成 **MobileServiceClient** 定義、クライアント プラットフォームごとに 1 つずつです。 これは、統合することによって生成されたコードを簡略化する絶好のタイミング、 `#if...#endif` ラップ **MobileServiceClient** 両方のバージョンのアプリで使用される単一のラップされていない定義を定義します。 クイック スタート アプリをダウンロードした場合に実行する必要はありません、 [Azure classic portal]します。

##<a name="add-authentication"></a> アプリケーションに認証を追加する

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app](../../includes/mobile-windows-universal-dotnet-authenticate-app.md)] 

これで、信頼できる id プロバイダーによって認証されたユーザーがアクセスできる、 *TodoItem* テーブルです。 ユーザー固有データのセキュリティを強化するには、承認を実装する必要もあります。 これを行うには、特定のユーザーのユーザー ID を取得します。この ID を使用することで、特定のリソースに対してユーザーが有する必要のあるアクセス権のレベルを特定できます。

##<a name="tokens"></a>クライアント側で認証トークンを保存する

前の例では、標準のサインインを示しました。標準のサインインでは、アプリケーションが開始するたびに、クライアントは ID プロバイダーとモバイル サービスの両方にアクセスする必要があります。 この方法は非効率であるだけなく、多くの顧客が同時にアプリケーションを開始すると、使用率に関連した問題が発生する場合があります。 よって、Mobile Services から返される承認トークンをキャッシュし、最初にその承認トークンの使用を試してから、プロバイダー ベースのサインインを使用するほうが効果的です。 

>[AZURE.NOTE]クライアントによって管理される、またはサービスによって管理される認証を使用しているかどうかに関係なく、モバイル サービスによって発行されたトークンをキャッシュすることができます。 このチュートリアルでは、サービスによって管理される認証を使用します。

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)] 

## <a name="next-steps"> </a>次のステップ

次のチュートリアルでは、 [モバイル サービス ユーザーのサービス側承認](mobile-services-javascript-backend-service-side-authorization.md), 、認証されたユーザーに基づいてモバイル サービスによって提供されるユーザー ID 値を受け取り、モバイル サービスから返されたデータをフィルター処理に使用します。 

##関連項目

+ [強化されたユーザー機能](http://go.microsoft.com/fwlink/p/?LinkId=506605)<br/>
呼び出すことによって、モバイル サービス内で id プロバイダーによって管理する追加のユーザー データを取得できます、 **user.getIdentities()** サーバー スクリプトで機能します。 

+ [Mobile Services .NET How-to Conceptual Reference]<br/>モバイル サービス .NET クライアントを使用する方法について説明します。


<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: mobile-services-javascript-backend-windows-store-dotnet-get-started.md
[Get started with authentication]: ../mobile-services-javascript-backend-windows-store-dotnet-get-started-users.md
[Get started with push notifications]: ../mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md
[Authorize users with scripts]: ../mobile-services-windows-store-dotnet-authorize-users-in-scripts.md

[Azure classic portal]: https://manage.windowsazure.com/
[Mobile Services .NET How-to Conceptual Reference]: mobile-services-windows-dotnet-how-to-use-client-library.md
[Register your Windows Store app package for Microsoft authentication]: ../mobile-services-how-to-register-store-app-package-microsoft-authentication.md
 

