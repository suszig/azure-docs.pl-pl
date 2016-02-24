<properties
    pageTitle="Azure リソース マネージャー テンプレートを使用して監視および診断を含む Windows 仮想マシンを登録する | Microsoft Azure"
    description="Azure リソース マネージャー テンプレートを使用して Azure 診断の拡張機能を含む新しい Windows 仮想マシンを作成する"
    services="virtual-machines"
    documentationCenter=""
    authors="sbtron"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="saurabh"/>

# Azure リソース マネージャー テンプレートを使用して監視および診断を含む Windows 仮想マシンを登録する

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。 

Azure 診断の拡張機能は、Windows ベースの Azure 仮想マシンに監視および診断機能を提供します。 Azure リソース マネージャー テンプレートの一部として拡張機能を含めることによって、仮想マシンでこれらの機能を有効にすることができます。 参照してください [VM 拡張機能での Azure リソース マネージャー テンプレートの作成](virtual-machines-extensions-authoring-templates.md) 詳細について、バーチャル マシン テンプレートの一部として任意の拡張機能を追加します。 この記事では、Windows 仮想マシン テンプレートに Azure 診断の拡張機能を追加する方法について説明します。  
  

## VM のリソースの定義に Azure 診断の拡張機能を追加する 

Windows 仮想マシンで診断の拡張機能を有効にするには、リソース マネージャー テンプレートで拡張機能を VM のリソースとして追加する必要があります。

単純なリソース マネージャーのベースの仮想マシンを追加する拡張機能の構成、 *リソース* 、仮想マシンの配列。 

    "resources": [
                {
                    "name": "Microsoft.Insights.VMDiagnosticsSettings",
                    "type": "extensions",
                    "location": "[resourceGroup().location]",
                    "apiVersion": "2015-06-15",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
                    ],
                    "tags": {
                        "displayName": "AzureDiagnostics"
                    },
                    "properties": {
                        "publisher": "Microsoft.Azure.Diagnostics",
                        "type": "IaaSDiagnostics",
                        "typeHandlerVersion": "1.5",
                        "autoUpgradeMinorVersion": true,
                        "settings": {
                            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
                            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
                        },
                        "protectedSettings": {
                            "storageAccountName": "[parameters('existingdiagnosticsStorageAccountName')]",
                            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
                            "storageAccountEndPoint": "https://core.windows.net"
                        }
                    }
                }
            ]


もう 1 つの一般的な方法では、拡張機能の構成を仮想マシンのリソース ノードの下に定義するのではなく、テンプレートのルート リソース ノードに追加します。 この方法では、拡張子とを使用して仮想マシンの間の階層型の関係を明示的に指定する必要がある、 *名前* と *型* 値。 次に例を示します。 
  
    "name": "[concat(variables('vmName'),'Microsoft.Insights.VMDiagnosticsSettings')]",
    "type": "Microsoft.Compute/virtualMachines/extensions",

拡張機能は常に仮想マシンと関連付けられるため、仮想マシンのリソース ノードの下に直接定義するか、ベース レベルで定義して階層的な名前付け規則を使用することで、仮想マシンとの関連付けを行うことができます。

仮想マシンのスケール設定の拡張機能の構成が指定されている、 *extensionProfile* のプロパティ、 *VirtualMachineProfile*します。
   
 *パブリッシャー* の値を持つプロパティ **Microsoft.Azure.Diagnostics** と *型* の値を持つプロパティ **IaaSDiagnostics** Azure 診断拡張機能を一意に識別します。

値、 *名前* リソース グループ内の拡張機能を参照するプロパティを使用することができます。 具体的に設定すること **Microsoft.Insights.VMDiagnosticsSettings** を確保する、監視をグラフ表示を正しく Azure クラシック ポータルで Azure クラシック ポータル ポータルで簡単に識別できることを有効になります。

 *TypeHandlerVersion* を使用するには、拡張機能のバージョンを指定します。 設定 *autoUpgradeMinorVersion* マイナー バージョンを **true** により提供される拡張機能の最新のマイナー バージョンが表示されます。 常に設定することを強くお勧め *autoUpgradeMinorVersion* が常にある **true** 常に最新の利用可能な診断拡張機能を使用して、すべての新機能とバグの修正を取得することです。 

 *設定* 要素に設定し、拡張機能 (パブリック構成とも呼ばれます) から読み取らできる拡張機能の構成プロパティが含まれています。  *Xmlcfg* プロパティには、診断ログ用の xml ベースの構成が含まれています。 パフォーマンス カウンターなど、診断エージェントによって収集されます。 参照してください [診断構成スキーマ](https://msdn.microsoft.com/library/azure/dn782207.aspx) 自体 xml スキーマの詳細についてです。 Azure リソース マネージャー テンプレート内の変数として、実際の xml 構成を保存し、連結し、一般的な方法があり、base64 エンコードの値を設定して *xmlcfg*します。 セクションを参照 [診断の構成変数](#diagnostics-configuration-variables) 変数に xml を格納する方法の詳細に把握します。  *StorageAccount* プロパティは、データの転送にどの診断ストレージ アカウントの名前を指定します。 
 
プロパティ *protectedSettings* (プライベートとしても参照される構成) を設定することができますが、設定後に読み取ることができません。 書き込み専用の性質 *protectedSettings* はストレージ アカウント キーのような機密情報を格納するため、診断データが書き込まれます。    

## 診断ストレージ アカウントをパラメーターとして指定する 

上記の診断拡張機能の json スニペットには、2 つのパラメーターが前提としています *existingdiagnosticsStorageAccountName* と
*existingdiagnosticsStorageAccountName* 診断ストレージ アカウントが診断データの格納場所を指定します。 診断ストレージ アカウントをパラメーターとして指定すると、たとえばテスト用と運用環境のデプロイメント用で異なる診断ストレージ アカウントを使用する場合などに、異なる環境間で診断ストレージ アカウントを変更することが簡単になります。  

        "existingdiagnosticsStorageAccountName": {
            "type": "string",
            "metadata": {
        "description": "The name of an existing storage account to which diagnostics data will be transfered."
            }        
        },
        "existingdiagnosticsStorageResourceGroup": {
            "type": "string",
            "metadata": {
        "description": "The resource group for the storage account specified in existingdiagnosticsStorageAccountName"
            }
        }

診断ストレージ アカウントは仮想マシンのリソース グループとは異なるリソース グループに指定することをお勧めします。 リソース グループは、独自の有効期間を持つデプロイメント単位と見なすことができます。新しい構成の更新により、仮想マシンはデプロイされたり、再デプロイされたりしますが、診断データを、仮想マシンのデプロイメント全体の同じストレージ アカウントに保存したい場合もあります。 異なるリソースにストレージ アカウントがあると、そのストレージ アカウントがさまざまな仮想マシンのデプロイメントからデータを受け入れられるようになり、異なるバージョンでの問題のトラブルシューティングが簡単になります。

>[AZURE.NOTE] Visual Studio から windows バーチャル マシン テンプレートを作成する場合は、バーチャル マシンの VHD がアップロードされる、同じストレージ アカウントを使用する既定のストレージ アカウントを設定する場合があります。 これにより、VM の初期セットアップが簡素化します。 パラメーターとして渡すことができる別のストレージ アカウントを使用するようにテンプレートをリファクタリングする必要があります。 

## 診断構成の変数
 
上記の診断拡張機能の json スニペットを定義、 *accountid* 、診断ストレージのストレージ アカウント キーの取得を簡素化する変数。   
    
    "accountid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/',parameters('existingdiagnosticsStorageResourceGroup'), '/providers/','Microsoft.Storage/storageAccounts/', parameters('existingdiagnosticsStorageAccountName'))]"


 *Xmlcfg* 連結された複数の変数を使用して診断拡張機能のプロパティを定義します。 これらの変数の値は xml 形式であるため、JSON 変数を設定するときに正しくエスケープする必要があります。

次に、Windows イベント ログおよび診断インフラストラクチャ ログと共に、標準のシステム レベルのパフォーマンス カウンターを収集する診断構成の xml について説明します。 構成をテンプレートの変数セクションに直接貼り付けることができるように、正しくエスケープされ、書式設定されています。 参照してください、 [診断構成スキーマ](https://msdn.microsoft.com/library/azure/dn782207.aspx) 構成 xml の場合は、人間より読み取り可能な例についてです。
    
        "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
        "wadperfcounters1": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Privileged Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU privileged time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% User Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU user time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Processor Information(_Total)\\Processor Frequency\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"CPU frequency\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\System\\Processes\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Processes\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Threads\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Handle Count\" sampleRate=\"PT15S\" unit=\"Count\"><annotation displayName=\"Handles\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\% Committed Bytes In Use\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Memory usage\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Available Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory available\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Committed Bytes\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory committed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\Memory\\Commit Limit\" sampleRate=\"PT15S\" unit=\"Bytes\"><annotation displayName=\"Memory commit limit\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active time\" locale=\"en-us\"/></PerformanceCounterConfiguration>",
        "wadperfcounters2": "<PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Read Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active read time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\% Disk Write Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk active write time\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Transfers/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Reads/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk read operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Writes/sec\" sampleRate=\"PT15S\" unit=\"CountPerSecond\"><annotation displayName=\"Disk write operations\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Read Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk read speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\PhysicalDisk(_Total)\\Disk Write Bytes/sec\" sampleRate=\"PT15S\" unit=\"BytesPerSecond\"><annotation displayName=\"Disk write speed\" locale=\"en-us\"/></PerformanceCounterConfiguration><PerformanceCounterConfiguration counterSpecifier=\"\\LogicalDisk(_Total)\\% Free Space\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Disk free space (percentage)\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
        "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters1'), variables('wadperfcounters2'), '<Metrics resourceId=\"')]",
        "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name , '/providers/', 'Microsoft.Compute/virtualMachines/')]",
        "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>"

上記の設定でメトリックの定義の xml ノードは重要な構成要素内の xml の前半のパフォーマンス カウンターの定義方法を定義する *PerformanceCounter* ノードを集計し、保存します。 

> [AZURE.IMPORTANT] これらのメトリックは、Azure ポータルで、監視グラフとアラートをドライブします。   **メトリック** を持つノード、 *resourceID* と **MetricAggregation** が監視データは Azure ポータルで VM を表示する場合に、VM の診断構成に含める必要があります。 

メトリックの定義に関する xml の例を次に示します。 

        <Metrics resourceId="/subscriptions/subscription().subscriptionId/resourceGroups/resourceGroup().name/providers/Microsoft.Compute/virtualMachines/vmName">
            <MetricAggregation scheduledTransferPeriod="PT1H"/>
            <MetricAggregation scheduledTransferPeriod="PT1M"/>
        </Metrics>

 *ResourceID* 属性は、サブスクリプションの仮想マシンを一意に識別します。 テンプレートがデプロイしているサブスクリプションとリソース グループに基づいて値を自動的に更新するように、subscription() と resourceGroup() 関数を使用してください。

ループ内で複数の仮想マシンを作成するかどうかは、設定する必要が、 *resourceID* 値でそれぞれ個別の VM を正しく区別するために copyIndex() 関数を使用します。  *XmlCfg* の値は、次のようにこれをサポートするために更新することができます。  

    "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), concat(parameters('vmNamePrefix'), copyindex()), variables('wadcfgxend')))]", 

MetricAggregation 値 *PT1H* と *PT1M* 1 分以上集計と集計に 1 時間以上のことを意味します。

## ストレージの WADMetrics テーブル

上記のメトリックの構成によって、次の名前付け規則による診断ストレージ アカウントのテーブルが生成されます。

- **WADMetrics** : WADMetrics のすべてのテーブルの標準のプレフィックス
- **PT1H** または **PT1M** : テーブルに 1 時間以上、または 1 分のデータ集計にはが含まれていることを示します。
- **P10D** : テーブルはテーブルがデータの収集を開始時から 10 日間データを含むことを示します。
- **V2S** : 文字列定数
- **yyyymmdd** : テーブルがデータの収集を開始された日付

例: *WADMetricsPT1HP10DV2S20151108* は 2015 年 11 月-11 - から 10 日間に 1 時間以上に集計されたメトリック データが含まれます    

各 WADMetrics テーブルには次の列が含まれます。

- **PartitionKey**: partitionkey は、に基づいて構築された、 *resourceID* VM リソースを一意に識別する値。 例:: 002Fsubscriptions:<subscriptionID>: 002FresourceGroups:002F<ResourceGroupName>: 002Fproviders:002FMicrosoft:002ECompute:002FvirtualMachines:002F<vmName>  
- **RowKey** : 形式に準じます <Descending time tick>:<Performance Counter Name>です。 降順の時間ティック計算は、最大時間ティックから集計期間の開始時間を引いたものです。 例:  2015 年 11 月 10 で、サンプリングの期間を開始し、00:00Hrs UTC、計算が立つ: DateTime.MaxValue.Ticks - (新しい DateTime(2015,11,10,0,0,0,DateTimeKind.Utc) します。タイマー刻み)。 メモリで使用可能なバイトのパフォーマンス カウンターの場合、行キーは2519551871999999999__:005CMemory:005CAvailable:0020Bytes のようになります。
- **CounterName** : パフォーマンス カウンターの名前を指定します。 これに対応、 *counterSpecifier* xml 構成で定義されています。
- **最大** : 集計の期間にわたってパフォーマンス カウンターの最大値。
- **最小** : 集計の期間にわたってパフォーマンス カウンターの最小値。
- **合計** : 集計の期間にわたってレポートされているパフォーマンス カウンターのすべての値の合計です。
- **カウント** : パフォーマンス カウンターの値の合計数が報告されます。
- **平均** : 集計の期間にわたってパフォーマンス カウンターの平均値 (合計/数) 値です。


## 次のステップ

- 診断拡張機能で Windows 仮想マシンの完全なサンプル テンプレートを参照してください [201-vm の監視-診断の拡張機能。](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-monitoring-diagnostics-extension)   
- リソース マネージャー テンプレートを使用して、展開 [Azure PowerShell](virtual-machines-deploy-rmtemplates-powershell.md) または [Azure コマンド ライン](virtual-machines-deploy-rmtemplates-powershell.md)
- 詳細について [Azure リソース マネージャーのテンプレートの作成](resource-group-authoring-templates.md)








