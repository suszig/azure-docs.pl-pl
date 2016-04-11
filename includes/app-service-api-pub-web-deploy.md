7. API アプリ プロジェクトを右クリックして **ソリューション エクスプ ローラー** 選択 **発行** 発行ダイアログ ボックスを開きます。 先ほど作成した発行プロファイルが選択されているはずです。 

9. クリックして **発行** 展開プロセスを開始します。 

    ![API アプリのデプロイ](./media/app-service-api-pub-web-deploy/26-5-deployment-success-v3.png)

     **Azure App Service アクティビティ** ウィンドウは、展開の進行状況を示しています。 

    ![[Azure App Service アクティビティ] ウィンドウの状態通知](./media/app-service-api-pub-web-deploy/26-5-deployment-success-v4.png)

    この展開プロセス中に Visual Studio に自動的に再起動に試行、 *ゲートウェイ*します。 ゲートウェイは、リソース グループ内のすべての API アプリの管理機能を処理する web アプリと API アプリの API 定義の変更を認識する再起動する必要があるか、 *apiapp.json* ファイルです。 
 
    API アプリを別の方法でデプロイする場合や Visual Studio によるゲートウェイの再起動が失敗する場合は、手動によるゲートウェイの再起動が必要になることがあります。 次の手順ではその方法について説明します。

1. ブラウザーでに移動、 [Azure プレビュー ポータル](https://portal.azure.com)します。 

2. 移動し、 **API アプリ** デプロイした API アプリのブレードです。

    については、 **API アプリ** ブレードで、表示、状態を確認する方法 [API アプリの管理](../articles/app-service-api/app-service-api-manage-in-portal.md)します。

4. クリックして、 **ゲートウェイ** リンクします。

3.  **ゲートウェイ** ブレードで、をクリックして **再起動**します。

    ![](./media/app-service-api-pub-web-deploy/restartgateway.png)

