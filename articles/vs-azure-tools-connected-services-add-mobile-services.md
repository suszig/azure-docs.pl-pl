<properties 
   pageTitle="Visual Studio の接続済みサービスを使用した Mobile Services の追加 | Microsoft Azure"
   description="Visual Studio の [接続済みサービスの追加] ダイアログ ボックスを使用して Mobile Services を追加する"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="tlee" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="mobile"
   ms.date="08/12/2015"
   ms.author="tarcher" />

# Visual Studio 接続済みサービスを使用した Mobile Services の追加

Visual Studio 2015 を使用して Azure Mobile Services に接続できる、 **[接続済みサービス** ダイアログ。 あらゆる C# クライアント アプリ、JavaScript アプリ、プラットフォーム非依存 Cordova アプリから接続できます。 接続すると、データを作成してアクセスしたり、カスタム API や定期ジョブを作成したり、プッシュ通信のサポートを追加したりできます。  接続済みサービスの操作は適切なあらゆる参照と接続コードを追加します。 また、Azure AD、Facebook、Twitter、Microsoft アカウントなど、さまざまな人気 ID スキームによる認証に対応しており、それを最大限に活用できます。

## サポートされているプロジェクトの種類

>[AZURE.NOTE] Visual Studio 2015 で、Windows Universal (Windows 10) のプロジェクトに接続されているサービスの追加] ダイアログ ボックスを使用して Azure Mobile Services を追加することはサポートされていません。 プロジェクトの NuGet パッケージ マネージャーを利用して適切なパッケージをインストールし、Azure Mobile Services を追加できます。

[接続済みサービス] ダイアログを使用して、次の種類のプロジェクトで Azure Mobile Services に接続することができます。

- .NET Windows 8.1 ストア、Phone、ユニバーサル アプリのプロジェクト

- JavaScript Windows 8.1 ストア、Phone、ユニバーサル アプリのプロジェクト

- Apache Cordova の Visual Studio ツールを使用して作成されたプロジェクト


## [接続済みサービスの追加] ダイアログを利用して Azure Mobile Services に接続する

1. Azure アカウントがあることを確認します。 サインアップすることができます、Azure アカウントを取得していない場合、 [無料評価版](http://go.microsoft.com/fwlink/?LinkId=518146)します。

1. 開いている、 **[接続済みサービス** ] ダイアログ ボックス。
 - .NET アプリの場合は、Visual Studio でプロジェクトを開き、コンテキスト メニューを開き、 **参照** ソリューション エクスプ ローラーでノードを選択し **接続済みサービスの追加**
 
        ![Connecting to Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)

 - ソリューション エクスプ ローラーでプロジェクト ノードのコンテキスト メニューを開きを選択し、Apache Cordova アプリのプロジェクトには、Visual Studio でプロジェクトを開きます **[接続済みサービス**します。

1.  **[接続済みサービス** ] ダイアログ ボックスで、選択 **Azure Mobile Services**, を選択し、 **構成** ] ボタンをクリックします。 Azure にまだログインしていない場合は、ログインを促すメッセージが表示される場合があります。

    ![Azure Mobile Service の追加](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)

1.  **Azure Mobile Services** ] ダイアログ ボックスで、いずれかがある場合は、既存のモバイル サービスを選択します。 新しい Azure モバイル サービスを作成する必要がある場合は、次の手順に従って作成します。 そうでない場合は、次の手順に進みます。

    新しいモバイル サービスのアカウントを作成します。
    1. 選択して、* * サービスの作成 * *] ダイアログ ボックスの下部にあるリンクです。
        ![新しいモバイル接続サービスを追加する](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)




    2. On the **Create Mobile Service** dialog box, you can choose a JavaScript backend mobile service, or a .NET backend mobile service from the **Runtime** drop-down list. 
  
        ![Creating a mobile service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)

        A JavaScript backend service is simple and powerful. If you create a JavaScript backend mobile service, the server-side JavaScript code is stored in the cloud, but you can edit server scripts by using Server Explorer, or the Azure management portal. 

        A .NET backend mobile service gives you the full power and flexibility of Web API and Entity Framework. If you create a .NET backend mobile service, a project is created for you and added to your solution. 

    1. Choose the **Region** where you want the mobile service, and then enter a user name and password for the server.
 
    1. After you've entered all the required information, choose the **Create** button to create the mobile service.
    2. The new mobile service should appear in the service list on the **Azure Mobile Services** dialog box. Choose the new mobile service in the list and then choose the **Add** button to add the service to your project.
    

1. 表示された作業の開始ページを確認し、プロジェクトがどのように変更されたかを確認します。 作業の開始ページは、接続済みサービスを追加したかどうかに関係なく、ブラウザーに表示されます。 推奨される次の手順とコード例を確認したり、[変更内容] ページに切り替えて、プロジェクトに追加された参照と、コードと構成ファイルがどのように変更されたかを確認したりすることができます。

1. コード サンプルをガイドとして利用し、モバイル サービスにアクセスするためのコードを記述します。

## プロジェクトを変更する方法

Visual Studio でプロジェクトを変更する方法はプロジェクトの種類によって異なります。 C# クライアント アプリで、次を参照してください。 [何が起こったか – c# プロジェクト](http://go.microsoft.com/fwlink/p/?LinkId=513119)します。 JavaScript クライアント アプリケーションの場合、次を参照してください。 [何が起こったか – JavaScript プロジェクト](http://go.microsoft.com/fwlink/p/?LinkId=513120)します。 Cordova アプリを参照してください。 [何が起こったか – Cordova プロジェクト](http://go.microsoft.com/fwlink/p/?LinkId=513116)します。


##次のステップ

質問してヘルプを表示します。 

 - [MSDN フォーラム: Azure Mobile Services](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)

 - [Microsoft Azure チーム ブログの Azure Mobile Services](http://azure.microsoft.com/blog/topics/mobile/)

 - [azure.microsoft.com の Azure Mobile Services](http://azure.microsoft.com/services/mobile-services/)

 - [azure.microsoft.com の Azure Mobile Services](http://azure.microsoft.com/documentation/services/mobile-services/)




