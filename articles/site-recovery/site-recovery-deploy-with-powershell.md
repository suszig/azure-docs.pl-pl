<properties
    pageTitle="Azure Site Recovery と PowerShell を使用して VMM クラウドの Hyper-V 仮想マシンをレプリケートする | Microsoft Azure"
    description="Azure Site Recovery と PowerShell を使用して VMM クラウドの Hyper-V 仮想マシンのレプリケートを自動化する方法を説明します。"
    services="site-recovery"
    documentationCenter=""
    authors="csilauraa"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/14/2015"
    ms.author="lauraa"/>

# Azure Site Recovery と PowerShell を使用して VMM クラウドの Hyper-V 仮想マシンをレプリケートする


## 概要

Azure Site Recovery は、さまざまなデプロイ シナリオでの仮想マシンのレプリケーション、フェールオーバー、復旧を調整してビジネス継続性と障害復旧 (BCDR) 戦略に貢献します。 シナリオの「展開の完全な一覧については、 [Azure Site Recovery の概要](site-recovery-overview.md)します。

この記事では、System Center VMM クラウドの Hyper-V 仮想マシンを Azure ストレージにレプリケートするように Azure Site Recovery を設定するときに実行する必要がある一般的なタスクを PowerShell を使用して自動化する方法を示します。

ガイドには、シナリオの前提条件が含まれています。また、Site Recovery コンテナーを設定する方法、ソース VMM サーバーに Azure Site Recovery プロバイダーをインストールする方法、このコンテナーにサーバーを登録する方法、Azure ストレージ アカウントを追加する方法、Hyper-V ホスト サーバーに Azure Recovery Services エージェントをインストールする方法、保護されるすべての仮想マシンに適用される VMM クラウドの保護設定を構成する方法、およびこれらの仮想マシンの保護を有効にする方法についても説明しています。 すべてが正しく動作していることを確認するために、最後にフェールオーバーをテストします。

このシナリオの設定の問題が発生した場合に質問を投稿、 [Azure Recovery Services フォーラム](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)します。


## 開始する前に

次の前提条件を満たしていることを確認してください。

### Azure の前提条件

- 必要があります、 [Microsoft Azure](http://azure.microsoft.com/) アカウントです。 まず、 [無料評価版](pricing/free-trial/)します。
- レプリケートしたデータを格納するには Azure ストレージ アカウントが必要になります。 アカウントでは geo レプリケーションを有効にする必要があります。 アカウントは Azure Site Recovery コンテナーと同じリージョンである必要があり、同じサブスクリプションに関連付けられている必要があります。 [Azure ストレージの詳細について](../storage/storage-introduction.md)します。
- 保護する仮想マシンに準拠しているかどうかを確認する必要があります [Azure の仮想マシンの前提条件](site-recovery-best-practices.md#virtual-machines)します。

### VMM の前提条件
- VMM サーバーの System Center 2012 R2 で実行されている必要があります。
- 保護する VMM サーバーにクラウドが少なくとも 1 つ必要です。 このクラウドには以下のものが含まれている必要があります。
    - 1 つ以上の VMM ホスト グループ。
    - 各ホスト グループ内に 1 つ以上の Hyper-V ホスト サーバーまたはクラスター。
    - ソース Hyper-V サーバー上に配置された 1 つ以上の仮想マシン。

### Hyper-V の前提条件

- ホスト Hyper-V サーバーは、少なくとも Hyper-V ロールを持つ Windows Server 2012 を実行しており、最新の更新プログラムがインストールされている必要があります。
- クラスターで Hyper-V を実行している場合に、静的 IP アドレス ベースのクラスターが存在すると、クラスター ブローカーが自動的に作成されません。 クラスター ブローカーを手動で構成する必要があります。 これを行う、サーバー マネージャーで > フェールオーバー クラスター マネージャーでは、クラスターに接続して、をクリックして **役割の構成** 選択 **HYPER-V レプリカ ブローカー** で、 **役割の選択** 高可用性ウィザードの画面です。 
- 保護を管理するすべての Hyper-V ホスト サーバーまたはクラスターが VMM クラウドに属している必要があります。

### ネットワーク マッピングの前提条件
仮想マシンを Azure ネットワーク マッピングで保護する場合、ソース VMM サーバー上の VM ネットワークとターゲットの Azure ネットワークをマップして以下を実現します。

- 同じネットワーク上でフェールオーバーするすべてのマシンは、どの復旧計画に含まれていても、相互に接続できる。
- ターゲットの Azure ネットワークでネットワーク ゲートウェイをセットアップすると、仮想マシンを他のオンプレミスの仮想マシンに接続できる。
- ネットワーク マッピングを構成しない場合、Azure へのフェールオーバー後、同じ復旧計画でフェールオーバーする仮想マシンのみ相互に接続できる。

ネットワーク マッピングをデプロイする場合は、以下のことが必要になります。

- ソース VMM サーバー上の保護する仮想マシンが VM ネットワークに接続している。 そのネットワークは、クラウドに関連付けられた論理ネットワークにリンクされている必要があります。
- レプリケートされた仮想マシンがフェールオーバー後に接続できる Azure ネットワーク。 フェールオーバー時にこのネットワークを選択します。 ネットワークは Azure Site Recovery サブスクリプションと同じリージョンである必要があります。
- [詳細については](site-recovery-network-mapping.md) ネットワーク マッピングに関する。

###PowerShell の前提条件
Azure PowerShell を使用する準備が整っていることを確認してください。 PowerShell を使用している場合は、0.8.10 以降のバージョンにアップグレードする必要があります。 PowerShell の設定の詳細については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](powershell-install-configure.md)します。 設定し、PowerShell を構成すると、すべてのサービスの利用可能なコマンドレットを表示できます [ここ](https://msdn.microsoft.com/library/dn850420.aspx)します。 

処理方法などのパラメーター値、入力呼び出し力は通常 Azure PowerShell でのコマンドレットを使用するのに役立つヒントについてを参照してください。 [Azure コマンドレットの概要](https://msdn.microsoft.com/library/azure/jj554332.aspx)します。

## ステップ 1: サブスクリプションを設定する 

PowerShell で、次のコマンドレットを実行します。



            $UserName = "<user@live.com>"
    $Password = "<password>"
    $AzureSubscriptionName = "prod_sub1"

    $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
    $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
    Add-AzureAccount -Credential $Cred;
    $AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName


"< >" 内の要素をユーザー固有の情報に置き換えます。

## ステップ 2: Site Recovery コンテナーを作成する

PowerShell で "< >" 内の要素をユーザー固有の情報に置き換え、次のコマンドを実行します。

```

    $VaultName = "<testvault123>"
    $VaultGeo  = "<Southeast Asia>"
    $OutputPathForSettingsFile = "<c:\>"

```


```
    New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
    $vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## ステップ 3: コンテナー登録キーを生成する

コンテナーの登録キーを生成します。 Azure Site Recovery プロバイダーをダウンロードして VMM サーバーにインストールした後で、このキーを使用して、VMM サーバーをコンテナーに登録します。

1.  コンテナー設定ファイルを取得し、コンテキストを設定します。
    
    ```
    
        $VaultName = "<testvault123>"
        $VaultGeo  = "<Southeast Asia>"
        $OutputPathForSettingsFile = "<c:\>"
    
        $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;
    
    ```
    
2.  次のコマンドを実行して、コンテナーのコンテキストを設定します。
    
    ``` 
        $VaultSettingFilePath = $vaultSetingsFile.FilePath 
        $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop
```

## Step 4: Install the Azure Site Recovery Provider

1.  On the VMM machine, create a directory by running the following command:
    
    ```
    
        pushd C:\ASR\
    
    ```
    
2. Extract the files using the downloaded provider by running the following command
    
    ```
    
        AzureSiteRecoveryProvider.exe /x:. /q
    
    ```
    
3. Install the provider using the following commands:
    
    ```
    
    .\SetupDr.exe /i
    
    ```
    
    ```
    
    $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
    do
    {
                    $isNotInstalled = $true;
                    if(Test-Path $installationRegPath)
                    {
                                    $isNotInstalled = $false;
                    }
    }While($isNotInstalled)
    
    ```
    
    Wait for the installation to finish.
    
4. Register the server in the vault using the following command:
    
    ```
    
    $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
    pushd $BinPath
    $encryptionFilePath = "C:\temp\"
    .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice
    
    ```
    
## Step 5: Create an Azure storage account

If you don't have an Azure storage account, create a geo-replication enabled account by running the following command:

```

$StorageAccountName ="teststorageacc1"
$StorageAccountGeo ="Southeast Asia"

新しい AzureStorageAccount StorageAccountName $StorageAccountName-$StorageAccountName にラベルを付ける-場所 $StorageAccountGeo です。

```

Note that the storage account must be in the same region as the Azure Site Recovery service, and be associated with the same subscription.


## Step 6: Install the Azure Recovery Services Agent

From the Azure portal, install the Azure Recovery Services agent on each Hyper-V host server located in the VMM clouds you want to protect.

Run the following command on all VMM hosts:

```

    marsagentinstaller.exe /q /nu

```


## Step 7: Configure cloud protection settings

1.  Create a cloud protection profile to Azure by running the following command:
    
    ```
    
    $ReplicationFrequencyInSeconds = "300";
    $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider  HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName `
    -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds     $ReplicationFrequencyInSeconds;
    
    ```
    
2.  Get a protection container by running the following commands:
    
    ```
    
        $PrimaryCloud = "testcloud"
        $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;    
    
    ```
    
3.  Start the association of the protection container with the cloud:
    
    ```
    
        $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -  ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;      
    
    ```
    
4.  After the job has finished, run the following command:

            $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
            if($job -eq $null -or $job.StateDescription -ne "Completed")
            {
                $isJobLeftForProcessing = $true;
            }
5. After the job has finished processing, run the following command:

        if($isJobLeftForProcessing)
            {
            Start-Sleep -Seconds 60
            }
                }While($isJobLeftForProcessing)
    
    
To check the completion of the operation, follow the steps in [Monitor Activity](#monitor).

## Step 8: Configure network mapping
Before you begin network mapping verify that virtual machines on the source VMM server are connected to a VM network. In addition, create one or more Azure virtual networks. Note that multiple VM networks can be mapped to a single Azure network.

Note that if the target network has multiple subnets and one of those subnets has the same name as subnet on which the source virtual machine is located, then the replica virtual machine will be connected to that target subnet after failover. If there is not a target subnet with a matching name, the virtual machine will be connected to the first subnet in the network.

The first command gets servers for the current Azure Site Recovery vault. The command stores the Microsoft Azure Site Recovery servers in the $Servers array variable.



    $Servers = Get-AzureSiteRecoveryServer


The second command gets the site recovery network for the first server in the $Servers array. The command stores the networks in the $Networks variable.

```

    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

```

The third command gets your Azure subscriptions by using the Get-AzureSubscription cmdlet, and then stores that value in the $Subscriptions variable. 

```

    $Subscriptions = Get-AzureSubscription

```

The fourth command gets Azure virtual networks by using the Get-AzureVNetSite cmdlet, and then that value in the $AzureVmNetworks variable.

```

    $AzureVmNetworks = Get-AzureVNetSite

```

The final cmdlet creates a mapping between the primary network and the Azure virtual machine network. The cmdlet specifies the primary network as the first element of $Networks. The cmdlet specifies a virtual machine network as the first element of $AzureVmNetworks by using its ID. The command includes your Azure subscription ID. 

```

PS C:\ > 新しい AzureSiteRecoveryNetworkMapping PrimaryNetwork $Networks [0] AzureSubscriptionId $Subscriptions [0]。SubscriptionId AzureVMNetworkId $AzureVmNetworks [0] をダブルクリックします。Id

```

## Step 9: Enable protection for virtual machines

After servers, clouds, and networks are configured correctly, you can enable protection for virtual machines in the cloud. Note the following:

Virtual machines must meet [Azure virtual machine prerequisites](site-recovery-best-practices.md#virtual-machines).

To enable protection the operating system and operating system disk properties must be set for the virtual machine. When you create a virtual machine in VMM using a virtual machine template you can set the property. You can also set these properties for existing virtual machines on the **General** and **Hardware Configuration** tabs of the virtual machine properties. If you don't set these properties in VMM you'll be able to configure them in the Azure Site Recovery portal.


    
1.  To enable protection, run the following command to get the protection container:
        
    ```
    
    $ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName
    
    ```
    
2. Get the protection entity (VM) by running the following command:
        
    ```
    
    $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer
        
        ```
            
3. Enable the DR for the VM by running the following command:

    
    $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity  -Protection Enable -Force
    

## Test your deployment

To test your deployment you can run a test failover for a single virtual machine, or create a recovery plan consisting of multiple virtual machines and run a test failover for the plan. Test failover simulates your failover and recovery mechanism in an isolated network. Note that:

- If you want to connect to the virtual machine in Azure using Remote Desktop after the failover, enable Remote Desktop Connection on the virtual machine before you run the test failover.
- After failover, you'll use a public IP address to connect to the virtual machine in Azure using Remote Desktop. If you want to do this, ensure you don't have any domain policies that prevent you from connecting to a virtual machine using a public address.

To check the completion of the operation, follow the steps in [Monitor Activity](#monitor).

### Create a recovery plan

1. Create an .xml file as a template for your recovery plan using the data below, and then save it as "C:\RPTemplatePath.xml".
2. Change the RecoveryPlan node Id, Name, PrimaryServerId, and SecondaryServerId.
3. Change the ProtectionEntity node PrimaryProtectionEntityId (vmid from VMM).
4. You can add more VMs by adding more ProtectionEntity nodes.
    
    ```
    
    <#
    <?xml version="1.0" encoding="utf-16"?>
    <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"  PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"  SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
      <Actions />
      <ActionGroups>
        <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
          <PreActionSequence />
          <PostActionSequence />
        </ShutdownAllActionGroup>
        <FailoverAllActionGroup Id="FailoverAllActionGroup">
          <PreActionSequence />
          <PostActionSequence />
        </FailoverAllActionGroup>
        <BootActionGroup Id="DefaultActionGroup">
          <PreActionSequence />
          <PostActionSequence />
          <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03- cf163cc36ef8" />
        </BootActionGroup>
      </ActionGroups>
      <ActionGroupSequence>
        <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"  Before="FailoverAllActionGroup" />
        <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"  After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
        <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
      </ActionGroupSequence>
    </RecoveryPlan>
    #>
    
    ```
    
4. Fill in the data in the template:
    
    ```
    
    $TemplatePath = "C:\RPTemplatePath.xml";
    
    ```
    
5. Create the RecoveryPlan:
    
    ```
    
        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;
    
    ```
    
### Run a test failover

1. Get the RecoveryPlan object by running the following command:
    
    ```
    
        $RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;
    
    ```
    
2. Start the test failover by running the following command:
    
    ```
    
    $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;
    
    ```
    
## <a name=monitor></a> Monitor Activity

Use the following commands to monitor the activity. Note that you have to wait in between jobs for the processing to finish.

```

操作を行います
{
                $job = get AzureSiteRecoveryJob-Id $associationJob.JobId です。
                Write-host"ジョブの状態: {0}、StateDescription: \\ 1 \\"-f Job.State $job します。StateDescription です。
                場合 ($job-eq $null- または $job です。StateDescription-ne「完了」)
                {
                                $isJobLeftForProcessing = $true です。
                }

```

```

if($isJobLeftForProcessing)
                {
                                Start-sleep - 60 (秒)
                }
} While($isJobLeftForProcessing)

```


## Next steps

[Read more](https://msdn.microsoft.com/library/dn850420.aspx) about Azure Site Recovery PowerShell cmdlets. </a>.




