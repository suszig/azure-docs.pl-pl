<properties 
    pageTitle="ネイティブ モードのレポート サーバーを実行する VM を PowerShell を使用して作成する | Microsoft Azure"
    description="このトピックでは、Azure 仮想マシンで SQL Server Reporting Services ネイティブ モードのレポート サーバーをデプロイおよび構成する手順について説明します。 "
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

# ネイティブ モードのレポート サーバーを実行する Azure VM を PowerShell を使用して作成する

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。
 

このトピックでは、Azure 仮想マシンで SQL Server Reporting Services ネイティブ モードのレポート サーバーをデプロイおよび構成する手順について説明します。 このドキュメントの手順では、仮想マシンを手動で作成する手順と、VM で Reporting Services を構成する Windows PowerShell スクリプトを組み合わせて使用しています。 構成スクリプトでは、HTTP または HTTPS のファイアウォール ポートを開きます。

>[AZURE.NOTE] 必要としない場合 **HTTPS** レポート サーバーで **手順 2 をスキップ**します。
>
>手順 1 で VM を作成したら、「スクリプトを使用してレポート サーバーと HTTP を構成する」に進みます。 スクリプトを実行すると、レポート サーバーの使用準備が完了します。

## 前提条件

- **Azure サブスクリプション**: Azure サブスクリプションで使用できるコアの数を確認します。 推奨される VM サイズを作成する場合 **A3**, 、する必要があります **4** 使用可能なコア。 VM のサイズを使用する場合 **A2**, 、する必要があります **2** 使用可能なコア。
    
    - サブスクリプションのコアの上限を確認するには、Azure クラシック ポータルの左側のウィンドウで [設定] をクリックし、上部のメニューの [使用状況] をクリックします。
    
    - コア クォータを増やすにお問い合わせ [Azure サポート](http://azure.microsoft.com/support/options/)します。 VM サイズについては、次を参照してください。 [Azure の仮想マシンのサイズ](virtual-machines-size-specs.md)します。

- **Windows PowerShell スクリプト**: トピックでは、Windows PowerShell の基本的な知識があると想定します。 Windows PowerShell の使用方法の詳細については、次のトピックをご覧ください。

    - [Starting Windows PowerShell on Windows Server (Windows Server での Windows PowerShell の起動)](https://technet.microsoft.com/library/hh847814.aspx)
    
    - [Getting Started with Windows PowerShell (Windows PowerShell の概要)](https://technet.microsoft.com/library/hh857337.aspx)

## 手順 1: Azure 仮想マシンをプロビジョニングする

1. Azure クラシック ポータルに移動します。

1. クリックして **仮想マシン** 左側のウィンドウでします。

    ![Microsoft Azure Virtual Machines](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC660124.gif)

1. クリックして **新しい**します。

    ![[新規] ボタン](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC692019.gif)

1. クリックして **ギャラリーから**します。

    ![ギャラリーから新しい VM を作成](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC692020.gif)

1. クリックして **SQL Server 2014 RTM Standard – Windows Server 2012 R2** し、矢印をクリックして続行します。

    ![次へ](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC692021.gif)

    Reporting Services のデータ ドリブン サブスクリプション機能を必要がある場合は、選択 **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**します。 SQL Server のエディションと機能のサポートに関する詳細については、次を参照してください。 [機能は、SQL Server 2012 の各エディションでサポートされている](https://msdn.microsoft.com/library/cc645993.aspx#Reporting)します。

1.  **仮想マシンの構成** ] ページで、次のフィールドを編集します。
                                    
    - 1 つ以上を使用する必要がある場合 **バージョンのリリース日**, 、最新のバージョンを選択します。
    
    - **仮想マシン名**: 既定のクラウド サービスの DNS 名として、次の構成ページでこのマシン名が使用もできます。 DNS 名は、Azure サービス全体で一意であることが必要です。 VM の用途を示すコンピューター名で VM を構成するよう考慮してください  (例: ssrsnativecloud)。
    
    - **層**: 標準
    
    - **サイズ: A3** は SQL Server のワークロードに推奨される VM サイズ。 VM をレポート サーバーとしてのみ使用する場合、レポート サーバーで大規模なワークロードが発生しないのであれば、VM サイズは A2 で十分です。 VM の料金情報については、次を参照してください。 [Virtual Machines 料金](http://azure.microsoft.com/pricing/details/virtual-machines/)します。
    
    - **新しいユーザー名**: 指定した名前が VM の管理者として作成されます。
    
    - **新しいパスワード** と **確認**します。 このパスワードは、新しい管理者アカウントに使用されるので、強力なパスワードを使用することをお勧めします。
    
    - クリックして **次**します。 ![次へ]](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC692021.gif)

1. 次のページでは、以下のフィールドを編集します。

    - **クラウド サービス**: 選択 **新しいクラウド サービスを作成する**です。
    
    - **クラウド サービス DNS 名**: これは、VM に関連付けられているクラウド サービスのパブリック DNS 名。 既定の名前は、VM 名として入力した名前です。 トピックの後の手順では、信頼された SSL 証明書を作成しの値には、DNS 名が使用する場合、"**に対して発行された**"証明書のです。
    
    - **リージョン/アフィニティ グループ/仮想ネットワーク**: エンドユーザーに最も近いリージョンを選択します。
    
    - **ストレージ アカウント**: 自動的に生成されたストレージ アカウントを使用します。
    
    - **可用性セット**: なし。
    
    - **エンドポイント** 保持、 **リモート デスクトップ** と **PowerShell** エンドポイントし、環境に応じて、HTTP または HTTPS のいずれかのエンドポイントを追加します。

        - **HTTP**: 既定のパブリックおよびプライベート ポート **80**します。 80 以外のプライベート ポートを使用する場合は、変更を **$HTTPport = 80** http スクリプトでします。

        - **HTTPS**: 既定のパブリックおよびプライベート ポート **443**します。 セキュリティのベスト プラクティスとして、プライベート ポートを変更し、そのプライベート ポートを使用するようにファイアウォールとレポート サーバーを構成することをお勧めします。 エンドポイントの詳細については、次を参照してください。 [仮想マシンとの通信を設定する方法](virtual-machines-set-up-endpoints.md)します。 443 以外のポートを使用する場合は、パラメーターを変更するメモ **$HTTPsport = 443** 、HTTPS スクリプトです。
    
    - [次へ] をクリックします。 ![次へ](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC692021.gif)

1. ウィザードの最後のページで、既定はそのまま **VM エージェントをインストール** 選択します。 このトピックの手順では VM エージェントを使用しませんが、この VM を保持する場合は、VM エージェントと拡張機能を使用することで CM を強化できます。  VM エージェントの詳細については、次を参照してください。 [VM エージェントと拡張機能-パート 1](http://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/)します。 インストールされ、実行されている既定の拡張機能の 1 つに "BGINFO" 拡張機能があります。この機能により、内部 IP やドライブの空き領域などのシステム情報が VM デスクトップに表示されます。

1. [完了] をクリックします。 ![OK](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC660122.gif)

1.  **ステータス** VM のとして表示されます。 **開始中 (プロビジョニング)** として表示およびプロビジョニング プロセス中に **を実行している** VM がプロビジョニングされ、準備ができての使用の場合。

## 手順 2: サーバー証明書を作成する

>[AZURE.NOTE] 場合は、レポート サーバーに HTTPS が不要で、 **手順 2 をスキップ** セクションに進むと **スクリプトを使用して、レポート サーバー、HTTP 構成**します。 HTTP スクリプトを使用すると、レポート サーバーが迅速に構成され、レポート サーバーの使用準備が完了します。

VM で HTTPS を使用するには、信頼済み SSL 証明書が必要です。 シナリオに応じて、次の 2 つの方法のいずれかを使用できます。

- 証明機関 (CA) が発行し、Microsoft が信頼する有効な SSL 証明書。 CA ルート証明書が Microsoft ルート証明書プログラムによって配布されている必要があります。 このプログラムの詳細については、次を参照してください。 [Windows および Windows Phone 8 SSL ルート証明書プログラム (メンバー Ca)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) と [が Microsoft ルート証明書プログラムの概要](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx)します。

- 自己署名証明書。 運用環境では、自己署名証明書を使用しないことをお勧めします。

### 信頼された証明機関 (CA) によって作成された証明書を使用するには

1. **証明機関から、web サイトのサーバー証明書を要求する**です。 

    Web サーバー証明書ウィザードを使用して、証明機関に送信する証明書要求ファイル (Certreq.txt) を生成することも、オンライン証明機関  (Windows Server 2012 の Microsoft 証明書サービスなど) への要求を生成することもできます。 サーバー証明書が提供する ID 保証レベルに応じて、証明機関によって要求が承認され、証明書ファイルが送られてくるまで数日から数か月かかります。 

    サーバー証明書の要求の詳細については、以下をご覧ください。 
    
    - 使用 [Certreq](https://technet.microsoft.com/library/cc725793.aspx), 、[Certreq](https://technet.microsoft.com/library/cc725793.aspx)します。
    
    - Windows Server 2012 を管理するためのセキュリティ ツール: 

    [Security Tools to Administer Windows Server 2012 (Windows Server 2012 を管理するためのセキュリティ ツール)](https://technet.microsoft.com/library/jj730960.aspx)

    >[AZURE.NOTE]  **に対して発行された** 信頼された SSL 証明書のフィールドには、同じにする必要があります、 **クラウド サービス DNS 名** 新しいバーチャル マシンのために使用します。

1. **Web サーバーにサーバー証明書をインストール**します。 この例の Web サーバーは、レポート サーバーをホストする VM です。Web サイトは、後で Reporting Services を構成するときに作成されます。 Web サーバーに証明書] MMC スナップインを使用して、サーバー証明書をインストールする方法の詳細については、次を参照してください。 [サーバー証明書をインストール](https://technet.microsoft.com/library/cc740068)します。
    
    このトピックに含まれているスクリプトを使用して、レポート サーバーの証明書の値を構成する **拇印** として、スクリプトのパラメーターが必要です。 証明書の拇印を取得する方法の詳細については、次のセクションをご覧ください。

1. サーバー証明書をレポート サーバーに割り当てます。 この割り当ては、次のセクションでレポート サーバーを構成したときに完了します。

### Virtual Machines の自己署名証明書を使用するには

VM をプロビジョニングしたときに、VM 上に自己署名証明書が作成されています。 この証明書には、VM の DNS 名と同じ名前が付けられています。 証明書エラーを防ぐためには、証明書が VM 上で信頼されているだけでなく、サイトのすべてのユーザーからも信頼されている必要があります。

1. ローカル VM 上の証明書のルート CA を信頼するように証明書を追加、 **信頼されたルート証明機関**します。 必要な手順の概要を次に示します。 CA を信頼する方法の詳細な手順については、次を参照してください。 [サーバー証明書をインストール](https://technet.microsoft.com/library/cc740068)します。

    1. Azure クラシック ポータルで VM を選択し、[接続] をクリックします。 ブラウザー構成によっては、VM に接続するために .rdp ファイルを保存するよう求められる場合があります。
    
        ![connect to azure virtual machine](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC650112.gif) ユーザー VM 名、ユーザー名と VM を作成したときに構成したパスワードを使用します。 
    
        たとえば、次の図に、VM 名は **ssrsnativecloud** で、ユーザー名は **testuser**します。
        
        ![VM 名が含まれたログイン](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC764111.png)
    
    1. Mmc.exe を実行します。 詳細については、次を参照してください。 [方法: MMC スナップインで証明書の表示](https://msdn.microsoft.com/library/ms788967.aspx)します。
    
    1. コンソール アプリケーションで **ファイル** ] メニューの [追加、 **証明書** スナップインで、[ **コンピューター アカウント** ] し、順にクリックして **次**します。
    
    1. 選択 **ローカル コンピューター** を管理し、をクリックし、 **完了**します。
    
    1. をクリックして **Ok** 順に展開し、 **証明書 - 個人** ノードをクリック **証明書**します。 証明書は、仮想マシンの DNS 名の後にという名前で終わる **cloudapp.net**します。 証明書の名前を右クリックし、クリックして **コピー**します。
    
    1. 展開、 **信頼されたルート証明機関** ノードと右クリック **証明書** ] をクリックし、 **貼り付け**します。
    
    1. 検証は、[証明書の名前をダブルクリック **信頼されたルート証明機関** ことを確認してエラーがない、証明書を参照してください。 このトピックに含まれている HTTPS スクリプトを使用して、レポート サーバーの証明書の値を構成する **拇印** として、スクリプトのパラメーターが必要です。 **拇印の値を取得する**, 、次を完了します。 セクションでサムプリントを取得するための PowerShell サンプルもあります [スクリプトを使用して、レポート サーバーと HTTPS を構成する](#use-script-to-configure-the-report-server-and-HTTPS)です。
        
        1. 証明書の名前 (例: ssrsnativecloud.cloudapp.net) をダブルクリックします。
        
        1. クリックして、 **詳細** ] タブをクリックします。
        
        1. クリックして **拇印**します。 詳細フィールドに拇印の値が表示されます (例: ‎a6 08 3c df f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f)。
        
        1. 拇印をコピーし、後で編集できるように値を保存するか、スクリプトをすぐに編集します。
        
        1. (*) スクリプトを実行する前に、値の各ペアの間にあるスペースを削除します。 たとえば、上記の拇印は ‎a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f になります。
        
        1. サーバー証明書をレポート サーバーに割り当てます。 この割り当ては、次のセクションでレポート サーバーを構成したときに完了します。

自己署名 SSL 証明書を使用している場合は、証明書の名前が VM のホスト名と既に一致しています。 したがって、マシンの DNS は既にグローバルに登録されており、どのクライアントからでもアクセスできます。

## 手順 3: レポート サーバーを構成する

ここでは、Reporting Services ネイティブ モードのレポート サーバーとして VM を構成する手順を説明します。 レポート サーバーは、次のいずれかの方法を使用して構成できます。

- スクリプトを使用してレポート サーバーを構成する

- 構成マネージャーを使用してレポート サーバーを構成する

詳細な手順については、セクションを参照してください。 [仮想マシンに接続し、Reporting Services 構成マネージャーを起動](virtual-machines-sql-server-business-intelligence.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager)します。

**認証に関する注意:** Windows 認証は、推奨される認証方法と、これが既定の Reporting Services の認証。 VM 上で構成されているユーザーだけが Reporting Services にアクセスでき、Reporting Services ロールに割り当てられます。

### スクリプトを使用してレポート サーバーと HTTP を構成する

Windows PowerShell スクリプトを使用してレポート サーバーを構成するには、次の手順を実行します。 この構成には、HTTPS ではなく HTTP が含まれます。

1. Azure クラシック ポータルで VM を選択し、[接続] をクリックします。 ブラウザー構成によっては、VM に接続するために .rdp ファイルを保存するよう求められる場合があります。

    ![connect to azure virtual machine](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC650112.gif) ユーザー VM 名、ユーザー名と VM を作成したときに構成したパスワードを使用します。 

    たとえば、次の図に、VM 名は **ssrsnativecloud** で、ユーザー名は **testuser**します。
    
    ![VM 名が含まれたログイン](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC764111.png)

1. VM で、開く **Windows PowerShell ISE** 管理者特権を使用します。 PowerShell ISE は、Windows Server 2012 に既定でインストールされています。 標準の Windows PowerShell ウィンドウではなく、ISE を使用することをお勧めします。ISE では、スクリプトを貼り付けて変更してから、スクリプトを実行できます。

1. Windows PowerShell ISE をクリックして、 **ビュー** クリックしてメニュー **スクリプト ウィンドウに表示**します。

1. 次のスクリプトをコピーし、Windows PowerShell ISE のスクリプト ウィンドウに貼り付けます。

        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
        
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change the value if you used a different port for the private HTTP endpoint when the VM was created.
        
        ## Set PowerShell execution policy to be able to run scripts
        Set-ExecutionPolicy RemoteSigned -Force
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Report Server Configuration Steps
        
        ## Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
           
        ## Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
        
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. 80 以外の HTTP ポートを使用して VM を作成した場合は、$HTTPport = 80 パラメーターを変更します。

1. スクリプトは現在、Reporting Services で構成されています。 Reporting Services 用のスクリプトを実行する場合は、Get-wmiobject ステートメントを"v11"にする名前空間へのパスのバージョン部分を変更します。

1. スクリプトを実行します。

**検証**: 基本的なレポート サーバーの機能が動作していることを確認するには、次を参照してください。、 [構成を確認する](#verify-the-configuration) このトピックで後述します。

### スクリプトを使用してレポート サーバーと HTTPS を構成する

Windows PowerShell を使用してレポート サーバーを構成するには、次の手順を実行します。 この構成には、HTTP ではなく HTTPS が含まれます。

1. Azure クラシック ポータルで VM を選択し、[接続] をクリックします。 ブラウザー構成によっては、VM に接続するために .rdp ファイルを保存するよう求められる場合があります。

    ![connect to azure virtual machine](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC650112.gif) ユーザー VM 名、ユーザー名と VM を作成したときに構成したパスワードを使用します。 

    たとえば、次の図に、VM 名は **ssrsnativecloud** で、ユーザー名は **testuser**します。

    ![VM 名が含まれたログイン](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC764111.png)

1. VM で、開く **Windows PowerShell ISE** 管理者特権を使用します。 PowerShell ISE は、Windows Server 2012 に既定でインストールされています。 標準の Windows PowerShell ウィンドウではなく、ISE を使用することをお勧めします。ISE では、スクリプトを貼り付けて変更してから、スクリプトを実行できます。

1. スクリプトの実行を有効にするには、次の Windows PowerShell コマンドを実行します。

        Set-ExecutionPolicy RemoteSigned

    次のコマンドを実行してポリシーを検証できます。

        Get-ExecutionPolicy

1.  **Windows PowerShell ISE**, をクリックして、 **ビュー** メニューをクリック **スクリプト ウィンドウに表示**します。

1. 次のスクリプトをコピーし、Windows PowerShell ISE のスクリプト ウィンドウに貼り付けます。

        ## This script configures the report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when the HTTPS endpoint was created.
        
        # You can run the following command to get (.cloudapp.net certificates) so you can copy the thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # The certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # the certificate hash should not contain spaces
        
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If the certificate is not a wildcard certificate, comment out the following line, and enable the full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
        
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
        
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
        
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
        
        write-host "The script will use $DNSNameAndPort as the DNS name and port" 
        
        ## Register for MSReportServer_ConfigurationSetting
        ## Change the version portion of the path to "v11" to use the script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
        
        ## Reporting Services Report Server Configuration Steps
        
        ## 1. Setting the web service URL ##
        write-host -foregroundcolor green "Setting the web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
        
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
        
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
            
        ## 2. Setting the Database ##
        write-host -foregroundcolor green "Setting the Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## GenerateDatabaseScript - for creating the database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
        
        ## Execute sql script to create the database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes to sqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
          
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
        
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
        
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
        
        ## 3. Setting the Report Manager URL ##
        
        write-host -foregroundcolor green "Setting the Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
        
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
        
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
        
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
        
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
        
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
        
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time

1. 変更、 **$certificatehash** スクリプトのパラメーター。

    - これは、 **必要** パラメーター。 以前の手順で証明書の値を保存しなかった場合は、次のいずれかの方法で、証明書の拇印から証明書のハッシュ値をコピーします。

        VM で Windows PowerShell ISE を開き、次のコマンドを実行します。

            dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer

        出力は次のようになります。 VM がたとえば構成された証明書を持たない場合、空白行を返し、スクリプトを参照してください [仮想マシンの自己署名証明書を使用する](#to-use-the-virtual-machines-self-signed-certificate)です。
    
    または
    
    - VM 上には、mmc.exe を実行し、追加、 **証明書** スナップインです。
    
    - 下にある、 **信頼されたルート証明機関** ノードで、証明書の名前をダブルクリックします。 VM の自己署名証明書を使用している場合、証明書は、仮想マシンの DNS 名の後にという名前で終わる **cloudapp.net**します。
    
    - クリックして、 **詳細** ] タブをクリックします。
    
    - クリックして **拇印**します。 詳細フィールドに拇印の値が表示されます (例: af 11 60 b6 4b 28 8d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48)。
    
    - **スクリプトを実行する前に**, 、値のペアの間にスペースを削除します。 (例: af1160b64b288d890a8212ff6ba9c3664f319048)。

1. 変更、 **$httpsport** パラメーター。 

    - HTTPS エンドポイントにポート 443 を使用した場合、スクリプトでこのパラメーターを更新する必要はありません。 それ以外の場合は、VM で HTTPS プライベート エンドポイントを構成したときに選択したポート値を使用します。

1. 変更、 **$DNSName** パラメーター。 

    - このスクリプトは、ワイルドカード証明書 $DNSName="+" 用に構成されています。 ワイルドカード証明書バインド用に構成しない場合は、$DNSName="+" をコメント アウトし、次の行の完全な $DNSNAme 参照である ##$DNSName="$server.cloudapp.net" を有効にします。

        仮想マシンの DNS 名を Reporting Services に使用しない場合は、$DNSName 値を変更します。 このパラメーターを使用する場合は、証明書でも同じ名前を使用する必要があります。DNS サーバーでその名前をグローバルに登録します。

1. スクリプトは現在、Reporting Services で構成されています。 Reporting Services 用のスクリプトを実行する場合は、Get-wmiobject ステートメントを"v11"にする名前空間へのパスのバージョン部分を変更します。

1. スクリプトを実行します。

**検証**: 基本的なレポート サーバーの機能が動作していることを確認するには、次を参照してください。、 [構成を確認する](#verify-the-connection) このトピックで後述します。 証明書バインドを確認するには、管理者特権でコマンド プロンプトを開き、次のコマンドを実行します。

    netsh http show sslcert

結果には次の行が含まれます。

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### 構成マネージャーを使用してレポート サーバーを構成する

レポート サーバーを構成する際に PowerShell スクリプトを実行しない場合は、このセクションの手順に従い、Reporting Services ネイティブ モード構成マネージャーを使用してレポート サーバーを構成します。

1. Azure クラシック ポータルで VM を選択し、[接続] をクリックします。 VM の作成時に構成したユーザー名とパスワードを使用します。

    ![connect to azure virtual machine](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC650112.gif)

1. Windows Update を実行し、VM に更新プログラムをインストールします。 VM の再起動が必要な場合は、VM を再起動し、Azure クラシック ポータルから VM に再接続します。

1. VM で [スタート] メニューから次のように入力します。 **Reporting Services** 開き **Reporting Services 構成マネージャー**します。

1. 既定値のままにして **サーバー名** と **レポート サーバー インスタンス**します。 クリックして **接続**します。

1. 左のウィンドウで **Web サービス URL**します。

1. 既定では、RS は HTTP ポート 80 で構成され、IP は "すべて割り当て" に設定されます。 HTTPS を追加するには、次の手順を実行します。

    1.  **SSL 証明書**: [VM 名] の例については、使用する証明書を選択します。 cloudapp.net します。 証明書が表示されない場合は、セクションを参照してください。 **手順 2: サーバー証明書を作成** インストールして、VM で証明書を信頼する方法の詳細。
    
    1.  **SSL ポート**: 443 を選択します。 VM で別のプライベート ポートを使用して HTTPS プライベート エンドポイントを構成した場合は、ここでその値を使用します。
    
    1. クリックして **適用** 操作が完了するまで待機します。

1. 左のウィンドウで **データベース**します。

    1. クリックして **変更 Databas**e。
    
    1. をクリックして **新しいレポート サーバー データベースを作成** ] をクリックし、 **次**します。
    
    1. 既定値のままにして **サーバー名**: VM 名として、既定値のままにして **認証の種類** として **現在のユーザー** – **統合セキュリティ**します。 クリックして **次**します。
    
    1. 既定値のままにして **データベース名** として **ReportServer** ] をクリック **次**します。
    
    1. 既定値のままにして **認証の種類** として **サービスの資格情報** ] をクリック **次**します。
    
    1. をクリックして **次** 上、 **概要** ページです。
    
    1. 構成が完了したら、クリックして **完了**します。

1. 左のウィンドウで **レポート マネージャー URL**します。 既定値のままにして **仮想ディレクトリ** として **レポート** ] をクリック **適用**します。

1. クリックして **終了** を Reporting Services 構成マネージャーを閉じます。

## 手順 4: Windows ファイアウォール ポートを開く

>[AZURE.NOTE] レポート サーバーを構成するスクリプトのいずれかを使用した場合は、このセクションを省略できます。 スクリプトにファイアウォール ポートを開く手順が含まれていました。 既定のポートは、HTTP では 80、HTTPS では 443 です。

仮想マシン上のレポート マネージャーまたはレポート サーバーにリモートから接続するには、VM に TCP エンドポイントが必要です。 また、VM のファイアウォールで同じポートを開く必要があります。 エンドポイントは、VM をプロビジョニングしたときに作成されています。

ここでは、ファイアウォール ポートを開く方法に関する基本情報を提供します。 詳細については、次を参照してください [レポート サーバーへのアクセスに対してファイアウォールを構成。](https://technet.microsoft.com/library/bb934283.aspx)

>[AZURE.NOTE] レポート サーバーを構成するスクリプトを使用した場合は、このセクションを省略できます。 スクリプトにファイアウォール ポートを開く手順が含まれていました。

HTTPS のプライベート ポートを 443 以外で構成した場合は、以下のスクリプトを適切に変更してください。 ポートを開く **443** 、Windows ファイアウォールで、次の操作します。

1. 管理者特権で Windows PowerShell ウィンドウを開きます。

1. VM で HTTPS エンドポイントを構成したときに 443 以外のポートを使用した場合は、次のコマンド内のポートを更新してから、コマンドを実行します。

        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443

1. コマンドが完了したら、 **Ok** コマンド プロンプトに表示されます。

ポートが開いていることを確認するには、Windows PowerShell ウィンドウを開き、次のコマンドを実行します。

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## 構成を確認する

レポート サーバーの基本的な機能が動作していることを確認するには、管理者特権でブラウザーを開き、次のレポート サーバー URL とレポート マネージャー URL を参照します。

- VM で、レポート サーバー URL を参照します。

        http://localhost/reportserver

- VM で、レポート マネージャー URL を参照します。

        http://localhost/Reports

- ローカル コンピューターに移動、 **リモート** VM 上のレポート マネージャー。 必要に応じて、次の例の DNS 名を更新します。 パスワードの入力を求められたら、VM をプロビジョニングしたときに作成した管理者資格情報を使用します。 ユーザー名は [ドメイン] \ [ユーザー名] の形式でドメインは VM のコンピューター名、たとえば ssrsnativecloud\testuser です。 HTTP を使用していない場合**S**, 、削除、 **s** URL にします。 VM で追加のユーザーを作成する方法については、次のセクションをご覧ください。

        https://ssrsnativecloud.cloudapp.net/Reports

- ローカル コンピューターから、リモート レポート サーバー URL を参照します。 必要に応じて、次の例の DNS 名を更新します。 HTTPS を使用していない場合は、URL の "s" を削除します。

        https://ssrsnativecloud.cloudapp.net/ReportServer

## ユーザーを作成してロールを割り当てる

レポート サーバーを構成して確認したら、一般的な管理タスクとして、1 人以上のユーザーを作成し、ユーザーを Reporting Services ロールに割り当てます。 詳細については、次のトピックを参照してください。

- [ローカル ユーザー アカウントを作成する](https://technet.microsoft.com/library/cc770642.aspx)

- [ユーザー アクセスの許可、レポート サーバー (レポート マネージャー) に](https://msdn.microsoft.com/library/ms156034.aspx))

- [ロールの割り当てを作成および管理する](https://msdn.microsoft.com/library/ms155843.aspx)

## レポートを作成して Azure 仮想マシンに発行するには

Microsoft Azure 仮想マシンでホストされているレポート サーバーに、オンプレミスのコンピューターから既存のレポートを発行する際に使用できるオプションの一部を次に示します。

- **RS.exe スクリプト**: RS.exe スクリプトからのレポート アイテムと既存のレポート サーバーの Microsoft Azure 仮想マシンへのコピーを使用します。 詳細については、「ネイティブ モードからネイティブ モード: Microsoft Azure 仮想マシン」セクションを参照してください [サンプル Reporting Services rs.exe スクリプト コンテンツを移行するレポート サーバー間で](https://msdn.microsoft.com/library/dn531017.aspx)します。

- **レポート ビルダー**: 仮想マシンには、クリックが含まれています。-Microsoft SQL Server レポート ビルダーのバージョンでは 1 回です。 仮想マシンでレポート ビルダーを初めて起動するときは、次の手順を実行します。

    1. 管理者特権でブラウザーを起動します。
    
    1. 仮想マシン上のレポート マネージャーを参照してクリックして **レポート ビルダー**  をリボンにします。

    詳細については、次を参照してください。 [インストール、アンインストール、およびレポート ビルダーをサポートする](https://technet.microsoft.com/library/dd207038.aspx)です。

- **SQL Server Data Tools: VM**: SQL Server 2012 で VM を作成するかどうか、SQL Server Data Tools の仮想マシンにインストールされて、作成に使用できる **レポート サーバー プロジェクト** と仮想マシンにレポートします。 SQL Server Data Tools では、仮想マシン上のレポート サーバーにレポートを発行できます。
    
    SQL Server 2014 で VM を作成した場合は、SQL Server Data Tools - BI for Visual Studio をインストールできます。 詳細については、次のトピックを参照してください。

    - [Microsoft SQL Server Data Tools - Business Intelligence for Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)

    - [Microsoft SQL Server Data Tools - Business Intelligence for Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)

    - [SQL Server Data Tools and SQL Server Business Intelligence (SSDT-BI)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)

- **SQL Server Data Tools: リモート**: ローカル コンピューター上には、Reporting Services レポートを含む SQL Server Data Tools で Reporting Services プロジェクトを作成します。 Web サービス URL に接続するようにプロジェクトを構成します。

    ![SSRS プロジェクトの SSDT プロジェクト プロパティ](./media/virtual-machines-sql-server-create-native-mode-report-server-powershell/IC650114.gif)

- **スクリプトを使用して**: スクリプトを使用してレポート サーバーのコンテンツをコピーします。 詳細については、次を参照してください。 [サンプル Reporting Services rs.exe スクリプト コンテンツを移行するレポート サーバー間で](https://msdn.microsoft.com/library/dn531017.aspx)します。

## VM を使用していない場合にコストを最小限に抑える

>[AZURE.NOTE] 使用しないときは Azure Virtual Machines の料金を最小限に抑えるには、Azure クラシック ポータルで VM をシャット ダウンします。 VM の内部で Windows の電源オプションを使用して VM をシャットダウンしても、VM に対して同じ金額が課金されます。 料金を削減するには、Azure クラシック ポータルで VM をシャットダウンする必要があります。 VM が不要になった場合は、ストレージの課金を避けるには、VM と関連付けられている .vhd ファイルを削除してください。詳細については、FAQ のセクションを参照してください。 [Virtual Machines 料金](http://azure.microsoft.com/pricing/details/virtual-machines)します。

## 詳細情報

### リソース

- SQL Server Business Intelligence と SharePoint 2013 のシングル サーバー配置に関連する類似のコンテンツでは、次を参照してください。 [Azure VM で SQL Server の BI と SharePoint 2013 を作成する Windows PowerShell を使用して](https://msdn.microsoft.com/library/azure/dn385843.aspx)します。

- SQL Server Business Intelligence と SharePoint 2013 のマルチ サーバー展開に関連する類似のコンテンツでは、次を参照してください。 [SQL Server Business Intelligence のデプロイ Azure Virtual Machines で](https://msdn.microsoft.com/library/dn321998.aspx)します。

- SQL Server Business Intelligence Azure Virtual Machines でのデプロイに関連の概要については、次を参照してください。 [SQL Server Business Intelligence Azure Virtual Machines で](virtual-machines-sql-server-business-intelligence.md)します。

- Azure のコンピューティング コストに関する詳細については、の [仮想マシン] タブを参照してください。 [Azure 料金計算ツール](http://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines)します。

### コミュニティ コンテンツ

- スクリプトを使用せず、Reporting Services ネイティブ モードのレポート サーバーを作成する方法の手順については、次を参照してください。 [Hosting SQL Reporting Service Azure 仮想マシンで](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html)します。

### Azure VM の SQL Server に関するその他のリソースへのリンク

[Azure Virtual Machines における SQL Server の概要](virtual-machines-sql-server-infrastructure-services.md)

