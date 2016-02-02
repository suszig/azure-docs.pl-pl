<properties
    pageTitle="iOS アプリ用 Azure Mobile Services の使用 | JavaScript バックエンド"
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
    ms.topic="hero-article"
    ms.date="11/04/2015"
    ms.author="krisragh"/>


# <a name="getting-started"> </a>モバイル サービスを使ってみる

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]


このチュートリアルでは、Azure Mobile Services を使用して iOS アプリにクラウドベースのバックエンド サービスを追加する方法を示します。

このチュートリアルでは、新しいモバイル サービスと、新しいモバイル サービスにアプリ データを保存する簡単な _To do list_ アプリの両方を作成します。 作成するモバイル サービスは、サーバー側ビジネス ロジックのために JavaScript を使用します。 サーバー側ビジネス ロジックを .NET でモバイル サービスを作成するには、[.NET バックエンド バージョン] のこのトピックを参照してください。
> [AZURE.NOTE] このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントを持っていない場合、Azure 評価版にサインアップして取得 [無料の評価終了後も使用することができますが、モバイル サービス](http://azure.microsoft.com/pricing/details/mobile-services/)します。 詳細については、「 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AE564AB28&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started-ios%2F%20)します。

## <a name="create-new-service"> </a>新しいモバイル サービスの作成

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## 新しい iOS アプリを作成する

Azure クラシック ポータルで簡単なクイック スタートに従い、モバイル サービスに接続された新しいアプリを作成できます。

1. クリックして、[Azure クラシック ポータル] で、 **Mobile Services**, 、し、先ほど作成したモバイル サービスをクリックします。

2. [クイック スタート] タブの [**プラットフォームの選択**] で [**iOS**] を選択し、[**新しい iOS アプリを作成する**] を展開します。 これにより、モバイル サービスに接続された iOS アプリを作成するための手順が表示されます。

3. **[TodoItem テーブルを作成する]** をクリックして、アプリケーション データを格納するテーブルを作成します。

4. **[アプリケーションをダウンロードして実行する]** の下の **[ダウンロード]** をクリックします。 これにより、Mobile Services に接続されている _To do list_ サンプル アプリケーションのプロジェクトが、Mobile Services iOS SDK と共にダウンロードされます。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

## 新しい iOS アプリを実行する

[AZURE.INCLUDE [mobile-services-ios-run-app](../../includes/mobile-services-ios-run-app.md)]

<ol start="4">
<li><p>ポータルに戻り、[Azure クラシック] をクリックして、 **データ** ] タブでをクリックし、 **TodoItem** テーブルです。これにより、アプリケーションによってテーブルに挿入されたデータを参照できます。<p></li></ol></p>

## <a name="next-steps"> </a>次のステップ

Mobile Services で重要になるこれ以外の作業については、以下のトピックをご覧ください。

* [オフライン データの同期の開始を取得]
    <br/>オフライン データの同期を使用して、応答性と信頼性にアプリを作成する方法について説明します。

* [既存のアプリケーションへの認証の追加]
    <br/>Id プロバイダーを使用してアプリケーションのユーザーを認証する方法について説明します。

* [既存のアプリにプッシュ通知の追加]
    <br/>アプリにごく基本的なプッシュ通知を送信する方法について説明します。

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]






[getting started with mobile services]: #getting-started 
[create a new mobile service]: #create-new-service 
[define the mobile service instance]: #define-mobile-service-instance 
[next steps]: #next-steps 
[6]: ./media/mobile-services-ios-get-started/mobile-portal-quickstart-ios.png 
[7]: ./media/mobile-services-ios-get-started/mobile-quickstart-steps-ios.png 
[8]: ./media/mobile-services-ios-get-started/mobile-xcode-project.png 
[10]: ./media/mobile-services-ios-get-started/mobile-quickstart-startup-ios.png 
[11]: ./media/mobile-services-ios-get-started/mobile-data-tab.png 
[12]: ./media/mobile-services-ios-get-started/mobile-data-browse.png 
[get started with offline data sync]: mobile-services-ios-get-started-offline-data.md 
[add authentication to an existing app]: mobile-services-dotnet-backend-ios-get-started-users.md 
[add push notifications to an existing app]: mobile-services-dotnet-backend-ios-get-started-push.md 
[mobile services ios sdk]: https://go.microsoft.com/fwLink/p/?LinkID=266533 
[azure classic portal]: https://manage.windowsazure.com/ 
[xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532 
[.net backend version]: mobile-services-dotnet-backend-ios-get-started.md 

