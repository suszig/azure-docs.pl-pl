<properties
    pageTitle="カスタム仮想マシンを作成する | Microsoft Azure"
    description="クラシック デプロイ モデルを使用して Azure クラシック ポータルからカスタム仮想マシンを作成する方法を説明します。"
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
    ms.date="08/11/2015"
    ms.author="cynthn"/>

#カスタム仮想マシンを作成する方法


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。
 


A *カスタム* 仮想マシンを使用して作成する仮想マシンを意味します **ギャラリーから** オプションよりも多くの構成の選択ができるため、 **簡易作成** オプション。 選択できるオプションは次のとおりです。

- 仮想ネットワークへ仮想マシンを接続する
- マルウェア対策としてなどで Azure Virtual Machine Agent や Azure Virtual Machine Extensions をインストールする。
- 仮想マシンを既存のクラウド サービスに追加する。
- 仮想マシンを既存のストレージ アカウントに追加する。
- 仮想マシンを可用性セットに追加する。

> [AZURE.IMPORTANT] ホスト名によって直接接続またはクロスプレミス接続を設定できるように、仮想ネットワークを使用する仮想マシンをする場合は、仮想マシンの作成時に、仮想ネットワークを指定することを確認します。 仮想マシンが仮想ネットワークに接続するように設定できるのは、仮想マシンの作成時に限られています。 仮想ネットワークの詳細については、「 [Azure 仮想ネットワークの概要](virtual-networks-overview.md)します。

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

