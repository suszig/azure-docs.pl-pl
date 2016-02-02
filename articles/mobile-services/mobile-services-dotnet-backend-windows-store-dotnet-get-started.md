<properties
    pageTitle="Windows ユニバーサル アプリ向けの Mobile Services の使用 | Microsoft Azure"
    description="このチュートリアルでは、ユニバーサル Windows を C# で開発するための Azure Mobile Services の使用方法を示します。"
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
    ms.topic="hero-article"
    ms.date="11/06/2015"
    ms.author="glenga"/>



# <a name="getting-started"> </a>モバイル サービスを使ってみる

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;
>[AZURE.TIP] Microsoft Azure を使用したモバイル開発に慣れていない場合 [Azure モバイル アプリを使ってみる](app-service-mobile-dotnet-backend-windows-store-dotnet-get-started-preview.md) Azure のモバイル サービスではなくMobile Apps は、 [もメリットが](app-service-mobile-value-prop-migration-from-mobile-services-preview.md)します。

このチュートリアルでは、Azure Mobile Services を使用してユニバーサル Windows アプリにクラウドベースのバックエンド サービスを追加する方法を示します。 ユニバーサル Windows アプリ ソリューションには、Windows ストア 8.1 と Windows Phone ストア 8.1 の両方のアプリのプロジェクトと、共通の共有プロジェクトが含まれます。 詳細については、次を参照してください。 [を Windows および Windows Phone を対象とするユニバーサル Windows アプリの構築](http://msdn.microsoft.com/library/windows/apps/xaml/dn609832.aspx)します。

このチュートリアルでは、新しいモバイル サービスと、新しいモバイル サービスにアプリケーション データを保存する簡単な *To do list* アプリケーションの両方を作成します。 作成するモバイル サービスは、サーバー側ビジネス ロジックのために Visual Studio を使用してサポートされる .NET 言語を使用し、モバイル サービスを管理します。 JavaScript でサーバー側ビジネス ロジックを記述できるモバイル サービスを作成する方法については、このトピックの「JavaScript バックエンド バージョン」を参照してください。
>[AZURE.NOTE]このトピックでは、Azure クラシック ポータルを使用して、新しいモバイル サービス プロジェクトとユニバーサル Windows アプリを作成する方法を示します。 Visual Studio 2013 更新プログラム 3 を使用して、新しいモバイル サービス プロジェクトを既存の Visual Studio ソリューションに追加することもできます。 詳細については、次を参照してください。 [既存のアプリケーションへの Mobile Services の追加](mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md)します。

>モバイル サービスを Windows Phone 8.0 または Windows Phone Silverlight 8.1 アプリ プロジェクトに追加するを参照してください。 [既存の Windows Phone アプリケーションへの Mobile Services の追加](mobile-services-dotnet-backend-windows-phone-get-started-data.md)します。

[AZURE.INCLUDE [mobile-services-windows-universal-get-started](../../includes/mobile-services-windows-universal-get-started.md)]

このチュートリアルを完了するには、以下が必要です。

* アクティブな Azure アカウント。 アカウントがない場合は、Azure 試用版にサインアップして最大 10 の無料モバイル サービスを取得し、試用期間が終わった後でも使用し続けることができます。 詳細については、「 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-dotnet-backend-windows-store-dotnet-get-started%2F)します。
* [Visual Studio 2013]。

## 新しいモバイル サービスを作成する

[AZURE.INCLUDE [mobile-services-dotnet-backend-create-new-service](../../includes/mobile-services-dotnet-backend-create-new-service.md)]

## 新しいユニバーサル Windows アプリを作成する

モバイル サービスを作成したら、Azure クラシック ポータルの簡単なクイック スタートに従って、新しいアプリケーションを作成するか、既存のアプリを変更して、モバイル サービスに接続することができます。

ここでは、モバイル サービスに接続された新しいユニバーサル Windows アプリを作成します。

1. クリックして、[Azure クラシック ポータル] で、 **Mobile Services**, 、し、先ほど作成したモバイル サービスをクリックします。

2. [クイック スタート] タブの **[プラットフォームの選択]** で **[Windows]** を選択し、**[新しい Windows ストア アプリを作成する]** を展開します。

    これにより、モバイル サービスに接続された Windows ストア アプリを作成するための簡単な 3 つの手順が表示されます。

    ![Mobile Services クイック スタートの手順](./media/mobile-services-dotnet-backend-windows-store-dotnet-get-started/mobile-quickstart-steps.png)

3. まだ行っていない、ダウンロードしてローカル コンピューターまたは仮想マシンで [Visual Studio 2013] をインストールします。

4. **[アプリケーションとサービスをダウンロードしてローカルに実行する]** で、Windows ストア アプリの言語を選択し、**[ダウンロード]** をクリックします。

    これにより、Mobile Services と Mobile Services に接続されている _To do list_ サンプル アプリケーションの両方のプロジェクトを含むソリューションがダウンロードされます。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

## ローカル モバイル サービスに対してアプリケーションをテストする

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service-dotnet](../../includes/mobile-services-dotnet-backend-test-local-service-dotnet.md)]
>[AZURE.NOTE]モバイル サービスにアクセスして MainPage.xaml.cs ファイルにあるデータをクエリおよび挿入するコードを確認できます。


## モバイル サービスを発行する

[AZURE.INCLUDE [mobile-services-dotnet-backend-publish-service](../../includes/mobile-services-dotnet-backend-publish-service.md)]


<ol start="4">
<li><p>共通のコード プロジェクトで App.xaml.cs ファイルを開き、作成するコードを見つけて、 <a href="http://msdn.microsoft.com/library/Windowsazure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx" target="_blank">MobileServiceClient</a> インスタンスを使用してこのクライアントを作成するコードをコメント アウト <em>localhost</em> 次のようにリモート モバイル サービスの URL を使用してクライアントを作成するコードをコメント解除します。</p>
        <pre><code>パブリック静的 MobileServiceClient MobileService = 新しい MobileServiceClient (
           "https://todolist.azure-mobile.net/",
           "XXXX-アプリケーション-キー-XXXXX") です。</code></pre>
    <p>クライアントは Azure に発行されたモバイル サービスにアクセスするようになります。</p></li>
</ol>

## Azure でホストされるモバイル サービスに対してアプリケーションをテストする

モバイル サービスが発行され、クライアントが Azure でホストされるリモート モバイル サービスに接続されたら、項目ストレージ用に Azure を使用してアプリケーションを実行できます。

[AZURE.INCLUDE [mobile-services-windows-universal-test-app](../../includes/mobile-services-windows-universal-test-app.md)]


## 次のステップ

クイック スタートはこれで完了です。Mobile Services で重要になるこれ以外の作業については、以下のトピックを参照してください。

* [既存のアプリケーションへの Mobile Services の追加 ][get started with data]
  <br/>格納して、モバイル サービスを使用してデータのクエリについて説明します。

* [オフライン データの同期の開始を取得]
  <br/>オフライン データの同期を使用して、応答性と信頼性にアプリを作成する方法について説明します。

* [Mobile Services アプリへの認証の追加 ][get started with authentication]
  <br/>Id プロバイダーを使用してアプリケーションのユーザーを認証する方法について説明します。

* [アプリへのプッシュ通知の追加 ][get started with push notifications]
  <br/>アプリにごく基本的なプッシュ通知を送信する方法について説明します。

* [Mobile Services .NET バックエンドのトラブルシューティングを行う]
  <br/> Mobile Services .NET バックエンドで発生する問題診断して解決する方法について説明します。

ユニバーサル Windows アプリについての詳細については、次を参照してください。 [シングル モバイル サービスから複数のデバイス プラットフォームをサポートする](mobile-services-how-to-use-multiple-clients-single-service.md#shared-vs)します。

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]









[visual studio 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546 
[get started with data]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-data.md 
[get started with data]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-data.md 
[get started with offline data sync]: mobile-services-windows-store-dotnet-get-started-offline-data.md 
[get started with authentication]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-users.md 
[get started with push notifications]: ../mobile-services-dotnet-backend-windows-store-dotnet-get-started-push.md 
[visual studio professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546 
[mobile services sdk]: http://go.microsoft.com/fwlink/?LinkId=257545 
[javascript and html]: mobile-services-win8-javascript/ 
[azure classic portal]: https://manage.windowsazure.com/ 
[troubleshoot a mobile services .net backend]: mobile-services-dotnet-backend-how-to-troubleshoot.md 

