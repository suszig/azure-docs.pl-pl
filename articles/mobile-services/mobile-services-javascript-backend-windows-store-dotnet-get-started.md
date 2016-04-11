<properties
    pageTitle="Windows ストア アプリ向け Mobile Services の使用 (C#) | Microsoft Azure"
    description="このチュートリアルでは、C# で Windows ストア用の開発を行う場合に Azure Mobile Services を使用する方法を示します。"
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
    ms.topic="get-started-article" 
    ms.date="11/06/2015"
    ms.author="glenga"/>

# <a name="getting-started"> </a>Mobile Services の使用

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

このチュートリアルでは、Azure Mobile Services を使用してユニバーサル Windows アプリにクラウドベースのバックエンド サービスを追加する方法を示します。 ユニバーサル Windows アプリ ソリューションには、Windows ストア 8.1 と Windows Phone ストア 8.1 の両方のアプリのプロジェクトと、共通の共有プロジェクトが含まれます。 詳細については、次を参照してください。 [を Windows および Windows Phone を対象とするユニバーサル Windows アプリの構築](http://msdn.microsoft.com/library/windows/apps/xaml/dn609832.aspx)します。

このチュートリアルでは、新しいモバイル サービスと、新しいモバイル サービスにアプリ データを保存する簡単な *To do list* アプリの両方を作成します。 作成するモバイル サービスは、サーバー側ビジネス ロジックのために JavaScript を使用します。 Visual Studio を使用してサポートされる .NET 言語でサーバー側ビジネス ロジックを記述できるモバイル サービスを作成する方法については、このトピックの「.NET バックエンド バージョン」を参照してください。

[AZURE.INCLUDE [mobile-services-windows-universal-get-started](../../includes/mobile-services-windows-universal-get-started.md)]

このチュートリアルを完了するには、以下が必要です。

* アクティブな Azure アカウント。 アカウントがない場合は、Azure 試用版にサインアップして、最大 10 件の無料モバイル サービスを入手できます。このサービスは評価終了後も使用できます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-javascript-backend-windows-store-javascript-get-started%2F)をご覧ください。
* [Visual Studio 2013 Express for Windows]

## 新しいモバイル サービスを作成する

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## 新しいユニバーサル Windows アプリを作成する

モバイル サービスを作成したら、Azure クラシック ポータルの簡単なクイック スタートに従って、新しいユニバーサル Windows アプリを作成するか、既存の Windows ストアまたは Windows Phone アプリ プロジェクトを変更してモバイル サービスに接続することができます。

ここでは、モバイル サービスに接続された新しいユニバーサル Windows アプリを作成します。

1.   [Azure classic portal], 、] をクリックして **Mobile Services**, 、作成したモバイル サービスをクリックします。


2. クイック スタート] タブをクリックして **Windows** [ **プラットフォームの選択]** 展開 **新しい Windows ストア アプリを作成**します。

    これにより、モバイル サービスに接続された Windows ストア アプリを作成するための簡単な 3 つの手順が表示されます。

    ![Mobile Services クイック スタートの手順](./media/mobile-services-javascript-backend-windows-store-dotnet-get-started/mobile-quickstart-steps.png)

3. ダウンロードしてインストールをまだ行っていない場合、 [Visual Studio 2013 Express for Windows] 、ローカル コンピューターまたは仮想マシン。

4. クリックして **作成 TodoItem テーブル** アプリケーション データを格納するテーブルを作成します。

5.  **をダウンロードして、アプリを実行**, をアプリの言語を選択してクリックして **ダウンロード**します。

    これにより、サンプルのプロジェクトがダウンロード *作業の一覧に* 、モバイル サービスに接続されているアプリケーションです。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

## Windows アプリケーションを実行する

[AZURE.INCLUDE [mobile-services-javascript-backend-run-app](../../includes/mobile-services-javascript-backend-run-app.md)]

>[AZURE.NOTE]モバイル サービスを照会および挿入の MainPage.xaml.cs ファイルにあるデータにアクセスするコードを確認できます。

## 次のステップ
クイック スタートはこれで完了です。Mobile Services で重要になるこれ以外の作業については、以下のトピックを参照してください。

* [Get started with offline data sync]
  オフライン データの同期を使用してアプリケーションの反応と信頼性を高める方法について説明します。

* [Mobile Services アプリへの認証の追加 ][Get started with authentication]  
  ID プロバイダーを使用してアプリケーションのユーザーを認証する方法について説明します。

* [アプリケーションにプッシュ通知を追加する][Get started with push notifications]  
  アプリケーションにごく基本的なプッシュ通知を送信する方法について説明します。

* [.NET クライアント ライブラリを使用する方法](mobile-services-windows-dotnet-how-to-use-client-library.md)  
 モバイル サービスのクエリ、データの処理、およびカスタム Api にアクセスする方法について説明します。

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]

<!-- Anchors. -->
[Getting started with Mobile Services]:#getting-started
[Create a new mobile service]:#create-new-service
[Define the mobile service instance]:#define-mobile-service-instance
[Next Steps]:#next-steps

<!-- Images. -->



<!-- URLs. -->
[Get started with offline data sync]: mobile-services-windows-store-dotnet-get-started-offline-data.md
[Get started with authentication]: ../mobile-services-windows-store-dotnet-get-started-users.md
[Get started with push notifications]: ../mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md
[Visual Studio 2013 Express for Windows]: http://go.microsoft.com/fwlink/?LinkId=257546
[Mobile Services SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure classic portal]: https://manage.windowsazure.com/
 

