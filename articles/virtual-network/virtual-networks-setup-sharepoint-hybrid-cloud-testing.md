<properties 
    pageTitle="SharePoint 2013 ファームのテスト環境 | Microsoft Azure" 
    description="開発または IT プロのテスト用のハイブリッド クラウド環境で 2 層の SharePoint 2013 イントラネット ファームを作成する方法について説明します。" 
    services="virtual-network" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-service-management"/>

<tags 
    ms.service="virtual-network" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="Windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/10/2015" 
    ms.author="josephd"/>

# テスト用のハイブリッド クラウドでの SharePoint イントラネット ファームの設定

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。
 

このトピックでは、Microsoft Azure でホストされる SharePoint イントラネット ファームをテストするためにハイブリッド クラウド環境を作成する手順について説明します。 完成すると次のような構成になります。

![](./media/virtual-networks-setup-sharepoint-hybrid-cloud-testing/CreateSPFarmHybridCloud_3.png)
 
この構成では、インターネット上の自分の場所から Azure 運用環境の SharePoint をシミュレートします。 構成は次のとおりです。

- 簡略化されたオンプレミス ネットワーク (Corpnet サブネット)。
- Microsoft Azure でホストされているクロスプレミスの仮想ネットワーク (TestVNET)
- サイト間 VPN 接続
- TestVNET 仮想ネットワーク内に 2 層 SharePoint ファームとセカンダリ ドメイン コントローラー

この構成を基盤や共通の出発点にして以下を実行できます。

- ハイブリッド クラウド環境の SharePoint イントラネット ファームでアプリケーションを開発してテストする。
- このハイブリッド クラウドをベースとした IT ワークロードのテストを実行する。

このハイブリッド クラウド テスト環境を設定する手順は、大きく次の 3 つのフェーズに分かれています。

1.  テスト用のハイブリッド クラウド環境を設定する。
2.  SQL Server コンピューター (SQL1) を構成する。
3.  SharePoint サーバー (SP1) を構成する。

無料試用版にサインアップすることができます、Azure のサブスクリプションがあるない場合 [Azure](http://azure.microsoft.com/pricing/free-trial/)します。 MSDN サブスクリプションがある場合は、次を参照してください。 [MSDN サブスクライバー向けの Azure 特典](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)します。

## フェーズ 1: ハイブリッド クラウド環境を設定する

」の指示に従って、 [テスト用のハイブリッド クラウド環境をセットアップ](virtual-networks-setup-hybrid-cloud-environment-testing.md) トピックです。 このテスト環境では Corpnet サブネット上に APP1 サーバーを配置する必要がないため、シャットダウンしてかまいません。

現在の構成は次のようになります。

![](./media/virtual-networks-setup-sharepoint-hybrid-cloud-testing/CreateSPFarmHybridCloud_1.png)

> [AZURE.NOTE] フェーズ 1 の場合もシミュレートされたハイブリッド クラウド テスト環境を設定することができます。 参照してください [テスト用のシミュレートされたハイブリッド クラウド環境をセットアップ](virtual-networks-setup-simulated-hybrid-cloud-environment-testing.md) 手順については、です。
 
## フェーズ 2: SQL Server コンピューター (SQL1) を構成する

DC2 コンピューターが起動されていない場合は、Microsoft Azure 管理ポータルから起動します。

まず、CORP\User1 の資格情報を使用して DC2 へのリモート デスクトップ接続を作成します。

次に、SharePoint ファーム管理者アカウントを作成します。 DC2 で管理者レベルの Windows PowerShell プロンプトを開き、次のコマンドを実行します。

    New-ADUser -SamAccountName SPFarmAdmin -AccountPassword (read-host "Set user password" -assecurestring) -name "SPFarmAdmin" -enabled $true -ChangePasswordAtLogon $false

SPFarmAdmin アカウント パスワードを指定するよう求められたら、強力なパスワードを入力し、そのメモを安全な場所に保管します。

続いて、ローカル コンピューターで Azure PowerShell コマンド プロンプトから次のコマンドを実行して、SQL1 用に Azure 仮想マシンを作成します。


    $storageacct="<Name of the storage account for your TestVNET virtual network>"
    $ServiceName="<The cloud service name for your TestVNET virtual network>"
    $cred1=Get-Credential -Message "Type the name and password of the local administrator account for SQL1."
    $cred2=Get-Credential -UserName "CORP\User1" -Message "Now type the password for the CORP\User1 account."
    Set-AzureStorageAccount -StorageAccountName $storageacct
    $image= Get-AzureVMImage | where { $_.ImageFamily -eq "SQL Server 2014 RTM Standard on Windows Server 2012 R2" } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vm1=New-AzureVMConfig -Name SQL1 -InstanceSize Large -ImageName $image
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain "CORP" -DomainUserName "User1" -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain "corp.contoso.com"
    $vm1 | Set-AzureSubnet -SubnetNames TestSubnet
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB 100 -DiskLabel SQLFiles -LUN 0 -HostCaching None
    New-AzureVM -ServiceName $ServiceName -VMs $vm1 -VNetName TestVNET

次に、新しい SQL1 仮想マシンに接続 *ローカル管理者アカウントを使用して*します。

1.  Azure 管理ポータルの左側のウィンドウで、クリックして **仮想マシン**, 、] をクリックし、 **を実行している** SQL1 の [状態] 列にします。
2.  タスク バーで、クリックして **接続**します。 
3.  SQL1.rdp を開くダイアログ ボックスが表示されたら、クリックして **開く**します。
4.  リモート デスクトップ接続のメッセージ ボックスが表示されたら、クリックして **接続**します。
5.  資格情報の入力を求められたら、次の情報を使用します。
    - [名前]: **SQL1\\**[ローカル管理者アカウント名]
    - パスワード: [ローカル管理者アカウントのパスワード]
6.  証明書に関するリモート デスクトップ接続のメッセージ ボックスが表示されたら、クリックして **はい**します。

続いて、基本的な接続テストと SQL Server のトラフィックを許可するように Windows ファイアウォールの規則を構成します。 SQL1 で管理者レベルの Windows PowerShell コマンド プロンプトから次のコマンドを実行します。

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

ping コマンドで IP アドレス 10.0.0.1 からの応答が 4 回成功する必要があります。

次に、余っているデータ ディスクを新しいボリュームとして追加し、ドライブ文字 F: を割り当てます。

1.  サーバー マネージャーの左側のウィンドウで、クリックして **ファイルおよび記憶域サービス**, 、] をクリックし、 **ディスク**します。
2.  コンテンツ ウィンドウでの **ディスク** グループで、[ **disk 2** (で、 **パーティション** に設定 **不明な**)。
3.  クリックして **タスク**, 、] をクリックし、 **新しいボリューム**します。
4.  前に新しいボリューム ウィザードのページを開始でクリックして **次**します。
5.  選択して、サーバーとディスク] ページで、をクリックして **ディスク 2**, 、クリックして **次**します。 メッセージが表示されたら、クリックして **OK**します。
6.  [ボリューム] ページのサイズの指定] をクリックして **次**します。
7.  ドライブ文字またはフォルダーのページへの割り当て、をクリックして **次**します。
8.  クリックして選択し、ファイル システムの設定] ページで、 **次**します。
9.  選択内容の確認] ページをクリックして **作成**します。
10. 完了したら、クリックして **閉じる**します。

SQL1 の Windows PowerShell コマンド プロンプトで、次のコマンドを実行します。

    md f:\Data
    md f:\Log
    md f:\Backup

次に、新しいデータベースとユーザー アカウント アクセス許可に F: ドライブを使用するように SQL Server 2014 を構成します。

1.  スタート画面から次のように入力します。 **SQL Server Management**, 、クリックして **SQL Server 2014 Management Studio**します。
2.   **サーバーへの接続**, 、クリックして **接続**します。
3.  オブジェクト エクスプ ローラーのツリー ペインで右クリックし **SQL1**, 、クリックして **プロパティ**します。
4.   **サーバーのプロパティ** ウィンドウで、をクリックして **データベース設定**します。
5.  検索、 **データベースの既定の場所** し、これらの値を設定します。 
    -  **データ**, 、パスを入力 **f:\Data**します。
    -  **ログ**, 、パスを入力 **f:\Log**します。
    -  **バックアップ**, 、パスを入力 **f:\Backup**します。
    - 新しいデータベースでのみ、これらの場所が使用されます。
6.  クリックして、 **OK** ウィンドウを閉じます。
7.   **オブジェクト エクスプ ローラー** ツリー ウィンドウで、 **セキュリティ**します。
8.  右クリック **ログイン** ] をクリックし、 **新しいログイン**します。
9.   **ログイン名**, 、型 **corp \user1**します。
10.  **サーバーの役割** ] ページで [ **sysadmin**, 、順にクリック **OK**します。
11.  **オブジェクト エクスプ ローラー** ツリー ウィンドウで右クリックし、 **ログイン**, 、クリックして **新しいログイン**します。
12.  **全般** ] ページの [ **ログイン名**, 、型 **\spfarmadmin**します。
13.  **サーバーの役割** ] ページで、[ **dbcreator**, 、順にクリック **OK**します。
14. Microsoft SQL Server Management Studio を閉じます。

現在の構成は次のようになります。

![](./media/virtual-networks-setup-sharepoint-hybrid-cloud-testing/CreateSPFarmHybridCloud_2.png)

 
## フェーズ 3: SharePoint サーバー (SP1) を構成する

まず、ローカル コンピューターの Azure PowerShell コマンド プロンプトで次のコマンドを使用して、SP1 用に Azure 仮想マシンを作成します。

    $ServiceName="<The cloud service name for your TestVNET virtual network>"
    $cred1=Get-Credential -Message "Type the name and password of the local administrator account for SP1."
    $cred2=Get-Credential -UserName "CORP\User1" -Message "Now type the password for the CORP\User1 account."
    $image= Get-AzureVMImage | where { $_.Label -eq "SharePoint Server 2013 Trial" } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vm1=New-AzureVMConfig -Name SP1 -InstanceSize Large -ImageName $image
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.GetNetworkCredential().Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain "CORP" -DomainUserName "User1" -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain "corp.contoso.com"
    $vm1 | Set-AzureSubnet -SubnetNames TestSubnet
    New-AzureVM -ServiceName $ServiceName -VMs $vm1 -VNetName TestVNET

次に、CORP\User1 の資格情報を使用して、SP1 仮想マシンに接続します。

続いて、基本的な接続テストを行うためのトラフィックを許可するよう Windows ファイアウォール規則を構成します。 SP1 で管理者レベルの Windows PowerShell プロンプトから、次のコマンドを実行します。

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc1.corp.contoso.com

ping コマンドで IP アドレス 10.0.0.1 からの応答が 4 回成功する必要があります。

次に、新しい SharePoint ファームと既定のチーム サイト向けに SP1 を構成します。

1.  スタート画面から次のように入力します。 **SharePoint 2013 製品**, 、クリックして **SharePoint 2013 製品構成ウィザード**します。 コンピューターに変更を加えるプログラムを許可するようにメッセージが表示されたら、クリックして **はい**します。
2.  SharePoint 製品のページへようこそ]、をクリックして **次**します。 
3.  一部のサービスが構成中に再起動する必要があることを通知する] ダイアログ ボックスで、[ **はい**します。
4.  [サーバー ファーム] ページへの接続、] をクリックして **新しいサーバー ファームを作成する**, 、順にクリック **次**します。
5.  [構成データベースの設定] ページで、次のように入力します。 **sql1.corp.contoso.com** で **データベース サーバー**, 、型 **\spfarmadmin** で **ユーザー名**, 、に SPFarmAdmin アカウント パスワードを入力 **パスワード**, 、] をクリックし、 **次**します。
6.  [ファームのセキュリティ設定の指定] ページで、次のように入力します。 **P@ssphrase** 両方で **パスフレーズ** と **パスフレーズの確認**, 、] をクリックし、 **次**します。
7.  SharePoint サーバーの全体管理 Web アプリケーションの構成] ページで、クリックして **次**します。
8.  完了 SharePoint 製品構成ウィザード ページをクリックして **次**します。 SharePoint 製品構成ウィザードは、完了するまで数分かかる場合があります。
9.  [構成成功] ページで、次のようにクリックします。 **完了**します。 完了後、Internet Explorer を起動すると、[ファーム構成の初期設定ウィザード] というタブが表示されます。
10.  **ヘルプ、SharePoint の品質向上** ダイアログ ボックスで、をクリックして **いいえ、参加する協力しません**, 、] をクリックし、 **[ok]**します。
11.  **SharePoint ファームを構成する方法ですか?**, をクリックして **ウィザードを開始する**です。
12. SharePoint ファームの構成] ページで、 **サービス アカウント**, をクリックして **管理アカウントの既存の**です。
13.  **サービス**, 、ボックス以外のすべてのチェック ボックスをオフに横に **状態サービス**, 、] をクリックし、 **次**します。 完了するまで、[ただいま処理中です] ページがしばらく表示されることがあります。
14. [サイト コレクションの作成] ページで **タイトルと説明**, 、型 **Contoso Corporation** で **タイトル**, 、URL を指定して **http://sp1**/、順にクリック **[ok]**します。 完了するまで、[ただいま処理中です] ページがしばらく表示されることがあります。 これにより、URL http://sp1 にチーム サイトが作成されます。
15. [ファーム構成ウィザードのページが完了すると、これをクリックして **完了**します。 Internet Explorer のタブに SharePoint 2013 サーバーの全体管理サイトが表示されます。
16. CORP\User1 アカウントの資格情報を使用して CLIENT1 コンピューターにログオンし、Internet Explorer を起動します。
17. アドレス バーに「 **http://sp1/** ENTER キーを押します。 Contoso Corporation の SharePoint チーム サイトが表示されます。 サイトが表示されるまで時間がかかることがあります。

現在の構成は次のようになります。

![](./media/virtual-networks-setup-sharepoint-hybrid-cloud-testing/CreateSPFarmHybridCloud_3.png)
 
これで、ハイブリッド クラウド環境の SharePoint イントラネット ファームをテストする準備が整いました。

## その他のリソース

[Azure インフラストラクチャ サービスでの SharePoint](http://msdn.microsoft.com/library/azure/dn275955.aspx)

[SharePoint サーバー ファーム](../virtual-machines/virtual-machines-sharepoint-farm-azure-preview.md)

[テスト用のハイブリッド クラウド環境の設定](virtual-networks-setup-hybrid-cloud-environment-testing.md)

[テスト用のハイブリッド クラウドでの Web ベース LOB アプリケーションの設定](virtual-networks-setup-lobapp-hybrid-cloud-testing.md)

[テスト用のハイブリッド クラウドでの Office 365 ディレクトリ同期 (DirSync) の設定](virtual-networks-setup-dirsync-hybrid-cloud-testing.md)

[テスト用のシミュレートされたハイブリッド クラウド環境の設定](virtual-networks-setup-simulated-hybrid-cloud-environment-testing.md)

[Azure ハイブリッド クラウド テスト環境](../virtual-machines/virtual-machines-hybrid-cloud-test-environments.md)

[Azure インフラストラクチャ サービス実装ガイドライン](../virtual-machines/virtual-machines-infrastructure-services-implementation-guidelines.md)
 

