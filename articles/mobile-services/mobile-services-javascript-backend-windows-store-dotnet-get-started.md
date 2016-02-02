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


# <a name="getting-started"> </a>モバイル サービスを使ってみる

[AZURE.INCLUDE [mobile-services-selector-get-started](../../includes/mobile-services-selector-get-started.md)]
&nbsp;

[AZURE.INCLUDE [mobile-services-hero-slug](../../includes/mobile-services-hero-slug.md)]

このチュートリアルでは、Azure Mobile Services を使用してユニバーサル Windows アプリにクラウドベースのバックエンド サービスを追加する方法を示します。 ユニバーサル Windows アプリ ソリューションには、Windows ストア 8.1 と Windows Phone ストア 8.1 の両方のアプリのプロジェクトと、共通の共有プロジェクトが含まれます。 詳細については、次を参照してください。 [を Windows および Windows Phone を対象とするユニバーサル Windows アプリの構築](http://msdn.microsoft.com/library/windows/apps/xaml/dn609832.aspx)します。

このチュートリアルでは、新しいモバイル サービスと、新しいモバイル サービスにアプリケーション データを保存する簡単な *To do list* アプリケーションの両方を作成します。 作成するモバイル サービスは、サーバー側ビジネス ロジックのために JavaScript を使用します。 Visual Studio を使用してサポートされる .NET 言語でサーバー側ビジネス ロジックを記述できるモバイル サービスを作成する方法については、このトピックの「.NET バックエンド バージョン」を参照してください。

[AZURE.INCLUDE [mobile-services-windows-universal-get-started](../../includes/mobile-services-windows-universal-get-started.md)]

このチュートリアルを完了するには、以下が必要です。

* アクティブな Azure アカウント。 アカウントがない場合は、Azure 試用版にサインアップして最大 10 の無料モバイル サービスを取得し、試用期間が終わった後でも使用し続けることができます。 詳細については、「 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-services-javascript-backend-windows-store-javascript-get-started%2F)します。
* [Visual Studio 2013 Express for Windows]

## 新しいモバイル サービスを作成する

[AZURE.INCLUDE [mobile-services-create-new-service](../../includes/mobile-services-create-new-service.md)]

## 新しいユニバーサル Windows アプリを作成する

モバイル サービスを作成したら、Azure クラシック ポータルの簡単なクイック スタートに従って、新しいユニバーサル Windows アプリを作成するか、既存の Windows ストアまたは Windows Phone アプリ プロジェクトを変更してモバイル サービスに接続することができます。

ここでは、モバイル サービスに接続された新しいユニバーサル Windows アプリを作成します。

1.  クリックして、[Azure クラシック ポータル] で、 **Mobile Services**, 、し、先ほど作成したモバイル サービスをクリックします。

2. [クイック スタート] タブの **[プラットフォームの選択]** で **[Windows]** を選択し、**[新しい Windows ストア アプリを作成する]** を展開します。

    これにより、モバイル サービスに接続された Windows ストア アプリを作成するための簡単な 3 つの手順が表示されます。

    ![Mobile Services クイック スタートの手順](./media/mobile-services-javascript-backend-windows-store-dotnet-get-started/mobile-quickstart-steps.png)

3. まだインストールしていない場合は、Visual Studio 2013 Express for Windows をダウンロードし、ローカル コンピューターまたは仮想マシンにインストールします。

4. **[TodoItem テーブルを作成する]** をクリックして、アプリケーション データを格納するテーブルを作成します。

5. **[アプリケーションをダウンロードして実行する]** で、アプリケーションの言語を選択し、**[ダウンロード]** をクリックします。

    これにより、モバイル サービスに接続する、*To do list* サンプル アプリケーションのプロジェクトがダウンロードされます。 圧縮されたプロジェクト ファイルをローカル コンピューターに保存し、保存場所を書き留めておいてください。

## Windows アプリケーションを実行する

[AZURE.INCLUDE [mobile-services-javascript-backend-run-app](../../includes/mobile-services-javascript-backend-run-app.md)]
>[AZURE.NOTE]モバイル サービスにアクセスして MainPage.xaml.cs ファイルにあるデータをクエリおよび挿入するコードを確認できます。

## 次のステップ

クイック スタートはこれで完了です。Mobile Services で重要になるこれ以外の作業については、以下のトピックを参照してください。

* [オフライン データの同期の開始を取得]  
  オフライン データの同期を使用してアプリケーションの反応と信頼性を高める方法について説明します。

* [Mobile Services アプリへの認証の追加 ][get started with authentication]  
  ID プロバイダーを使用してアプリケーションのユーザーを認証する方法について説明します。

* [アプリへのプッシュ通知の追加 ][get started with push notifications]  
  アプリケーションにごく基本的なプッシュ通知を送信する方法について説明します。

* [.NET クライアント ライブラリを使用する方法](mobile-services-windows-dotnet-how-to-use-client-library.md)  
 モバイル サービスのクエリ、データの処理、およびカスタム Api にアクセスする方法について説明します。

[AZURE.INCLUDE [app-service-disqus-feedback-slug](../../includes/app-service-disqus-feedback-slug.md)]








[getting started with mobile services]: #getting-started 
[create a new mobile service]: #create-new-service 
[define the mobile service instance]: #define-mobile-service-instance 
[next steps]: #next-steps 
[get started with offline data sync]: mobile-services-windows-store-dotnet-get-started-offline-data.md 
[get started with authentication]: ../mobile-services-windows-store-dotnet-get-started-users.md 
[get started with push notifications]: ../mobile-services-javascript-backend-windows-store-dotnet-get-started-push.md 
[visual studio 2013 express for windows]: http://go.microsoft.com/fwlink/?LinkId=257546 
[mobile services sdk]: http://go.microsoft.com/fwlink/?LinkId=257545 
[azure classic portal]: https://manage.windowsazure.com/ 

