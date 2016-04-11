<properties
    pageTitle="Linux を実行する仮想マシンを Azure クラシック ポータルで作成する | Microsoft Azure"
    description="Azure クラシック ポータルを使用して、Azure リソース グループで Linux を実行する Azure 仮想マシン (VM) を作成します。"
    services="virtual-machines"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2015"
    ms.author="rasquill"/>

# Linux を実行する仮想マシンを Azure ポータルを使用して作成する

> [AZURE.SELECTOR]
- [Windows azure ポータル](virtual-machines-windows-tutorial.md)
- [Azure PowerShell](virtual-machines-ps-create-preconfigure-windows-resource-manager-vms.md)
- [Azure PowerShell のテンプレート](virtual-machines-create-windows-powershell-resource-manager-template.md)
- [Azure ポータル - Linux](virtual-machines-linux-tutorial-portal-rm.md)
- [Azure CLI](virtual-machines-linux-tutorial.md)

<br>


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。

Linux を実行する Azure 仮想マシン (VM) は簡単に作成できます。 このチュートリアルは、Azure ポータルを使用して、簡単に作成する方法を示しています、使用して、 `~/.ssh/id_rsa.pub` 公開キー ファイルをセキュリティで保護された、 **SSH** VM に接続します。 使用して Linux Vm を作成することもできます。 [テンプレートとして独自のイメージ](virtual-machines-linux-create-upload-vhd.md)します。

> [AZURE.NOTE] このチュートリアルでは、Azure リソース グループ API で管理されている Azure の仮想マシンを作成します。 詳細については、「 [Azure リソース マネージャーの概要](resource-group-overview.md)します。

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

## イメージの選択

プレビュー ポータルで Azure Marketplace に移動して、必要な Windows Server VM イメージを検索します。

1. サインイン、 [プレビュー ポータル](https://portal.azure.com)します。

2. [ハブ] メニューをクリックして **新規** > **コンピューティング** > **Ubuntu Server 14.04 LTS**します。

    ![VM イメージの選択](media/virtual-machines-linux-tutorial-portal-rm/chooseubuntuvm.png)

    > [AZURE.TIP] その他のイメージを検索するクリックして **Marketplace** し検索または使用可能な項目のフィルターのとします。

3. 下部にある、 **Ubuntu Server 14.04 LTS** ] ページで、[ **リソース マネージャー スタックを使用する** Azure リソース マネージャーで VM を作成します。 新しいワークロードのほとんどについて、リソース マネージャー スタックを使用することをお勧めします。 考慮事項については、次を参照してください。 [Azure コンピューティング、ネットワーク、記憶域プロバイダー Azure リソース マネージャーにおける](virtual-machines-azurerm-versus-azuresm.md)します。

4. 次に、をクリックします。 ![[作成] ボタン](media/virtual-machines-linux-tutorial-portal-rm/createbutton.png).

    ![リソース マネージャー コンピューティング スタックへの変更](media/virtual-machines-linux-tutorial-portal-rm/changetoresourcestack.png)

## 仮想マシンの作成

イメージを作成したら、ほとんどの構成で Azure の既定の設定を使用できるため、迅速に VM を作成できます。

1.  **仮想マシンを作成** ブレードで、をクリックして **基本**します。 入力、 **名前** VM、および公開キー ファイルに使用する (で **ssh rsa** 形式、ここでの `~/.ssh/id_rsa.pub` ファイル)。 複数のサブスクリプションがあれば、指定の新しい VM だけでなく、新しいまたは既存の **リソース グループ** と Azure データ センター **場所**します。

    ![](media/virtual-machines-linux-tutorial-portal-rm/step-1-thebasics.png)

    > [AZURE.NOTE] ここでのユーザー名/パスワード認証の選択をセキュリティで保護したくない場合は、その情報を入力もあります、 **ssh** パブリックとプライベート キーの交換とセッションです。

2. クリックして **サイズ** のニーズに応じた VM サイズを選択します。 それぞれのサイズによって、コンピューティング コアの数、メモリ、および Premium Storage サポートなどの他の機能が指定されており、これが価格に影響します。 Azure では、ユーザーが選択したイメージに応じて、特定のサイズが自動的に推奨されます。 終了したら、クリックして ![選択ボタン](media/virtual-machines-linux-tutorial-portal-rm/selectbutton-size.png)します。

    >[AZURE.NOTE] Premium storage は、特定の地域で DS シリーズ仮想マシンで使用できます。 Premium Storage は、データベースなどの高負荷のワークロードに最適なストレージ オプションです。 詳細については、「 [Premium Storage: Azure 仮想マシン ワークロード向けの高パフォーマンス ストレージ](storage-premium-storage-preview-portal.md)します。

3. クリックして **設定** 新しいバーチャル マシンの記憶域やネットワークの設定を表示します。 最初の VM については、通常、既定の設定をそのまま使用します。 サポートされる VM サイズを選択した場合お試しいただく Premium Storage を選択して **Premium (SSD)** [ **の種類のディスク**します。 終了したら、クリックして ![[ok] ボタン](media/virtual-machines-linux-tutorial-portal-rm/okbutton.png)します。

    ![](media/virtual-machines-linux-tutorial-portal-rm/step-3-settings.png)

6. クリックして **概要** を選択した構成を確認します。 確認または設定の更新が終了したら、クリックして ![[ok] ボタン](media/virtual-machines-linux-tutorial-portal-rm/createbutton.png) します。

    ![作成の概要](media/virtual-machines-linux-tutorial-portal-rm/summarybeforecreation.png)

8. 進行状況を追跡するには Azure が VM を作成中に **通知**, 、ハブ メニュー。 Azure では、VM を作成、表示されます、スタート画面にオフにした場合を除き、 **スタート画面にピン** で、 **仮想マシンを作成** ブレードです。

    > [AZURE.NOTE] 概要がないこと、パブリック DNS 名はサービス管理コンピューティング スタックを使用するクラウド サービス内に VM を作成する方法に注意してください。

## 使用して、Azure Linux VM への接続 **ssh**

使用して Ubuntu VM に接続できるようになりました **ssh** で標準的な方法です。 ただし、その VM とそのリソースのタイルを開いて、Azure VM に割り当てられている IP アドレスを検出する必要があります。 行うことができますか、これをクリックして **参照**, を選択し、 **最近使用したファイル** と作成されると、VM の検索、または、スタート画面に自動的に作成、タイルをクリックするとします。 いずれの場合を見つけてコピー、 **パブリック IP アドレス** 値、次の図に示すようにします。

![正常な作成の概要](media/virtual-machines-linux-tutorial-portal-rm/successresultwithip.png)

できるようになりました **ssh** して Azure の仮想マシンに移動する準備ができたらとします。

    ssh ops@23.96.106.1 -p 22
    The authenticity of host '23.96.106.1 (23.96.106.1)' can't be established.
    ECDSA key fingerprint is bc:ee:50:2b:ca:67:b0:1a:24:2f:8a:cb:42:00:42:55.
    Are you sure you want to continue connecting (yes/no)? yes
    Warning: Permanently added '23.96.106.1' (ECDSA) to the list of known hosts.
    Welcome to Ubuntu 14.04.2 LTS (GNU/Linux 3.16.0-43-generic x86_64)

     * Documentation:  https://help.ubuntu.com/

      System information as of Mon Jul 13 21:36:28 UTC 2015

      System load: 0.31              Memory usage: 5%   Processes:       208
      Usage of /:  42.1% of 1.94GB   Swap usage:   0%   Users logged in: 0

      Graph this data and manage this system at:
        https://landscape.canonical.com/

      Get cloud support with Ubuntu Advantage Cloud Guest:
        http://www.ubuntu.com/business/services/cloud

    0 packages can be updated.
    0 updates are security updates.

    The programs included with the Ubuntu system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
    applicable law.

    ops@ubuntuvm:~$


> [AZURE.NOTE] ポータルで仮想マシンの完全修飾ドメイン名 (FQDN) を構成することもできます。 詳細について [、ポータルで Fqdn を作成する](virtual-machines-create-fqdn-on-portal.md)です。

## 次のステップ

Azure 上の Linux の詳細については、次の内容を参照してください。

- [Azure での Linux とオープン ソース コンピューティング](virtual-machines-linux-opensource.md)

- [Mac および Linux 用 Azure コマンド ライン ツールの使用方法](virtual-machines-command-line-tools.md)

- [Linux 向けに Azure カスタム スクリプト拡張機能を使って LAMP アプリをデプロイする](virtual-machines-linux-script-lamp.md)

- [Azure の Linux 用 Docker 仮想マシン拡張機能](virtual-machines-docker-vm-extension.md)

