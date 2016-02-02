<properties
 pageTitle="Azure VM エージェントおよび拡張機能 | Microsoft Azure"
 description="エージェントおよび拡張機能の概要と、クラシック デプロイ モデルを使用してエージェントをインストールする方法について説明します。"
 services="virtual-machines"
 documentationCenter=""
 authors="squillace"
 manager="timlt"
 editor=""
 tags="azure-service-management"/>

<tags
 ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="infrastructure-services"
 ms.date="09/22/2015"
 ms.author="rasquill"/>


# 仮想マシンのエージェントおよび拡張機能について

Azure 仮想マシン エージェント (VM エージェント) は、Azure 仮想マシン拡張機能 (VM 拡張機能) のインストール、構成、管理、および実行に使用します。 VM 拡張機能により、Microsoft とその他のサード パーティが提供する動的機能を使用できます。 エージェントと拡張機能は、主に、管理ポータルで追加されますが、使用することも、 [Powershell](../install-configure-powershell.md) コマンドレットまたは [AZURE-CLI](xplat-install.md) に追加し、VM を作成するときにまたは既存の vm のいずれかを構成します。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。



VM 拡張機能により以下を実行できます。

-   セキュリティおよび ID 機能の変更 (アカウント値のリセットやマルウェア対策の使用など)
-   監視および診断の開始、停止、構成
-   接続機能 (RDP や SSH など) のリセットまたはインストール
-   VM の診断、監視、管理

他にも多くの機能があります。VM 拡張機能は定期的にリリースされます。 この記事では、Windows および Linux 向けの Azure VM エージェントと、それらのエージェントの VM 拡張機能のサポート方法について説明します。 VM 拡張機能の機能分類別一覧については、次を参照してください。 [Azure VM 拡張機能と機能](virtual-machines-extensions-features.md)します。

## Windows および Linux 用 Azure VM エージェント

Azure Virtual Machines エージェント (VM エージェント) はセキュリティで保護された軽量のプロセスであり、イメージ ギャラリーから Azure Virtual Machines のインスタンスに対して、また VM エージェントをインストール済みのカスタム VM インスタンスに対して、VM 拡張機能のインストール、構成、削除を行います。 VM エージェントは、安全な Azure VM のローカル制御サービスとして機能します。 エージェントから読み込まれるされる拡張機能によって、インスタンスを使用して生産性を向上させる機能が提供されます。

Azure VM エージェントには、Windows VM 向けと Linux VM 向けの 2 種類があります。 既定では、VM エージェントはイメージ ギャラリーから VM を作成したときに自動でインストールされます。ただし、インスタンスの作成後、または自分でアップロードしたカスタム VM イメージへのインスタンスのインストール後に、VM エージェントをインストールすることもできます。
>[AZURE.IMPORTANT] これらの VM エージェントは、仮想マシン インスタンスを安全に管理できるようにする非常に軽量のサービスです。 VM エージェントを必要としない場合もあります。 この場合には、VM エージェントがインストールされていない VM を作成する必要があります。 VM エージェントは物理的に削除できますが、インスタンス上でのすべての VM 拡張機能の動作は定義されていません。 このため、インストールされた VM エージェントの削除は、この時点ではサポートされていません。

VM エージェントは、次のような状況で有効化されます。

-   管理ポータルの**簡易作成**の方法を使用して仮想マシンのインスタンスを作成した場合。または、管理ポータルの**カスタム作成**の方法を使用し、**[VM エージェントのインストール]** チェックボックスをオンにして (下記画像を参照) 仮想マシンのインスタンスを作成した場合。 詳細については、次を参照してください。 [カスタム仮想マシンを作成する方法](virtual-machines-create-custom.md)します。

    ![VM エージェントのチェックボックス](./media/virtual-machines-extensions-agent-about/IC719409.png "VM Agent Checkbox")

-   使用して仮想マシンのインスタンスを作成するとき、 [New-azurevm](https://msdn.microsoft.com/library/azure/dn495254.aspx) または [New-azurequickvm](https://msdn.microsoft.com/library/azure/dn495183.aspx) コマンドレットです。 VM を作成するには、VM エージェントを追加することによってインストールせず、 **– DisableGuestAgent** パラメーターを [Add-azureprovisioningconfig](https://msdn.microsoft.com/library/azure/dn495299.aspx) コマンドレットです。

-   VM エージェント (Window バージョンまたは Linux バージョンのいずれか) を手動でダウンロードして既存の VM インスタンスにインストールし、PowerShell または REST 呼び出しで **ProvisionGuestAgent** の値を **true** に設定した場合 (VM エージェントの手動インストール後にこの値を設定しない場合、VM エージェントを追加したことが正常に検出されません)。 次のコード例は、PowerShell を使用してこれを実行する方法を示しています。 ここで、 `$svc` と `$name` 引数は既に決定されています。

        $vm = Get-AzureVM –serviceName $svc –Name $name
        $vm.VM.ProvisionGuestAgent = $TRUE
        Update-AzureVM –Name $name –VM $vm.VM –ServiceName $svc

-   VM エージェントを Azure へアップロードする前にインストールした VM イメージを作成した場合。Windows VM では、ダウンロード、 [Windows VM Agent .msi ファイル](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) し、VM エージェントをインストールします。Linux vm の場合は、インストールするの github リポジトリから <https://github.com/Azure/WALinuxAgent>します。Linux VM エージェントをインストールする方法の詳細については、次を参照してください。、 [Azure Linux VM エージェント ユーザー ガイド](virtual-machines-linux-agent-user-guide.md)します。

>[AZURE.NOTE]PaaS では VM エージェントは **GuestAgent** と呼ばれ、Web ロール VM および worker ロール VM でいつでも使用できます (詳細については、を参照してください [Azure ロール アーキテクチャ](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx).)。 ロール VM 用の VM エージェントで、永続 Virtual Machines の場合と同じ方法でクラウド サービス VM に拡張機能を追加できるようになりました。 ロール VM の VM 拡張機能と永続 VM の VM 拡張機能の最も大きな違いは、ロール VM では、拡張機能がクラウド サービス、そのクラウド サービス内のデプロイの順に追加されることです。

>別の
>[Get-azureserviceavailableextension](https://msdn.microsoft.com/library/azure/dn722498.aspx)
>すべての利用可能なロール VM 拡張機能を一覧表示するコマンドレットを使用します。

## VM 拡張機能の検索、追加、更新、および削除

これらのタスクの詳細については、「 [追加、検索、更新、および拡張機能の削除](virtual-machines-extensions-install.md)します。





