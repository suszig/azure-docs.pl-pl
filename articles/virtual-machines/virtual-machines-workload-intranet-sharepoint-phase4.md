<properties
    pageTitle="SharePoint Server 2013 ファームのフェーズ 4 | Microsoft Azure"
    description="SharePoint サーバーの仮想マシンおよび新しい SharePoint ファームを作成します。"
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

# SharePoint イントラネット ファーム ワークロードのフェーズ 4: SharePoint サーバーの構成

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。

イントラネット専用 SharePoint 2013 ファームと SQL Server AlwaysOn 可用性グループを Azure インフラストラクチャ サービスにデプロイする作業のこのフェーズでは、SharePoint ファームのアプリケーション層と Web 層を構築し、SharePoint 構成ウィザードを使用してファームを作成します。

進むには、このフェーズを完了する必要があります [フェーズ 5](virtual-machines-workload-intranet-sharepoint-phase5.md)します。 参照してください [SharePoint と SQL Server AlwaysOn 可用性グループで、Azure のデプロイ](virtual-machines-workload-intranet-sharepoint-overview.md) の全フェーズ。

## Azure での SharePoint サーバー仮想マシンの作成

SharePoint サーバー仮想マシンは 4 つあります。 2 つはフロントエンド Web サーバー用であり、残りの 2 つは SharePoint アプリケーションの管理およびホスト用です。 各層の 2 つの SharePoint サーバーによって高可用性が実現されます。

最初に、クライアント トラフィックが、Azure によって 2 つのフロントエンド Web サーバーに均等に配分されるように、内部ロード バランサーを構成します。 この場合、名前とサブネットのアドレス空間 (Azure 仮想ネットワークに割り当てたアドレス空間) から取得した独自の IP アドレスで構成される内部負荷分散インスタンスを指定する必要があります。 

> [AZURE.NOTE] 次のコマンドは、1.0 以降、Azure PowerShell を使用を設定します。 詳細については、次を参照してください。 [Azure PowerShell 1.0](https://azure.microsoft.com/blog/azps-1-0/)します。

削除して、変数の値を指定します < と > 文字です。 次の Azure PowerShell コマンド セットでは、以下の表の値を使用します。

- 表 M (仮想マシン)
- 表 V (仮想ネットワーク設定)
- 表 S (サブネット)
- 表 ST (ストレージ アカウント)
- 表 A (可用性セット)

表 M が定義されていることを思い出してください [フェーズ 2: ドメイン コント ローラーの構成](virtual-machines-workload-intranet-sharepoint-phase2.md) し、表 V、S、ST、および A [フェーズ 1: Azure を構成する](virtual-machines-workload-intranet-sharepoint-phase1.md)です。

適切な値をすべて指定したら、Azure PowerShell コマンド プロンプトでそのブロックを実行します。

    # Set up key variables
    $rgName="<resource group name>"
    $locName="<Azure location of your resource group>"
    $vnetName="<Table V – Item 1 – Value column>"
    $privIP="<available IP address on the subnet>"
    $vnet=Get-AzureRMVirtualNetwork -Name $vnetName -ResourceGroupName $rgName

    $frontendIP=New-AzureRMLoadBalancerFrontendIpConfig -Name SharePointWebServers-LBFE -PrivateIPAddress $privIP -SubnetId $vnet.Subnets[1].Id
    $beAddressPool=New-AzureRMLoadBalancerBackendAddressPoolConfig -Name SharePointWebServers-LBBE

    # This example assumes unsecured (HTTP-based) web traffic to the web servers.
    $healthProbe=New-AzureRMLoadBalancerProbeConfig -Name WebServersProbe -Protocol "TCP" -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    $lbrule=New-AzureRMLoadBalancerRuleConfig -Name "WebTraffic" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol "TCP" -FrontendPort 80 -BackendPort 80
    New-AzureRMLoadBalancer -ResourceGroupName $rgName -Name "SharePointWebServersInAzure" -Location $locName -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe -FrontendIpConfiguration $frontendIP

次に、SharePoint ファームの完全修飾ドメイン名 (spfarm.corp.contoso.com など) を、内部ロード バランサーに割り当てられている IP アドレス (前の Azure PowerShell コマンド ブロックの $privIP の値) に解決する DNS アドレス レコードを、組織の内部 DNS インフラストラクチャに追加します。

Azure PowerShell コマンドの次のブロックを使用して、4 つの SharePoint サーバーの仮想マシンを作成します。 適切な値をすべて指定したら、Azure PowerShell コマンド プロンプトでそのブロックを実行します。

    # Set up key variables
    $rgName="<resource group name>"
    $locName="<Azure location of your resource group>"
    $saName="<Table ST – Item 2 – Storage account name column>"
    $vnetName="<Table V – Item 1 – Value column>"
    $avName="<Table A – Item 3 – Availability set name column>"
    
    # Create the first application server
    $vmName="<Table M – Item 6 - Virtual machine name column>"
    $vmSize="<Table M – Item 6 - Minimum size column>"
    $vnet=Get-AzureRMVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $nic=New-AzureRMNetworkInterface -Name ($vmName +"-NIC") -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[1].Id
    $avSet=Get-AzureRMAvailabilitySet –Name $avName –ResourceGroupName $rgName 
    $vm=New-AzureRMVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the first application server." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSharePoint -Offer MicrosoftSharePointServer -Skus 2013 -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

    # Create the second application server
    $vmName="<Table M – Item 7 - Virtual machine name column>"
    $vmSize="<Table M – Item 7 - Minimum size column>"
    $vnet=Get-AzureRMVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $nic=New-AzureRMNetworkInterface -Name ($vmName +"-NIC") -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[1].Id
    $avSet=Get-AzureRMAvailabilitySet –Name $avName –ResourceGroupName $rgName 
    $vm=New-AzureRMVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the second application server." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSharePoint -Offer MicrosoftSharePointServer -Skus 2013 -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

    # Change the availability set
    $avName="<Table A – Item 4 – Availability set name column>"

    # Set up key variables
    $beSubnetName="<Table S - Item 2 - Name column>"
    $webLB=Get-AzureRMLoadBalancer -ResourceGroupName $rgName -Name "SharePointWebServersInAzure"   
    $vnet=Get-AzureRMVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $backendSubnet=Get-AzureRMVirtualNetworkSubnetConfig -Name $beSubnetName -VirtualNetwork $vnet
    
    # Create the first front end web server virtual machine
    $vmName="<Table M – Item 8 - Virtual machine name column>"
    $vmSize="<Table M – Item 8 - Minimum size column>"
    $nic=New-AzureRMNetworkInterface -Name ($vmName + "-NIC") -ResourceGroupName $rgName -Location $locName -Subnet $backendSubnet -LoadBalancerBackendAddressPool $webLB.BackendAddressPools[0]
    $avSet=Get-AzureRMAvailabilitySet -Name $avName –ResourceGroupName $rgName 
    $vm=New-AzureRMVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the first front end web server." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSharePoint -Offer MicrosoftSharePointServer -Skus 2013 -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm
    
    # Create the second front end web server virtual machine
    $vmName="<Table M – Item 9 - Virtual machine name column>"
    $vmSize="<Table M – Item 9 - Minimum size column>"
    $nic=New-AzureRMNetworkInterface -Name ($vmName + "-NIC") -ResourceGroupName $rgName -Location $locName -Subnet $backendSubnet -LoadBalancerBackendAddressPool $webLB.BackendAddressPools[0]
    $vm=New-AzureRMVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the second front end web server." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSharePoint -Offer MicrosoftSharePointServer -Skus 2013 -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

> [AZURE.NOTE] これらの仮想マシンでは、イントラネット アプリケーションであるために、いない、割り当てられているパブリック IP アドレスまたは DNS ドメイン名のラベル、インターネットに公開します。 ただし、これは Azure ポータルから接続できないことを意味します。  **接続** ] ボタンは使用できません、仮想マシンのプロパティを表示するとします。

好みのリモート デスクトップ クライアントを使用し、仮想マシンのそれぞれへのリモート デスクトップ接続を作成します。 仮想マシンのイントラネット DNS 名またはコンピューター名と、ローカル管理者アカウントの資格情報を使用します。

次に、Windows PowerShell プロンプトで次のコマンドを実行し、各仮想マシンを適切な Active Directory ドメインに参加させます。

    $domName="<Active Directory domain name to join, such as corp.contoso.com>"
    Add-Computer -DomainName $domName
    Restart-Computer

メモを入力した後のドメイン アカウントの資格情報を指定する必要があります、 **Add-computer** コマンドです。

使用して再起動した後、 [リモート デスクトップ接続プロシージャを使用して仮想マシンにログオン](virtual-machines-workload-intranet-sharepoint-phase2.md#logon) 4 回、1 回、SharePoint サーバーごとに [Domain] \sp_farm_db アカウントを使用してログオンする資格情報します。 これらの資格情報を作成した [フェーズ 2: ドメイン コント ローラー構成](virtual-machines-workload-intranet-sharepoint-phase2.md)します。

使用して、 [接続プロシージャをテストする](virtual-machines-workload-intranet-sharepoint-phase2.md#testconn) 組織のネットワーク上の場所への接続をテストする各 SharePoint サーバーに対して 1 回の 4 回です。

> [AZURE.NOTE] SharePoint サーバーは、SharePoint Server 2013 試用版イメージから作成されます。 インストールを変換して SharePoint Server 2013 の Standard エディションまたは Enterprise エディションの製品版またはボリューム ライセンス キーを使用する必要があります。 

## SharePoint ファームの構成

ファームの最初の SharePoint サーバーを構成するには、次の手順に従います。

1.  最初の SharePoint アプリケーション サーバーのデスクトップで、ダブルクリック **SharePoint 2013 製品構成ウィザード**します。 プログラムはコンピューターに変更を加え、をクリックするように求められたらと **はい**します。
2.   **SharePoint 製品へようこそ** ] ページで [ **次**します。
3.  A **SharePoint 製品構成ウィザード** サービス (IIS) などの再起動またはリセット行われることを警告するダイアログが表示されます。 クリックして **はい**します。
4.   **サーバー ファームへの接続** ] ページで、[ **新しいサーバー ファームを作成する**, 、] をクリックし、 **次**します。
5.   **構成データベースの設定** ページ。
 -  **データベース サーバー**, 、プライマリ データベース サーバーの名前を入力します。
 -  **ユーザー名**, 、[ドメイン] を入力**\sp_farm_db** (で作成した [フェーズ 2: ドメイン コント ローラー構成](virtual-machines-workload-intranet-sharepoint-phase2.md))。 sp_farm_db アカウントには、データベース サーバーに対する sysadmin 権限があります。
 -  **パスワード**, 、sp_farm_db アカウントのパスワードを入力します。
6.  クリックして **次**します。
7.   **ファームのセキュリティ設定の指定** ] ページで、パスフレーズを 2 回入力します。 後で参照できるように、パスフレーズを記録して、安全な場所に保管しておきます。 クリックして **次**します。
8.   **SharePoint サーバーの全体管理 Web アプリケーションの構成** ] ページで [ **次**します。
9.   **SharePoint 製品構成ウィザードの完了** ページが表示されます。 クリックして **次**します。
10.  **SharePoint 製品の構成中** ページが表示されます。 構成プロセスが完了するまで約 8 分待ちます。
11. ファームが正常に構成されているしたら **完了**します。 新しい管理 Web サイトが開始されます。
12. SharePoint ファームの構成を開始するクリックして **ウィザードを開始する**です。

2 番目の SharePoint アプリケーション サーバーと、2 つのフロントエンド Web サーバーで、次の手順を実行します。

1.  ダブルクリックして、デスクトップから **SharePoint 2013 製品構成ウィザード**します。 プログラムはコンピューターに変更を加え、をクリックするように求められたらと **はい**します。
2.   **SharePoint 製品へようこそ** ] ページで [ **次**します。
3.  A **SharePoint 製品構成ウィザード** サービス (IIS) などの再起動またはリセット行われることを警告するダイアログが表示されます。 クリックして **はい**します。
4.   **サーバー ファームへの接続** ] ページで [ **既存のサーバー ファームへの接続**, 、] をクリックし、 **次**します。
5.   **構成データベースの設定** ] ページで、プライマリ データベース サーバーの名前を入力 **データベース サーバー**, 、クリックして **データベース名の取得**します。
6.  クリックして **SharePoint_Config** データベース名の一覧をクリックし **次**します。
7.   **ファームのセキュリティ設定の指定** ] ページで、前の手順のパスフレーズを入力します。 クリックして **次**します。
8.   **SharePoint 製品構成ウィザードの完了** ページが表示されます。 クリックして **次**します。
9.   **構成成功** ] ページで [ **完了**します。

ファームの作成時に、プライマリ SQL Server 仮想マシンで一連のサーバー ログインが構成されます。 SQL Server 2012 では、包含データベースのパスワードを持つユーザーの概念が導入されています。 データベース自体にすべてのデータベース メタデータとユーザー情報が格納されており、このデータベースで定義されたユーザーは、対応するログインを持つ必要はありません。 このデータベース内の情報は、可用性グループによってレプリケートされ、フェールオーバー後に使用できます。 詳細については、次を参照してください。 [包含データベース](http://go.microsoft.com/fwlink/p/?LinkId=262794)します。

ただし、既定では SharePoint データベースは包含データベースではありません。 そのため、SharePoint ファーム アカウントの一連のログインがプライマリ データベース サーバーと同じものになるように、セカンダリ データベース サーバーを手動で構成する必要があります。 この同期は、両方のサーバーに同時に接続することで、SQL Server Management Studio から実行できます。

この初期セットアップが完了すると、SharePoint ファームの機能について、さらに多くの構成オプションを使用できます。 詳細については、次を参照してください。 [Azure インフラストラクチャ サービスでの SharePoint 2013 の計画](http://msdn.microsoft.com/library/dn275958.aspx)します。

次の図は、このフェーズが問題なく完了したときの構成を示しています。 

![](./media/virtual-machines-workload-intranet-sharepoint-phase4/workload-spsqlao_04.png)

## 次のステップ

- 使用 [フェーズ 5](virtual-machines-workload-intranet-sharepoint-phase5.md) このワークロードの構成を続行します。

