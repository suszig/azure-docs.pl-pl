<properties
    pageTitle="Azure Virtual Machines をバックアップする環境の準備 | Microsoft Azure"
    description="Azure Virtual Machines をバックアップする環境を準備します"
    services="backup"
    documentationCenter=""
    authors="Jim-Parker"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/17/2015"
    ms.author="trinadhk; aashishr; jimpark; markgal"/>

# Azure 仮想マシンをバックアップする環境の準備
Azure 仮想マシン (VM) をバックアップする環境を準備するには、次の前提条件を満たす必要があります。 開始する場合は、 [の Vm をバックアップする](backup-azure-vms.md)です。 満たしていない場合は、次の手順で環境を準備してください。


## 1.バックアップ資格情報コンテナー

![バックアップ資格情報コンテナー](./media/backup-azure-vms-prepare/step1.png)

Azure 仮想マシンのバックアップを開始するには、まずバックアップ資格情報コンテナーを作成する必要があります。 資格情報コンテナーは、時間の経過と共に作成されるすべてのバックアップと復旧ポイントを格納するエンティティです。 資格情報コンテナーには、バックアップ対象の仮想マシンに適用されるバックアップ ポリシーも含まれています。

このイメージは、さまざまな Azure Backup エンティティ間の関係を示しています。
![Azure Backup のエンティティとの関係](./media/backup-azure-vms-prepare/vault-policy-vm.png)

バックアップ資格情報コンテナーを作成するには:

1. [Azure ポータル](http://manage.windowsazure.com/)にサインインします。

2. クリックして **新しい** > **Data Services** > **復旧サービス** > **バックアップ コンテナー** > **簡易作成**します。 組織のアカウントに関連付けられたサブスクリプションが複数ある場合は、正しいアカウントを選択してバックアップ資格情報コンテナーに関連付けてください。 各 Azure サブスクリプションに複数のバックアップ資格情報コンテナーを保有し、保護する仮想マシンを編成できます。

3.  **名**, 、コンテナーを識別するフレンドリ名を入力します。 これは、サブスクリプションごとに一意である必要があります。

4.  **地域**, 、資格情報コンテナーのリージョンを選択します。 資格情報コンテナーは、保護する仮想マシンと同じリージョンにある必要があります。 仮想マシンが別のリージョンにある場合は、資格情報コンテナーをそれぞれ作成します。 バックアップ データを格納するストレージ アカウントを指定する必要はありません。バックアップ資格情報コンテナーと Azure Backup サービスはこれを自動的に処理します。

    ![バックアップ資格情報コンテナーの作成](./media/backup-azure-vms-prepare/backup_vaultcreate.png)

5. クリックして **コンテナーを作成して**します。 バックアップ資格情報コンテナーが作成されるまで時間がかかることがあります。 ポータルの下部にある状態通知を監視します。

    ![資格情報コンテナーのトースト通知の作成](./media/backup-azure-vms-prepare/creating-vault.png)

6. コンテナーが正常に作成されたことを確認するメッセージが表示されます。 表示されます、 **Recovery Services** としてページ **Active**します。 コンテナーを作成したら、必ず適切なストレージの冗長オプションを選択してください。 詳細について [、バックアップ コンテナーにストレージの冗長オプションを設定する](backup-configure-vault.md#azure-backup---storage-redundancy-options)です。

    ![バックアップ資格情報コンテナーの一覧](./media/backup-azure-vms-prepare/backup_vaultslist.png)

7. [バックアップ コンテナーに移動する] をクリックして、 **クイック スタート** ] ページで、Azure の仮想マシンをバックアップするための指示が表示されます。

    ![ダッシュボード ページの仮想マシンのバックアップ手順](./media/backup-azure-vms-prepare/vmbackup-instructions.png)



## 2.ネットワーク接続

![ネットワーク接続](./media/backup-azure-vms-prepare/step2.png)

バックアップ拡張機能が正常に機能するには、Azure パブリック IP アドレスへの接続が必要です。これは、Azure Storage エンドポイント (HTTP URL) に対して、VM のスナップショットを管理するコマンドを送信するためです。 適切なインターネット接続を利用できない場合、VM からの HTTP 要求はタイムアウトになり、バックアップ操作は失敗します。

### NSG を使用したネットワーク制限

(たとえば、ネットワーク セキュリティ グループ (NSG) を使用して) デプロイメントにアクセス制限が適用されている場合、バックアップ コンテナーへのバックアップ トラフィックが影響を受けないようにするには、追加の手順を実行する必要があります。

バックアップ トラフィック用のパスを指定するには、次の 2 つの方法があります。

1. ホワイト リスト、 [Azure データ センターの IP 範囲](http://www.microsoft.com/en-us/download/details.aspx?id=41653)します。
2. トラフィックをルーティングする HTTP プロキシをデプロイする。

管理の容易さ、細かな制御、およびコストの間のトレードオフは次のとおりです。

|オプション|長所|短所|
|------|----------|-------------|
|オプション 1: IP 範囲のホワイトリストへの登録| 追加のコストはありません。<br><br>NSG のアクセスを開くを使用して、 <i>セット AzureNetworkSecurityRule</i> コマンドレットを実行します。 | 時間の経過と共に変更 IP の範囲を複雑で、影響を受けるとして管理します。<br>記憶域だけでなく、Azure 全体へのアクセスを提供します。|
|オプション 2: HTTP プロキシ| ストレージをプロキシの詳細に制御 Url を許可します。<br>Vm への単一のポイントのインターネット アクセス。<br>Azure の IP アドレスを変更影響を受けないします。| プロキシ ソフトウェアで VM を実行するための追加のコストが発生する。|

### VM のバックアップに HTTP プロキシを使用する
VM をバックアップする際、HTTPS API を使用してスナップショット管理コマンドがバックアップ拡張機能から Azure Storage に送信されます。 パブリック インターネットにアクセスできるように構成されるのはプロキシのみのため、このトラフィックは、拡張機能からプロキシを介してルーティングされる必要があります。

>[AZURE.NOTE] 使用するプロキシのソフトウェアの推奨事項はありません。 以降の構成手順と互換性があるプロキシを選択してください。

次の例では、パブリック インターネットに送信されるすべての HTTP トラフィックにプロキシ VM を使用するようにアプリ VM を構成する必要があります。 プロキシ VM は、仮想ネットワーク内の VM からの着信トラフィックを許可するように構成する必要があります。 Nsg 最後に、(という名前 *NSG ロックダウン*)、新しいセキュリティ ルール プロキシ VM からの発信インターネット トラフィックを許可する必要があります。

![HTTP プロキシ デプロイメントを使用した NSG の図](./media/backup-azure-vms-prepare/nsg-with-http-proxy.png)

**A) 発信方向のネットワーク接続を許可する:**

1. Windows コンピューターの場合、管理者特権でのコマンド プロンプトで、次のコマンドを実行します。

    ```
    netsh winhttp set proxy http://<proxy IP>:<proxy port>
    ```

    これにより、コンピューター全体のプロキシ構成が設定され、すべての発信 HTTP/ HTTPS トラフィックに使用されます。

2. Linux コンピューターの場合、次の行を ```/etc/environment``` ファイルに追加します。

    ```
    http_proxy=http://<proxy IP>:<proxy port>
    ```

    次の行を ```/etc/waagent.conf``` ファイルに追加します。

    ```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

**B) Allow incoming connections on the proxy server:**

1. Open Windows Firewall on the proxy server. Right-click  **Inbound Rules** and click **New Rule...**.

    ![Open the Firewall](./media/backup-azure-vms-prepare/firewall-01.png)

    ![Create a new rule](./media/backup-azure-vms-prepare/firewall-02.png)
2. In the **New Inbound Rule Wizard**, choose the **Custom** option for the **Rule Type** and click **Next**. On the page to select the **Program**, choose **All Programs** and click **Next**.

3. On the **Protocol and Ports** page, use the inputs in the table below and click **Next**:

    ![Create a new rule](./media/backup-azure-vms-prepare/firewall-03.png)

| Input field | Value |
| --- | --- |
| Protocol type | TCP |
| Local port    | Select **Specific Ports** in the dropdown. In the text box, enter the ```<Proxy Port>``` that has been configured. |
| Remote port   | Select **All Ports** in the dropdown. |

For the rest of the wizard, click all the way to the end and give this rule a name.

**C) Add an exception rule to the NSG:**

In an Azure PowerShell command prompt, type out the following command:

```
Get AzureNetworkSecurityGroup の名前を「ロックダウン NSG」|
セット AzureNetworkSecurityRule-「できるようにプロキシ」という名前のアクションが許可する - TCP プロトコル-送信を入力-優先順位 200 SourceAddressPrefix「10.0.0.5/32」- SourcePortRange"*"DestinationAddressPrefix インターネット - DestinationPortRange"80-443"
```

This command adds an exception to the NSG, which allows TCP traffic from any port on 10.0.0.5 to any Internet address on port 80 (HTTP) or 443 (HTTPS). If you need to hit a specific port in the public Internet, make sure that you add that to the ```-DestinationPortRange``` as well.

*Ensure that you replace the names in the example with the details appropriate to your deployment.*

## 3. VM agent

![VM agent](./media/backup-azure-vms-prepare/step3.png)

Before you can back up the Azure virtual machine, you should ensure that the Azure VM agent is correctly installed on the virtual machine. Since the VM agent is an optional component at the time that the virtual machine is created, ensure that the check box for the VM agent is selected before the virtual machine is provisioned.

### Manual installation and update

The VM agent is already present in VMs that are created from the Azure gallery. However, virtual machines that are migrated from on-premises datacenters would not have the VM agent installed. For such VMs, the VM agent needs to be installed explicitly. Read more about [installing the VM agent on an existing VM](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

| **Operation** | **Windows** | **Linux** |
| --- | --- | --- |
| Installing the VM agent | <li>Download and install the [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). You will need Administrator privileges to complete the installation. <li>[Update the VM property](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) to indicate that the agent is installed. | <li> Install the latest [Linux agent](https://github.com/Azure/WALinuxAgent) from GitHub. You will need Administrator privileges to complete the installation. <li> [Update the VM property](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) to indicate that the agent is installed. |
| Updating the VM agent | Updating the VM agent is as simple as reinstalling the [VM agent binaries](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br><br>Ensure that no backup operation is running while the VM agent is being updated. | Follow the instructions on [updating the Linux VM agent ](../virtual-machines-linux-update-agent.md). <br><br>Ensure that no backup operation is running while the VM agent is being updated. |
| Validating the VM agent installation | <li>Navigate to the *C:\WindowsAzure\Packages* folder in the Azure VM. <li>You should find the WaAppAgent.exe file present.<li> Right-click the file, go to **Properties**, and then select the **Details** tab. The Product Version field should be 2.6.1198.718 or higher. | - |


Learn about the [VM agent](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) and [how to install it](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/).

### Backup extension

To back up the virtual machine, the Azure Backup service installs an extension to the VM agent. The Azure Backup service seamlessly upgrades and patches the backup extension without additional user intervention.

The backup extension is installed if the VM is running. A running VM also provides the greatest chance of getting an application-consistent recovery point. However, the Azure Backup service will continue to back up the VM--even if it is turned off, and the extension could not be installed (aka Offline VM). In this case, the recovery point will be *crash consistent* as discussed above.


## Limitations

- Backing up Azure Resource Manager-based (aka IaaS V2) virtual machines is not supported.
- Backing up virtual machines with more than 16 data disks is not supported.
- Backing up virtual machines using Premium storage is not supported.
- Backing up virtual machines with a reserved IP address and no defined endpoint is not supported.
- Replacing an existing virtual machine during restore is not supported. First delete the existing virtual machine and any associated disks, and then restore the data from backup.
- Cross-region backup and restore is not supported.
- Backing up virtual machines by using the Azure Backup service is supported in all public regions of Azure (see the [checklist](http://azure.microsoft.com/regions/#services) of supported regions). If the region that you are looking for is unsupported today, it will not appear in the dropdown list during vault creation.
- Backing up virtual machines by using the Azure Backup service is supported only for select operating system versions:
  - **Linux**: See [the list of distributions that are endorsed by Azure](../virtual-machines-linux-endorsed-distributions.md). Other Bring-Your-Own-Linux distributions also should work as long as the VM agent is available on the virtual machine.
  - **Windows Server**:  Versions older than Windows Server 2008 R2 are not supported.
- Restoring a domain controller (DC) VM that is part of a multi-DC configuration is supported only through PowerShell. Read more about [restoring a multi-DC domain controller](backup-azure-restore-vms.md#restoring-domain-controller-vms).
- Restoring virtual machines that have the following special network configurations is supported only through PowerShell. VMs that you create by using the restore workflow in the UI will not have these network configurations after the restore operation is complete. To learn more, see [Restoring VMs with special network configurations](backup-azure-restore-vms.md#restoring-vms-with-special-netwrok-configurations).
    - Virtual machines under load balancer configuration (internal and external)
    - Virtual machines with multiple reserved IP addresses
    - Virtual machines with multiple network adapters

## Questions?
If you have questions, or if there is any feature that you would like to see included, [send us feedback](http://aka.ms/azurebackup_feedback).

## Next steps

- [Plan your VM backup infrastructure](backup-azure-vms-introduction.md)
- [Back up virtual machines](backup-azure-vms.md)
- [Manage virtual machine backups](backup-azure-manage-vms.md)

