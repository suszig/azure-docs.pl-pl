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
>[AZURE.NOTE] このトピックでは Web サイトではなくクラウド サービスへのデプロイについて説明します。 Web サイトに展開する方法の詳細については、次を参照してください。 [Azure App Service で web アプリのデプロイ](web-sites-deploy.md)します。

Visual C# と Visual Basic の両方でサポートされているテンプレートの一覧については、このトピックで後述する「**サポートされているプロジェクト テンプレート**」を参照してください。

まず、Visual Studio から Web アプリケーションを Azure で有効にする必要があります。 次の図は、デプロイメントに使用する Azure プロジェクトを追加して既存の Web アプリケーションを発行する際の主な手順を示しています。 このプロセスでは、必要な Web ロールを含む Azure プロジェクトをソリューションに追加します。 Web プロジェクトの種類に基づいて、デプロイメントのためにサービス パッケージに他のアセンブリが必要な場合は、アセンブリのプロジェクト プロパティも更新されます。

![Microsft Azure への Web アプリケーションの発行](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)
>[AZURE.NOTE] **[変換]** - **[Microsoft Azure クラウド サービス プロジェクトに変換]** コマンドは、ソリューションの Web プロジェクトにのみ表示されます。 たとえば、このコマンドはソリューションの Silverlight プロジェクトでは使用できません。
サービス パッケージの作成時や Azure へのアプリケーションの発行時に、警告またはエラーが発生することがあります。これらの警告とエラーにより、Azure にデプロイする前に問題を解決できます。たとえば、アセンブリが見つからないことを示す警告が表示される場合があります。警告をエラーとして処理する方法の詳細については、次を参照してください。 [Visual Studio で Azure クラウド サービス プロジェクトを構成する](vs-azure-tools-configuring-an-azure-project.md)します。アプリケーションをビルドし、コンピューティング エミュレーターを使用してローカルで実行するか、Azure に発行すると、**[エラー一覧]** ウィンドウに、**指定されたパス、ファイル名、またはその両方が長すぎます**というエラーが表示される場合があります。このエラーは、Azure プロジェクトの完全修飾名が長すぎるために発生します。プロジェクト名の長さは、完全なパスを含めて、146 文字以下にする必要があります。たとえば、これは、Silverlight アプリケーションに対して作成される Azure プロジェクトのファイル パスを含む完全なプロジェクト名: `c:\users\ < ユーザー名 > \documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`します。プロジェクトの完全修飾名を短くするために、ソリューションを短いパスの別のディレクトリに移動することが必要な場合があります。

Visual Studio から Azure に Web アプリケーションを移行および発行するには、次の手順に従います。

## Azure への Web アプリケーションのデプロイメントの有効化

### Azure への Web アプリケーションのデプロイメントを有効にするには

1. Azure への Web アプリケーションのデプロイメントを有効にするには、ソリューションの Web プロジェクトのショートカット メニューを開き、[Azure デプロイメント プロジェクトの追加] を選択します。

    次のアクションが発生します。

    - Azure のプロジェクトの `< web プロジェクトの名前 >。Azure` がアプリケーションのソリューションに追加します。

    - Web プロジェクトの Web ロールがこの Azure プロジェクトに追加されます。

    - MVC 2、MVC 3、MVC 4、Silverlight の各ビジネス アプリケーションに必要なアセンブリの **[ローカルにコピー]** プロパティが true に設定されます。 これにより、デプロイメントに使用されるサービス パッケージにこれらのアセンブリが追加されます。
  >[AZURE.IMPORTANT] この Web アプリケーションに必要な他のアセンブリまたはファイルがある場合は、それらのファイルのプロパティを手動で設定する必要があります。 これらのプロパティを設定する方法については、この記事で後述する「**サービス パッケージにファイルを含める**」を参照してください。

  >[AZURE.NOTE] 特定の Web プロジェクトの Web ロールがソリューションの Azure プロジェクトに既に存在する場合、この Web プロジェクトのショートカット メニューに **[変換]** - **[Microsoft Azure クラウド サービス プロジェクトに変換]** は表示されません。

  Web アプリケーションに複数の Web プロジェクトがあり、各 Web プロジェクトの Web ロールを作成する場合は、Web プロジェクトごとにこの手順を実行する必要があります。 これにより、Web ロールごとに個別の Azure プロジェクトが作成されます。 各 Web プロジェクトは個別に発行できます。 また、Web アプリケーションの既存の Azure プロジェクトに別の Web ロールを手動で追加することもできます。 そのためには、Azure プロジェクトの **[ロール]** フォルダーのショートカット メニューを開き、**[追加]**、**[ソリューション内の Web ロール プロジェクト]** の順にクリックし、Web ロールとして追加するプロジェクトを選択して、**[OK]** ボタンをクリックします。

## アプリケーションでの Azure SQL Database の使用

オンプレミスの SQL Server データベースを使用する Web アプリケーションの接続文字列がある場合、Azure でホストされる SQL Database のインスタンスを代わりに使用するために、この接続文字列を変更する必要があります。
>[AZURE.IMPORTANT] サブスクリプションで SQL Database を使用できるようにする必要があります。 Microsoft Azure 管理ポータルからサブスクリプションにアクセスすると、サブスクリプションで提供されるサービスを確認できます。 次の手順は、リリース済みの管理ポータルに適用されます。 プレビュー管理ポータルを使用している場合は、この次の手順に進んでください。

### Web ロールで接続文字列に SQL Database インスタンスを使用するには

1. Azure 管理ポータルで SQL データベースのインスタンスを作成する次の記事の手順に従います: [SQL データベース サーバーの作成](http://go.microsoft.com/fwlink/?LinkId=225109)します。
    >[AZURE.NOTE] SQL Database インスタンスのファイアウォール規則を設定するときに、**[Microsoft Azure サービスにサーバーへのアクセスを許可します]** チェック ボックスをオンにする必要があります。

1. 接続文字列を使用する SQL データベースのインスタンスを作成する次の記事では、次のセクションで手順に従います: [SQL データベースの作成](http://go.microsoft.com/fwlink/?LinkId=225110)します。

1. 接続文字列に使用する ADO.NET 接続文字列をコピーするには、Azure 管理ポータルで次の手順を実行します。

  1. **[データベース]** ボタンをクリックし、SQL Database インスタンスを作成する際に使用したサブスクリプションのノードを開きます。

  1. SQL Database の使用可能なインスタンスを表示するには、**[SQL Database]** ノードを選択します。

  1. データベースのプロパティを表示するには、データベースを選択します。 **[プロパティ]** ビューが表示されます。
      >[AZURE.NOTE] **[プロパティ]** ビューが表示されない場合は、分割バーを使用してビューを開く必要があります。

  1. 接続文字列を表示するには、[表示] の横にある省略記号 (...) ボタンをクリックします。

    **[接続文字列]** ダイアログ ボックスが表示されます。

  1. ADO.NET 接続文字列をコピーするには、テキストを強調表示し、Ctrl キーを押しながら C キーを押します。

  1. ダイアログ ボックスを閉じるには、**[閉じる]** ボタンをクリックします。

1. SQL Database のこのインスタンスを使用するために、web.config ファイルで接続文字列を置き換えるには、web.config ファイルを開き、既存の接続文字列エントリを強調表示して、Ctrl キーを押しながら V キーを押します。 既存の接続文字列が、SQL Database インスタンスの ADO.NET 接続文字列で置き換えられます。

1. パラメーターを追加する必要がありますも `MultipleActiveResultSets = True` 接続文字列にします。 接続文字列は、次の形式にする必要があります。

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```

1. (省略可能) web.config ファイルで接続文字列を直接変更する代わりに、サービス パッケージを作成する際に使用するビルド構成に応じて、いずれかの web.config 変換ファイルにセクションを追加することもできます。 Web.Debug.Config ファイルまたは Web.Release.Config ファイルを開きます。 このファイルに次のセクションを追加します。

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```

1. 変更したファイルを保存し、アプリケーションを再発行します。

### Microsoft Azure 管理ポータルを使用して SQL Database インスタンスを使用するには

1. [Azure 管理ポータル](http://go.microsoft.com/fwlink/?LinkID=213885), 、SQL データベース] ノードを選択します。

  - 使用する SQL Database インスタンスが表示されたら、インスタンスを選択して開きます。

  - インスタンスをまだ作成していない場合は、該当するリンクをクリックし、インスタンスを作成します。

1. データベース インスタンスを開くか、インスタンスを作成したら、**[接続文字列]** リンクを選択します。

1. ページの下部にある、ファイアウォール設定を構成するためのリンクをクリックし、既定値をそのまま使用するか、必要な値を構成します。

1. ADO.NET 接続文字列をコピー、内部設置型データベースの以前の接続文字列を web.config ファイルに貼り付けますおよびを必ず追加 `MultipleActiveResultSets = True`します。

## Azure への Web アプリケーションの発行

### Azure に Web アプリケーションを発行するには

1. Azure コンピューティング エミュレーターを使用して、ローカル開発環境でアプリケーションをテストするには、Web ロールの Azure プロジェクトのショートカット メニューを開き、**[スタートアップ プロジェクトに設定]** を選択します。 次に、**[デバッグ]**、**[デバッグの開始]** の順に選択します (または、キーボードの **F5** キーを押します)。

    **[Azure デバッグ環境の起動]** ダイアログ ボックスが開き、ブラウザーでアプリケーションが起動します。 コンピューティング エミュレーターで各種 Web アプリケーションを起動する方法の詳細については、このセクションの表をご覧ください。

1. Azure に発行するアプリケーションのサービスを設定するには、Microsoft アカウントと Azure サブスクリプションが必要です。 次のトピックの手順を使用して、サービスを設定します。 [発行または Visual Studio から Azure アプリケーションをデプロイするための準備](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md)します。

1. Web アプリケーションを Azure に発行するには、Web プロジェクトのショートカット メニューを開き、**[Azure への発行]** を選択します。

    **[Azure アプリケーションの公開]** ダイアログ ボックスが開き、Visual Studio でデプロイメント プロセスが開始されます。 セクションを参照して、アプリケーションを発行する方法の詳細については、 **Visual Studio から Azure アプリケーションを発行** で [Azure ツールを使用してクラウド サービスの発行](vs-azure-tools-publishing-a-cloud-service.md)します。
    >[AZURE.NOTE] Web アプリケーションは Azure プロジェクトから発行することもできます。 そのためには、Azure プロジェクトのショートカット メニューを開き、**[発行]** を選択します。

1. デプロイメントの進行状況は、**[Microsoft Azure のアクティビティ ログ]** ウィンドウで確認できます。 このログは、デプロイメント プロセスの開始時に自動的に表示されます。 次の図に示すように、アクティビティ ログの行項目を展開して詳細情報を表示できます。

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (省略可能) デプロイメント プロセスをキャンセルするには、アクティビティ ログの行項目のショートカット メニューを開き、**[取り消して削除]** を選択します。 これにより、デプロイメント プロセスが停止し、Azure からデプロイメント環境が削除されます。
    >[AZURE.NOTE] このデプロイメント環境をデプロイ後に削除するには、Microsoft Azure 管理ポータルを使用する必要があります。

1. (省略可能) ロール インスタンスが起動すると、Visual Studio の **Cloud Explorer** または**サーバー エクスプローラー**の **[Azure Compute]** ノードにデプロイメント環境が自動的に表示されます。 ここから、個々のロール インスタンスの状態を確認できます。

    次の図は、まだ初期化中の状態である**サーバー エクスプローラー**内のロール インスタンスを示しています。

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)

1. デプロイメント後、アプリケーションにアクセスするには、**[Microsoft Azure のアクティビティ ログ]** に **[完了]** 状態が表示されているときに、デプロイメントの横の矢印をクリックします。 これにより、Azure での Web アプリケーションの URL が表示されます。 Azure から特定の種類の Web アプリケーションを起動する方法の詳細については、次の表をご覧ください。

    Azure から特定の Web アプリケーションを起動する方法、または Azure コンピューティング エミュレーターを使用して Web アプリケーションをローカルで実行またはデバッグする方法の詳細を次の表に示します。

   | Web アプリケーションの種類| コンピューティング エミュレーターを使用したローカルでの実行/デバッグ| Azure での実行|
   |---|---|---|
   | ASP.NET Web アプリケーション| メニュー バーで、**[デバッグ]**、**[デバッグの開始]** の順に選択します (または、キーボードの **F5** キーを押します)。| **[Microsoft Azure のアクティビティ ログ]** の **[デプロイメント]** タブに表示される URL ハイパーリンクを選択して、ブラウザーでスタート ページを読み込みます。|
   | ASP.NET MVC 2 Web アプリケーション| メニュー バーで、**[デバッグ]**、**[デバッグの開始]** の順に選択します (または、キーボードの **F5** キーを押します)。| **[Microsoft Azure のアクティビティ ログ]** の **[デプロイメント]** タブに表示される URL ハイパーリンクを選択して、ブラウザーでスタート ページを読み込みます。|
   | ASP.NET MVC 3 Web アプリケーション| メニュー バーで、**[デバッグ]**、**[デバッグの開始]** の順にクリックします (または、キーボードの **F5** キーを押します)。| **[Microsoft Azure のアクティビティ ログ]** の **[デプロイメント]** タブに表示される URL ハイパーリンクを選択して、ブラウザーでスタート ページを読み込みます。|
   | ASP.NET MVC 4 Web アプリケーション| メニュー バーで、**[デバッグ]**、**[デバッグの開始]** の順に選択します (または、キーボードの **F5** キーを押します)。| **[Microsoft Azure のアクティビティ ログ]** の **[デプロイメント]** タブに表示される URL ハイパーリンクを選択して、ブラウザーでスタート ページを読み込みます。|
   | 空の ASP.NET Web アプリケーション| Web プロジェクトのスタート ページとして設定したアプリケーションに .aspx ページを追加する必要があります。次にメニュー バーで、**[デバッグ]**、**[デバッグの開始]** の順に選択します (または、キーボードの **F5** キーを押します)。| アプリケーションで既定の .aspx ページを使用している場合は、**[Microsoft Azure のアクティビティ ログ]** の **[デプロイメント]** タブに表示される URL ハイパーリンクを選択すると、ブラウザーでそのページが読み込まれます。別の .aspx ページがあれば、この url は、次の形式を使用して特定のページに移動する必要があります: `< デプロイの url >/< ページの名前 > .aspx`|
   | Silverlight アプリケーション| メニュー バーで、**[デバッグ]**、**[デバッグの開始]** の順にクリックします (または、キーボードの **F5** キーを押します)。| Url は、次の形式を使用して、アプリケーションの特定のページに移動する必要があります: `< デプロイの url >/< ページの名前 > .aspx`|
   | Silverlight ビジネス アプリケーション| メニュー バーで、**[デバッグ]**、**[デバッグの開始]** の順に選択します (または、キーボードの **F5** キーを押します)。| Url は、次の形式を使用して、アプリケーションの特定のページに移動する必要があります: `< デプロイの url >/< ページの名前 > .aspx`|
   | Silverlight ナビゲーション アプリケーション| メニュー バーで、**[デバッグ]**、**[デバッグの開始]** の順に選択します (または、キーボードの **F5** キーを押します)。| Url は、次の形式を使用して、アプリケーションの特定のページに移動する必要があります:`< デプロイの url >/< ページの名前 > .aspx`|
   | WCF サービス アプリケーション| .svc ファイルを WCF サービス プロジェクトのスタート ページとして設定する必要があります。次にメニュー バーで、**[デバッグ]**、**[デバッグの開始]** の順に選択します (または、キーボードの **F5** キーを押します)。| Url は、次の形式を使用して、アプリケーションの svc ファイルに移動する必要があります: `< デプロイの url >/< サービス ファイルの名前 > .svc`|
   | WCF ワークフロー サービス アプリケーション| .svc ファイルを WCF サービス プロジェクトのスタート ページとして設定する必要があります。次にメニュー バーで、**[デバッグ]**、**[デバッグの開始]** の順に選択します (または、キーボードの **F5** キーを押します)。| Url は、次の形式を使用して、アプリケーションの svc ファイルに移動する必要があります: `< デプロイの url >/< サービス ファイルの名前 > .svc`|
   | ASP.NET 動的エンティティ| メニュー バーで、**[デバッグ]**、**[デバッグの開始]** の順に選択します (または、キーボードの **F5** キーを押します)。| 接続文字列を更新する必要があります (次のセクションを参照)。Url は、次の形式を使用して、アプリケーションの特定のページに移動する必要があります: `< デプロイの url >/< ページの名前 > .aspx`|
   | ASP.NET 動的データ LINQ to SQL| メニュー バーで、**[デバッグ]**、**[デバッグの開始]** の順に選択します (または、キーボードの **F5** キーを押します)。| 「アプリケーションでの Azure SQL Database の使用」の手順に従う必要があります (このトピックの前のセクションを参照)。Url は、次の形式を使用して、アプリケーションの特定のページに移動する必要があります: `< デプロイの url >/< ページの名前 > .aspx`|


## ASP.NET 動的エンティティの接続文字列の更新

### ASP.NET 動的エンティティの接続文字列を更新するには

1. ASP.NET 動的エンティティ Web アプリケーションで使用できる SQL Azure データベースを作成するには、前述の「**アプリケーションでの Azure SQL Database の使用**」の手順に従います。

1. Microsoft Azure 管理ポータルからこのデータベースに必要なフィールドとテーブルを追加します。

1. この種類のアプリケーションの接続文字列は、web.config ファイルで次の形式になっています。

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    SQL Azure データベースの ADO.NET 接続文字列の *connectionString* 値を次のように更新します。

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

1. 接続文字列を変更した web.config ファイルを保存するには、メニュー バーで **[ファイル]**、**[web.config の保存]** の順に選択します。

## サポートされているプロジェクト テンプレート

Azure に Web アプリケーションを発行するには、次の表に記載されている C# または Visual Basic のプロジェクト テンプレートのいずれかをアプリケーションで使用する必要があります。

| プロジェクト テンプレート グループ| プロジェクト テンプレート|
|---|---|
| Web| ASP.NET Web アプリケーション|
| Web| ASP.NET MVC 2 Web アプリケーション|
| Web| ASP.NET MVC 3 Web アプリケーション|
| Web| ASP.NET MVC 4 Web アプリケーション|
| Web| 空の ASP.NET Web アプリケーション|
| Web| 空の ASP.NET MVC 2 Web アプリケーション|
| Web| ASP.NET 動的データ エンティティ Web アプリケーション|
| Web| ASP.NET 動的データ LINQ to SQL Web アプリケーション|
| Silverlight| Silverlight アプリケーション|
| Silverlight| Silverlight ビジネス アプリケーション|
| Silverlight| Silverlight ナビゲーション アプリケーション|
| WCF| WCF サービス アプリケーション|
| WCF| WCF ワークフロー サービス アプリケーション|
| ワークフロー| WCF ワークフロー サービス アプリケーション|

## 次のステップ

発行の詳細については、次を参照してください。 [発行または Visual Studio から Azure アプリケーションをデプロイするための準備](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md)します。 チェック アウト
[名前付き認証資格情報の設定](vs-azure-tools-setting-up-named-authentication-credentials.md)します。





