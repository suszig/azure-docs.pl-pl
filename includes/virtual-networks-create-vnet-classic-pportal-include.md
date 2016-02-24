## Azure プレビュー ポータルで従来の VNet を作成する方法

上記のシナリオに基づいて従来の VNet を作成するには、次の手順に従います。

1. ブラウザーからの [http://portal.azure.com に移動し、必要に応じて、サインイン、Azure アカウントを使用しています。
2. をクリックして **新規** > **ネットワーク** > **仮想ネットワークの**, 、ことに注意して、 **展開モデルを選択して** 一覧既に表示されています **クラシック**, 、] をクリックし、 **作成**, 、次の図に示すようにです。

    ![プレビュー ポータルでの VNet の作成](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)

3.  **仮想ネットワークの** ブレードで、種類、 **名前** クリックして、VNet の **アドレス空間**します。 VNet との最初のサブネットのアドレス空間の設定を構成し、クリックして **OK**します。 次の図では、このシナリオでの CIDR ブロック設定を示しています。

    ![アドレス空間のブレード](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)

4. クリックして **リソース グループ** 、VNet を追加するリソース グループを選択 **新しいリソース グループの作成** 、VNet を新しいリソース グループに追加します。 次の図は、リソースと呼ばれる新しいリソース グループのグループの設定を示します **TestRG**します。 リソース グループの詳細については、次を参照してください。 [Azure リソース マネージャーの概要](resource-group-overview.md/#resource-groups)します。

    ![リソース グループ ブレードの作成](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)

5. 必要に応じて、変更、 **サブスクリプション** と **場所** VNet に対して設定します。 

6. タイルとして、VNet を表示しないかどうか、 **スタート画面**, 、無効にする **スタート画面にピン**します。 

7. をクリックして **作成** という名前のタイルに注目して **を作成する仮想ネットワーク** 次の図に示すようにします。

    ![ポータルで VNet を作成](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)

8. VNet が作成されるのを待ち、以下のタイルが表示されたら、クリックしてサブネットを追加します。

    ![ポータルで VNet を作成](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)

9. 確認する必要があります、 **構成** 、VNet を次に示すようにします。 

    ![ポータルで VNet を作成](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)

10. をクリックして **サブネット** > **追加**, 、入力、 **名** を指定し、 **アドレス範囲 (CIDR ブロック)** サブネット、およびクリックの **[ok]**します。 次の図は、このシナリオの設定を示しています。

    ![プレビュー ポータルでの VNet の作成](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)
