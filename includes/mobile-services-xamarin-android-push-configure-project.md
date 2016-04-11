
1. Solution] ビューで (または **ソリューション エクスプ ローラー** Visual Studio で) を右クリックし、 **コンポーネント** フォルダー] をクリックして  **コンポーネントの取得]**, 、検索、 **Google Cloud Messaging クライアント** コンポーネントをプロジェクトに追加します。

2. ToDoActivity.cs プロジェクト ファイルを開き、次の using ステートメントをクラスに追加します。

        using Gcm.Client;

3.  **ToDoActivity** クラスを次の新しいコードを追加します。 

        // Create a new instance field for this activity.
        static ToDoActivity instance = new ToDoActivity();

        // Return the current activity instance.
        public static ToDoActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }
        // Return the Mobile Services client.
        public MobileServiceClient CurrentClient
        {
            get
            {
                return client;
            }
        }

    これにより、プッシュ ハンドラー サービス プロセスからモバイル クライアント インスタンスにアクセスできるようになります。

4.  次のコードを追加、 **OnCreate** メソッド後に、 **MobileServiceClient** を作成します。

        // Set the current instance of TodoActivity.
        instance = this;

        // Make sure the GCM client is set up correctly.
        GcmClient.CheckDevice(this);
        GcmClient.CheckManifest(this);

        // Register the app for push notifications.
        GcmClient.Register(this, ToDoBroadcastReceiver.senderIDs);

 **ToDoActivity** がプッシュ通知を追加するための準備が整いました。
