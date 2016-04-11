<properties
    pageTitle="SharePoint Server 2013 ファームのフェーズ 3 | Microsoft Azure"
    description="コンピューターと SQL Server クラスターを作成し、可用性グループを有効にします。"
    documentationCenter=""
    services="virtual-machines"
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="Windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/11/2015"
    ms.author="josephd"/>

# SharePoint イントラネット ファーム ワークロードのフェーズ 3: SQL Server インフラストラクチャの構成

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。

イントラネット専用 SharePoint 2013 ファームと SQL Server AlwaysOn 可用性グループを Azure インフラストラクチャ サービスにデプロイする作業のこのフェーズでは、2 台の SQL Server コンピューターとクラスター マジョリティ ノード コンピューターを作成および構成し、Windows Server クラスターに統合します。

進むには、このフェーズを完了する必要があります [フェーズ 4](virtual-machines-workload-intranet-sharepoint-phase4.md)します。 参照してください [SharePoint と SQL Server AlwaysOn 可用性グループで、Azure のデプロイ](virtual-machines-workload-intranet-sharepoint-overview.md) の全フェーズ。

> [AZURE.NOTE] 次の手順は、Azure イメージ ギャラリーで SQL Server イメージを使用し、SQL Server のライセンスを使用するための継続的な費用を請求します。 Azure で仮想マシンを作成し、独自の SQL Server のライセンスをインストールすることもできますが、Azure 仮想マシンを含む、仮想マシンで SQL Server のライセンスを使用するには、ソフトウェア アシュアランスおよびライセンス モビリティが必要になります。 仮想マシンに SQL Server をインストールする方法の詳細については、次を参照してください。 [SQL Server のインストール](https://msdn.microsoft.com/library/bb500469.aspx)します。

## Azure での SQL Server クラスターの仮想マシンの作成

SQL サーバー仮想マシンは 2 つあります。 一方の SQL サーバーには、可用性グループのプライマリ データベース レプリカが格納されます。 もう 1 つの SQL サーバーには、セカンダリ バックアップ レプリカが格納されます。 このバックアップは、高可用性を確保するために用意されます。 追加の仮想マシンは、クラスター マジョリティ ノード用です。

PowerShell コマンドの次のブロックを使用して、3 つのサーバーの仮想マシンを作成します。 削除して、変数の値を指定します < と > 文字です。 この PowerShell コマンド セットでは、次の表の値を使用します。

- 表 M (仮想マシン)
- 表 V (仮想ネットワーク設定)
- 表 S (サブネット)
- 表 ST (ストレージ アカウント)
- 表 A (可用性セット)

表 M が定義されていることを思い出してください [フェーズ 2: ドメイン コント ローラーの構成](virtual-machines-workload-intranet-sharepoint-phase2.md) し、表 V、S、ST、および A [フェーズ 1: Azure を構成する](virtual-machines-workload-intranet-sharepoint-phase1.md)です。

> [AZURE.NOTE] 次のコマンドは、1.0 以降、Azure PowerShell を使用を設定します。 詳細については、次を参照してください。 [Azure PowerShell 1.0](https://azure.microsoft.com/blog/azps-1-0/)します。

適切な値をすべて指定したら、Azure PowerShell コマンド プロンプトでそのブロックを実行します。


    # Set up key variables
    $rgName="<your resource group name>"
    $locName="<Azure location of your resource group>"
    $saName="<Table ST – Item 1 – Storage account name column>"
    $vnetName="<Table V – Item 1 – Value column>"
    $avName="<Table A – Item 2 – Availability set name column>"
    
    # Create the first SQL Server virtual machine
    $vmName="<Table M – Item 3 - Virtual machine name column>"
    $vmSize="<Table M – Item 3 - Minimum size column>"
    $vnet=Get-AzureRMVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $nic=New-AzureRMNetworkInterface -Name ($vmName +"-NIC") -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[1].Id
    $avSet=Get-AzureRMAvailabilitySet –Name $avName –ResourceGroupName $rgName 
    $vm=New-AzureRMVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
    
    $diskSize=<size of the extra disk for SQL data in GB>
    $diskLabel="<the label on the disk>"
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name $diskLabel -DiskSizeInGB $diskSize -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the first SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Enterprise -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm
    
    # Create the second SQL Server virtual machine
    $vmName="<Table M – Item 4 - Virtual machine name column>"
    $vmSize="<Table M – Item 4 - Minimum size column>"
    $nic=New-AzureRMNetworkInterface -Name ($vmName +"-NIC") -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[1].Id
    $vm=New-AzureRMVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
    
    $diskSize=<size of the extra disk for SQL data in GB>
    $diskLabel="<the label on the disk>"
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name $diskLabel -DiskSizeInGB $diskSize -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the second SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Enterprise -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm
    
    # Create the cluster majority node server
    $vmName="<Table M – Item 5 - Virtual machine name column>"
    $vmSize="<Table M – Item 5 - Minimum size column>"
    $nic=New-AzureRMNetworkInterface -Name ($vmName +"-NIC") -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[1].Id
    $vm=New-AzureRMVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the cluster majority node server." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

> [AZURE.NOTE] これらの仮想マシンでは、イントラネット アプリケーションであるために、いない、割り当てられているパブリック IP アドレスまたは DNS ドメイン名のラベル、インターネットに公開します。 ただし、これは Azure ポータルから接続できないことを意味します。  **接続** ] ボタンは使用できません、仮想マシンのプロパティを表示するとします。 プライベート IP アドレスまたはイントラネット DNS 名を使用して仮想マシンに接続する場合は、リモート デスクトップ接続のアクセサリまたは別のリモート デスクトップ ツールを使用します。

## SQL Server コンピューターの構成

SQL Server を実行する仮想マシンのそれぞれについて、好みのリモート デスクトップ クライアントを使用して、リモート デスクトップ接続を作成します。 仮想マシンのイントラネット DNS 名またはコンピューター名と、ローカル管理者アカウントの資格情報を使用します。

Windows PowerShell プロンプトで次のコマンドを実行し、SQL Server を実行する各仮想マシンを適切な AD DS ドメインに参加させます。

    $domName="<AD DS domain name to join, such as corp.contoso.com>"
    Add-Computer -DomainName $domName
    Restart-Computer

Add-Computer コマンドを入力した後、ドメイン アカウントの資格情報を指定する必要があることに注意してください。

仮想マシンの再起動後、ローカル管理者特権を持つアカウントを使用して再接続します。

SQL サーバーごとに次の手順を実行します。

1. 使用して、 [リモート デスクトップ接続プロシージャを使用して仮想マシンにログオン](virtual-machines-workload-intranet-sharepoint-phase2.md#logon) ローカル管理者アカウントの資格情報を使用してログオンします。

2. 使用して、 [空のディスクの手順を初期化するために](virtual-machines-workload-intranet-sharepoint-phase2.md#datadisk) を 2 回追加データ ディスクを追加する各 SQL server 用に 1 回です。

3. Windows PowerShell コマンド プロンプトで次のコマンドを実行します。

        md f:\Data
        md f:\Log
        md f:\Backup

4. 使用して、 [接続プロシージャをテストする](virtual-machines-workload-intranet-sharepoint-phase2.md#testconn) を組織のネットワーク上の場所への接続をテストします。 この手順により、DNS 名前解決が正しく機能していること (仮想ネットワークの DNS サーバーで仮想マシンが正しく構成されていること) と、クロスプレミス仮想ネットワーク間でパケットを送受信できることが確認されます。

次の手順を 2 回 (SQL サーバーごとに 1 回) 実行して、新しいデータベースおよびアカウントとアクセス許可に F: ドライブを使用するように SQL サーバーを構成します。

1.  [スタート画面で、次のように入力します。 **SQL Studio**, 、クリックして **SQL Server 2014 Management Studio**します。
2.   **サーバーへの接続**, 、クリックして **接続**します。
3.  左側のウィンドウで最上位ノードを右クリックし、コンピューター後という名前の既定のインスタンス: 順にクリック **プロパティ**します。
4.   **サーバーのプロパティ**, 、クリックして **データベース設定**します。
5.   **データベースの既定の場所**, 、次の値を設定します。
-  **データ**, 、パスを設定 **f:\Data**します。
-  **ログ**, 、パスを設定 **f:\Log**します。
-  **バックアップ**, 、パスを設定 **f:\Backup**します。
- 新しいデータベースでのみ、これらの場所が使用されます。
6.  クリックして **OK** ウィンドウを閉じます。
7.  左側のペインで、展開、 **セキュリティ フォルダー**します。
8.  右クリック **ログイン**, 、クリックして **新しいログイン**します。
9.   **ログイン名**, 、型 *ドメイン*\sp_farm_db (を *ドメイン* sp_farm_db アカウントが作成したドメインの名前を指定します)。
10. [ **ページの選択**, 、] をクリックして **サーバーの役割**, 、] をクリックして **sysadmin**, 、クリックして **[ok]**します。
11. SQL Server 2014 Management Studio を閉じます。

次の手順を 2 回 (SQL サーバーごとに 1 回) 実行して、sp_farm_db アカウントを使用したリモート デスクトップ接続を許可します。

1.  スタート画面で右クリックして **PC**, 、クリックして **プロパティ**します。
2.   **システム** ウィンドウで、をクリックして **リモート設定**します。
3.   **リモート デスクトップ** ] をクリックして **ユーザーの選択**, 、クリックして **追加**します。
4.   **選択するオブジェクト名を入力**, 、「[domain]**\sp_farm_db**, 順にクリック **OK** 3 回です。

SQL Server には、クライアントがデータベース サーバーへのアクセスに使用するポートが必要です。 また、SQL Server Management Studio に接続するためのポートと、高可用性グループを管理するためのポートも必要です。 次に、管理者レベルの Windows PowerShell コマンド プロンプトで次のコマンドを 2 回 (SQL サーバーごとに 1 回) 実行して、SQL サーバーへの受信トラフィックを許可するファイアウォール ルールを追加します。

    New-NetFirewallRule -DisplayName "SQL Server ports 1433, 1434, and 5022" -Direction Inbound –Protocol TCP –LocalPort 1433,1434,5022 -Action Allow

SQL Server 仮想マシンごとに、ローカル管理者としてサインアウトします。

Azure で SQL Server のパフォーマンスを最適化する方法の詳細については、次を参照してください。 [Azure 仮想マシンにおける SQL Server のパフォーマンスのベスト プラクティス](virtual-machines-sql-server-performance-best-practices.md)します。 SharePoint ファームのストレージ アカウントの 地理冗長ストレージ (GRS) を無効にし、記憶域を使用して IOP を最適化することもできます。

## クラスター マジョリティ ノード サーバーの構成

使用して、 [リモート デスクトップ接続プロシージャを使用して仮想マシンにログオン](virtual-machines-workload-intranet-sharepoint-phase2.md#logon) 、クラスター マジョリティ ノード用のドメイン アカウントの資格情報を使用してログオンします。

クラスター マジョリティ ノードで使用して、 [接続プロシージャをテストする](virtual-machines-workload-intranet-sharepoint-phase2.md#testconn) を組織のネットワーク上の場所への接続をテストします。

## Windows Server クラスターの作成

SQL Server AlwaysOn 可用性グループでは、Windows Server の Windows Server フェールオーバー クラスタリング (WSFC) 機能を利用します。 この機能により、複数のコンピューターが 1 つのグループとしてクラスターに参加できるようになります。 1 台のコンピューターで障害が発生すると、別のコンピューターがすぐに機能を引き継ぎます。 したがって、最初の作業として、参加するすべてのコンピューターでフェールオーバー クラスタリング機能を有効にします。次のコンピューターで作業を行います。

- プライマリ SQL サーバー
- セカンダリ SQL サーバー
- クラスター マジョリティ ノード

フェールオーバー クラスターには少なくとも 3 つの VM が必要です。 2 台のコンピューターで SQL Server をホストします。 2 番目の SQL Server VM は同期セカンダリ レプリカであり、プライマリ コンピューターで障害が発生した場合にデータの損失を防ぎます。 3 台目のコンピューターでは、SQL Server をホストする必要はありません。 クラスター マジョリティ ノードが、WSFC でクォーラムを提供します。 WSFC クラスターではクォーラムを利用して正常性を監視するため、WSFC クラスターがオンラインであるためには、常に過半数存在する必要があります。 クラスターにコンピューターが 2 台しかない場合、2 台のうち 1 台で障害が発生すると、過半数に届かなくなります。 詳細については、次を参照してください。 [WSFC クォーラム モードと投票の構成 (SQL Server)](http://msdn.microsoft.com/library/hh270280.aspx)します。

両方の SQL サーバー コンピューターとクラスター マジョリティ ノードに対して、管理者レベルの Windows PowerShell コマンド プロンプトで次のコマンドを実行します。

    Install-WindowsFeature Failover-Clustering -IncludeManagementTools

現在、Azure の DHCP によって RFC に準拠していない動作が発生するため、Windows Server フェールオーバー クラスタリング (WSFC) クラスターの作成に失敗する可能性があります。 詳細については、「Azure Virtual Machines 内の SQL Server の高可用性と障害復旧」で、「Azure ネットワークでの WSFC クラスターの動作」を検索してください。 ただし、回避策があります。 次の手順に従って、クラスターを作成します。

1.  プライマリ SQL Server 仮想マシンにログオン、 **sp_install** アカウントです。
2.  スタート画面から次のように入力します。 **フェールオーバー**, 、クリックして **フェールオーバー クラスター マネージャー**します。
3.  左側のウィンドウで右クリック **フェールオーバー クラスター マネージャー**, 、クリックして **クラスターの作成**します。
4.  開始する前に] ページで、クリックして **次**します。
5.  [サーバーの選択] ページで、プライマリ SQL Server コンピューターの名前を入力します] をクリックして **追加**, 、クリックして **次**します。
6.  [検証の警告] ページで、**[いいえ、このクラスターに Microsoft のサポートは必要ありませんので、検証テストを実行しません。次をクリックすると、クラスターの作成を続行**, 、クリックして **次**します。
7.  クラスター] ページを管理するためのアクセス ポイント、 **クラスター名** テキスト ボックスが、クラスターの名前を入力し、クリックして **次**します。
8.  確認ページで、クリックして **次** クラスターの作成を開始します。
9.  [概要] ページをクリックして **完了**します。
10. 左ウィンドウで新しいクラスターをクリックします。  **クラスター コア リソース** セクションのコンテンツ ウィンドウで、サーバー クラスター名を開きます。  **IP アドレス** リソースが表示される、 **失敗** 状態です。 クラスターにコンピューター自体と同じ IP アドレスが割り当てられているため、IP アドレス リソースをオンラインにすることができません。 その結果、アドレスの重複が発生します。
11. 障害が発生したを右クリックし **IP アドレス** リソース、およびクリック **プロパティ**します。
12.  **IP アドレスのプロパティ** ] ダイアログ ボックスをクリックして **静的 IP アドレス**です。
13. SQL server が、配置されているサブネットに対応するアドレスの範囲内の未使用の IP を入力し、クリックして **OK**します。
14. 障害が発生した IP アドレス リソースを右クリックし、 **オンライン**します。 両方のリソースがオンラインになるまで待ちます。 クラスター名リソースがオンラインになると、新しい Active Directory (AD) コンピューター アカウントでドメイン コントローラーが更新されます。 この AD アカウントは、後で可用性グループのクラスター化サービスを実行する際に使用されます。
15. AD アカウントが作成されたので、クラスター名をオフラインにします。 クラスター名を右クリックして **クラスター コア リソース**, 、クリックして **オフライン**します。
16. クラスター IP アドレスを削除するには、右クリックし **IP アドレス**, 、] をクリックして **削除**, 、順にクリック **はい** 入力を求められたらします。 クラスター リソースは、IP アドレス リソースに依存しているため、オンラインにすることはできなくなります。 ただし、可用性グループは、適切に機能するために、クラスター名と IP アドレスのどちらにも依存しません。 そのため、クラスター名をオフラインのままにしておくことができます。
17. 残りのノードをクラスターに追加するには、左側のウィンドウでクラスター名を右クリックしクリックして **ノードの追加**します。
18. 開始する前に] ページで、クリックして **次**します。
19. [サーバーの選択] ページで、名前を入力し、クリックして **追加** 、セカンダリ SQL サーバーとクラスター マジョリティ ノードをクラスターに追加します。 2 台のコンピューターを追加したら、クリックして **次**します。
コンピューターを追加できず、"リモート レジストリは実行されていません" というエラー メッセージが表示された場合は、次の操作を行います。 コンピューターにログオンし、サービス スナップイン (services.msc) を開いて、リモート レジストリを有効にします。 詳細については、次を参照してください。 [リモート レジストリ サービスに接続できない](http://technet.microsoft.com/library/bb266998.aspx)します。
20. [検証の警告] ページで、**[いいえ、このクラスターに Microsoft のサポートは必要ありませんので、検証テストを実行しません。次をクリックすると、クラスターの作成を続行**, 、クリックして **次**します。
21. [確認] ページをクリックして **次**します。
22. [概要] ページをクリックして **完了**します。
23. 左のウィンドウで **ノード**します。 3 台のコンピューターがすべて表示されます。

## AlwaysOn 可用性グループの有効化

次に、SQL Server 構成マネージャーを使用して、AlwaysOn 可用性グループを有効にします。 SQL Server の可用性グループは、Azure 可用性セットとは異なります。 可用性グループには、可用性が高く回復可能なデータベースが含まれます。 Azure 可用性セットでは、さまざまな障害ドメインに仮想マシンを割り当てます。 障害ドメインの詳細については、次を参照してください。 [仮想マシンの可用性を管理](virtual-machines-manage-availability.md)します。

SQL Server の AlwaysOn 可用性グループを有効にするには、次の手順に従います。

1.  プライマリ SQL server を使用して、ログオン、 **sp_farm_db** アカウント、または SQL server の sysadmin サーバー ロールのあるその他のアカウントです。
2.  [スタート画面で、次のように入力します。 **SQL Server の構成**, 、クリックして **SQL Server 構成マネージャー**します。
3.  左のウィンドウで **SQL Server サービス**します。
4.  コンテンツ ウィンドウでダブルクリック **SQL Server (MSSQLSERVER)**します。
5.   **SQL Server (MSSQLSERVER) プロパティ**, 、] をクリックして、 **AlwaysOn 高可用性** ] タブで [ **AlwaysOn 可用性グループを有効にする**, 、] をクリックして **適用**, 、] をクリックし、 **[ok]** 入力を求められたらします。 プロパティ ウィンドウはまだ閉じないでください。
6.  仮想マシンの管理-可用性] タブをクリックし、[ドメイン] を入力**\sqlservice** で **アカウント名**します。 Sqlservice アカウントのパスワードを入力 **パスワード** と **パスワードの確認入力**, 、] をクリックし、 **OK**します。
7.  メッセージ ウィンドウで [ **はい** SQL Server サービスを再起動します。
8.  セカンダリ SQL サーバーにログオンし、このプロセスを繰り返します。

次の図は、このフェーズが問題なく完了したときの構成を示しています。プレースホルダーにコンピューター名が示されています。

![](./media/virtual-machines-workload-intranet-sharepoint-phase3/workload-spsqlao_03.png)

## 次のステップ

- 使用 [フェーズ 4](virtual-machines-workload-intranet-sharepoint-phase4.md) このワークロードの構成を続行します。

