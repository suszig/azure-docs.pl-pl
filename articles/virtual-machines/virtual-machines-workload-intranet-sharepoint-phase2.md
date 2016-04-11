<properties
    pageTitle="SharePoint Server 2013 ファームのフェーズ 2 | Microsoft Azure"
    description="2 つの Active Directory のレプリカ ドメイン コント ローラーを作成および構成します。"
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

# SharePoint イントラネット ファーム ワークロードのフェーズ 2: ドメイン コントローラーの構成

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。

イントラネット専用 SharePoint 2013 ファームと SQL Server AlwaysOn 可用性グループを Azure インフラストラクチャ サービスにデプロイする作業のこのフェーズでは、Azure 仮想ネットワーク内の 2 つのドメイン コント ローラーを構成します。 これで、SharePoint ファーム リソースに対するクライアント Web 要求は、サイト間 VPN または Azure ExpressRoute 接続経由でオンプレミス ネットワークに認証のトラフィックを送信する代わりに、Azure の仮想ネットワーク内で認証できるようになります。

進むには、このフェーズを完了する必要があります [フェーズ 3](virtual-machines-workload-intranet-sharepoint-phase3.md)します。 参照してください [SharePoint と SQL Server AlwaysOn 可用性グループで、Azure のデプロイ](virtual-machines-workload-intranet-sharepoint-overview.md) の全フェーズ。

## Azure でのドメイン コントローラー仮想マシンの作成

最初に記入する必要があります、 **仮想マシン名** 表 M の列で、必要に応じて、仮想マシンのサイズを変更し、 **最小サイズ** 列です。  

項目 | 仮想マシン名 | ギャラリー イメージ | 最小サイズ
--- | --- | --- | ---
1. | ______________ (最初のドメイン コントローラー (例: DC1)) | Windows Server 2012 R2 Datacenter | Standard_A2
2. | ______________ (2 番目のドメイン コントローラー (例: DC2)) | Windows Server 2012 R2 Datacenter | Standard_A2
3. | ______________ (最初の SQL Server コンピューター (例: SQL1)) | Microsoft SQL Server 2014 Enterprise - Windows Server 2012 R2 | Standard_A5
4. | ______________ (2 番目の SQL Server コンピューター (例: SQL2)) | Microsoft SQL Server 2014 Enterprise - Windows Server 2012 R2 | Standard_A5
5. | ______________ (クラスターのマジョリティ ノード (例: MN1)) | Windows Server 2012 R2 Datacenter | Standard_A1
6. | ______________ (最初の SharePoint アプリケーション サーバー (例: APP1)) | Microsoft SharePoint Server 2013 評価版 - Windows Server 2012 R2 | Standard_A4
7. | ______________ (2 番目の SharePoint アプリケーション サーバー (例: APP2)) | Microsoft SharePoint Server 2013 評価版 - Windows Server 2012 R2 | Standard_A4
8. | ______________ (最初の SharePoint Web サーバー (例: WEB1)) | Microsoft SharePoint Server 2013 評価版 - Windows Server 2012 R2 | Standard_A4
9. | ______________ (2 番目の SharePoint Web サーバー (例: WEB2)) | Microsoft SharePoint Server 2013 評価版 - Windows Server 2012 R2 | Standard_A4

**表 M - Azure の SharePoint 2013 イントラネット ファームの仮想マシン**

仮想マシンのサイズの完全な一覧については [仮想マシンのサイズ](virtual-machines-size-specs.md)します。

Azure PowerShell コマンドの次のブロックを使用して、2 つのドメイン コントローラーの仮想マシンを作成します。 削除して、変数の値を指定します < と > 文字です。 この Azure PowerShell コマンド セットでは、次の値を使用します。

- 表 M (仮想マシン)
- 表 V (仮想ネットワーク設定)
- 表 S (サブネット)
- 表 ST (ストレージ アカウント)
- 表 A (可用性セット)

表 V、S、ST、および A を定義したもの [フェーズ 1: Azure を構成する](virtual-machines-workload-intranet-sharepoint-phase1.md)です。

> [AZURE.NOTE] 次のコマンドは、1.0 以降、Azure PowerShell を使用を設定します。 詳細については、次を参照してください。 [Azure PowerShell 1.0](https://azure.microsoft.com/blog/azps-1-0/)します。

適切な値をすべて指定したら、Azure PowerShell プロンプトでそのブロックを実行します。

    # Set up key variables
    $rgName="<resource group name>"
    $locName="<Azure location of your resource group>"
    $saName="<Table ST – Item 1 – Storage account name column>"
    $vnetName="<Table V – Item 1 – Value column>"
    $avName="<Table A – Item 1 – Availability set name column>"
    
    # Create the first domain controller
    $vmName="<Table M – Item 1 - Virtual machine name column>"
    $vmSize="<Table M – Item 1 - Minimum size column>"
    $staticIP="<Table V – Item 6 - Value column>"
    $vnet=Get-AzureRMVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $nic=New-AzureRMNetworkInterface -Name ($vmName +"-NIC") -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[1].Id -PrivateIpAddress $staticIP
    $avSet=Get-AzureRMAvailabilitySet –Name $avName –ResourceGroupName $rgName 
    $vm=New-AzureRMVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
    
    $diskSize=<size of the extra disk for AD DS data in GB>
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "ADDSData" -DiskSizeInGB $diskSize -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the first domain controller." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm
    
    # Create the second domain controller
    $vmName="<Table M – Item 2 - Virtual machine name column>"
    $vmSize="<Table M – Item 2 - Minimum size column>"
    $staticIP="<Table V – Item 7 - Value column>"
    $vnet=Get-AzureRMVirtualNetwork -Name $vnetName -ResourceGroupName $rgName
    $nic=New-AzureRMNetworkInterface -Name ($vmName +"-NIC") -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[1].Id -PrivateIpAddress $staticIP
    $avSet=Get-AzureRMAvailabilitySet –Name $avName –ResourceGroupName $rgName 
    $vm=New-AzureRMVMConfig -VMName $vmName -VMSize $vmSize -AvailabilitySetId $avset.Id
    
    $diskSize=<size of the extra disk for AD DS data in GB>
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-ADDSDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "ADDSData" -DiskSizeInGB $diskSize -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the second domain controller." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/" + $vmName + "-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

> [AZURE.NOTE] これらの仮想マシンでは、イントラネット アプリケーションであるために、いない、割り当てられているパブリック IP アドレスまたは DNS ドメイン名のラベル、インターネットに公開します。 ただし、これは Azure ポータルから接続できないことを意味します。  **接続** ] ボタンは使用できません、仮想マシンのプロパティを表示するとします。 プライベート IP アドレスまたはイントラネット DNS 名を使用して仮想マシンに接続する場合は、リモート デスクトップ接続のアクセサリまたは別のリモート デスクトップ ツールを使用します。

## 最初のドメイン コントローラーの構成

好みのリモート デスクトップ クライアントを使用し、最初のドメイン コントローラー仮想マシンへのリモート デスクトップ接続を作成します。 仮想マシンのイントラネット DNS 名またはコンピューター名と、ローカル管理者アカウントの資格情報を使用します。

次に、最初のドメイン コントローラーにデータ ディスクを追加する必要があります。

### <a id="datadisk"></a>空のディスクを初期化するには

1.  サーバー マネージャーの左側のウィンドウで、クリックして **ファイルおよび記憶域サービス**, 、] をクリックし、 **ディスク**します。
2.  コンテンツ ウィンドウで、[で、 **ディスク** グループで、ディスクを [ **2** (で、 **パーティション** に設定 **不明な**)。
3.  クリックして **タスク**, 、] をクリックし、 **新しいボリューム**します。
4.  前に新しいボリューム ウィザードのページを開始でクリックして **次**します。
5.  選択して、サーバーとディスク] ページで、をクリックして **ディスク 2**, 、クリックして **次**します。 メッセージが表示されたら、[OK] をクリックします。
6.  [ボリューム] ページのサイズの指定] をクリックして **次**します。
7.  ドライブ文字またはフォルダーのページへの割り当て、をクリックして **次**します。
8.  クリックして選択し、ファイル システムの設定] ページで、 **次**します。
9.  選択内容の確認] ページをクリックして **作成**します。
10. 完了したら、クリックして **閉じる**します。

次に、最初のドメイン コントローラーから組織のネットワーク上の場所への接続をテストします。

### <a id="testconn"></a>接続をテストするには

1.  デスクトップで Windows PowerShell プロンプトを開きます。
2.  使用して、 **ping** ping 名および組織のネットワーク上のリソースの IP アドレスをコマンドです。

この手順により、DNS 名前解決が正しく機能していること (オンプレミス DNS サーバーで仮想マシンが正しく構成されていること) と、クロスプレミス仮想ネットワーク間でパケットを送受信できることが確認されます。 この基本テストに失敗した場合は、IT 部門に連絡し、DNS 名前解決とパケット配信の問題のトラブルシューティングを行ってください。

次に、最初のドメイン コントローラーの Windows PowerShell コマンド プロンプトで、次のコマンドを実行します。

    $domname="<DNS domain name of the domain for which this computer will be a domain controller>"
    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -InstallDns –DomainName $domname  -DatabasePath "F:\NTDS" -SysvolPath "F:\SYSVOL" -LogPath "F:\Logs"

ドメイン管理者アカウントの資格情報の入力が求められます。 コンピューターが再起動します。

## 2 番目のドメイン コントローラーの構成

好みのリモート デスクトップ クライアントを使用し、2 番目のドメイン コントローラー仮想マシンへのリモート デスクトップ接続を作成します。 仮想マシンのイントラネット DNS 名またはコンピューター名と、ローカル管理者アカウントの資格情報を使用します。

次に、2 番目のドメイン コントローラーにデータ ディスクを追加する必要があります。 参照してください、 [空のディスクの手順を初期化するために](#datadisk)します。

次に、2 番目のドメイン コントローラーの Windows PowerShell プロンプトで、以下のコマンドを実行します。

    $domname="<DNS domain name of the domain for which this computer will be a domain controller>"
    Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
    Install-ADDSDomainController -InstallDns –DomainName $domname  -DatabasePath "F:\NTDS" -SysvolPath "F:\SYSVOL" -LogPath "F:\Logs"

ドメイン管理者アカウントの資格情報の入力が求められます。 コンピューターが再起動します。

次に、DNS サーバーとして使用する 2 つの新しいドメイン コントローラーの IP アドレスが仮想マシンに割り当てられるように、仮想ネットワークの DNS サーバーを更新する必要があります。 この手順では、表 V (仮想ネットワーク設定) および表 M (仮想マシン) の値を使用します。

1.  Azure ポータルの左側のウィンドウで、クリックして **仮想ネットワーク**, 、仮想ネットワーク (テーブル V-項目 1-値] 列) の名前をクリックします。
2.   **設定** ] ウィンドウで、をクリックして **DNS サーバー**します。
3.   **DNS サーバー** ] ウィンドウで、次を入力します。
    -  **プライマリ DNS サーバー**: テーブル V-項目 6-値] 列
    -  **セカンダリ DNS サーバー**: テーブル V-項目 7-値] 列
4.  Azure ポータルの左側のウィンドウで、クリックして **仮想マシン**します。
5.   **仮想マシン ウィンドウ**, 、最初のドメイン コント ローラー (表 M-項目 1 - 仮想マシン名] 列) の名前をクリックします。
6.  仮想マシンのウィンドウで、次のようにクリックします。 **再起動**します。
7.  最初のドメイン コント ローラーを開始したときに、2 番目のドメイン コント ローラーの名前をクリックしてで、 **仮想マシン** ウィンドウ (表 M – 項目 2 - 仮想マシン名] 列)。
8.  仮想マシンのウィンドウで、次のようにクリックします。 **再起動**します。 2 番目のドメイン コントローラーが起動するまで待ちます。

2 つのドメイン コントローラーを再起動するのは、オンプレミス DNS サーバーで DNS サーバーとして構成されないようにするためです。 これらのドメイン コントローラーは、それ自体が DNS サーバーであるため、ドメイン コントローラーに昇格されたときに、オンプレミス DNS サーバーで DNS フォワーダーとして自動的に構成されています。

次に、Azure 仮想ネットワークのサーバーでローカル ドメイン コントローラーが使用されるように、Active Directory レプリケーション サイトを作成する必要があります。 ドメイン管理者アカウントを使用してプライマリ ドメイン コントローラーにログオンし、管理者レベルの Windows PowerShell プロンプトで次のコマンドを実行します。

    $vnet="<Table V – Item 1 – Value column>"
    $vnetSpace="<Table V – Item 5 – Value column>"
    New-ADReplicationSite -Name $vnet 
    New-ADReplicationSubnet –Name $vnetSpace –Site $vnet

## SharePoint ファームのアカウントとアクセス許可の構成

SharePoint ファームでは、次のユーザー アカウントが必要です。

- sp_farm: SharePoint ファームを管理するためのユーザー アカウント。
- sp_farm_db: SQL Server インスタンスに対する sysadmin 権限を持つユーザー アカウント。
- sp_install: ロールと機能をインストールするために必要なドメイン管理権限を持つユーザー アカウント。
- sqlservice: SQL Server インスタンスを実行できるユーザー アカウント。

次に、コンピューターにログオン任意のドメイン コント ローラーがメンバーでは、開いているドメインのドメイン管理者アカウントで、管理者レベルの Windows PowerShell コマンド プロンプトとこれらのコマンドを実行 *一度に 1 つずつ*:

    New-ADUser -SamAccountName sp_farm -AccountPassword (read-host "Set user password" -assecurestring) -name "sp_farm" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false

    New-ADUser -SamAccountName sp_farm_db -AccountPassword (read-host "Set user password" -assecurestring) -name "sp_farm_db" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false

    New-ADUser -SamAccountName sp_install -AccountPassword (read-host "Set user password" -assecurestring) -name "sp_install" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false

    New-ADUser -SamAccountName sqlservice -AccountPassword (read-host "Set user password" -assecurestring) -name "sqlservice" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false

コマンドごとにパスワードの入力を求められます。 これらのアカウント名とパスワードを記録し、安全な場所に保管しておきます。

次に、次の手順を実行して、アカウント プロパティを新しいユーザー アカウントに追加します。

1.  スタート画面から次のように入力します。 **Active Directory ユーザー**, 、クリックして **Active Directory ユーザーとコンピューター**します。
2.  ツリー ペインで、ドメインを開きクリックして **ユーザー**します。
3.  コンテンツ ウィンドウで右クリック **sp_install**, 、クリックして **グループに追加**します。
4.   **グループの選択** ダイアログで、「 **ドメイン管理者**, 順にクリック **OK** 2 回クリックします。
5.  ダイアログ ボックスで [ **ビュー [高度な機能] をクリックして**します。 このオプションでは、すべての非表示コンテナーと、Active Directory オブジェクトのプロパティ ウィンドウ内の非表示タブを表示できます。
6.  ドメイン名を右クリックし、クリックして **プロパティ**します。
7.   **プロパティ** ダイアログ ボックスで、をクリックして、 **セキュリティ** タブをクリックし、をクリックして、 **[詳細設定** ] ボタンをクリックします。
8.   **の高度なセキュリティ設定 <YourDomain>** ウィンドウで、をクリックして **追加**します。
9.   **のアクセス許可エントリ <YourDomain>** ウィンドウで、をクリックして **プリンシパルの選択**します。
10. [テキスト ボックスに入力 **<YourDomain>\sp_install**, 、] をクリックし、 **OK**します。
11. 選択 **許可** の **コンピューター オブジェクトの作成**, 、] をクリックし、 **[ok]** 3 回です。

次の図は、このフェーズが問題なく完了したときの構成を示しています。プレースホルダーにコンピューター名が示されています。

![](./media/virtual-machines-workload-intranet-sharepoint-phase2/workload-spsqlao_02.png)

## 次のステップ

- 使用 [フェーズ 3](virtual-machines-workload-intranet-sharepoint-phase3.md) このワークロードの構成を続行します。

