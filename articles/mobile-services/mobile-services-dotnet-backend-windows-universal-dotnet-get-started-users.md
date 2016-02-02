<properties
    pageTitle="ユニバーサル Windows 8.1 アプリへの認証の追加 | Microsoft Azure"
    description="Mobile Services を使用して、Google、Facebook、Twitter、および Microsoft などのさまざまな ID プロバイダーを通じてユニバーサル Windows 8.1 アプリのユーザーを認証する方法について説明します。"
    services="mobile-services"
    documentationCenter="windows"
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/23/2015"
    ms.author="glenga"/>


# Mobile Services アプリへの認証の追加

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;

[AZURE.INCLUDE [mobile-services-selector-get-started-users](../../includes/mobile-services-selector-get-started-users.md)]

## 概要

このトピックでは、ユニバーサル Windows アプリから Azure Mobile Services のユーザーを認証する方法を示します。 このチュートリアルでは、Mobile Services でサポートされている ID プロバイダーを使用して、クイック スタート プロジェクトに認証を追加します。 モバイル サービスによって正常に認証および承認されると、ユーザー ID 値が表示されます。

このチュートリアルは、Mobile Services のクイック スタートに基づいています。 また、最初のチュートリアルを完了する必要があります [を使ってみるモバイル サービス]、または [既存のアプリケーションへの Mobile Services の追加](mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md)します。
>[AZURE.NOTE]このチュートリアルでは、Windows ストア アプリおよび Windows Phone ストア 8.1 アプリで、ユーザー主導の認証によってユーザー認証する方法について説明します。 詳細について、クライアント主導型認証は、次を参照してください。 [Google、Microsoft、Facebook の Sdk で Azure Mobile Services を使用してログイン](http://azure.microsoft.com/blog/2014/10/27/logging-in-with-google-microsoft-and-facebook-sdks-to-azure-mobile-services/)します。

## <a name="register"></a>アプリケーションを認証に登録し、モバイル サービスを構成します。

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

[AZURE.INCLUDE [mobile-services-dotnet-backend-aad-server-extension](../../includes/mobile-services-dotnet-backend-aad-server-extension.md)]

## <a name="permissions"></a>認証されたユーザーへのアクセス許可を制限します。

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;6. Visual Studio で、TodoList アプリの Windows ストア プロジェクトを右クリックして、**[スタートアップ プロジェクトに設定]** をクリックします。

&nbsp;&nbsp;7. 共有プロジェクトで、App.xaml.cs プロジェクト ファイルを開き、定義を見つけ、 [MobileServiceClient](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx), 、Azure で実行されているモバイル サービスに接続するように構成されているかどうかを確認します。
>[AZURE.NOTE]Visual Studio ツールを使用してアプリをモバイル サービスに接続する場合、ツールはクライアント プラットフォームごとに 1 つずつ、**MobileServiceClient** 定義を合計 2 セット生成します。 この機会に、`#if...#endif` でラップされた **MobileServiceClient** 定義を、両方のバージョンのアプリで使用される、ラップされていない 1 つの定義に統合することによって、生成されたコードを単純化することができます。 [Azure クラシック ポータル] からクイック スタート アプリケーションをダウンロードしたときに実行する必要はありません。

&nbsp;&nbsp;8. F5 キーを押して、Windows ストア アプリを実行します。アプリケーションの開始後に、状態コード 401 (許可されていません) のハンドルされない例外が発生することを確認します。

&nbsp;&nbsp;この問題が発生するのは、認証されないユーザーとしてアプリケーションが Mobile Services にアクセスしようとすると、*TodoItem* テーブルで認証が要求されるようになったことが原因です。

次に、モバイル サービスのリソースを要求する前にユーザーを認証するようにアプリケーションを更新します。

## <a name="add-authentication"></a>アプリに認証を追加します。

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app](../../includes/mobile-windows-universal-dotnet-authenticate-app.md)]
>[AZURE.NOTE]モバイル サービスの Windows ストア アプリ パッケージ情報が登録されている場合を呼び出す必要があります、 <a href="http://go.microsoft.com/fwlink/p/?LinkId=311594" target="_blank">LoginAsync</a> メソッドの値を指定して **true** の *useSingleSignOn* パラメーター。 この操作を行わない場合、login メソッドが呼び出されるたびに、引き続きユーザーにログイン プロンプトが表示されます。

## <a name="tokens"></a>クライアントに認証トークンを保存します。

前の例では、標準のサインインを示しました。標準のサインインでは、アプリケーションが開始するたびに、クライアントは ID プロバイダーとモバイル サービスの両方にアクセスする必要があります。 この方法は非効率であるだけなく、多くの顧客が同時にアプリケーションを開始すると、使用率に関連した問題が発生する場合があります。 よって、Mobile Services から返される承認トークンをキャッシュし、最初にその承認トークンの使用を試してから、プロバイダー ベースのサインインを使用するほうが効果的です。
>[AZURE.NOTE]クライアントによって管理される認証とサービスによって管理される認証のどちらを使用する場合でも、Mobile Services が発行したトークンをキャッシュできます。 このチュートリアルでは、サービスによって管理される認証を使用します。

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]


## <a name="next-steps"> </a>次のステップ

次のチュートリアルでは、 [[スクリプトを使用したユーザーの承認] モバイル サービス ユーザーのサービス側承認][authorize users with scripts], 、認証されたユーザーに基づいてモバイル サービスによって提供されるユーザー ID 値を受け取り、モバイル サービスから返されたデータをフィルター処理に使用します。

## 関連項目

+ [強化されたユーザー機能](http://azure.microsoft.com/blog/2014/10/02/custom-login-scopes-single-sign-on-new-asp-net-web-api-updates-to-the-azure-mobile-services-net-backend/)<br/>
呼び出すことによって、モバイル サービス内で id プロバイダーによって管理する追加のユーザー データを取得できます、 **ServiceUser.GetIdentitiesAsync()** .NET バックエンドでのメソッドです。

+ [モバイル サービス .NET の使用方法の概念リファレンス]<br/>モバイル サービス .NET クライアントを使用する方法について説明します。





[register your app for authentication and configure mobile services]: #register 
[restrict table permissions to authenticated users]: #permissions 
[add authentication to the app]: #add-authentication 
[store authentication tokens on the client]: #tokens 
[next steps]: #next-steps 
[submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582 
[my applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039 
[live sdk for windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253 
[get started with mobile services]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md 
[get started with data]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md 
[get started with authentication]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-users.md 
[get started with push notifications]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-push.md 
[authorize users with scripts]: ../mobile-services-dotnet-backend-windows-store-dotnet-authorize-users-in-scripts.md 
[javascript and html]: ../mobile-services-dotnet-backend-windows-store-javascript-get-started-users.md 
[azure classic portal]: https://manage.windowsazure.com/ 
[mobile services .net how-to conceptual reference]: mobile-services-windows-dotnet-how-to-use-client-library.md 
[register your windows store app package for microsoft authentication]: ../mobile-services-how-to-register-store-app-package-microsoft-authentication.md 

