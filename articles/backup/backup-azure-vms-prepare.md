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

Azure 仮想マシン (VM) をバックアップする環境を準備するには、次の前提条件を満たす必要があります。 開始する場合は、 [の Vm をバックアップする](backup-azure-vms.md)します。 満たしていない場合は、次の手順で環境を準備してください。


## 1.バックアップ資格情報コンテナー

![バックアップ資格情報コンテナー](./media/backup-azure-vms-prepare/step1.png)

Azure 仮想マシンのバックアップを開始するには、まずバックアップ資格情報コンテナーを作成する必要があります。 資格情報コンテナーは、時間の経過と共に作成されるすべてのバックアップと復旧ポイントを格納するエンティティです。 資格情報コンテナーには、バックアップ対象の仮想マシンに適用されるバックアップ ポリシーも含まれています。

このイメージは、さまざまな Azure Backup エンティティ間の関係を示しています。
![Azure Backup のエンティティとの関係](./media/backup-azure-vms-prepare/vault-policy-vm.png)

バックアップ資格情報コンテナーを作成するには:

1. サインイン、 [Azure ポータル](http://manage.windowsazure.com/)します。

2. **[新規]** > **[Data Services]** > **[Recovery Services]** > **[バックアップ資格情報コンテナー]** > **[簡易作成]** の順にクリックします。 組織のアカウントに関連付けられたサブスクリプションが複数ある場合は、正しいアカウントを選択してバックアップ資格情報コンテナーに関連付けてください。 各 Azure サブスクリプションに複数のバックアップ資格情報コンテナーを保有し、保護する仮想マシンを編成できます。

3. **[名前]** ボックスに、コンテナーを識別する表示名を入力します。 これは、サブスクリプションごとに一意である必要があります。

4. **[リージョン]** ボックスで、コンテナーのリージョンを選択します。 資格情報コンテナーは、保護する仮想マシンと同じリージョンにある必要があります。 仮想マシンが別のリージョンにある場合は、資格情報コンテナーをそれぞれ作成します。 バックアップ データを格納するストレージ アカウントを指定する必要はありません。バックアップ資格情報コンテナーと Azure Backup サービスはこれを自動的に処理します。

    ![バックアップ資格情報コンテナーの作成](./media/backup-azure-vms-prepare/backup_vaultcreate.png)

5. **[資格情報コンテナーの作成]** をクリックします。 バックアップ資格情報コンテナーが作成されるまで時間がかかることがあります。 ポータルの下部にある状態通知を監視します。

    ![資格情報コンテナーのトースト通知の作成](./media/backup-azure-vms-prepare/creating-vault.png)

6. コンテナーが正常に作成されたことを確認するメッセージが表示されます。 **[復旧サービス]** ページに、コンテナーが **[アクティブ]** と表示されます。 コンテナーを作成したら、必ず適切なストレージの冗長オプションを選択してください。 詳細について [、バックアップ コンテナーにストレージの冗長オプションを設定する](backup-configure-vault.md#azure-backup---storage-redundancy-options)します。

    ![バックアップ資格情報コンテナーの一覧](./media/backup-azure-vms-prepare/backup_vaultslist.png)

7. バックアップ資格情報コンテナーをクリックして **[クイック スタート]** ページに進むと、Azure 仮想マシンのバックアップ手順が表示されます。

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

| オプション| 長所| 短所|
|------|----------|-------------|
| オプション 1: IP 範囲のホワイトリストへの登録| 追加のコストはありません。<br><br>NSG のアクセスを開くを使用して、 <i>セット AzureNetworkSecurityRule</i> コマンドレットです。| 時間の経過と共に変更 IP の範囲を複雑で、影響を受けるとして管理します。<br>ストレージだけでなく、Azure 全体へのアクセスを提供します。|
| オプション 2: HTTP プロキシ| ストレージをプロキシの詳細に制御 Url を許可します。<br>1 つの Vm へのポイントのインターネット アクセス<br>Azure の IP の対象とならない変更に対応します。| プロキシ ソフトウェアで VM を実行するための追加のコストが発生する。|

### VM のバックアップに HTTP プロキシを使用する

VM をバックアップする際、HTTPS API を使用してスナップショット管理コマンドがバックアップ拡張機能から Azure Storage に送信されます。 パブリック インターネットにアクセスできるように構成されるのはプロキシのみのため、このトラフィックは、拡張機能からプロキシを介してルーティングされる必要があります。
>[AZURE.NOTE] 使用するプロキシ ソフトウェアについて推奨事項はありません。 以降の構成手順と互換性があるプロキシを選択してください。

次の例では、パブリック インターネットに送信されるすべての HTTP トラフィックにプロキシ VM を使用するようにアプリ VM を構成する必要があります。 プロキシ VM は、仮想ネットワーク内の VM からの着信トラフィックを許可するように構成する必要があります。 最後に、NSG (*NSG-lockdown*) には、プロキシ VM からの発信インターネット トラフィックを許可する新しいセキュリティ規則が必要です。

![HTTP プロキシ デプロイメントを使用した NSG の図](./media/backup-azure-vms-prepare/nsg-with-http-proxy.png)

**A) 発信方向のネットワーク接続を許可する:**

1. Windows コンピューターの場合、管理者特権でのコマンド プロンプトで、次のコマンドを実行します。

    ```
    netsh winhttp set proxy http://<proxy IP>:<proxy port>
    ```

    これにより、コンピューター全体のプロキシ構成が設定され、すべての発信 HTTP/ HTTPS トラフィックに使用されます。

2. Linux マシン用に次の行を追加、 `/etc/環境` ファイル。

    ```
    http_proxy=http://<proxy IP>:<proxy port>
    ```

    次の行を追加、 `/etc/waagent.conf` ファイル。

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

このコマンドでは、NSG に例外を追加します。これにより、10.0.0.5 の任意のポートから、ポート 80 (HTTP) または 443 (HTTPS) 上の任意のインターネット アドレスに TCP トラフィックを送信できます。 パブリック インターネット上で特定のポートをヒットする必要がある場合に追加するように、 `- DestinationPortRange` もします。

*例で使用されている名前は、デプロイメントに適した詳細情報に置き換えてください。*

## 3.VM エージェント

![VM エージェント](./media/backup-azure-vms-prepare/step3.png)

Azure 仮想マシンのバックアップを開始する前に、Azure VM エージェントが仮想マシンに正しくインストールされていることを確認してください。 VM エージェントは仮想マシンを作成する際のオプション コンポーネントであるため、仮想マシンをプロビジョニングする前に、VM エージェントのチェック ボックスがオンになっていることを確認します。

### 手動によるインストールと更新

VM エージェントは、Azure ギャラリーから作成された仮想マシン内に既に存在しています。 しかし、オンプレミスのデータセンターから移行された仮想マシンには VM エージェントがインストールされていません。 このような仮想マシンについては、VM エージェントを明示的にインストールする必要があります。 詳細について [既存の仮想マシンに VM エージェントをインストールする](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx)します。

| **操作**| **Windows**| **Linux**|
| --- | --- | --- |
| VM エージェントのインストール| <li>ダウンロードし、インストール、 [エージェント MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)します。インストールを実行するには、管理者特権が必要です。<li>[VM プロパティを更新](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) をエージェントがインストールされていることを示します。| <li> 最新のインストール [Linux エージェント](https://github.com/Azure/WALinuxAgent) GitHub からです。インストールを実行するには、管理者特権が必要です。<li> [VM プロパティを更新](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) をエージェントがインストールされていることを示します。|
| VM エージェントの更新| 再インストールするだけでは、VM エージェントを更新、 [VM エージェント バイナリ](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)します。<br><br>バックアップ操作が実行されていないこと、VM エージェントの更新を確認します。| 上の指示に従って [Linux VM エージェントを更新 ](../virtual-machines-linux-update-agent.md)します。<br><br>バックアップ操作が実行されていないこと、VM エージェントの更新を確認します。|
| VM エージェントのインストールを検証します。| <li>移動し、 *C:\WindowsAzure\Packages* Azure VM 内のフォルダーです。<li>WaAppAgent.exe ファイルを検索する必要があります。<li>このファイルを右クリックして、**[プロパティ]** をクリックした後、**[詳細]** タブを選択します。製品バージョン] が 2.6.1198.718 をする必要がありますまたはそれ以降。| -|


詳細について、 [VM エージェント](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) と [をインストールする方法](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/)します。

### バックアップ拡張機能

仮想マシンをバックアップするために、Azure Backup サービスは VM エージェントの拡張機能をインストールします。 Azure Backup サービスは、バックアップ拡張機能のアップグレードと修正プログラムの適用をシームレスに自動実行します。

VM が実行されている場合、バックアップ拡張機能がインストールされます。 また、VM が実行されている場合は、アプリケーション整合性復旧ポイントを取得できる可能性が最も高くなります。 Azure Backup サービスは、VM がオフであり、拡張機能をインストールできない場合 (オフライン VM の場合) でも、VM のバックアップを続行します。 この場合、復旧ポイントは、前述した*クラッシュ整合性*復旧ポイントになります。


## 制限事項

- Azure リソース マネージャーに基づく (別名 IaaS V2) 仮想マシンのバックアップはサポートされません。
- 16 台以上のデータ ディスクを搭載した仮想マシンのバックアップはサポートされません。
- Premium Storage を使用した仮想マシンのバックアップはサポートされません。
- 予約済み IP アドレスはあるがエンドポイントが定義されていない仮想マシンのバックアップはサポートされません。
- 復元中に既存の仮想マシンを置き換えることはサポートされません。 まず既存の仮想マシンに関連付けられているディスクを削除し、次にバックアップからデータを復元します。
- リージョン間のバックアップと復元はサポートされません。
- Azure のすべてのパブリック領域に Azure Backup サービスを使用して仮想マシンのバックアップがサポートされている (を参照してください、 [チェックリスト](http://azure.microsoft.com/regions/#services) のサポートされているリージョン)。 目的のリージョンが現在サポートされていない場合は、資格情報コンテナーの作成時にドロップダウン リストに表示されません。
- オペレーティング システムのバージョンの選択でサポートされるのは、Azure Backup サービスを使用した仮想マシンのバックアップのみです。
  - **Linux**: を参照してください [Azure の動作保証済みディストリビューションの一覧](../virtual-machines-linux-endorsed-distributions.md)します。 他の個人所有の Linux ディストリビューションも、仮想マシン上で VM エージェントが動作する限り使用できます。
  - **Windows Server**: Windows Server 2008 R2 より前のバージョンがサポートされていません。
- マルチ DC 構成の一部であるドメイン コントローラー (DC) VM の復元は、PowerShell を通じてのみサポートされます。 詳細について [マルチ DC のドメイン コント ローラーを復元する](backup-azure-restore-vms.md#restoring-domain-controller-vms)します。
- 次のような特殊なネットワーク構成を持つ仮想マシンの復元は、PowerShell でのみサポートされています。 UI の復元ワークフローを使用して作成する VM には、復元操作の完了後、これらのネットワーク構成は含まれません。 詳細については、次を参照してください。 [Vm 特別なネットワーク構成を復元する](backup-azure-restore-vms.md#restoring-vms-with-special-netwrok-configurations)します。
    - ロード バランサー構成 (内部および外部の) での仮想マシン
    - 複数の予約済み IP アドレスを持つ仮想マシン
    - 複数のネットワーク アダプターを持つ仮想マシン

## 疑問がある場合

質問がある場合、または「希望する機能が [フィードバックの送信](http://aka.ms/azurebackup_feedback)します。

## 次のステップ

- [VM バックアップ インフラストラクチャを計画します。](backup-azure-vms-introduction.md)
- [仮想マシンをバックアップします。](backup-azure-vms.md)
- [仮想マシンのバックアップを管理します。](backup-azure-manage-vms.md)





