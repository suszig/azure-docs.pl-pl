
このセクションでは、SQL Server Express のインストール、TCP/IP の有効化、静的なポートの設定、およびハイブリッド接続で使用可能なデータベースを作成する方法を説明します。  

###SQL Server Express をインストールする

ハイブリッド接続でオンプレミスの SQL Server または SQL Server Express のデータベースを使用するには、TCP/IP が静的ポートで有効になっている必要があります。 SQL Server の既定のインスタンスは静的ポート 1433 を使用しますが、名前付きインスタンスは静的ポート 1433 を使用しません。 このため、既定のインスタンスをインストールします。 SQL Server Express の既定のインスタンスが既にインストールされている場合は、このセクションを省略できます。

1. SQL Server Express をインストールするには、実行、 **SQLEXPRWT_x64_ENU.exe** または **SQLEXPR_x86_ENU.exe** ダウンロードしたファイルです。 SQL Server インストール センター ウィザードが表示されます。
    
2. 選択 **SQL Server の新規スタンドアロン インストールか、既存のインストールに機能を追加**, 、次の手順については、表示されるまで、既定の選択と設定を受け入れ、 **インスタンスの構成** ページです。
    
3.  **インスタンスの構成** ページで、選択 **既定のインスタンス**, で、既定の設定を受け入れる、 **サーバー構成** ページです。

    >[AZURE.NOTE]既にインストールされている SQL Server の既定のインスタンスがある場合は、次のセクションをスキップし、ハイブリッド接続がこのインスタンスを使用します。 
    
5.  **データベース エンジンの構成** ] ページで、[ **認証モード**, を選択 **混合モード (SQL Server 認証と Windows 認証)**, 、組み込みのセキュリティで保護されたパスワードを指定して **sa** 管理者アカウントです。
    
    このチュートリアルでは、SQL Server 認証を使用します。 後で必要になるため、入力したパスワードを忘れないでください。
    
6. ウィザードを終了して、インストールを完了します。

###TCP/IP を有効にして、静的ポートを設定する

このセクションでは、SQL Server Express をインストールした際にインストールされた SQL Server 構成マネージャーを使用して、TCP/IP を有効にし、静的ポートを設定します。 

1. 手順に従います [for SQL Server の TCP/IP ネットワーク プロトコルの有効化](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) インスタンスに TCP/IP がアクセスを有効にします。

2. (省略可能)既定のインスタンスを使用できない場合の手順に従えば必要があります [特定の TCP ポートでリッスンするようにサーバーを構成する ](https://msdn.microsoft.com/library/ms177440.aspx) インスタンスの静的ポートを設定します。 この手順を完了すると、ポート 1433 ではなく、定義する新しいポートを使用して接続します。

3. (省略可能) 必要な場合は、SQL Server のプロセス (sqlservr.exe) へのリモート アクセスを許可するために、ファイアウォールに例外を追加します。

###オンプレミスの SQL Server インスタンスに新しいデータベースを作成する

1. SQL Server Management Studio で、インストールした SQL Server に接続します (場合、 **サーバーへの接続** に移動し、ダイアログが自動的に表示されない **オブジェクト エクスプ ローラー** 左のウィンドウで **接続**, 、] をクリックし、 **データベース エンジン**.)     

    ![サーバーへの接続](./media/hybrid-connections-create-on-premises-database/A04SSMSConnectToServer.png)
    
     **サーバーの種類**, 、選択 **データベース エンジン**します。  **サーバー名**, 、使用する **localhost** または SQL Server がインストールされているコンピューターの名前。 選択 **SQL Server 認証**, 、し、先ほど作成した sa ログインのパスワードを指定します。 
    
2. SQL Server Management Studio を使用して、新しいデータベースを作成するには、右 **データベース** オブジェクト エクスプ ローラーで、をクリックしてに **新しいデータベース**します。
    
3.  **新しいデータベース** ダイアログで、「 `OnPremisesDB`, 、] をクリックし、 **OK**します。 
    
4. 展開する場合は、オブジェクト エクスプ ローラーで **データベース**, 、新しいデータベースが作成されたことが表示されます。

###新しい SQL Server ログインを作成し、アクセス許可を設定する

最後に、アクセス許可を制限した新しい SQL Server ログインを作成します。 Azure サービスは、サーバーに対する完全なアクセス許可を持つ組み込みの sa ログインではなく、このログインを使用してオンプレミスの SQL Server に接続します。

1. SQL Server Management Studio オブジェクト エクスプ ローラーで、右クリックし、 **OnPremisesDB** データベースを **新しいクエリ**します。

2.  次の TSQL クエリを、クエリ ウィンドウに貼り付けます。

        USE [master]
        GO
        
        /* Replace the PASSWORD in the following statement with a secure password. 
           If you save this script, make sure that you secure the file to 
           securely maintain the password. */ 
        CREATE LOGIN [HybridConnectionLogin] WITH PASSWORD=N'<**secure_password**>', 
            DEFAULT_DATABASE=[OnPremisesDB], DEFAULT_LANGUAGE=[us_english], 
            CHECK_EXPIRATION=OFF, CHECK_POLICY=ON
        GO
    
        USE [OnPremisesDB]
        GO
    
        CREATE USER [HybridConnectionLogin] FOR LOGIN [HybridConnectionLogin] 
        WITH DEFAULT_SCHEMA=[dbo]
        GO

        GRANT CONNECT TO [HybridConnectionLogin]
        GRANT CREATE TABLE TO [HybridConnectionLogin]
        GRANT CREATE SCHEMA TO [HybridConnectionLogin]
        GO  
   
3. 上記のスクリプトで、文字列置換 `<**secure_password**>` の新しいセキュリティで保護されたパスワードを使用して *HybridConnectionsLogin*します。

4. **実行** クエリは、新しいログインを作成し、内部設置型データベースに必要なアクセス許可を付与します。


