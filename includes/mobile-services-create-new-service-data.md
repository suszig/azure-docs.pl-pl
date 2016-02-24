

次に、データ ストレージのメモリ内のリストを置き換える新しいモバイル サービスを作成します。 新しいモバイル サービスを作成するには、次のステップに従います。

1. ログイン、 [Azure クラシック ポータル](https://manage.windowsazure.com/)します。 
2.  ナビゲーション ウィンドウの下部にあるをクリックして **+ 新規**します。

    ![plus-new](./media/mobile-services-create-new-service-data/plus-new.png)

3.  展開 **コンピューティング** と **Mobile Service**, 、順にクリックして **作成**します。

    ![mobile-create](./media/mobile-services-create-new-service-data/mobile-create.png)

    これを表示、 **新しいモバイル サービス** ダイアログ。

4.   **モバイル サービスを作成する** ] ページで、[ **無料の 20 MB SQL データベースの作成**, で新しいモバイル サービスのサブドメイン名を入力、 **URL** テキスト ボックス、および待機の名前が確認します。 名前の確認が完了したら、右矢印ボタンをクリックして次のページに進みます。   

    ![mobile-create-page1](./media/mobile-services-create-new-service-data/mobile-create-page1.png)

    これを表示、 **データベース設定の指定** ページです。

    
    > [AZURE.NOTE] As part of this tutorial, you create a new SQL Database instance and server. You can reuse this new database and administer it as you would any other SQL Database instance. If you already have a database in the same region as the new mobile service, you can instead choose **Use existing Database** and then select that database. The use of a database in a different region is not recommended because of additional bandwidth costs and higher latencies.

5.   **名**, 、名前を入力の新しいデータベースを入力し、 **ログイン名**, 、管理者のログインであるという名前を次のように新しい SQL データベース サーバーの種類と、パスワードの確認し、プロセスを完了するチェック ボタンをクリックします。

    ![mobile-create-page2](./media/mobile-services-create-new-service-data/mobile-create-page2.png)

    
    > [AZURE.NOTE] When the password that you supply does not meet the minimum requirements or when there is a mismatch, a warning is displayed.  
    >
    > We recommend that you make a note of the administrator login name and password that you specify; you will need this information to reuse the SQL Database instance or the server in the future.

これで、モバイル アプリケーションで使用できる新しいモバイル サービスが作成されました。 次に、アプリケーション データを格納するための新しいテーブルを追加します。 このテーブルは、アプリケーションがメモリ内のコレクションの代わりに使用します。


