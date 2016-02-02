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
- [Azure Portal - Windows](virtual-machines-windows-tutorial.md)
- [Azure PowerShell](virtual-machines-ps-create-preconfigure-windows-resource-manager-vms.md)
- [Azure PowerShell - Template](virtual-machines-create-windows-powershell-resource-manager-template.md)
- [Azure Portal - Linux](virtual-machines-linux-tutorial-portal-rm.md)
- [Azure CLI](virtual-machines-linux-tutorial.md)


<br>


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。

Linux を実行する Azure 仮想マシン (VM) は簡単に作成できます。 このチュートリアルは、Azure ポータルを使用して、簡単に作成する方法を示しています、使用して、 `~/.ssh/id_rsa.pub` 公開キー ファイルをセキュリティで保護された、 **SSH** VM に接続します。 使用して Linux Vm を作成することもできます。 [テンプレートとして独自のイメージ](virtual-machines-linux-create-upload-vhd.md)します。
> [AZURE.NOTE] このチュートリアルでは、Azure リソース グループ API で管理される Azure 仮想マシンを作成します。 詳細については、「 [Azure リソース マネージャーの概要](resource-group-overview.md)します。

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

## イメージの選択

プレビュー ポータルで Azure Marketplace に移動して、必要な Windows Server VM イメージを検索します。

1. サインイン、 [プレビュー ポータル](https://portal.azure.com)します。

2. ハブ メニューで、**[新規]**、**[Compute]**、**[Ubuntu Server 14.04 LTS]** の順にクリックします。

    ![VM イメージの選択](media/virtual-machines-linux-tutorial-portal-rm/chooseubuntuvm.png)
    > [AZURE.TIP] その他のイメージを見つけるには、**[Marketplace]** をクリックし、使用可能な項目を検索するか、フィルターで抽出します。

3. **[Ubuntu Server 14.04 LTS]** ページの下部にある **[リソース マネージャー スタックを使用]** を選択して、Azure リソース マネージャーで VM を作成します。 新しいワークロードのほとんどについて、リソース マネージャー スタックを使用することをお勧めします。 考慮事項については、次を参照してください。 [Azure コンピューティング、ネットワーク、記憶域プロバイダー Azure リソース マネージャーにおける](virtual-machines-azurerm-versus-azuresm.md)します。

4. 次に、クリックして ![ボタンを作成する](media/virtual-machines-linux-tutorial-portal-rm/createbutton.png)します。

    ![リソース マネージャー コンピューティング スタックへの変更](media/virtual-machines-linux-tutorial-portal-rm/changetoresourcestack.png)

## 仮想マシンの作成

イメージを作成したら、ほとんどの構成で Azure の既定の設定を使用できるため、迅速に VM を作成できます。

1. **[仮想マシンの作成]** ブレードで **[基本]** をクリックします。 入力、 **名** VM、および公開キー ファイルに使用する (で **ssh rsa** 形式、ここでの `~/.ssh/id_rsa.pub` ファイル)。 複数のサブスクリプションがある場合は、新しい VM に対して 1 つ指定し、新規または既存の**リソース グループ**と、Azure データ センターの**場所**を指定します。

    ![](media/virtual-machines-linux-tutorial-portal-rm/step-1-thebasics.png)
    > [AZURE.NOTE] 公開キーと秘密キーの交換によって **ssh** セッションをセキュリティで保護しない場合は、ここでユーザー名/パスワードの認証を選択して、情報を入力する必要もあります。

2. **[サイズ]** をクリックし、ニーズに応じた VM サイズを選択します。 それぞれのサイズによって、コンピューティング コアの数、メモリ、および Premium Storage サポートなどの他の機能が指定されており、これが価格に影響します。 Azure では、ユーザーが選択したイメージに応じて、特定のサイズが自動的に推奨されます。 終了したら、クリックして ![選択ボタン](media/virtual-machines-linux-tutorial-portal-rm/selectbutton-size.png)します。
    >[AZURE.NOTE] Premium Storage は、特定のリージョンの DS シリーズの仮想マシンで使用できます。 Premium Storage は、データベースなどの高負荷のワークロードに最適なストレージ オプションです。 詳細については、「 [Premium Storage: Azure 仮想マシン ワークロード向けの高パフォーマンス ストレージ](storage-premium-storage-preview-portal.md)します。

3. **[設定]** をクリックして、新しい VM のストレージおよびネットワークの設定を表示します。 最初の VM については、通常、既定の設定をそのまま使用します。 Premium Storage がサポートされる VM サイズを選択した場合は、**[ディスクの種類]** の **[Premium (SSD)]** を選択することで、Premium Storage をお試しいただくことができます。 終了したら、クリックして ![[ok] ボタン](media/virtual-machines-linux-tutorial-portal-rm/okbutton.png)します。

    ![](media/virtual-machines-linux-tutorial-portal-rm/step-3-settings.png)

6. **[概要]** をクリックして、構成の選択内容を確認します。 確認または設定の更新が終了したら、クリックして ![[ok] ボタン](media/virtual-machines-linux-tutorial-portal-rm/createbutton.png) します。

    ![作成の概要](media/virtual-machines-linux-tutorial-portal-rm/summarybeforecreation.png)

8. Azure が VM を作成している間の進捗状況は、ハブ メニューの **[通知]** で確認できます。 Azure で VM が作成されるとスタート画面に表示されます。ただし、**[仮想マシンの作成]** ブレードで **[スタート画面にピン留めする]** をオフにした場合は表示されません。
    > [AZURE.NOTE] サービス管理コンピューティング スタックを使用して VM をクラウド サービス内に作成すると、パブリック DNS 名が概要に含まれますが、この概要にはパブリック DNS 名が含まれないことに注意してください。

## **ssh** での Azure Linux VM への接続

これで、**ssh** を使用して、標準的な方法で Ubuntu VM に接続できるようになりました。 ただし、その VM とそのリソースのタイルを開いて、Azure VM に割り当てられている IP アドレスを検出する必要があります。 これを行うには、**[参照]**、**[最近]** の順に選択して、作成した VM を探すか、スタート画面に作成されたタイルをクリックします。 いずれの場合も、次の図に示すように、**パブリック IP アドレス**の値を見つけてコピーします。

![正常な作成の概要](media/virtual-machines-linux-tutorial-portal-rm/successresultwithip.png)

これで、**ssh** を使用して Azure VM に接続できるようになったので、次に進むことができます。

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

> [AZURE.NOTE] ポータルで仮想マシンに完全修飾ドメイン名 (FQDN) を構成することもできます。 詳細について [、ポータルで Fqdn を作成する](virtual-machines-create-fqdn-on-portal.md)します。

## 次のステップ

Azure 上の Linux の詳細については、次の内容を参照してください。

- [Linux とオープン ソース Azure でのコンピューティング](virtual-machines-linux-opensource.md)

- [Mac および Linux 用 Azure コマンド ライン ツールを使用する方法](virtual-machines-command-line-tools.md)

- [Linux 向け Azure カスタム スクリプト拡張機能を使って LAMP アプリを展開します。](virtual-machines-linux-script-lamp.md)

- [Azure 上の Linux 用 Docker 仮想マシン拡張機能](virtual-machines-docker-vm-extension.md)





