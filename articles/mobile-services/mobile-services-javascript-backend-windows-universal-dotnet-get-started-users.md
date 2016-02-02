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

このチュートリアルは、Mobile Services のクイック スタートに基づいています。 先にチュートリアル「Mobile Services の使用」を完了している必要があります。
>[AZURE.NOTE]このチュートリアルでは、Windows ストア アプリおよび Windows Phone Store 8.1 アプリでユーザーを認証する方法について説明します。 Windows Phone 8.0 または Windows Phone Silverlight 8.1 アプリでは、このバージョンを参照してください。 [モバイル サービスで認証を使ってみる](mobile-services-windows-phone-get-started-users.md)します。

## <a name="register"></a> アプリケーションを認証に登録し、モバイル サービスを構成します。

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

## <a name="permissions"></a> 認証されたユーザーへのアクセス許可を制限します。

[AZURE.INCLUDE [mobile-services-restrict-permissions-windows](../../includes/mobile-services-restrict-permissions-windows.md)]
>[AZURE.NOTE] Visual Studio ツールを使用してアプリをモバイル サービスに接続する場合、ツールはクライアント プラットフォームごとに 1 つずつ、**MobileServiceClient** 定義を合計 2 セット生成します。 この機会に、`#if...#endif` でラップされた **MobileServiceClient** 定義を、両方のバージョンのアプリで使用される、ラップされていない 1 つの定義に統合することによって、生成されたコードを単純化することができます。 [Azure クラシック ポータル] からクイック スタート アプリケーションをダウンロードした場合に実行する必要はありません。

## <a name="add-authentication"></a> アプリに認証を追加します。

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app](../../includes/mobile-windows-universal-dotnet-authenticate-app.md)]

これで、信頼できる ID プロバイダーによって認証されたユーザーは、*TodoItem* テーブルにアクセスできます。 ユーザー固有データのセキュリティを強化するには、承認を実装する必要もあります。 これを行うには、特定のユーザーのユーザー ID を取得します。この ID を使用することで、特定のリソースに対してユーザーが有する必要のあるアクセス権のレベルを特定できます。

## <a name="tokens"></a>クライアントに認証トークンを保存します。

前の例では、標準のサインインを示しました。標準のサインインでは、アプリケーションが開始するたびに、クライアントは ID プロバイダーとモバイル サービスの両方にアクセスする必要があります。 この方法は非効率であるだけなく、多くの顧客が同時にアプリケーションを開始すると、使用率に関連した問題が発生する場合があります。 よって、Mobile Services から返される承認トークンをキャッシュし、最初にその承認トークンの使用を試してから、プロバイダー ベースのサインインを使用するほうが効果的です。
>[AZURE.NOTE]クライアントによって管理される認証とサービスによって管理される認証のどちらを使用する場合でも、Mobile Services が発行したトークンをキャッシュできます。 このチュートリアルでは、サービスによって管理される認証を使用します。

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"> </a>次のステップ

次のチュートリアルでは、 [モバイル サービス ユーザーのサービス側承認](mobile-services-javascript-backend-service-side-authorization.md), 、認証されたユーザーに基づいてモバイル サービスによって提供されるユーザー ID 値を受け取り、モバイル サービスから返されたデータをフィルター処理に使用します。

## 関連項目

+ [強化されたユーザー機能](http://go.microsoft.com/fwlink/p/?LinkId=506605)<br/>
呼び出すことによって、モバイル サービス内で id プロバイダーによって管理する追加のユーザー データを取得できます、 **user.getIdentities()** サーバー スクリプトで機能します。

+ [モバイル サービス .NET の使用方法の概念リファレンス]<br/>モバイル サービス .NET クライアントを使用する方法について説明します。





[register your app for authentication and configure mobile services]: #register 
[restrict table permissions to authenticated users]: #permissions 
[add authentication to the app]: #add-authentication 
[store authentication tokens on the client]: #tokens 
[next steps]: #next-steps 
[submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582 
[my applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039 
[live sdk for windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253 
[get started with mobile services]: mobile-services-javascript-backend-windows-store-dotnet-get-started.md 
[get started with authentication]: ../mobile-services-javascript-backend-windows-store-dotnet-get-started-users.md 
[get started with push notifications]: ../mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md 
[authorize users with scripts]: ../mobile-services-windows-store-dotnet-authorize-users-in-scripts.md 
[azure classic portal]: https://manage.windowsazure.com/ 
[mobile services .net how-to conceptual reference]: mobile-services-windows-dotnet-how-to-use-client-library.md 
[register your windows store app package for microsoft authentication]: ../mobile-services-how-to-register-store-app-package-microsoft-authentication.md 

