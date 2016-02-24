<properties
   pageTitle="Microsoft Azure のセキュリティと監査ログの管理 | Microsoft Azure"
   description="Azure でホストされているサービスからのセキュリティ ログの生成、収集、および分析の概要について説明します。  組織のセキュリティおよびコンプライアンス担当者など、日常的に情報資産の管理に携わる IT プロフェッショナルとセキュリティ アナリストを対象とした内容です。"
   services="virtual-machines, cloud-services, storage"
   documentationCenter="na"
   authors="nayak-mahesh"
   manager="msStevenPo"
   editor=""/>

<tags
   ms.service="azure-security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/10/2015"
   ms.author="mnayak;tomsh;terrylan"/>

# Microsoft Azure のセキュリティと監査ログの管理

Azure を使用すると、セキュリティ イベントを生成し、Azure Infrastructure as a Service (IaaS) および Platform as a Service (PaaS) ロールからサブスクリプションの中央ストレージに収集できます。 ユーザーは、使用して [HDInsight](http://azure.microsoft.com/documentation/services/hdinsight/) を集計し、収集したイベントを分析します。 さらに、これらの収集されたイベントをオンプレミスのセキュリティ情報およびイベント管理 (SIEM) システムにエクスポートして継続的に監視できます。

Azure のセキュリティ ログ、分析、および監視のライフサイクルには次のものが含まれます。

- **生成**: イベントを発生させるには、アプリケーションとインフラストラクチャをインストルメント化
- **コレクション**: ストレージ アカウント内のさまざまなセキュリティ ログを収集する Azure の構成
- **分析**: を使用して Azure HDInsight などのツールと内部設置型 SIEM システム ログを分析し、セキュリティの詳細情報を生成するには
- **監視とレポート**: Azure では、タイムリーなアラートを継続的なビジュアル化および一元化された監視と分析のシステム

この記事では、ライフサイクルの生成および収集フェーズについて説明します。

## ログの生成
セキュリティ イベントが発生するは、[Windows イベント ログを **システム**, 、**セキュリティ**, 、および **アプリケーション** 仮想マシン内のチャネル。 データが失われることなくイベントがログに記録されるようにするには、イベント ログのサイズを適切に構成することが重要です。 イベント ログのサイズは、監査ポリシーの設定が生成するイベントの数と、定義されているイベント収集ポリシーに基づいて決定します。 詳細については、次を参照してください。 [セキュリティ監査の監視と管理を計画する](http://technet.microsoft.com/library/ee513968.aspx#BKMK_4)です。

>[AZURE.NOTE] Windows イベントの転送 (WEF) または Azure 診断を使用する場合 (で説明されている、 [ログの収集](#log-collection) セクション) クラウド サービスや仮想マシンからログをプルするためのシステムの障害の潜在的な影響を検討してください。 たとえば、WEF 環境がしばらくダウンした場合は、時間が長くなっても大丈夫なようにログのサイズが十分に大きいこと、またはログ データ喪失の可能性に備えられていることを、確認する必要があります。

Azure およびから作成された仮想マシンで展開されているクラウド サービス アプリケーションに対して、 [Azure Virtual Machines Marketplace](http://azure.microsoft.com/marketplace/virtual-machines/#microsoft), 、オペレーティング システムのセキュリティ イベントのセットが既定で有効にします。 ユーザーは、オペレーティング システムの監査ポリシーをカスタマイズすることによって、監査対象のイベントを追加、削除、または変更できます。 詳細については、次を参照してください。 [セキュリティ ポリシー設定のリファレンス](http://technet.microsoft.com/library/jj852210.aspx)します。

次の方法を使用して、オペレーティング システム (監査ポリシーの変更など) および Windows コンポーネント (IIS など) から追加のログを生成できます。

- ドメインに参加している Azure 内の仮想マシンにポリシー設定をロールアウトするためのグループ ポリシー
- ポリシー設定をプッシュおよび管理するための Desired State Configuration (DSC)。 詳細については、次を参照してください [Azure PowerShell DSC。](http://blogs.msdn.com/b/powershell/archive/2014/08/07/introducing-the-azure-powershell-dsc-desired-state-configuration-extension.aspx)
- Cloud Services に設定をロールアウトするためのサービス デプロイメント ロール スタートアップ コード (PaaS のシナリオ)

Azure ロールのスタートアップ タスクを構成すると、ロールが開始する前にコードを実行できます。 追加することで、ロールのスタートアップ タスクを定義する、 **スタートアップ** 次の例で示すように、サービス定義ファイル内のロールの定義に要素。 詳細については、次を参照してください。 [Azure におけるスタートアップ タスクの実行](http://msdn.microsoft.com/library/azure/hh180155.aspx)します。

スタートアップ タスクとして実行するタスク ファイル (以下の例では EnableLogOnAudit.cmd) をビルド パッケージに含める必要があります。 Visual Studio を使用している場合は、ファイルをクラウド プロジェクトに追加をファイル名を右クリックして **プロパティ**, 、し設定 **出力ディレクトリにコピーする** に **常にコピー**します。

    <Startup>
        <Task commandLine="EnableLogOnAudit.cmd" executionContext="elevated" taskType="simple" />
    </Startup>

EnableLogOnAudit.cmd の内容:

    @echo off
    auditpol.exe /set /category:"Logon/Logoff" /success:enable /failure:enable
    Exit /B 0

[Auditpol.exe](https://technet.microsoft.com/library/cc731451.aspx) 監査ポリシーの設定を管理することができる Windows Server オペレーティング システムに含まれるコマンド ライン ツールは、前の例で使用します。

Windows イベント ログを生成するだけでなく、セキュリティ分析および監視に重要なログを生成するように、Windows オペレーティング システムのさまざまなコンポーネントを構成できます。 たとえば、インターネット インフォメーション サービス (IIS) のログと http.err ログは Web ロールに対して自動的に生成され、収集対象として構成できます。 これらのログは、承認されていないアクセスまたは Web ロールに対する攻撃を識別するために使用できる貴重な情報を提供します。 詳細については、次を参照してください。 [IIS のログを構成する](http://technet.microsoft.com/library/hh831775.aspx) と [IIS – カスタム ログのログ出力の高度な](http://www.iis.net/learn/extensions/advanced-logging-module/advanced-logging-for-iis-custom-logging)です。

Web ロールのサービス定義ファイルにスタートアップ タスクを追加して、Web ロールの IIS ログを変更できます。 次の例では、Contoso という名前の Web サイトの HTTP ログを有効にし、IIS が Contoso Web サイトに対するすべての要求をログに記録するよう指定しています。

IIS の構成を更新するタスクを、Web ロールのサービス定義ファイルに含める必要があります。 サービス定義ファイルに対する次の変更は、ConfigureIISLogging.cmd という名前のスクリプトを実行することによって IIS ログを構成するスタートアップ タスクを実行します。

    <Startup>
        <Task commandLine="ConfigureIISLogging.cmd" executionContext="elevated" taskType="simple" />
    </Startup>

ConfigureIISLogging.cmd の内容

    @echo off
    appcmd.exe set config "Contoso" -section:system.webServer/httpLogging /dontLog:"True" /commit:apphost
    appcmd.exe set config "Contoso" -section:system.webServer/httpLogging /selectiveLogging:"LogAll" /commit
    Exit /B 0


## <a name="log-collection"></a>ログ コレクション
Azure の Cloud Services または Virtual Machines からのセキュリティ イベントおよびログの収集は、2 つの主要な方法で行われます。

- Azure 診断は、顧客の Azure ストレージ アカウントでのイベントを収集します。
- Windows イベント転送 (WEF) は、Windows を実行しているコンピューターでのテクノロジです。

これら 2 つのテクノロジの主な相違点を次の表にまとめます。 要件およびこれらの主な違いに基づき、適切な方法を選択してログ収集を実装する必要があります。

| Azure 診断 | Windows イベント転送 |
|-----|-----|
|Azure Virtual Machines と Azure Cloud Services をサポートします | ドメインに参加している Azure Virtual Machines のみサポートします |
|Windows イベント ログなど、ログ ファイル形式をサポートしている [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) トレース、および IIS ログに記録します。 詳細については、次を参照してください [Azure 診断がサポートされるデータ ソース。](#diagnostics) |Windows イベント ログのみをサポートします |
|収集されたデータを Azure Storage にプッシュします |収集されたデータを中央のコレクター サーバーに移動します |

##  Windows イベント転送でのセキュリティ イベント データ収集
ドメインに参加している Azure Virtual Machines の場合、オンプレミスのドメイン参加コンピューターと同じ方法でグループ ポリシー設定を使用して WEF を構成できます。 詳細については、次を参照してください。 [ハイブリッド クラウド](http://www.microsoft.com/server-cloud/solutions/hybrid-cloud.aspx)します。

このアプローチを使用して、組織が IaaS サブスクリプションの購入を使用して、企業ネットワークに接続 [ExpressRoute](http://azure.microsoft.com/services/expressroute/) またはサイト対サイト VPN し、会社のドメインにある Azure で仮想マシンに参加します。 その後は、ドメインに参加しているコンピューターから WEF を構成できます。

イベント転送は、ソースとコレクターの 2 つの部分に分かれます。 ソースは、セキュリティ ログを生成するコンピューターです。 コレクターは、イベント ログを収集して統合する中央のサーバーです。 IT 管理者はイベントをサブスクライブすることで、リモート コンピューター (イベント ソース) から転送されるイベントを受信して格納できます。 詳細については、次を参照してください。 [に転送し、イベントの収集には、[コンピューターの構成](http://technet.microsoft.com/library/cc748890.aspx)します。

収集された Windows イベントは、さらに詳しい分析のために SIEM などのオンプレミスの分析ツールに送信できます。

## Azure 診断でのセキュリティ データの収集
Azure 診断を使用すると、Azure で実行しているクラウド サービスの worker ロールや Web ロールまたは仮想マシンから、診断データを収集できます。 Azure 診断は定義済みのゲスト エージェント拡張機能であり、データを収集するには有効化して構成する必要があります。 ユーザーのサブスクリプションには、Azure Storage へのデータのプッシュを含めることができます。

データは暗号化されて転送されます (HTTPS を使用)。 このドキュメントの例では、Azure 診断 1.2 を使用しています。 セキュリティ データを収集するにはバージョン 1.2 以降にアップグレードすることをお勧めします。 詳細については、次を参照してください。 [Azure 診断を使用してログ データの収集](http://msdn.microsoft.com/library/gg433048.aspx)します。

次の図では、Azure 診断を使用したデータ収集とその後の分析および監視に関するデータ フローの概要を示します。

![][1]

Azure 診断は、お客様のクラウド サービス アプリケーションからログを移動し、 [Azure の仮想マシン](virtual-machines-about.md) Azure ストレージにします。 ログの形式に基づき、データは Azure テーブルまたは BLOB に格納されます。 収集されたデータを [Azure Storage](storage-introduction.md) 監視と分析の Azure ストレージ クライアント ライブラリを使用して、内部設置型 SIEM システムにダウンロードできます。

また、HDInsight を使用するとクラウド内のデータをさらに分析できます。 Azure 診断を使用するセキュリティ データ収集の例を次に示します。

### Azure 診断を使用した Azure Virtual Machines からのセキュリティ データ収集

次の例では、Azure 診断 1.2 と Azure PowerShell コマンドレットを使用して、仮想マシンからセキュリティ データを収集できるようにしています。 データは、仮想マシンからスケジュールされた (構成可能な) 間隔で収集されて、ユーザーのサブスクリプション内の Azure Storage にプッシュされます。
このセクションでは、Azure 診断を使用する 2 つのログ収集シナリオについて説明します。

1. 仮想マシンにセキュリティ ログ収集パイプラインの新しいインスタンスを設定します。
2. 仮想マシンの既存のセキュリティ ログ収集パイプラインを新しい構成で更新します。

#### 仮想マシンにセキュリティ ログ収集パイプラインの新しいインスタンスを設定する
この例では、Azure 診断を使用するセキュリティ ログ収集パイプラインの新しいインスタンスを設定し、仮想マシンからログオン失敗イベント (イベント ID 4624 と 4625) を検出します。 開発環境から次の手順を実装することも、リモート デスクトップ プロトコル (RDP) 経由でクラウド内のノードへのリモート デスクトップ セッションを使用することもできます。

##### ステップ 1: Azure PowerShell SDK をインストールする
Azure PowerShell SDK は、Azure Virtual Machines に Azure 診断を構成するためのコマンドレットを提供します。 必要なコマンドレットは、Azure PowerShell バージョン 0.8.7 以降で使用できます。 詳細については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](powershell-install-configure.md)します。

##### ステップ 2: 構成ファイルを準備する
収集するイベントに基づいて構成ファイルを準備します。 Windows イベントの収集対象の Azure 診断構成ファイルの例を次に示します、 **セキュリティ** だけログオン成功と失敗イベントの収集に追加されるフィルターのチャネル。 詳細については、次を参照してください。 [Azure 診断 1.2 構成スキーマ](http://msdn.microsoft.com/library/azure/dn782207.aspx)します。

ストレージ アカウントは、構成ファイルで指定することも、Azure PowerShell コマンドレットを実行して Azure 診断をセットアップするときにパラメーターとして指定することもできます。

    <?xml version="1.0" encoding="utf-8" ?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
        <WadCfg>
            <DiagnosticMonitorConfiguration overallQuotaInMB="10000">
                <WindowsEventLog scheduledTransferPeriod="PT1M">
                    <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
                </WindowsEventLog>
            </DiagnosticMonitorConfiguration>
        </WadCfg>
    </PublicConfig>

以前の内容を XML ファイルとして保存するときにエンコーディングを設定 **utf-8**します。 メモ帳を使用している場合は、[名前を付けて保存] ダイアログ ボックスに使用可能なエンコード オプションが表示されます。 次の表では、構成ファイルの重要な属性を示します。

| 属性 | 説明 |
|----- |-----|
| overallQuotaInMB | Azure 診断で使用できるローカル ディスク領域の最大量 (値は構成可能)。 |
| scheduledTransferPeriod | Azure Storage への転送のスケジュール間隔。最も近い分単位に切り上げられます。 |
| Name | WindowsEventLog では、この属性は収集する Windows イベントを記述する XPath クエリです。 イベント ID、プロバイダー名、チャネルなどのフィルターを追加することで、データの収集をフィルター処理できます。 |

すべての Windows イベント ログ データはという名前のテーブルに移動 **WADWindowsEventLogsTable**します。 現時点では、Azure 診断はテーブル名の変更をサポートしていません。

##### <a name="step3"></a> ステップ 3: 構成 XML ファイルを検証する
構成 XML ファイルにエラーがないこと、および Azure 診断スキーマと互換性があることを検証するには、次の手順を使用します。

1. スキーマ ファイルをダウンロードするには、次のコマンドを実行し、ファイルを保存します。

    (Get-azureserviceavailableextension-extensionname 'PaaSDiagnostics' - ProviderNamespace 'Microsoft.Azure.Diagnostics')。PublicConfigurationSchema |Out-file-utf8 - FilePath 'WadConfigSchema.xsd' エンコーディング

2. スキーマ ファイルをダウンロードした後は、スキーマに対して構成 XML ファイルを検証することができます。 Visual Studio を使用してファイルを検証するには:
  - Visual Studio で XML ファイルを開きます。
  - F4 キーを開くには **プロパティ**
  - クリックして **スキーマ**, 、] をクリックして **追加**, (WadConfigSchema.XSD) をダウンロードしたスキーマ ファイルを選択して、をクリックし、 **OK**

3.   **ビュー** ] メニューのをクリックして **エラー一覧** 検証エラーがあるかどうかを確認します。

##### <a name="step4"></a> ステップ 4: Azure 診断を構成する
 次の手順を使用して、Azure 診断を有効にし、データの収集を開始します。

 1. Azure PowerShell を開くには入力 **Add-azureaccount**, 、ENTER キーを押します。
 2. Azure のアカウントを使用してサインインします。
 3. 次の PowerShell スクリプトを実行します。 storage_name、key、config_path、service_name、vm_name を忘れずに更新してください。

 ```PowerShell
$storage_name ="<Storage Name>"
$key = "<Storage Key>"
$config_path="<Path Of WAD Config XML>"
$service_name="<Service Name. Usually it is same as VM Name>"
$vm_name="<VM Name>"
$storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
$VM1 = Get-AzureVM -ServiceName $service_name -Name $vm_name
$VM2 = Set-AzureVMDiagnosticsExtension -DiagnosticsConfigurationPath $config_path -Version "1.*" -VM $VM1 -StorageContext $storageContext
$VM3 = Update-AzureVM -ServiceName $service_name -Name $vm_name -VM $VM2.VM
 ```

##### ステップ 5: イベントを生成する
説明のため、ログオン イベントをいくつか作成し、データが Azure Storage に転送されることを確認します。 手順 2. で上記のように、XML ファイルからの構成を収集し、イベント ID 4624 (ログオン成功) イベント ID 4625 (ログオンの失敗)、 **セキュリティ** チャネル。

 これらのイベントを生成するには:

1.  仮想マシンに対する RDP セッションを開きます。
2.  正しくない資格情報を入力し、ログオン失敗イベント (イベント ID 4625) を生成します。
3.  ログオンを何回か失敗した後、正しい資格情報を入力してログオン成功イベント (イベント ID 4624) を生成します。

##### ステップ 6: データを表示する
前の手順を完了してから約 5 分後には、XML ファイルの構成に基づいてユーザーのストレージ アカウントへのデータの転送が開始します。 複数のツールを使用して Azure Storage のデータを表示できます。 詳細については、次を参照してください。

- [サーバー エクスプローラーを使用したストレージ リソースの参照](http://msdn.microsoft.com/library/azure/ff683677.aspx)
- [Azure Storage Explorer 6 Preview 3 (August 2014)](http://azurestorageexplorer.codeplex.com/)

データを表示するには:

1.  (2013年、2012年および 2010 SP1) の Visual Studio で [ **ビュー**, 、クリックして **サーバー エクスプ ローラー**します。
2.  ストレージ アカウントに移動します。
3.  クリックして **テーブル** し、仮想マシンから収集されたセキュリティ ログを表示する適切なテーブルをダブルクリックします。
![][2]

4.  WADWindowsEventLogsTable をという名前のテーブルを右クリックし、クリックして **データの表示** を次に示すようにテーブルのビューを開きます。

![][3]

前のストレージ テーブルで **PartitionKey**, 、**RowKey**, 、および **タイムスタンプ** システム プロパティです。

- **PartitionKey** タイムスタンプ (秒単位) があり、テーブル内のパーティションの一意の識別子。
- **RowKey** パーティション内のエンティティの一意な識別子を指定します。

同時に、 **PartitionKey** と **RowKey** テーブル内の各エンティティを一意に識別します。

- タイムスタンプは日付/時刻の値であり、エンティティが最後に変更されたときを追跡するためにサーバー上に保持されます。

>[AZURE.NOTE] Azure のストレージ テーブルの最大行サイズは、1 MB に制限されています。 アカウントが 2012 年 6 月より後に作成された場合、ストレージ アカウントは BLOB、キュー、およびテーブルからのデータを最大 200 TB まで格納できます。 そのため、BLOB およびキューがストレージ領域を使用しない場合、テーブルのサイズは最大 200 TB まで拡大できます。 2012 年 6 月以前に作成されたアカウントには、100 TB の制限があります。

Storage Explorer ではテーブルのデータを編集することもできます。 テーブル ビューの特定の行をダブルクリックして、次に示すような [エンティティの編集] ウィンドウを開きます。

![][4]

#### 仮想マシンの既存のセキュリティ ログ収集パイプラインを新しい構成で更新する
このセクションでは、仮想マシンの既存の Azure 診断セキュリティ ログ収集パイプラインを更新し、Windows アプリケーション イベント ログのエラーを検出します。

##### ステップ 1: 対象のイベントを含むように構成ファイルを更新する
前の例で作成した Azure 診断ファイルを、Windows アプリケーション イベント ログ エラーの種類を含むように更新する必要があります。

>[AZURE.NOTE] 既存の Azure 診断の構成設定は、新しい構成ファイルにマージする必要があります。 新しいファイルで定義された設定により、既存の構成は上書きされます。

既存の構成設定を取得するには、使用することができます、 **Get-azurevmdiagnosticsextension** コマンドレットです。 既存の構成を取得するサンプルの Azure PowerShell スクリプトを次に示します。

    $service_name="<VM Name>"
    $VM1 = Get-AzureVM -ServiceName $service_name
    $config = Get-AzureVMDiagnosticsExtension -VM $VM1 | Select -Expand PublicConfiguration | % {$_.substring($_.IndexOf(':"')+2,$_.LastIndexOf('",')-$_.IndexOf(':"')-2)}
    [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($config))
次のように、Windows アプリケーション イベント ログ エラーおよび重大なイベントを収集するように、Azure 診断の構成を更新します。

    <?xml version="1.0" encoding="utf-8" ?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
        <DiagnosticMonitorConfiguration overallQuotaInMB="10000">
            <WindowsEventLog scheduledTransferPeriod="PT1M">
                <DataSource name="Application!*[System[(Level =2)]]" />
                <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
            </WindowsEventLog>
        </DiagnosticMonitorConfiguration>
    </WadCfg>
    </PublicConfig>

前に示したように、同じ手順を使用して、構成ファイルの検証 [手順 3: 構成 XML ファイルを検証](#step3)します。

##### ステップ 2: 新しい構成ファイルを使用するように Azure 診断を更新する
使用して、 **Set-azurevmdiagnosticsextension** と **Update-azurevm** の前に示したように構成を更新するコマンドレット [手順 4: Azure 診断の構成](#step4)します。

##### ステップ 3: 構成設定を確認する
次のコマンドを実行して、構成設定が更新されたことを確認します。

    $service_name="<VM Name>"
    $VM1 = Get-AzureVM -ServiceName $service_name
    Get-AzureVMDiagnosticsExtension -VM $VM1

##### ステップ 4: イベントを生成する
この例では、型のアプリケーション イベント ログを生成するには、次のコマンドを実行 **エラー**:

    eventcreate /t error /id 100 /l application /d "Create event in application log for Demo Purpose"

![][5]

イベント ビューアーを開き、イベントが作成されたことを確認します。

![][6]

##### ステップ 5: データを表示する
Visual Studio でサーバー エクスプローラーを開き、ログ データを表示します。 確認する必要があります、 **イベント Id 100** 上に作成 **ContosoDesktop** 次のようにします。

![][7]

## Azure 診断を使用した Azure Cloud Services からのセキュリティ データ収集

次に、Azure 診断を使用して、前の Virtual Machines (IaaS) セクションと同じ 2 つのログ収集シナリオを今度は Azure Cloud Services から行う場合について説明します。

1.  クラウド サービスでセキュリティ ログ パイプラインの新しいインスタンスを設定します。
2.  クラウド サービスの既存のログ収集パイプラインを新しい構成で更新します。

このセクションの手順は次のとおりです。

1.  クラウド サービスを作成します。
2.  Azure 診断を使用したセキュリティ ログ収集用にクラウド サービスを構成します。
3.  クラウド サービスでセキュリティ イベントの生成と収集を実演します。

    - 特権が昇格されたローカル グループに管理者を追加する
    - 新しいプロセスを作成する
4.  クラウド サービスの既存のログ収集パイプラインを更新します。

    - Auditpol を使用して (ネットワーク セキュリティ イベントの例として) ホスト ファイアウォール イベントの監査を有効にします。
    - 収集するファイアウォール監査データを構成し、ユーザーのストレージ アカウントに収集されたイベントを表示します。
5.  Windows セキュリティ イベントの分布とスパイクの検出を示します。
6.  IIS ログの収集を構成し、データを確認します。

すべてのイベントとログは、Azure のユーザーのストレージ アカウントに収集されます。 ユーザーは、イベントを表示し、オンプレミスの SIEM システムにエクスポートできます。 また、HDInsight を使用して集計および分析することもできます。

### クラウド サービスにログ収集パイプラインの新しいインスタンスを設定する
この例では、Azure 診断を使用するセキュリティ ログ収集パイプラインの新しいインスタンスをセットアップし、ローカル グループへのユーザーの追加およびクラウド サービス インスタンスでの新規プロセス作成イベントを検出します。

#### ステップ 1: クラウド サービス (Web ロール) を作成してデプロイする

1.  開発コンピューター上で Visual Studio 2013 を起動します。
2.  新しいクラウド サービス プロジェクトを作成します (この例では ContosoWebRole を使用します)。
3.  選択、 **ASP.NET** web ロール。
4.  選択、 **MVC** プロジェクトです。
5.  ソリューション エクスプ ローラーでクリックして **ロール**, 、web ロールを開くには、(WebRole1) をダブルクリック、 **プロパティ** ウィンドウです。
6.   **構成** タブで、[、 **診断を有効にする** を Visual Studio 2013 が付属している Azure 診断のバージョンを無効にする] チェック ボックスです。
![][8]

7.  ソリューションを構築してエラーが発生しないことを確認します。
8.  WebRole1/Controllers/HomeController.cs ファイルを開きます。
9.  次のメソッドを追加し、サンプルの IIS ログ イベントとして HTTP ステータス コード 500 をサンプル アプリケーションがログに記録できるようにします (これを後の IIS の例で使用します)。

    ```
    public ActionResult StatusCode500()
        {
            throw new InvalidOperationException("Response.StatusCode is 500");
        }
    ```

10.  クラウド サービス プロジェクトの名前を右クリックし、クリックして **発行**します。

#### ステップ 2: 構成ファイルを準備する
次の状況の検出に役立つイベントを追加するように Azure 診断構成ファイルを準備します。

- ローカル グループに新しいユーザーを追加する
- 新しいプロセスを作成する

```
<?xml version="1.0" encoding="UTF-8"?>
<PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
        <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
            <WindowsEventLog scheduledTransferPeriod="PT1M">
                <DataSource name="Security!*[System[(EventID=4732 or EventID=4733 or EventID=4688)]]" />
            </WindowsEventLog>
        </DiagnosticMonitorConfiguration>
    </WadCfg>
</PublicConfig>
```

#### ステップ 3: 構成 XML ファイルを検証する
前に示したように、同じ手順を使用して、構成ファイルの検証 [手順 3: 構成 XML ファイルを検証](#step3)します。
 
#### ステップ 4: Azure 診断を構成する
次の Azure PowerShell スクリプトを実行して、Azure 診断を有効にします (storage_name、key、config_path、service_name を忘れずに構成してください)。

    $storage_name = "<storage account name>"
    $key = " <storage key>"
    $config_path="<path to configuration XML file>"
    $service_name="<Cloud Service Name>"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name

サービスの診断構成が最新であることを確認するには、次の Azure PowerShell コマンドを実行します。

    Get-AzureServiceDiagnosticsExtension -ServiceName <ServiceName>

#### ステップ 5: イベントを生成する
イベントを生成するには:

1.  クラウド サービス インスタンスへのリモート デスクトップ セッションを開始するには、Visual Studio でサーバー エクスプローラーを開き、ロールのインスタンスを右クリックして、[リモート デスクトップを使用して接続] をクリックします。
2.  管理者特権のコマンド プロンプトを開き、次のコマンドを実行して、仮想マシンにローカル管理者アカウントを作成します。


    net user contosoadmin  <enterpassword> /add
    net localgroup administrators contosoadmin  /add

3.  イベント ビューアーを開く、開く、 **セキュリティ** チャネル、および次のようにイベント 4732 が作成されたことに注意してください。

![][9]

#### ステップ 6: データを表示する
Azure 診断エージェントがストレージ テーブルにイベントをプッシュするまで、約 5 分間待ちます。

![][10]

プロセス作成イベントを確認するには、メモ帳を開きます。 次のように、プロセス作成イベントが Security チャネルに記録されています。

![][11]

次に示すように、ストレージ アカウントでも同じイベントを表示できます。

![][12]

### クラウド サービスの既存のログ収集パイプラインを新しい構成で更新する
このセクションでは、既存の Azure 診断セキュリティ ログ収集パイプラインを更新し、クラウド サービス インスタンスでの Windows ファイアウォール変更イベントを検出します。

ファイアウォールの変更を検出するため、ファイアウォール変更イベントを含むように既存の構成を更新します。

#### ステップ 1: 既存の構成を取得する
>[AZURE.NOTE] 新しい構成設定は、既存の構成が上書きされます。 したがって、既存の Azure 診断の構成設定を新しい構成ファイルとマージすることが重要です。

既存の構成設定を取得するには、使用することができます、 **Get AzureServiceDiagnosticsExtension** コマンドレット。

    Get-AzureServiceDiagnosticsExtension -ServiceName <ServiceName>

#### ステップ 2: ファイアウォール イベントを含むように構成 XML を構成する

    <?xml version="1.0" encoding="UTF-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
        <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
        <WindowsEventLog scheduledTransferPeriod="PT1M">
            <DataSource name="Security!*[System[(EventID=4732 or EventID=4733 or EventID=4688)]]" />
            <DataSource name="Security!*[System[(EventID &gt;= 4944 and EventID &lt;= 4958)]]" />
        </WindowsEventLog>
        </DiagnosticMonitorConfiguration>
    </WadCfg>
    </PublicConfig>

前述のように同じ検証プロセスを使用して XML コンテンツを検証 [手順 3: 構成 XML ファイルを検証](#step3)します。

#### ステップ 3: 新しい構成を使用するように Azure 診断を更新する

次の Azure PowerShell スクリプトを実行し、新しい構成を使用するように Azure 診断を構成します (storage_name、key、config_path、service_name を実際のクラウド サービスの情報に置き換えます)。

    Remove-AzureServiceDiagnosticsExtension -ServiceName <ServiceName> -Role <RoleName>
    $storage_name = "<storage account name>"
    $key = " <storage key>"
    $config_path="<path to configuration XML file>"
    $service_name="<Cloud Service Name>"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name

サービスの診断構成が最新であることを確認するには、次の Azure PowerShell コマンドを実行します。

    Get-AzureServiceDiagnosticsExtension -ServiceName <ServiceName>

#### ステップ 4: ファイアウォール イベントを有効にする

1.  クラウド サービス インスタンスへのリモート デスクトップ セッションを開きます。
2.  管理者特権のコマンド プロンプトを開き、次のコマンドを実行します。

    ```
    auditpol.exe /set /category:"Policy Change" /subcategory:"MPSSVC rule-level Policy Change" /success:enable /failure:enable
    ```

#### ステップ 5: イベントを生成する

1.  Windows ファイアウォールを開き、をクリックして **受信の規則**します。
2.  クリックして **新しいルールの追加**, 、] をクリックし、 **ポート**します。
3.   **のローカル ポート** フィールドに「 **5000**, 、] をクリックし、 **次** 3 回です。
4.   **名前** フィールドに「 **Test5000**, 、] をクリックし、 **完了**します。
5.  イベント ビューアーを開く、開く、 **セキュリティ** チャネル、および次のようにイベント ID 4946 が作成されたことに注意してください。

![][13]

#### ステップ 6: データを表示する
Azure 診断エージェントがストレージ テーブルにイベント データをプッシュするまで、約 5 分間待ちます。

![][14]

### セキュリティ イベントの分布とスパイクの検出
イベントがユーザーのストレージ アカウントに転送された後、アプリケーションはストレージ クライアント ライブラリを使用してイベントにアクセスし、イベントの集計を実行できます。 テーブル データにアクセスするサンプル コードを参照してください。 [方法: テーブル データの取得](storage-dotnet-how-to-use-tables.md)します。

イベント集計の例を以下に示します。 イベント分布のスパイクで異常なアクティビティについて詳細に調査できます。

![][15]

### HDInsight を使用した IIS ログの収集と処理
このセクションでは、Web ロールのインスタンスから IIS ログを収集し、ユーザーのストレージ アカウントの Azure BLOB にログを移動します。

#### ステップ 1: IIS ログの主集を含むように構成ファイルを更新する

    <?xml version="1.0" encoding="UTF-8"?>
    <PublicConfig xmlns="http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration">
    <WadCfg>
        <DiagnosticMonitorConfiguration overallQuotaInMB="25000">
        <Directories scheduledTransferPeriod="PT5M">
        <IISLogs containerName="iislogs" />
        </Directories>
        <WindowsEventLog scheduledTransferPeriod="PT1M">
            <DataSource name="Security!*[System[(EventID=4732 or EventID=4733 or EventID=4688)]]" />
            <DataSource name="Security!*[System[(EventID &gt;= 4944 and EventID &lt;= 4958)]]" />
        </WindowsEventLog>
        </DiagnosticMonitorConfiguration>
    </WadCfg>
    </PublicConfig>

以前の Azure 診断の構成で **containerName** お客様のストレージ アカウント内で、ログを移動先となる blob コンテナー名です。

前に示したように、同じ手順を使用して、構成ファイルの検証 [手順 3: 構成 XML ファイルを検証](#step3)します。


#### ステップ 2: 新しい構成を使用するように Azure 診断を更新する
次の Azure PowerShell スクリプトを実行し、新しい構成を使用するように Azure 診断を構成します (storage_name、key、config_path、service_name を実際のクラウド サービスの情報に置き換えます)。

    Remove-AzureServiceDiagnosticsExtension -ServiceName <ServiceName> -Role <RoleName>
    $storage_name = "<storage account name>"
    $key = " <storage key>"
    $config_path="<path to configuration XML file>"
    $service_name="<Cloud Service Name>"
    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name

サービスの診断構成が最新であることを確認するには、次の Azure PowerShell コマンドを実行します。

    Get-AzureServiceDiagnosticsExtension -ServiceName <ServiceName>

#### ステップ 3: IIS ログを生成する

1.  Web ブラウザーを開き、クラウド サービス web ロール (たとえば、http://contosowebrole.cloudapp.net/) に移動します。
2.  移動、 **に関する** と **連絡先** をいくらか作成するページは、イベントを記録します。
3.  状態コード 500 (http://contosowebrole.cloudapp.net/Home/StatusCode500 など) を生成するページに移動します。
次のようなエラーが表示されます。 コードが追加されているに注意してください **StatusCode500** ログ コレクション パイプラインで ServiceName はクラウド上の新しいインスタンスを設定セクションの手順 1 でします。
![][16]
4.  クラウド サービス インスタンスへのリモート デスクトップ セッションを開きます。
5.  IIS マネージャーを開きます。
6.  IIS ログは既定で有効になり、すべてのフィールドを含む W3C 形式のファイルを 1 時間ごとに生成するように設定されます。 クリックして **参照**, とが少なくとも 1 つのログ ファイルでは、次のようにします。
![][17]

7.  Azure 診断エージェントがログ ファイルを BLOB コンテナーにプッシュするまで、約 5 分間待ちます。 データを検証するには、開く **サーバー エクスプ ローラー** > **ストレージ** > **ストレージ アカウント** > **Blob**します。 ここでは、blob を示すように **iislogs** を作成します。
![][18]

8.  右クリックし、選択 **[Blob コンテナーの** blob に格納されている IIS ログ ファイルを表示します。
![][19]
9.  IIS イベントがユーザーのストレージ アカウントに格納された後、HDInsight の分析を利用するアプリケーションを使用してイベントの集計を実行できます。 次の折れ線グラフは、HTTP ステータス コード 500 を表示するイベントの集計タスクの例を示します。
![][20]

## セキュリティ ログの収集に関する推奨事項
セキュリティ ログを収集するときは、次のことをお勧めします。

- 独自のサービス アプリケーション固有の監査ログ イベントを収集します。
- 分析と監視に必要なデータのみを構成します。 キャプチャするデータが多すぎると、トラブルシューティングが困難になり、サービスまたはストレージのコストに影響を与えることがあります。
- 既存の Azure 診断の構成設定と行う変更をマージします。 既存の構成設定は、新しい構成ファイルで上書きされます。
- 選択、 **スケジュールされた転送期間** 間隔賢いです。 転送間隔が短いとデータの関連性は増えますが、ストレージのコストと処理のオーバーヘッドが増加します。

>[AZURE.NOTE] 収集されるデータ量に大きく影響するが、他の変数は、ログ記録レベルです。 ログ レベルでログをフィルター処理する方法の例を次に示します。

    System!*[System[(Level =2)]]

ログ レベルは累積されます。 フィルターが設定されている場合 **警告**, 、し **エラー** と **重大** イベントは収集されます。

- 不要になった診断データを Azure Storage から定期的に消去します。

>[AZURE.NOTE] についての詳細については、診断データを参照してください [ストアと Azure Storage で診断データを表示](https://msdn.microsoft.com/library/azure/hh411534.aspx)します。  診断データを格納するコンテナーとテーブルは、他のコンテナーやテーブルと同じであり、他のデータの場合と同じように BLOB やエンティティをそこから削除できます。 1 つのストレージ クライアント ライブラリのまたはを使用して視覚的にプログラムから使用して診断データを削除する、 [ストレージ エクスプ ローラー クライアント](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx)です。

- サービスのデータとセキュリティ ログのデータを異なるストレージ アカウントに格納するのが最善の方法です。 このように分離すると、セキュリティ ログ データの保存が運用サービス データのストレージ パフォーマンスに影響しません。
- 組織のコンプライアンス ポリシーとデータ分析および監視の要件に基づいて、ログの保持期間を選択します。

## 別のシステムへのセキュリティ ログのエクスポート
Azure Storage Client Library を使用して BLOB データをダウンロードした後、オンプレミスのシステムにエクスポートして処理できます。 Blob データを管理するサンプル コードを参照してください。 [.NET から Blob ストレージを使用する方法](storage-dotnet-how-to-use-blobs.md)します。

同様に、Azure Storage Client Library を使用して、Azure のテーブルに格納されているセキュリティ データをダウンロードできます。 テーブルに格納されているデータにアクセスする方法の詳細については、次を参照してください。 [.NET からテーブル ストレージを使用する方法](storage-dotnet-how-to-use-tables.md)します。

## Azure Active Directory のレポート
Azure Active Directory (Azure AD) に含まれるセキュリティ、使用状況、および監査ログのレポートを使用すると、Azure AD テナントの整合性とセキュリティに関する詳細がわかります。 たとえば、Azure AD には、ユーザーのアクティビティを自動的に分析して異常なアクセスを検出し、ユーザーが見ることのできるレポートを作成する機能があります。

これらのレポートは利用、 [Azure 管理ポータル](https://manage.windowsazure.com/) [ **Active Directory** > **ディレクトリ**します。 これらのレポートの一部は無料で、他は Azure AD Premium Edition の一部として提供されます。 Azure AD のレポートの詳細については、次を参照してください。 [アクセスおよび使用状況レポートの表示](http://msdn.microsoft.com/library/azure/dn283934.aspx)します。

## Azure の操作ログ
Azure サブスクリプション リソースに関連する操作は、使用のログを **操作ログ** 管理ポータルで機能します。

表示する、 **操作ログ**, 、開かれている、 [Azure 管理ポータル](https://manage.windowsazure.com/), をクリックして **管理サービス**, 、順にクリック **操作ログ**します。

## <a name="diagnostics"></a> Azure 診断がサポートされるデータ ソース

| データ ソース | 説明 |
|----- | ----- |
| IIS ログ | IIS Websites に関する情報 |
| Azure 診断インフラストラクチャ ログ | Azure 診断に関する情報 |
| IIS の失敗した要求ログ | IIS Web サイトまたはアプリケーションへの失敗した要求に関する情報 |
| Windows イベント ログ | Windows イベント ログ システムに送信された情報 |
| パフォーマンス カウンター | オペレーティング システムとカスタム パフォーマンス カウンター |
| クラッシュ ダンプ | アプリケーションがクラッシュした場合のプロセスの状態に関する情報 |
| カスタム エラー ログ | アプリケーションまたはサービスで作成されたログ |
| .NET EventSource | .NET EventSource クラスを使用してコードで生成されたイベント |
| マニフェスト ベースの ETW | すべてのプロセスによって生成された Windows イベントのイベント トレース |

## その他のリソース
次のリソースでは、Microsoft Azure およびそれに関連する Microsoft サービスに関する一般情報を提供します。

- [Microsoft Azure トラスト センター](http://azure.microsoft.com/support/trust-center/)

    セキュリティとプライバシーが Azure の開発に埋め込まれる方法と、Azure が国際および業界固有の標準のセットを満たす方法について説明します

- [Microsoft Azure のホーム ページ](http://www.microsoft.com/windowsazure/)

    Microsoft Azure に関する一般的な情報とリンク

- [Microsoft Azure ドキュメント センター](http://msdn.microsoft.com/windowsazure/default.aspx)

    Azure のサービスと自動化スクリプトのためのガイダンス

- [Microsoft Security Response Center (MSRC)](http://www.microsoft.com/security/msrc/default.aspx)

    MSRC はパートナーおよび世界中のセキュリティ研究者と協力して、セキュリティ インシデントを防止し、Microsoft 製品のセキュリティを強化します

- [Microsoft Security Response Center の電子メール](mailto:secure@microsoft.com)

    Microsoft Azure などの Microsoft のセキュリティ脆弱性を電子メールで報告します

<!--Image references-->
[1]: ./media/azure-security-audit-log-management/sec-security-data-collection-flow.png
[2]: ./media/azure-security-audit-log-management/sec-storage-table-security-log.PNG
[3]: ./media/azure-security-audit-log-management/sec-wad-windows-event-logs-table.png
[4]: ./media/azure-security-audit-log-management/sec-edit-entity.png
[5]: ./media/azure-security-audit-log-management/sec-app-event-log-cmd.png
[6]: ./media/azure-security-audit-log-management/sec-event-viewer.png
[7]: ./media/azure-security-audit-log-management/sec-event-id100.png
[8]: ./media/azure-security-audit-log-management/sec-diagnostics.png
[9]: ./media/azure-security-audit-log-management/sec-event4732.png
[10]: ./media/azure-security-audit-log-management/sec-step6.png
[11]: ./media/azure-security-audit-log-management/sec-process-creation-event.png
[12]: ./media/azure-security-audit-log-management/sec-process-creation-event-storage.png
[13]: ./media/azure-security-audit-log-management/sec-event4946.png
[14]: ./media/azure-security-audit-log-management/sec-event4946-storage.png
[15]: ./media/azure-security-audit-log-management/sec-event-aggregation.png
[16]: ./media/azure-security-audit-log-management/sec-status-code500.png
[17]: ./media/azure-security-audit-log-management/sec-w3c-format.png
[18]: ./media/azure-security-audit-log-management/sec-blob-iis-logs.png
[19]: ./media/azure-security-audit-log-management/sec-view-blob-container.png
[20]: ./media/azure-security-audit-log-management/sec-hdinsight-analysis.png

