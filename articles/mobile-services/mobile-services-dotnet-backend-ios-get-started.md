<properties
    pageTitle="iOS アプリ用 Azure Mobile Services の使用"
    description="次のチュートリアルに従って、iOS 開発用の Azure Mobile Services を使用します。"
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
    ms.date="11/04/2015"
    ms.author="krisragh"/>


# <a name="getting-started"> </a>モバイル サービスを使ってみる

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;
>[AZURE.TIP] Microsoft Azure を使用したモバイル開発に慣れていない場合 [Azure モバイル アプリを使ってみる](app-service-mobile-dotnet-backend-ios-get-started-preview.md) Azure のモバイル サービスではなくMobile Apps は、 [もメリットが](app-service-mobile-value-prop-migration-from-mobile-services-preview.md)します。

このチュートリアルでは、Azure Mobile Services を使用して iOS アプリにクラウドベースのバックエンド サービスを追加する方法を示します。 このチュートリアルでは、新しいモバイル サービスと、新しいモバイル サービスにアプリ データを保存する簡単な _To do list_ アプリの両方を作成します。 Mobile Services はサーバー側ビジネス ロジックの .NET と Visual Studio を使用します。 JavaScript でサーバー側ビジネス ロジックをモバイル サービスを作成するには、[JavaScript バックエンド バージョン] のこのトピックを参照してください。
> [AZURE.NOTE] このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントを持っていない場合、Azure 評価版にサインアップして取得 [無料の評価終了後も使用することができますが、モバイル サービス](http://azure.microsoft.com/pricing/details/mobile-services/)します。 詳細については、「 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-ios-get-started%2F)します。

## <a name="create-new-service"> </a>新しいモバイル サービスの作成

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## ローカル コンピューターに、モバイル サービスとアプリケーションをダウンロードします。

モバイル サービスが作成されたので、ローカルで実行できるプロジェクトをダウンロードします。

1. 先ほど作成したモバイル サービスをクリックし、[クイック スタート] タブの [**プラットフォームの選択**] で [**iOS**] をクリックし、[**新しい iOS アプリを作成する**] を展開します。

2. Windows PC で、[**サービスのダウンロードとクラウドへの発行**] の下の [**ダウンロード**] をクリックします。 これにより、モバイル サービスを実装する Visual Studio プロジェクトがダウンロードされます。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

3. Mac で、[**アプリケーションをダウンロードして実行する**] の下の [**ダウンロード**] をクリックします。 これにより、Mobile Services に接続されている _To do list_ サンプル アプリケーションのプロジェクトが、Mobile Services iOS SDK と共にダウンロードされます。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

## モバイル サービスをテストする

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## モバイル サービスを発行する

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]


## 新しい iOS アプリを実行する

[AZURE.INCLUDE [mobile-services-ios-run-app](../../includes/mobile-services-ios-run-app.md)]


## <a name="next-steps"> </a>次のステップ

これで、Azure で実行されているモバイル サービスに対して新しいクライアント アプリケーションを実行する方法が示されます。 iOS アプリをローカル コンピューターで実行しているモバイル サービスでテストする前に、iOS 開発コンピューターからのアクセスを許可するように Web サーバーおよびファイアウォールを構成する必要があります。 詳細については、次を参照してください。 [ローカル モバイル サービスへの接続を許可するローカル web サーバーを構成する](mobile-services-dotnet-backend-how-to-configure-iis-express.md)します。

Mobile Services で重要になるこれ以外の作業については、以下のトピックをご覧ください。

* [オフライン データの同期の開始を取得]
  <br/>オフライン データの同期を使用して、応答性と信頼性にアプリを作成する方法について説明します。

* [既存のアプリケーションへの認証の追加]
  <br/>Id プロバイダーを使用してアプリケーションのユーザーを認証する方法について説明します。

* [既存のアプリにプッシュ通知の追加]
  <br/>アプリにごく基本的なプッシュ通知を送信する方法について説明します。

* [Mobile Services .NET バックエンドのトラブルシューティングを行う]
  <br/> Mobile Services .NET バックエンドで発生する問題診断して解決する方法について説明します。

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]





[getting started with mobile services]: #getting-started 
[create a new mobile service]: #create-new-service 
[define the mobile service instance]: #define-mobile-service-instance 
[next steps]: #next-steps 
[0]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-completed-ios.png 
[1]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-steps-vs.png 
[6]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-portal-quickstart-ios.png 
[7]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-steps-ios.png 
[8]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-xcode-project.png 
[10]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-quickstart-startup-ios.png 
[11]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-data-tab.png 
[12]: ./media/mobile-services-dotnet-backend-ios-get-started/mobile-data-browse.png 
[get started with offline data sync]: mobile-services-ios-get-started-offline-data.md 
[add authentication to an existing app]: mobile-services-dotnet-backend-ios-get-started-users.md 
[add push notifications to an existing app]: mobile-services-dotnet-backend-ios-get-started-push.md 
[troubleshoot mobile services .net backend]: mobile-services-dotnet-backend-how-to-troubleshoot.md 
[mobile services ios sdk]: https://go.microsoft.com/fwLink/p/?LinkID=266533 
[xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532 
[javascript backend version]: mobile-services-ios-get-started.md 

