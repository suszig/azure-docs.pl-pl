<properties
    pageTitle="エラスティック データベース Split-Merge ツールに関するチュートリアル | Microsoft Azure"
    description="エラスティック データベース ツールによる分割とマージ"
    services="sql-database"  
    documentationCenter=""
    authors="sidneyh"
    manager="jeffreyg"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/01/2015"
    ms.author="sidneyh" />

# エラスティック データベース Split-Merge ツールに関するチュートリアル

## 分割-結合パッケージのダウンロード
1. 最新の NuGet バージョンをダウンロード [NuGet](http://docs.nuget.org/docs/start-here/installing-nuget)します。
2. コマンド プロンプトを開き、nuget.exe をダウンロードしたディレクトリに移動します。
3. 次のコマンドで最新の Split-Merge パッケージを現在のディレクトリにダウンロードします。
`nuget install Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge`  

前の手順を実行すると Split-Merge ファイルが現在のディレクトリにダウンロードされます。 という名前のディレクトリにファイルを配置している **Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge.x.x.xxx.x** 、 *x.x.xxx.x* バージョン番号が反映されます。 分割-結合サービス ファイル、 **content \splitmerge\service** サブディレクトリ、および、Split-merge PowerShell スクリプト (および必要な .dll) で、 **\splitmerge\powershell** サブディレクトリ。

## 前提条件

1. Split-Merge ステータス データベースとして使用する Azure SQL DB を作成します。 移動して、 [Azure ポータル](https://ms.portal.azure.com)します。 新しい **SQL Database**します。 データベース名を入力し、新しいユーザーとパスワードを作成します。 今後の使用のために、パスワードと名前を必ず記録しておいてください。

2. Azure SQL DB サーバーで Azure サービスからの接続が許可されていることを確認します。 ポータルで、 **ファイアウォールの設定**, 、いることを確認、 **Azure サービスへのアクセスを許可する** に設定する **に**します。 [保存] アイコンをクリックします。

    ![使用できるサービス][1]

3. 診断の出力に使用する Azure Storage アカウントを作成します。 Azure ポータルにアクセスします。 左側のバーでをクリックして **新規**, をクリックして **データ + ストレージ**, 、し **ストレージ**します。

4. Split-Merge サービスが含まれる Azure クラウド サービスを作成します。  Azure ポータルにアクセスします。 左側のバーでをクリックして **新規**, 、し **コンピューティング**, 、**クラウド サービス**, 、および **作成**します。 


## 分割-結合サービスの構成

### Split-Merge サービスの構成

1. Split/merge ビットをダウンロードしたフォルダー内のコピーを作成、 **ServiceConfiguration.Template.cscfg** 付属ファイル **SplitMergeService.cspkg** それを呼び出す **ServiceConfiguration.cscfg**します。

2. 使い慣れたテキスト エディターで、ServiceConfiguration.cscfg を開きます。 証明書の拇印の形式などの入力値が検証されるため、Visual Studio を使用することをお勧めします。

3. 新しいデータベースを作成するか、Split/Merge 操作用のステータス データベースとして使用する既存のデータベースを選択し、そのデータベースの接続文字列を取得します。 

    **重要な** この時点で、状態のデータベースがラテン語の照合順序 (SQL\_Latin1\_General\_CP1\_CI\_AS) を使用する必要があります。 詳細については、次を参照してください。 [Windows 照合順序名 (TRANSACT-SQL)](https://msdn.microsoft.com/library/ms188046.aspx)します。

    Azure SQL DB では、通常、接続文字列の形式は次のようになります。

        "Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30" .
4.    両方で cscfg ファイルの接続文字列を入力、 **SplitMergeWeb** と **SplitMergeWorker** ElasticScaleMetadata 設定のセクションではロールです。

5.     **SplitMergeWorker** ロール、Azure ストレージへの有効な接続文字列を入力して、 **WorkerRoleSynchronizationStorageAccountConnectionString** 設定します。
        
### セキュリティの構成
サービスのセキュリティを構成する詳細な手順についてを参照してください、 [Split-merge セキュリティ構成](sql-database-elastic-scale-split-merge-security-configuration.md)します。

このチュートリアルの簡単なテスト デプロイのため、最小限の構成の手順セットを行ってサービスを起動および実行します。 以下の手順では、サービスを実行する 1 つのコンピューター/アカウントのみがサービスと通信できます。

### 自己署名証明書の作成

新しいディレクトリを作成し、このディレクトリから実行を使用して次のコマンド、 [Visual Studio の開発者コマンド プロンプト](http://msdn.microsoft.com/library/ms229859.aspx) ウィンドウ。

    makecert ^
    -n "CN=*.cloudapp.net" ^
    -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2" ^
    -a sha1 -len 2048 ^
    -sr currentuser -ss root ^
    -sv MyCert.pvk MyCert.cer

秘密キーを保護するパスワードの入力を求められます。 強力なパスワードを入力し、確定します。 その後、パスワードをもう一度使用するよう求められます。 をクリックして **はい** 末尾には、信頼されたルート証明機関ストアにインポートします。

### PFX ファイルの作成

makecert を実行した同じウィンドウから次のコマンドを実行します。証明書の作成に使用したものと同じパスワードを使用します。

    pvk2pfx -pvk MyCert.pvk -spc MyCert.cer -pfx MyCert.pfx -pi <password>

### 個人用ストアへのクライアント証明書のインポート
1. Windows エクスプ ローラーでダブルクリック **MyCert.pfx**します。
2.  **証明書インポート ウィザード** 選択 **現在のユーザー** ] をクリック **次**します。
3. ファイル パスを確認し、クリックして **次**します。
4. パスワードを入力でそのまま使用 **すべての拡張プロパティを含める** チェックし、をクリックして **次**します。
5. ままにして **自動的に証明書ストア [...] を選択** チェックし、クリックして **次**します。
6. をクリックして **完了** と **OK**します。

### クラウド サービスへの PFX ファイルのアップロード

移動して、 [Azure ポータル](https://portal.azure.com)します。

1. 選択 **クラウド サービス**します。
2. 分割/結合サービス用に上で作成したクラウド サービスを選択します。
3. クリックして **証明書** 上部のメニュー。
4. クリックして **アップロード** 下部のバーでします。
5. PFX ファイルを選択し、前述と同じパスワードを入力します。
6. 完了したら、一覧内の新しいエントリから証明書の拇印をコピーします。

### サービス構成ファイルの更新

上記でコピーした証明書の拇印を、次の設定で、サムプリントと属性値に貼り付けます。
worker ロール:

    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

Web ロール:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />
    <Setting name="AllowedClientCertificateThumbprints" value="" />
    <Setting name="DataEncryptionPrimaryCertificateThumbprint" value="" />
    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />
    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />


運用デプロイメントでは、CA、暗号化、サーバー証明書、クライアント証明書のそれぞれに異なる証明書を使用する必要があることに注意してください。 この詳細な手順については、次を参照してください。 [セキュリティの構成](sql-database-elastic-scale-split-merge-security-configuration.md)します。

### Split-Merge サービスのデプロイ

1. 移動して、 [Azure ポータル](https://manage.windowsazure.com)します。
2. クリックして、 **Cloud Services** 、左側のタブし、先ほど作成したクラウド サービスを選択します。
3. クリックして **ダッシュ ボード**します。
4. ステージング環境を選択し、クリックして **新しいステージング環境のデプロイをアップロード**します。

    ![ステージング][3]

5. ダイアログ ボックスにデプロイ ラベルを入力します。 'Package' と 'Configuration' の両方でローカルから] をクリックして、 **SplitMergeService.cspkg** ファイルと、先ほど構成した .cscfg ファイルです。
6. というラベルが付いたチェック ボックスを必ず **1 つまたは複数のロールに単一のインスタンスが含まれる場合でもデプロイ** がオンになっています。
7. 右下のチェック マークをクリックしてデプロイを開始します。 完了には数分かかります。

![アップロード][4]


## デプロイのトラブルシューティング

Web ロールのオンライン化に失敗した場合は、セキュリティの構成に問題があると考えられます。 SSL が前の説明どおりに構成されていることをご確認ください。

worker ロールのオンライン化に失敗した場合に最も考えられるのは、先に作成した状態データベースへの接続に問題があることです。

* 使用する .cscfg の接続文字列が正確であることをご確認ください。
* サーバーとデータベースが存在し、ユーザー ID とパスワードが正しいことを確認します。
* Azure SQL DB の場合、接続文字列の形式は次のようにする必要があります。

        "Server=myservername.database.windows.net; Database=mydatabasename;User ID=myuserID; Password=mypassword; Encrypt=True; Connection Timeout=30" .

* サーバー名がで始まっていないことを確認 **https://**します。
* Azure SQL DB サーバーで Azure サービスからの接続が許可されていることを確認します。 これを行うには、https://manage.windowsazure.com を開いて左側の [SQL Database] をクリックし、上部の [サーバー] をクリックしてから使用するサーバーを選択します。 をクリックして **構成** 上部にあることを確認し、 **Azure Services** 設定が"Yes"に設定します。 (この記事の冒頭にある前提条件をご覧ください)。

## Split-Merge サービスのデプロイのテスト

### Web ブラウザーを使用した接続

Split-Merge サービスの Web エンドポイントを決定します。 これで見つかります Azure 旧ポータルに移動して、 **ダッシュ ボード** 、クラウド サービスと検索の **サイトの URL** 右側にあります。 置換 **http://** と **https://** 既定のセキュリティ設定は、HTTP エンドポイントを無効にするためです。 この URL のページをブラウザーに読み込みます。

### PowerShell スクリプトを使用したテスト

付属の PowerShell スクリプト サンプルを実行して、デプロイメントと環境をテストできます。

付属のスクリプト ファイルは、次のとおりです。

1. **SetupSampleSplitMergeEnvironment.ps1** -Split/merge のテスト データ層の設定 (詳細については、次の表を参照してください)
2. **ExecuteSampleSplitMerge.ps1** -テストのテスト操作を実行します (詳細については、次の表を参照してください) データを階層化
3. **GetMappings.ps1** トップレベル – シャード マッピングの現在の状態を出力するサンプル スクリプトです。
4. **ShardManagement.psm1**  – ShardManagement API をラップするヘルパー スクリプト
5. **SqlDatabaseHelpers.psm1** – を作成して、SQL データベースを管理するためのヘルパー スクリプト

<table style="width:100%">
  <tr>
    <th>PowerShell ファイル</th>
    <th>手順</th>
  </tr>
  <tr>
    <th rowspan="5">SetupSampleSplitMergeEnvironment.ps1</th>
    <td>1.    シャードのマップ マネージャー データベースを作成します。</td>
  </tr>
  <tr>
    <td>2.    2 つのシャード データベースを作成します。
  </tr>
  <tr>
    <td>3.    これらのデータベース用のシャード マップを作成します (これらのデータベースに関する既存のシャード マップを削除します)。 </td>
  </tr>
  <tr>
    <td>4.    両シャード上に小さいサンプル テーブルを作成し、いずれかのシャードで、このテーブルにデータを読み込みます。</td>
  </tr>
  <tr>
    <td>5.    シャード化したテーブルの SchemaInfo を宣言します。</td>
  </tr>

</table>

<table style="width:100%">
  <tr>
    <th>PowerShell ファイル</th>
    <th>手順</th>
  </tr>
<tr>
    <th rowspan="4">ExecuteSampleSplitMerge.ps1 </th>
    <td>1.    データを半分に分割して最初のシャードから 2 番目のシャードに渡す分割要求を Split-Merge サービスの Web フロントエンドに送信します。</td>
  </tr>
  <tr>
    <td>2.    分割要求の状態を Web フロントエンドに対してポーリングし、要求が完了するまで待機します。</td>
  </tr>
  <tr>
    <td>3.    2 番目のシャードから最初のシャードにデータを戻すための移動を求めるマージ要求を、Split-Merge サービスの Web フロントエンドに送信します。</td>
  </tr>
  <tr>
    <td>4.    マージ要求の状態を Web フロントエンドに対してポーリングし、要求が完了するまで待機します。</td>
  </tr>
</table>

##PowerShell によるデプロイの確認

1.    新しい PowerShell ウィンドウを開き、分割-結合パッケージをダウンロードしたディレクトリに移動し、"powershell" ディレクトリに移動します。
2.    Azure SQL データベース サーバーを作成 (または既存のサーバーを選択) します。ここでシャード マップ マネージャーとシャードが作成されます。

    Note: The SetupSampleSplitMergeEnvironment.ps1 script creates all these databases on the same server by default to keep the script simple. This is not a restriction of the Split-Merge Service itself.

    A SQL authentication login with read/write access to the DBs will be needed for the Split-Merge service to move data and update the shard map. Since the Split-Merge Service runs in the cloud, it does not currently support Integrated Authentication.

    Make sure the Azure SQL server is configured to allow access from the IP address of the machine running these scripts. You can find this setting under the Azure SQL server / configuration / allowed IP addresses.

3.    SetupSampleSplitMergeEnvironment.ps1 スクリプトを実行してサンプル環境を作成します。

    Running this script will wipe out any existing shard map management data structures on the shard map manager database and the shards. It may be useful to rerun the script if you wish to re-initialize the shard map or shards.

    Sample command line:

        .\SetupSampleSplitMergeEnvironment.ps1 `
            -UserName 'mysqluser' `
            -Password 'MySqlPassw0rd' `
            -ShardMapManagerServerName 'abcdefghij.database.windows.net'

4.    サンプル環境に現在あるマッピングを表示するには、Getmappings.ps1 スクリプトを実行します。

        .\GetMappings.ps1 `
            -UserName 'mysqluser' `
            のパスワード 'MySqlPassw0rd' '
            ShardMapManagerServerName 'abcdefghij.database.windows.net'

5.    ExecuteSampleSplitMerge.ps1 スクリプトを実行して、分割操作を実行 (データの半分を最初のシャードから 2 番目のシャードに移動) し、次にマージ操作を実行 (データを最初のシャードに戻すために移動) します。 SSL が構成済みで、http エンドポイントが無効のままになっている場合は、代わりに https:// のエンドポイントを使用します。

    Sample command line:

        .\ExecuteSampleSplitMerge.ps1 `
            -UserName 'mysqluser' `
            -Password 'MySqlPassw0rd' `
            -ShardMapManagerServerName 'abcdefghij.database.windows.net' `
            -SplitMergeServiceEndpoint 'https://mysplitmergeservice.cloudapp.net' `
            -CertificateThumbprint '0123456789abcdef0123456789abcdef01234567'

    If you receive the below error, it is most likely a problem with your Web endpoint’s certificate. Try connecting to the Web endpoint with your favorite Web browser and check if there is a certificate error.

        Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLSsecure channel.

    If it succeeded, the output should look like the below:

        > .\ExecuteSampleSplitMerge.ps1 -UserName 'mysqluser' -Password 'MySqlPassw0rd' -ShardMapManagerServerName 'abcdefghij.database.windows.net' -SplitMergeServiceEndpoint 'http://mysplitmergeservice.cloudapp.net' –CertificateThumbprint 0123456789abcdef0123456789abcdef01234567
        Sending split request
        Began split operation with id dc68dfa0-e22b-4823-886a-9bdc903c80f3
        Polling split-merge request status. Press Ctrl-C to end
        Progress: 0% | Status: Queued | Details: [Informational] Queued request
        Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
        Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
        Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
        Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Waiting for reference tables copy     completion.
        Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
        Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
        Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
        ...
        ...
        Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
        Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing the request.
        Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.
        Sending merge request
        Began merge operation with id 6ffc308f-d006-466b-b24e-857242ec5f66
        Polling request status. Press Ctrl-C to end
        Progress: 0% | Status: Queued | Details: [Informational] Queued request
        Progress: 5% | Status: Starting | Details: [Informational] Starting split-merge state machine for request.
        Progress: 5% | Status: Starting | Details: [Informational] Performing data consistency checks on target     shards.
        Progress: 20% | Status: CopyingReferenceTables | Details: [Informational] Moving reference tables from     source to target shard.
        Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Moving key range [100:110) of     Sharded tables
        Progress: 44% | Status: CopyingShardedTables | Details: [Informational] Successfully copied key range     [100:110) for table [dbo].[MyShardedTable]
        ...
        ...
        Progress: 90% | Status: Completing | Details: [Informational] Successfully deleted shardlets in table     [dbo].[MyShardedTable].
        Progress: 90% | Status: Completing | Details: [Informational] Deleting any temp tables that were created     while processing the request.
        Progress: 100% | Status: Succeeded | Details: [Informational] Successfully processed request.

6.    他のデータ型も試してみてください。 これらのすべてのスクリプトでは、オプションの -ShardKeyType パラメーターを使用してキーの種類を指定することができます。 既定値は Int32 ですが、Int64、Guid、またはバイナリも指定できます。

## 独自の要求の作成

サービスは、Web UI を使用するか、インポートするか、Web ロールで要求を送信する SplitMerge.psm1 PowerShell モジュールを使用する方法のいずれかで利用できます。

Split-Merge サービスは、シャード化したテーブルと参照テーブルの両方にデータを移動できます。 シャード テーブルには、シャード キー列があり、シャードごとに異なる行データを保持します。 参照テーブルはシャードされないため、すべてのシャードに同じ行データが含まれています。 データを頻繁には変更しない場合に参照テーブルを使用すると便利です。また、これをクエリに使用してシャード化されたテーブルとの JOIN を行います。

分割やマージの操作を実行するには、移動の対象となるシャード化したテーブルと参照テーブルを宣言する必要があります。 これを行うと、 **SchemaInfo** API です。 この API は、 **Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.Schema** 名前空間。

1.    シャード テーブルごとに、作成、 **ShardedTableInfo** テーブルの親のスキーマ名を表すオブジェクト (省略可能、既定値は"dbo")、テーブル名とシャーディング キーが含まれているテーブル内の列名。
2.    参照テーブルごとに、作成、 **ReferenceTableInfo** テーブルの親のスキーマ名を表すオブジェクト (省略可能、既定値は"dbo") とテーブル名。
3.    前の TableInfo オブジェクトを新しい追加 **SchemaInfo** オブジェクトです。
4.    参照を取得、 **ShardMapManager** オブジェクト、および呼び出し **GetSchemaInfoCollection**します。
5.    追加、 **SchemaInfo** に、 **SchemaInfoCollection**, 、シャード マップ名を指定します。

この例は、SetupSampleSplitMergeEnvironment.ps1 スクリプトで確認できます。

Split-Merge サービスはターゲット データベース (またはデータベースにある任意のテーブル用のスキーマ) を作成しないことに注意してください。 サービスに要求を送信する前に作成しておく必要があります。


## トラブルシューティング
サンプル Powershell スクリプトの実行中に、次のメッセージが表示されることがあります。

    Invoke-WebRequest : The underlying connection was closed: Could not establish trust relationship for the SSL/TLS secure channel.

このエラーは、SSL 証明書が正しく構成されていないことを意味しています。 「Web ブラウザーを使って接続する」のセクションの手順に従ってください。

要求を送信できない場合、次の内容が表示される可能性があります。

 [Exception] System.Data.SqlClient.SqlException (0x80131904): Could not find stored procedure 'dbo.InsertRequest'. 

この場合は、構成ファイル、特にの設定を確認 **WorkerRoleSynchronizationStorageAccountConnectionString**します。 このエラーは、通常、Worker ロールがメタデータ データベースを初回使用時に正常に初期化できなかったことを示しています。 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/allowed-services.png
[2]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/manage.png
[3]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/staging.png
[4]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/upload.png
[5]: ./media/sql-database-elastic-scale-configure-deploy-split-and-merge/storage.png
 


