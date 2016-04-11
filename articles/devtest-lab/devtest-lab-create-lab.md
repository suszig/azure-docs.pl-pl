    <properties
    pageTitle="Create a DevTest Lab | Microsoft Azure"
    description="Create a new DevTest Lab lab for virtual machines"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/01/2015"
    ms.author="tarcher"/>

# Azure DevTest ラボの作成

## 前提条件

DevTest ラボを作成するには、次のものが必要です。

- Azure サブスクリプション。 Azure の購入オプションについては、次を参照してください。 [Azure の購入方法](http://azure.microsoft.com/pricing/purchase-options/) または [無料評価版の 1 か月](https://azure.microsoft.com/pricing/free-trial/)します。 ラボを作成するには、サブスクリプションの所有者である必要があります。
- ラボ用の Azure リソース グループ。 参照してください [Azure リソース マネージャーの概要](resource-group-overview.md) と [の管理とリソースへのアクセスを監査](./azure-portal/resource-group-rbac.md)します。


## ラボを作成する

1. サインイン、 [Azure プレビュー ポータル](http://portal.azure.com)します。

1. タップ **参照**します。

1. タップ **DevTest ラボ** リストからです。

1.  **DevTest ラボ** ブレードで、tap **追加**します。

    ![DevTest ラボの追加](./media/devtest-lab-create-lab/add-lab-button.png)

1.  **DevTest ラボ作成** ブレード。

    1. 入力、 **ラボ名前** 新しいラボのです。
    1. 選択、 **サブスクリプション** 、ラボに関連付ける。
    1. 選択、 **場所** 、ラボを格納します。
    1. タップ **作成**します。

    ![DevTest ラボ ブレードの作成](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

## 次のステップ

ラボを作成した後は、次の手順を考慮します。

- [DevTest ラボへのアクセスをセキュリティで保護された](devtest-lab-add-devtest-user.md)します。

- [ラボのポリシーを設定する](devtest-lab-set-lab-policy.md)です。

- [ラボ テンプレートを作成](devtest-lab-create-template.md)します。

- [Vm 用のカスタムの成果物を作成](devtest-lab-artifact-author.md)します。

- [成果物と VM を Azure DevTest ラボ追加](devtest-lab-add-vm-with-artifacts.md)します。

