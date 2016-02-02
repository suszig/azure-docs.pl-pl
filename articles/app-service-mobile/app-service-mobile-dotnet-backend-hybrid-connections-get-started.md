<properties
    pageTitle="ハイブリッド接続を使用して Azure モバイル アプリをオンプレミスの SQL Server に接続する | Microsoft Azure"
    description="ハイブリッド接続を使用して App Service のモバイル アプリからオンプレミスの SQL Server に接続する方法について説明します。"
    services="app-service\mobile"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.date="10/23/2015"
    ms.author="glenga"/>



# ハイブリッド接続を使用して Mobile Apps からオンプレミスの SQL Server に接続する

企業がクラウドに移行するときに、すべての資産を Azure にすぐに移行できない場合があります。 ハイブリッド接続は、Azure App Service の Mobile Apps 機能がオンプレミスのアセットに安全に接続できるようにします。 この方法で、モバイル クライアントが Azure を使用することによってオンプレミスのデータにアクセスできるようにします。 サポートされている資産には、Microsoft SQL Server、MySQL、HTTP Web APIs、およびほとんどのカスタム Web サービスなど、静的 TCP ポートで実行されるすべてのリソースが含まれます。 ハイブリッド接続では、Shared Access Signature (SAS) 認証を使用して、モバイル サービスとオンプレミスの Hybrid Connection Manager からハイブリッド接続への接続を保護します。 詳細については、次を参照してください。 [ハイブリッド接続の概要](../integration-hybrid-connection-overview.md)します。

このチュートリアルでは、サービスでプロビジョニングされた既定の Azure SQL Database の代わりに、ローカルのオンプレミスの SQL Server データベースを使用するようにモバイル アプリの .NET バックエンドを変更する方法について説明します。

[AZURE.INCLUDE [app-service-mobile-to-web-and-api](../../includes/app-service-mobile-to-web-and-api.md)]

## 前提条件

このチュートリアルを完了するには、次のものが必要です。

- **既存の Mobile App バックエンド** <br/>に従って、 [クイック スタート チュートリアル](app-service-mobile-windows-store-dotnet-get-started.md) を作成し、[Azure ポータル] から、新しい .NET バックエンド モバイル アプリをダウンロードします。

[AZURE.INCLUDE [hybrid-connections-prerequisites](../../includes/hybrid-connections-prerequisites.md)]

## SQL Server Express をインストールし、TCP/IP を有効にして、オンプレミスの SQL Server データベースを作成する

[AZURE.INCLUDE [hybrid-connections-create-on-premises-database](../../includes/hybrid-connections-create-on-premises-database.md)]

## ハイブリッド接続を作成する

モバイル アプリ バックエンドのコード部分 (Web アプリ) 用の新しいハイブリッド接続と BizTalk サービスを作成する必要があります。

1. [Azure ポータル、] で [モバイル アプリを参照してクリックして **ネットワーク** に設定します。

    ![Web アプリに移動する](./media/app-service-mobile-dotnet-backend-hybrid-connections-get-started/mobile-app-link-to-web-app-backend.png)

2. **[ハイブリッド接続]** セクションで **[ハイブリッド接続エンドポイントの構成]** をクリックします。

    ![ハイブリッド接続](./media/app-service-mobile-dotnet-backend-hybrid-connections-get-started/start-hybrid-connection.png)

2. [ハイブリッド接続] ブレードで、**[追加]**、**[新しいハイブリッド接続]** の順にクリックします。

3. **ハイブリッド接続の作成] ブレード**, 、提供、 **名** と **ホスト名** ハイブリッド接続とセットの **ポート** に `1433年`します。

    ![Create a hybrid connection](./media/app-service-mobile-dotnet-backend-hybrid-connections-get-started/create-hybrid-connection.png)

4. **[Biz Talk Service]** をクリックし、BizTalk サービスの名前を入力し、**[OK]** をクリックします。

    このチュートリアルでは、**mobile1** を使用します。 新しい BizTalk サービスの一意の名前を指定する必要があります。

    処理が完了すると、**[通知]** 領域に緑色の "**SUCCESS**" という文字が点滅します。**[ハイブリッド接続]** ブレードには、状態が **[未接続]** の新しいハイブリッド接続が表示されます。

    ![1 つのハイブリッド接続が作成された](./media/app-service-mobile-dotnet-backend-hybrid-connections-get-started/hybrid-connection-created.png)

これで、クラウド ハイブリッド接続インフラストラクチャの重要な部分が完了しました。 次に、対応するオンプレミスの部分を作成します。

## オンプレミスのハイブリッド接続マネージャーをインストールして接続を完了する

[AZURE.INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

## SQL Server データベースに接続するモバイル アプリ バックエンド プロジェクトを構成する

このステップでは、オンプレミスのデータベースに接続するための接続文字列を定義し、この接続を使用するようにモバイル アプリ バックエンドを変更します。

1. Visual Studio 2013 で、モバイル アプリ バックエンドを定義するプロジェクトを開きます。

    .NET バックエンド プロジェクトをダウンロードする方法については、次を参照してください。 [クイック スタート チュートリアル](app-service-mobile-windows-store-dotnet-get-started.md)します。

2. ソリューション エクスプローラーで、Web.config ファイルを開き、 **connectionStrings** セクションを探し、次のような新しい SqlClient エントリを追加します。このエントリはオンプレミスの SQL Server データベースを指しています。

        <add name="OnPremisesDBConnection"
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />

    置き換えるようにして `< * * secure_password * * >` 用に作成したパスワードを使用してこの文字列で  *HybridConnectionLogin*します。

3. Visual Studio で **[保存]** をクリックして、Web.config ファイルを保存します。
    > [AZURE.NOTE]この接続設定は、ローカル コンピューターで実行されるときに使用されます。 Azure で実行される場合、この設定は、ポータルで定義された接続設定によってオーバーライドされます。

4. **Models** フォルダーを展開し、*Context.cs* で終わるデータ モデル ファイルを開きます。

6. 変更、 **DbContext** インスタンス コンス トラクターに値を渡す `OnPremisesDBConnection` ベース **DbContext** コンス トラクター、次のスニペットに似ています。

        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }

    これで、サービスは、SQL Server データベースへの新しい接続を使用するようになります。

## オンプレミスの接続文字列を使用するように Azure を更新する

次に、この新しい接続文字列用のアプリ設定を追加して、Azure から使用できるようにする必要があります。

1. ポータルに戻り、[Azure] でモバイル アプリの web アプリのバックエンド コード、クリックして **すべての設定**, 、し **アプリケーション設定**します。

3. **Web アプリ設定** ブレードで、下へスクロールして **接続文字列** し、新しい追加 **SQL Server** という名前の接続文字列 `OnPremisesDBConnection` のような値を持つ `Server = OnPremisesServer, 1433; データベース = OnPremisesDB;ユーザー ID = HybridConnectionsLogin です。パスワード = < * * secure_password * * >`します。

    置換 `< * * secure_password * * >` 、内部設置型データベースのセキュリティ保護されたパスワードです。

    ![オンプレミスのデータベースの接続文字列](./media/app-service-mobile-dotnet-backend-hybrid-connections-get-started/set-sql-server-database-connection.png)

2. [**保存**] をクリックして、今作成したハイブリッド接続と接続文字列を保存します。

## モバイル アプリ バックエンドを Azure に発行してテストする

最後に、モバイル アプリ バックエンドを Azure に発行し、ハイブリッド接続を使用してオンプレミスのデータベースにデータを格納することを検証します。

3. Visual Studio でプロジェクトを右クリックし、**[発行]**、**[Web の発行]**、**[Microsoft Azure Websites]** の順にクリックします。

    Visual Studio を使用する代わりにすることもできます [Git を使用してバックエンドを発行する](mobile-services-dotnet-backend-store-code-source-control.md)します。

2. Azure の資格情報でサインインし、**[既存の Web サイトの選択]** から目的のサービスを選択します。

    Visual Studio によって、Azure から直接、発行設定がダウンロードされます。

3. 最後に、**[発行]** をクリックします。

    発行が完了すると、サービスが再起動し、バックエンド スタート ページが表示されます。

4. モバイル アプリに接続されたクライアント アプリケーションを使用して、データベースの変更を生成する操作を呼び出します。

5. SQL Server Management Studio で、SQL Server インスタンスに接続し、オブジェクト エクスプローラーを開き、**OnPremisesDB** データベースを展開し、**[テーブル]** を展開します。

6. **hybridService1.TodoItems** テーブルを右クリックし、**[先頭の 1000 行を選択]** をクリックすると、結果が表示されます。

    クライアント アプリで発生した変更が、モバイル アプリ バックエンドによって、ハイブリッド接続を使用して、オンプレミスのデータベースに保存されていることに注意してください。

## 関連項目

+ [ハイブリッド接続の web サイト](../../services/biztalk-services/)
+ [ハイブリッド接続の概要](../integration-hybrid-connection-overview.md)
+ [BizTalk サービス: ダッシュ ボード、モニター、スケール、構成、およびハイブリッド接続] タブ](../biztalk-dashboard-monitor-scale-tabs.md)
+ [.NET バックエンド モバイル サービスの変更をデータ モデルを作成する方法](../mobile-services-dotnet-backend-how-to-use-code-first-migrations.md)






[azure portal]: https://portal.azure.com/ 
[azure classic portal]: http://go.microsoft.com/fwlink/p/?linkid=213885 
[get started with mobile services]: ../mobile-services-windows-store-dotnet-get-started.md 

