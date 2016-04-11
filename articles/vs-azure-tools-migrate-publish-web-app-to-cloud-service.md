<properties
   pageTitle="方法: Visual Studio から Azure クラウド サービスへの Web アプリケーションの移行および発行 | Microsoft Azure"
   description="Visual Studio を使用して、Azure クラウド サービスに Web アプリケーションを移行および発行する方法について説明します。"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="09/30/2015"
   ms.author="tarcher" />

# 方法: Visual Studio から Azure クラウド サービスへの Web アプリケーションの移行および発行

Azure のホスティング サービスとスケーラビリティを利用するために、Azure クラウド サービスに Web アプリケーションを移行し、発行できます。 既存のアプリケーションに最小限の変更を加えることで、Azure で Web アプリケーションを実行できます。

>[AZURE.NOTE] このトピックではない web サイトへのクラウド サービスへのデプロイに関するです。 Web サイトに展開する方法の詳細については、次を参照してください。 [Azure App Service で web アプリのデプロイ](web-sites-deploy.md)します。

Visual c# および Visual Basic の両方でサポートされている特定のテンプレートの一覧は、セクションを参照してください。 **プロジェクト テンプレートのサポートされている** このトピックで後述します。

まず、Visual Studio から Web アプリケーションを Azure で有効にする必要があります。 次の図は、デプロイメントに使用する Azure プロジェクトを追加して既存の Web アプリケーションを発行する際の主な手順を示しています。 このプロセスでは、必要な Web ロールを含む Azure プロジェクトをソリューションに追加します。 Web プロジェクトの種類に基づいて、デプロイメントのためにサービス パッケージに他のアセンブリが必要な場合は、アセンブリのプロジェクト プロパティも更新されます。

![Microsft Azure への Web アプリケーションの発行](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

>[AZURE.NOTE]  **変換**, 、**Azure クラウド サービス プロジェクトを変換する** コマンドがソリューションに web プロジェクトにのみ表示されます。 たとえば、このコマンドはソリューションの Silverlight プロジェクトでは使用できません。
サービス パッケージの作成時や Azure へのアプリケーションの発行時に、警告またはエラーが発生することがあります。 これらの警告とエラーにより、Azure にデプロイする前に問題を解決できます。 たとえば、アセンブリが見つからないことを示す警告が表示される場合があります。 警告をエラーとして処理する方法の詳細については、次を参照してください。 [Visual Studio で Azure クラウド サービス プロジェクトを構成する](vs-azure-tools-configuring-an-azure-project.md)です。 コンピューティング エミュレーターを使用してローカルで実行、または Azure に発行するアプリケーションを作成するに次エラーを表示する可能性があります、 **エラー一覧** ウィンドウ: **指定したパス、ファイル名またはその両方が長すぎます**します。 このエラーは、Azure プロジェクトの完全修飾名が長すぎるために発生します。 プロジェクト名の長さは、完全なパスを含めて、146 文字以下にする必要があります。 たとえば、`c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj` は、Silverlight アプリケーション用に作成された Azure プロジェクトのファイル パスを含む完全なプロジェクト名です。 プロジェクトの完全修飾名を短くするために、ソリューションを短いパスの別のディレクトリに移動することが必要な場合があります。

Visual Studio から Azure に Web アプリケーションを移行および発行するには、次の手順に従います。

## Azure への Web アプリケーションのデプロイメントの有効化

### Azure への Web アプリケーションのデプロイメントを有効にするには

1. Azure への Web アプリケーションのデプロイメントを有効にするには、ソリューションの Web プロジェクトのショートカット メニューを開き、[Azure デプロイメント プロジェクトの追加] を選択します。

    次のアクションが発生します。

    - `<name of the web project>.Azure` という Azure プロジェクトがアプリケーションのソリューションに追加されます。

    - Web プロジェクトの Web ロールがこの Azure プロジェクトに追加されます。

    -  **ローカル コピー** プロパティが 4、および Silverlight のビジネス アプリケーションは MVC 2、MVC 3、MVC の必要なアセンブリを true に設定します。 これにより、デプロイメントに使用されるサービス パッケージにこれらのアセンブリが追加されます。

  >[AZURE.IMPORTANT] その他のアセンブリまたはこの web アプリケーションに必要なファイルがあれば、これらのファイルのプロパティを手動で設定する必要があります。 これらのプロパティを設定する方法については、セクションを参照してください。 **サービス パッケージにファイルを含める** この記事で後述します。

  >[AZURE.NOTE] 特定の web プロジェクト用の web ロールは、ソリューションでは、Azure プロジェクトに既に存在する場合 **変換**, 、**Azure クラウド サービス プロジェクトを変換する** はこの web プロジェクトのショートカット メニューに表示されません。

  Web アプリケーションに複数の Web プロジェクトがあり、各 Web プロジェクトの Web ロールを作成する場合は、Web プロジェクトごとにこの手順を実行する必要があります。 これにより、Web ロールごとに個別の Azure プロジェクトが作成されます。 各 Web プロジェクトは個別に発行できます。 また、Web アプリケーションの既存の Azure プロジェクトに別の Web ロールを手動で追加することもできます。 ショートカット メニューを開き、 **ロール** 、Azure プロジェクトのフォルダーを選択 **追加**, 、し **ソリューションで Web ロール プロジェクト**, 、クリックして、web ロールとして追加するプロジェクトの選択、 **[ok]** ] ボタンをクリックします。

## アプリケーションでの Azure SQL Database の使用

オンプレミスの SQL Server データベースを使用する Web アプリケーションの接続文字列がある場合、Azure でホストされる SQL Database のインスタンスを代わりに使用するために、この接続文字列を変更する必要があります。

>[AZURE.IMPORTANT] お客様のサブスクリプションには、SQL データベースを使用することが有効にする必要があります。 Microsoft Azure 管理ポータルからサブスクリプションにアクセスすると、サブスクリプションで提供されるサービスを確認できます。 次の手順は、リリース済みの管理ポータルに適用されます。 プレビュー管理ポータルを使用している場合は、この次の手順に進んでください。

### Web ロールで接続文字列に SQL Database インスタンスを使用するには

1. Azure 管理ポータルで SQL データベースのインスタンスを作成する次の記事の手順に従います。 [SQL データベース サーバーの作成](http://go.microsoft.com/fwlink/?LinkId=225109)します。

    >[AZURE.NOTE] 選択する必要があります、SQL データベース インスタンスのファイアウォール ルールを設定すると、 **他の Azure サービスをこのサーバーへのアクセスを許可する** チェック ボックスをオンします。

1. 接続文字列を使用する SQL データベースのインスタンスを作成する次の記事では、次のセクションで手順に従います。 [SQL データベースの作成](http://go.microsoft.com/fwlink/?LinkId=225110)します。

1. 接続文字列に使用する ADO.NET 接続文字列をコピーするには、Azure 管理ポータルで次の手順を実行します。  

  1. 選択、 **データベース** ボタンをクリックし、SQL データベースのインスタンスを作成するために使用するサブスクリプションのノードを開きます。

  1. SQL データベースの使用可能なインスタンスを表示するには、 **SQL データベース** ノードです。

  1. データベースのプロパティを表示するには、データベースを選択します。  **プロパティ** ビューが表示されます。

      >[AZURE.NOTE] 場合、 **プロパティ** ビューが表示されない、区分線を使用して開く必要があります。

  1. 接続文字列を表示するには、[表示] の横にある省略記号 (...) ボタンをクリックします。

    The **Connection Strings** dialog box appears.

  1. ADO.NET 接続文字列をコピーするには、テキストを強調表示し、Ctrl キーを押しながら C キーを押します。

  1. ダイアログ ボックスを閉じるには、選択、 **を閉じる** ] ボタンをクリックします。

1. SQL Database のこのインスタンスを使用するために、web.config ファイルで接続文字列を置き換えるには、web.config ファイルを開き、既存の接続文字列エントリを強調表示して、Ctrl キーを押しながら V キーを押します。 既存の接続文字列が、SQL Database インスタンスの ADO.NET 接続文字列で置き換えられます。

1. 接続文字列に `MultipleActiveResultSets=True` パラメーターを追加することも必要です。 接続文字列は、次の形式にする必要があります。

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```

1. (省略可能) web.config ファイルで接続文字列を直接変更する代わりに、サービス パッケージを作成する際に使用するビルド構成に応じて、いずれかの web.config 変換ファイルにセクションを追加することもできます。 Web.Debug.Config ファイルまたは Web.Release.Config ファイルを開きます。 このファイルに次のセクションを追加します。

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```

1. 変更したファイルを保存し、アプリケーションを再発行します。

### Microsoft Azure 管理ポータルを使用して SQL Database インスタンスを使用するには

1.  [Azure 管理ポータル](http://go.microsoft.com/fwlink/?LinkID=213885), 、SQL データベース] ノードを選択します。

  - 使用する SQL Database インスタンスが表示されたら、インスタンスを選択して開きます。

  - インスタンスをまだ作成していない場合は、該当するリンクをクリックし、インスタンスを作成します。

1. 後で開くか、データベース インスタンスを作成する、 **接続文字列** リンクします。

1. ページの下部にある、ファイアウォール設定を構成するためのリンクをクリックし、既定値をそのまま使用するか、必要な値を構成します。

1. ADO.NET 接続文字列をコピーし、web.config ファイルに貼り付けて、オンプレミス データベースの古い接続文字列を上書きします。また、`MultipleActiveResultSets=True` を必ず追加します。

## Azure への Web アプリケーションの発行

### Azure に Web アプリケーションを発行するには

1. ローカルの開発でアプリケーションをテストする環境は、Azure を使用してコンピューティング エミュレーター、web ロールの Azure プロジェクトのショートカット メニューを開き **スタートアップ プロジェクトとして設定**します。 選択し、 **デバッグ**, 、**[デバッグ開始]** (キーボード: **f5 キーを押して**)。

     **Azure デバッグ環境の開始** ] ダイアログ ボックスが開き、アプリケーションは、ブラウザーで起動します。 コンピューティング エミュレーターで各種 Web アプリケーションを起動する方法の詳細については、このセクションの表をご覧ください。

1. Azure に発行するアプリケーションのサービスを設定するには、Microsoft アカウントと Azure サブスクリプションが必要です。 次のトピックの手順を使用して、サービスを設定します。 [発行または Visual Studio から Azure アプリケーションをデプロイするための準備](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md)します。

1. Azure に web アプリケーションを発行する web プロジェクトのショートカット メニューを開くし、を選択 **Publish to Azure**します。

     **Azure アプリケーションの発行** ] ダイアログ ボックスが開き、Visual Studio は、展開プロセスを開始します。 セクションを参照して、アプリケーションを発行する方法の詳細については、 **Visual Studio から Azure アプリケーションを発行** で [Azure ツールを使用してクラウド サービスの発行](vs-azure-tools-publishing-a-cloud-service.md)します。

    >[AZURE.NOTE] Azure プロジェクトから web アプリケーションを発行することもできます。 これを行うには、Azure プロジェクトのショートカット メニューを開き、選択 **発行**します。

1. 展開の進行状況を表示するを参照して、 **Azure のアクティビティ ログ** ウィンドウです。 このログは、デプロイメント プロセスの開始時に自動的に表示されます。 次の図に示すように、アクティビティ ログの行項目を展開して詳細情報を表示できます。

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (省略可能)展開プロセスをキャンセルする、動作状況ログの行項目のショートカット メニューを開き、選択 **取り消して削除**します。 これにより、デプロイ プロセスが停止し、Azure からデプロイ環境が削除されます。

    >[AZURE.NOTE] 展開された後は、このデプロイ環境を削除するには、Azure 管理ポータルを使用する必要があります。

1. (省略可能)Visual Studio が自動的にデプロイ環境を表示して、ロール インスタンスが起動すると、 **Azure コンピューティング** 内のノード **クラウド エクスプ ローラー** または **サーバー エクスプ ローラー**します。 ここから、個々のロール インスタンスの状態を確認できます。

    次の図は、ロール インスタンスで **サーバー エクスプ ローラー** は、初期化中のままにします。

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)

1. デプロイ後に、アプリケーションにアクセスするの状態のときにデプロイの横の矢印を選択して **完了** に表示されます、 **Azure のアクティビティ ログ**します。 これにより、Azure での Web アプリケーションの URL が表示されます。 Azure から特定の種類の Web アプリケーションを起動する方法の詳細については、次の表をご覧ください。

    Azure から特定の Web アプリケーションを起動する方法、または Azure コンピューティング エミュレーターを使用して Web アプリケーションをローカルで実行またはデバッグする方法の詳細を次の表に示します。

    |Web アプリケーションの種類|コンピューティング エミュレーターを使用したローカルでの実行/デバッグ|Azure での実行|
    |---|---|---|
    |ASP.NET Web アプリケーション|メニュー バー **デバッグ**, 、**[デバッグ開始]** (キーボード: 選択、 **f5 キーを押して** キーです。)。|表示される URL ハイパーリンクをクリックして、 **展開** タブに移動して、 **Azure のアクティビティ ログ** 、ブラウザーでスタート ページを読み込みます。|
    |ASP.NET MVC 2 Web アプリケーション|メニュー バー **デバッグ**, 、**[デバッグ開始]** (キーボード: 選択、 **f5 キーを押して** キーです。)。|表示される URL ハイパーリンクをクリックして、 **展開** タブに移動して、 **Azure のアクティビティ ログ** 、ブラウザーでスタート ページを読み込みます。|
    |ASP.NET MVC 3 Web アプリケーション|メニュー バー **デバッグ**, 、**[デバッグ開始]** (キーボード: 選択、 **f5 キーを押して** キーです。)。|表示される URL ハイパーリンクをクリックして、 **展開** タブに移動して、 **Azure のアクティビティ ログ** 、ブラウザーでスタート ページを読み込みます。|
    |ASP.NET MVC 4 Web アプリケーション|メニュー バー **デバッグ**, 、**[デバッグ開始]** (キーボード: 選択、 **f5 キーを押して** キーです。)。|表示される URL ハイパーリンクをクリックして、 **展開** タブに移動して、 **Azure のアクティビティ ログ** 、ブラウザーでスタート ページを読み込みます。|
    |空の ASP.NET Web アプリケーション|Web プロジェクトのスタート ページとして設定したアプリケーションに .aspx ページを追加する必要があります。 メニュー バーで、次のように選択します。 **デバッグ**, 、**[デバッグ開始]** (キーボード: 選択、 **f5 キーを押して** キーです。)。|アプリケーションに既定の .aspx ページがある場合に表示される URL ハイパーリンクを選択して、 **展開** タブに移動して、 **Azure のアクティビティ ログ** とこのページがブラウザーに読み込まれます。 別の .aspx ページを使用している場合は、`<url for deployment>/<name of page>.aspx` 形式の URL を使用して、そのページに移動する必要があります。|
    |Silverlight アプリケーション|メニュー バー **デバッグ**, 、**[デバッグ開始]** (キーボード: 選択、 **f5 キーを押して** キーです。)。|`<url for deployment>/<name of page>.aspx` 形式の URL を使用して、アプリケーションの特定のページに移動する必要があります。|
    |Silverlight ビジネス アプリケーション|メニュー バー **デバッグ**, 、**[デバッグ開始]** (キーボード: 選択、 **f5 キーを押して** キーです。)。|`<url for deployment>/<name of page>.aspx` 形式の URL を使用して、アプリケーションの特定のページに移動する必要があります。|
    |Silverlight ナビゲーション アプリケーション|メニュー バー **デバッグ**, 、**[デバッグ開始]** (キーボード: 選択、 **f5 キーを押して** キーです。)。|`<url for deployment>/<name of page>.aspx` 形式の URL を使用して、アプリケーションの特定のページに移動する必要があります。|
    |WCF サービス アプリケーション|.svc ファイルを WCF サービス プロジェクトのスタート ページとして設定する必要があります。 メニュー バーで、次のように選択します。 **デバッグ**, 、**[デバッグ開始]** (キーボード: 選択、 **f5 キーを押して** キーです。)。|`<url for deployment>/<name of service file>.svc` 形式の URL を使用して、アプリケーションの svc ファイルに移動する必要があります。|
    |WCF ワークフロー サービス アプリケーション|.svc ファイルを WCF サービス プロジェクトのスタート ページとして設定する必要があります。 メニュー バーで、次のように選択します。 **デバッグ**, 、**[デバッグ開始]** (キーボード: 選択、 **f5 キーを押して** キーです。)。|`<url for deployment>/<name of service file>.svc` 形式の URL を使用して、アプリケーションの svc ファイルに移動する必要があります。|
    |ASP.NET 動的エンティティ|メニュー バー **デバッグ**, 、**[デバッグ開始]** (キーボード: 選択、 **f5 キーを押して** キーです。)。|接続文字列を更新する必要があります (次のセクションを参照)。 また、`<url for deployment>/<name of page>.aspx` 形式の URL を使用して、アプリケーションの特定のページに移動する必要があります。|
    |ASP.NET 動的データ LINQ to SQL|メニュー バー **デバッグ**, 、**[デバッグ開始]** (キーボード: 選択、 **f5 キーを押して** キーです。)。|「アプリケーションでの Azure SQL Database の使用」の手順に従う必要があります (このトピックの前のセクションを参照)。 また、`<url for deployment>/<name of page>.aspx` 形式の URL を使用して、アプリケーションの特定のページに移動する必要があります。|

## ASP.NET 動的エンティティの接続文字列の更新

### ASP.NET 動的エンティティの接続文字列を更新するには

1. ASP.NET 動的エンティティ web アプリケーションを使用できる SQL Azure データベースを作成する手順の手順に従います **SQL Azure データベースを使用して、アプリケーションの** このトピックの前です。

1. Microsoft Azure 管理ポータルからこのデータベースに必要なフィールドとテーブルを追加します。

1. この種類のアプリケーションの接続文字列は、web.config ファイルで次の形式になっています。  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    更新プログラム、 *connectionString* 値の次のように、SQL Azure データベースの ADO.NET 接続文字列を使用します。

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

1. 接続文字列に対して行った変更と web.config ファイルを保存するメニュー バーで [ **ファイル**, 、**web.config を保存**します。

## サポートされているプロジェクト テンプレート

Azure に Web アプリケーションを発行するには、次の表に記載されている C# または Visual Basic のプロジェクト テンプレートのいずれかをアプリケーションで使用する必要があります。

|プロジェクト テンプレート グループ|プロジェクト テンプレート|
|---|---|
|Web|ASP.NET Web アプリケーション|
|Web|ASP.NET MVC 2 Web アプリケーション|
|Web|ASP.NET MVC 3 Web アプリケーション|
|Web|ASP.NET MVC 4 Web アプリケーション|
|Web|空の ASP.NET Web アプリケーション|
|Web|空の ASP.NET MVC 2 Web アプリケーション|
|Web|ASP.NET 動的データ エンティティ Web アプリケーション|
|Web|ASP.NET 動的データ LINQ to SQL Web アプリケーション|
|Silverlight|Silverlight アプリケーション|
|Silverlight|Silverlight ビジネス アプリケーション|
|Silverlight|Silverlight ナビゲーション アプリケーション|
|WCF|WCF サービス アプリケーション|
|WCF|WCF ワークフロー サービス アプリケーション|
|ワークフロー|WCF ワークフロー サービス アプリケーション|

## 次のステップ
発行の詳細については、次を参照してください。 [発行または Visual Studio から Azure アプリケーションをデプロイするための準備](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md)します。 チェック アウト
[名前付き認証資格情報の設定](vs-azure-tools-setting-up-named-authentication-credentials.md)します。

