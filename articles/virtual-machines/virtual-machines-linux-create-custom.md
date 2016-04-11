<properties
    pageTitle="Linux VM の作成 | Microsoft Azure"
    description="Linux オペレーティング システムを実行するクラシック デプロイ モデルを使用してカスタム仮想マシンを作成する方法について説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="dsk-2015"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/14/2015"
    ms.author="dkshir"/>

# カスタム Linux VM を作成する方法

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーの model](virtual-machines-linux-tutorial.md) です。

このトピックを作成する方法について説明する *カスタム* クラシック展開モデルを使用して Azure CLI を使用して仮想マシン。 使用可能な Linux イメージを使用して **イメージ** Azure 上です。 Azure CLI では、次のような構成を選択できます。

- 仮想ネットワークと VM との接続
- 既存のクラウド サービスへの VM の追加
- 既存のストレージ アカウントへの VM の追加
- 可用性セットまたは場所への VM の追加

> [AZURE.IMPORTANT] 仮想マシンのホスト名またはクロスプレミス接続の設定した直接接続できますが、仮想ネットワークを使用する場合は、仮想マシンを作成するときに、仮想ネットワークを指定することを確認します。 仮想マシンが仮想ネットワークに接続するように設定できるのは、仮想マシンの作成時に限られています。 仮想ネットワークの詳細については、「 [仮想ネットワークの概要](http://go.microsoft.com/fwlink/p/?LinkID=294063)します。


## クラシック デプロイ モデルを使用して Linux 仮想マシンを作成する方法

[AZURE.INCLUDE [virtual-machines-create-LinuxVM](../../includes/virtual-machines-create-linuxvm.md)]

