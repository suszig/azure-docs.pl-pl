3.  **ソリューション エクスプ ローラー**, プロジェクト (ソリューションではありません) を右クリックして、選択 **追加 > Azure API アプリ クライアント**です。 

    ![](./media/app-service-api-dotnet-add-generated-client/03-add-azure-api-client-v3.png)
    
3.  **Azure API アプリ クライアントの追加** ダイアログ ボックスで、をクリックして **Azure API アプリからダウンロード**します。 

5. ドロップダウン リストから、呼び出す API アプリを選択します。 

7. Click **OK**. 

    ![生成画面](./media/app-service-api-dotnet-add-generated-client/04-select-the-api-v3.png)

    ウィザードによって、API メタデータ ファイルがダウンロードされ、API アプリを呼び出すための型指定されたインターフェイスが生成されます。

    ![生成中を示す画面](./media/app-service-api-dotnet-add-generated-client/05-metadata-downloading-v3.png)

    新しいフォルダーを参照してくださいコード生成が完了したら、 **ソリューション エクスプ ローラー**, 、API アプリの名前に置き換えます。 このフォルダーには、クライアント クラスとデータ モデルを実装するコードが含まれています。 

    ![生成完了画面](./media/app-service-api-dotnet-add-generated-client/06-code-gen-output-v3.png)

