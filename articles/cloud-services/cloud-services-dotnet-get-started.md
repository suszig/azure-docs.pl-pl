<properties
    pageTitle="Azure Cloud Services と ASP.NET を使ってみる | Microsoft Azure"
    description="ASP.NET MVC と Azure を使用して多層アプリケーションを作成する方法について説明します。ここで取り上げるアプリケーションは、クラウド サービス内で、Web ロールと worker ロールと連係して動作します。Entity Framework、SQL Database、Azure Storage のキューと BLOB を使用しています。"
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

アプリケーションを使用して、 [キューを中心とした作業パターン](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern) バックエンド プロセスに縮小表示の作成の CPU 負荷の高い作業をオフロードします。

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

このチュートリアルでは、理解している [基本的な概念については、Azure クラウド サービス](fundamentals-application-models.md#CloudServices) など *web ロール* と *ワーカー ロール* 用語です。 使用する方法を知っているまた [ASP.NET MVC](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started) または [Web フォーム](http://www.asp.net/web-forms/tutorials/aspnet-45/getting-started-with-aspnet-45-web-forms/introduction-and-overview) Visual Studio のプロジェクトです。 サンプル アプリケーションでは MVC を使用しますが、チュートリアルのほとんどは Web フォームにも当てはまります。

Azure サブスクリプションがなくてもアプリケーションをローカルに実行できますが、アプリケーションをクラウドにデプロイするには Azure サブスクリプションが必要になります。 アカウントを持っていない場合は、 [MSDN サブスクライバーの特典をアクティブ化](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A55E3C668) または [無料評価版にサインアップ](/pricing/free-trial/?WT.mc_id=A55E3C668)します。

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

3. **[ファイル]** メニューの **[プロジェクトを開く]** を選択し、ソリューションをダウンロードした場所に移動して、ソリューション ファイルを開きます。

3. Ctrl + Shift + B キーを押して、ソリューションをビルドします。

    既定では、*.zip* ファイルに含まれていなかった NuGet パッケージのすべての内容が Visual Studio によって自動的に復元されます。 パッケージが復元されない場合は、**[ソリューションの NuGet パッケージの管理]** ダイアログ ボックスの右上にある **[復元]** ボタンをクリックしてパッケージを手動でインストールします。

3. **ソリューション エクスプローラー**で、**ContosoAdsCloudService** がスタートアップ プロジェクトとして選択されていることを確認します。

1. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

    クラウド サービス プロジェクトをローカルで実行すると、Visual Studio によって Azure *コンピューティング エミュレーター*と Azure *ストレージ エミュレーター*が自動的に起動されます。 コンピューティング エミュレーターは、コンピューターのリソースを使用して、Web ロール環境と worker ロール環境をシミュレートします。 ストレージ エミュレーターを使用して、 [SQL Server Express LocalDB](http://msdn.microsoft.com/library/hh510202.aspx) Azure クラウド ストレージをシミュレートします。

    クラウド サービス プロジェクトを初めて実行するときは、エミュレーターが起動されるまで 1 分程度かかります。 エミュレーターの起動が完了すると、既定のブラウザーが開き、アプリケーションのホーム ページが表示されます。

    ![Contoso Ads architecture](./media/cloud-services-dotnet-get-started/home.png)

2. クリックして  **Create an Ad**します。

2. 何らかのテスト データを入力し、アップロードする画像 (*.jpg*) を選択したら、**[Create]** をクリックします。

    ![Create page](./media/cloud-services-dotnet-get-started/create.png)

    [Index] ページが表示されますが、新しい広告の処理が完了していないため、縮小表示はまだ表示されません。

3. しばらく待ってから [Index] ページを更新すると、縮小表示が表示されます。

    ![Index ページ](./media/cloud-services-dotnet-get-started/list.png)

4. 広告に対応する **[Details]** をクリックすると、フルサイズの画像が表示されます。

    ![Details page](./media/cloud-services-dotnet-get-started/details.png)

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

2. **[Compute]、[コンピューティング]、[クラウド サービス]、[簡易作成]** の順にクリックします。

4. [URL] 入力ボックスに、URL プレフィックスを入力します。

    この URL は一意である必要があります。 選択したプレフィックスが既に他のユーザーによって使用されている場合は、エラー メッセージが表示されます。

5. アプリケーションをデプロイするリージョンを選択します。

    このフィールドは、クラウド サービスがホストされるデータセンターを指定します。 運用アプリケーションの場合は、顧客に最も近いリージョンを選択してください。 このチュートリアルでは、自分に最も近いリージョンを選択してください。

6. **[クラウド サービスを作成する]** をクリックします。

    次の画像では、クラウド サービスは contosoads.cloudapp.net という URL で作成されます。

    ![新しいクラウド サービス](./media/cloud-services-dotnet-get-started/newcs.png)

### Azure SQL データベースの作成

アプリをクラウドで実行すると、クラウドベースのデータベースがアプリによって使用されます。

1. [Azure クラシック ポータル](http://manage.windowsazure.com), 、クリックして **新規 > Data Services > SQL データベース > 簡易作成**します。

1. **[データベース名]** ボックスに「*contosoads*」と入力します。

1. **[サーバー]** ボックスの一覧の **[新しい SQL データベース サーバー]** を選択します。

    または、サブスクリプションに既にサーバーが設定されている場合は、ドロップダウン リストからそのサーバーを選択します。

1. **[リージョン]** で、クラウド サービスに選択したリージョンと同じリージョンを選択します。

    クラウド サービスとデータベースが異なるデータ センター (異なるリージョン) にあると、遅延時間が長くなり、データ センター外の帯域幅に対して課金されることになります。 データ センター内の帯域幅は無料です。

1. 管理者の**ログイン名**と**パスワード**を入力します。

    **[新しい SQL データベース サーバー]** を選択した場合は、既存の名前とパスワードではなく、このデータベースへのアクセス時に使用する新しい名前とパスワードを入力してください。 以前に作成したサーバーを選択した場合は、既に作成されている管理ユーザー アカウントのパスワードを入力します。

1. **[SQL データベースの作成]** をクリックします。

    ![New SQL Database](./media/cloud-services-dotnet-get-started/newdb.png)

1. Azure によってデータベースが作成されたら、ポータルの左側のウィンドウにある **[SQL データベース]** タブをクリックし、新しいデータベースの名前をクリックします。

2. **[ダッシュボード]** タブをクリックします。

3. **[使用できる IP アドレスの管理]** をクリックします。

4. **[使用できるサービス]** で、**[Azure サービス]** を **[はい]** に変更します。

5. **[保存]** をクリックします。

### Azure のストレージ アカウントの作成

Azure ストレージ アカウントは、キューおよび BLOB データをクラウドに格納するためのリソースを提供します。

現実のアプリケーションでは、通常、アプリケーション データとログ データ、テスト データと運用データに別個のアカウントを作成します。 このチュートリアルでは、アカウントを 1 つだけ使用します。

1. [Azure クラシック ポータル](http://manage.windowsazure.com), 、クリックして **新規 > Data Services > ストレージ > 簡易作成**します。

4. **[URL]** ボックスに、URL プレフィックスを入力します。

    このプレフィックスに、このボックスの下に表示されているテキストを連結したものが、ストレージ アカウントに固有の URL になります。 入力したプレフィックスが既に他のユーザーによって使用されている場合は、別のプレフィックスを選択する必要があります。

5. **[リージョン]** ボックスの一覧で、クラウド サービスに選択したリージョンと同じリージョンを選択します。

    クラウド サービスとストレージ アカウントが異なるデータ センター (異なるリージョン) にあると、遅延時間が長くなり、データ センター外の帯域幅に対して課金されることになります。 データ センター内の帯域幅は無料です。

    Azure アフィニティ グループには、データ センター内のリソース間の距離を最短にする機能が備わっており、それにより遅延時間を短縮できます。 このチュートリアルでは、アフィニティ グループを使用しません。 詳細については、次を参照してください。 [Azure でアフィニティ グループを作成する方法](http://msdn.microsoft.com/library/jj156209.aspx)します。

6. **[レプリケーション]** ボックスを **[ローカル冗長]** に設定します。

    Geo レプリケーションをストレージ アカウントに対して有効にすると、1 次拠点で重大な障害が発生した場合に備えて、保存したコンテンツは 2 次データセンターに複製されて、フェイルオーバーが可能になります。 Geo レプリケーションには追加費用が発生する場合があります。 また、テストおよび開発アカウントの場合は、一般的に Geo レプリケーションに対する課金は避けたいと考えるでしょう。 詳細については、次を参照してください。 [の作成、管理、またはストレージ アカウントを削除](../storage-create-storage-account/#replication-options)します。

5. **[ストレージ アカウントの作成]** をクリックします。

    ![New storage account](./media/cloud-services-dotnet-get-started/newstorage.png)

    URL を使用して、イメージのストレージ アカウントが作成された `contosoads.core.windows.net`します。

### Azure で実行されたときに Azure SQL データベースを使用するためのソリューションの設定

Web プロジェクトと worker ロール プロジェクトはそれぞれ独自のデータベース接続文字列を持ち、アプリが Azure で実行されたときにそれぞれが Azure SQL データベースを指し示す必要があります。

使用して、 [Web.config の変換](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) web ロールとワーカー ロールの設定、クラウド サービス環境です。
>[AZURE.NOTE] このセクションと次のセクションでは、資格情報をプロジェクト ファイルに格納します。 [は公開されているソース コード リポジトリに機密データを格納しないでください](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control#secrets)します。

1. ContosoAdsWeb プロジェクトで開き、 *Web.Release.config* アプリケーションの変換ファイル *Web.config* を含むコメント ブロックを削除、 `< connectionStrings >` 要素、および、次のコードをその場所に貼り付けます。

        <connectionStrings>
            <add name="ContosoAdsContext" connectionString="{connectionstring}"
            providerName="System.Data.SqlClient" xdt:Transform="SetAttributes" xdt:Locator="Match(name)"/>
        </connectionStrings>

    ファイルは編集できるように開いたままにしておきます。

2. [Azure クラシック ポータル](http://manage.windowsazure.com), 、] をクリックして **SQL データベース** 左側のウィンドウでこのチュートリアル用に作成したデータベースをクリックし、 **ダッシュ ボード** タブをクリックし、をクリックし、 **接続文字列の表示**します。

    ![接続文字列の表示](./media/cloud-services-dotnet-get-started/showcs.png)

    接続文字列と、パスワードのプレースホルダーがポータルに表示されます。

    ![Connection strings](./media/cloud-services-dotnet-get-started/connstrings.png)

4. *Web.Release.config* 削除は、ファイルを変換 `{connectionstring}` と Azure クラシック ポータルからの ADO.NET 接続文字列をその場所に貼り付けます。

5. 貼り付けた接続文字列で、 *Web.Release.config* 変換ファイルで置き換えます `{your_password_here}` 、新しい SQL データベース用に作成したパスワードを使用します。

7. ファイルを保存します。

6. 以降の worker ロール プロジェクトを構成するための手順で使用する接続文字列を選択してコピーします (引用符は除きます)。

5. **ソリューション エクスプローラー**で、該当するクラウド サービス プロジェクトの **[ロール]** の下の **[ContosoAdsWorker]** を右クリックし、**[プロパティ]** をクリックします。

    ![ロール プロパティ](./media/cloud-services-dotnet-get-started/rolepropertiesworker.png)

6. **[設定]** タブをクリックします。

7. **[サービス構成]** を **[クラウド]** に変更します。

7. テキストを選択、 `ContosoAdsDbConnectionString` を設定して、このチュートリアルの前のセクションからコピーした接続文字列を貼り付けます。

    ![Database connection string for worker role](./media/cloud-services-dotnet-get-started/workerdbcs.png)

7. 変更を保存します。

### Azure で実行されたときに Azure ストレージ アカウントを使用するためのソリューションの設定

Web ロール プロジェクトと worker ロール プロジェクトの Azure ストレージ アカウント接続文字列は、クラウド サービス プロジェクトの環境設定に保存されます。 それぞれのプロジェクトには、アプリケーションがローカルに実行されたときに使用される設定とクラウドで実行されたときに使用される設定の別個のセットがあります。 ここでは、Web ロール プロジェクトと worker ロール プロジェクトの両方のクラウド環境設定を更新します。

4. **ソリューション エクスプローラー**で、**[ContosoAdsCloudService]** プロジェクトの **[ロール]** の下にある **[ContosoAdsWeb]** を右クリックし、**[プロパティ]** をクリックします。

    ![ロール プロパティ](./media/cloud-services-dotnet-get-started/roleproperties.png)

5. **[設定]** タブをクリックします。 **[サービス構成]** ボックスの一覧の **[クラウド]** を選択します。

 ![クラウドの構成
](./media/cloud-services-dotnet-get-started/sccloud.png)

6. **StorageConnectionString** エントリを選択すると、行の右端に省略記号 **[...]** ボタンが表示されます。 省略記号ボタンをクリックして、**[ストレージ アカウント接続文字列の作成]** ダイアログ ボックスを開きます。

    ![[接続文字列の作成] ボックスを開く](./media/cloud-services-dotnet-get-started/opencscreate.png)

1. **[ストレージ接続文字列の作成]** ダイアログ ボックスで、**[サブスクリプション]** オプションをクリックします。前に作成したストレージ アカウントを選択し、**[OK]** をクリックします。 まだログインしていない場合は、Azure アカウントの資格情報を入力するよう求められます。

 ![ストレージ接続文字列の作成
](./media/cloud-services-dotnet-get-started/createstoragecs.png)

1. 変更を保存します。

2. 使用した同じ手順に従って、 `StorageConnectionString` 設定への接続文字列、 `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` 接続文字列。

    この接続文字列は、ログに使用されます。

2. **ContosoAdsWeb** ロールの場合と同じ手順に従って、**ContosoAdsWorker** ロールの両方の接続文字列を設定します。 このとき、**[サービス構成]** を **[クラウド]** に設定するのを忘れないでください。

Visual Studio UI を使用して構成したロールの環境設定は、ContosoAdsCloudService プロジェクトの次のファイルに格納されます。

* *ServiceDefinition.csdef* - 設定名を定義します。
* *ServiceConfiguration.Cloud.cscfg* - アプリがクラウドで実行されるときに値を提供します。
* *ServiceConfiguration.Local.cscfg* - アプリがローカルで実行されるときに値を提供します。

たとえば、ServiceDefinition.csdef には、次の定義が含まれます。

        <ConfigurationSettings>
          <Setting name="StorageConnectionString" />
          <Setting name="ContosoAdsDbConnectionString" />
        </ConfigurationSettings>

*ServiceConfiguration.Cloud.cscfg* ファイルには、Visual Studio でこれらの設定に入力した値が含まれます。

        <Role name="ContosoAdsWorker">
          <Instances count="1" />
          <ConfigurationSettings>
            <Setting name="StorageConnectionString" value="{yourconnectionstring}" />
            <Setting name="ContosoAdsDbConnectionString" value="{yourconnectionstring}" />
            
          </ConfigurationSettings>
          
        </Role>

`< インスタンス >` 設定は、Azure は、ワーカー ロール コードが実行する仮想マシンの数を指定します。 [次のステップ](#next-steps) セクションには、クラウド サービスのスケール アウトに関する情報へのリンクが含まれています。

### Azure にプロジェクトをデプロイする

3.  **ソリューション エクスプローラー**で、**[ContosoAdsCloudService]** クラウド プロジェクトを右クリックし、**[発行]** をクリックします。

    ![[発行] メニュー](./media/cloud-services-dotnet-get-started/pubmenu.png)

2. **Azure アプリケーションの発行**ウィザードの [**サインイン**] 手順で、[**次へ**] をクリックします。

 ![[サインイン] 手順
](./media/cloud-services-dotnet-get-started/pubsignin.png)

3. ウィザードの **[設定]** 手順で、**[次へ]** をクリックします。

    ![[設定] 手順](./media/cloud-services-dotnet-get-started/pubsettings.png)

    **[詳細設定]** タブの既定の設定はこのチュートリアルではそのまま使用します。 高度なタブについては、次を参照してください。 [Azure アプリケーションの発行ウィザード](http://msdn.microsoft.com/library/hh535756.aspx)します。

2. **[概要]** 手順で、**[発行]** をクリックします。

 ![[概要] 手順
](./media/cloud-services-dotnet-get-started/pubsummary.png)

**[Azure のアクティビティ ログ]** ウィンドウが Visual Studio で開きます。

2. 右矢印のアイコンをクリックしてデプロイの詳細を展開します。

    デプロイが完了するまで 5 分程度かかる場合があります。

    ![[Azure のアクティビティ ログ] ウィンドウ](./media/cloud-services-dotnet-get-started/waal.png)

1. ステータスのデプロイが完了したら、**Web サイトの URL** をクリックしてアプリケーションを開始します。

9. この段階で、アプリケーションをローカルで実行したときと同様に、広告を作成、表示、編集して、アプリをテストできます。

>[AZURE.NOTE] テストが終了したら、クラウド サービスを削除または停止します。 クラウド サービスを使用していない場合でも仮想マシンのリソースが予約されるため、課金が発生します。 またクラウド サービスを実行したままにしておくと、その URL を見つけた他のユーザーが広告を作成したり表示したりする可能性があります。  [Azure クラシック ポータル](http://manage.windowsazure.com), に移動して、 **ダッシュ ボード** 、クラウド サービス] タブをクリックして、 **削除** ページの下部にあるボタンをクリックします。 一時的に他のユーザーがこのサイトにアクセスできないようにするには、代わりに **[停止]** をクリックします。 この場合は、課金が継続されます。 同様の手順で、不要になった SQL データベースとストレージ アカウントを削除できます。

## アプリケーションを最初から作成する

まだ
[、完成したアプリケーション](http://code.msdn.microsoft.com/Simple-Azure-Cloud-Service-e01df2e4), はすぐに実行します。 以降の操作では、ダウンロードしたプロジェクトから新しいプロジェクトにファイルをコピーします。

Contoso Ads アプリケーションを作成するには、次の手順を実行します。

* クラウド サービスの Visual Studio ソリューションを作成します。
* NuGet パッケージを更新して追加します。
* プロジェクト参照を設定します。
* 接続文字列を構成します。
* コード ファイルを追加します。

ソリューションを作成したら、クラウド サービス プロジェクト、Azure BLOB、およびキューに一意のコードを見ていきます。

### クラウド サービス Visual Studio ソリューションの作成

1. Visual Studio で、**[ファイル]** メニューの **[新しいプロジェクト]** を選択します。

2. **[新しいプロジェクト]** ダイアログ ボックスの左側のウィンドウで、**[Visual C#]** を展開し、**[クラウド]** テンプレート、**[Azure クラウド サービス]** テンプレートの順に選択します。

3. プロジェクトおよびソリューションに ContosoAdsCloudService という名前を付けて **[OK]** をクリックします。

    ![新しいプロジェクト](./media/cloud-services-dotnet-get-started/newproject.png)

4. **[新しい Azure クラウド サービス]** ダイアログ ボックスで、Web ロールと worker ロールを追加します。 Web ロールに ContosoAdsWeb という名前を付け、worker ロールに ContosoAdsWorker という名前を付けます (右側のウィンドウの鉛筆アイコンを使用して、ロールの既定の名前を変更します)。

    ![新しいクラウド サービス プロジェクト](./media/cloud-services-dotnet-get-started/newcsproj.png)

5. Web ロールの **[新しい ASP.NET プロジェクト]** ダイアログ ボックスで、MVC テンプレートを選択し、次に **[認証の変更]** をクリックします。

    ![認証の変更](./media/cloud-services-dotnet-get-started/chgauth.png)

7. **[認証の変更]** ダイアログ ボックスで、**[認証なし]** をクリックし、次に **[OK]** をクリックします。

    ![[認証なし]](./media/cloud-services-dotnet-get-started/noauth.png)

8. **[新しい ASP.NET プロジェクト]** ダイアログで **[OK]** をクリックします。

9. **ソリューション エクスプローラー**で (プロジェクトのソリューション以外の) ソリューションを右クリックし、**[追加]、[新しいプロジェクト]** の順に選択します。

11. **[新しいプロジェクトの追加]** ダイアログ ボックスで、左側のウィンドウの **[Visual C#]** の下の **[Windows デスクトップ]** を選択し、**[クラス ライブラリ]** テンプレートをクリックします。

10. プロジェクトに *ContosoAdsCommon* という名前を付けて **[OK]** をクリックします。

    Web ロール プロジェクトと worker ロール プロジェクトの両方から Entity Framework のコンテキストとデータ モデルを参照する必要があります。 代わりの方法として、Entity Framework に関連するクラスを Web ロール プロジェクトに定義して、worker ロール プロジェクトからそのプロジェクトを参照することもできます。 ただし別の方法では、worker ロール プロジェクトに、必要としない Web アセンブリへの参照が含まれることになります。

### NuGet パッケージの更新および追加

11. ソリューションの **[NuGet パッケージの管理]** ダイアログ ボックスを開きます。

12. 左側のウィンドウで、**[更新プログラム]** を選択します。

13. *WindowsAzure.Storage* のパッケージを見つけます。このパッケージが一覧にない場合は、**[更新]** をクリックして、ストレージ クライアント ライブラリの最新バージョンを取得します。

    ![Update SCL](./media/cloud-services-dotnet-get-started/updstg.png)

    ストレージ クライアント ライブラリは Visual Studio プロジェクト テンプレートよりも頻繁に更新されるため、新しく作成したプロジェクトのバージョンの更新が必要になることがよくあります。

14. 左側のウィンドウで、**[オンライン]** を選択します。

16. *EntityFramework* NuGet パッケージを見つけて、3 つのプロジェクトすべてにインストールします。

17. *Microsoft.WindowsAzure.ConfigurationManager* NuGet パッケージを見つけ、worker ロール プロジェクトにインストールします。

### プロジェクト参照の設定

10. ContosoAdsWeb プロジェクトで、ContosoAdsCommon プロジェクトの参照を設定します。 ContosoAdsWeb プロジェクトを右クリックし、**[参照]**、**[参照の追加]** の順にクリックします。 **[参照マネージャー]** ダイアログ ボックスで、左側のウィンドウの **[ソリューション]、[プロジェクト]** の順に選択し、**[ContosoAdsCommon]** を選択して **[OK]** をクリックします。

11. ContosoAdsWorker プロジェクトで、ContosAdsCommon プロジェクトの参照を設定します。

    ContosoAdsCommon は、Entity Framework データ モデルおよびコンテキスト クラスを含みます。これらは、フロントエンドとバックエンドの両方で使用されます。

11. ContosoAdsWorker プロジェクトで参照を設定 `System.Drawing`します。

    このアセンブリは、画像を縮小表示に変換するためにバックエンドで使用されます。

### 接続文字列の構成

このセクションでは、Azure Storage および SQL 接続文字列をローカルでのテスト用に構成します。 チュートリアルの前半で紹介したデプロイ手順では、アプリケーションをクラウドで実行する場合の接続文字列の設定方法について説明しました。

3. ContosoAdsWeb プロジェクトでアプリケーションの Web.config ファイルを開き、次を挿入 `connectionStrings` 要素の後に、 `configSections` 要素。

        <connectionStrings>
          <add name="ContosoAdsContext" connectionString="Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;" providerName="System.Data.SqlClient" />
        </connectionStrings>

3. 変更を保存します。

2. ContosoAdsCloudService プロジェクトで、**[ロール]** の下の [ContosoAdsWeb] を右クリックし、**[プロパティ]** をクリックします。

 ![ロール プロパティ
](./media/cloud-services-dotnet-get-started/roleproperties.png)

3. **[ContosAdsWeb [ロール]]** プロパティ ウィンドウで、**[設定]** タブをクリックし、**[設定の追加]** をクリックします。

    **[サービス構成]** は **[すべての構成]** のままにしておきます。

4. *StorageConnectionString* という名前の新しい設定を追加します。 **[種類]** を *[接続文字列]* に設定し、**[値]** を *UseDevelopmentStorage=true* に設定します。

 ![新しい接続文字列
](./media/cloud-services-dotnet-get-started/scall.png)

5. 変更を保存します。

3. 同じ手順を実行して、ストレージの接続文字列を ContosoAdsWorker ロール プロパティに追加します。

5. **[ContosoAdsWorker [Role]]** プロパティ ウィンドウで、もう 1 つ接続文字列を追加します。

    * 名前: ContosoAdsDbConnectionString
    * 型: 文字列
    * 値: Web ロール プロジェクトに使用したのと同じ接続文字列を貼り付けます。

            Data Source=(localdb)\v11.0; Initial Catalog=ContosoAds; Integrated Security=True; MultipleActiveResultSets=True;


### コード ファイルの追加

このセクションでは、ダウンロードしたソリューションから新しいソリューションにコード ファイルをコピーします。 以降のセクションでは、このコードの重要な部分について説明します。

プロジェクトまたはフォルダーにファイルを追加するには、プロジェクトまたはフォルダーを右クリックし、**[追加]**、**[既存の項目]** の順にクリックします。 目的のファイルを選択してから **[追加]** をクリックします。 既存のファイルを置き換えるかどうかをたずねるメッセージが表示されたら、**[はい]** をクリックします。

3. ContosoAdsCommon プロジェクトで、*Class1.cs* ファイルを削除します。その場所に、ダウンロードしたプロジェクトから *Ad.cs* ファイルと *ContosoAdscontext.cs* ファイルを追加します。

3. ContosoAdsWeb プロジェクトで、ダウンロードしたプロジェクトから次のファイルを追加します。
    - *Global.asax.cs*。
    - *Views \shared* フォルダー: <em>\_Layout.cshtml</em>します。
    - *Views\Home* フォルダー: *Index.cshtml*。
    - *Controllers* フォルダー: *AdController.cs*。
    - *Views\Ad* フォルダー (最初にフォルダーを作成): 5 つの *.cshtml* ファイル。

3. ContosoAdsWorker プロジェクトで、ダウンロードしたプロジェクトから *WorkerRole.cs* を追加します。

この段階で、チュートリアルの前の説明で示した手順に従ってアプリケーションをビルドして実行できます。アプリは、ローカル データベースおよびストレージ エミュレーター リソースを使用します。

以降のセクションでは、Azure 環境、BLOB、およびキューの操作に関連するコードについて説明します。 このチュートリアルでは、スキャフォールディングを使用した MVC コントローラーおよびビューの作成方法、SQL Server データベースで動作する Entity Framework コードの作成方法、ASP.NET 4.5 での非同期プログラミングの基礎については説明していません。 これらのトピックについては、次のリソースを参照してください。

* [MVC 5 を概要します。](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
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

次に、*.cscfg* ファイルからのストレージ接続文字列を使用してストレージ アカウントへのアクセスが取得されます。

        var storageAccount = CloudStorageAccount.Parse
            (RoleEnvironment.GetConfigurationSettingValue("StorageConnectionString"));

次に、*images* BLOB コンテナーの参照が取得され、コンテナーがまだ存在していない場合はこれが作成され、新しいコンテナーのアクセス許可が設定されます。 既定では、新しいコンテナーは、ストレージ アカウント資格情報を持つクライアントのみに BLOB へのアクセスを許可します。 Web サイトで画像 BLOB を指し示す URL を使用して画像を表示できるようにするには、BLOB をパブリックに設定する必要があります。

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

似たようなコードを使用して、*images* キューの参照を取得し、新しいキューを作成できます。 この場合、アクセス許可の変更は必要ありません。

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        var imagesQueue = queueClient.GetQueueReference("images");
        imagesQueue.CreateIfNotExists();

### ContosoAdsWeb - \_Layout.cshtml

*_Layout.cshtml* ファイルは、ヘッダーとフッターをアプリの名前を設定し、"Ads"メニュー エントリを作成します。

### ContosoAdsWeb - Views\Home\Index.cshtml

*Views\Home\Index.cshtml* ファイルは、ホーム ページにカテゴリ リンクを表示します。 リンクの整数値を渡す、 `カテゴリ` Ads Index ページに、querystring 変数の列挙型。

        <li>@Html.ActionLink("Cars", "Index", "Ad", new { category = (int)Category.Cars }, null)</li>
        <li>@Html.ActionLink("Real estate", "Index", "Ad", new { category = (int)Category.RealEstate }, null)</li>
        <li>@Html.ActionLink("Free stuff", "Index", "Ad", new { category = (int)Category.FreeStuff }, null)</li>
        <li>@Html.ActionLink("All", "Index", "Ad", null, null)</li>

### ContosoAdsWeb - AdController.cs

*AdController.cs* ファイルのコンス トラクターによって、 `InitializeStorage` blob およびキューを操作するための API を提供する Azure ストレージ クライアント ライブラリ オブジェクトを作成する方法です。

次に、*Global.asax.cs* と同様に、*images* BLOB コンテナーへの参照が取得されます。 処理中に、既定値を設定して [再試行ポリシー](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) web アプリに適したします。 既定の指数関数的バックオフ再試行ポリシーでは、一時的な障害に対する反復的再試行操作によって Web アプリが 1 分以上停止する可能性があります。 ここでは、再試行ごとに 3 秒待機し、再試行の回数を 3 回までとする再試行ポリシーを指定しています。

        var blobClient = storageAccount.CreateCloudBlobClient();
        blobClient.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesBlobContainer = blobClient.GetContainerReference("images");

似たようなコードを使用して、*images* キューの参照を取得できます。

        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
        queueClient.RetryPolicy = new LinearRetry(TimeSpan.FromSeconds(3), 3);
        imagesQueue = queueClient.GetQueueReference("images");

コントローラー コードのほとんどは、DbContext クラスを使用した Entity Framework データ モデルの操作に典型的なものです。 例外は、HttpPost `作成` メソッドは、ファイルをアップロードして blob ストレージに保存します。 モデル バインダーは、 [HttpPostedFileBase](http://msdn.microsoft.com/library/system.web.httppostedfilebase.aspx) メソッド オブジェクト。

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

アップロードするコードは、 `UploadAndSaveBlobAsync` メソッドです。 このコードでは、BLOB の GUID 名が作成され、ファイルがアップロードおよび保存され、保存された BLOB への参照が返されます。

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

HttpPost 後 `作成` メソッドは、blob をアップロードしてデータベースを更新するでイメージが、縮小表示に変換できる状態であることをバックエンド プロセスに通知するキュー メッセージを作成します。

        string queueMessageString = ad.AdId.ToString();
        var queueMessage = new CloudQueueMessage(queueMessageString);
        await queue.AddMessageAsync(queueMessage);

Httppost コード `編集` メソッドは似ていますが、ユーザーが新しい画像ファイルを選択した場合、既に存在する blob を削除する必要があります。

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

*Index.cshtml* ファイルは、縮小表示を他の広告データと共に表示します。

        <img  src="@Html.Raw(item.ThumbnailURL)" />

*Details.cshtml* ファイルは、フルサイズの画像を表示します。

        <img src="@Html.Raw(Model.ImageURL)" />

### ContosoAdsWeb - Views\Ad\Create.cshtml および Edit.cshtml

*Create.cshtml* と *Edit.cshtml* ファイルは、コント ローラーを取得できるようにするフォームのエンコードを指定、 `HttpPostedFileBase` オブジェクトです。

        @using (Html.BeginForm("Create", "Ad", FormMethod.Post, new { enctype = "multipart/form-data" }))

`< 入力 >` 要素が、ファイル選択ダイアログを提供するようブラウザーに指示します。

        <input type="file" name="imageFile" accept="image/*" class="form-control fileupload" />

### ContosoAdsWorker - WorkerRole.cs - OnStart メソッド

Azure ワーカー ロールの環境の呼び出し、 `OnStart` メソッドに、 `WorkerRole` worker ロールの起動時は、および呼び出し時、 `実行` メソッドと、 `OnStart` メソッドが終了します。

`OnStart` メソッドからデータベース接続文字列を取得、 *.cscfg* ファイルし、Entity Framework の DbContext クラスに渡します。 既定で SQLClient プロバイダーが使用されるため、プロバイダーを指定する必要はありません。

        var dbConnString = CloudConfigurationManager.GetSetting("ContosoAdsDbConnectionString");
        db = new ContosoAdsContext(dbConnString);

次に、ストレージ アカウントへの参照が取得され、BLOB コンテナーおよびキューが存在しない場合はこれらが作成されます。 そのため、コードは既に確認したもので、web ロールと似ています `Application_Start` メソッドです。

### ContosoAdsWorker - WorkerRole.cs - Run メソッド

`実行` メソッドが呼び出されます、 `OnStart` メソッドが初期化操作を完了するとします。 このメソッドは、新しいキュー メッセージを監視する無限ループを実行して、キュー メッセージが到着したときにそれを処理します。

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

ときには、キュー メッセージの内容が原因で処理エラーが生じることがあります。 このようなメッセージは*有害メッセージ*と呼ばれ、エラーをログに記録してループを再開するだけでは、このメッセージを処理する試みが際限なく繰り返されることになります。 そこで、catch ブロックに if ステートメントを含めて、アプリが現在のメッセージを処理しようと試みた回数を調べ、その回数が 5 回を超えた場合はメッセージをキューから削除するようにしています。

`ProcessQueueMessage` キュー メッセージが見つかったときに呼び出されます。

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

`RoleEnvironment` オブジェクトは、Azure でアプリケーションを実行したとき、または Azure コンピューティング エミュレーターを使用してローカルで実行すると、Azure によって提供されます。 アプリケーションをローカルで実行しているときにこのエラーが発生した場合は、ContosoAdsCloudService プロジェクトがスタートアップ プロジェクトとして設定されていることを確認してください。 こうすることで、Azure コンピューティング エミュレーターを使用して実行するようにプロジェクトが設定されます。

アプリケーションが Azure RoleEnvironment を使用する目的の 1 つに、*.cscfg* ファイルに格納されている接続文字列値を取得することがあります。したがって、この例外の別の原因として、接続文字列が見つからないことが考えられます。 ContosoAdsWeb プロジェクトのクラウド構成とローカル構成の両方に対して StorageConnectionString 設定が作成されていること、および ContosoAdsWorker プロジェクトの両方の構成に対して両方の接続文字列が作成されていることを確認してください。 ソリューション全体を対象に StorageConnectionString の **"すべて検索"** を実行すると、6 つのファイルに 9 件見つかります。

### ポート xxx にオーバーライドできません。新しいポートが http プロトコルで許容されている最小値 8080 を下回っています

Web プロジェクトで使用するポート番号を変更してください。 ContosoAdsWeb プロジェクトを右クリックし、**[プロパティ]** をクリックします。 **[Web]** タブで、**[プロジェクトの URL]** 設定のポート番号を変更します。

この問題を解決する別の方法としては、次のセクションを参照してください。

### ローカルで実行しているときに発生するその他のエラー

既定では、新しいクラウド サービス プロジェクトは、Azure コンピューティング エミュレーター Express を使用して Azure 環境をシミュレートします。 これはフル装備のコンピューティング エミュレーターの軽量バージョンです。フル装備のエミュレーターは、Express バージョンが動作しない特定の条件でも動作します。

フル装備のエミュレーターを使用するようにプロジェクトを変更するには、ContosoAdsCloudService プロジェクトを右クリックし、**[プロパティ]** をクリックします。 **[プロパティ]** ウィンドウで、**[Web]** タブをクリックし、**[フル装備のエミュレーターを使用する]** をクリックします。

フル装備のエミュレーターを使用してアプリケーションを実行するには、管理者特権で Visual Studio を開く必要があります。

## 次のステップ

Contoso Ads アプリケーションは、入門用のチュートリアルという目的から意図的にシンプルに作られています。 実装されていないなど [依存関係の注入](http://www.asp.net/mvc/tutorials/hands-on-labs/aspnet-mvc-4-dependency-injection) または [作業パターンのリポジトリと単位](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application#repo), 、しない [ログ記録用のインターフェイスを使用](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry#log), を使用しない [EF Code First Migrations](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application) データ モデルの変更を管理するまたは [EF 接続の回復性](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application) 一時的なネットワーク エラーを管理するためです。

以下に、より現実的なコーディング手法を示すいくつかのクラウド サービス サンプル アプリケーションを、やさしいものから順に示します。

* [PhluffyFotos](http://code.msdn.microsoft.com/PhluffyFotos-Sample-7ecffd31). 概念は Contoso Ads に似ていますが、より多くの機能とより現実的なコーディング手法が実装されています。
* [Azure クラウド サービス多層アプリケーションは、テーブル、キュー、および Blob を](http://code.msdn.microsoft.com/windowsazure/Windows-Azure-Multi-Tier-eadceb36)します。 Azure のストレージ テーブル、blob、キューを紹介し、付属の [ステップ バイ ステップのチュートリアル シリーズ](../cloud-services-dotnet-multi-tier-app-storage-1-overview.md)します。
* [クラウドの Microsoft Azure サービスの基礎](http://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649)します。 Microsoft Patterns and Practices グループから提供された、さまざまなベスト プラクティスを紹介する包括的なサンプルです。

クラウド向けの開発の詳細については、次を参照してください。 [Azure での実際のクラウド アプリケーションのビルド](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction)します。

Azure Storage に関するベスト プラクティスとパターンを紹介ビデオについては、次を参照してください。 [Microsoft Azure Storage – 新規、ベスト プラクティスおよびパターンは何](http://channel9.msdn.com/Events/Build/2014/3-628)します。

詳細については、次のリソースを参照してください。

* [Azure Cloud Services パート 1: 概要](http://justazure.com/microsoft-azure-cloud-services-part-1-introduction/)
* [クラウド サービスを管理する方法](cloud-services-how-to-manage.md)
* [Azure ストレージ](/documentation/services/storage/)





