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

## ステップ 4: Azure Site Recovery プロバイダーをインストールする

1.  VMM マシンで、次のコマンドを実行してディレクトリを作成します。
    
    ```
    
        pushd C:\ASR\
    
    ```
    
2. 次のコマンドを実行して、ダウンロードしたプロバイダーを使用して、ファイルを抽出します。
    
    ```
    
        AzureSiteRecoveryProvider.exe /x:. /q
    
    ```
    
3. 次のコマンドを使用してプロバイダーをインストールします。
    
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
    
    インストールが完了するのを待ちます。
    
4. 次のコマンドを使用して、コンテナーにサーバーを登録します。
    
    ```
    
    $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
    pushd $BinPath
    $encryptionFilePath = "C:\temp\"
    .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice
    
    ```
    
## ステップ 5: Azure のストレージ アカウントを作成する

Azure のストレージ アカウントを持っていない場合、次のコマンドを実行して、geo レプリケーションが有効になっているアカウントを作成します。

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

ストレージ アカウントは Azure Site Recovery サービスと同じリージョンである必要があり、同じサブスクリプションに関連付けられている必要があります。


## ステップ 6: Azure Recovery Services エージェントをインストールする

Azure ポータルから、保護する VMM クラウドに配置されている Hyper-V ホスト サーバーごとに、Azure Recovery Services エージェントをインストールします。

すべての VMM ホストで次のコマンドを実行します。

```

    marsagentinstaller.exe /q /nu

```


## ステップ 7: クラウドの保護設定を構成する

1.  次のコマンドを実行して、Azure にクラウド保護のプロファイルを作成します。
    
    ```
    
    $ReplicationFrequencyInSeconds = "300";
    $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider  HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName `
    -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds     $ReplicationFrequencyInSeconds;
    
    ```
    
2.  次のコマンドを実行して、保護コンテナーを取得します。
    
    ```
    
        $PrimaryCloud = "testcloud"
        $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;    
    
    ```
    
3.  クラウドと保護コンテナーとのアソシエーションを開始します。
    
    ```
    
        $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -  ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;      
    
    ```
    
4.  ジョブが完了したら、次のコマンドを実行します。

            $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
            if($job -eq $null -or $job.StateDescription -ne "Completed")
            {
                $isJobLeftForProcessing = $true;
            }
5. ジョブの処理が完了したら、次のコマンドを実行します。

        if($isJobLeftForProcessing)
            {
            Start-Sleep -Seconds 60
            }
                }While($isJobLeftForProcessing)
    
    
操作の完了を確認する」の手順に従います [アクティビティを監視する](#monitor)です。

## ステップ 8: ネットワーク マッピングを構成する
ネットワーク マッピングを開始する前に、ソース VMM サーバー上の仮想マシンが VM ネットワークに接続されていることを確認してください。 さらに、1 つまたは複数の Azure 仮想ネットワークを作成します。 複数の VM ネットワークを 1 つの Azure ネットワークにマップできることに注意してください。

ターゲット ネットワークに複数のサブネットがあり、そのサブネットのいずれかが、ソースの仮想マシンが配置されているサブネットと同じ名前である場合、フェールオーバー後、レプリカの仮想マシンはそのターゲット サブネットに接続することに注意してください。 ターゲットのサブネットで名前が一致するものがなければ、仮想マシンはネットワークの最初のサブネットに接続されます。

最初のコマンドは、現在の Azure Site Recovery コンテナーのサーバーを取得します。 このコマンドは、$Servers 配列変数に、Microsoft Azure Site Recovery のサーバーを格納します。



    $Servers = Get-AzureSiteRecoveryServer


2 番目のコマンドは、$Servers 配列内の最初のサーバーの Site Recovery ネットワークを取得します。 コマンドは、$Networks 変数に、ネットワークを格納します。

```

    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

```

3 番目のコマンドは、Get-AzureSubscription コマンドレットを使用して Azure のサブスクリプションを取得し、$Subscriptions 変数にその値を格納します。 

```

    $Subscriptions = Get-AzureSubscription

```

4 番目のコマンドは、Get AzureVNetSite コマンドレットを使用して Azure の仮想ネットワークを取得し、$AzureVmNetworks 変数にその値を格納します。

```

    $AzureVmNetworks = Get-AzureVNetSite

```

最後のコマンドレットは、プライマリ ネットワークと Azure の仮想マシンのネットワーク間のマッピングを作成します。 このコマンドレットは $Networks の最初の要素として、プライマリ ネットワークを指定します。 このコマンドレットは、仮想マシンのネットワークを、その ID を使用して $AzureVmNetworks の最初の要素として指定します。 このコマンドは、Azure サブスクリプションの ID を含みます。 

```

PS C:\> New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id

```

## ステップ 9: 仮想マシンの保護を有効化する

サーバー、クラウド、およびネットワークを正しく構成した後で、クラウド内の仮想マシンの保護を有効にすることができます。 以下の点に注意してください。

仮想マシンが満たす必要がある [Azure の仮想マシンの前提条件](site-recovery-best-practices.md#virtual-machines)します。

オペレーティング システムとオペレーティング システム ディスクの保護を有効にするには、仮想マシンにプロパティを設定する必要があります。 仮想マシン テンプレートを使用して VMM 内で仮想マシンを作成する際に、プロパティを設定できます。 既存の仮想マシンに対してこれらのプロパティを設定することも、 **全般的な** と **ハードウェア構成** 、仮想マシンのプロパティ タブ。 VMM でこれらのプロパティを設定していない場合は、Azure Site Recovery ポータルで構成できます。


    
1.  保護を有効にするには、次のコマンドを実行して保護コンテナーを取得します。
        
    ```
    
    $ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName
    
    ```
    
2. 次のコマンドを実行して、保護エンティティ (VM) を取得します。
        
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
    
    $TemplatePath =「C:\RPTemplatePath.xml」です。
    
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
    
    $jobIDResult = 開始 AzureSiteRecoveryTestFailoverJob RecoveryPlan $RPObject-の方向 PrimaryToRecovery です。
    
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





