## Azure プレビュー ポータルで VNet を作成する方法

Azure プレビュー ポータルを使用して、上記のシナリオに基づいた VNet を作成するには、次の手順に従います。

1. ブラウザーからの [http://portal.azure.com に移動し、必要に応じて、サインイン、Azure アカウントを使用しています。
2. をクリックして **新規** > **ネットワーク** > **仮想ネットワークの**, 、順にクリックして **リソース マネージャー** から、 **展開モデルを選択して** 一覧をクリックして **作成**, 、次の図に示すようにです。

    ![プレビュー ポータルでの VNet の作成](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure1.gif)

3.  **仮想ネットワークの作成** ブレードで、次の図に示すように、VNet 設定を構成します。

    ![仮想ネットワーク ブレードの作成](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure2.png)

4. クリックして **リソース グループ** 、VNet を追加するリソース グループを選択 **[新規作成** 、VNet を新しいリソース グループに追加します。 次の図は、リソースと呼ばれる新しいリソース グループのグループの設定を示します **TestRG**します。 リソース グループの詳細については、次を参照してください。 [Azure リソース マネージャーの概要](resource-group-overview.md/#resource-groups)します。

    ![リソース グループ](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure3.png)

5. 必要に応じて、変更、 **サブスクリプション** と **場所** VNet に対して設定します。 

6. タイルとして、VNet を表示しないかどうか、 **スタート画面**, 、無効にする **スタート画面にピン**します。 

7. をクリックして **作成** という名前のタイルに注目して **を作成する仮想ネットワーク** 次の図に示すようにします。

    ![仮想ネットワーク タイルの作成](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure4.png)

8. VNet を作成するを待つ、 **仮想ネットワークの** ブレードで、をクリックして **すべての設定** > **サブネット** > **追加** 以下に示すようです。

    ![プレビュー ポータルでのサブネットの追加](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure5.gif)

9. サブネットの設定を指定、 *バックエンド* クリックして、次のように、サブネット **OK**します。 

    ![サブネット設定](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure6.png)

10. 次の図に示すように、サブネットの一覧が表示されます。

    ![VNet 内のサブネットの一覧](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure7.png)
