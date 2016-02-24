<properties 
   pageTitle="プレビュー ポータルを使用して ARM モードで静的プライベート IP を設定する方法 | Microsoft Azure"
   description="プライベート IP (DIP) とそれらをプレビュー ポータルを使用して ARM モードで管理する方法を理解します。"
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

# プレビュー ポータルでの静的プライベート IP アドレスの設定方法

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] この記事では、リソース マネージャーの配置モデルについて説明します。 こともできます [従来のデプロイ モデルでの静的なプライベート IP アドレスを管理](virtual-networks-static-private-ip-classic-pportal.md)します。

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

次のサンプルの手順では、単純な環境が既に作成されていると想定します。 このドキュメントに表示されている手順を実行する場合は、最初に説明されているテスト環境にビルド [vnet を作成する](virtual-networks-create-vnet-arm-pportal.md)です。

## 静的プライベート IP アドレスをテストするために VM を作成する方法

プレビュー ポータルを使用して、リソース マネージャーのデプロイメント モードで VM を作成する際に、静的プライベート IP アドレスを設定することはできません。 まず、VM を作成してから、そのプライベート IP が静的になるように設定する必要があります。

という名前の VM を作成する *DNS01* で、 *フロント エンド* という名前の VNet のサブネット *TestVNet*, 、次の手順に従います。

1. ブラウザーからの [http://portal.azure.com に移動し、必要に応じて、サインイン、Azure アカウントを使用しています。
2. をクリックして **新規** > **コンピューティング** > **Windows Server 2012 R2 Datacenter**, 、ことに注意して、 **展開モデルを選択して** 一覧既に表示されています **リソース マネージャー**, 、] をクリックし、 **作成**, 、次の図に示すようにです。

    ![プレビュー ポータルでの VM の作成](./media/virtual-networks-static-ip-arm-pportal/figure01.png)

3.  **基本** ブレードで、作成されるバーチャル マシンの名前を入力 (*DNS01* このシナリオでは)、ローカル管理者アカウントとパスワードを次の図に示すようにします。

    ![[基本] ブレード](./media/virtual-networks-static-ip-arm-pportal/figure02.png)

4. 確認、 **場所** が選択されている *中央アメリカ*, 、順にクリックして **既存の選択** [ **リソース グループ**, 、順にクリックして **リソース グループ** 再度] をクリックし、 *TestRG*, 、順にクリック **[ok]**します。

    ![[基本] ブレード](./media/virtual-networks-static-ip-arm-pportal/figure03.png)

5.  **サイズを選択する** ブレードで、 **A1 標準**, 、] をクリックし、 **選択**します。

    ![[サイズの選択] ブレード](./media/virtual-networks-static-ip-arm-pportal/figure04.png) 

6.  **設定** ブレードで、次のプロパティが設定されているかどうかを確認、次の値を設定し、クリックして **OK**します。

    -**ストレージ アカウント**: *vnetstorage*
    - **ネットワーク**: *TestVNet*
    - **サブネット**: *フロント エンド*

    ![[サイズの選択] ブレード](./media/virtual-networks-static-ip-arm-pportal/figure05.png)  

7.  **概要** ブレードで、をクリックして **OK**します。 ダッシュ ボードに以下のタイルが表示されることを確認します。

    ![プレビュー ポータルでの VM の作成](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## VM 用の静的プライベート IP アドレス情報を取得する方法

上記の手順で作成された VM の静的プライベート IP アドレス情報を表示するには、次の手順を実行します。

1. Azure プレビュー ポータルで、次のようにクリックします。 **すべてを参照** > **仮想マシン** > **DNS01** > **すべての設定** > **ネットワーク インターフェイス** 表示されている唯一のネットワーク インターフェイスの順にクリックします。

    ![VM のデプロイ タイル](./media/virtual-networks-static-ip-arm-pportal/figure07.png)

2.  **ネットワーク インターフェイス** ブレードで、をクリックして **すべての設定** > **IP アドレス** に注目してください、 **割り当て** と **IP アドレス** 値。

    ![VM のデプロイ タイル](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## 既存の VM に静的プライベート IP アドレスを追加する方法
上記の手順を使用して作成した VM に静的プライベート IP アドレスを追加するには、次の手順に従います。

1.  **IP アドレス** 前に、示したブレードをクリックして **静的** [ **割り当て**します。
2. 型 *192.168.1.101* の **IP アドレス**, 、クリックして **保存**します。

    ![プレビュー ポータルでの VM の作成](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

>[AZURE.NOTE] クリックすると if **保存** 割り当てに設定されていることを確認する **動的**, 、入力した IP アドレスが使用中で既にことを意味します。 別の IP アドレスを試してください。

## VM から静的プライベート IP アドレスを削除する方法
上記で作成した VM から静的プライベート IP アドレスを削除するには、以下の手順に従います。
    
1.  **IP アドレス** 前に、示したブレードをクリックして **動的** [ **割り当て**, 、順にクリック **保存**します。

## 次のステップ

- について学習 [予約済みパブリック IP](../virtual-networks-reserved-public-ip) アドレス。
- について学習 [インスタンス レベル パブリック IP (ILPIP)](../virtual-networks-instance-level-public-ip) アドレス。
- 参照してください、 [予約済み IP REST Api](https://msdn.microsoft.com/library/azure/dn722420.aspx)します。
