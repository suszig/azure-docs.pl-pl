<properties
    pageTitle="Xamarin Android アプリケーション用 Mobile Services の使用 | Microsoft Azure"
    description="次のチュートリアルに従って、Xamarin Android 開発用の Azure Mobile Services を使用します。"
    services="mobile-services"
    documentationCenter="xamarin"
    authors="lindydonna"
    manager="dwrede"
    editor="mollybos"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="10/12/2015"
    ms.author="donnam"/>

# <a name="getting-started"> </a>Mobile Services の使用

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

>[AZURE.TIP] Microsoft Azure を使用したモバイル開発に慣れていない場合 [Azure モバイル アプリを使ってみる](app-service-mobile-dotnet-backend-xamarin-android-get-started-preview.md) Azure のモバイル サービスではなくMobile Apps は、 [もメリットが](app-service-mobile-value-prop-migration-from-mobile-services-preview.md)です。

このチュートリアルでは、Azure Mobile Services を使用して Xamarin Android アプリケーションにクラウドベースのバックエンド サービスを追加する方法を示します。 このチュートリアルでは、新しいモバイル サービスと、新しいモバイル サービスにアプリ データを保存する簡単な _To do list_ アプリの両方を作成します。 作成するモバイル サービスは、サーバー側ビジネス ロジックのために Visual Studio を使用してサポートされる .NET 言語を使用し、モバイル サービスを管理します。 JavaScript でサーバー側ビジネス ロジックを記述できるモバイル サービスを作成するには、参照してください、 [JavaScript backend version] このトピックのです。

>[AZURE.NOTE]このトピックでは、Azure クラシック ポータルを使用して、新しいモバイル サービス プロジェクトを作成する方法を示します。 Visual Studio 2013 更新プログラム 2 を使用して、新しいモバイル サービス プロジェクトを既存の Visual Studio ソリューションに追加することもできます。 詳細については、次を参照してください [クイック スタート: モバイル サービス (.NET バックエンド) を追加する。](http://msdn.microsoft.com/library/windows/apps/dn629482.aspx)

完成したアプリケーションのスクリーンショットは次のようになります。

![][0]

このチュートリアルを完了することは、Xamarin Android アプリケーションの他のすべての Mobile Services チュートリアルの前提条件です。

>[AZURE.NOTE]このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントがない場合は、Azure 試用版にサインアップして、最大 10 件の無料モバイル サービスを入手できます。このサービスは評価終了後も使用できます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-xamarin-android-get-started)をご覧ください。
>このチュートリアルでは [Visual Studio Professional 2013](https://go.microsoft.com/fwLink/p/?LinkID=257546)します。 無料試用版が利用できます。

## 新しいモバイル サービスを作成する

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## 新しい Xamarin Android アプリケーションを作成する

モバイル サービスを作成したら、Azure クラシック ポータルの簡単なクイック スタートに従って、新しいアプリケーションを作成するか、既存のアプリを変更して、モバイル サービスに接続することができます。

ここでは、モバイル サービス用の新しい Xamarin Android アプリケーションとサービス プロジェクトをダウンロードします。

1.  [Azure classic portal], 、] をクリックして **Mobile Services**, 、作成したモバイル サービスをクリックします。

2. クイック スタート] タブをクリックして **Xamarin** [ **プラットフォームの選択]** 展開 **新しい Xamarin アプリケーションを作成**します。

    ![][6]

    これにより、モバイル サービスに接続された Xamarin Android アプリケーションを作成するための簡単な 3 つの手順が表示されます。

    ![][7]

3. まだインストールしていない場合は、[Visual Studio Professional 2013](https://go.microsoft.com/fwLink/p/?LinkID=257546) をダウンロードし、ローカル コンピューターまたは仮想マシンにインストールします。

4. ダウンロードしてインストールをまだ行っていない場合、 [Xamarin Studio] または Xamarin for Visual Studio です。

5. [ **のダウンロードし、クラウドにサービスを発行**, [ **Android** ] をクリック **ダウンロード**します。

    ソリューションには、このダウンロードには、モバイルの両方のサービスと、サンプルのプロジェクトが含まれています。 _作業の一覧に_ 、モバイル サービスに接続されているアプリケーションです。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

6. 発行プロファイルをダウンロードしてローカル コンピューターに保存し、保存場所を書き留めておいてください。

## モバイル サービスをテストする

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service](../../includes/mobile-services-dotnet-backend-test-local-service.md)]

## モバイル サービスを発行する

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]

## Xamarin Android アプリケーションを実行する

このチュートリアルの最後に、新しいアプリケーションをビルドして実行します。

1. Visual Studio または Xamarin Studio で、モバイル サービス ソリューション内のクライアント プロジェクトに移動します。

2. キーを押して、 **実行** 、プロジェクトをビルドし、アプリケーションを開始] ボタンをクリックします。 エミュレーターまたは接続されている USB デバイスを選択するよう求めるメッセージが表示されます。

    > [AZURE.NOTE] プロジェクトを Android エミュレーターで実行できるようにするを少なくとも 1 つ Android Virtual Device (AVD) を定義する必要があります。 これらのデバイスを作成および管理するには、AVD Manager を使用します。

3. アプリケーションで、意味のあるテキストをなど入力 _に関するチュートリアルを完了_ プラス記号をクリックして (**+**) のアイコン。

    ![][10]

    これで、Azure でホストされている新しいモバイル サービスに POST 要求が送信されます。 要求のデータは TodoItem テーブルに挿入されます。 テーブルに格納された項目がモバイル サービスによって返され、データが一覧に表示されます。

    > [AZURE.NOTE]
    > モバイル サービスにアクセスして ToDoActivity.cs C# ファイルにあるデータをクエリおよび挿入するコードを確認できます。

## 次のステップ
クイック スタートはこれで完了です。Mobile Services で重要になるこれ以外の作業については、以下のトピックを参照してください。

* [Get started with offline data sync]
  <br/>クイック スタートでオフライン データの同期を使用して、アプリケーションの応答性と信頼性を高める方法について説明します。

* [Get started with authentication]
  <br/>ID プロバイダーを使用してアプリケーションのユーザーを認証する方法について説明します。

* [Get started with push notifications]
  <br/>アプリケーションにごく基本的なプッシュ通知を送信する方法について説明します。

* [Troubleshoot a Mobile Services .NET backend]
  <br/> Mobile Services の .NET バックエンドで発生する問題を診断して解決する方法について説明します。

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Getting started with Mobile Services]:#getting-started
[Create a new mobile service]:#create-new-service
[Next Steps]:#next-steps



<!-- Images. -->
[0]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-quickstart-completed-android.png
[6]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-portal-quickstart-xamarin.png
[7]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-quickstart-steps-xamarin-android.png
[8]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-xamarin-project-android-vs.png
[9]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-xamarin-project-android-xs.png
[10]: ./media/mobile-services-dotnet-backend-xamarin-android-get-started/mobile-quickstart-startup-android.png

<!-- URLs. -->
[Get started with offline data sync]: mobile-services-xamarin-android-get-started-offline-data.md
[Get started with authentication]: mobile-services-dotnet-backend-xamarin-android-get-started-users.md
[Get started with push notifications]: mobile-services-dotnet-backend-xamarin-android-get-started-push.md
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile Services SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[JavaScript and HTML]: mobile-services-win8-javascript/
[Azure classic portal]: https://manage.windowsazure.com/
[JavaScript backend version]: mobile-services-android-get-started.md
[Troubleshoot a Mobile Services .NET backend]: mobile-services-dotnet-backend-how-to-troubleshoot.md


[Xamarin Studio]: http://xamarin.com/download
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532&clcid=0x409
[Xamarin for Windows]: https://go.microsoft.com/fwLink/?LinkID=330242&clcid=0x409


