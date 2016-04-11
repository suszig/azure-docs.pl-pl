<properties 
   pageTitle="プレビュー ポータルを使用して ARM モードで NSG を作成する方法 | Microsoft Azure"
   description="プレビュー ポータルを使用して ARM で NSG を作成してデプロイする方法について"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/11/2015"
   ms.author="telmos" />

# プレビュー ポータルを使用して NSG を管理する方法

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] この記事では、リソース マネージャーの配置モデルについて説明します。 こともできます [クラシック展開モデルに Nsg を作成](virtual-networks-create-nsg-classic-ps.md)します。

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## [デプロイ] をクリックして ARM テンプレートをデプロイする

この時点では、プレビューから、NSG を作成できません。 ただし、既存の NSG は管理します。 NSG を管理するためには、パブリック リポジトリで使用可能なサンプル テンプレートを使用して、上記のシナリオで説明したリソースを作成します。 展開 [このこのテンプレート](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), 、クリックして **Deploy to Azure**, 、必要に応じて、既定のパラメーター値を置き換えるし、ポータルの指示に従います。

## 既存の NSG に規則を作成する

プレビュー ポータルから既存の NSG に規則を作成するには、次の手順に従います。

1. ブラウザーからの [http://portal.azure.com に移動し、必要に応じて、サインイン、Azure アカウントを使用しています。
2. クリックして **参照 >** > **ネットワーク セキュリティ グループ**します。

![プレビュー ポータル - NSG](./media/virtual-networks-create-nsg-arm-pportal/figure1.png)

3. Nsg の一覧でクリックして **NSG フロント エンド** > **受信のセキュリティ規則**

![プレビュー ポータル - NSG-FrontEnd](./media/virtual-networks-create-nsg-arm-pportal/figure2.png)

4. 一覧で **受信のセキュリティ規則**, 、クリックして **追加**します。

![プレビュー ポータル - 規則の追加](./media/virtual-networks-create-nsg-arm-pportal/figure3.png)

5.  **受信用のセキュリティ ルールの追加** ブレードで、という名前のルールを作成 *web ルール* の優先順位を持つ *200* 経由でアクセスを許可する *TCP* ポートに *80* いずれかから任意の VM にソースで実行され] をクリックして **OK**します。 これらの設定のほとんどは、既定値が設定されています。

![プレビュー ポータル - 規則の設定](./media/virtual-networks-create-nsg-arm-pportal/figure4.png)

6. 数秒後に、NSG の新しい規則が表示されます。

![プレビュー ポータル - 新しい規則](./media/virtual-networks-create-nsg-arm-pportal/figure5.png)
