<properties
   pageTitle="Azure 診断でのパフォーマンス カウンターの使用 | Microsoft Azure"
   description="Azure Cloud Services と仮想マシンで、パフォーマンス カウンターを使用して、ボトルネックの特定とパフォーマンスの調整を行います。"
   services="cloud-services"
   documentationCenter=".net"
   authors="rboucher"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/25/2015"
   ms.author="robb" />


# Azure アプリケーションでのパフォーマンス カウンターの作成と使用

この記事では、Azure アプリケーションにパフォーマンス カウンターを配置する利点と方法について説明します。 パフォーマンス カウンターを使用すると、データの収集、ボトルネックの特定、およびシステムとアプリケーションのパフォーマンスの調整を行うことができます。

また、Windows Server、IIS、ASP.NET で使用できるパフォーマンス カウンターでは、Azure の Web ロール、worker ロール、Virtual Machines のパフォーマンス データを収集して状態を把握することもできます。 カスタム パフォーマンス カウンターを作成して使用することもできます。

パフォーマンス カウンター データを調べることができます。
1. リモート デスクトップを使用してアクセス パフォーマンス モニタ ツールを使用してアプリケーション ホストで直接
2. System Center Operations Manager と Azure の管理パックを使用します。
3. Azure Storage に転送された診断データにアクセスするその他の監視ツールを使用します。 参照してください [ストアと Azure Storage で診断データを表示](https://msdn.microsoft.com/library/azure/hh411534.aspx) の詳細。

アプリケーションのパフォーマンスの監視方法について、 [Azure クラシック ポータル](http://manage.azure.com/), を参照してください [クラウド サービスの監視方法](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/)します。

詳細なガイダンスについては、ログとトレース戦略、問題のトラブルシューティングを行うアプリケーションを最適化する Azure 診断とその他の手法を使用して作成する、次を参照してください。 [Azure アプリケーションの開発に関するトラブルシューティングのベスト プラクティス](https://msdn.microsoft.com/library/azure/hh771389.aspx)します。


## パフォーマンス カウンターの監視を有効にする

パフォーマンス カウンターは既定では有効になっていません。 アプリケーションまたはスタートアップ タスクで、各ロール インスタンスを監視する特定のパフォーマンス カウンターを含めるように、診断エージェントの既定の構成を変更する必要があります。

### Microsoft Azure で使用できるパフォーマンス カウンター

Azure は、Windows Server、IIS、および ASP.NET スタックで使用できるパフォーマンス カウンターのサブセットを備えています。 次の表に、Azure アプリケーションに特に関係のあるパフォーマンス カウンターの一部を示します。

| カウンター カテゴリ: オブジェクト (インスタンス)| カウンター名| リファレンス|
|---|---|---|
| .NET CLR Exceptions(_Global_)| # Exceps Thrown / sec| 例外パフォーマンス カウンター|
| .NET CLR Memory(_Global_)| % Time in GC| メモリ パフォーマンス カウンター|
| ASP.NET| Application Restarts| ASP.NET 用のパフォーマンス カウンター|
| ASP.NET| Request Execution Time| ASP.NET 用のパフォーマンス カウンター|
| ASP.NET| Requests Disconnected| ASP.NET 用のパフォーマンス カウンター|
| ASP.NET| Worker Process Restarts| ASP.NET 用のパフォーマンス カウンター|
| ASP.NET Applications(__Total__)| Requests Total| ASP.NET 用のパフォーマンス カウンター|
| ASP.NET Applications(__Total__)| Requests/Sec| ASP.NET 用のパフォーマンス カウンター|
| ASP.NET v4.0.30319| Request Execution Time| ASP.NET 用のパフォーマンス カウンター|
| ASP.NET v4.0.30319| Request Wait Time| ASP.NET 用のパフォーマンス カウンター|
| ASP.NET v4.0.30319| Requests Current| ASP.NET 用のパフォーマンス カウンター|
| ASP.NET v4.0.30319| Requests Queued| ASP.NET 用のパフォーマンス カウンター|
| ASP.NET v4.0.30319| Requests Rejected| ASP.NET 用のパフォーマンス カウンター|
| メモリ| Available MBytes| メモリ パフォーマンス カウンター|
| メモリ| Committed Bytes| メモリ パフォーマンス カウンター|
| Processor(_Total)| プロセッサ時間の割合| ASP.NET 用のパフォーマンス カウンター|
| TCPv4| 接続の失敗| TCP オブジェクト|
| TCPv4| 確立された接続| TCP オブジェクト|
| TCPv4| 接続のリセット| TCP オブジェクト|
| TCPv4| セグメントの送信/秒| TCP オブジェクト|
| ネットワーク Interface(*)| 受信したバイトの数/秒| ネットワーク インターフェイス オブジェクト|
| ネットワーク Interface(*)| 送信バイト数/秒| ネットワーク インターフェイス オブジェクト|
| ネットワーク インターフェイス (Microsoft 仮想マシン バス ネットワーク アダプター _ 2)| 受信したバイトの数/秒| ネットワーク インターフェイス オブジェクト|
| ネットワーク インターフェイス (Microsoft 仮想マシン バス ネットワーク アダプター _ 2)| 送信バイト数/秒| ネットワーク インターフェイス オブジェクト|
| ネットワーク インターフェイス (Microsoft 仮想マシン バス ネットワーク アダプター _ 2)| Bytes total/sec| ネットワーク インターフェイス オブジェクト|

## カスタム パフォーマンス カウンターの作成とアプリケーションへの追加

Azure は、Web ロールと worker ロールのカスタム パフォーマンス カウンターの作成と変更をサポートしています。 カスタム パフォーマンス カウンターを使用すると、アプリケーション固有の動作を追跡および監視できます。 管理者特権のアクセス許可を使用して、スタートアップ タスク、Web ロール、または worker ロール用のカスタム パフォーマンス カウンターのカテゴリと指定子を作成または削除できます。
>[AZURE.NOTE] カスタム パフォーマンス カウンターに変更を加えるコードを実行するには、管理者特権のアクセス許可が必要です。ロールが、タグを含める必要があります、コードが web ロールまたはワーカー ロールである場合は、 <Runtime executionContext="elevated" /> を正常に初期化するロールの ServiceDefinition.csdef ファイルにします。

カスタム パフォーマンス カウンターのデータを診断エージェントを使用して Azure Storage に送信できます。

標準のパフォーマンス カウンターのデータは、Azure プロセスによって生成されます。 カスタム パフォーマンス カウンターのデータは、Web ロールまたは worker ロール アプリケーションで作成する必要があります。 参照してください [パフォーマンス カウンターの種類](https://msdn.microsoft.com/library/z573042h.aspx) カスタム パフォーマンス カウンターに格納できるデータの種類についてです。 参照してください [PerformanceCounters のサンプル](http://code.msdn.microsoft.com/azure/) を作成し、web ロールでカスタム パフォーマンス カウンター データを設定する例についてです。

## パフォーマンス カウンター データの保存および表示

パフォーマンス カウンターのデータは、他の診断情報と共に Azure にキャッシュされます。 このデータは、パフォーマンス モニターなどのツールを表示するためにリモート デスクトップ アクセスを使用してロール インスタンスが実行されているときに、リモート監視を行うために使用できます。 このデータをロール インスタンスの外部に保存するには、診断エージェントによってデータが Azure Storage に転送される必要があります。 キャッシュされるパフォーマンス カウンター データのサイズ制限は、診断エージェントで構成するか、すべての診断データに対する共有制限の一部として構成することができます。 バッファー サイズの設定に関する詳細については、次を参照してください。 [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) と [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx)します。 参照してください [ストアと Azure Storage で診断データを表示](https://msdn.microsoft.com/library/azure/hh411534.aspx) データをストレージ アカウントに転送する診断エージェントの設定の概要についてです。

構成された各パフォーマンス カウンター インスタンスは指定したサンプリング レートで記録され、サンプリングされたデータは、スケジュール設定された転送要求またはオンデマンドの転送要求によってストレージ アカウントに転送されます。 自動転送を毎分 1 回の頻度でスケジュール設定することもできます。 診断エージェントによって転送されたパフォーマンス カウンター データは、ストレージ アカウント内のテーブル WADPerformanceCountersTable に保存されます。 このテーブルには、標準の Azure Storage API メソッドを使用してアクセスし、クエリを実行することができます。 参照してください [Microsoft Azure PerformanceCounters のサンプル](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) クエリを実行して、WADPerformanceCountersTable テーブルからのパフォーマンス カウンター データの表示の例です。
>[AZURE.NOTE] 診断エージェントの転送頻度やキューの待機時間によっては、ストレージ アカウント内の最新のパフォーマンス カウンター データが数分前のデータになる場合があります。

## 診断構成ファイルを使用してパフォーマンス カウンターを有効にする

Azure アプリケーションで、パフォーマンス カウンターを有効にするには、以下の手順を実行します。

## 前提条件

このセクションは、アプリケーションに診断モニターがインポートされ、Visual Studio ソリューションに診断構成ファイル (SDK 2.4 以前の場合は diagnostics.wadcfg、SDK 2.5 以降の場合は diagnostics.wadcfgx) が追加されていることを前提としています。 手順 1 と 2 を参照してください [Azure クラウド サービスおよび仮想マシンで診断を有効にすると](./cloud-services-dotnet-diagnostics.md)) の詳細。

## 手順 1. パフォーマンス カウンターからデータを収集して保存する

Visual Studio ソリューションに診断ファイルを追加すると、Azure アプリケーションでパフォーマンス カウンター データの収集と保存を構成できます。 そのためには、診断ファイルにパフォーマンス カウンターを追加します。 パフォーマンス カウンターを含む診断データは、まずインスタンスで収集されます。 データは、このため、アプリケーションでストレージ アカウントを指定する必要がありますも、Azure テーブル サービスの WADPerformanceCountersTable テーブルに永続化します。 コンピューティング エミュレーターでアプリケーションをローカルにテストする場合、ストレージ エミュレーターで診断データをローカルに保存することもできます。 初めてアクセスする必要があります診断データを格納する前に、 [Azure クラシック ポータル](http://manage.windowsazure.com/) し、ストレージ アカウントを作成します。 ベスト プラクティスとして、Azure アプリケーションと同じ地理的な場所にあるストレージ アカウントを特定することにより、外部帯域幅のコストが生じないようにしたり、遅延時間を短縮することができます。

### 診断ファイルにパフォーマンス カウンターを追加する

使用できるパフォーマンス カウンターは多数あります。 次の例に、Web ロールおよび worker ロールの監視に推奨されるパフォーマンス カウンターをいくつか示します。

診断ファイル (diagnostics.wadcfg SDK 2.4 以下で、または SDK 2.5 以降は diagnostics.wadcfgx) を開き、次のコードを DiagnosticMonitorConfiguration 要素に追加します。

```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
       <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    

       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    

       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
       <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
    </PerformanceCounters>
```

BufferQuotaInMB 属性をデータ コレクションの種類 (Azure ログ、IIS ログなど) で利用可能なファイル システム ストレージの最大サイズを指定します。 既定値は 0 です。 クォータに達した場合、新しいデータが追加されるときに最も古いデータが削除されます。 すべての bufferQuotaInMB プロパティの合計は、OverallQuotaInMB 属性の値より大きくなければなりません。 診断データの収集に必要な量のストレージを決定する詳細については、の WAD のセットアップ」セクションを参照してください。 [Azure アプリケーションの開発に関するトラブルシューティングのベスト プラクティス](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx)します。

データ転送のスケジュールの間隔を指定するには、scheduledTransferPeriod 属性は最も近い分単位に丸められます。 次の例では、PT30M (30 分) に設定されます。 転送期間を小さい値 (1 分など) に設定すると、運用環境のアプリケーションのパフォーマンスにマイナスの影響が及びますが、テスト時に診断が機能するかをすぐに確認する場合に役立つことがあります。 スケジュールされた転送期間は、インスタンスで診断データが上書きされないような小さい値にし、かつアプリケーションのパフォーマンスに影響を与えないような大きい値にしてください。

CounterSpecifier 属性では、収集するパフォーマンス カウンターを指定します。SampleRate 属性は、あるパフォーマンス カウンターをサンプリングする、ここでは 30 秒速度を指定します。

収集するパフォーマンス カウンターを追加したら、変更を診断ファイルに保存します。 次に、診断データを保存するストレージ アカウントを指定する必要があります。

### ストレージ アカウントを指定する

診断情報を Azure ストレージ アカウントに保存するには、サービス構成 (ServiceConfiguration.cscfg) ファイルで接続文字列を指定する必要があります。

Azure SDK 2.5 の場合、ストレージ アカウントは diagnostics.wadcfgx ファイルで指定できます。
>[AZURE.NOTE] 以下の手順は、Azure SDK 2.4 以前にのみ適用されます。 Azure SDK 2.5 の場合、ストレージ アカウントは diagnostics.wadcfgx ファイルで指定できます。

接続文字列を設定するには、次の手順に従います。

1. 任意のテキスト エディターを使用して ServiceConfiguration.Cloud.cscfg ファイルを開き、ストレージの接続文字列を設定します。  *AccountName* と *AccountKey* のストレージ アカウント ダッシュ ボードの [キーの管理下にある Azure クラシック ポータルで値があります。

    ```
    <ConfigurationSettings>
       <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. ServiceConfiguration.Cloud.cscfg ファイルを保存します。

3. ServiceConfiguration.Local.cscfg ファイルを開き、UseDevelopmentStorage が設定されていることを確認 true に設定します。

    ```
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
これで、接続文字列が設定され、アプリケーションのデプロイ時にアプリケーションで診断データがストレージ アカウントに保存されるようになります。
4. プロジェクトを保存してビルドし、アプリケーションをデプロイします。

## 手順 2. (省略可能) カスタム パフォーマンス カウンターを作成する

事前定義されたパフォーマンス カウンターに加えて、独自のカスタム パフォーマンス カウンターを追加して Web ロールまたは worker ロールを監視できます。 カスタム パフォーマンス カウンターを使用すると、アプリケーション固有の動作を追跡して監視することができます。また、カスタム パフォーマンス カウンターは、昇格されたアクセス許可を持つスタートアップ タスク、Web ロール、worker ロールで作成または削除できます。

Azure 診断エージェントは起動の 1 分後に .wadcfg ファイルのパフォーマンス カウンター構成を更新します。 OnStart メソッドでカスタム パフォーマンス カウンターを作成していて、スタートアップ タスクの実行に 1 分以上かかる場合は、Azure 診断エージェントがカスタム パフォーマンス カウンターの読み込みを試行する時点でカスタム パフォーマンス カウンターがまだ作成されていません。 このようなシナリオでは、Azure 診断でカスタム パフォーマンス カウンターを除くすべての診断データが正しくキャプチャされていると表示されます。 この問題を解決するには、スタートアップ タスクでパフォーマンス カウンターを作成するか、パフォーマンス カウンターの作成後にスタートアップ タスクの作業の一部を OnStart メソッドに移します。

次の手順を実行して、"\MyCustomCounterCategory\MyButton1Counter" という簡単なカスタム パフォーマンス カウンターを作成します。

1. アプリケーションのサービス定義ファイル (CSDEF) を開きます。
2. Runtime 要素を WebRole または WorkerRole 要素により、昇格した特権で実行されるを追加します。

    ```
    <RuntimeexecutionContext="elevated"/>
    ```
3. ファイルを保存します。
4. 診断ファイル (diagnostics.wadcfg SDK 2.4 以下で、または SDK 2.5 以降は diagnostics.wadcfgx) を開き、DiagnosticMonitorConfiguration に次の追加

    ```
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
     <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. ファイルを保存します。
6. ベースを呼び出す前に、ロールの OnStart メソッドでのカスタム パフォーマンス カウンター カテゴリを作成します。OnStart します。 次の C# の例では、まだ存在しない場合にカスタム カテゴリを作成します。

    ```
    public override bool OnStart()
    {
    if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
    {
       CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

       // add a counter tracking user button1 clicks
       CounterCreationData operationTotal1 = new CounterCreationData();
       operationTotal1.CounterName = "MyButton1Counter";
       operationTotal1.CounterHelp = "My Custom Counter for Button1";
       operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
       counterCollection.Add(operationTotal1);

       PerformanceCounterCategory.Create(
         "MyCustomCounterCategory",
         "My Custom Counter Category",
         PerformanceCounterCategoryType.SingleInstance, counterCollection);

       Trace.WriteLine("Custom counter category created.");
    }
    else{
       Trace.WriteLine("Custom counter category already exists.");
    }

    return base.OnStart();
    }
    ```
7. アプリケーション内でカウンターを更新します。 次の例では、Button1_Click イベントでカスタム パフォーマンス カウンターを更新します。

    ```
    protected void Button1_Click(object sender, EventArgs e)
        {
         PerformanceCounter button1Counter = new PerformanceCounter(
           "MyCustomCounterCategory",
           "MyButton1Counter",
           string.Empty,
           false);
         button1Counter.Increment();
        this.Button1.Text = "Button 1 count: " +
           button1Counter.RawValue.ToString();
        }
    ```
8. ファイルを保存します。

これらの手順を完了すると、カスタム パフォーマンス カウンター データが Azure 診断モニターによって収集されます。

## 手順 3. パフォーマンス カウンターのデータのクエリを実行する

アプリケーションがデプロイされたら、診断モニターを実行すると、パフォーマンス カウンターの収集が開始され、そのデータが Azure ストレージに保存されます。 Visual Studio で、サーバー エクスプ ローラーなどのツールを使用する  [Azure ストレージ エクスプ ローラー](http://azurestorageexplorer.codeplex.com/), 、または [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) cerebrata を表示するパフォーマンス カウンターの WADPerformanceCountersTable テーブル内のデータです。 You can also programatically query the Table service using [C#](../storage/storage-dotnet-how-to-use-tables.d),  [Java](../storage/storage-java-how-to-use-table-storage.md),  [Node.js](../storage/storage-nodejs-how-to-use-table-storage.md), [Python](../storage/storage-python-how-to-use-table-storage.md), [Ruby](../storage/storage-ruby-how-to-use-table-storage.md), or [PHP](../storage/storage-php-how-to-use-table-storage.md).

次の c# の例では、WADPerformanceCountersTable テーブルに対する単純なクエリを示していて、CSV ファイルに診断データを保存します。 パフォーマンス カウンターが CSV ファイルに保存されたら、Microsoft Excel や他のツールのグラフ作成機能を使用してデータを視覚化できます。 必ず、Microsoft.WindowsAzure.Storage.dll への参照を追加してください。これは、Azure SDK for .NET (2012 年 10 月) 以降に含まれています。 このアセンブリは、%Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ ディレクトリにインストールされます。

```
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ...

    // Get the connection string. When using Microsoft Azure Cloud Services, it is recommended
    // you store your connection string using the Microsoft Azure service configuration
    // system (*.csdef and *.cscfg files). You can you use the CloudConfigurationManager type
    // to retrieve your storage connection string.  If you're not using Cloud Services, it's
    // recommended that you store the connection string in your web.config or app.config file.
    // Use the ConfigurationManager type to retrieve your storage connection string.

    string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
    //string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

    // Get a reference to the storage account using the connection string.  You can also use the development
    // storage account (Storage Emulator) for local debugging.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    //CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

    // Create the CloudTable object that represents the "WADPerformanceCountersTable" table.
    CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

    // Create the table query, filter on a specific CounterName, DeploymentId and RoleInstance.
    TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
       .Where(
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
          TableOperators.And,
          TableQuery.CombineFilters(
          TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
          TableOperators.And,
          TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
          )
       )
    );

    // Execute the table query.
    IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

    // Process the query results and build a CSV file.
    StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

    foreach (PerformanceCountersEntity entity in result)
    {
       sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
          + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
    }

    StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
    sw.Write(sb.ToString());
    sw.Close();
```

エンティティから派生したカスタム クラスを使用して c# オブジェクトにマップ **TableEntity**します。 次のコード内のパフォーマンス カウンターを表すエンティティ クラスを定義する、 **WADPerformanceCountersTable** テーブルです。


    public class PerformanceCountersEntity : TableEntity
    {
       public long EventTickCount { get; set; }
       public string DeploymentId { get; set; }
       public string Role { get; set; }
       public string RoleInstance { get; set; }
       public string CounterName { get; set; }
       public double CounterValue { get; set; }
    }

## 次のステップ

これで、パフォーマンス カウンターの収集の基本を学習できました。より複雑なトラブルシューティング シナリオを実装する方法については、次のリンク先を参照してください。

[Azure アプリケーションの開発に関するトラブルシューティングのベスト プラクティス](https://msdn.microsoft.com/library/azure/hh771389.aspx)

[クラウド サービスを監視する方法](./how-to-monitor-a-cloud-service.md)





