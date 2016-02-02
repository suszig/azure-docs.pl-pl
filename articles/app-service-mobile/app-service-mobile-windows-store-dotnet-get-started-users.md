<properties
    pageTitle="Windows Runtime 8.1 ユニバーサル アプリへの認証の追加 | Azure Mobile Apps"
    description="Azure App Service Mobile Apps を使用して、AAD、Google、Facebook、Twitter、Microsoft などのさまざまな ID プロバイダーを使って Windows アプリのユーザーを認証する方法を説明します。"
    services="app-service\mobile"
    documentationCenter="windows"
    authors="mattchenderson" 
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/23/2015"
    ms.author="glenga"/>


# Windows アプリに認証を追加する

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]
&nbsp;  
[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

このトピックでは、モバイル アプリにクラウド ベースの認証を追加する方法を説明します。 このチュートリアルでは、Azure App Service でサポートされている ID プロバイダーを使用して、Mobile Apps のクイック スタート プロジェクトに認証を追加します。 Mobile Apps のバックエンドによって正常に認証され、承認されると、ユーザー ID 値が表示されます。

このチュートリアルは、Mobile Apps のクイック スタートに基づいています。 最初のチュートリアルを完了する必要があります [モバイル アプリを使ってみる](app-service-mobile-windows-store-dotnet-get-started.md)します。

## <a name="register"></a>アプリケーションを認証に登録し、App Service を構成します。

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="permissions"></a>認証されたユーザーへのアクセス許可を制限します。

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

スタートアップ プロジェクトとして設定された Windows アプリ プロジェクトの 1 つを使用して、F5 キーを押してアプリを実行します。アプリケーションの開始後、状態コード 401 (許可されていません) のハンドルされない例外が発生することを確認します。 この問題は、認証されていないユーザーとしてアプリケーションがモバイル アプリ コードにアクセスしようとしても、現在の *TodoItem* テーブルでは認証が要求されるために発生します。

次に、App Service のリソースを要求する前にユーザーを認証するようにアプリケーションを更新します。

## <a name="add-authentication"></a>アプリに認証を追加します。

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app](../../includes/mobile-windows-universal-dotnet-authenticate-app.md)]


## <a name="tokens"></a>クライアントに認証トークンを保存します。

前の例では、標準のサインインを示しました。標準のサインインでは、アプリケーションが開始するたびに、クライアントは ID プロバイダーと App Service の両方にアクセスする必要があります。 この方法は非効率であるだけでなく、多くの顧客が同時にアプリケーションを開始すると、使用率に関連した問題が発生する場合があります。 よって、App Service から返される承認トークンをキャッシュし、最初にその承認トークンの使用を試してから、プロバイダー ベースのサインインを使用するほうが効果的です。
>[AZURE.NOTE]クライアントによって管理される認証とサービスによって管理される認証のどちらを使用する場合でも、App Services が発行したトークンをキャッシュできます。 このチュートリアルでは、サービスによって管理される認証を使用します。

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## 次のステップ

これで基本的な認証チュートリアルは完了しましたので、引き続き次のいずれかのチュートリアルのご利用を検討してください。

+ [Windows アプリにプッシュ通知を追加します。](app-service-mobile-windows-store-dotnet-get-started-push.md)   
  追加のプッシュ通知がアプリへのサポートし、プッシュ通知を送信する Azure Notification Hubs を使用して、Mobile App バックエンドを構成する方法について説明します。

+ [Windows アプリのオフライン同期を有効にします。](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  モバイル アプリ バックエンドを使用して、アプリのオフライン サポートを追加する方法について説明します。 オフライン同期を使用すると、エンド ユーザーはネットワークにアクセスできなくても、データの表示、追加、変更など、モバイル アプリとやり取りできます。




[get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md 

