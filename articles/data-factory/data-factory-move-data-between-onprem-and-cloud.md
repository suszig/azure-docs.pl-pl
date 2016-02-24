<properties 
    pageTitle="Azure Data Factory を使用してオンプレミスとクラウドの間でデータを移動する" 
    description="Data Management Gateway と Azure Data Factory を使用してオンプレミスとクラウドの間でデータを移動する方法を説明します。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/29/2015" 
    ms.author="spelluru"/>

# Data Management Gateway を使用してオンプレミスのソースとクラウドの間でデータを移動する
最新のデータ統合の課題の 1 つは、オンプレミスとクラウドの間でシームレスにデータを移動することです。 Data Factory は、Data Management Gateway によってこの統合をシームレスにします。 Data factory 管理ゲートウェイは、オンプレミス ハイブリッド パイプラインを有効にするをインストールできるエージェントです。

この記事では、オンプレミスのデータ ストアとクラウド データ ストアの統合および Data Factory を使用したクラウド処理の概要について説明します。 この記事に基づき、 [データ移動アクティビティ](data-factory-data-movement-activities.md) 資料とその他のデータ ファクトリ コア概念に関する記事です。 次の概要では、パイプライン、アクティビティ、データセット、コピー アクティビティなどの Data Factory の概念は既に理解しているものとします。

Data Gateway には次の機能が備わっています。

1.  同じ Data Factory 内にオンプレミスのデータ ソースとクラウドのデータ ソースをモデル化して、データを移動します。
2.  Data Factory のクラウド ダッシュボードにゲートウェイのステータスを表示して、一元的に監視および管理を行います。
3.  オンプレミスのデータ ソースへの安全なアクセスを管理します。
    1. 企業のファイアウォールを変更する必要はありません。 Gateway は、インターネットを開くために送信 HTTP ベースの接続のみを行います。
    2. 証明書でオンプレミスのデータ ストアの資格情報を暗号化します。
4.  データを効率的に移動します。データは並列に転送されます。自動再試行ロジックにより、断続的なネットワークの問題にも対応できます。

## Data Management Gateway を使用する上で考慮すること
1.  Data Management Gateway の 1 つのインスタンスが複数の内部設置型データ ソースを使用できますに注意してください **1 つの gateway インスタンスが 1 つだけの Azure データ ファクトリに関連付けられている** 別の data factory を使って共有ことはできません。
2.  した **Data Management Gateway のインスタンスを 1 つだけ** 1 台のコンピューターにインストールされています。 例えば、オンプレミスのデータ ソースにアクセスする必要があるデータ ファクトリが 2 つあるとすると、オンプレミスのコンピューター 2 台にゲートウェイをインストールし、各ゲートウェイを別々のデータ ファクトリに結び付ける必要があります。
3.   **ゲートウェイはデータ ソースと同じコンピューター上にする必要はありません**, 、データ ソースへの接続にゲートウェイの時間が短縮の近くに、データ ソースができます。 ゲートウェイとデータ ソースの間でリソースの競合が発生しないように、オンプレミス データ ソースをホストするコンピューターとは異なるコンピューターにゲートウェイをインストールすることをお勧めします。
4.  した **同じオンプレミス データ ソースに接続する異なるコンピューターで複数のゲートウェイ**します。 たとえば、2 つのデータ ファクトリを提供する 2 つのゲートウェイがあり、どちらのデータ ファクトリにも同じオンプレミス データ ソースが登録されている場合があります。
5.  コンピューター サービスにインストールされているゲートウェイがあれば、 **Power BI** シナリオでは、インストールしてください、 **Azure Data Factory 用のゲートウェイ** を別のコンピューターです。
6.  必要な **ExpressRoute を使用する場合でも、ゲートウェイを使用して**します。 
7.  (つまり、ファイアウォールの内側) 内部設置型データ ソースとして使用するデータ ソースを扱う必要があります使用する場合でも **ExpressRoute** と **ゲートウェイを使用して** サービスとデータ ソースの間の接続を確立します。 

## ゲートウェイのインストール - 前提条件
1.  サポートされている **オペレーティング システム** バージョンが Windows 7、Windows 8/8.1、Windows Server 2008 R2、Windows Server 2012、Windows Server 2012 R2。
2.  推奨される **構成** ゲートウェイ マシンは、少なくとも 2 GHz、4 コア、8 GB の RAM、80 GB のディスクです。
3.  ホスト コンピューターが休止状態の場合、ゲートウェイはデータ要求に応答できません。 そのため、適切な構成 **電源プラン** ゲートウェイをインストールする前にコンピューターにします。 コンピューターが休止状態に構成されている場合、ゲートウェイのインストール時にメッセージが表示されます。

コピー アクティビティは特定の頻度で実行するので、コンピューターのリソース (CPU、メモリ) の使用も同じピークとアイドルのパターンに従います。 リソース使用率はまた、移動されるデータの量に大きく依存します。 複数のコピー ジョブが進行中のとき、ピーク時にはリソース使用率が上昇するのがわかります。 上記は最小限の構成ですが、データ移動の特定の負荷に応じて、最小構成より多くのリソースを構成することを常にお勧めします。

## インストール
Data Management Gateway は、Microsoft ダウンロード センターから MSI セットアップ パッケージをダウンロードしてインストールできます。  MSI は、すべての設定を保持しながら、既存の Data Management Gateway を最新のバージョンにアップグレードするためにも使用できます。 以下の手順に従って、Azure クラシック ポータルから MSI パッケージへのリンクを見つけることができます。

### インストールのベスト プラクティス:
1.  コンピューターが休止状態にならないように、ゲートウェイ用のホスト コンピューターの電源プランを構成します。 ホスト コンピューターが休止状態の場合、ゲートウェイはデータ要求に応答できません。
2.  ゲートウェイに関連付けられている証明書をバックアップする必要があります。

### インストールのトラブルシューティング:
ファイアウォールまたはプロキシ サーバーを使用している場合、Data Management Gateway が Microsoft クラウド サービスに接続できないときは追加の手順が必要になることがあります。 

#### イベント ビューアーでの Gateway ログの確認:

Gaetway 構成マネージャー アプリケーションでは、「切断」または「接続」のようにゲートウェイのステータスが表示されます。

さらに詳細な情報は、Windows イベント ログのゲートウェイ ログで確認できます。 Windows を使用して、それらを検索することができます **イベント ビューアー** [ **アプリケーションとサービス ログ** > **Data Management Gateway** トラブルシューティング ゲートウェイ関連のイベント ビューアーのエラー レベルのイベント、懸案事項が参照されます。


#### ファイアウォール関連の問題で発生する可能性がある症状:

1. ゲートウェイを登録しようとすると次のエラーが発生します。「ゲートウェイのキーを登録できませんでした。 ゲートウェイ キーの登録を再試行する前に、Data Management Gateway が接続状態で、Data Management Gateway Host Service が起動していることを確認してください。」
2. 構成マネージャーを開くと、ステータスは「切断」または「接続」と表示されます。 [イベント ビューアー]、[アプリケーションとサービス ログ]、[Data Management Gateway] で Windows イベント ログを表示すると、「リモート サーバーに接続できません」や「Data Management Gateway のコンポーネントが応答しなくなり、自動的に再起動されます。 コンポーネント名: Gateway」といったエラー メッセージが表示されます。

これらの原因はファイアウォールまたはプロキシ サーバーの不適切な構成であり、Data Management Gateway がクラウド サービスに接続して認証するのをブロックしています。

スコープ内にある可能性のある 2 つのファイアウォールは、組織の中央ルーターで実行している企業ファイアウォールと、ゲートウェイがインストールされているローカル コンピューター上のデーモンとして構成されている Windows ファイアウォールです。 次に、いくつか考慮事項を示します。

- 企業ファイアウォールの受信ポリシーを変更する必要はありません。
- 企業ファイアウォールと Windows ファイアウォールはどちらも、TCP ポート 80、443、9350 ～ 9354 での送信ルールを有効にする必要があります。 これらは、クラウド サービスと Data Management Gateway の間の接続を確立するために Microsoft Azure Service Bus で使用されます。

MSI のセットアップでは、ゲートウェイ コンピューターに対する受信ポートの Windows ファイアウォール ルールが自動的に構成されます (前の「ポートとセキュリティに関する考慮事項」を参照)。

ただし、セットアップでは、上記の送信ポートがローカル コンピューターと企業ファイアウォールにおいて既定で許可されるものと仮定しています。 そうでない場合は、これらの送信ポートを有効にする必要があります。 サードパーティ製のファイアウォールで Windows ファイアウォールを置き換えた場合は、これらのポートを手動で開く必要がある場合があります。 

会社でプロキシ サーバーを使用している場合は、Microsoft Azure をホワイト リストに追加する必要があります。 有効な Microsoft Azure の IP アドレスの一覧をダウンロードすることができます、 [Microsoft ダウンロード センター](http://msdn.microsoft.com/library/windowsazure/dn175718.aspx)します。

## Data Gateway の使用手順
このチュートリアルでは、オンプレミスの SQL Server Database から、Azure BLOB にデータを移動するパイプラインを備えたデータ ファクトリを作成します。 

### 手順 1. Azure Data Factory を作成する
この手順でという名前の Azure Data Factory インスタンスを作成する Azure クラシック ポータルを使用して **ADFTutorialOnPremDF**します。 Azure Data Factory のコマンドレットを使用してデータ ファクトリを作成することもできます。 

1.  ログインした後、 [Azure ポータル](https://portal.azure.com), 、] をクリックして **新規** の左下隅から選択 **データ分析** で、 **作成** ブレードをクリック **Data Factory** 上、 **データ分析** ブレードです。

    ![[新規] -> [DataFactory]](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png) 
  
6.  **新しいデータ ファクトリ** ブレード。
    1. 入力 **ADFTutorialOnPremDF** の **名前**します。
    2. クリックして **リソース グループ名** 選択 **ADFTutorialResourceGroup**します。 既存のリソース グループを選択するか、新しく作成することができます。 新しいリソース グループを作成するには:
        1. クリックして **新しいリソース グループを作成**します。
        2.  **リソース グループの作成] ブレード**, 、入力、 **名前** リソース グループとクリック **[ok]**します。

7. なお **スタート画面に追加** がオンになって、 **新しいデータ ファクトリ** ブレードです。

    ![スタート画面への追加](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

8.  **新しいデータ ファクトリ** ブレードで、をクリックして **作成**します。

    Azure Data Factor の名前はグローバルに一意にする必要があります。 エラーが発生する場合: **データ ファクトリ名"ADFTutorialOnPremDF"は使用できません**, 、data factory (yournameADFTutorialOnPremDF など) の名前を変更し、もう一度作成してください。 このチュートリアルでは以降の手順の実行中に、この名前を ADFTutorialOnPremDF の代わりに使用します。  

9. クリックして、作成プロセスからの通知を探します、 **通知** 次の図のように、タイトル バー ボタンをクリックします。 通知ウィンドウを閉じるには、もう一度クリックします。 

    ![[通知] ハブ](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNotificationsHub.png)

11. 表示の作成が完了した後、 **Data Factory** ブレードが次のようにします。

    ![Data Factory ホーム ページ](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

### 手順 2. Data Management Gateway を作成する
5.  **DATA FACTORY** ブレードで、をクリックして **作成者および展開** を起動するタイル、 **エディター** data factory のです。

    ![タイルの作成とデプロイ](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
6.  Data Factory エディターをクリックして **.(省略記号)** クリックしてツールバー **新しいデータ ゲートウェイ**します。 

    ![ツールバーでのデータ ゲートウェイの新規作成](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
2.  **作成** ブレードで、入力 **adftutorialgateway** の **名前**, 、] をクリック **[ok]**します。    

    ![[ゲートウェイの作成] ブレード](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

3.  **構成** ブレードで、をクリックして **このコンピューターに直接インストール**します。 これにより、ゲートウェイのインストール パッケージのダウンロードと、コンピューターへのインストール、構成、および登録が行われます。  

    > [AZURE.NOTE] Internet Explorer または Microsoft ClickOnce 互換の web ブラウザーを使用してください。

    ![[ゲートウェイ - 構成 ブレード](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    これは、たった 1 つの手順 (クリック 1 回) でゲートウェイのダウンロード、インストール、構成、および登録を行う、最も簡単な方法です。 ご覧の **Microsoft Data Management Gateway 構成マネージャー** お使いのコンピューターにアプリケーションをインストールします。 実行可能ファイルを検索することもできます。 **ConfigManager.exe** フォルダー内: **C:\Program files \microsoft Data Management Gateway\1.0\Shared**します。

    またをダウンロードし、このブレード内のリンクを使用してゲートウェイを手動でインストールおよび登録できるに示すようにキーを使用して、 **キーで登録** テキスト ボックスです。
    
    ベスト プラクティスや重要な考慮事項など、ゲートウェイの詳細については、この記事の最初の方のセクションを参照してください。

    >[AZURE.NOTE] インストールし、Data Management Gateway を正常に構成するには、ローカル コンピューターの管理者があります。 他のユーザーをローカル Windows グループの Data Management Gateway Users に追加できます。 このグループのメンバーは、Data Management Gateway 構成マネージャー ツールを使用して、ゲートウェイを構成できます。 

5. 数分待ってから、起動 **Data Management Gateway 構成マネージャー** コンピューターにアプリケーションです。  **検索** ウィンドウに、「 **Data Management Gateway** このユーティリティにアクセスしますします。 また、実行可能ファイルを検索することもできます **ConfigManager.exe** フォルダー内: **C:\Program files \microsoft Data Management Gateway\1.0\Shared。** 

    ![Gateway Configuration Manager](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)

6. 値が以下の状態になるまで待ちます。
    1. **ステータス** に設定されている **開始**します。
    2. **ゲートウェイ名** に設定されている **adftutorialgateway**します。
    3. **インスタンス名** に設定されている **adftutorialgateway**します。
    4. **登録** に設定されている **登録されている**します。
    5. ステータス バーの下部にある表示 **Data Management Gateway クラウド サービスに接続されている** と共に、 **緑色のチェック マーク**します。

8. 切り替えて、 **証明書** ] タブをクリックします。 このタブで指定された証明書は、ポータルで指定したオンプレミス データ ストアの資格情報の暗号化/暗号解除に使用されます。 クリックして **変更** 代わりに独自の証明書を使用します。 既定では、ゲートウェイは Data Factory サービスによって自動生成される証明書を使用します。

    ![ゲートウェイ証明書の構成](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)
9. (省略可能)切り替えて、 **診断** チェック] タブ、 **トラブルシューティングのための詳細ログ記録を有効にする** ゲートウェイに関する問題のトラブルシューティングに使用できる詳細なログ記録を有効にする場合は、オプションです。 ログ情報は記載されて **イベント ビューアー** [ **アプリケーションとサービス ログ** ]-> [ **Data Management Gateway** ノードです。 

    ![[診断] タブ](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    このページを使用することもできます。 **接続をテストする** ゲートウェイを使用する内部設置型のデータ ソースにします。
10. Azure クラシック ポータルでをクリックして **OK** に、 **構成** ブレードし、 **新しいデータ ゲートウェイ** ブレードです。
6. はず **adftutorialgateway** [ **データ ゲートウェイ** 左側のツリー ビューで。  クリックすると、関連する JSON が表示されます。 
    

### 手順 3: リンクされたサービスを作成する 
この手順では、2 つのリンクされたサービスを作成します: **StorageLinkedService** と **SqlServerLinkedService**します。  **SqlServerLinkedService** 内部設置型 SQL Server データベースをリンクし、 **StorageLinkedService** リンクされたサービスでは、Azure blob ストアをデータ ファクトリにリンクします。 このチュートリアルの後半で、オンプレミスの SQL Server Database から Azure BLOB ストアにデータをコピーするパイプラインを作成します。 

#### オンプレミスの SQL Server Database にリンクされたサービスを追加する
1.   **Data Factory エディター**, 、] をクリックして **新しいデータ ストア** ツールバーの選択 [ **SQL Server**します。 

    ![SQL Server のリンクされているサービス](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png) 
3.   **JSON エディター**, 、次の操作を行います。 
    1.  **GatewayName**, 、指定 **adftutorialgateway**します。 
    2. Windows 認証を使用している場合:
        1.  **ConnectionString**: 
            1. 設定、 **統合セキュリティ** に **true**.
            2. データベースを指定 **サーバー名** と **データベース名**します。 
            2. 削除 **ユーザー ID** と **パスワード**します。 
        3. ユーザー名とパスワードを指定 **userName** と **パスワード** プロパティです。
        
                "typeProperties": {
                    "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;",
                    "gatewayName": "adftutorialgateway",
                    "userName": "<Specify user name if you are using Windows Authentication>",
                    "password": "<Specify password for the user account>"
                }

    4. SQL 認証を使用している場合:
        1. データベースを指定 **サーバー名**, 、**データベース名**, 、**ユーザー ID**, 、および **パスワード** で、 **connectionString**します。       
        2. 最後の 2 つの JSON プロパティの削除 **userName** と **パスワード** - JSON からです。
        3. 末尾の削除 **, 、(コンマ)** の値を指定する行の最後に、 **gatewayName** プロパティです。 

                "typeProperties": {
                    "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
                    "gatewayName": "<Name of the gateway that the Data Factory service should use to connect to the on-premises SQL Server database>"
                }
       
2.  クリックして **展開** [SQL Server を展開するコマンド バーのリンクされたサービスです。 

#### Azure ストレージ アカウント用のリンクされたサービスを追加する
 
1.  **Data Factory エディター**, をクリックして **新しいデータ ストア** コマンド バーで **Azure ストレージ**します。
2. Azure ストレージ アカウントの名前を入力、 **アカウント名**します。
3. Azure ストレージ アカウントのキーを入力、 **アカウント キー**します。
4. クリックして **展開** を展開する、 **StorageLinkedService**します。
   
 
### 手順 4: 入力データセットと出力データセットを作成する
このステップでは、コピー操作 (オンプレミスの SQL Server Database => Azure BLOB ストレージ) の入力および出力データを表す入出力データ セットを作成します。 データセットまたはテーブル (四角形のデータセット) を作成する前に、以下を実施する必要があります (一覧の後に詳細な手順があります)。

- という名前のテーブルを作成する **emp** SQL Server データベース内に追加したリンクされたサービスとしてデータをテーブルにサンプル エントリ ファクトリと挿入をいくつか。
- という名前の blob コンテナーを作成する **adftutorial** Azure blob ストレージ アカウントのデータ ファクトリにリンクされたサービスとして追加します。

### チュートリアル用にオンプレミスの SQL Server を用意します。

1. 内部設置型の指定したデータベースで SQL Server リンク サービス (**SqlServerLinkedService**)、作成する次の SQL スクリプトを使用して、 **emp** データベース内のテーブルです。


        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL, 
            FirstName varchar(50),
            LastName varchar(50),
            CONSTRAINT PK_emp PRIMARY KEY (ID)
        )
        GO
 

2. テーブルにサンプルをいくつか挿入します。 


        INSERT INTO emp VALUES ('John', 'Doe')
        INSERT INTO emp VALUES ('Jane', 'Doe')



### 入力テーブルの作成

1.  **Data Factory エディター**, をクリックして **新しいデータセット** コマンド バーで **SQL Server テーブル**します。 
2.  右側のウィンドウの JSON を次のテキストに置き換えます。    

        {
          "name": "EmpOnPremSQLTable",
          "properties": {
            "type": "SqlServerTable",
            "linkedServiceName": "SqlServerLinkedService",
            "typeProperties": {
              "tableName": "emp"
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
              }
            }
          }
        }

    以下の点に注意してください。 
    
    - **型** に設定されている **SqlServerTable**します。
    - **tableName** に設定されている **emp**します。
    - **linkedServiceName** に設定されている **SqlServerLinkedService** (手順 2. でこのリンクされたサービスを作成した)。
    - 設定する必要が別の Azure Data Factory パイプラインでは生成されない入力テーブル、 **外部** に **true**します。 これは、入力データが Azure Data Factory サービスの外部で生成されることを意味します。 使用して外部データ ポリシーを指定することもできます。、 **externalData** 内の要素、 **ポリシー** セクションです。    

    JSON プロパティの詳細については、[JSON スクリプト リファレンス][json-script-reference]を参照してください。

2. クリックして **展開** 、データセットをデプロイ コマンド バーで (テーブルは四角形のデータセットです)。 確認を示すタイトル バーにメッセージを参照してください **テーブルが正常にデプロイされました**します。 


### 出力テーブルの作成

1.   **Data Factory エディター**, 、クリックして **新しいデータセット** コマンド バーで **Azure Blob ストレージ**します。
2.  右側のウィンドウの JSON を次のテキストに置き換えます。 

        {
          "name": "OutputBlobTable",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
              "folderPath": "adftutorial/outfromonpremdf",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }
  
    以下の点に注意してください。 
    
    - **型** に設定されている **AzureBlob**します。
    - **linkedServiceName** に設定されている **StorageLinkedService** (手順 2. でこのリンクされたサービスを作成した)。
    - **folderPath** に設定されている **adftutorial/outfromonpremdf** outfromonpremdf は adftutorial コンテナー内のフォルダーです。 だけを作成する必要がある、 **adftutorial** コンテナーです。
    -  **可用性** に設定されている **毎時** (**頻度** に設定 **時間** と **間隔** に設定 **1**)。  Data Factory サービスは、1 時間ごとに出力データ スライスには生成、 **emp** Azure SQL データベースのテーブルです。 

    指定しない場合、 **fileName** の **入力テーブル**, 、入力フォルダーからすべてのファイルまたは blob (**folderPath**) の入力と見なされます。 JSON で fileName を指定した場合は、指定されたファイル/BLOB のみが入力と見なされます。 例については、[チュートリアル][adf-tutorial]のサンプル ファイルを参照してください。
 
    指定しない場合、 **ファイル名** の **出力テーブル**, 、生成されるファイルに、 **folderPath** 次の形式で名前が: データ。<Guid>します。txt (例:: 付けられます-93ff-4c6f-b3be-f69616f1df7a.txt。)。

    設定する **folderPath** と **fileName** に基づいて動的に、 **SliceStart** 今度は、partitionedBy プロパティを使用します。 次の例では、folderPath に SliceStart (処理されるスライスの開始時刻) の年、月、日を使用し、fileName に SliceStart の時間を使用します。 たとえば、スライスが 2014-10-20T08:00:00 に生成されている場合、folderName は wikidatagateway/wikisampledataout/2014/10/20 に設定され、fileName は 08.csv に設定されます。 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

 

    JSON プロパティの詳細については、[JSON スクリプト リファレンス][json-script-reference]を参照してください。

2.  クリックして **展開** 、データセットをデプロイ コマンド バーで (テーブルは四角形のデータセットです)。 確認を示すタイトル バーにメッセージを参照してください **テーブルが正常にデプロイされました**します。
  

### 手順 5: パイプラインを作成して実行する
この手順で作成、 **パイプライン** いずれかで **コピー アクティビティ** を使用する **EmpOnPremSQLTable** 入力としてと **OutputBlobTable** として出力します。

1.   **DATA FACTORY** ブレードで、をクリックして **作成者および展開** を起動するタイル、 **エディター** data factory のです。

    ![タイルの作成とデプロイ](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
2.  クリックして **新しいパイプライン** コマンド バーにします。 ボタンが表示されない場合はクリックして **.(省略記号)** コマンド バーを展開します。
2.  右側のウィンドウの JSON を次のテキストに置き換えます。   


        {
          "name": "ADFTutorialPipelineOnPrem",
          "properties": {
            "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
            "activities": [
              {
                "name": "CopyFromSQLtoBlob",
                "description": "Copy data from on-prem SQL server to blob",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "EmpOnPremSQLTable"
                  }
                ],
                "outputs": [
                  {
                    "name": "OutputBlobTable"
                  }
                ],
                "typeProperties": {
                  "source": {
                    "type": "SqlSource",
                    "sqlReaderQuery": "select * from emp"
                  },
                  "sink": {
                    "type": "BlobSink"
                  }
                },
                "Policy": {
                  "concurrency": 1,
                  "executionPriorityOrder": "NewestFirst",
                  "style": "StartOfInterval",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-02-13T00:00:00Z",
            "end": "2015-02-14T00:00:00Z",
            "isPaused": false
          }
        }

    Note the following:
 
    - In the activities section, there is only activity whose **type** is set to **Copy**.
    - **Input** for the activity is set to **EmpOnPremSQLTable** and **output** for the activity is set to **OutputBlobTable**.
    - In the **transformation** section, **SqlSource** is specified as the **source type** and **BlobSink **is specified as the **sink type**.
    - SQL query **select * from emp** is specified for the **sqlReaderQuery** property of **SqlSource**.

    Replace the value of the **start** property with the current day and **end** value with the next day. Both start and end datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601). For example: 2014-10-14T16:32:41Z. The **end** time is optional, but we will use it in this tutorial. 
    
    If you do not specify value for the **end** property, it is calculated as "**start + 48 hours**". To run the pipeline indefinitely, specify **9/9/9999** as the value for the **end** property. 
    
    You are defining the time duration in which the data slices will be processed based on the **Availability** properties that were defined for each Azure Data Factory table.
    
    In the example above, there will be 24 data slices as each data slice is produced hourly.
    
2. クリックして **展開** 、データセットをデプロイ コマンド バーで (テーブルは四角形のデータセットです)。 確認を示すタイトル バーにメッセージを参照してください **パイプラインが正常にデプロイされました**します。  
5. これで、閉じる、 **エディター** ブレードをクリックして **X**です。 クリックして **X** ツールバーおよびツリー ビューで [ADFTutorialDataFactory] ブレードを閉じます。 表示される場合 **、未保存の編集は破棄されます** メッセージで、をクリックして **OK**します。
6. する必要があります、 **DATA FACTORY** ブレード、 **ADFTutorialOnPremDF**します。

**おめでとうございます!**これで、Azure Data Factory、リンクされたサービス、テーブル、およびパイプラインの作成と、パイプラインのスケジュール設定が完了しました。

#### ダイアグラム ビューでの Data Factory の表示 
1.  **Azure ポータル**, 、クリックして **ダイアグラム** のホーム ページでタイル、 **ADFTutorialOnPremDF** データ ファクトリ。 :

    ![Diagram Link](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)

2. 以下のような図が表示されるはずです。

    ![Diagram View](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    パイプラインとテーブルは、拡大、縮小、100% に拡大、ウィンドウのサイズに合わせて大きさを変更、自動的に配置などの表示が可能です。また、系列情報を表示 (選択した項目の上位項目や下位項目を強調表示) することもできます。  オブジェクト (入力/出力テーブルまたはパイプライン) をダブルクリックすると、そのオブジェクトのプロパティを表示できます。 

### 手順 6: データセットとパイプラインを監視する
このステップでは、Azure クラシック ポータルを使用して、Azure Data Factory の状況を監視します。 PowerShell コマンドレットを使用して、データセットとパイプラインを監視することもできます。 監視する方法の詳細については、「 [モニターおよび管理のパイプライン](monitor-manage-pipelines.md)します。

1. 移動 **Azure ポータル** (閉じていた) の場合
2. 場合ブレード **ADFTutorialOnPremDF** いないを開きをクリックして開くには **ADFTutorialOnPremDF** 上、 **スタート画面**します。
3. 表示されます、 **カウント** と **名** テーブルとパイプラインのこのブレード上に作成したのです。

    ![Data Factory ホーム ページ](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)
4. をクリックして **データセット** を並べて表示します。
5.  **データセット** ブレードで、をクリックして、 **EmpOnPremSQLTable**します。

    ![EmpOnPremSQLTable スライス](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)

6. 現在の時刻までのデータ スライスが既に生成されており、 **準備**します。 これは、SQL Server Database に挿入したデータが、現在まで残っているためです。 スライス表示されていないことの確認、 **問題のあるスライス** 下部のセクションです。


    Both **Recently updated slices** and **Recently failed slices** lists are sorted by the **LAST UPDATE TIME**. The update time of a slice is changed in the following situations. 
    

    -  You update the status of the slice manually, for example, by using the **Set-AzureRmDataFactorySliceStatus** (or) by clicking **RUN** on the **SLICE** blade for the slice.
    -  The slice changes status due to an execution (e.g. a run started, a run ended and failed, a run ended and succeeded, etc).
 
    Click on the title of the lists or **... (ellipses)** to see the larger list of slices. Click **Filter** on the toolbar to filter the slices.  
    
    To view the data slices sorted by the slice start/end times instead, click **Data slices (by slice time)** tile.

7.  **データセット** ブレードで、をクリックして **OutputBlobTable**します。

    ![OputputBlobTable スライス][image-data-factory-output-blobtable-slices]
8. 現在の時刻までのスライスが生成されることを確認し、 **準備**します。 現在の時刻までのスライスの状態に設定されるまで待機 **準備**します。
9. 一覧からいずれかのデータ スライスをクリックして表示されます、 **データ スライス** ブレードです。

    ![[データ スライス] ブレード](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    スライスのではない場合、 **準備ができて** 状態、準備ができていないし、現在のスライスの実行をブロックするアップ ストリームのスライスを確認することができます、 **準備ができていないアップ ストリーム スライス** ] ボックスの一覧です。

10. をクリックして、 **アクティビティの実行** を下にあるリストから **アクティビティ実行の詳細**します。

    ![[アクティビティの実行の詳細] ブレード][image-data-factory-activity-run-details]

11. クリックして **X** まで、すべてのブレードを閉じる 
12. ホーム ブレードに戻るため、 **ADFTutorialOnPremDF**します。
14. (省略可能)クリックして **パイプライン**, 、] をクリックして **ADFTutorialOnPremDF**, 、入力テーブルをドリルスルーしたり (**Consumed**) または出力テーブル (**Produced**)。
15. ツールを使用して **Azure ストレージ エクスプ ローラー** 、出力を確認します。

    ![Azure ストレージ エクスプローラー](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## コンピューターから別のコンピューターへのゲートウェイの移動
このセクションでは、1 台のコンピューターから別のコンピューターにゲートウェイ クライアントを移動する手順を説明します。 

2. ポータルに移動、 **Data Factory のホーム ページ**, 、] をクリックし、 **リンクされたサービス** を並べて表示します。 

    ![データ ゲートウェイ リンク](./media/data-factory-move-data-between-onprem-and-cloud/DataGatewaysLink.png) 
3. ゲートウェイを選択して、 **データ ゲートウェイ** のセクションで、 **リンクされたサービス** ブレードです。
    
    ![ゲートウェイが選択された状態の [リンクされたサービス] ブレード](./media/data-factory-move-data-between-onprem-and-cloud/LinkedServiceBladeWithGateway.png)
4.  **Data gateway** ブレードで、をクリックして **データ gateway ダウンロードしてインストール**します。
    
    ![ダウンロード ゲートウェイ リンク](./media/data-factory-move-data-between-onprem-and-cloud/DownloadGatewayLink.png) 
5.  **構成** ブレードで、をクリックして **をダウンロードし、データ ゲートウェイをインストール** マシン上のデータ ゲートウェイをインストールする指示に従います。 

    ![[構成] ブレード](./media/data-factory-move-data-between-onprem-and-cloud/ConfigureBlade.png)
6. 保持、 **Microsoft Data Management Gateway 構成マネージャー** を開きます。 
 
    ![構成マネージャー](./media/data-factory-move-data-between-onprem-and-cloud/ConfigurationManager.png)  
7.  **構成** 、ポータルのブレードをクリックして **キーの再作成** コマンド バーで **[はい]** 警告メッセージのです。 クリックして **コピーボタン** 、キーをクリップボードにコピーするキーのテキストの横にあります。 古いコンピューターのゲートウェイは、キーが再作成されるとすぐに機能を停止することに注意してください。  
    
    ![キーの再作成](./media/data-factory-move-data-between-onprem-and-cloud/RecreateKey.png)
     
8. 貼り付け、 **キー** でテキスト ボックスに、 **ゲートウェイの登録** のページ、 **Data Management Gateway 構成マネージャー** コンピューターにします。 (省略可能)クリックして **ショー ゲートウェイのキー** キーのテキストを表示する] チェック ボックスです。 
 
    ![キーのコピーと登録](./media/data-factory-move-data-between-onprem-and-cloud/CopyKeyAndRegister.png)
9. をクリックして **登録** クラウド サービスにゲートウェイを登録します。
10.  **証明書の指定** ] ページで [ **参照** 古いゲートウェイで使用されていた、同じ証明書を選択するには、入力、 **パスワード**, 、] をクリック **完了**します。 
 
    ![証明書の指定](./media/data-factory-move-data-between-onprem-and-cloud/SpecifyCertificate.png)

    古いゲートウェイから証明書をエクスポートするには、次の手順に従って: 古いコンピューターで Data Management Gateway 構成マネージャーを起動してに切り替えて、 **の証明書が** ] タブをクリックして **エクスポート** ] ボタンをクリックし、指示に従います。 
10. ゲートウェイの登録が成功後に表示されます、 **登録** に設定 **登録されている** と **ステータス** に設定 **開始** Gateway 構成マネージャーのホーム ページにします。 

## 資格情報とセキュリティの設定

SQL Server リンク サービスを作成するには、Data Factory エディターではなくリンクされたサービス ブレードを使用することができます。 
 
3.  Data Factory のホーム ページをクリックして **リンクされたサービス** を並べて表示します。 
4.   **リンクされたサービス** ブレードで、をクリックして **新しいデータ ストア** コマンド バーにします。 
4.  入力 **SqlServerLinkedService** の **名前**します。 
2.  矢印をクリックして次に、 **型**, を選択して **SQL Server**します。

    ![新しいデータ ストアの作成](./media/data-factory-move-data-between-onprem-and-cloud/new-data-store.png)
3.  次の複数の設定を作成する必要があります、 **型** 設定します。
4.   **Data gateway** ゲートウェイを作成する設定を選択します。 

    ![SQL Server の設定](./media/data-factory-move-data-between-onprem-and-cloud/sql-server-settings.png)
4.  データベース サーバーの名前を入力、 **Server** 設定します。
5.  データベースの名前を入力、 **データベース** 設定します。
6.  矢印をクリックして次に **資格情報**します。

    ![[資格情報] ブレード](./media/data-factory-move-data-between-onprem-and-cloud/credentials-dialog.png)
7.   **資格情報** ブレードで、をクリックして **ここをクリックして、資格情報を設定する**です。
8.   **資格情報の設定** ] ダイアログ ボックスで、次の操作します。

    ![[資格情報の設定] ダイアログ](./media/data-factory-move-data-between-onprem-and-cloud/setting-credentials-dialog.png)
    1.  選択 **認証** Data Factory サービスを使用して、データベースに接続するようにします。 
    2.  データベースへのアクセスを持つユーザーの名前を入力用、 **USERNAME** 設定します。 
    3.  ユーザーのパスワードを入力、 **パスワード** 設定します。  
    4.  クリックして **OK** ] ダイアログ ボックスを閉じます。 
4. をクリックして **OK** を閉じる、 **資格情報** ブレードです。 
5. をクリックして **OK** 上、 **新しいデータ ストア** ブレードです。    
6. いることを確認の状態 **SqlServerLinkedService** はリンクされたサービス] ブレードでオンラインに設定します。
    ![SQL Server リンク サービスの状態](./media/data-factory-move-data-between-onprem-and-cloud/sql-server-linked-service-status.png)

ゲートウェイ コンピューターとは異なるコンピューターからポータルにアクセスする場合は、資格情報マネージャー アプリケーションがゲートウェイ コンピューターに接続できることを確認する必要があります。 アプリケーションがゲートウェイ コンピューターにアクセスできない場合、データ ソースの資格情報を設定したり、データ ソースへの接続をテストしたりすることはできません。 

Azure クラシック ポータルから起動した [資格情報の設定] アプリケーションを使用してオンプレミスのデータ ソースの資格情報を設定すると、ポータルは、ユーザーがゲートウェイ コンピューターの Data Management Gateway Configuration Manager の [証明書] タブで指定した証明書で資格情報を暗号化します。 

使用することができます、資格情報を暗号化するための API ベース アプローチを探している場合、 [新規 AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) PowerShell コマンドレットで資格情報を暗号化します。 コマンドレットはゲートウェイに構成されている証明書を使用して資格情報を暗号化します。 このコマンドレットによって返される資格情報を暗号化することができで使用する JSON ファイルに connectionString の EncryptedCredential 要素に追加、 [新規 AzureRmDataFactoryLinkedService](https://msdn.microsoft.com/library/mt603647.aspx) コマンドレットまたはポータルで Data Factory エディターでの JSON スニペットでします。 

    "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",

**注:** 「資格情報の設定」アプリケーションを使用する場合は、自動的に設定、暗号化された資格情報、リンクされたサービスで上記のようにします。

Data Factory エディターを使用して資格情報を設定するもう 1 つの方法があります。 エディターを使用して SQL Server のリンクされたサービスを作成し、プレーン テキストで資格情報を入力した場合、資格情報は、ゲートウェイが使用するように構成されている証明書ではなく、Data Factory が所有する証明書で暗号化されます。 この方法は、場合によっては少し高速ですが、セキュリティは低下します。 したがって、この方法は開発/テストの目的のみに使用することをお勧めします。 


## Azure PowerShell を使用したゲートウェイの作成と登録 
このセクションでは、Azure PowerShell コマンドレットを使用して、ゲートウェイを作成し登録する方法について説明します。 

1. 起動 **Azure PowerShell** 管理者モードでします。 
2. Azure Data Factory コマンドレットに、 **AzureResourceManager** モードです。 切り替えるには、次のコマンドを実行、 **AzureResourceManager** モードです。     

        switch-azuremode AzureResourceManager


2. 使用して、 **新規 AzureRmDataFactoryGateway** コマンドレットを次のように論理ゲートウェイを作成します。

        New-AzureRmDataFactoryGateway -Name <gatewayName> -DataFactoryName <dataFactoryName> -ResourceGroupName ADF –Description <desc>

    **例のコマンドと出力の**:


        PS C:\> New-AzureRmDataFactoryGateway -Name MyGateway -DataFactoryName $df -ResourceGroupName ADF –Description “gateway for walkthrough”

        Name              : MyGateway
        Description       : gateway for walkthrough
        Version           :
        Status            : NeedRegistration
        VersionStatus     : None
        CreateTime        : 9/28/2014 10:58:22
        RegisterTime      :
        LastConnectTime   :
        ExpiryTime        :
        ProvisioningState : Succeeded


3. 使用して、 **新規 AzureRmDataFactoryGatewayKey** コマンドレットでは、新しく作成したゲートウェイの登録キーを生成し、キーはローカル変数に保存を **$Key**:

        New-AzureRmDataFactoryGatewayKey -GatewayName <gatewayname> -ResourceGroupName ADF -DataFactoryName <dataFactoryName>

    
    **コマンドの出力例:**


        PS C:\> $Key = New-AzureRmDataFactoryGatewayKey -GatewayName MyGateway -ResourceGroupName ADF -DataFactoryName $df 

    
4. Azure PowerShell で、フォルダーに切り替えます。 **C:\Program files \microsoft Data Management Gateway\1.0\PowerShellScript\** し、実行 **$key** ローカル変数に関連付けられているスクリプト **$Key** 前に作成した論理ゲートウェイ コンピューターにインストールされているクライアント エージェントに登録する次のコマンドに示すようにします。

        PS C:\> .\RegisterGateway.ps1 $Key.GatewayKey
        
        Agent registration is successful!

5. 使用することができます、 **Get AzureRmDataFactoryGateway** コマンドレットをデータ ファクトリ内のゲートウェイの一覧を取得します。 ときに、 **ステータス** を示しています **オンライン**, 、ゲートウェイが使用できるようになります。

        Get-AzureRmDataFactoryGateway -DataFactoryName <dataFactoryName> -ResourceGroupName ADF

使用してゲートウェイを削除することができます、 **削除 AzureRmDataFactoryGateway** を使用してゲートウェイのコマンドレットと更新プログラムの説明、 **セット AzureRmDataFactoryGateway** コマンドレットです。 これらのコマンドレットの文法やその他の詳細については「Data Factory Cmdlet Reference (Data Factory コマンドレット リファレンス)」を参照してください。  


## Data Management Gateway を使用したコピーのデータ フロー
データ パイプラインでコピー アクティビティを使用してオンプレミスのデータを処理のためにクラウドに取り込むとき、またはクラウドの結果データをオンプレミスのデータ ストアにエクスポートして戻すときは、コピー アクティビティは内部的にゲートウェイを使用してオンプレミスのデータ ソースからクラウドに、またはその逆に、データを転送します。

ここでの高レベルのデータ フローとデータ ゲートウェイでのコピー手順の概要:
![ゲートウェイを使用したデータ フロー](./media/data-factory-move-data-between-onprem-and-cloud/data-flow-using-gateway.png)

1.  データ開発者は、いずれかを使用して Azure Data Factory の新しいゲートウェイを作成、 [Azure Classic Portal](http://portal.azure.com) または [PowerShell コマンドレット](https://msdn.microsoft.com/library/dn820234.aspx)します。 
2.  データ開発者は、[リンクされたサービス] パネルを使用して、オンプレミスのデータ ストアとゲートウェイのための新しいリンクされたサービスを定義します。 リンクされたサービスの設定の一部として、データ開発者は、手順で示したように資格情報の設定アプリケーションを使用して認証の種類と資格情報を指定します。  資格情報の設定アプリケーションのダイアログは、データ ストアと通信して接続をテストし、ゲートウェイと通信して資格情報を保存します。
3.  ゲートウェイは、資格情報をクラウドに保存する前に、(開発者によって提供された) ゲートウェイと関連付けられた証明書で資格情報を暗号化します。
4.  Data Factory 移動サービスは、共有 Azure Service Bus キューを使用する制御チャネルを介して、ジョブのスケジューリングと管理のためにゲートウェイと通信します。 コピー アクティビティ ジョブを開始する必要がある場合、Data Factory はリクエストと資格情報をキューに入れます。 ゲートウェイは、キューをポーリングした後でジョブを開始します。
5.  ゲートウェイは、同じ証明書で資格情報を復号化し、適切な認証の種類を使用してオンプレミスのデータ ストアに接続します。
6.  ゲートウェイは、データ パイプラインでのコピー アクティビティの構成方法に応じて、オンプレミスのストアからクラウドのストレージに、またはクラウドのストレージからオンプレミスのデータ ストアに、データをコピーします。 注: この手順では、ゲートウェイは、セキュリティで保護された (HTTPS) チャネルを使用して、クラウド ベースのストレージ サービス (例: Azure BLOB、Azure SQL など) と直接通信します。

### ポートとセキュリティに関する考慮事項

1. ステップ バイ ステップ チュートリアルで前述したように、Data Factory でオンプレミスのデータ ストアに資格情報を設定するには複数の方法があります。 ポートの考慮事項は、これらのオプションによって異なります。 

    - 使用して、 **資格情報の設定** アプリ: 既定では、Data Management Gateway インストール プログラムを開きます **8050** と **8051** 、ゲートウェイ コンピューターのファイアウォールの [windows のローカル ポートです。 資格情報の設定アプリケーションはこれらのポートを使用して、ゲートウェイに資格情報を中継します。 これらのポートは、ローカル Windows ファイアウォール上のコンピューターに対してのみ開かれます。 これらのポートにはインターネットからは到達できないので、企業全体のファイアウォールで開く必要はありません。
    2.  使用して、 [新規 AzureRmDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) powershell コマンドレット: します。  PowerShell コマンドを使用して資格情報を暗号化しており、結果としてゲートウェイのインストールで Windows ファイアウォールのゲートウェイ コンピューターの受信ポートを開きたくない場合は、インストールの間に次のコマンドを使用して行うことができます。
    
            msiexec /q /i DataManagementGateway.msi NOFIREWALL=1
3.  使用している場合、 **資格情報の設定** アプリケーションをデータ ソースの資格情報を設定して、データ ソースへの接続をテストできるように、Data Management Gateway に接続できるコンピューターで、これを実行する必要があります。
4.  オンプレミスの SQL Server と Azure SQL Database の間でデータをコピーするときは、次のことを確認してください。   
    -   ゲートウェイ コンピューターのファイアウォールで TCP 通信の発信を許可する **TCP** ポート **1433年**します。
    -   構成 [Azure SQL ファイアウォール設定](https://msdn.microsoft.com/library/azure/jj553530.aspx) を追加する、 **ゲートウェイの IP アドレス** するマシン、 **使用できる IP アドレス**です。
5.  データをコピーから内部設置型 SQL Server に任意のコピー先と、ゲートウェイ コンピューターと SQL Server コンピューターが異なるときに、次の操作: [Windows ファイアウォールを構成する](https://msdn.microsoft.com/library/ms175043.aspx) 、SQL Server コンピューターで、ゲートウェイが、SQL Server インスタンスがリッスンするポートを経由してデータベースにアクセスできるようにします。 既定のインスタンスの場合、ポート 1433 です。




