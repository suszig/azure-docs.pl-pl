<properties
    pageTitle="仮想マシンのイメージについて | Microsoft Azure"
    description="Azure で仮想マシンのイメージを使用する方法について説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/13/2015"
    ms.author="cynthn"/>


# 仮想マシンのイメージについて

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。

イメージは、Azure でオペレーティング システムを持つ新しい仮想マシンを提供するために使用します。 また、イメージには 1 つ以上のデータ ディスクも保持できます。 イメージは複数のソースから利用できます。

-   Azure 内のイメージには、 [Marketplace](http://azure.microsoft.com/gallery/virtual-machines/)します。 最近のバージョンの Windows Server や Linux オペレーティング システムのディストリビューションがあります。 また、いくつかのイメージには、SQL Server などのアプリが含まれます。 MSDN 特典や MSDN の従量課金制サブスクライバーでは、その他のイメージにアクセスできます。
-   オープン ソース コミュニティからイメージを提供する [VM Depot](http://vmdepot.msopentech.com/List/Index)します。
-   また、既存の Azure 仮想マシンをキャプチャしてイメージとして使用するか、イメージをアップロードすることで、Azure 内で独自のイメージを使用することもできます。

## VM イメージと OS イメージについて

Azure では、*VM イメージ* と *OS イメージ*の 2 つの種類のイメージを使用できます。 VM イメージには、オペレーティング システムと、イメージの作成時に、仮想マシンにアタッチされているすべてのディスクが含まれています。 これは新しいタイプのイメージです。 VM イメージが導入される以前は、Azure 内のイメージが保持できるのは汎用オペレーティング システムのみで、追加のディスクは保持できませんでした。 汎用オペレーティング システムのみを含む VM イメージは、基本的に元のタイプのイメージと同じで、これを OS イメージといいます。

Azure 内の仮想マシンや、他の場所で稼働している仮想マシンを元にして、コピーおよびアップロードして独自のイメージを作成できます。 イメージを使用して、1 つ以上の仮想マシンを作成する場合は、準備としてイメージを一般化する必要があります。 Windows Server イメージを作成するには、.vhd ファイルをアップロードする前に、サーバー上で Sysprep コマンドを実行して一般化を行います。 Sysprep の詳細については、「 [sysprep の使用方法: An Introduction](http://go.microsoft.com/fwlink/p/?LinkId=392030)します。 Linux イメージを作成するには、Azure Linux エージェントを実行する他に、ソフトウェアのディストリビューションにより、固有のコマンドを実行する必要があります。

## イメージの操作

Azure サブスクリプションに使用できるイメージの管理には、Mac、Linux、Windows、Azure PowerShell モジュール用の Azure コマンド ライン インターフェイス (CLI) を使用することができます。 いくつかのイメージ タスクには Azure クラシック ポータルを使用することもできますが、コマンドラインの方が多くのオプションを使用できます。

リソース マネージャーの展開でこれらのツールを使用する方法については、次を参照してください。 [PowerShell と Azure CLI でイメージのナビゲーションと Azure の仮想マシンを選択する](resource-groups-vm-searching.md)します。

次にクラシック デプロイでのツールの使用方法の例を示します。

- CLI ので「Azure の仮想マシン イメージの管理用コマンド」を参照してください。 [Mac、Linux、および Azure サービス管理で Windows 用 Azure CLI の使用](virtual-machines-command-line-tools.md)
- Azure PowerShell については、次のコマンドの一覧をご覧ください。 VM を作成するイメージの検索の例を参照してください"の手順 3:: 確認、ImageFamily" [を作成し、Windows ベースの仮想マシンを事前構成する Azure PowerShell を使用して](virtual-machines-ps-create-preconfigure-windows-vms.md)

-   **すべてのイメージを取得**:`Get-azurevmimage`、現在のサブスクリプションで使用できるすべてのイメージの一覧を返します。 イメージだけでなく Azure またはパートナーから提供されているものです。 つまり、取得する一覧はかなり大きくなる可能性があります。 次の例では、取得する一覧を小さくする方法を示します。
-   **イメージ ファミリを取得**:`Get-azurevmimage | 選択 ImageFamily` 文字列を表示することによって、イメージ ファミリの一覧を取得 **ImageFamily** プロパティです。
-   **特定ファミリ内のすべてのイメージを取得**: `Get-azurevmimage |Where-object {$_ です。ImageFamily-eq $family}`
-   **VM イメージの検索**: `Get-azurevmimage |、{(gm – InputObject $_-名前 DataDiskConfigurations)-ne $null} |選択 - プロパティ ラベル ImageName` VM イメージにのみ適用される DataDiskConfiguration プロパティをフィルター処理によって機能します。 この例では、ラベルとイメージの名前のみの出力もフィルタリングします。
-   **一般化されたイメージを保存**: `保存 AzureVMImage – ServiceName"myServiceName"– 名前"MyVMtoCapture"– OSState は – ImageName「と」を「一般化」– ImageLabel"This is my 一般化されたイメージ"`
-   **専用のイメージを保存**: `保存 AzureVMImage – ServiceName"mySvc2"– 名前"MyVMToCapture2"– ImageName"myFirstVMImageSP"– OSState「特殊な」- 詳細`
>オペレーティング システム ディスクとデータ ディスクを含む VM イメージを作成する場合、[Azure.Tip]、OSState パラメーターが必要です。 パラメーターを使用しない場合、コマンドレットは、OS イメージを作成します。 パラメーターの値は、オペレーティング システム ディスクを再利用できるように準備されているかどうかによって、イメージを一般化するか、または専用化するかを示しています。
-   **イメージの削除**: `削除 AzureVMImage – ImageName"MyOldVmImage"`

## その他のリソース

[Linux 仮想マシンを作成するさまざまな方法](virtual-machines-linux-choices-create-vm.md)

[Windows 仮想マシンを作成するさまざまな方法](virtual-machines-windows-choices-create-vm.md)





