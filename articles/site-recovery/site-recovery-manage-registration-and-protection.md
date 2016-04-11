<properties
    pageTitle="サーバーの登録解除と保護の無効化 | Microsoft Azure" 
    description="Azure Site Recovery は、オンプレミスのサーバーに配置されている仮想マシンの Azure またはセカンダリ データセンターへのレプリケーション、フェールオーバー、および復旧を調整します。 この記事を使用して、Site Recovery コンテナーからサーバーの登録を解除して、仮想マシンと物理サーバーの保護を無効にします。" 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="12/14/2015" 
    ms.author="raynew"/>

# サーバーの登録解除と保護の無効化

この記事では、Site Recovery コンテナーからサーバーの登録を解除する方法と Site Recovery によって保護される仮想マシンの保護を無効にする方法について説明します。 質問内容がある場合は、この記事に投稿して、 [Azure Recovery Services フォーラム](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)します。

## VMM サーバーの登録解除

サーバーを削除して、資格情報コンテナーから VMM サーバーの登録を解除する、 **サーバー** Azure Site Recovery ポータルでタブをクリックします。 以下の点に注意してください。

-  **接続されている VMM サーバー**: Azure に接続されているときに、VMM サーバーの登録を解除することをお勧めします。 これにより、オンプレミスの VMM サーバーの設定、およびオンプレミスの VMM サーバーと関連付けられている VMM サーバー (削除するサーバー上のクラウドにマップされているクラウドを含む VMM サーバー) の設定が正しくクリーンアップされます。 接続に永続的な問題がある場合は、接続されていないサーバーのみを削除することをお勧めします。
- **VMM サーバーが接続されていない**: 削除すると、VMM サーバーが接続されていない場合、クリーンアップを実行するには、手動でスクリプトを実行する必要があります。 スクリプトが含まれている、 [Microsoft ギャラリー](https://gallery.technet.microsoft.com/scriptcenter/Cleanup-Script-for-Windows-95101439)します。 手動のクリーンアップ プロセスを完了するために、サーバーの VMM ID を書き留めてください。
- **クラスター内の VMM サーバー**: クラスターに展開されている VMM サーバーの登録を解除する必要がある場合、次の操作します。

    - サーバーが接続されている場合は、接続されている VMM サーバーを削除、 **サーバー** ] タブをクリックします。 サーバー上のプロバイダーをアンインストールするには、各クラスター ノードにログインし、コントロール パネルからアンインストールします。 登録エントリを削除するには、クラスター内のすべてのパッシブ ノードで、前のセクションに記載されたクリーンアップ スクリプトを実行します。
    - サーバーが接続されていない場合は、すべてのクラスター ノードでクリーンアップ スクリプトを実行する必要があります。

### 接続されていない VMM サーバーの登録解除

削除する VMM サーバーで、次の手順を実行します。

1. Azure ポータルから VMM サーバーの登録を解除します。
2. VMM サーバーで、クリーンアップ スクリプトをダウンロードします。
3. [管理者として実行] オプションを使用して PowerShell を開き、既定 (LocalMachine) のスコープの実行ポリシーを変更します。
4. スクリプトの手順に従います。 

削除するサーバー上のクラウドとペアになっているクラウドが存在する VMM サーバーで、次の手順を実行します。

1. クリーンアップ スクリプトを実行し、ステップ 2 ～ 4 に従います。
2. 登録が解除されている VMM サーバーの VMM ID を指定します。 
3. このスクリプトは、VMM サーバーの登録情報とクラウドのペアリング情報を削除します。


## Hyper-V サイトでの Hyper-V サーバーの登録解除

(VMM サーバーがない) Hyper-V サイトの Hyper-V サーバー上にある仮想マシンを保護するために Azure Site Recovery がデプロイされている場合、次のように、コンテナーから Hyper-V サーバーの登録を解除できます。

1. Hyper-V サーバーに配置された仮想マシンの保護を無効にします。
2.  **サーバー** タブ Azure Site Recovery ポータルで、サーバーを選択 > を削除します。 この操作を実行するときに、このサーバーが Azure に接続されている必要はありません。
3. 次のスクリプトを実行して、サーバー上の設定をクリーンアップし、コンテナーからサーバーの登録を解除します。 

        pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }
    
            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host
        
            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }
        
            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }
        
            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'
        
            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."  
                    Remove-Item -Recurse -Path $registrationPath
                }
    
                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }
    
                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }
    
            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {   
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0] 
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd


## Hyper-V 仮想マシンの保護を停止する

Hyper-V 仮想マシンの保護を停止する場合は、保護を削除する必要があります。 保護を削除する方法によっては、コンピューターで手動で保護設定をクリーンアップする必要があります。 

### 保護を削除する

1.  **仮想マシン** ] タブをクリックして、クラウドのプロパティは、仮想マシンを選択 > **削除**します。
2.  **仮想マシンの削除の確認** ページにはいくつかのオプションがあります。

    - **保護を無効にする**-有効にして、このオプションを保存すると、仮想マシンは Site Recovery によって保護されなくします。 仮想マシンの保護設定は、自動的にクリーンアップされます。
    - **資格情報コンテナーから削除**— 仮想マシンは Site Recovery コンテナーからのみ削除されますが、このオプションを選択する場合。 仮想マシンのオンプレミスの保護設定には影響しません。 設定を手動でクリーンアップして保護設定を削除し、Azure サブスクリプションから仮想マシンを削除する必要があります。さらに、次の指示を使用して手動でクリーンアップする必要がある保護設定も削除する必要があります。

仮想マシンとそのハード ディスクを削除することを選択した場合は、それらはターゲットの場所から削除されます。

### 保護設定を手動でクリーンアップする (VMM サイト間)

選択した場合は **仮想マシンの管理を停止**, 、設定を手動でクリーンアップします。

1. プライマリ サーバーで VMM コンソールからこのスクリプトを実行し、プライマリ仮想マシンの設定をクリーンアップします。 VMM コンソールで、[PowerShell] ボタンをクリックし、VMM PowerShell コンソールを開きます。 SQLVM1 を仮想マシンの名前に置き換えます。

         $vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. 次のように、セカンダリ VMM サーバーでこのスクリプトを実行して、セカンダリ仮想マシンの設定をクリーンアップします。

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force

3. セカンダリ VMM サーバーで、スクリプトを実行した後に Hyper-V ホスト サーバー上の仮想マシンを更新し、セカンダリ仮想マシンが VMM コンソールで再検出されるようにします。
4. 上記の手順によって、VMM サーバーのみのレプリケーション設定がクリアされます。 仮想マシンの仮想マシン レプリケーションを削除する場合、 プライマリおよびセカンダリ仮想マシンの両方に対して以下の手順を実行する必要があります。 以下のスクリプトを実行してレプリケーションを削除し、SQLVM1 を仮想マシンの名前に置き換えます。

        Remove-VMReplication –VMName “SQLVM1”


### 保護設定を手動でクリーンアップする (オンプレミスの VMM サイトと Azure の間)

1. 次のように、ソース VMM サーバーでこのスクリプトを実行して、プライマリ仮想マシンの設定をクリーンアップします。

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. 上記の手順によって、VMM サーバーのみのレプリケーション設定がクリアされます。 VMM サーバーからレプリケーションを削除したら、このスクリプトを使用して Hyper-V ホスト サーバー上で実行されている仮想マシンのレプリケーションを削除してください。 SQLVM1 を仮想マシンの名前で、host01.contoso.com を Hyper-V ホスト サーバーの名前に置き換えます。

        $vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

### 保護設定を手動でクリーンアップする (Hyper-V サイトと Azure の間)

1. ソース Hyper-V ホスト サーバーで仮想マシンのレプリケーションを削除するには、このスクリプトを使用します。 SQLVM1 を仮想マシンの名前に置き換えます。

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

## VMware 仮想マシンまたは物理サーバーの保護を停止する

VMware 仮想マシンまたは物理サーバーの保護を停止する場合は、保護を削除する必要があります。 保護を削除する方法によっては、コンピューターで手動で保護設定をクリーンアップする必要があります。 

### 保護を削除する

1.  **仮想マシン** ] タブをクリックして、クラウドのプロパティは、仮想マシンを選択 > **削除**します。
2.  **仮想マシンの削除** オプションのいずれかを選択します。

    - **(復旧ドリルとボリューム サイズ変更) の保護を無効にする**— をのみを参照してくださいしている場合、このオプションを有効にします。
        - **仮想マシンのボリュームのサイズを変更**— 仮想マシンが重大な状態にボリュームのサイズを変更するとします。 このような場合、このオプションを選択します。 Azure での復旧ポイントを維持したまま、保護が無効になります。 マシンの保護を再度有効にすると、サイズが変更されたボリュームのデータが Azure に転送されます。
        - **フェールオーバーを実行する**-内部設置型 VMware 仮想マシンまたは物理サーバーから Azure にフェールオーバーを実行して環境をテストして後、は、内部設置型仮想マシンをもう一度保護を開始するには、このオプションを選択します。 このオプションにより各仮想マシンが無効になり、各マシンの保護を再度有効にしなければならなくなります。 以下の点に注意してください。
            - この設定を使用して仮想マシンを無効にしても、Azure のレプリカ仮想マシンには影響しません。
            - 仮想マシンからモビリティ サービスをアンインストールしないでください。
    
    - **保護を無効にする**-有効にして、このオプションを保存すると、マシンは Site Recovery によって保護されなくします。 マシンの保護設定は、自動的にクリーンアップされます。
    - **資格情報コンテナーから削除**-コンピューターは、Site Recovery コンテナーからのみ削除されますが、このオプションを選択する場合。 マシンのオンプレミスの保護設定には影響しません。 マシンの設定を削除したり、Azure のサブスクリプションから仮想マシンを削除したりするには、モビリティ サービスをアンインストールして、設定をクリーンアップする必要があります。
        ![オプションを削除します。](./media/site-recovery-manage-registration-and-protection/RegistrationProtection_RemoveVM.png)



















