1.  オンラインへのサインイン [Microsoft Azure プレビュー ポータル](https://portal.azure.com/)します。
2.  ジャンプバーで、次のようにクリックします。 **新規**, 、順にクリック **データ + ストレージ**, 、クリックして **Azure DocumentDB**します。 
  
    ![[新規作成] ボタン、データ + 作成 ブレード内のストレージとデータの Azure DocumentDB を強調表示、データベースを作成する Azure プレビュー ポータルのスクリーン ショット + ストレージ ブレード](media/documentdb-create-dbaccount/ca1.png)  

3.  **新しい DocumentDB アカウント** ブレードで、DocumentDB アカウントに対して必要な構成を指定します。 
 
    ![新しい [DocumentDB] ブレードのスクリーン ショット](media/documentdb-create-dbaccount/ca3.png) 


    - In the **ID** box, enter a name to identify the DocumentDB account.  When the **ID** is validated, a green check mark appears in the **ID** box. The **ID** value becomes the host name within the URI. The **ID** may contain only lowercase letters, numbers, and the '-' character, and must be between 3 and 50 characters. Note that *documents.azure.com* is appended to the endpoint name you choose, the result of which will become your DocumentDB account endpoint.
    

    - The **Account Tier** lens is locked because DocumentDB supports a single standard account tier. For more information, see [DocumentDB pricing](http://go.microsoft.com/fwlink/p/?LinkID=402317&clcid=0x409).
    
    - For **Subscription**, select the Azure subscription that you want to use for the DocumentDB account. If your account has only one subscription, that account is selected by default.

    - In **Resource Group**, select or create a resource group for your DocumentDB account.  By default, a new Resource group will be created.  You may, however, choose to select an existing resource group to which you would like to add your DocumentDB account. For more information, see [Using the Azure Preview Portal to manage your Azure resources](resource-group-portal.md).
 
    - Use **Location** to specify the geographic location in which to host your DocumentDB account.   

4.  新しい DocumentDB アカウントのオプションが構成されると、クリックして **作成**します。  DocumentDB アカウントの作成には数分かかる場合があります。  状態を確認するには、スタート画面で進行状況を監視してください。  
    ![スタート画面でオンライン データベース クリエーターの [作成] タイルのスクリーン ショット](media/documentdb-create-dbaccount/ca4.png)  
  
    または、通知ハブから進行状況を監視できます。  

    ![迅速なデータベースの作成 - DocumentDB アカウントを作成中であることを示す通知ハブのスクリーンショット](media/documentdb-create-dbaccount/ca5.png)  

    ![DocumentDB アカウントが正常に作成され、リソース グループにデプロイされたことを示す通知ハブのスクリーンショット - オンライン データベース クリエーターの通知](media/documentdb-create-dbaccount/ca6.png)

5.  DocumentDB アカウントが作成されたら、オンライン ポータルの既定の設定で使用するための準備が整います。 DocumentDB アカウントの既定の一貫性に設定されているメモ **セッション**します。  クリックして既定の一貫性の設定を調整することができます、 **既定の一貫性** タイルを **DocumentDB アカウント** ブレードです。

    ![[リソース グループ] ブレードのスクリーン ショット - アプリケーション開発の開始](media/documentdb-create-dbaccount/ca7.png)  

[How to: Create a DocumentDB account]: #Howto
[Next steps]: #NextSteps
[documentdb-manage]:../articles/documentdb/documentdb-manage.md

