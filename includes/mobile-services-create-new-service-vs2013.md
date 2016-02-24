

次のステップでは、Azure で新しいモバイル サービスを作成し、アプリケーションをこの新しいサービスに接続するコードをプロジェクトに追加します。 Visual Studio 2013 は、お客様に代わって Azure に接続し、提供された資格情報を使って新しいモバイル サービスを作成します。 新しいモバイル サービスを作成するとき、アプリケーション データを格納するためにモバイル サービスで使用される Azure SQL Database を指定する必要があります。 


1. Visual Studio 2013 でソリューション エクスプ ローラーを開き、Windows ストア アプリ プロジェクトを右クリックし、クリックして **追加**, 、クリックして **接続済みサービス]**します。  

2. サービス マネージャーのダイアログ ボックスで、[ **サービスを作成する...**, 選択してから、 **& lt;... 管理 & gt;**   **サブスクリプション** モバイル サービスの作成] ダイアログ ボックス。  

    ![サービス管理サブスクリプションの作成](./media/mobile-services-create-new-service-vs2013/mobile-create-service-from-vs2013.png)

3. Microsoft Azure サブスクリプションの管理] をクリックして **署名イン...** (必要な場合)、Azure アカウントにサインインで使用可能なサブスクリプションを選択し、をクリックする **閉じる**します。

    サブスクリプションに 1 つ以上の既存のモバイル サービスが含まれている場合、サービス名が表示されます。 

5. 戻り、 **モバイル サービスの作成** ダイアログで、 **サブスクリプション**, 、 **JavaScript** でバックエンド **ランタイム** と **地域** 、モバイル サービスの入力、 **名前** 、モバイル サービスのです。

    >[AZURE.NOTE]モバイル サービスの名前は一意である必要があります。 赤い X が横に表示されます **名前** 指定した名前が利用できない場合。 

6.  **データベース**, [ **& lt;無料の SQL Database & gt; を作成**, 指定した後、 **サーバーのユーザー名**, 、**サーバーのパスワード**, 、および **サーバー パスワードの確認入力** ] をクリックし、 **作成**します。

    ![VS 2013 での新しいモバイル サービスの作成](./media/mobile-services-create-new-service-vs2013/mobile-create-service-from-vs2013-2.png)


    > [AZURE.NOTE]
    > As part of this tutorial, you create a new free SQL Database instance and server. You can reuse this new database and administer it as you would any other SQL Database instance. You can only have one free database instance. If you already have a database in the same region as the new mobile service, you can instead choose the existing database. When you choose an existing database, make sure that you supply correct login credentials. If you supply incorrect login credentials, the mobile service is created in an unhealthy state.

7. サービス マネージャーのリストから新しく作成したモバイル サービスを選択し、クリックして、モバイル サービスを作成した後 **OK**します。

    ウィザードの完了後に、必要な NuGet パッケージがインストールされ、Mobile Services クライアント ライブラリへの参照がプロジェクトに追加されて、プロジェクトのソース コードが更新されます。

