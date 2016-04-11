## [デプロイ] をクリックして ARM テンプレートをデプロイする

Microsoft によって管理され、コミュニティにも公開されている Github リポジトリにアップロードされた定義済み ARM テンプレートは、再利用できます。 これらのテンプレートは、Github から直接デプロイすることもできるほか、ダウンロードしてユーザーのニーズに合わせて変更することもできます。 2 つのサブネットを含む VNet を作成するテンプレートをデプロイするには、次の手順に従います。

1. 移動し、ブラウザーから、 [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates)します。
2. テンプレートの一覧を下にスクロールし、 **101 2 サブネット**します。 チェック、 **README.md** ファイルを次のようにします。

    ![Github の READEME.md ファイル](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. クリックして **を Azure にデプロイ**します。 必要な場合は、Azure ログイン資格情報を入力します。 
4.  **パラメーター** ブレードで、クリックして、新しい VNet の作成に使用する値を入力して **OK**します。 次の図で、このシナリオの値を示します。

    ![ARM テンプレートのパラメーター](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

4. クリックして **リソース グループ** 、VNet を追加するリソース グループを選択 **[新規作成** 、VNet を新しいリソース グループに追加します。 リソース グループの詳細については、次を参照してください。 []()します。 次の図は、リソースと呼ばれる新しいリソース グループのグループの設定を示します **TestRG**します。

    ![リソース グループ](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

5. 必要に応じて、変更、 **サブスクリプション** と **場所** VNet に対して設定します。
6. タイルとして、VNet を表示しないかどうか、 **スタート画面**, 、無効にする **スタート画面にピン**します。
5. をクリックして **Leagl 条項**, 条項を読み] をクリックして **購入** ことに同意します。 
6. クリックして **作成** 、VNet を作成します。

    ![プレビュー ポータルでのデプロイ タイルの送信](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

7. 展開が完了すると、クリックして **TestVNet** > **すべての設定** > **サブネット** を次に示すように、サブネットのプロパティを参照してください。

    ![プレビュー ポータルでの VNet の作成](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.gif)
