<properties 
   pageTitle="プレビュー ポータルを使用してクラシック モードで静的プライベート IP を設定する方法 | Microsoft Azure"
   description="静的プライベート IP (DIP) とそれらをポータルのクラシック モードで管理する方法を理解します。"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/11/2015"
   ms.author="telmos" />

# プレビュー ポータルでの静的プライベート IP アドレス (クラシック) の設定方法

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] この記事では、従来のデプロイ モデルについて説明します。 こともできます [リソース マネージャーの配置モデルの静的なプライベート IP アドレスを管理](virtual-networks-static-private-ip-arm-pportal.md)します。

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

次のサンプルの手順では、単純な環境が既に作成されていると想定します。 このドキュメントに表示されている手順を実行する場合は、最初に説明されているテスト環境にビルド [vnet を作成する](virtual-networks-create-vnet-classic-pportal.md)です。

## VM 作成時に静的プライベート IP アドレスを指定する方法
という名前の VM を作成する *DNS01* で、 *フロント エンド* という名前の VNet のサブネット *TestVNet* の静的なプライベート ip アドレス *192.168.1.101*, 、次の手順に従います。

1. ブラウザーからの [http://portal.azure.com に移動し、必要に応じて、サインイン、Azure アカウントを使用しています。
2. をクリックして **新規** > **コンピューティング** > **Windows Server 2012 R2 Datacenter**, 、ことに注意して、 **展開モデルを選択して** 一覧既に表示されています **クラシック**, 、] をクリックし、 **作成**します。

    ![プレビュー ポータルでの VM の作成](./media/virtual-networks-static-ip-classic-pportal/figure01.png)

3.  **VM の作成** ブレードで、作成されるバーチャル マシンの名前を入力 (*DNS01* このシナリオでは)、ローカル管理者アカウントとパスワード。

    ![プレビュー ポータルでの VM の作成](./media/virtual-networks-static-ip-classic-pportal/figure02.png)

4. クリックして **オプションの構成** > **ネットワーク** > **仮想ネットワーク**, 、] をクリックし、 **TestVNet**します。 場合 **TestVNet** が利用できなくなったを使用しているかどうかを確認、 *中央アメリカ* 場所し、この記事の冒頭で説明されているテスト環境を作成します。

    ![プレビュー ポータルでの VM の作成](./media/virtual-networks-static-ip-classic-pportal/figure03.png)

5.  **ネットワーク** ブレードで、現在選択されているサブネットが確認 *フロント エンド*, 、順にクリックして **IP アドレス**, [ **IP アドレスの割り当て** ] をクリックして **静的**, と入力し、 *192.168.1.101* の **IP アドレス** 以下に示すようです。

    ![プレビュー ポータルでの VM の作成](./media/virtual-networks-static-ip-classic-pportal/figure04.png) 

6. をクリックして **[ok]** で、 **IP アドレス** ブレードで、をクリックして **[ok]** で、 **ネットワーク** ブレードをクリック **[ok]** で、 **省略可能な構成** ブレードです。
7.  **VM の作成** ブレードで、をクリックして **作成**します。 ダッシュ ボードに以下のタイルが表示されることを確認します。

    ![プレビュー ポータルでの VM の作成](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## VM 用の静的プライベート IP アドレス情報を取得する方法

上記の手順で作成された VM の静的プライベート IP アドレス情報を表示するには、次の手順を実行します。

1. Azure プレビュー ポータルからクリックして **すべてを参照** > **(クラシック) の仮想マシン** > **DNS01** > **すべての設定** > **IP アドレス** を以下に示すように割り当てる IP アドレスと IP アドレスを確認します。

    ![プレビュー ポータルでの VM の作成](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## VM から静的プライベート IP アドレスを削除する方法
上記で作成した VM から静的プライベート IP アドレスを削除するには、以下の手順に従います。
    
1.  **IP アドレス** 前に、示したブレードをクリックして **動的** の右側に **IP アドレスの割り当て**, 、順にクリックして **保存**, 、順にクリック **[はい]**します。

    ![プレビュー ポータルでの VM の作成](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## 既存の VM に静的プライベート IP アドレスを追加する方法
上記の手順を使用して作成した VM に静的プライベート IP アドレスを追加するには、次の手順に従います。

1.  **IP アドレス** 前に、示したブレードをクリックして **静的** の右側に **IP アドレスの割り当て**します。
2. 型 *192.168.1.101* の **IP アドレス**, 、順にクリックして **保存**, 、順にクリック **はい**します。

## 次のステップ

- について学習 [予約済みパブリック IP](../virtual-networks-reserved-public-ip) アドレス。
- について学習 [インスタンス レベル パブリック IP (ILPIP)](../virtual-networks-instance-level-public-ip) アドレス。
- 参照してください、 [予約済み IP REST Api](https://msdn.microsoft.com/library/azure/dn722420.aspx)します。
