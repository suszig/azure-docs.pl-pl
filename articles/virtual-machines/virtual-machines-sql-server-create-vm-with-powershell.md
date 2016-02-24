<properties 
    pageTitle="PowerShell での SQL Server 仮想マシンの作成 | Microsoft Azure"
    description="SQL Server 仮想マシン ギャラリー のイメージを使用して Azure VM を作成するための手順と PowerShell スクリプトを提供します。"
    services="virtual-machines"
    documentationCenter="na"
    authors="rothja"
    manager="jeffreyg"
    editor="monicar" 
    tags="azure-service-management"
     />
<tags 
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/26/2015"
    ms.author="jroth" />

# Azure での SQL Server 仮想マシンの作成 (PowerShell)

> [AZURE.SELECTOR]
- [Azure クラシック ポータル](virtual-machines-provision-sql-server.md)
- [PowerShell](virtual-machines-sql-server-create-vm-with-powershell.md)

## 概要

この記事では、PowerShell コマンドレットを使用して、Azure で SQL Server 仮想マシンを作成する手順を説明します。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


## PowerShell のインストールと構成

1. Azure アカウントがないを参照してください。 [Azure 無料評価版](https://azure.microsoft.com/pricing/free-trial/)します。 
 
2. [最新の Azure PowerShell コマンドレット インストール](../powershell-install-configure.md/#how-to-install-azure-powershell)します。

3. [PowerShell を Azure サブスクリプションに接続](../powershell-install-configure.md/#how-to-connect-to-your-subscription)します。

## ターゲット Azure リージョンの決定

SQL Server の仮想マシンは、特定の Azure リージョンに存在するクラウド サービスでホストされます。 次の手順は、このチュートリアルの残りの部分に使用されるリージョン、ストレージ アカウント、およびクラウド サービスの決定に役立ちます。

1. SQL Server VM のホストに使用するデータ センターを決定します。 次の PowerShell コマンドによって、使用可能なリージョンの詳細が文末の要約リストとともに表示されます。

        Get-AzureLocation
        (Get-AzureLocation).Name

2.  任意の場所を特定した、という名前の変数を設定 **$dcLocation** 領域にします。

        $dcLocation = "<region name>"

## サブスクリプションとストレージ アカウントの設定

1. 新しい仮想マシンに使用する Azure サブスクリプションを決定します。

        (Get-AzureSubscription).SubscriptionName

1. ターゲットの Azure サブスクリプションを割り当てるには **$subscr** 変数です。 次に、これを現在の Azure のサブスクリプションとして設定します。 

        $subscr="<subscription name>"
        Select-AzureSubscription -SubscriptionName $subscr –Current

1. 既存のストレージ アカウントを確認します。 次のスクリプトでは、選択したリージョン内に存在するすべてのストレージ アカウントが表示されます。

        (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName

    >[AZURE.NOTE] 次の例のように New-azurestorageaccount コマンドを使用して、すべて小文字のストレージ アカウント名がまず作成するには、新しいストレージ アカウントが必要がある場合: **New-azurestorageaccount StorageAccountName"<storage account name>"-$dcLocation の場所**

1. 対象のストレージ アカウント名を割り当てる、 **$staccount**します。 使用して **Set-azuresubscription** 現在のストレージ アカウントとサブスクリプションを設定します。

        $staccount="<storage account name>"
        Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

## SQL Server 仮想マシン イメージの選択

1. ギャラリーから使用可能な SQL Server 仮想マシン イメージのリストを見つけます。 これらのイメージがすべて、 **ImageFamily** "SQL"で始まるプロパティです。 次のクエリでは、SQL Server が事前にインストールされている、使用可能なイメージ ファミリが表示されます。

        Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily

1. 仮想マシン イメージ ファミリが見つかったら、可能性がありますパブリッシュされた複数のイメージこのファミリ。 選択したイメージ ファミリの最新の公開済みの仮想マシン イメージ名を検索する次のスクリプトを使用して (よう **Windows Server 2012 R2 の SQL Server 2014 SP1 Enterprise**)。

        $family="<ImageFamily value>"
        $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

        echo "Selected SQL Server image name:"
        echo "   $image"

## 仮想マシンの作成

最後に、PowerShell を使用して仮想マシンを作成します。

1. 新しい VM をホストするクラウド サービスを作成します。 既存のクラウド サービスを代わりに使用することもできます。 新しい変数を作成 **$svcname** クラウド サービスの短い名前に置き換えます。

        $svcname = "<cloud service name>"
        New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

2. 仮想マシンの名前とサイズを指定します。 仮想マシンのサイズの詳細については、次を参照してください。 [Azure の仮想マシンのサイズ](virtual-machines-size-specs.md)します。

        $vmname="<machine name>"
        $vmsize="<Specify a valid machine size>" # see the link to virtual machine sizes
        $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

3. ローカル管理者アカウントとパスワードを指定します。

        $cred=Get-Credential -Message "Type the name and password of the local administrator account."
        $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

4. 次のスクリプトを実行して、仮想マシンを作成します。

        New-AzureVM –ServiceName $svcname -VMs $vm1

>[AZURE.NOTE] 追加の説明と構成オプションでは、「、 **コマンド セットを構築** セクション [を作成し、Windows ベースの仮想マシンを事前構成する Azure PowerShell を使用して](virtual-machines-ps-create-preconfigure-windows-vms.md)います。

## PowerShell サンプル スクリプト

次のスクリプトを作成する完全なスクリプトの例は、 **Windows Server 2012 R2 の SQL Server 2014 SP1 Enterprise** 仮想マシン。 このスクリプトを使用する場合、このトピックの前のステップに基づいて、初期変数をカスタマイズする必要があります。

    # Customize these variables based on your settings and requirements:
    $dcLocation = "East US"
    $subscr="mysubscription"
    $staccount="mystorageaccount"
    $family="SQL Server 2014 SP1 Enterprise on Windows Server 2012 R2"
    $svcname = "mycloudservice"
    $vmname="myvirtualmachine"
    $vmsize="A5" 
    
    # Set the current subscription and storage account
    # Comment out the New-AzureStorageAccount line if the account already exists
    Select-AzureSubscription -SubscriptionName $subscr –Current
    New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount
    
    # Select the most recent VM image in this image family:
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    
    # Create the new cloud service; comment out this line if cloud service exists already:
    New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation
    
    # Create the VM config:
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image
    
    # Set administrator credentials:
    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
    
    # Create the SQL Server VM:
    New-AzureVM –ServiceName $svcname -VMs $vm1
     

## リモート デスクトップへの接続

1. 現在のユーザーのドキュメント フォルダに .RDP ファイルを作成し、これらの仮想マシンを起動してセットアップを完了します。

        $documentspath = [environment]::getfolderpath("mydocuments")
        Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"

1. ドキュメント ディレクトリで、RDP ファイルを起動します。 以前に提供された管理ユーザー名とパスワードを使用して接続します (たとえば、ユーザー名が VMAdmin であった場合、ユーザーとして「\VMAdmin」を指定し、パスワードを入力します)。

        .\vm1.rdp

## リモート アクセス用の SQL Server コンピューターの構成を完了する

リモート デスクトップを使用してコンピューターに記録された後の手順に応じた SQL Server を構成する [を Azure VM で SQL Server の接続を構成する手順](virtual-machines-sql-server-connectivity.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm)します。

## 次のステップ

Powershell での仮想マシンのプロビジョニング用の追加の手順を参照して、 [virtual machines のドキュメント](virtual-machines-ps-create-preconfigure-windows-vms.md)します。 SQL Server と Premium Storage に関連する追加のスクリプトでは、次を参照してください。 [Azure Premium Storage を使用して、仮想マシン上の SQL Server を](virtual-machines-sql-server-use-premium-storage.md)します。

多くの場合、次の手順はこの新しい SQL Server VM にデータベースを移行することです。 データベースの移行のガイドラインを参照してください。 [Azure VM 上の SQL Server にデータベースを移行する](virtual-machines-migrate-onpremises-database.md)です。

Azure クラシック ポータルからこれらの手順を実行する方法を確認することに興味も場合 [Azure で SQL Server 仮想マシンをプロビジョニング](virtual-machines-provision-sql-server.md)します。

これらのリソースだけでなくをお勧め」を参照する [Azure Virtual Machines における SQL Server を実行に関連するその他のトピック](virtual-machines-sql-server-infrastructure-services.md)します。

