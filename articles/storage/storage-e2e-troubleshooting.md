<properties 
    pageTitle="Azure Storage のメトリックおよびログ、AzCopy、Message Analyzer を使用したエンド ツー エンド トラブルシューティング | Microsoft Azure" 
    description="Azure Storage Analytics、AzCopy、Microsoft Message Analyzer を使用したエンド ツー エンド トラブルシューティングについて説明するチュートリアル" 
    services="storage" 
    documentationCenter="dotnet" 
    authors="tamram" 
    manager="adinah"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="12/01/2015" 
    ms.author="tamram"/>

# Azure Storage のメトリックおよびログ、AzCopy、Message Analyzer を使用したエンド ツー エンド トラブルシューティング 

[AZURE.INCLUDE [storage-selector-portal-e2e-troubleshooting](../../includes/storage-selector-portal-e2e-troubleshooting.md)]

## 概要

診断とトラブルシューティングは、Microsoft Azure Storage を使用してクライアント アプリケーションを構築し、サポートするうえでの重要なスキルです。 Azure アプリケーションの分散型の性質により、エラーやパフォーマンスの問題に対する診断とトラブルシューティングは、従来の環境で実施するよりも複雑になる場合があります。

このチュートリアルでは、クライアント アプリケーションの最適化を目的に、パフォーマンスに影響する可能性のあるクライアントのエラーを特定すると共に、Microsoft と Azure Storage が提供するツールを使用して、これらのエラーについてエンド ツー エンドのトラブルシューティングを実施する方法を説明します。 

このチュートリアルでは、エンド ツー エンド トラブルシューティングのシナリオを実践的に学ぶことができます。 詳細な概念ガイド Azure storage アプリケーションのトラブルシューティングを参照してください。 [モニター、診断、および記憶域のトラブルシューティングを行う](storage-monitoring-diagnosing-troubleshooting.md)します。 

## Azure Storage アプリケーションのトラブルシューティングに使用するツール

Microsoft Azure Storage を使用してクライアント アプリケーションのトラブルシューティングを実施する場合、問題がいつ発生したのか、原因は何なのかを見極めるために複数のツールを組み合わせて使用することができます。 これには以下のツールが含まれます。

- **Azure Storage Analytics**します。 [Azure Storage Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx) メトリックおよび Azure Storage のログ記録を提供します。
    - **ストレージ メトリック** トランザクション メトリックとストレージ アカウントの容量のメトリックを追跡します。 メトリックを使用すると、アプリケーションがどのように機能しているかを、さまざまな基準に従って判断できます。 参照してください [Storage Analytics Metrics のテーブル スキーマ](http://msdn.microsoft.com/library/azure/hh343264.aspx) Storage Analytics が追跡するメトリックの種類の詳細についてです。 

    - **ストレージ ログ** サーバー側のログを Azure ストレージ サービスに対する各要求をログに記録します。 このログは、実行された操作、操作のステータス、遅延情報などを含む各要求の詳細データを追跡します。 参照してください [Storage Analytics のログ形式](http://msdn.microsoft.com/library/azure/hh343259.aspx) 詳細については、Storage Analytics によってログに書き込まれる要求と応答データ。

- **Azure ポータル**します。 メトリックとログを構成するには、ストレージ アカウント内の [Azure ポータル](portal.azure.com)します。 アプリケーションの経時的な動作を表すチャートやグラフを表示できるほか、指定したメトリックに関してアプリケーションが予期しない動作をした場合に通知するよう、アラートを構成することもできます。 
    
    参照してください [ストレージ アカウントの監視方法](storage-monitor-storage-account.md) Azure ポータルでの監視を構成する方法についてです。

- **AzCopy**します。 Azure Storage のサーバー ログは BLOB として格納されているので、AzCopy を使用してログ BLOB をローカル ディレクトリにコピーし、Microsoft Message Analyzer による分析に使用することができます。 参照してください [Microsoft Azure Storage で AzCopy を使用する方法](storage-use-azcopy.md) AzCopy の詳細についてです。

- **Microsoft Message Analyzer**します。 Message Analyzer はログ ファイルを使用してログ データを画像形式で表示するツールです。これにより、ログ データのフィルター処理や検索のほか、エラーやパフォーマンス問題の分析に使用できる有用なデータ セットへのグループ化が容易になります。 参照してください [Microsoft Message Analyzer の操作ガイド](http://technet.microsoft.com/library/jj649776.aspx) Message Analyzer の詳細についてです。

## サンプル シナリオについて

このチュートリアルでは、Azure Storage を呼び出すアプリケーションの成功率に関して、Azure Storage メトリックが低い割合を示すシナリオについて調べます。 低い成功率メトリック (として表示 **PercentSuccess** で、 [Azure ポータル](portal.azure.com) およびメトリック テーブル内) が成功しますが、299 を超える数字である HTTP ステータス コードを返す操作を追跡します。 トランザクション状態でサーバー側ストレージ ログ ファイルで、これらの操作が記録されます **ClientOtherErrors**します。 低い成功率メトリックの詳細については、次を参照してください。 [メトリックの示す percentsuccess が低い、または分析ログ エントリがトランザクション ステータスが ClientOtherErrors の操作をある](storage-monitoring-diagnosing-troubleshooting.md#metrics-show-low-percent-success)です。

Azure Storage の操作では、その通常の機能の一部として 299 を超える数字の HTTP 状態コードを返す場合があります。 しかしこれらのエラーは、クライアント アプリケーションのパフォーマンスを最適化できる可能性があることを示している場合もあります。 

このシナリオでは、100% 未満の値についてはすべて低い成功率であると判断します。 ただし、必要に応じて異なるメトリック レベルを選択することができます。 アプリケーションのテスト時には、主なパフォーマンス メトリックについて基準となる許容値を設定することをお勧めします。 たとえば、アプリケーションの成功率は 90% または 85% を維持する必要があることを、テストに基づいて決定することができます。 アプリケーションがその数値から外れていることをメトリック データが示している場合、その増加を引き起こしているのが何なのかを調査することができます。 

サンプル シナリオでは、成功率メトリックの割合を 100% 未満に設定した後、ログを調べてそのメトリックと相関関係があるエラーを見つけ、それを使用して成功率が低くなる原因を解明します。 ここでは、特に 400 番台のエラーに着目します。 その後、404 (Not Found) エラーに的を絞って調査します。

### 400 番台のエラーの原因

以下の例では、Azure Blob Storage への要求に関するいくつかの 400 番台のエラーと、その考えられる原因を示します。 これらのエラーは、300 および 500 番台のエラーと同様に、いずれも低い成功率の一因となる可能性があります。 

以下の一覧は不完全であるという点に注意してください。 参照してください [ステータス コードとエラー コード](http://msdn.microsoft.com/library/azure/dd179382.aspx) 、msdn の通常の Azure Storage エラーおよび各ストレージ サービスに固有のエラー詳細。

**ステータス コード 404 (Not Found) の例**

BLOB またはコンテナーが見つからないことが原因で、それらに対する読み取り操作が失敗した場合に発生します。

- この要求の前に別のクライアントによってコンテナーまたは BLOB が削除された場合に発生します。 
- コンテナーまたは BLOB を作成する前に、それらが存在するかどうかを確認する API 呼び出しを使用する場合に発生します。 CreateIfNotExists API は最初に HEAD 呼び出しを実行し、コンテナーまたは BLOB が存在するかどうかを確認します。存在しない場合は 404 エラーが返され、その後、2 番目の PUT 呼び出しを実行してコンテナーまたは BLOB を書き込みます。

**ステータス コード 409 (Conflict) の例**

- Create API を使用して最初に存在を確認することなくコンテナーまたは BLOB を作成する際、同じ名前のコンテナーまたは BLOB が既に存在する場合に発生します。 
- コンテナーが削除される際、その削除操作が完了する前に同じ名前の新しいコンテナーを作成しようとした場合に発生します。
- コンテナーまたは BLOB にリースを指定する際、既にリースが存在している場合に発生します。
 
**ステータス コード 412 (Precondition Failed) の例**

- 条件ヘッダーにより指定された条件が満たされていない場合に発生します。
- 指定されたリース ID がコンテナーまたは BLOB のリース ID と一致しない場合に発生します。

## 分析用にログ ファイルを生成する

このチュートリアルでは、Message Analyzer を使用して 3 種類のログ ファイルを操作しますが、実際にはどれか 1 つを選択して操作できます。

-  **サーバー ログ**, 、Azure Storage のログを有効にすると作成されます。 サーバー ログには、Azure Storage サービスの 1 つ、つまり BLOB、キュー、テーブル、ファイルに対して呼び出された各操作についてのデータが含まれます。 サーバー ログは、どの操作が呼び出され、どの状態コードが返されたかに加え、要求と応答に関するその他の詳細を示します。
-  **.NET クライアント ログ**, 、.NET アプリケーション内部からクライアント側のログ記録を有効にしたときに作成されます。 このクライアント ログには、クライアントが要求を準備し、応答を受信して処理する方法についての詳細情報が含まれます。 
-  **HTTP ネットワーク トレース ログ**, 、データを含む Azure Storage に対する操作について、HTTP または HTTPS 要求および応答データを収集します。 このチュートリアルでは、Message Analyzer を使用してネットワーク トレースを生成します。

### サーバー側のログとメトリックを構成する

まず、クライアント アプリケーションから分析用データを取得できるよう、Azure Storage のログとメトリックを構成する必要があります。 を介して、ログと、さまざまな方法でメトリックを構成することができます、 [Azure ポータル](portal.azure.com), 、PowerShell を使用してまたはプログラムを使用しています。 参照してください [ストレージ メトリックの有効化とメトリック データの表示](http://msdn.microsoft.com/library/azure/dn782843.aspx) と [ストレージ ログの有効化とログ データへのアクセス](http://msdn.microsoft.com/library/azure/dn782840.aspx) ログとメトリックを構成する方法についての MSDN のです。

**Azure ポータルの使用**

構成する、記憶域のログとメトリック アカウントを使用して、 [Azure ポータル](portal.azure.com), 、」の手順に従って [ストレージ アカウントの監視方法](storage-monitor-storage-account.md)します。

> [AZURE.NOTE] Azure ポータルを使用して分単位のメトリックを設定することはできません。 ただし、このチュートリアルのため、さらにアプリケーションが抱えるパフォーマンスの問題を調査するためにも、これを設定することをお勧めします。 分単位メトリックは、PowerShell (以下を参照)、プログラム、ストレージ クライアント ライブラリのいずれかを使用して設定できます。
>
> Azure ポータルで表示できるのは時間単位メトリックのみであり、分単位メトリックは表示できない点に注意してください。 

**PowerShell を使用**

Azure の PowerShell で開始するを参照してください。 [をインストールして、Azure PowerShell を構成する方法](../install-configure-powershell.md)します。

1. 使用して、 [Add-azureaccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) コマンドレットは、PowerShell ウィンドウへ、Azure ユーザー アカウントを追加します。

    ```
    Add-AzureAccount
    ```

2.  **Microsoft Azure へのサインイン** ウィンドウで、電子メール アドレスと、アカウントに関連付けられているパスワードを入力します。 Azure により資格情報が認証および保存され、ウィンドウが閉じます。
3. PowerShell ウィンドウで次のコマンドを実行し、既定のストレージ アカウントをチュートリアル用に使用しているストレージ アカウントに設定します。

    ```
    $SubscriptionName = 'Your subscription name'
    $StorageAccountName = 'yourstorageaccount' 
    Set-AzureSubscription -CurrentStorageAccountName $StorageAccountName -SubscriptionName $SubscriptionName 
    ```

4. BLOB サービスのストレージ ログを有効にします。 
 
    ```
    Set-AzureStorageServiceLoggingProperty -ServiceType Blob -LoggingOperations Read,Write,Delete -PassThru -RetentionDays 7 -Version 1.0 
    ```
5. 設定することを確認、Blob サービスのストレージ メトリックを有効にする **-metricstype** に `Minute`:

    ```
    Set-AzureStorageServiceMetricsProperty -ServiceType Blob -MetricsType Minute -MetricsLevel ServiceAndApi -PassThru -RetentionDays 7 -Version 1.0 
    ```

### .NET のクライアント側のログを構成する

.NET アプリケーションのクライアント側のログを構成するには、アプリケーションの構成ファイル (web.config または app.config) 内で .NET 診断を有効にします。 参照してください [クライアント側のストレージ クライアント ライブラリを使用してログを](http://msdn.microsoft.com/library/azure/dn782839.aspx) と [クライアント側の Microsoft Azure Storage SDK for Java を使用してログオン](http://msdn.microsoft.com/library/azure/dn782844.aspx) 詳細については、MSDN のです。

クライアント側のログには、クライアントが要求を準備し、応答を受信して処理する方法についての詳細情報が含まれます。

クライアント側のログは、アプリケーションの app.config または web.config ファイル内で構成されます。 詳細については、「 [クライアント側のストレージ クライアント ライブラリを使用してログを](http://msdn.microsoft.com/library/azure/dn782839.aspx) MSDN にします。

ストレージ クライアント ライブラリは、クライアント側のログ データをアプリケーションの構成ファイル (web.config または app.config) 内で指定した場所に格納します。 

### ネットワーク トレースを収集する

Message Analyzer を使用して、クライアント アプリケーションの動作時の HTTP/HTTPS ネットワーク トレースを収集することができます。 メッセージのアナライザーは [Fiddler](http://www.telerik.com/fiddler) バック エンドにします。 ネットワーク トレースを収集する前に、暗号化されていない HTTPS トラフィックを記録するよう、Fiddler を構成することをお勧めします。

1. インストール [Fiddler](http://www.telerik.com/download/fiddler)します。
2. Fiddler を起動します。
2. 選択 **ツール |Fiddler オプション**します。
3. [オプション] ダイアログで、いることを確認 **Capture HTTPS CONNECTs** と **Decrypt HTTPS Traffic** した場合は、次のようにします。

![Fiddler オプションの構成](./media/storage-e2e-troubleshooting/fiddler-options-1.png)

このチュートリアルでは、まず Message Analyzer でネットワーク トレースを収集して保存し、次に分析セッションを作成してトレースとログを分析します。 Message Analyzer でネットワーク トレースを収集するには:

1. Message Analyzer で、次のように選択します **ファイル |。Quick Trace |HTTPS が暗号化されていない**します。
2. すぐにトレースが開始します。 選択 **停止** トレースを停止できるように、ストレージ トラフィックのみをトレースすることを構成できます。
3. 選択 **編集** トレース セッションを編集します。
4. 選択、 **構成** の右側のリンクを **Microsoft-Pef-webproxy** ETW プロバイダーです。
5.  **の詳細設定** ダイアログ ボックスで、をクリックして、 **プロバイダー** ] タブをクリックします。
6.  **Hostname Filter** フィールドに、スペースで区切られたストレージ エンドポイントを指定します。 たとえば、次のようにエンドポイントを指定することができます。`storagesample` をストレージ アカウントの名前に変更してください。
    
    ``` 
    storagesample.blob.core.windows.net storagesample.queue.core.windows.net storagesample.table.core.windows.net 
    ```

7. ダイアログを終了し、 **再起動** Azure Storage ネットワーク トラフィックのみがトレースに含まれるように、ホスト名フィルターを使用してトレースの収集を開始します。

>[AZURE.NOTE] ネットワーク トレースの収集が完了したら、後に HTTPS トラフィックを復号化する Fiddler で変更した設定を元に戻すことを強くお勧めします。 Fiddler Options] ダイアログで、 **Capture HTTPS CONNECTs** と **Decrypt HTTPS Traffic** チェック ボックスをオンします。

参照してください [ネットワーク トレース機能を使用して](http://technet.microsoft.com/library/jj674819.aspx) 詳細については、Technet のです。

## Azure ポータルでメトリック データを確認する

アプリケーションが実行されている一定時間後で表示されるメトリック チャートを確認できます、 [Azure ポータル](portal.azure.com) 、サービスが実行された状況を確認します。 最初に、Azure ポータルでストレージ アカウントに移動しのグラフを追加、 **成功のパーセンテージ** メトリックです。

Azure ポータルで今すぐわかります **成功のパーセンテージ** 監視チャート内に他のメトリックと共に追加した場合します。 次に調査するシナリオでは Message Analyzer でログを分析しますが、成功率が 100% をいくらか下回っています。

メトリックを監視ページに追加する方法の詳細については、次を参照してください。 [方法: メトリック テーブルへのメトリックの追加](storage-monitor-storage-account.md#addmonitoringmetrics)します。

> [AZURE.NOTE] ストレージ メトリックを有効にした後、Azure ポータルに表示されるメトリック データの時間がかかる場合があります。 これは、1 つ前の時間単位メトリックが、現在の単位時間が経過するまでは Azure ポータルに表示されないためです。 また、分単位メトリックについては現在のところ Azure ポータルには表示されません。 つまり、メトリックを有効にしたタイミングによっては、メトリック データが表示されるまでに最大で 2 時間を要する可能性があります。

## AzCopy を使用してサーバー ログをローカル ディレクトリにコピーする

Azure Storage は、サーバー ログ データを BLOB に書き込みますが、メトリックについてはテーブルに書き込みます。 ログ BLOB は、ストレージ アカウントの `$logs` コンテナー内にあります。 ログ BLOB は、年、月、日、時間により階層的に名前が付けられるので、調査する時間範囲が簡単に見つかります。 たとえば、`storagesample` アカウントでの、2015 年 1 月 2 日の午前 8 時から 9 時に記録されたログ BLOB のコンテナーは、`https://storagesample.blob.core.windows.net/$logs/blob/2015/01/08/0800` です。 このコンテナー内の個々の BLOB は、`000000.log` から始まる連番で名前が付けられます。

AzCopy コマンドライン ツールを使用して、これらのサーバー側のログ ファイルをローカル コンピューター上の好きな場所にダウンロードすることができます。 たとえば、次のコマンドを使用して、2015 年 1 月 2 日に記録された BLOB 操作のログ ファイルを、`C:\Temp\Logs\Server` フォルダーにダウンロードすることができます。`<storageaccountname>` はストレージ アカウント名に置き換え、`<storageaccountkey>` はアカウントのアクセス キーに置き換えます。

    AzCopy.exe /Source:http://<storageaccountname>.blob.core.windows.net/$logs /Dest:C:\Temp\Logs\Server /Pattern:"blob/2015/01/02" /SourceKey:<storageaccountkey> /S /V

AzCopy はからダウンロードする、 [Azure のダウンロード](http://azure.microsoft.com/downloads/) ページです。 AzCopy の使用に関する詳細については、「 [Microsoft Azure Storage で AzCopy を使用する方法](storage-use-azcopy.md)します。

サーバー側のログのダウンロードの詳細については、次を参照してください。 [ストレージ ログの有効化とログ データへのアクセス](http://msdn.microsoft.com/library/azure/dn782840.aspx#DownloadingStorageLogginglogdata)します。 

## Microsoft Message Analyzer を使用してログ データを分析する

Microsoft Message Analyzer は、トラブルシューティングや診断のシナリオにおいて、プロトコル メッセージング トラフィック、イベント、およびその他のシステムやアプリケーションに対してキャプチャ、表示、分析を実施するためのツールです。 また、Message Analyzer では、ログや保存したトレース ファイルからのデータの読み込み、集計、分析ができます。 Message Analyzer の詳細については、次を参照してください。 [Microsoft Message Analyzer の操作ガイド](http://technet.microsoft.com/library/jj649776.aspx)します。

Message Analyzer には、サーバー、クライアント、およびネットワーク ログの分析に役立つ、Azure Storage 向けのアセットが含まれています。 このセクションでは、それらのツールを使用して、ストレージ ログにおける低い成功率の問題に対処する方法を説明します。

### Message Analyzer と Azure Storage アセットをダウンロードしてインストールする

1. ダウンロード [Message Analyzer](http://www.microsoft.com/download/details.aspx?id=44226) 、microsoft ダウンロード センター、およびインストーラーを実行します。
2. Message Analyzer を起動します。
3.  **開始** に移動] ページで、 **ダウンロード**, 、そしてでフィルター処理 **Azure Storage**します。 以下の画像のように、Azure Storage アセットが表示されます。
4. クリックして **Sync All Displayed Items** Azure Storage アセットをインストールします。 以下に利用できるアセットを挙げます。 
    - **Azure Storage Color Rules:** Azure Storage カラー ルールを有効にすると、色、テキスト、およびフォント スタイルを使用して、トレース内の特定の情報を含むメッセージを強調表示する特別なフィルターを定義します。
    - **Azure Storage Charts:** Azure Storage チャートは、サーバー ログ データをグラフ化する定義済みのグラフ。 現時点で Azure Storage チャートを使用する場合、Analysis Grid に読み込むことができるのはサーバー ログのみである点に注意してください。
    - **Azure Storage Parsers:** Azure Storage パーサーは Analysis Grid に表示するために Azure Storage クライアント、サーバー、および HTTP のログを解析します。
    - **Azure Storage Filters:** Azure Storage フィルターは、Analysis Grid でデータをクエリするために使用できる定義済みの条件。
    - **Azure Storage View Layouts:** Azure Storage ビュー レイアウトは列の定義済みレイアウトであり、Analysis Grid 内のグループ化します。
4. アセットのインストール後に、Message Analyzer を再起動します。

![Message Analyzer の [Start] ページ](./media/storage-e2e-troubleshooting/mma-start-page-1.png)

> [AZURE.NOTE] このチュートリアルの目的で示すように Azure Storage アセットはすべてをインストールします。

### ログ ファイルを Message Analyzer にインポートする

すべての保存済みログ ファイル (サーバー側、クライアント側、ネットワーク) を、分析のために Microsoft Message Analyzer 内の単一のセッションにインポートできます。

1.  **ファイル** Microsoft Message Analyzer のメニューをクリックして **新しいセッション**, 、] をクリックし、 **Blank Session**します。  **新しいセッション** ] ダイアログ ボックスで、分析セッションの名前を入力します。  **セッションの詳細** パネルで、をクリックして、 **ファイル** ] ボタンをクリックします。 
1. Message Analyzer によって生成されるネットワーク トレース データを読み込むには、をクリックして **ファイルの追加**, を参照して、web トレース セッションから .matp ファイルを保存した場所に、.matp ファイルを選択] をクリックして **開く**します。 
1. をクリックすると、サーバー側のログ データを読み込むには、 **ファイルの追加**, 、サーバー側ログをダウンロードした場所に移動し、分析、およびをクリックする時間範囲のログ ファイルを選択して **開く**します。 次に、 **セッションの詳細** パネルで、設定、 **Text Log Configuration** ドロップダウンには、各サーバー側のログ ファイルを **[azurestoragelog]** Microsoft Message Analyzer がログ ファイルを正しく解析できるようにすることを確認します。
1. をクリックすると、クライアント側のログ データを読み込むには、 **ファイルの追加**, 、クライアント側ログを保存した場所に移動し、分析、およびをクリックするログ ファイルの選択 **開く**します。 次に、 **セッションの詳細** パネルで、設定、 **Text Log Configuration** ドロップダウンには、各クライアント側のログ ファイルを **[azurestorageclientdotnetv4]** Microsoft Message Analyzer がログ ファイルを正しく解析できるようにすることを確認します。
1. クリックして **開始** で、 **新しいセッション** 読み込みし、ログ データを解析するためのダイアログ。 ログ データは Message Analyzer Analysis Grid に表示されます。

以下の画像では、サーバー、クライアント、およびネットワーク トレースのログ ファイルで構成したセッションの例が表示されています。

![Message Analyzer のセッションの構成](./media/storage-e2e-troubleshooting/configure-mma-session-1.png)

Message Analyzer はログ ファイルをメモリに読み込むという点に注意してください。 大きなサイズのログ データ セットを持っている場合は、Message Analyzer のパフォーマンスを最大限に引き出すため、そのファイルにフィルターを適用することをお勧めします。

最初に、確認したいタイム フレームを決めますが、このタイム フレームはできる限り短い期間にします。 多くの場合、確認するタイム フレームは数分から、最大でも数時間に制限する必要があります。 必要条件を満たすログ セットの中から、最小サイズのものをインポートします。

まだサイズの大きいログ データが残っている場合は、セッション フィルターを指定して、読み込む前にログ データをフィルター処理する必要があります。  **Session Filter** ボックスで、選択、 **ライブラリ** クリックを定義済みのフィルターを選択します。 たとえば、選択 **Global Time Filter I** Azure Storage フィルターからフィルター処理する時間間隔にします。 その後、フィルター条件を編集し、確認したい時間間隔について開始および終了時のタイムスタンプを指定することができます。 特定の状態コードによってフィルターを適用することもできます。たとえば、状態コードが 404 のログ エントリのみを読み込むように選択できます。

Microsoft Message Analyzer にログ データをインポートする方法の詳細については、次を参照してください。 [メッセージ データの取得](http://technet.microsoft.com/library/dn772437.aspx) technet します。

### クライアント要求 ID を使用してログ ファイル データを関連付ける

Azure ストレージ クライアント ライブラリは、すべての要求に対して自動的に一意のクライアント要求 ID を生成します。 この値はクライアント ログ、サーバー ログ、ネットワーク トレースに書き込まれるため、Message Analyzer 内の 3 種類のログすべてにわたって、この値をデータの関連付けに使用することができます。 参照してください [クライアント要求 ID](storage-monitoring-diagnosing-troubleshooting.md#client-request-id) 詳細については、クライアント要求 id です。

以下のセクションでは、定義済みのカスタム レイアウト ビューを使用して、クライアント要求 ID に基づきデータの関連付けとグループ化を行う方法について説明します。

### ビュー レイアウトを選択して Analysis Grid に表示する

Message Analyzer 向けの Storage アセットには Azure Storage View Layouts という定義済みのビューが含まれていますが、これを使用すると、さまざまなシナリオに役立つグループや列によってデータを表示することができます。 カスタム ビュー レイアウトを作成し、それを保存して再利用することもできます。 

以下の画像、 **ビューのレイアウトを** を選択して使用できるメニューで、 **ビューのレイアウトを** ツール バー リボンからです。 Azure Storage のビュー レイアウトは、[グループ化されて、 **Azure Storage** 、メニュー内のノードです。 検索ボックスで `Azure Storage` を検索すると、Azure Storage ビュー レイアウトのみでフィルター処理できます。 ビュー レイアウトの横にある星を選択してお気に入りに設定し、メニューの最上部に表示することもできます。

![[View Layout] メニュー](./media/storage-e2e-troubleshooting/view-layout-menu.png)

まず始めに、選択 **ClientRequestID and Module でグループ化された**します。 このグループの表示レイアウト ログ、3 つすべてのログからデータを最初はソース ログ ファイルで、次のクライアント要求 ID (または **モジュール** Message Analyzer で)。 このビューを使用すると、特定のクライアント要求 ID へとドリル ダウンし、そのクライアント要求 ID について、3 種類すべてのログ ファイルのデータを見ることができます。

以下の画像では、このレイアウト ビューがサンプル ログ データに適用され、列のサブセットが表示されています。 特定のクライアント要求 ID について、Analysis Grid がクライアント ログ、サーバー ログ、ネットワーク トレースのデータを表示していることがわかります。

![Azure Storage View Layout](./media/storage-e2e-troubleshooting/view-layout-client-request-id-module.png)

>[AZURE.NOTE] ログ ファイルの種類の複数の列があるため、Analysis Grid に複数のログ ファイルのデータを表示すると、いくつかの列が特定の行をデータは含まれません。 たとえば上の画像では、 
クライアント ログの行のすべてのデータを表示しない、 **タイムスタンプ**, 、**TimeElapsed**, 、**ソース**, 、および **宛先** 列、これらの列は、クライアント ログに存在しないが、ネットワーク トレース内に存在しているためです。 同様に、 **タイムスタンプ** 列には、サーバーのログからのタイムスタンプ データが表示されますが、データが表示されないため、 **TimeElapsed**, 、**ソース**, と **宛先** 列で、サーバーのログの一部ではないです。 

Azure Storage ビュー レイアウトを使用するだけでなく、独自のビュー レイアウトを定義して保存することもできます。 データのグループ化に必要なその他のフィールドを選択し、そのグループをカスタム レイアウトの一部として保存することができます。 

### Analysis Grid にカラー ルールを適用する

Storage アセットにはカラー ルールも含まれており、これによって Analysis Grid で異なる種類のエラーを視覚的に識別できます。 定義済みのカラー ルールは HTTP エラーに適用されるため、サーバー ログとネットワーク トレースに対してだけ影響があります。

色のルールを適用する選択 **色ルール** ツール バー リボンからです。 Azure Storage カラー ルールがメニューに表示されます。 このチュートリアルでは、選択 **Client Errors (StatusCode between 400 and 499)**, 、次の図に示すようにします。

![Azure Storage View Layout](./media/storage-e2e-troubleshooting/color-rules-menu.png)

Azure Storage カラー ルールを使用するだけでなく、独自のカラー ルールを定義して保存することもできます。

### ログ データをグループ化してフィルターを適用し、400 番台のエラーを見つける

次に、ログ データをグループ化してフィルターを適用し、400 番台のすべてのエラーを見つけます。

1. 検索、 **StatusCode** Analysis Grid 内の列見出しをクリックして列を右クリックして **グループ**します。
2. 次に、グループ化の条件、 **ClientRequestId** 列です。 これで、Analysis Grid 内のデータが状態コードとクライアント要求 ID によって整理されたことがわかります。
1. View Filter ツール ウィンドウがまだ表示されていない場合、これを表示します。 ツール バー リボンで選択 **ツール ウィンドウ**, 、し **ビュー フィルター**します。
2. 400 番台のエラーのみを表示するログ データを抽出するには、次のフィルター条件を追加、 **ビュー フィルター** ウィンドウ、およびクリック **適用**:

        (AzureStorageLog.StatusCode >= 400 && AzureStorageLog.StatusCode <=499) || (HTTP.StatusCode >= 400 && HTTP.StatusCode <= 499)

以下の画像では、このグループ化およびフィルター適用の結果が表示されています。 展開する、 **ClientRequestID** ステータス コード 409 のグループの下にあるフィールドは、そのステータス コードを引き起こした操作を例を示します。

![Azure Storage View Layout](./media/storage-e2e-troubleshooting/400-range-errors1.png)

このフィルターを適用すると、わかること、クライアント ログの行が除外されているクライアントとログは含まれません、 **StatusCode** 列です。 まず、サーバーおよびネットワーク トレース ログを確認して 404 エラーを見つけてから、クライアント ログに戻ってそのエラーを引き起こしたクライアント操作を調べます。

>[AZURE.NOTE] フィルター処理すること、 **StatusCode** 列しつつ、状態コードが null であるログ エントリを含むフィルターに式を追加するには、クライアント ログを含む 3 つすべてのログのデータを表示します。 このフィルター式を作成するには、以下を使用します:
>
> <code>&#42;StatusCode >= 400 or !&#42;StatusCode</code> 
>
> このフィルターは、クライアント ログのすべての行を返し、サーバー ログと HTTP ログからは状態コードが 400 を超える行のみを返します。 クライアント要求 ID とモジュールによりグループ化したビュー レイアウトにこれを適用した場合、ログ エントリを検索するか下へスクロールすれば、3 つのログがすべて表示されている場所を見つけることができます。   

### ログ データにフィルターを適用して 404 エラーを見つける

Storage アセットには定義済みのフィルターが含まれており、これを使用してログ データを絞り込み、探しているエラーや傾向を見つけることができます。 次は、2 つの定義済みフィルターを適用します。1 つは 404 エラーについてサーバーおよびネットワーク トレース ログに適用するフィルターで、もう 1 つは指定した時間範囲についてそれらのデータに適用するフィルターです。

1. View Filter ツール ウィンドウがまだ表示されていない場合、これを表示します。 ツール バー リボンで選択 **ツール ウィンドウ**, 、し **ビュー フィルター**します。
2. View Filter ウィンドウで選択 **ライブラリ**, 、および検索 `Azure Storage` フィルターするには、Azure ストレージを検索します。 フィルターを選択 **404 (Not Found) のすべてのログ メッセージ**します。
3. 表示、 **ライブラリ** メニューをもう一度選択およびを探す、 **Global Time Filter**します。
4. フィルター内に表示されるタイムスタンプを、確認したい範囲に編集します。 これにより、分析するデータの範囲を絞り込むことができます。
5. フィルターは以下の例のように表示されます。 クリックして **適用** Analysis Grid にフィルターを適用します。

        ((AzureStorageLog.StatusCode == 404 || HTTP.StatusCode == 404)) And 
        (#Timestamp >= 2014-10-20T16:36:38 and #Timestamp <= 2014-10-20T16:36:39)

![Azure Storage View Layout](./media/storage-e2e-troubleshooting/404-filtered-errors1.png)

### ログ データを分析する

データをグループ化してフィルターを適用したので、404 エラーを生成した個々の要求について詳細を調べることができます。 現在のビュー レイアウトでは、データはまずクライアント要求 ID、次にログ ソースによってグループ化されています。 StatusCode フィールドに 404 が含まれる要求によってフィルター処理しているため、サーバーおよびネットワーク トレースのデータのみが表示され、クライアント ログのデータは表示されません。

以下の画像の要求を見ると、BLOB が存在しないために、Get Blob 操作によって 404 が発生しています。 関連データを表示するために、標準ビューから削除されている列があることに注意してください。

![フィルター選択されたサーバーとネットワークのトレース ログ](./media/storage-e2e-troubleshooting/server-filtered-404-error.png)

次に、このクライアント要求 ID とクライアント ログ データを関連付けて、エラーが発生した際にクライアントがどのように対処していたかを確認します。 このセッションのために新しい Analysis Grid ビューを表示してクライアント ログ データを確認することができますが、このビューは 2 番目のタブに開きます。

1. 値を最初に、コピー、 **ClientRequestId** フィールドをクリップボードにします。 こうことどちらかの行を選択すると、検索、 **ClientRequestId** フィールドに、データ値を右クリックし、選択する **Copy 'ClientRequestId'**します。 
1. ツール バー リボンで選択 **新しいビューアー**, 選択してから、 **Analysis Grid** 新しいタブを開きます。 新しいタブは、ログ ファイル内のすべてのデータを、グループ化やフィルター、カラー ルールを適用せずに表示します。 
2. ツール バー リボンで選択 **ビューのレイアウトを**, 選択してから、 **All .NET Client Columns** 下にある、 **Azure Storage** セクションです。 このビュー レイアウトでは、サーバーおよびネットワーク トレース ログだけでなく、クライアント ログのデータも表示されます。 既定の並べ替えは、 **MessageNumber** 列です。
3. 次に、クライアント ログでクライアント要求 ID を検索します。 ツール バー リボンで選択 **Find Messages**, でクライアント要求 ID について、カスタム フィルターを指定、 **検索** フィールドです。 フィルターには、この構文を使用して独自のクライアント要求 ID を指定します。

        *ClientRequestId == "398bac41-7725-484b-8a69-2a9e48fc669a"

Message Analyzer は、検索条件がクライアント要求 ID に一致する最初のログ エントリを見つけて選択します。 クライアント ログにエントリがあるいくつかの各クライアント要求 ID でグループ化するため、 **ClientRequestId** フィールドをそれらをまとめて確認しやすきます。 以下の画像では、指定したクライアント要求 ID のクライアント ログにあるすべてのメッセージが表示されています。 

![404 エラーを示すクライアント ログ](./media/storage-e2e-troubleshooting/client-log-analysis-grid1.png)

これら 2 つのタブのビュー レイアウトに表示されているデータを使用することで、要求データを分析して何がエラーを引き起こしたのかを判断することができます。 また、これに先行する要求を確認して、以前のイベントが 404 エラーを引き起こした可能性があるかどうかを調べることもできます。 たとえば、このクライアント要求 ID に先行するクライアント ログを確認して、BLOB が削除されていないか、あるいはクライアント アプリケーションがコンテナーまたは BLOB に対して CreateIfNotExists API を呼び出したことがエラーの発生原因ではないかを判断できます。 クライアント ログには、blob のアドレスを見つけることができます、 **説明** フィールド; で、サーバーとネットワーク トレース ログにこの情報が表示されます、 **概要** フィールドです。

404 エラーを引き起こした BLOB のアドレスが判明したら、さらに調査を進めることができます。 同じ BLOB に対する操作に関連付けられた他のメッセージのログ エントリを検索した場合、クライアントが事前にそのエンティティを削除していたかどうかが確認できます。 

## 他の種類のストレージ エラーを分析する

Message Analyzer を使用したログ データの分析に慣れてきたところで、ビュー レイアウト、カラー ルール、検索、フィルター処理を使用して他の種類のログ データを分析することもできます。 以下の表は、発生する可能性のある問題と、それらの特定に使用できるフィルター条件の一覧です。 これらのフィルターおよび Message Analyzer フィルタ リング言語の構築の詳細については、次を参照してください。 [メッセージ データのフィルタ リング](http://technet.microsoft.com/library/jj819365.aspx)します。

|    調査目的…                                                                                               |    使用するフィルター式…                                                                                                                                                                                                                                        |    式が適用されるログ (クライアント、サーバー、ネットワーク、すべて)    |
|------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
|    キューのメッセージ配信での予期しない遅延                                                              |    Azurestorageclientdotnetv4.description にには、「再試行中操作に失敗しました。」が含まれています。                                                                                                                                                                                |    クライアント                                                        |
|    HTTP の PercentThrottlingError の増加                                                                       |    HTTP です。いる Response.StatusCode = 500 & #124; #124 文字です。HTTP です。いる Response.StatusCode 503 = =                                                                                                                                                                                          |    ネットワーク                                                       |
|    PercentTimeoutError の増加                                                                               |    HTTP です。いる Response.StatusCode = 500                                                                                                                                                                                                                             |    ネットワーク                                                       |
|    PercentTimeoutError の増加 (すべて)                                                                         |    * StatusCode = 500                                                                                                                                                                                                                                          |    すべて                                                           |
|    PercentNetworkError を増加します。                                                                               |    増加 AzureStorageClientDotNetV4.EventLogEntry.Level < 2                                                                                                                                                                                                          |    クライアント                                                        |
|    HTTP 403 (禁止) メッセージ                                                                                 |    HTTP です。いる Response.StatusCode 403 = =                                                                                                                                                                                                                             |    ネットワーク                                                       |
|    HTTP 404 (Not found) メッセージ                                                                                 |    HTTP です。いる Response.StatusCode 404 = =                                                                                                                                                                                                                             |    ネットワーク                                                       |
|    404 (すべて)                                                                                                     |    * StatusCode = 404                                                                                                                                                                                                                                          |    すべて                                                           |
|    Shared Access Signature (SAS) 認証の問題                                                             |    AzureStorageLog.RequestStatus"SASAuthorizationError"= =                                                                                                                                                                                                     |    ネットワーク                                                       |
|    HTTP 409 (Conflict) メッセージ                                                                                  |    HTTP です。いる Response.StatusCode 409 = =                                                                                                                                                                                                                             |    ネットワーク                                                       |
|    409 (すべて)                                                                                                     |    * StatusCode = 409                                                                                                                                                                                                                                          |    すべて                                                           |
|    Percentsuccess が低い、または分析ログ エントリがあるトランザクション ステータスが ClientOtherErrors の操作    |    AzureStorageLog.RequestStatus ="ClientOtherError"                                                                                                                                                                                                         |    サーバー                                                        |
|    Nagle 警告                                                                                               |    ((AzureStorageLog.EndToEndLatencyMS-AzureStorageLog.ServerLatencyMS) > (AzureStorageLog.ServerLatencyMS * 1.5)) と (AzureStorageLog.RequestPacketSize < 1460年) と (AzureStorageLog.EndToEndLatencyMS - AzureStorageLog.ServerLatencyMS > = 200)        |    サーバー                                                        |
|    サーバーおよびネットワーク ログで時間の範囲                                                                    |    #タイムスタンプ > = 2014年-10-20T16:36:38 と #Timestamp < = 2014年-10-20T16:36:39                                                                                                                                                                                     |    サーバー、ネットワーク                                               |
|    サーバーのログで時間の範囲                                                                                |    AzureStorageLog.Timestamp > = 2014年-10-20T16:36:38 と AzureStorageLog.Timestamp < = 2014年-10-20T16:36:39                                                                                                                                                     |    サーバー                                                        |


## 次のステップ

Azure Storage におけるエンド ツー エンド シナリオのトラブルシューティングの詳細については、次のリソースを参照してください。

- [Storage の監視、診断、およびトラブルシューティング](storage-monitoring-diagnosing-troubleshooting.md)
- [Storage Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx)
- [ストレージ アカウントの監視方法](storage-monitor-storage-account.md)
- [AzCopy コマンド ライン ユーティリティを使ったデータの転送](storage-use-azcopy)
- [Microsoft Message Analyzer の操作ガイド](http://technet.microsoft.com/library/jj649776.aspx)
 
 


