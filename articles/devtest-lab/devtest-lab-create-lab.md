    <properties
    pageTitle="Create a DevTest Lab | Microsoft Azure"
    description="Create a new DevTest Lab lab for virtual machines"
    services="devtest-lab,virtual-machines"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic=「get-開始の記事」
    ms.date="11/01/2015"
    ms.author="tarcher"/>

# Azure DevTest ラボの作成

## 前提条件

DevTest ラボを作成するには、次のものが必要です。

- Azure サブスクリプション。 Azure の購入オプションについては、次を参照してください。 [Azure の購入方法](http://azure.microsoft.com/pricing/purchase-options/) または [無料評価版の 1 か月](https://azure.microsoft.com/pricing/free-trial/)します。 ラボを作成するには、サブスクリプションの所有者である必要があります。
- ラボ用の Azure リソース グループ。 参照してください [Azure リソース マネージャーの概要](resource-group-overview.md) と [の管理とリソースへのアクセスを監査](./azure-portal/resource-group-rbac.md)します。


## ラボを作成する

1. サインイン、 [Azure プレビュー ポータル](http://portal.azure.com)します。

1. **[参照]** をタップします。

1. 一覧から **[DevTest ラボ]** をタップします。

1. **[DevTest ラボ]** ブレードで **[追加]** をタップします。

    ![DevTest ラボの追加](./media/devtest-lab-create-lab/add-lab-button.png)

1. **[DevTest ラボの作成]** ブレードで:

    1. 新しいラボの **[ラボ名]** を入力します。
    1. ラボに関連付ける **[サブスクリプション]** を選択します。
    1. ラボを格納する **[場所]** を選択します。
    1. **[作成]** をタップします。

    ![DevTest ラボ ブレードの作成](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

## 次のステップ

ラボを作成した後は、次の手順を考慮します。

- [DevTest ラボへのアクセスをセキュリティで保護された](devtest-lab-add-devtest-user.md)します。

- [Set lab policies](devtest-lab-set-lab-policy.md).

- [ラボ テンプレートを作成する](devtest-lab-create-template.md)します。

- [Vm 用のカスタムの成果物を作成](devtest-lab-artifact-author.md)します。

- [のアーティファクトで VM を Azure DevTest ラボ追加](devtest-lab-add-vm-with-artifacts.md)します。





