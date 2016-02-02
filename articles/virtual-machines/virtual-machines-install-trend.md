<properties
    pageTitle="VM に Trend Micro Deep Security をインストールする | Microsoft Azure"
    description="この記事では、Azure でクラシック デプロイ モデルを使用して作成された VM で Trend Micro のセキュリティをインストールして構成する方法について説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/14/2015"
    ms.author="dkshir"/>



# Windows VM に Trend Micro Deep Security をサービスとしてインストールし、構成する方法

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


この記事では、Windows Server を実行している既存の仮想マシン (VM) または新しい VM に Trend Micro Deep Security as a Service をインストールして構成する方法を説明します。 Deep Security as a Service には、ウイルス対策、ファイアウォール、侵入防止システム、変更監視が含まれています。

このクライアントは、VM エージェントを使用することにより、セキュリティ拡張機能としてインストールされます。 新しい仮想マシンの場合には、Deep Security Agent と同時に VM エージェントをインストールします。 既存の仮想マシンで、VM エージェントがインストールされていない場合には、最初に VM エージェントをダウンロードしてインストールする必要があります。 この記事では、この両方の場合について説明します。

Trend Micro のオンプレミスのソリューション用サブスクリプションが既にある場合には、それを使用して Azure の仮想マシンを保護できます。 サブスクリプションがない場合には、サインアップして試用サブスクリプションを利用できます。 このソリューションの詳細については、Trend Micro ブログの投稿を参照してください。 [Microsoft Azure VM エージェント拡張機能の Deep Security](http://go.microsoft.com/fwlink/p/?LinkId=403945)します。

## 新しい仮想マシンに Deep Security Agent をインストールする

[Azure クラシック ポータル](http://manage.windowsazure.com) を使用する場合に、VM エージェントと Trend Micro のセキュリティ拡張機能をインストールすることができます、 **ギャラリーから** 仮想マシンを作成するにはオプションです。 仮想マシンを 1 つだけ作成する場合には、この方法が Trend Micro の保護機能を追加する最も簡単な方法です。

この **[ギャラリーから]** をクリックすると、仮想マシンの設定を支援するウィザードが起動します。 ウィザードの最後のページで、VM エージェントと Trend Micro のセキュリティ拡張機能をインストールします。 一般的な手順については、次を参照してください。 [Azure クラシック ポータルで Windows を実行する仮想マシンを作成する](virtual-machines-windows-tutorial-classic-portal.md)します。 ウィザードの最後のページで、次の手順を実行します。

1.  **[VM エージェント]** で **[VM エージェントをインストールする]** チェック ボックスをオンにします。

2.  **[セキュリティ拡張機能]** で **[Trend Micro Deep Security エージェント]** チェック ボックスをオンにします。

    ![VM エージェントと Deep Security エージェントのインストール](./media/virtual-machines-install-trend/InstallVMAgentandTrend.png)

3.  チェック マークをクリックして、仮想マシンを作成します。

## 既存の仮想マシンに Deep Security Agent をインストールする

このインストールには、次の条件が必要です。

- Azure PowerShell モジュール Version 0.8.2 以降がローカル コンピューターにインストールされていること。 インストールした Azure PowerShell のバージョンは、**Get-Module azure | format-table version** コマンドを使用して確認できます。 手順と最新バージョンへのリンクでは、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](../install-configure-powershell.md)します。

- VM エージェントがターゲットの仮想マシンにインストールされていること。

最初に、VM エージェントがインストールされていることを確認します。クラウド サービス名と仮想マシン名を入力して、管理者レベルの Azure PowerShell のコマンド プロンプトで、次のコマンドを実行します。置換を含む引用符内のすべての < と > 文字です。

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

クラウド サービスや仮想マシンの名前がわからない場合は、**Get-AzureVM** を実行します。現在のサブスクリプションのすべての仮想マシンの情報が表示されます。

**write-host** コマンドによって **True** が返された場合は、VM エージェントがインストールされています。 返された場合 **False**, 、指示し、Azure ブログの記事のダウンロードへのリンクを参照してください [VM エージェントと拡張機能 - パート 2](http://go.microsoft.com/fwlink/p/?LinkId=403947)します。

VM エージェントがインストールされている場合は、次のコマンドを実行します。

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA
    
    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## 次のステップ

エージェントがインストールされると、起動までに数分かかります。 起動後、仮想マシン上で Deep Security をアクティブにする必要があります。これは Deep Security Manager で管理できるようにするためです。 詳細については、次をご覧ください。

- このソリューションに関する trend の記事 [Microsoft Azure 用の Instant-On クラウド セキュリティ](http://go.microsoft.com/fwlink/?LinkId=404101)
- A [サンプル Windows PowerShell スクリプト](http://go.microsoft.com/fwlink/?LinkId=404100) 仮想マシンを構成するには
- [指示](http://go.microsoft.com/fwlink/?LinkId=404099)  サンプル

## その他のリソース

[Windows Server を実行する仮想マシンにログオンする方法]

[Azure VM 拡張機能と機能]




[how to log on to a virtual machine running windows server]: virtual-machines-log-on-windows-server.md 
[azure vm extensions and features]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409 

