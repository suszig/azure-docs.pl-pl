1.  **ソリューション エクスプ ローラー**, プロジェクト (ソリューションではありません) を右クリックし、クリックして、 **発行**します。 

    ![プロジェクトの [発行] メニュー オプション](./media/app-service-api-pub-web-create/20-publish-gesture-v3.png)

2. クリックして、 **プロファイル** ] タブでをクリックし、 **Microsoft Azure API Apps (プレビュー)**します。 

    ![[Web の発行] ダイアログ](./media/app-service-api-pub-web-create/21-select-api-apps-for-deployment-v2.png)

3. クリックして **新規** 、Azure サブスクリプションに新しい API アプリをプロビジョニングします。

    ![既存の API サービス選択ダイアログ](./media/app-service-api-pub-web-create/23-publish-to-apiapps-v3.png)

4.  **API アプリの作成** ] ダイアログ ボックスで、次を入力します。

    -  **API アプリの名前**, 、このチュートリアルで使用する名前を入力します。 
    - 複数の Azure サブスクリプションがある場合は、使用するサブスクリプションを選択します。
    -  **App Service プラン**, を既存の App Service プランを選択するか選択 **App Service プランの新規作成** 新しいプランの名前を入力します。 
    -  **リソース グループ**, を既存のリソース グループから選択するかを選択、 **新しいリソース グループの作成** 、名前を入力します。 
    -  **アクセス レベル**, [ **だれでも利用できる**です。 Azure プレビュー ポータルを使用して後でアクセスを制限することができます。
    -  **地域**, 、近くのリージョンを選択します。  

    ![Microsoft Azure Web アプリの構成ダイアログ](./media/app-service-api-pub-web-create/24-new-api-app-dialog-v3.png)

5. をクリックして **OK** 、サブスクリプションの API アプリを作成します。 

    このプロセスには数分かかることがあるため、Visual Studio に確認ダイアログ ボックスが表示されます。  

6. クリックして **OK** 確認ダイアログ ボックスでします。 
 
    プロビジョニング プロセスにより、Azure サブスクリプションにリソース グループと API アプリが作成されます。 Visual Studio での進行状況の表示、 **Azure App Service アクティビティ** ウィンドウです。 

    ![[Azure App Service アクティビティ] ウィンドウによる状態の通知](./media/app-service-api-pub-web-create/26-provisioning-success-v3.png)
