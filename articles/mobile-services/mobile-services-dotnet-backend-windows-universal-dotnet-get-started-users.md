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

>[AZURE.NOTE]このチュートリアルでは、Windows ストアと Windows Phone ストア 8.1 アプリでユーザーに対してサーバー主導型認証を使用する方法を示します。 詳細について、クライアント主導型認証は、次を参照してください。 [Google、Microsoft、Facebook の Sdk で Azure Mobile Services を使用してログイン](http://azure.microsoft.com/blog/2014/10/27/logging-in-with-google-microsoft-and-facebook-sdks-to-azure-mobile-services/)します。

##<a name="register"></a>アプリケーションを認証に登録し、Mobile Services を構成する

[AZURE.INCLUDE [mobile-services-register-authentication](../../includes/mobile-services-register-authentication.md)]

[AZURE.INCLUDE [mobile-services-dotnet-backend-aad-server-extension](../../includes/mobile-services-dotnet-backend-aad-server-extension.md)]

##<a name="permissions"></a>アクセス許可を、認証されたユーザーだけに制限する

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;6. Visual Studio で、TodoList アプリの Windows ストア プロジェクトを右クリックし、をクリックして **スタートアップ プロジェクトとして設定**します。

&nbsp;&nbsp;7. 共有プロジェクトで、App.xaml.cs プロジェクト ファイルを開き、定義を見つけ、 [MobileServiceClient](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx), 、Azure で実行されているモバイル サービスに接続するように構成されているかどうかを確認します。

>[AZURE.NOTE]Visual Studio のツールを使用して、アプリをモバイル サービスに接続する場合、ツールは、2 つのセットを生成 **MobileServiceClient** 定義、クライアント プラットフォームごとに 1 つずつです。 これは、統合することによって生成されたコードを簡略化する絶好のタイミング、 `#if...#endif` ラップ **MobileServiceClient** 定義をアプリの両方のバージョンで使用される 1 つのラップされていない定義にします。 クイック スタート アプリをダウンロードしたときに実行する必要はありません、 [Azure classic portal]します。

&nbsp;&nbsp;8. F5 キーを押して、Windows ストア アプリを実行します。アプリケーションの開始後に、状態コード 401 (許可されていません) のハンドルされない例外が発生することを確認します。

& nbsp; & nbsp;これは、問題は、アプリケーションは、認証されないユーザーとしてモバイル サービスにアクセスしようとしていますが、 *TodoItem* テーブルには、今すぐ認証が必要です。

次に、モバイル サービスのリソースを要求する前にユーザーを認証するようにアプリケーションを更新します。

##<a name="add-authentication"></a>アプリケーションに認証を追加する

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app](../../includes/mobile-windows-universal-dotnet-authenticate-app.md)]

>[AZURE.NOTE]モバイル サービスの Windows ストア アプリ パッケージ情報が登録されている場合を呼び出す必要があります、 <a href="http://go.microsoft.com/fwlink/p/?LinkId=311594" target="_blank">LoginAsync</a> メソッドの値を指定して **true** の *useSingleSignOn* パラメーター。 この操作を行わない場合、login メソッドが呼び出されるたびに、引き続きユーザーにログイン プロンプトが表示されます。

##<a name="tokens"></a>クライアント側で認証トークンを保存する

前の例では、標準のサインインを示しました。標準のサインインでは、アプリケーションが開始するたびに、クライアントは ID プロバイダーとモバイル サービスの両方にアクセスする必要があります。 この方法は非効率であるだけなく、多くの顧客が同時にアプリケーションを開始すると、使用率に関連した問題が発生する場合があります。 よって、Mobile Services から返される承認トークンをキャッシュし、最初にその承認トークンの使用を試してから、プロバイダー ベースのサインインを使用するほうが効果的です。

>[AZURE.NOTE]クライアントによって管理される、またはサービスによって管理される認証を使用しているかどうかに関係なく、モバイル サービスによって発行されたトークンをキャッシュすることができます。 このチュートリアルでは、サービスによって管理される認証を使用します。

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]


## <a name="next-steps"> </a>次のステップ

次のチュートリアルでは、[モバイル サービス ユーザーのサービス側の承認] で [スクリプトを持つユーザーを承認する] を認証されたユーザーに基づいてモバイル サービスによって提供されるユーザー ID 値を受け取りしてモバイル サービスから返されたデータをフィルター処理に使用します。

##関連項目

+ [強化されたユーザー機能](http://azure.microsoft.com/blog/2014/10/02/custom-login-scopes-single-sign-on-new-asp-net-web-api-updates-to-the-azure-mobile-services-net-backend/)<br/>
呼び出すことによって、モバイル サービス内で id プロバイダーによって管理する追加のユーザー データを取得できます、 **ServiceUser.GetIdentitiesAsync()** .NET バックエンドでのメソッドです。

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
[Get started with Mobile Services]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
[Get started with data]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md
[Get started with authentication]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-users.md
[Get started with push notifications]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-push.md
[Authorize users with scripts]: ../mobile-services-dotnet-backend-windows-store-dotnet-authorize-users-in-scripts.md
[JavaScript and HTML]: ../mobile-services-dotnet-backend-windows-store-javascript-get-started-users.md

[Azure classic portal]: https://manage.windowsazure.com/
[Mobile Services .NET How-to Conceptual Reference]: mobile-services-windows-dotnet-how-to-use-client-library.md
[Register your Windows Store app package for Microsoft authentication]: ../mobile-services-how-to-register-store-app-package-microsoft-authentication.md


