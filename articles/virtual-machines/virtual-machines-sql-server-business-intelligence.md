<properties 
    pageTitle="SQL Server Business Intelligence | Microsoft Azure"
    description="このトピックでは、クラシック デプロイ モデルを使用して作成されたリソースを使用し、Azure 仮想マシン (VM) 上で実行されている SQL Server で使用できる Business Intelligence (BI) 機能について説明します。"
    services="virtual-machines"
    documentationCenter="na"
    authors="rothja"
    manager="jeffreyg"
    editor="monicar" 
    tags="azure-service-management"/>
<tags 
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="12/11/2015"
    ms.author="jroth" />

# Azure Virtual Machines での SQL Server Business Intelligence

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。
 
 
Microsoft Azure 仮想マシン ギャラリーには、SQL Server インストールを含むイメージが用意されています。 ギャラリー イメージでサポートされている SQL Server のエディションは、オンプレミスのコンピューターにも仮想マシンにもインストールできるインストール ファイルです。 このトピックでは、イメージにインストールされている SQL Server Business Intelligence (BI) 機能の概要と、仮想マシンのプロビジョニング後に必要な構成手順について説明します。 また、BI 機能用にサポートされているデプロイ トポロジとベスト プラクティスについても説明します。

## ライセンスに関する考慮事項

Microsoft Azure Virtual Machines の SQL Server のライセンスを取得するには、次の 2 つの方法があります。

1. ソフトウェア アシュアランスの一部であるライセンス モビリティを利用する。 詳細については、次を参照してください。 [Azure でのソフトウェア アシュアランスによるライセンス モビリティ](http://azure.microsoft.com/pricing/license-mobility/)します。

1. SQL Server がインストールされている Azure Virtual Machines の時間単位の料金を支払う。 「SQL Server」のセクションを参照してください [Virtual Machines 料金](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql)します。

ライセンスと現在の料金の詳細については、次を参照してください。 [仮想マシンのライセンス FAQ](http://azure.microsoft.com/pricing/licensing-faq/%20)します。

## Azure 仮想マシン ギャラリーで提供される SQL Server イメージ

Microsoft Azure 仮想マシン ギャラリーには、Microsoft SQL Server を含むイメージがいくつか用意されています。 仮想マシン イメージにインストールされているソフトウェアは、オペレーティング システムのバージョンや SQL Server のバージョンによって異なります。 Azure 仮想マシン ギャラリーで提供されるイメージのリストは頻繁に変更されます。

![Azure VM ギャラリーの SQL イメージ](./media/virtual-machines-sql-server-business-intelligence/IC741367.png)

![PowerShell](./media/virtual-machines-sql-server-business-intelligence/IC660119.gif) 次の PowerShell スクリプトには、"SQL Server"、ImageName を含む Azure のイメージの一覧が返されます。

    # assumes you have already uploaded a management certificate to your Microsoft Azure Subscription. View the thumbprint value from the "settings" menu in Azure classic portal.
    
    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is the one specified with the -ss parameter on MakeCert
    
    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID
    
    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description
    
    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2012"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2012*"} | select imagename,category, location, label, description

SQL Server でサポートされているエディションと機能の詳細については、以下をご覧ください。

- [SQL Server のエディション](https://www.microsoft.com/server-cloud/products/sql-server-editions/#fbid=Zae0-E6r5oh)

- [SQL Server 2014 の各エディションがサポートする機能](https://msdn.microsoft.com/library/cc645993.aspx)

### SQL Server 仮想マシン ギャラリー イメージにインストールされている BI 機能

次の表は、SQL Server の一般的な Microsoft Azure 仮想マシン ギャラリー イメージにインストールされている Business Intelligence 機能を示しています。

- SQL Server 2014 RTM Enterprise

- SQL Server 2014 Standard

- SQL Server 2012 SP2 Enterprise

- SQL Server 2012 SP2 Standard

|SQL Server BI 機能|ギャラリー イメージにインストールされているかどうか|メモ|
|---|---|---|
|**Reporting Services ネイティブ モード**|あり|インストールされていますが、レポート マネージャー URL などの構成が必要です。 セクションを参照して [Reporting Services の構成](#configure-reporting-services)します。|
|**Reporting Services SharePoint モード**|いいえ|Microsoft Azure 仮想マシン ギャラリー イメージには、SharePoint も SharePoint インストール ファイルも含まれていません。 <sup>1</sup>|
|**Analysis Services 多次元およびデータ マイニング (OLAP)**|あり|インストールされ、既定の Analysis Services インスタンスとして構成されています。|
|**Analysis Services 表形式**|いいえ|SQL Server 2012 および 2014 のイメージでサポートされていますが、既定ではインストールされていません。 Analysis Services の別のインスタンスをインストールします。 このトピックの「SQL Server のその他のサービスと機能のインストール」をご覧ください。|
|**Analysis Services Power Pivot for SharePoint**|いいえ|Microsoft Azure 仮想マシン ギャラリー イメージには、SharePoint も SharePoint インストール ファイルも含まれていません。 <sup>1</sup>|

<sup>1</sup> SharePoint と Azure の仮想マシンの詳細については、次を参照してください。 [SharePoint 2013 用の Microsoft Azure アーキテクチャ](https://technet.microsoft.com/library/dn635309.aspx) と [Microsoft Azure Virtual Machines での SharePoint のデプロイ](https://www.microsoft.com/download/details.aspx?id=34598)します。

![PowerShell](./media/virtual-machines-sql-server-business-intelligence/IC660119.gif) サービス名に"SQL"を含むインストールされているサービスの一覧を取得する次の PowerShell コマンドを実行します。

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## 一般的な推奨事項とベスト プラクティス

- 仮想マシンは、最小推奨サイズ **A3** SQL Server Enterprise Edition を使用する場合。  **A4** Analysis Services および Reporting Services の SQL Server BI デプロイの仮想マシンのサイズをお勧めします。

    現在の VM サイズについては、次を参照してください。 [Azure の仮想マシンのサイズ](virtual-machines-size-specs.md)します。

- 以外のデータ、ログ、およびドライブ上のバックアップ ファイルを格納するディスクの管理のベスト プラクティスは、 **C**: と **D**: です。 たとえば、データ ディスクを作成 **E**: と **F**: です。

    - ドライブ キャッシュ ポリシーは既定のドライブの **C**: 操作のデータに最適ではありません。
    
    -  **D**: ドライブは、主にページファイル用に使用される一時ドライブです。  **D**: ドライブは永続化されていないと、blob ストレージには保存されません。 仮想マシンへの変更などの管理タスクのサイズ、 **D**: ドライブです。 ことが推奨 **いない** を使用して、 **D**: tempdb も含め、データベース ファイルのドライブにします。

    作成して、ディスクの接続の詳細については、次を参照してください。 [にデータ ディスクを仮想マシンに接続する方法](storage-windows-attach-disk.md)します。

- 使用する予定がないサービスは、停止またはアンインストールします。 たとえば、仮想マシンを Reporting Services だけに使用する場合は、Analysis Services と SQL Server Integration Services を停止またはアンインストールします。 次の図は、既定で開始されるサービスの例を示しています。

    ![SQL Server サービス](./media/virtual-machines-sql-server-business-intelligence/IC650107.gif)

    >[AZURE.NOTE] サポートされている BI シナリオでは、SQL Server データベース エンジンが必要です。 1 つのサーバー VM のトポロジでは、データベース エンジンが同じ VM 上で実行されている必要があります。

    詳細については、次を参照してください: [Reporting Services のアンインストール](https://msdn.microsoft.com/library/hh479745.aspx) と [Analysis Services のインスタンスをアンインストール](https://msdn.microsoft.com/library/ms143687.aspx)します。

- 確認 **Windows Update** で新しい"更新プログラムが重要な' です。 Microsoft Azure 仮想マシン イメージは頻繁に更新されます。ただし重要な更新プログラムから利用できます **Windows Update** VM イメージが最後に更新された後です。

## デプロイ トポロジの例

Microsoft Azure Virtual Machines を使用するデプロイの例を以下に示します。 これらの図のトポロジは、SQL Server BI 機能と Microsoft Azure Virtual Machines で使用できるトポロジの一部にすぎません。

### 1 つの仮想マシン

1 つの仮想マシン上の Analysis Services、Reporting Services、SQL Server データベース エンジン、データ ソース。

![1 つの仮想マシンを使用する BI IaaS シナリオ](./media/virtual-machines-sql-server-business-intelligence/IC650108.gif)

### 2 つの仮想マシン

- 1 つの仮想マシン上の Analysis Services、Reporting Services、SQL Server データベース エンジン。 このデプロイには、レポート サーバー データベースが含まれます。

- 2 つ目の VM 上のデータ ソース。 2 つ目の VM には、データ ソースとして SQL Server データベース エンジンが含まれます。

![仮想マシン 2 台の bi iaas シナリオ](./media/virtual-machines-sql-server-business-intelligence/IC650109.gif)

### Azure が混在 - Azure SQL Database 上のデータ

- 1 つの仮想マシン上の Analysis Services、Reporting Services、SQL Server データベース エンジン。 このデプロイには、レポート サーバー データベースが含まれます。

- データ ソースは Azure SQL Database です。

![VM とデータ ソースとして Azure SQL を使用する BI IaaS シナリオ](./media/virtual-machines-sql-server-business-intelligence/IC650110.gif)

### ハイブリッド - オンプレミスのデータ

- このデプロイ例では、1 つの仮想マシン上で Analysis Services、Reporting Services、SQL Server データベース エンジンが実行されます。 仮想マシンは、レポート サーバー データベースをホストします。 仮想マシンは、Azure Virtual Networking またはその他の VPN トンネリング ソリューションを使用して、オンプレミス ドメインに参加しています。

- データ ソースはオンプレミスです。

![VM とオンプレミス データ ソースを使用する BI IaaS シナリオ](./media/virtual-machines-sql-server-business-intelligence/IC654384.gif)

## Reporting Services ネイティブ モードの構成

SQL Server の仮想マシン ギャラリー イメージには、Reporting Services ネイティブ モードがインストールされていますが、レポート サーバーは構成されていません。 このセクションの手順では、Reporting Services レポート サーバーを構成します。 Reporting Services ネイティブ モードの構成について詳細を参照してください。 [インストール Reporting Services ネイティブ モード レポート サーバー (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx)します。

>[AZURE.NOTE] レポート サーバーを構成する Windows PowerShell スクリプトを使用する類似のコンテンツを参照してください。 [、Azure VM で、ネイティブ モードのレポート サーバーを作成する PowerShell を使用して](virtual-machines-sql-server-create-native-mode-report-server-powershell.md)します。

### 仮想マシンへの接続と Reporting Services 構成マネージャーの起動

Azure 仮想マシンに接続するための 2 つの一般的なワークフローがあります。

- クリックして、仮想マシンの名前で、接続 **接続**します。 リモート デスクトップ接続が開き、コンピューター名が自動的に設定されます。

    ![connect to azure virtual machine](./media/virtual-machines-sql-server-business-intelligence/IC650112.gif)

- Windows リモート デスクトップ接続を使用して、仮想マシンに接続します。 リモート デスクトップのユーザー インターフェイスで、次の手順を実行します。

    1. 型、 **クラウド サービス名** コンピューター名として。
    
    1. コロン (:) を入力し、TCP リモート デスクトップ エンドポイント用に構成されているパブリック ポート番号を入力します。
        
        Myservice.cloudapp.net:63133
        
        詳細については、次を参照してください。 [クラウド サービスは何ですか?](http://www.windowsazure.com/manage/services/cloud-services/what-is-a-cloud-service/)します。

**Reporting Services 構成マネージャーを起動します。**

1.  **Windows Server 2012**:

1.  **開始** 画面で「 **Reporting Services** アプリの一覧を表示します。

1. 右クリック **Reporting Services 構成マネージャー** ] をクリック **管理者として実行**します。

1.  **Windows Server 2008 R2**:

1. クリックして **開始**, 、] をクリックし、 **すべてのプログラム**します。

1. クリックして **Microsoft SQL Server 2012**します。

1. クリックして **構成ツール**します。

1. 右クリック **Reporting Services 構成マネージャー** ] をクリック **管理者として実行**します。

または

1. クリックして **開始**します。

1.  **プログラムとファイルの検索** ダイアログの種類 **レポート サービス**します。 VM が Windows Server 2012 を実行している場合は、入力 **レポート サービス** Windows Server 2012 のスタート画面にします。

1. 右クリック **Reporting Services 構成マネージャー** ] をクリック **管理者として実行**します。

    ![SSRS 構成マネージャーの検索](./media/virtual-machines-sql-server-business-intelligence/IC650113.gif)

### Reporting Services の構成

**サービス アカウントと Web サービス URL:**

1. 確認、 **サーバー名** はローカル サーバー名とクリック **接続**します。

1. 空白に注意してください **レポート サーバー データベース名**します。 データベースは構成の完了時に作成されます。

1. 確認、 **レポート サーバーの状態** は **開始**します。 Windows サーバー マネージャーでサービスを確認する場合、サービスは、 **SQL Server Reporting Services** Windows サービスです。

1. クリックして **サービス アカウント** し、必要に応じて、アカウントを変更します。 組み込みの非ドメインに参加している環境で仮想マシンが使用されている場合 **ReportServer** アカウントで十分です。 サービス アカウントの詳細については、次を参照してください。 [サービス アカウント](https://msdn.microsoft.com/library/ms189964.aspx)します。

1. クリックして **Web サービス URL** 左側のウィンドウでします。

1. クリックして **適用** 既定値を構成します。

1. 注、 **レポート サーバー Web サービスの Url**します。 既定の TCP ポートが 80 であり、URL に含まれていることを確認します。 後の手順で、このポートの Microsoft Azure 仮想マシン エンドポイントを作成します。

1.  **結果** ] ウィンドウで、操作が正常に完了したことを確認します。

**データベース:**

1. クリックして **データベース** 左側のウィンドウでします。

1. クリックして **データベース変更**します。

1. 確認 **新しいレポート サーバー データベースを作成する** が選択されているし、[次へ] をクリックします。

1. 確認 **サーバー名** ] をクリック **接続のテスト**します。

1. 結果の場合 **が成功した接続をテストする**, 、] をクリックして **[ok]** ] をクリックし、 **次**します。

1. データベース名は、 **ReportServer** と **レポート サーバー モード** は **ネイティブ** 順にクリックして **次**します。

1. をクリックして **次** 上、 **資格情報** ページです。

1. をクリックして **次** 上、 **概要** ページです。

1. をクリックして **次** 上、 **進行状況し、完了日** ページです。

**レポート マネージャー URL:**

1. クリックして **レポート マネージャー URL** 左側のウィンドウでします。

1. クリックして **適用**します。

1.  **結果** ] ウィンドウで、操作が正常に完了したことを確認します。

1. クリックして **終了**します。

レポート サーバーの権限については、次を参照してください。 [ネイティブ モードのレポート サーバーに対する権限の許可](https://msdn.microsoft.com/library/ms156014.aspx)します。

### ローカル レポート マネージャーの参照

構成を確認するには、VM 上でレポート マネージャーを参照します。

1. VM で、管理者特権を使用して Internet Explorer を起動します。

1. Http://localhost/reports VM 上に移動します。

### リモート レポート マネージャーに接続するには

リモート コンピューターから仮想マシン上のレポート マネージャーに接続する場合は、新しい仮想マシン TCP エンドポイントを作成します。 既定では、レポート サーバーが HTTP 要求をリッスン **ポート 80**します。 別のポートを使用するようにレポート サーバーの URL を構成した場合は、次の手順でそのポート番号を指定する必要があります。

1. TCP ポート 80 の仮想マシンのエンドポイントを作成します。 詳細については、次を参照してください。、、 [仮想マシン エンドポイントとファイアウォール ポート](#virtual-machine-endpoints-and-firewall-ports) このドキュメントの「します。

1. 仮想マシンのファイアウォールでポート 80 を開きます。

1. Azure の仮想マシンを使用してレポート マネージャーを参照 **DNS 名** URL でサーバー名として。 次に例を示します。 

    **レポート マネージャー**: http://uebi.cloudapp.net/reportserver
    **レポート サーバー**: http://uebi.cloudapp.net/reports

    [レポート サーバー アクセスに対するファイアウォールの構成](https://technet.microsoft.com/library/bb934283.aspx)

### レポートを作成して Azure 仮想マシンに発行するには

Microsoft Azure 仮想マシンでホストされているレポート サーバーに、オンプレミスのコンピューターから既存のレポートを発行する際に使用できるオプションの一部を次に示します。

- **レポート ビルダー**: 仮想マシンには、クリックが含まれています。-Microsoft SQL Server レポート ビルダーのバージョンでは 1 回です。 仮想マシンでレポート ビルダーを初めて起動するときは、次の手順を実行します。
                                            
    1. 管理者特権でブラウザーを起動します。
    
    1. 仮想マシン上のレポート マネージャーを参照してクリックして **レポート ビルダー** をリボンにします。
    
    詳細については、次を参照してください。 [インストール、アンインストール、およびレポート ビルダーをサポートする](https://technet.microsoft.com/library/dd207038.aspx)です。

- **SQL Server Data Tools**: VM: SQL Server Data Tools が仮想マシンにインストールされているし、作成に使用できる **レポート サーバー プロジェクト** と仮想マシンにレポートします。 SQL Server Data Tools では、仮想マシン上のレポート サーバーにレポートを発行できます。

- **SQL Server Data Tools: リモート**: ローカル コンピューター上には、Reporting Services レポートを含む SQL Server Data Tools で Reporting Services プロジェクトを作成します。 Web サービス URL に接続するようにプロジェクトを構成します。

    ![SSRS プロジェクトの SSDT プロジェクト プロパティ](./media/virtual-machines-sql-server-business-intelligence/IC650114.gif)

- レポートを格納する .VHD ハード ドライブを作成し、ドライブをアップロードして接続します。

    1. ローカル コンピューター上で、レポートを格納する .VHD ハード ドライブを作成します。
    
    1. 管理証明書を作成し、インストールします。
    
    1. Add-azurevhd コマンドレットを使用して Azure に VHD ファイルをアップロード [の作成とアップロード Azure への Windows Server VHD](virtual-machines-create-upload-vhd-windows-server.md)します。
    
    1. ディスクを仮想マシンに接続します。

## SQL Server のその他のサービスと機能のインストール

表形式モードの Analysis Services など、SQL Server のその他のサービスをインストールするには、SQL Server セットアップ ウィザードを実行します。 セットアップ ファイルは仮想マシンのローカル ディスクにあります。

1. クリックして **開始** ] をクリックし、 **すべてのプログラム**します。

1. クリックして **Microsoft SQL Server 2014** または **Microsoft SQL Server 2012** ] をクリックし、 **構成ツール**します。

1. クリックして **SQL Server インストール センター**します。

または、C:\SQLServer_12.0_full\setup.exe か C:\SQLServer_11.0_full\setup.exe を実行します。

>[AZURE.NOTE] 初めて SQL Server セットアップを実行する多くのセットアップ ファイルはダウンロードすることし、仮想マシンを再起動し、SQL Server セットアップの再起動を必要とします。
>
>Microsoft Azure 仮想マシンから選択したイメージを何度もカスタマイズする必要がある場合は、独自の SQL Server イメージを作成することを検討してください。 SQL Server 2012 SP1 CU2 で、Analysis Services SysPrep 機能が有効になりました。 詳細については、次を参照してください。 [SysPrep を使用して SQL Server のインストールに関する考慮事項](https://msdn.microsoft.com/library/ee210754.aspx)します。

### Analysis Services 表形式モードをインストールするには

このセクションの手順 **要約** Analysis Services 表形式モードのインストール。 詳細については、次のトピックを参照してください。

- [表形式モードでの Analysis Services のインストール](https://msdn.microsoft.com/library/hh231722.aspx)

- [テーブル モデリング (Adventure Works チュートリアル)](https://technet.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**Analysis Services 表形式モードをインストールするには:**

1. SQL Server インストール ウィザードで次のようにクリックします。 **インストール** 左側のウィンドウをクリックし **新しい SQL server のスタンドアロン インストールまたは既存のインストールに機能の追加**します。

    - 表示される場合、 **フォルダーの参照**, c:\SQLServer_12.0_full または c:\SQLServer_11.0_full を参照し、をクリックし、 **Ok**します。

1. をクリックして **次** 製品の更新プログラム] ページにします。

1.  **インストールの種類** ] ページで、[ **SQL Server の新規インストールを実施** ] をクリック **次**します。

1.  **セットアップ ロール** ] ページで [ **SQL Server 機能のインストール**します。

1.  **機能の選択** ] ページで [ **Analysis Services**します。

1.  **インスタンスの構成** ] ページで、わかりやすい名前を入力します。 **Tabular** に **名前付きインスタンスを** と **インスタンス Id** テキスト ボックスです。

1.  **Analysis Services の構成** ] ページで、[ **表形式モード**します。 現在のユーザーを管理権限の一覧に追加します。

1. SQL Server インストール ウィザードを完了して閉じます。

## Analysis Services の構成

### Analysis Services サーバーへのリモート アクセス

Analysis Services サーバーでは、Windows 認証だけがサポートされています。 SQL Server Management Studio や SQL Server Data Tools などのクライアント アプリケーションから Analysis Services にリモート アクセスするには、Azure Virtual Networking を使用して、仮想マシンをローカル ドメインに参加させる必要があります。 詳細については、「 [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)します。

A **既定のインスタンス** TCP ポートでリッスンする Analysis Services の **2383年**します。 仮想マシンのファイアウォールでポートを開きます。 クラスター化された名前付き Analysis Services のインスタンスもポートでリッスン **2383年**します。

 **名前付きインスタンス** Analysis Services の SQL Server Browser サービスはポートへのアクセスの管理に必要です。 SQL Server Browser の既定の構成は、ポート **2382年**します。

仮想マシン ファイアウォールでポートを開く **2382年** 静的 Analysis Services が名前付きインスタンス ポートを作成します。

1. VM 上で既に使用されているポートと、ポートを使用しているプロセスを確認するには、管理者特権で次のコマンドを実行します。

        netstat /ao

1. SQL Server Management Studio を使用して、表形式 AS インスタンスの全般プロパティで "ポート" 値を更新し、Analysis Services の名前付きインスタンスの静的ポートを作成します。 詳細についてを参照してください、"固定ポートを使用して、既定または名前付きインスタンス" [Analysis Services のアクセスを許可するための Windows ファイアウォールを構成する](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed)です。

1. Analysis Services サービスの表形式のインスタンスを再起動します。

詳細については、次を参照してください。、、 **仮想マシン エンドポイントとファイアウォール ポート** このドキュメントの「します。

## 仮想マシン エンドポイントとファイアウォール ポート

ここでは、作成する Microsoft Azure 仮想マシン エンドポイントと、仮想マシンのファイアウォールで開くポートの概要を説明します。 次の表の **TCP** ポートのエンドポイントを作成して、仮想マシン ファイアウォールで開くポートをします。

- VM を 1 つだけ使用し、次の 2 つの条件に該当する場合は、VM エンドポイントを作成する必要はありません。また、VM 上のファイアウォールでポートを開く必要もありません。

    - VM 上の SQL Server 機能にリモート接続しない。 VM へのリモート デスクトップ接続を確立し、VM 上で SQL Server 機能にローカルでアクセスすることは、SQL Server 機能へのリモート接続とは見なされません。
    
    - Azure Virtual Networking または別の VPN トンネリング ソリューションを使用して、VM をオンプレミス ドメインに参加させない。

- 仮想マシンがドメインに参加していない場合に VM 上の SQL Server 機能にリモート接続するには、次の操作を行います。 

    - VM 上のファイアウォールでポートを開きます。
    
    - アスタリスク (*) が付いているポートの仮想マシン エンドポイントを作成します。

- 仮想マシンが Azure Virtual Networking などの VPN トンネルを使用してドメインに参加している場合、エンドポイントは不要です。 ただし、VM 上のファイアウォールでポートを開いてください。

    |Port|種類の種類Description|
|---|---|---|
|**80**|TCP|レポート サーバー リモート アクセス (*) です |。
|**1433**|TCP|SQL Server Management Studio (*) です |。
|**1434**|UDP|SQL Server Browser。 これで VM をドメインに参加しているときに必要です |。
|**2382**|TCP|SQL Server Browser |。
|**2383**|TCP|SQL Server Analysis Services インスタンスとクラスター化された名前付きインスタンスを既定値です |。
|**ユーザー定義**|TCP|名前付きインスタンス ポートを選択すると、ポート番号の静的分析サービスを作成し、ファイアウォールでポート番号のブロックを解除します |。

エンドポイントの作成の詳細については、次のトピックをご覧ください。

- エンドポイントの作成:[仮想マシンに対してエンドポイントを設定する方法](virtual-machines-set-up-endpoints.md)します。

- SQL Server: の「完全な構成は、仮想マシンを使用する SQL Server Management Studio に接続する手順」セクションを参照してください [Azure で SQL Server 仮想マシンをプロビジョニング](virtual-machines-provision-sql-server.md)します。

次の図は、VM 上の機能とコンポーネントへのリモート アクセスを可能にするために、VM のファイアウォールで開くポートを示しています。

![Azure VM の BI アプリケーションに対して開くポート](./media/virtual-machines-sql-server-business-intelligence/IC654385.gif)

## リソース

- Azure 仮想マシン環境で使用される、マイクロソフト サーバー ソフトウェアのサポート ポリシーを確認します。 BitLocker、フェールオーバー クラスタリング、ネットワーク負荷分散などの機能のサポートについては、 [Microsoft サーバー ソフトウェア サポート Azure Virtual Machines の](http://support.microsoft.com/kb/2721672)です。

- [Azure Virtual Machines における SQL Server の概要](virtual-machines-sql-server-infrastructure-services.md)

- [Virtual Machines](http://azure.microsoft.com/documentation/services/virtual-machines/)

- [Azure での SQL Server 仮想マシンのプロビジョニング](virtual-machines-provision-sql-server.md)

- [データ ディスクを仮想マシンに接続する方法](storage-windows-attach-disk.md)

- [Azure VM の SQL Server へのデータベースの移行](virtual-machines-migrate-onpremises-database.md)

- [Analysis Services インスタンスのサーバー モードの決定](https://msdn.microsoft.com/library/gg471594.aspx)

- [多次元モデリング (Adventure Works チュートリアル)](https://technet.microsoft.com/library/ms170208.aspx)

- [Azure ドキュメント センター](http://azure.microsoft.com/documentation/)

- [Using Power BI in a Hybrid Environment (ハイブリッド環境での Power BI の使用)](https://msdn.microsoft.com/library/dn798994.aspx)

>[AZURE.NOTE] [フィードバックと連絡先の情報を Microsoft SQL Server の接続を介して送信](https://connect.microsoft.com/SQLServer/Feedback)

### コミュニティ コンテンツ

- [Azure SQL Database Management with PowerShell (PowerShell を使用した Azure SQL Database の管理)](http://blogs.msdn.com/b/windowsazure/archive/2013/02/07/windows-azure-sql-database-management-with-powershell.aspx)

