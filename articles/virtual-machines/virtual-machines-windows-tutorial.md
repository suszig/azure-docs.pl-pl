<properties
    pageTitle="Windows を実行する仮想マシンを Azure ポータルで作成する | Microsoft Azure"
    description="Azure ポータルで Azure Marketplace を使用して Windows を実行する Azure VM (仮想コンピューター) を作成する方法について説明します。"
    keywords="azure vm,create a virtual machine,virtual computer,setting up a virtual machine"
    services="virtual-machines"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/14/2015"
    ms.author="cynthn"/>

# Windows を実行する仮想マシンを Azure ポータルで作成する#

> [AZURE.SELECTOR]
- [Windows azure ポータル](virtual-machines-windows-tutorial.md)
- [Azure PowerShell](virtual-machines-ps-create-preconfigure-windows-resource-manager-vms.md)
- [Azure PowerShell のテンプレート](virtual-machines-create-windows-powershell-resource-manager-template.md)
- [Azure ポータル - Linux](virtual-machines-linux-tutorial-portal-rm.md)
- [Azure CLI](virtual-machines-linux-tutorial.md)

<br>



このチュートリアルでは、Azure ポータルで Azure 仮想マシンをわずか数分で簡単に作成する方法を示します。 仮想マシンを作成するための例として、Windows Server 2012 R2 Datacenter イメージを使用しますが、これは Azure によって提供される多くのイメージの 1 つにすぎません。 イメージの選択肢は、サブスクリプションによって異なります。 たとえば、デスクトップ イメージは MSDN サブスクリプション会員のみが使用できますです。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。 リソース マネージャーの詳細については、次を参照してください。 [Azure リソース マネージャーの概要](resource-group-overview.md)します。

リソース マネージャーのテンプレートまたは自動化ツールと共に、独自のイメージを使用して仮想マシンを作成することもできます。 別の方法の詳細については、次を参照してください。 [Windows 仮想マシンを作成するさまざまな方法](virtual-machines-windows-choices-create-vm.md)します。

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

## ビデオ チュートリアル

以下は、このチュートリアルのガイドです。

[AZURE.VIDEO create-a-virtual-machine-running-windows-in-the-azure-preview-portal]

## 仮想マシンイメージを選択する

1. Azure ポータルにサインインします。

2. [ハブ] メニューをクリックして **新規** > **コンピューティング** > **Windows Server 2012 R2 Datacenter**します。

    ![プレビュー ポータルで使用できる Azure VM イメージを示すスクリーンショット](./media/virtual-machines-windows-tutorial/marketplace_new.png)

    >[AZURE.TIP] その他のイメージを検索するクリックして **Marketplace** し検索または使用可能な項目のフィルターのとします。

3.  **Windows Server 2012 R2 Datacenter** ] ページで、[ **展開モデルを選択**, [ **リソース マネージャー**します。 クリックして **作成**します。

    ![Azure VM で選択するデプロイを示すスクリーンショット](./media/virtual-machines-windows-tutorial/marketplace_search_select.png)

## 仮想マシンの作成

イメージを作成したら、ほとんどの構成で Azure の既定の設定を使用できるため、迅速に仮想マシンを作成できます。

1.  **仮想マシンを作成** ブレードで、をクリックして **基本**します。 入力、 **名前** 用仮想マシンを管理する **ユーザー名**, と強力な **パスワード**します。 複数のサブスクリプションがある場合は、既存と新規のバーチャル マシンだけでなく、新しいに対して 1 つを指定 **リソース グループ** と Azure データ センター **場所**します。

    ![Azure VM で構成する基本的な設定を示すスクリーンショット](./media/virtual-machines-windows-tutorial/create_vm_basics.PNG)

    >[AZURE.NOTE]**ユーザー名** サーバーを管理するために使用する管理者アカウントを参照します。 覚えやすい (また、他の人が予測しずらい) パスワードを作成します。 **ユーザー名とパスワード、バーチャル マシンにログオンする必要になります**します。

2. クリックして **サイズ** し、ニーズに適切な仮想マシンのサイズを選択します。 それぞれのサイズによって、コンピューティング コアの数、メモリ、および Premium Storage サポートなどの他の機能が指定されており、これが価格に影響します。 Azure では、ユーザーが選択したイメージに応じて、特定のサイズが自動的に推奨されます。

    ![選択できる Azure VM サイズを示すスクリーンショット](./media/virtual-machines-windows-tutorial/create_vm_size.PNG)

    >[AZURE.NOTE] Premium storage は、特定の地域で DS シリーズ仮想マシンで使用できます。 Premium Storage は、データベースなどの高負荷のワークロードに最適なストレージ オプションです。 詳細については、「 [Premium Storage: Azure 仮想マシン ワークロード向けの高パフォーマンス ストレージ](storage-premium-storage-preview-portal.md)します。

3. クリックして **設定** 、新しいバーチャル マシンの記憶域やネットワークの設定を表示します。 最初の仮想マシンについては、通常、既定の設定をそのまま使用します。 これをサポートする仮想マシンのサイズを選択した場合お試しいただく Premium Storage を選択して **Premium (SSD)** [ **の種類のディスク**します。

    ![Azure VM で構成できるオプションの機能を示すスクリーンショット](./media/virtual-machines-windows-tutorial/create_vm_settings.PNG)

6. クリックして **概要** を選択した構成を確認します。 確認または設定の更新が終了したら、クリックして **作成**します。

    ![Azure VM で選択できる構成の概要を示すスクリーンショット](./media/virtual-machines-windows-tutorial/create_vm_summary.PNG)

8. 進行状況を追跡するには Azure が仮想マシンを作成中に **通知**, 、ハブ メニュー。 仮想マシンが作成されるが表示されますが、スタート画面にオフにした場合 **スタート画面にピン** で、 **仮想マシンの作成** ブレードです。

## 仮想マシンへのログオン

仮想マシンを作成したら、その仮想マシンにログオンして、その設定や、そこで実行するアプリケーションを管理できます。

>[AZURE.NOTE] 要件とトラブルシューティングのヒントについては、次を参照してください。 [RDP または SSH で Azure 仮想マシンに接続する](https://msdn.microsoft.com/library/azure/dn535788.aspx)です。

1. まだサインインしていない場合は、Azure ポータルにサインインします。

2. スタート画面の仮想マシンをクリックします。 見つける必要がある場合は、クリックして **すべてを参照** > **最近使用したファイル** または **すべてを参照** > **仮想マシン**します。 その後、一覧から仮想マシンを選択します。

3. [仮想マシン] ブレードをクリックして **接続**します。

    ![Azure VM ブレードの [接続] ボタンがある場所を示すスクリーンショット](./media/virtual-machines-windows-tutorial/connect_vm_portal.png)

4. クリックして **開く** Windows Server 仮想マシンに対して自動的に作成されるリモート デスクトップ プロトコル ファイルを使用します。

5. クリックして **接続**します。

6. ユーザー名と仮想マシンを作成したときに設定したパスワードを入力し、クリックして **OK**します。

7. をクリックして **はい** 仮想マシンの id を確認します。

    これで、仮想マシンを他のサーバーとまったく同様に扱うことができます。

## 次のステップ

* Azure PowerShell と Azure CLI を使用して [検索し、仮想マシン イメージの選択](resource-groups-vm-searching.md)します。
* 仮想マシンとワークロードの展開および管理を使用して自動化 [Azure リソース マネージャー](virtual-machines-how-to-automate-azure-resource-manager.md) と [Azure リソース マネージャー テンプレート](http://azure.microsoft.com/documentation/templates/)します。

