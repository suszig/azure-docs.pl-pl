<properties
    pageTitle="Windows を実行するカスタム仮想マシンを作成する | Microsoft Azure"
    description="Azure で Windows を実行するカスタム仮想マシンを作成する方法について説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>


<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2015"
    ms.author="cynthn"/>

#Azure で Windows を実行するカスタム仮想マシンの作成

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


A *カスタム* 仮想マシンを作成する仮想マシンを意味します **ギャラリーから** オプションよりも多くの構成の選択ができるため、 **簡易作成** オプション。 選択できるオプションは次のとおりです。

- 仮想ネットワークへ仮想マシンを接続する
- VM エージェントとマルウェア対策用などの拡張機能のインストール
- 仮想マシンを既存のクラウド サービスに追加する。
- 仮想マシンを既存のストレージ アカウントに追加する。
- 仮想マシンを可用性セットに追加する。

> [AZURE.IMPORTANT] 仮想マシンのホスト名またはクロスプレミス接続の設定した直接接続できますが、仮想ネットワークを使用する場合は、仮想マシンを作成するときに、仮想ネットワークを指定することを確認します。 仮想マシンが仮想ネットワークに接続するように設定できるのは、仮想マシンの作成時に限られています。 仮想ネットワークの詳細については、「 [仮想ネットワークの概要](virtual-networks-overview.md)します。

##仮想マシンを作成するには

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

