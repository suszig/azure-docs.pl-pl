<properties
    pageTitle="ハイブリッド接続を使用して .NET バックエンド モバイル サービスから内部設置型 SQL Server への接続 | Azure Mobile Services"
    description="Azure ハイブリッド接続を使用して .NET バックエンド モバイル サービスからオンプレミスの SQL Server に接続する方法について説明します。"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="12/11/2015"
    ms.author="glenga"/>


# ハイブリッド接続を使用して Azure Mobile Services からオンプレミスの SQL Server に接続する

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


企業がクラウドに移行するときに、すべての資産を Azure にすぐに移行できない場合があります。 ハイブリッド接続を使用すると、Azure Mobile Services からオンプレミスの資産に安全に接続できます。 この方法で、モバイル クライアントが Azure を使用することによってオンプレミスのデータにアクセスできるようにします。 サポートされている資産には、Microsoft SQL Server、MySQL、HTTP Web APIs、およびほとんどのカスタム Web サービスなど、静的 TCP ポートで実行されるすべてのリソースが含まれます。 ハイブリッド接続では、Shared Access Signature (SAS) 認証を使用して、モバイル サービスとオンプレミスの Hybrid Connection Manager からハイブリッド接続への接続を保護します。 詳細については、次を参照してください。 [ハイブリッド接続の概要](../integration-hybrid-connection-overview.md)します。

このチュートリアルでは、サービスでプロビジョニングされた既定の Azure SQL Database の代わりに、ローカルのオンプレミスの SQL Server データベースを使用するように .NET バックエンド モバイル サービスを変更する方法について説明します。 」の説明に従って、ハイブリッド接続は JavaScript バックエンド モバイル サービスのサポートもされて [今回](http://blogs.msdn.com/b/azuremobile/archive/2014/05/12/connecting-to-an-external-database-with-node-js-backend-in-azure-mobile-services.aspx)します。

##前提条件##

このチュートリアルを完了するには、次のものが必要です。

- **既存の .NET バックエンド モバイル サービス** <br/>、チュートリアルに従って、[モバイル サービスの開始を取得] を作成し、[Azure クラシック ポータル] から、新しい .NET バックエンド モバイル サービスをダウンロードします。

[AZURE.INCLUDE [hybrid-connections-prerequisites](../../includes/hybrid-connections-prerequisites.md)]

## SQL Server Express をインストールし、TCP/IP を有効にして、オンプレミスの SQL Server データベースを作成する

[AZURE.INCLUDE [hybrid-connections-create-on-premises-database](../../includes/hybrid-connections-create-on-premises-database.md)]

## ハイブリッド接続を作成する

[AZURE.INCLUDE [hybrid-connections-create-new](../../includes/hybrid-connections-create-new.md)]

## オンプレミス ハイブリッド接続マネージャーをインストールして接続を完了する

[AZURE.INCLUDE [hybrid-connections-install-connection-manager](../../includes/hybrid-connections-install-connection-manager.md)]

## SQL Server データベースに接続するモバイル サービス プロジェクトを構成する

このステップでは、オンプレミスのデータベースに接続するための接続文字列を定義し、この接続が使用されるようにモバイル サービスを変更します。

1. Visual Studio 2013 で、.NET バックエンドのモバイル サービスを定義するプロジェクトを開きます。

    .NET バックエンド プロジェクトをダウンロードする方法については、次を参照してください。 [モバイル サービスを使ってみる](mobile-services-dotnet-backend-windows-store-dotnet-get-started.md) します。

2. ソリューション エクスプ ローラーで、Web.config ファイルを開き、検索、 **connectionStrings** セクションで、内部設置型 SQL Server データベースをポイントする次のような新しい SqlClient エントリを追加します。

        <add name="OnPremisesDBConnection"
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />

    置き換えるようにして `<**secure_password**>` 用に作成したパスワードを使用してこの文字列内で、 *HbyridConnectionLogin*します。

3. をクリックして **保存** Visual studio を Web.config ファイルを保存します。

    > [AZURE.NOTE]この設定は、ローカル コンピューターで実行されているときに使用されます。 Azure で実行される場合、この設定は、ポータルで定義された接続設定によってオーバーライドされます。

4. 展開、 **モデル** フォルダーとで終わるデータ モデル ファイルを開きます *Context.cs*します。

6. 変更、 **DbContext** インスタンス コンス トラクターに値を渡す `OnPremisesDBConnection` ベース **DbContext** コンス トラクター、次のスニペットに似ています。

        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }

    これで、サービスは、SQL Server データベースへの新しい接続を使用するようになります。

##データベース接続をローカルでテストする

ハイブリッド接続を Azure に発行して使用する前に、ローカルで実行したときにデータベース接続が機能するかどうかを確認することをお勧めします。 これによって、接続に関する問題を簡単に診断して修正した後、ハイブリッド接続を再発行して使用を開始できます。

[AZURE.INCLUDE [mobile-services-dotnet-backend-test-local-service-api-documentation](../../includes/mobile-services-dotnet-backend-test-local-service-api-documentation.md)]

## オンプレミスの接続文字列を使用するように Azure を更新する

データベース接続の確認が終わったら、この新しい接続文字列が Azure  から使用できるようにアプリ設定を追加して、モバイル サービスを Azure に発行する必要があります。

1. [Azure クラシック ポータル]、モバイル サービスに移動します。

1. クリックして、 **構成** タブをクリックし、検索 **接続文字列** セクションです。

    ![オンプレミスのデータベースの接続文字列](./media/mobile-services-dotnet-backend-hybrid-connections-get-started/11.png)

2. 新しい接続を追加する **SQL Server** という名前の文字列 `OnPremisesDBConnection` で次のような値。

        Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>


    置換 `<**secure_password**>` のセキュリティ保護されたパスワード *HybridConnectionLogin*します。

2. キーを押して **保存** を保存、ハイブリッド接続と接続文字列で作成しました。

3. Visual Studio を使用して、更新済みのモバイル サービス プロジェクトを Azure に発行します。

    サービスのスタート ページが表示されます。

4. いずれかを使用して、 **[今すぐ試す** スタート ページにボタン、または、モバイル サービスに接続されたクライアント アプリケーション、データベースの変更を生成するいくつかの操作を呼び出します。

    >[AZURE.NOTE]使用すると、 **[今すぐ試す** ] をクリックする API のヘルプ ページを起動 (使用するユーザー名は空白) パスワードとしてアプリケーション キーを指定します。

4. SQL Server Management Studio で、SQL Server インスタンスへの接続、オブジェクト エクスプ ローラーを開き、展開、 **OnPremisesDB** データベースし、展開 **テーブル**します。

5. 右クリックし、 **hybridService1.TodoItems** テーブルが表示され選択 **[上位 1000年行** 結果を表示します。

    アプリで発生した変更が、モバイル サービスによって、ハイブリッド接続を介してオンプレミスのデータベースに保存されたことに注意してください。

##関連項目##

+ [ハイブリッド接続の Web サイト](../../services/biztalk-services/)
+ [ハイブリッド接続の概要](../integration-hybrid-connection-overview.md)
+ [BizTalk サービス: [ダッシュボード]、[監視]、[スケール]、[構成]、および [ハイブリッド接続] タブ](../biztalk-dashboard-monitor-scale-tabs.md)
+ [データ モデルの変更を .NET バックエンド モバイル サービスに加える方法](mobile-services-dotnet-backend-how-to-use-code-first-migrations.md)

<!-- IMAGES -->


<!-- Links -->
[Azure classic portal]: http://manage.windowsazure.com
[Get started with Mobile Services]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
