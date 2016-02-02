<properties
    pageTitle="Windows VM でパスワードまたはリモート デスクトップをリセットする |Microsoft Azure"
    description="リソース マネージャーのデプロイ モデルで作成された Windows VM で管理者のパスワードまたはリモート デスクトップ サービスをリセットします。"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/14/2015"
    ms.author="dkshir"/>


# Windows 仮想マシンのパスワードまたはリモート デスクトップ サービスをリセットする方法

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。


パスワードを忘れたか、リモート デスクトップ サービスの構成に問題があるために Windows 仮想マシンに接続できない場合は、Azure ポータルまたは VMAccess 拡張機能を使用してローカル管理者パスワードをリセットするか、リモート デスクトップ サービスの構成をリセットします。

## Azure ポータル

リモート デスクトップ サービスをリセットする、 [Azure ポータル](https://portal.azure.com), 、] をクリックして **[すべて参照** > **仮想マシン (クラシック)** > *Windows 仮想マシン* > **リモート アクセスのリセット**します。 次のページが表示されます。


![](./media/virtual-machines-windows-reset-password/Portal-RDP-Reset-Windows.png)

名前とでローカル管理者アカウントのパスワードをリセットする、 [Azure ポータル](https://portal.azure.com), 、] をクリックして **すべてを参照** > **仮想マシン (クラシック)** > *Windows 仮想マシン* > **すべての設定** > **パスワード リセット**します。 次のページが表示されます。

![](./media/virtual-machines-windows-reset-password/Portal-PW-Reset-Windows.png)


## VMAccess 拡張機能と PowerShell

開始する前に、次の条件が必要です。

- Azure PowerShell モジュール Version 0.8.5 以降。インストールした Azure PowerShell のバージョンは、**Get-Module azure | format-table version** コマンドで確認できます。手順と最新バージョンへのリンクでは、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](http://go.microsoft.com/fwlink/p/?linkid=320552&clcid=0x409)します。
- 新しいローカル管理者アカウント パスワード。 リモート デスクトップ サービスの構成をリセットする場合、これは必要ありません。
- VM エージェント。

VMAccess 拡張機能は使用前にインストールする必要はありません。 VM エージェントが仮想マシンにインストールされていれば、**Set-AzureVMExtension** コマンドレットを使用する Azure PowerShell コマンドを実行すると自動的に拡張機能が読み込まれます。

最初に、VM エージェントがインストールされていることを確認します。クラウド サービス名と仮想マシン名を追加して、管理者レベルの Azure PowerShell のコマンド プロンプトで、次のコマンドを実行します。置換を含む引用符内のすべての < と > 文字です。

    $csName = "<cloud service name>"
    $vmName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $csName -Name $vmName
    write-host $vm.VM.ProvisionGuestAgent

クラウド サービスや仮想マシンの名前がわからない場合は、**Get-AzureVM** を実行します。現在のサブスクリプションのすべての仮想マシンの情報が表示されます。

**write-host** コマンドで **True** が表示される場合は、VM エージェントがインストールされています。表示される場合 **False**, 、手順とダウンロードへのリンクを参照してください、 [VM エージェントと拡張機能 - パート 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure ブログの投稿です。

ポータルで仮想マシンを作成する場合は、次の追加のコマンドを実行します。

    $vm.GetInstance().ProvisionGuestAgent = $true

このコマンドは、次のセクションで **Set-AzureVMExtension** コマンドを実行する場合、"IaaS VM アクセス拡張機能を設定する前に、VM オブジェクトでゲスト エージェントのプロビジョニングを有効にする必要がある" というエラーを防ぐことができます。

これで、以下のタスクを実行できます。

- ローカル管理者アカウント パスワードをリセットする。
- リモート デスクトップ サービスの構成をリセットする。

## ローカル管理者アカウント パスワードのリセット

現在のローカル管理者アカウント名と新しいパスワードを追加し、次のコマンドを実行します。

    $cred=Get-Credential –Message "Type the name of the current local administrator account and the new password."
    Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password  | Update-AzureVM

- 現在のアカウントと異なる名前を入力すると、VMAccess 拡張機能によってローカル管理者アカウントの名前が変更されて、そのアカウントにパスワードが割り当てられます。さらに、リモート デスクトップのログオフが発行されます。
- ローカル管理者アカウントが無効になっている場合は、VMAccess 拡張機能によって有効に設定されます。

これらのコマンドはリモート デスクトップ サービスの構成もリセットします。

## リモート デスクトップ サービスの構成のリセット

リモート デスクトップ サービスの構成をリセットするには、次のコマンドを実行します。

    Set-AzureVMAccessExtension –vm $vm | Update-AzureVM

VMAccess 拡張機能によって仮想マシンで次の 2 つのコマンドが実行されます。

- **netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes**

    このコマンドで、組み込みの Windows ファイアウォール グループが有効になり、TCP ポート 3389 を使用するリモート デスクトップの着信トラフィックが許可されます。

- **Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0**

    このコマンドは fDenyTSConnections レジストリ値を 0 に設定します。これでリモート デスクトップ接続が有効になります。

リモート デスクトップ アクセスの問題が解決しなかった場合は、実行、 [Azure IaaS (Windows) 診断パッケージ](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)します。

1.  診断パッケージで、[**Microsoft Azure IaaS (Windows) 診断パッケージ**] をクリックして、新しい診断セッションを作成します。
2.  [**Azure VM で発生している問題**] ページで、[**Azure VM との RDP 接続 (再起動が必要)**] の問題を選択します。

詳細については、次を参照してください。、 [Microsoft Azure IaaS (Windows) 診断パッケージ](http://support.microsoft.com/kb/2976864) サポート技術情報の記事です。

参照してくださいにできない場合、Azure IaaS (Windows) 診断パッケージを実行するか、それを実行しても問題が解決しない [Windows ベースの Azure 仮想マシンへのリモート デスクトップのトラブルシューティングを行う接続](virtual-machines-troubleshoot-remote-desktop-connections.md)します。


## その他のリソース

[Azure VM 拡張機能と機能](virtual-machines-extensions-features.md)

[RDP または SSH で Azure の仮想マシンに接続します。](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Windows ベースの Azure 仮想マシンへのリモート デスクトップ接続に関するトラブルシューティングします。](virtual-machines-troubleshoot-remote-desktop-connections.md)





