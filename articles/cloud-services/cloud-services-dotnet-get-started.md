<properties
    pageTitle="Azure Cloud Services と ASP.NET を使ってみる | Microsoft Azure"
    description="ASP.NET MVC と Azure を使用して多層アプリケーションを作成する方法について説明します。 ここで取り上げるアプリケーションは、クラウド サービス内で、Web ロールと worker ロールと連係して動作します。 Entity Framework、SQL Database、Azure Storage のキューと BLOB を使用しています。"
    services="cloud-services, storage"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="09/01/2015"
    ms.author="tdykstra"/>

# Azure Cloud Services と ASP.NET を使ってみる

> [AZURE.SELECTOR]
- [Node.js](cloud-services-nodejs-develop-deploy-app.md)
- [.NET](cloud-services-dotnet-get-started.md)

## 概要

このチュートリアルでは ASP.NET mvc フロント エンドを多層 .NET アプリケーションを作成してデプロイする方法、 [Azure クラウド サービス](fundamentals-application-models.md#CloudServices)します。 アプリケーションを使用して [Azure SQL Database](http://msdn.microsoft.com/library/azure/ee336279), 、 [Azure Blob サービス](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), 、および [Azure Queue サービス](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern)します。 実行できます [Visual Studio プロジェクトをダウンロード](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4) MSDN コード ギャラリーからです。

このチュートリアルでは、アプリケーションを作成してローカルで実行する方法、アプリケーションを Azure にデプロイしてクラウドで実行する方法、およびアプリケーションを最初から作成する方法について説明します。 アプリケーションを最初から作成する手順から始め、必要に応じて後でアプリケーションのテストとデプロイ手順に進んでください。

## Contoso Ads アプリケーション

このアプリケーションは、広告の掲示板です。 ユーザーは、テキストを入力し、画像をアップロードして広告を作成します。 広告の一覧には縮小表示画像が表示されます。広告を選択すると、フルサイズ画像と詳細が表示されます。

![Ad list](./media/cloud-services-dotnet-get-started/list.png)

アプリケーションを使用して、 [キューを中心とした作業パターン](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) をバックエンド プロセスに縮小表示の作成の CPU 負荷の高い作業をオフロードします。

## 代替アーキテクチャ: Websites および WebJobs

このチュートリアルでは、Azure クラウド サービスでフロントエンドとバックエンドの両方を実行する方法について説明します。 代わり、フロント エンドを実行する方法が、 [Azure の web サイト](/services/web-sites/) を使用して、 [web ジョブ](http://go.microsoft.com/fwlink/?LinkId=390226) バック エンドの (現在プレビュー中) の機能です。 Web ジョブを使用するチュートリアルについては、次を参照してください。 [Azure web ジョブ SDK を使ってみる](../websites-dotnet-webjobs-sdk-get-started.md)します。 自分のシナリオに最適なサービスを選択する方法については、次を参照してください。 [Azure Websites、Cloud Services、および仮想マシンの比較](../choose-web-site-cloud-service-vm.md)します。

## 学習内容

* Azure SDK をインストールして、Azure 向け開発用にコンピューターを準備する方法
* ASP.NET MVC の Web ロールと worker ロールを使用して、Visual Studio クラウド サービス プロジェクトを作成する方法
* Azure ストレージ エミュレーターを使用してクラウド サービス プロジェクトをローカルでテストする方法
* Azure ストレージ アカウントを使用してクラウド プロジェクトを Azure クラウド サービスに発行し、テストする方法
* ファイルを Azure BLOB サービスにアップロードして保存する方法
* Azure Queue サービスを使用して階層間の通信を行う方法

## 前提条件

このチュートリアルでは、理解している [基本的な概念については、Azure クラウド サービス](fundamentals-application-models.md#CloudServices) など *web ロール* と *ワーカー ロール* 用語です。  使用する方法を知っているまた [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) または [Web フォーム](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview) Visual Studio のプロジェクトです。 サンプル アプリケーションでは MVC を使用しますが、チュートリアルのほとんどは Web フォームにも当てはまります。

Azure サブスクリプションがなくてもアプリケーションをローカルに実行できますが、アプリケーションをクラウドにデプロイするには Azure サブスクリプションが必要になります。 アカウントを持っていない場合は、[MSDN サブスクライバーの特典を有効にする](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668)か、[無料試用版にサインアップ](/pricing/free-trial/?WT.mc_id=A55E3C668)してください。

このチュートリアルの手順は、次のどちらの製品でも使用できます。

* Visual Studio 2013
* Visual Studio 2013 Express for Web

これらの製品をお持ちでない場合、Azure SDK をインストールすると Visual Studio 2013 Express for Web が自動的にインストールされます。

## アプリケーションのアーキテクチャ

このアプリでは、広告を SQL データベースに格納します。その際、テーブルを作成してデータにアクセスするために Entity Framework Code First を使用します。 それぞれの広告に対し、フルサイズ画像用と縮小表示画像用の 2 つの URL がデータベースに格納されます。

![広告表
](./media/cloud-services-dotnet-get-started/adtable.png)

ユーザー イメージをアップロードすると、web ロールで実行するフロント エンドによってその画像、 [Azure blob](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage), 、広告情報がその blob を示す URL と共にデータベースに格納しています。 同時に、メッセージが Azure キューに書き込まれます。 worker ロールで実行されているバックエンド プロセスは、定期的にキューをポーリングして新しいメッセージの有無を確認します。 新しいメッセージが出現すると、worker ロールはその画像の縮小表示を作成し、その広告の縮小表示 URL データベース フィールドを更新します。 次の図に、アプリケーションの各パーツのやり取りを示します。

![Contoso Ads architecture](./media/cloud-services-dotnet-get-started/apparchitecture.png)

[AZURE.INCLUDE [install-sdk-2013-only](../../includes/install-sdk-2013-only.md)]

## 完成したソリューションをダウンロードして実行する

1. ダウンロードして解凍、 [完成したソリューション](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4)します。

2. Visual Studio を起動します。

3.  **ファイル** メニュー **プロジェクトを開く**, 、ソリューションをダウンロードした場所に移動し、ソリューション ファイルを開きます。

3. Ctrl + Shift + B キーを押して、ソリューションをビルドします。

    既定では、Visual Studio によって自動的に復元されたに含まれていなかった NuGet パッケージのコンテンツ、 *.zip* ファイルです。 パッケージが復元されない場合に手動でインストールする、 **ソリューションの NuGet パッケージの管理** ] ダイアログ ボックスをクリックし、 **復元** 右上にあるボタンをクリックします。

3.  **ソリューション エクスプ ローラー**, 、ことを確認して **ContosoAdsCloudService** がスタートアップ プロジェクトとして選択されています。

1. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

    クラウド サービス プロジェクトをローカルで実行すると Visual Studio が、Azure を自動的に起動 *コンピューティング エミュレーター* と Azure *ストレージ エミュレーター*します。 コンピューティング エミュレーターは、コンピューターのリソースを使用して、Web ロール環境と worker ロール環境をシミュレートします。 ストレージ エミュレーターを使用して、 [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) Azure クラウド ストレージをシミュレートします。

    クラウド サービス プロジェクトを初めて実行するときは、エミュレーターが起動されるまで 1 分程度かかります。 エミュレーターの起動が完了すると、既定のブラウザーが開き、アプリケーションのホーム ページが表示されます。

    ![Contoso Ads architecture](./media/cloud-services-dotnet-get-started/home.png)

2. クリックして  **Create an Ad**します。

2. 何らかのテスト データを入力し、選択、 *.jpg* 、アップロードする画像をクリックして **作成**します。

    ![作成ページ](./media/cloud-services-dotnet-get-started/create.png)

    [Index] ページが表示されますが、新しい広告の処理が完了していないため、縮小表示はまだ表示されません。

3. しばらく待ってから [Index] ページを更新すると、縮小表示が表示されます。

    ![Index ページ](./media/cloud-services-dotnet-get-started/list.png)

4. クリックして **詳細** 、フルサイズの画像を表示する広告のです。

    ![詳細ページ](./media/cloud-services-dotnet-get-started/details.png)

これまで、クラウドへの接続なしで、アプリケーションを完全にローカル コンピューター上で実行してきました。 ストレージ エミュレーターは、キュー データと BLOB データを SQL Server Express LocalDB データベースに格納します。アプリケーションは、広告データを別の LocalDB データベースに格納します。 Web アプリが初めてアクセスしようとしたとき、広告データベースが Entity Framework Code First によって自動的に作成されました。

次のセクションでは、クラウドで実行されたときにキュー、BLOB、およびアプリケーション データベース用に Azure クラウド リソースを使用するソリューションを構成します。 ローカルでの実行を継続しながらクラウド ストレージおよびデータベース リソースを使用したい場合は、接続文字列の設定を構成します。その方法については、後で説明します。

## Azure にアプリケーションをデプロイする

アプリケーションをクラウドで実行するには、次の手順を実行します。

* Azure クラウド サービスを作成します。
* Azure SQL データベースを作成します。
* Azure ストレージ アカウントを作成します。
* Azure で実行されたときに Azure SQL データベースを使用するためのソリューションを構成します。
* Azure で実行されたときに Azure ストレージ アカウントを使用するためのソリューションを構成します。
* プロジェクトを Azure クラウド サービスにデプロイします。

### Azure クラウド サービスの作成

Azure クラウド サービスは、アプリケーションの実行環境です。

1. ブラウザーで開く、 [Azure クラシック ポータル](http://manage.windowsazure.com)します。

2. クリックして **新しい > コンピューティング > クラウド サービス > 簡易作成**します。

4. [URL] 入力ボックスに、URL プレフィックスを入力します。

    この URL は一意である必要があります。  選択したプレフィックスが既に他のユーザーによって使用されている場合は、エラー メッセージが表示されます。

5. アプリケーションをデプロイするリージョンを選択します。

    このフィールドは、クラウド サービスがホストされるデータセンターを指定します。 運用アプリケーションの場合は、顧客に最も近いリージョンを選択してください。 このチュートリアルでは、自分に最も近いリージョンを選択してください。

6. クリックして **クラウド サービスを作成する**です。

    次の画像では、クラウド サービスは contosoads.cloudapp.net という URL で作成されます。

    ![新しいクラウド サービス](./media/cloud-services-dotnet-get-started/newcs.png)

### Azure SQL データベースの作成

アプリをクラウドで実行すると、クラウドベースのデータベースがアプリによって使用されます。

1.  [Azure クラシック ポータル](http://manage.windowsazure.com), 、クリックして **新規 > Data Services > SQL データベース > 簡易作成**します。

1.  **データベース名** ボックスに、入力 *contosoads*します。

1.  **Server** ドロップダウン リストで、選択 **新しい SQL データベース サーバー**します。

    または、サブスクリプションに既にサーバーが設定されている場合は、ドロップダウン リストからそのサーバーを選択します。

1. 同じ選択 **地域** で選択したクラウド サービスです。

    クラウド サービスとデータベースが異なるデータ センター (異なるリージョン) にあると、遅延時間が長くなり、データ センター外の帯域幅に対して課金されることになります。 データ センター内の帯域幅は無料です。

1. 管理者が入力 **ログイン名** と **パスワード**します。

    選択した場合は **新しい SQL データベース サーバー** 、既存の名前とパスワードで入力していない、新しい名前と、データベースへのアクセス時に使用するようになりました定義しているパスワードを入力してください。 以前に作成したサーバーを選択した場合は、既に作成されている管理ユーザー アカウントのパスワードを入力します。

1. クリックして **SQL データベースの作成**します。

    ![新しい SQL データベース](./media/cloud-services-dotnet-get-started/newdb.png)

1. Azure では、データベースの作成が完了したら、クリックして、 **SQL データベース** ] タブで、ポータルの左側のウィンドウと、新しいデータベースの名前をクリックします。

2. クリックして、 **ダッシュ ボード** ] タブをクリックします。

3. クリックして **使用できる IP アドレス管理**します。

4.  **使用できるサービス**, 、変更 **Azure サービス** に **はい**します。

5. クリックして **保存**します。

### Azure のストレージ アカウントの作成

Azure ストレージ アカウントは、キューおよび BLOB データをクラウドに格納するためのリソースを提供します。

現実のアプリケーションでは、通常、アプリケーション データとログ データ、テスト データと運用データに別個のアカウントを作成します。 このチュートリアルでは、アカウントを 1 つだけ使用します。

1.  [Azure クラシック ポータル](http://manage.windowsazure.com), 、クリックして **新規 > Data Services > ストレージ > 簡易作成**します。

4.  **URL** ボックスで、URL プレフィックスを入力します。

    このプレフィックスに、このボックスの下に表示されているテキストを連結したものが、ストレージ アカウントに固有の URL になります。 入力したプレフィックスが既に他のユーザーによって使用されている場合は、別のプレフィックスを選択する必要があります。

5. 設定、 **地域** クラウド サービス用に選択したリージョンと同じリージョンのドロップダウン リストです。

    クラウド サービスとストレージ アカウントが異なるデータ センター (異なるリージョン) にあると、遅延時間が長くなり、データ センター外の帯域幅に対して課金されることになります。 データ センター内の帯域幅は無料です。

    Azure アフィニティ グループには、データ センター内のリソース間の距離を最短にする機能が備わっており、それにより遅延時間を短縮できます。 このチュートリアルでは、アフィニティ グループを使用しません。 詳細については、次を参照してください。 [Azure でアフィニティ グループを作成する方法](http://msdn.microsoft.com/library/jj156209.aspx)します。

6. 設定、 **レプリケーション** ドロップ ダウン リスト **ローカル冗長**します。

    Geo レプリケーションをストレージ アカウントに対して有効にすると、1 次拠点で重大な障害が発生した場合に備えて、保存したコンテンツは 2 次データセンターに複製されて、フェイルオーバーが可能になります。 Geo レプリケーションには追加費用が発生する場合があります。 また、テストおよび開発アカウントの場合は、一般的に Geo レプリケーションに対する課金は避けたいと考えるでしょう。 詳細については、次を参照してください。 [の作成、管理、またはストレージ アカウントを削除](../storage-create-storage-account/#replication-options)します。

5. クリックして **ストレージ アカウントの作成**します。

    ![新しいストレージ アカウント ](./media/cloud-services-dotnet-get-started/newstorage.png)

    次の画像では、ストレージ アカウントは `contosoads.core.windows.net` という URL で作成されます。

### Azure で実行されたときに Azure SQL データベースを使用するためのソリューションの設定

Web プロジェクトと worker ロール プロジェクトはそれぞれ独自のデータベース接続文字列を持ち、アプリが Azure で実行されたときにそれぞれが Azure SQL データベースを指し示す必要があります。

使用して、 [Web.config の変換](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) web ロールとワーカー ロールの設定、クラウド サービス環境です。

>[AZURE.NOTE] このセクションと、次のセクションでは、プロジェクト ファイルで資格情報を格納します。 [公開されているソース コード リポジトリに機密データを格納しないでください](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets)します。

1. ContosoAdsWeb プロジェクトで開き、 *Web.Release.config* アプリケーションの変換ファイル *Web.config* を含むコメント ブロックを削除、 `<connectionStrings>` 要素、および、次のコードをその場所に貼り付けます。

        <connectionStrings>
            <add name="ContosoAdsContext" connectionString="{connectionstring}"
            providerName="System.Data.SqlClient" xdt:Transform="SetAttributes" xdt:Locator="Match(name)"/>
        </connectionStrings>

    ファイルは編集できるように開いたままにしておきます。

2.  [Azure クラシック ポータル](http://manage.windowsazure.com), 、] をクリックして **SQL データベース** 左側のウィンドウでこのチュートリアル用に作成したデータベースをクリックし、 **ダッシュ ボード** タブをクリックし、をクリックし、 **接続文字列の表示**します。

    ![接続文字列の表示](./media/cloud-services-dotnet-get-started/showcs.png)

    接続文字列と、パスワードのプレースホルダーがポータルに表示されます。

    ![接続文字列](./media/cloud-services-dotnet-get-started/connstrings.png)

4.  *Web.Release.config* 削除は、ファイルを変換 `{connectionstring}` と Azure クラシック ポータルからの ADO.NET 接続文字列をその場所に貼り付けます。

5. 貼り付けた接続文字列で、 *Web.Release.config* 変換ファイルで置き換えます `{your_password_here}` 、新しい SQL データベース用に作成したパスワードを使用します。

7. ファイルを保存します。  

6. 以降の worker ロール プロジェクトを構成するための手順で使用する接続文字列を選択してコピーします (引用符は除きます)。

5.  **ソリューション エクスプ ローラー**, [ **ロール** 、クラウド サービス プロジェクトを右クリックして **ContosoAdsWorker** ] をクリックし、 **プロパティ**します。

    ![ロール プロパティ](./media/cloud-services-dotnet-get-started/rolepropertiesworker.png)

6. クリックして、 **設定** ] タブをクリックします。

7. 変更 **サービス構成** に **クラウド**します。

7. `ContosoAdsDbConnectionString` 設定のテキストを選択し、チュートリアルの前のセクションでコピーした接続文字列を貼り付けます。

    ![Database connection string for worker role](./media/cloud-services-dotnet-get-started/workerdbcs.png)

7. 変更を保存します。  

### Azure で実行されたときに Azure ストレージ アカウントを使用するためのソリューションの設定

Web ロール プロジェクトと worker ロール プロジェクトの Azure ストレージ アカウント接続文字列は、クラウド サービス プロジェクトの環境設定に保存されます。 それぞれのプロジェクトには、アプリケーションがローカルに実行されたときに使用される設定とクラウドで実行されたときに使用される設定の別個のセットがあります。 ここでは、Web ロール プロジェクトと worker ロール プロジェクトの両方のクラウド環境設定を更新します。

4.  **ソリューション エクスプ ローラー**, を右クリックして **ContosoAdsWeb** [ **ロール** で、 **ContosoAdsCloudService** [プロジェクト] をクリックして **プロパティ**します。

    ![ロール プロパティ](./media/cloud-services-dotnet-get-started/roleproperties.png)

5. クリックして、 **設定** ] タブをクリックします。  **サービス構成** ドロップダウン ボックスで、選択 **クラウド**します。

    ![クラウドの構成](./media/cloud-services-dotnet-get-started/sccloud.png)

6. 選択、 **StorageConnectionString** エントリ、省略記号が表示されます (**...**) の行の右端にあるボタンをクリックします。 開くには、省略記号ボタンをクリックして、 **ストレージ アカウント接続文字列の作成** ] ダイアログ ボックス。

    ![[接続文字列の作成] ボックスを開く](./media/cloud-services-dotnet-get-started/opencscreate.png)

1.  **ストレージ接続文字列の作成** ダイアログ ボックスで、をクリックして **サブスクリプション**, を事前に作成したストレージ アカウントを選択してクリックして **[ok]**します。 まだログインしていない場合は、Azure アカウントの資格情報を入力するよう求められます。

    ![ストレージ接続文字列の作成
](./media/cloud-services-dotnet-get-started/createstoragecs.png)

1. 変更を保存します。

2. `StorageConnectionString` 接続文字列の場合と同じ手順を使用して、`Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` 接続文字列を設定します。

    この接続文字列は、ログに使用されます。

2. 使用した同じ手順に従って、 **ContosoAdsWeb** 両方の接続文字列を設定するにはロール、 **ContosoAdsWorker** ロールです。 設定することを忘れないでください **サービス構成** に **クラウド**します。

Visual Studio UI を使用して構成したロールの環境設定は、ContosoAdsCloudService プロジェクトの次のファイルに格納されます。

* *ServiceDefinition.csdef* -設定名を定義します。
* *ServiceConfiguration.Cloud.cscfg* -アプリがクラウドで実行されるときの値を提供します。
* *ServiceConfiguration.Local.cscfg* -アプリがローカルで実行されるときの値を提供します。

たとえば、ServiceDefinition.csdef には、次の定義が含まれます。

        <ConfigurationSettings>
          <Setting name="StorageConnectionString" />
          <Setting name="ContosoAdsDbConnectionString" />
        </ConfigurationSettings>

および *ServiceConfiguration.Cloud.cscfg* ファイルには、Visual Studio でこれらの設定に入力した値が含まれています。

        <Role name="ContosoAdsWorker">
          <Instances count="1" />
          <ConfigurationSettings>
            <Setting name="StorageConnectionString" value="{yourconnectionstring}" />
            <Setting name="ContosoAdsDbConnectionString" value="{yourconnectionstring}" />
            <!-- other settings not shown -->
          </ConfigurationSettings>
          <!-- other settings not shown -->
        </Role>

`<Instances>` 設定は、Azure で worker ロール コードが実行される仮想マシンの数を指定します。  [次のステップ](#next-steps) セクションには、クラウド サービスのスケール アウトに関する情報へのリンクが含まれています。

###  Azure にプロジェクトをデプロイする

3.   **ソリューション エクスプ ローラー**, を右クリックし、 **ContosoAdsCloudService** クラウド プロジェクトを選択し、 **発行**します。

    ![[発行] メニュー](./media/cloud-services-dotnet-get-started/pubmenu.png)

2.  **サインイン** のステップ、 **Azure アプリケーションの発行** ウィザード] をクリックして **次**します。

    ![[サインイン] 手順
](./media/cloud-services-dotnet-get-started/pubsignin.png)

3.  **設定** 手順と、ウィザードのをクリックして **次**します。

    ![[設定] 手順](./media/cloud-services-dotnet-get-started/pubsettings.png)

    既定の設定、 **詳細** ] タブはこのチュートリアルで問題ありません。 高度なタブについては、次を参照してください。 [Azure アプリケーションの発行ウィザード](http://msdn.microsoft.com/library/hh535756.aspx)します。

2.  **概要** の手順として、クリックして **発行**します。

    ![[概要] 手順](./media/cloud-services-dotnet-get-started/pubsummary.png)

    **Azure のアクティビティ ログ** Visual Studio でウィンドウが開きます。

2. 右矢印のアイコンをクリックしてデプロイの詳細を展開します。

    デプロイメントが完了するまで 5 分程度かかる場合があります。

    ![[Azure のアクティビティ ログ] ウィンドウ](./media/cloud-services-dotnet-get-started/waal.png)

1. 展開の状態が完了したら、クリックして、 **web サイトの URL** アプリケーションを起動します。

9. この段階で、アプリケーションをローカルで実行したときと同様に、広告を作成、表示、編集して、アプリをテストできます。

>[AZURE.NOTE] 完了したら、テストを削除またはクラウド サービスを停止します。 クラウド サービスを使用していない場合でも仮想マシンのリソースが予約されるため、課金が発生します。 またクラウド サービスを実行したままにしておくと、その URL を見つけた他のユーザーが広告を作成したり表示したりする可能性があります。  [Azure クラシック ポータル](http://manage.windowsazure.com), に移動して、 **ダッシュ ボード** 、クラウド サービス] タブをクリックして、 **削除** ページの下部にあるボタンをクリックします。 一時的に他のユーザーをサイトにアクセスできないように、をクリックするかどうかだけ **停止** 代わりにします。 この場合は、課金が継続されます。 同様の手順で、不要になった SQL データベースとストレージ アカウントを削除できます。

## アプリケーションを最初から作成する

まだ
[完成したアプリケーション](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4), はすぐに実行します。 以降の操作では、ダウンロードしたプロジェクトから新しいプロジェクトにファイルをコピーします。

Contoso Ads アプリケーションを作成するには、次の手順を実行します。

* クラウド サービスの Visual Studio ソリューションを作成します。
* NuGet パッケージを更新して追加します。
* プロジェクト参照を設定します。
* 接続文字列を構成します。
* コード ファイルを追加します。

ソリューションを作成したら、クラウド サービス プロジェクト、Azure BLOB、およびキューに一意のコードを見ていきます。

### クラウド サービス Visual Studio ソリューションの作成

1. Visual Studio で、次のように選択します。 **新しいプロジェクト** から、 **ファイル** メニュー。

2. 左側のウィンドウで、 **新しいプロジェクト** ] ダイアログ ボックスで、展開 **Visual c#** を選択 **クラウド** テンプレートを選択し、 **Azure クラウド サービス** テンプレートです。

3. プロジェクトおよびソリューション ContosoAdsCloudService、名前をクリックして **OK**します。

    ![新しいプロジェクト](./media/cloud-services-dotnet-get-started/newproject.png)

4.  **新しい Azure クラウド サービス** ] ダイアログ ボックスで、web ロールと worker ロールを追加します。 Web ロールに ContosoAdsWeb という名前を付け、worker ロールに ContosoAdsWorker という名前を付けます (右側のウィンドウの鉛筆アイコンを使用して、ロールの既定の名前を変更します)。

    ![新しいクラウド サービス プロジェクト](./media/cloud-services-dotnet-get-started/newcsproj.png)

5. 表示されている場合、 **新しい ASP.NET プロジェクト** web ロールのためのダイアログ ボックスが、MVC テンプレートを選択し、クリックして **認証の変更**します。

    ![認証の変更](./media/cloud-services-dotnet-get-started/chgauth.png)

7.  **認証の変更** ] ダイアログ ボックスで、選択 **認証なし**, 、] をクリックし、 **OK**します。

    ![[認証なし]](./media/cloud-services-dotnet-get-started/noauth.png)

8.  **新しい ASP.NET プロジェクト** ダイアログ ボックスで、をクリックして **OK**します。

9.  **ソリューション エクスプ ローラー**, (いないプロジェクトの 1 つ、)、ソリューションを右クリックし、選択、 **追加]、[新しいプロジェクト**します。

11.  **新しいプロジェクトの追加** ] ダイアログ ボックスで、選択 **Windows デスクトップ** [ **Visual c#** 左側のウィンドウをクリックし、 **クラス ライブラリ** テンプレートです。  

10. プロジェクトに名前を *ContosoAdsCommon*, 、] をクリックし、 **OK**します。

    Web ロール プロジェクトと worker ロール プロジェクトの両方から Entity Framework のコンテキストとデータ モデルを参照する必要があります。 代わりの方法として、Entity Framework に関連するクラスを Web ロール プロジェクトに定義して、worker ロール プロジェクトからそのプロジェクトを参照することもできます。 ただし別の方法では、worker ロール プロジェクトに、必要としない Web アセンブリへの参照が含まれることになります。

### NuGet パッケージの更新および追加

11. 開いている、 **NuGet パッケージの管理** ソリューションのダイアログ ボックス。

12. 左側のウィンドウで [ **更新**します。

13. 検索、 *WindowsAzure.Storage* パッケージ化、および一覧にある場合にをクリックして **更新** ストレージ クライアント ライブラリの最新バージョンを取得します。

    ![Update SCL](./media/cloud-services-dotnet-get-started/updstg.png)

    ストレージ クライアント ライブラリは Visual Studio プロジェクト テンプレートよりも頻繁に更新されるため、新しく作成したプロジェクトのバージョンの更新が必要になることがよくあります。

14. 左側のウィンドウで [ **オンライン**します。

16. 検索、 *EntityFramework* NuGet パッケージを見つけて次の 3 つのプロジェクトすべてにインストールします。

17. 検索、 *Microsoft.WindowsAzure.ConfigurationManager* NuGet パッケージを見つけて、ワーカー ロール プロジェクトにインストールします。

### プロジェクト参照の設定

10. ContosoAdsWeb プロジェクトで、ContosoAdsCommon プロジェクトの参照を設定します。 ContosoAdsWeb プロジェクトを右クリックし、 **参照** - **参照の追加**します。  **参照マネージャー** ダイアログ ボックスで、 **ソリューション-プロジェクト** 左ペインで選択 **ContosoAdsCommon**, 、順にクリック **OK**します。

11. ContosoAdsWorker プロジェクトで、ContosAdsCommon プロジェクトの参照を設定します。

    ContosoAdsCommon は、Entity Framework データ モデルおよびコンテキスト クラスを含みます。これらは、フロントエンドとバックエンドの両方で使用されます。

11. ContosoAdsWorker プロジェクトで、`System.Drawing` プロジェクトの参照を設定します。

    このアセンブリは、画像を縮小表示に変換するためにバックエンドで使用されます。

### 接続文字列の構成

このセクションでは、Azure Storage および SQL 接続文字列をローカルでのテスト用に構成します。 チュートリアルの前半で紹介したデプロイ手順では、アプリケーションをクラウドで実行する場合の接続文字列の設定方法について説明しました。

3. ContosoAdsWeb プロジェクトで、アプリケーションの Web.config ファイルを開き、`connectionStrings` 要素を `configSections` 要素の後に挿入します。

        <connectionStrings>
          <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
        </connectionStrings>

3. 変更を保存します。

2. ContosoAdsCloudService プロジェクトの右下の [ContosoAdsWeb **ロール**, 、] をクリックし、 **プロパティ**します。

    ![ロール プロパティ](./media/cloud-services-dotnet-get-started/roleproperties.png)

3.  **ContosAdsWeb [ロール]** プロパティ] ウィンドウでをクリックして、 **設定** タブをクリックし、をクリックして **設定の追加**します。

    ままにして **サービス構成** 設定 **すべての構成**します。

4. 名前付き新しい設定を追加 *StorageConnectionString*します。 設定 **型** に *ConnectionString*, 、設定と **値** に *UseDevelopmentStorage = true*します。

    ![新しい接続文字列](./media/cloud-services-dotnet-get-started/scall.png)

5. 変更を保存します。

3. 同じ手順を実行して、ストレージの接続文字列を ContosoAdsWorker ロール プロパティに追加します。

5. 引き続き、 **ContosoAdsWorker [Role]** プロパティ] ウィンドウで別の接続文字列を追加します。

    * 名前: ContosoAdsDbConnectionString
    * 型: 文字列
    * 値: Web ロール プロジェクトに使用したのと同じ接続文字列を貼り付けます。

            Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;

### コード ファイルの追加

このセクションでは、ダウンロードしたソリューションから新しいソリューションにコード ファイルをコピーします。 以降のセクションでは、このコードの重要な部分について説明します。

プロジェクトまたはフォルダーにファイルを追加するにプロジェクトまたはフォルダーを右クリックし、クリックして **追加** - **既存項目の**です。 をクリックしてファイルを選択 **追加**します。 既存のファイルを置換するかどうかをクリックして確認された場合は **はい**します。

3. ContosoAdsCommon プロジェクトで、削除、 *Class1.cs* ファイルし、その場所に追加、 *Ad.cs* と *ContosoAdscontext.cs* 、ダウンロードしたプロジェクトからのファイルです。

3. ContosoAdsWeb プロジェクトで、ダウンロードしたプロジェクトから次のファイルを追加します。
    - *Global.asax.cs*します。  
    -  *Views \shared* フォルダー: <em>\_Layout.cshtml</em>します。
    -  *Views \home* フォルダー: *Index.cshtml*します。
    -  *コント ローラー* フォルダー: *AdController.cs*します。
    -  *Views \ad* フォルダー (最初にフォルダーを作成): 5 つ *.cshtml* ファイルです。

3. ContosoAdsWorker プロジェクトで追加 *WorkerRole.cs* 、ダウンロードしたプロジェクトからです。

この段階で、チュートリアルの前の説明で示した手順に従ってアプリケーションをビルドして実行できます。アプリは、ローカル データベースおよびストレージ エミュレーター リソースを使用します。

以降のセクションでは、Azure 環境、BLOB、およびキューの操作に関連するコードについて説明します。 このチュートリアルでは、スキャフォールディングを使用した MVC コントローラーおよびビューの作成方法、SQL Server データベースで動作する Entity Framework コードの作成方法、ASP.NET 4.5 での非同期プログラミングの基礎については説明していません。 これらのトピックについては、次のリソースを参照してください。

* [MVC 5 を使ってみる](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [EF 6 と MVC 5 を使ってみる](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc)
* [.NET 4.5 で非同期プログラミングの概要](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices#async)します。

### ContosoAdsCommon - Ad.cs

Ad.cs ファイルは、広告カテゴリの enum と広告情報の POCO エンティティ クラスを定義します。

        public enum Category
        {
            Cars,
            [Display(Name="Real Estate")]
            RealEstate,
            [Display(Name = "Free Stuff")]
            FreeStuff
        }

        public class Ad
        {
            public int AdId { get; set; }

            [StringLength(100)]
            public string Title { get; set; }

            public int Price { get; set; }

            [StringLength(1000)]
            [DataType(DataType.MultilineText)]
            public string Description { get; set; }

            [StringLength(1000)]
            [DisplayName("Full-size Image")]
            public string ImageURL { get; set; }

            [StringLength(1000)]
            [DisplayName("Thumbnail")]
            public string ThumbnailURL { get; set; }

            [DataType(DataType.Date)]
            [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
            public DateTime PostedDate { get; set; }

            public Category? Category { get; set; }
            [StringLength(12)]
            public string Phone { get; set; }
        }

### ContosoAdsCommon - ContosoAdsContext.cs

ContosoAdsContext クラスは、Entity Framework によって SQL データベースに格納される DbSet コレクションで Ad クラスを使用することを指定します。

        public class ContosoAdsContext : DbContext
        {
            public ContosoAdsContext() : base("name=ContosoAdsContext")
            {
            }
            public ContosoAdsContext(string connString)
                : base(connString)
            {
            }
            public System.Data.Entity.DbSet<Ad> Ads { get; set; }
        }

このクラスには 2 つのコンストラクターがあります。 1 つ目のコンストラクターは Web プロジェクトによって使用され、Web.config ファイルに格納される接続文字列の名前を指定します。 2 つ目のコンストラクターは、実際の接続文字列を渡すために使用します。 これは、worker ロール プロジェクトで必要になります。その理由は、worker ロール プロジェクトが Web.config ファイルを持たないためです。 この接続文字列がどこに格納されるかについては既に説明しました。後のセクションで、DbContext クラスを初期化するときに接続文字列がどのように取得されるかについて説明します。

### ContosoAdsWeb - Global.asax.cs

コードから呼び出される、 `Application_Start` メソッドは、作成、 *イメージ* blob コンテナーおよび *イメージ* キューが既に存在していない場合。 これにより、新しいストレージ アカウントの使用を開始するたびに、または新しいコンピューターでストレージ エミュレーターの使用を開始するたびに、必要な BLOB コンテナーとキューが自動的に作成されます。

コードからストレージ接続文字列を使用してストレージ アカウントへのアクセスの取得、 *.cscfg* ファイルです。

        var storageAccount = CloudStorageAccount.Parse
            (RoleEnvironment.GetConfigurationSettingValue("StorageConnectionString"));

参照を取得し、 *イメージ* コンテナーの blob コンテナーが作成され、まだ存在しないし、アクセス、新しいコンテナーのアクセス許可を設定します。 既定では、新しいコンテナーは、ストレージ アカウント資格情報を持つクライアントのみに BLOB へのアクセスを許可します。 Web サイトで画像 BLOB を指し示す URL を使用して画像を表示できるようにするには、BLOB をパブリックに設定する必要があります。

        var blobClient = storageAccount.CreateCloudBlobClient();
        var imagesBlobContainer = blobClient.GetContainerReference("images");
        if (imagesBlobContainer.CreateIfNotExists())
        {
            imagesBlobContainer.SetPermissions(
                new BlobContainerPermissions
                {
                    PublicAccess =BlobContainerPublicAccessType.Blob
                });
        }

似たようなコードへの参照を取得する、 *イメージ* キューにして、新しいキューを作成します。 この場合、アクセス許可の変更は必要ありません。

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        var imagesQueue = queueClient.GetQueueReference("images");
        imagesQueue.CreateIfNotExists();

### ContosoAdsWeb - \_Layout.cshtml

 *_Layout.cshtml* ファイルは、ヘッダーとフッターをアプリの名前を設定し、"Ads"メニュー エントリを作成します。

### ContosoAdsWeb - Views\Home\Index.cshtml

 *Views\Home\Index.cshtml* ファイルは、ホーム ページにカテゴリ リンクを表示します。 リンクは、querystring 変数の `Category` enum の整数値を Ads Index ページに渡します。

        <li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
        <li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
        <li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
        <li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>

### ContosoAdsWeb - AdController.cs

 *AdController.cs* ファイルのコンス トラクターによって、 `InitializeStorage` blob およびキューを操作するための API を提供する Azure ストレージ クライアント ライブラリ オブジェクトを作成します。

コードへの参照を取得し、 *イメージ* blob コンテナーの前に見たよう *Global.asax.cs*します。 処理中に、既定値を設定して [再試行ポリシー](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) web アプリに適したします。 既定の指数関数的バックオフ再試行ポリシーでは、一時的な障害に対する反復的再試行操作によって Web アプリが 1 分以上停止する可能性があります。 ここでは、再試行ごとに 3 秒待機し、再試行の回数を 3 回までとする再試行ポリシーを指定しています。

        var blobClient = storageAccount.CreateCloudBlobClient();
        blobClient.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesBlobContainer = blobClient.GetContainerReference("images");

似たようなコードへの参照を取得する、 *イメージ* キューです。

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        queueClient.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesQueue = queueClient.GetQueueReference("images");

コントローラー コードのほとんどは、DbContext クラスを使用した Entity Framework データ モデルの操作に典型的なものです。 ただし、ファイルをアップロードして Blob Storage に保存する HttpPost `Create` メソッドは例外です。 モデル バインダーは、 [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) メソッド オブジェクト。

        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> Create(
            [Bind(Include = "Title,Price,Description,Category,Phone")] Ad ad,
            HttpPostedFileBase imageFile)

ユーザーがアップロードするファイルを選択すると、ファイルがアップロードされて BLOB に保存されます。さらに、Ad データベース レコードが更新され、その BLOB を示す URL が反映されます。

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            blob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = blob.Uri.ToString();
        }

アップロード操作を実行するコードは `UploadAndSaveBlobAsync` メソッドに含まれています。 このコードでは、BLOB の GUID 名が作成され、ファイルがアップロードおよび保存され、保存された BLOB への参照が返されます。

        private async Task<CloudBlockBlob> UploadAndSaveBlobAsync(HttpPostedFileBase imageFile)
        {
            string blobName = Guid.NewGuid().ToString() + Path.GetExtension(imageFile.FileName);
            CloudBlockBlob imageBlob = imagesBlobContainer.GetBlockBlobReference(blobName);
            using (var fileStream = imageFile.InputStream)
            {
                await imageBlob.UploadFromStreamAsync(fileStream);
            }
            return imageBlob;
        }

HttpPost `Create` メソッドは、BLOB をアップロードしてデータベースを更新した後、画像を縮小表示に変換する準備が整ったことをバックエンド プロセスに通知するキュー メッセージを作成します。

        string queueMessageString = ad.AdId.ToString();
        var queueMessage = new CloudQueueMessage(queueMessageString);
        await queue.AddMessageAsync(queueMessage);

HttpPost `Edit` メソッドのコードも似ていますが、ユーザーが新しい画像ファイルを選択した場合に既に存在している BLOB を削除する必要がある点が異なります。

        if (imageFile != null && imageFile.ContentLength != 0)
        {
            await DeleteAdBlobsAsync(ad);
            imageBlob = await UploadAndSaveBlobAsync(imageFile);
            ad.ImageURL = imageBlob.Uri.ToString();
        }

次の例で示すのは、広告が削除されたときに BLOB を削除するコードです。

        private async Task DeleteAdBlobsAsync(Ad ad)
        {
            if (!string.IsNullOrWhiteSpace(ad.ImageURL))
            {
                Uri blobUri = new Uri(ad.ImageURL);
                await DeleteAdBlobAsync(blobUri);
            }
            if (!string.IsNullOrWhiteSpace(ad.ThumbnailURL))
            {
                Uri blobUri = new Uri(ad.ThumbnailURL);
                await DeleteAdBlobAsync(blobUri);
            }
        }
        private static async Task DeleteAdBlobAsync(Uri blobUri)
        {
            string blobName = blobUri.Segments[blobUri.Segments.Length - 1];
            CloudBlockBlob blobToDelete = imagesBlobContainer.GetBlockBlobReference(blobName);
            await blobToDelete.DeleteAsync();
        }

### ContosoAdsWeb - Views\Ad\Index.cshtml および Details.cshtml

 *Index.cshtml* ファイルが他の広告データと共に表示されます。

        <img  src="@Html.Raw(item.ThumbnailURL)" />

 *Details.cshtml* ファイルは、フルサイズの画像を表示します。

        <img src="@Html.Raw(Model.ImageURL)" />

### ContosoAdsWeb - Views\Ad\Create.cshtml および Edit.cshtml

 *Create.cshtml* と *Edit.cshtml* ファイルは、コント ローラーを取得できるようにするフォームのエンコードを指定、 `HttpPostedFileBase` オブジェクトです。

        @using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))

`<input>` 要素は、ファイル選択ダイアログを表示するようにブラウザーに指示します。

        <input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />

### ContosoAdsWorker - WorkerRole.cs - OnStart メソッド

Azure worker ロール環境は、worker ロールの起動時に `WorkerRole` クラスの `OnStart` メソッドを呼び出し、`OnStart` メソッドの終了時に `Run` メソッドを呼び出します。

 `OnStart` メソッドからデータベース接続文字列を取得、 *.cscfg* ファイルし、Entity Framework の DbContext クラスに渡します。 既定で SQLClient プロバイダーが使用されるため、プロバイダーを指定する必要はありません。

        var dbConnString = CloudConfigurationManager.GetSetting("ContosoAdsDbConnectionString");
        db = new ContosoAdsContext(dbConnString);

次に、ストレージ アカウントへの参照が取得され、BLOB コンテナーおよびキューが存在しない場合はこれらが作成されます。 この操作を行うためのコードは、Web ロールの `Application_Start` メソッドで示したコードに似ています。

### ContosoAdsWorker - WorkerRole.cs - Run メソッド

`Run` メソッドは、`OnStart` メソッドが初期化操作を終了するときに呼び出されます。 このメソッドは、新しいキュー メッセージを監視する無限ループを実行して、キュー メッセージが到着したときにそれを処理します。

        public override void Run()
        {
            CloudQueueMessage msg = null;

            while (true)
            {
                try
                {
                    msg = this.imagesQueue.GetMessage();
                    if (msg != null)
                    {
                        ProcessQueueMessage(msg);
                    }
                    else
                    {
                        System.Threading.Thread.Sleep(1000);
                    }
                }
                catch (StorageException e)
                {
                    if (msg != null && msg.DequeueCount > 5)
                    {
                        this.imagesQueue.DeleteMessage(msg);
                    }
                    System.Threading.Thread.Sleep(5000);
                }
            }
        }

ループの各イテレーションの後でキュー メッセージが見つからなかった場合、プログラムは 1 秒間スリープ状態になります。 これにより、worker ロールによって過剰な CPU 時間およびストレージ トランザクション コストが発生しないようにしています。 Microsoft Customer Advisory Team は、運用環境にデプロイし、休暇に必ず含める、忘れている開発者の概要、ストーリーに指示します。 開発者が休暇から戻ると、その不注意によって休暇分を超える損失が生じていました。

ときには、キュー メッセージの内容が原因で処理エラーが生じることがあります。 これと呼ばれますが、 *有害なメッセージ*, 、だけ、エラーを記録してループを再開、でした際限なく繰り返されるときにメッセージを処理します。  そこで、catch ブロックに if ステートメントを含めて、アプリが現在のメッセージを処理しようと試みた回数を調べ、その回数が 5 回を超えた場合はメッセージをキューから削除するようにしています。

`ProcessQueueMessage` は、キュー メッセージが見つかったときに呼び出されます。

        private void ProcessQueueMessage(CloudQueueMessage msg)
        {
            var adId = int.Parse(msg.AsString);
            Ad ad = db.Ads.Find(adId);
            if (ad == null)
            {
                throw new Exception(String.Format("AdId {0} not found, can't create thumbnail", adId.ToString()));
            }

            CloudBlockBlob inputBlob = this.imagesBlobContainer.GetBlockBlobReference(ad.ImageURL);

            string thumbnailName = Path.GetFileNameWithoutExtension(inputBlob.Name) + "thumb.jpg";
            CloudBlockBlob outputBlob = this.imagesBlobContainer.GetBlockBlobReference(thumbnailName);

            using (Stream input = inputBlob.OpenRead())
            using (Stream output = outputBlob.OpenWrite())
            {
                ConvertImageToThumbnailJPG(input, output);
                outputBlob.Properties.ContentType = "image/jpeg";
            }

            ad.ThumbnailURL = outputBlob.Uri.ToString();
            db.SaveChanges();

            this.imagesQueue.DeleteMessage(msg);
        }

このコードには、データベースを読み取って画像の URL を取得する操作、画像を縮小表示に変換する操作、縮小表示を BLOB に保存する操作、縮小表示 BLOB の URL を反映するようにデータベースを更新する操作、およびキュー メッセージを削除する操作が含まれています。

>[AZURE.NOTE] 内のコード、 `ConvertImageToThumbnailJPG` メソッドは、わかりやすくするため、System.Drawing 名前空間でクラスを使用します。 ただし、この名前空間のクラスは Windows フォーム用に設計されています。 これらのクラスは、Windows または ASP.NET サービスでの使用に関してサポートされていません。 イメージの処理オプションの詳細については、次を参照してください。 [イメージの動的生成](http://www.hanselman.com/blog/BackToBasicsDynamicImageGenerationASPNETControllersRoutingIHttpHandlersAndRunAllManagedModulesForAllRequests.aspx) と [深いイメージのサイズ変更](http://www.hanselminutes.com/313/deep-inside-image-resizing-and-scaling-with-aspnet-and-iis-with-imageresizingnet-author-na)します。

## トラブルシューティング

このチュートリアルの手順に従って作業しているときに発生する可能性のある一般的なエラーとその解決方法について説明します。

### ServiceRuntime.RoleEnvironmentException

`RoleEnvironment` オブジェクトは、アプリケーションを Azure で実行したときまたは Azure コンピューティング エミュレーターを使用してローカルで実行したときに Azure によって提供されます。  アプリケーションをローカルで実行しているときにこのエラーが発生した場合は、ContosoAdsCloudService プロジェクトがスタートアップ プロジェクトとして設定されていることを確認してください。 こうすることで、Azure コンピューティング エミュレーターを使用して実行するようにプロジェクトが設定されます。

接続に格納されている文字列値を取得するには、アプリケーションの Azure RoleEnvironment を使用することの 1 つ、 *.cscfg* ファイルをこの例外の別の原因が不足している接続文字列であるためです。 ContosoAdsWeb プロジェクトのクラウド構成とローカル構成の両方に対して StorageConnectionString 設定が作成されていること、および ContosoAdsWorker プロジェクトの両方の構成に対して両方の接続文字列が作成されていることを確認してください。 構成した場合、 **すべて検索** 検索ソリューション全体で StorageConnectionString、するものが表示に 9 件で 6 つのファイルです。

### ポート xxx にオーバーライドできません。 新しいポートが http プロトコルで許容されている最小値 8080 を下回っています

Web プロジェクトで使用するポート番号を変更してください。 ContosoAdsWeb プロジェクトを右クリックし、 **プロパティ**します。 クリックして、 **Web** タブをクリックし、[ポート番号を変更、 **プロジェクト Url** 設定します。

この問題を解決する別の方法としては、次のセクションを参照してください。

### ローカルで実行しているときに発生するその他のエラー

既定では、新しいクラウド サービス プロジェクトは、Azure コンピューティング エミュレーター Express を使用して Azure 環境をシミュレートします。 これはフル装備のコンピューティング エミュレーターの軽量バージョンです。フル装備のエミュレーターは、Express バージョンが動作しない特定の条件でも動作します。  

フル装備のエミュレーターを使用するプロジェクトを変更するには、ContosoAdsCloudService プロジェクトを右クリックし、順にクリックして **プロパティ**します。  **プロパティ** ウィンドウをクリックして、 **Web** タブをクリックし、をクリックして、 **Full Emulator を使用** オプション ボタンをクリックします。

フル装備のエミュレーターを使用してアプリケーションを実行するには、管理者特権で Visual Studio を開く必要があります。

## 次のステップ

Contoso Ads アプリケーションは、入門用のチュートリアルという目的から意図的にシンプルに作られています。 実装されていないなど [依存関係の注入](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) または [作業パターンのリポジトリと単位](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), 、しない [ログ記録用のインターフェイスを使用](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), を使用しない [EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) データ モデルの変更を管理するまたは [EF 接続の回復性](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) 一時的なネットワーク エラーを管理するためです。

以下に、より現実的なコーディング手法を示すいくつかのクラウド サービス サンプル アプリケーションを、やさしいものから順に示します。

* [PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31)します。 概念は Contoso Ads に似ていますが、より多くの機能とより現実的なコーディング手法が実装されています。
* [Azure クラウド サービス多層アプリケーションを使用するテーブル Queues, and Blobs](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36)します。 Azure のストレージ テーブル、blob、キューを紹介し、付属の [ステップ バイ ステップのチュートリアル シリーズ](../cloud-services-dotnet-multi-tier-app-storage-1-overview.md)します。
* [Microsoft Azure サービスの基礎をクラウド](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)します。 Microsoft Patterns and Practices グループから提供された、さまざまなベスト プラクティスを紹介する包括的なサンプルです。

クラウド向けの開発の詳細については、次を参照してください。 [Azure での実際のクラウド アプリケーションのビルド](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction)します。

Azure Storage に関するベスト プラクティスとパターンを紹介ビデオについては、次を参照してください。 [Microsoft Azure Storage – 新規、ベスト プラクティスおよびパターンは何](http://channel9.msdn.com/Events/Build/2014/3-628)します。

詳細については、次のリソースを参照してください。

* [Azure Cloud Services Part 1: Introduction (Azure Cloud Services パート 1: 概要)](http://justazure.com/microsoft-azure-cloud-services-part-1-introduction/)
* [Cloud Services の管理方法](cloud-services-how-to-manage.md)
* [Azure Storage (Azure Storage)](/documentation/services/storage/)

