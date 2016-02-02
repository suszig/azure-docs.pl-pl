<properties
    pageTitle="Xamarin iOS アプリケーション用 Mobile Services の使用 | Microsoft Azure"
    description="次のチュートリアルに従って、Xamarin iOS 開発用の Azure Mobile Services を使用します。"
    services="mobile-services"
    documentationCenter="xamarin"
    authors="lindydonna"
    manager="dwrede"
    editor="mollybos"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="11/05/2015"
    ms.author="donnam"/>


# <a name="getting-started"> </a>モバイル サービスを使ってみる

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;
>[AZURE.TIP] Microsoft Azure を使用したモバイル開発に慣れていない場合 [Azure モバイル アプリを使ってみる](app-service-mobile-dotnet-backend-xamarin-ios-get-started-preview.md) Azure のモバイル サービスではなくMobile Apps は、 [もメリットが](app-service-mobile-value-prop-migration-from-mobile-services-preview.md)します。

このチュートリアルでは、Azure Mobile Services を使用して Xamarin iOS アプリケーションにクラウドベースのバックエンド サービスを追加する方法を示します。 このチュートリアルでは、新しいモバイル サービスと、新しいモバイル サービスにアプリ データを保存する簡単な _To do list_ アプリの両方を作成します。 作成するモバイル サービスは、サーバー側ビジネス ロジックのために Visual Studio を使用してサポートされる .NET 言語を使用し、モバイル サービスを管理します。 JavaScript でサーバー側ビジネス ロジックを記述できるモバイル サービスを作成するには、[JavaScript バックエンド バージョン] のこのトピックを参照してください。
>[AZURE.NOTE]このトピックでは、Azure クラシック ポータルを使用して、新しいモバイル サービス プロジェクトを作成する方法について説明します。 Visual Studio 2013 更新プログラム 2 を使用して、新しいモバイル サービス プロジェクトを既存の Visual Studio ソリューションに追加することもできます。 詳細については、を参照してください [クイック スタート: モバイル サービス (.NET バックエンド) を追加する](http://msdn.microsoft.com/library/windows/apps/dn629482.aspx)。

完成したアプリケーションのスクリーンショットは次のようになります。

![][0]


このチュートリアルを完了することは、Xamarin iOS アプリケーションの他のすべての Mobile Services チュートリアルの前提条件です。
>[AZURE.NOTE]このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントがない場合は、Azure 試用版にサインアップして最大 10 の無料モバイル サービスを取得し、試用期間が終わった後でも使用し続けることができます。 詳細については、「 <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-xamarin-ios-get-started" target="_blank">Azure 無料試用版</a>.<br />このチュートリアルでは <a href="https://go.microsoft.com/fwLink/p/?LinkID=257546" target="_blank">Visual Studio Professional 2013</a>します。 無料試用版が利用できます。

## 新しいモバイル サービスを作成する

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## 新しい Xamarin iOS アプリケーションを作成する

モバイル サービスを作成したら、Azure クラシック ポータルの簡単なクイック スタートに従って、新しいアプリケーションを作成するか、既存のアプリを変更して、モバイル サービスに接続することができます。

ここでは、モバイル サービス用の新しい Xamarin iOS アプリケーションとサービス プロジェクトをダウンロードします。

1. クリックして、[Azure クラシック ポータル] で、 **Mobile Services**, 、し、先ほど作成したモバイル サービスをクリックします。

2. [クイック スタート] タブの **[プラットフォームの選択]** で **[Xamarin]** を選択し、**[新しい Xamarin アプリケーションを作成する]** を展開します。

    ![][6]

    これにより、モバイル サービスに接続された Xamarin iOS アプリケーションを作成するための簡単な 3 つの手順が表示されます。

    ![][7]

3. ダウンロードしてインストールをまだ行っていない場合、 <a href="https://go.microsoft.com/fwLink/p/?LinkID=257546" target="_blank">Visual Studio Professional 2013</a> 、ローカル コンピューターまたは仮想マシン。

4. ダウンロードし、[Xcode] v4.4 以降のバージョンおよび [Xamarin Studio] をインストールします。 また、Xamarin for Visual Studio を使用することもできます。

5. **[サービスのダウンロードとクラウドへの発行]** で **[iOS]** を選択し、**[ダウンロード]** をクリックします。

    これにより、Mobile Services と Mobile Services に接続されている _To do list_ サンプル アプリケーションの両方のプロジェクトを含むソリューションがダウンロードされます。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

6. 発行プロファイルをダウンロードしてローカル コンピューターに保存し、保存場所を書き留めておいてください。

## モバイル サービスをテストする

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## モバイル サービスを発行する

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

## Xamarin iOS アプリケーションを実行する

このチュートリアルの最後に、新しいアプリケーションをビルドして実行します。

1. Visual Studio または Xamarin Studio で、モバイル サービス ソリューション内のクライアント プロジェクトに移動します。

    ![][8]

    ![][9]

2. **[実行]** ボタンを押してクライアント プロジェクトをビルドし、iPhone エミュレーターでアプリケーションを開始します。

3. アプリケーションで、たとえば、tutorial_ の意味のあるテキストを入力し、正符号 (**+**) のアイコン。

    ![][10]

    これで、Azure でホストされている新しいモバイル サービスに POST 要求が送信されます。 要求のデータは TodoItem テーブルに挿入されます。 テーブルに格納された項目がモバイル サービスによって返され、データが一覧に表示されます。

>[AZURE.NOTE]モバイル サービスにアクセスして QSTodoService.cs C# ファイルにあるデータをクエリおよび挿入するコードを確認できます。


## 次のステップ

クイック スタートはこれで完了です。Mobile Services で重要になるこれ以外の作業については、以下のトピックを参照してください。

* [オフライン データの同期の開始を取得]
  <br/>クイック スタートでオフライン データの同期を使用して、応答性と信頼性の高いアプリケーションを作成する方法について説明します。

* [認証の使用の開始を取得]
  <br/>Id プロバイダーを使用してアプリケーションのユーザーを認証する方法について説明します。

* [開始プッシュ通知の使用]
  <br/>アプリにごく基本的なプッシュ通知を送信する方法について説明します。

* [Mobile Services .NET バックエンドのトラブルシューティングを行う]
  <br/> Mobile Services .NET バックエンドで発生する問題診断して解決する方法について説明します。

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]





[getting started with mobile services]: #getting-started 
[create a new mobile service]: #create-new-service 
[next steps]: #next-steps 
[0]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-quickstart-completed-ios.png 
[6]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-portal-quickstart-xamarin-ios.png 
[7]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-quickstart-steps-xamarin-ios.png 
[8]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-xamarin-project-ios-vs.png 
[9]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-xamarin-project-ios-xs.png 
[10]: ./media/mobile-services-dotnet-backend-xamarin-ios-get-started/mobile-quickstart-startup-ios.png 
[get started with offline data sync]: mobile-services-xamarin-ios-get-started-offline-data.md 
[get started with authentication]: mobile-services-dotnet-backend-xamarin-ios-get-started-users.md 
[get started with push notifications]: mobile-services-dotnet-backend-xamarin-ios-get-started-push.md 
[visual studio professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546 
[mobile services sdk]: http://go.microsoft.com/fwlink/?LinkId=257545 
[javascript and html]: mobile-services-win8-javascript/ 
[azure classic portal]: https://manage.windowsazure.com/ 
[javascript backend version]: mobile-services-ios-get-started.md 
[troubleshoot a mobile services .net backend]: mobile-services-dotnet-backend-how-to-troubleshoot.md 
[xamarin studio]: http://xamarin.com/download 
[xcode]: https://go.microsoft.com/fwLink/?LinkID=266532&clcid=0x409 
[xamarin for windows]: https://go.microsoft.com/fwLink/?LinkID=330242&clcid=0x409 

