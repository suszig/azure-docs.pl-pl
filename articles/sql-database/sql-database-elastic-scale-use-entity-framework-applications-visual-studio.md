<properties 
    pageTitle="Entity Framework でのエラスティック データベース クライアント ライブラリの使用 | Microsoft Azure" 
    description="Elastic Database クライアント ライブラリと Entity Framework をデータベースのコーディングに使用する" 
    services="sql-database" 
    documentationCenter="" 
    manager="jeffreyg" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/04/2015" 
    ms.author="torsteng;sidneyh"/>

# Entity Framework による Elastic Database クライアント ライブラリ 
 
このドキュメントと統合するために必要な Entity Framework アプリケーションの変更、 [エラスティック データベース ツール](sql-database-elastic-scale-introduction.md)します。 作成にフォーカスがある [シャード マップの管理](sql-database-elastic-scale-shard-map-management.md) と [データ依存型ルーティング](sql-database-elastic-scale-data-dependent-routing.md) Entity Framework で **Code First** アプローチです。  [新しいデータベースで Code First –](http://msdn.microsoft.com/data/jj193542.aspx) EF のチュートリアルは、このドキュメントで例としては機能します。 このドキュメントに付属するサンプル コードは、Visual Studio のコード サンプルに含まれるエラスティック データベース ツールのサンプルの一部です。
  
## サンプル コードのダウンロードと実行
この記事のコードをダウンロードするには:

* Visual Studio 2012 以降が必要です。 
* Visual Studio を起動します。 
* Visual Studio で、[ファイル] の [新しいプロジェクト] を選択します。 
* 新しいプロジェクト] ダイアログ ボックスに移動、 **オンライン サンプル** の **Visual c#** し、右上にある検索ボックスに「elastic db」を入力します。
    
    ![Entity Framework と エラスティック データベースのサンプル アプリ][1] 

    という名前のサンプルを選択して **Elastic DB Tools for Azure SQL – Entity Framework Integration**します。 ライセンスに同意すると、サンプルが読み込まれます。 

このサンプルを実行するには、Azure SQL Database で次の 3 つの空のデータベースを作成する必要があります。

* シャード マップ マネージャー データベース
* シャード 1 データベース
* シャード 2 データベース

これらのデータベースを作成すると、入力内のプレース ホルダー **Program.cs** 、Azure SQL DB サーバー名、データベース名およびデータベースへの接続に資格情報を使用します。 Visual Studio でソリューションをビルドします。 Visual Studio によるビルド プロセスの一部として、エラスティック データベース クライアント ライブラリ、Entity Framework、一時的エラーの処理のための必要な NuGet パッケージがダウンロードされます。 ソリューションで NuGet パッケージの復元が有効になっていることを確認します。 この設定は、Visual Studio ソリューション エクスプローラーでソリューション ファイルを右クリックして有効にすることができます。 

## Entity Framework のワークフロー 

Entity Framework 開発者は、アプリケーションをビルドし、アプリケーション オブジェクトの持続性を確認する場合に、次の 4 つのワークフローのいずれかに依存します。 

* **コード ファースト (新しいデータベース)**: EF 開発者は、アプリケーション コードでモデルを作成し、EF はそこからデータベースを生成します。 
* **コード ファースト (既存のデータベース)**: 開発者、既存のデータベースからモデルのアプリケーション コードを生成する EF を使用します。
* **モデル ファースト**: 開発者が EF デザイナーでモデルを作成し、EF がモデルからデータベースを作成します。
* **データベース ファースト**: 開発者が EF 既存のデータベースからモデルを推論ツールを使用します。 

これらの手法はすべて DbContext クラスに依存して、アプリケーションのデータベース接続とデータベース スキーマを透過的に管理します。 後で詳しく説明しますが、DbContext 基本クラスのさまざまなコンストラクターにより、接続の作成、データベースのブートストラップ、スキーマの作成をさまざまなレベルで制御できます。 このとき、主に、EF によって提供されるデータベース接続の管理機能が、エラスティック データベース クライアント ライブラリによって提供されるデータ依存ルーティング インターフェイスの接続の管理機能と交差するという事実から、課題が生まれます。 

## Elastic Database ツールの前提条件 

用語の定義を参照してください。 [エラスティック データベース ツールの用語集](sql-database-elastic-scale-glossary.md)します。

エラスティック データベース クライアント ライブラリでは、"シャードレット" と呼ばれる、アプリケーション データのパーティションを定義します。 シャードレットはシャーディング キーによって識別され、特定のデータベースにマップされます。 アプリケーションは、任意の数のデータベースを持つことができ、シャードレットを分散して現在のビジネス要件に対して十分な容量またはパフォーマンスを提供します。 シャーディング キー値とデータベースとの間のマッピングは、エラスティック データベース クライアント API によって提供されるシャード マップによって格納されます。 この機能 **シャード マップの管理**, 、または略して SMM します。 シャード マップは、シャーディング キーを格納する要求のデータベース接続用のためのブローカーとしても機能します。 この機能と呼ば **データ依存型ルーティング**します。 
 
シャード マップ マネージャーは、同時シャードレット管理操作 (たとえば、データのシャード間の移動) の実行中に発生する可能性があるシャードレット データの一貫性のないビューからユーザーを保護します。 これを実現するために、クライアント ライブラリによって管理されるシャード マップは、アプリケーションのデータベース接続を仲介します。 これにより、接続が作成されているシャードレットがシャード管理操作の影響を与える可能性のあるときにシャード マップ機能によってデータベース接続を自動的に強制終了することができます。 この方法は、EF の機能の一部 (たとえば、既存の接続から新しい接続を作成してデータベースの存在を確認する機能) と統合する必要があります。 一般に、標準的な DbContext コンストラクターは EF の処理用に安全に複製できる閉じたデータベース接続に対してのみ確実に動作する、というのが従来の考え方でした。 これに対し、エラスティック データベースの設計上の原則は、開かれている接続のみを仲介することにあります。 クライアント ライブラリによって仲介される接続を EF DbContext に渡す前に閉じればこの問題を解決できると考える読者もいることでしょう。 ただし、接続を閉じた後、再び開く操作を EF に依存することで、ライブラリによって実行される検証と一貫性チェックが失われてしまいます。 ただし、EF の移行機能は、これらの接続を使用して、アプリケーションに対して透過的な形で基になるデータベース スキーマを管理します。 同じアプリケーション内でこれらのエラスティック データベース クライアント ライブラリの機能と EF の機能をそのまま組み合わせることができれば理想的です。 次のセクションでは、これらのプロパティと要件について詳しく説明します。 


## 必要条件 

エラスティック データベース クライアント ライブラリと Entity Framework API の両方を使用する場合は、次のプロパティを維持することを想定します。 

* **スケール アウト**: を追加またはデータ層アプリケーションの容量要求に合わせて必要に応じてシャード化されたアプリケーションのデータベースを削除します。 これには、データベースの作成と削除を制御することに加え、エラスティック データベース シャード マップ マネージャー API を使用してデータベースを管理したりシャードレットのマッピングを管理したりすることが含まれます。 

* **整合性**: アプリケーションは、シャーディングを使用し、クライアント ライブラリのデータ依存ルーティング機能を使用します。 破損や誤ったクエリ結果を回避するために、シャード マップ マネージャーを通じて接続が仲介されます。 これにより、検証と一貫性も維持されます。
 
* **コード ファースト**: EF の code first パラダイムの利便性を保持します。 Code First では、アプリケーションのクラスは基になるデータベース構造に透過的にマップされます。 アプリケーション コードは、基になるデータベースの処理に関連するほとんどの側面をマスクする Dbset と対話します。
 
* **スキーマ**: Entity Framework は、初期のデータベース スキーマの作成とそれ以降のスキーマの展開を移行を通じてを処理します。 これらの機能を維持することで、データの進化に対して簡単にアプリを適合させることができます。 

エラスティック データベース ツールを使用してこれらの Code First アプリケーションの要件を満たす方法を次のガイダンスに示します。 

## EF DbContext を使用したデータ依存ルーティング 

Entity Framework でのデータベース接続の管理が通常のサブクラス、 **DbContext**します。 派生させて、これらのサブクラスを作成 **DbContext**します。 これは、定義した場合、 **DbSets** データベースにバックアップされる、アプリケーションの CLR オブジェクトのコレクションを実装します。 データ依存ルーティングのコンテキストでは、他の Code First アプリケーション シナリオには必ずしも該当しない、次のようないくつかの有用なプロパティを識別できます。 

* データベースが既に存在し、エラスティック データベース シャード マップに登録されている。 
* アプリケーションのスキーマが既にデータベースにデプロイされている (後で説明します)。 
* データベースへのデータ依存ルーティング接続はシャード マップによって仲介される。 

統合する **DbContexts** のスケール アウトのためのデータ依存型ルーティングを使用します。

1. シャード マップ マネージャーのエラスティック データベース クライアント インターフェイスを介して物理的なデータベース接続を作成します。 
2. 接続をラップします、 **DbContext** サブクラス
3. 接続を渡します、 **DbContext** 基本クラスも EF の側のすべての処理が実行されるようにします。 

この方法を次のコード例に示します (このコードは付属の Visual Studio プロジェクトにも含まれています)。

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data dependent routing. This call will open a validated connection 
        // routed to the proper shard by the shard map manager. 
        // Note that the base class c'tor call will fail for an open connection
        // if migrations need to be done and SQL credentials are used. This is the reason for the 
        // separation of c'tors into the data-dependent routing case (this c'tor) and the internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map to broker a validated connection for the given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## 要点
* DbContext サブクラスの既定のコンストラクターが新しいコンストラクターで置き換えられます。 
* 新しいコンストラクターは、データ依存ルーティングに必要な次の引数をエラスティック データベース クライアント ライブラリ経由で受け取ります。
    * データ依存ルーティング インターフェイスにアクセスするためのシャード マップ
    * シャードレットを識別するためのシャーディング キー
    * シャードへのデータ依存ルーティング接続に使用する資格情報を含む接続文字列 
 
* 基本クラス コンストラクターの呼び出しは、データ依存ルーティングに必要なすべての手順を実行する静的メソッドに迂回します。 
   * コンストラクターが、シャード マップ上でエラスティック データベース クライアント インターフェイスの OpenConnectionForKey 呼び出しを使用して、開いた接続を確立します。
   * シャード マップにより、特定のシャーディング キーのシャードレットを保持するシャードへの開いた接続が作成されます。
   * この開いた接続が再び DbContext の基本クラス コンストラクターに渡され、EF が新しい接続を自動的に作成する代わりにこの接続を使用することが示されます。 このようにエラスティック データベース クライアント API によって接続がタグ付けされ、シャード マップ管理操作での一貫性が保証されます。
 
  
コードの既定のコンストラクターに代えて、新しいコンストラクターを DbContext サブクラスに使用します。 たとえば次のようになります。 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

新しいコンス トラクターの値によって識別されるシャードレットのデータを保持するシャードへの接続を開く **tenantid1**します。 内のコード、 **を使用して** アクセスをブロックは変更されません、 **DbSet** 用のシャードで EF を使用してブログの **tenantid1**します。 1 つのシャードにすべてのデータベース操作のスコープが設定は今すぐに、このブロックを使用して、コードのセマンティクスが変更、 **tenantid1** を保持します。 ブログ経由での LINQ クエリでは、 **DbSet** 、現在のシャードに格納されているブログは他のシャードに格納されていないを返すだけです。  

#### 一時的エラーの処理
Microsoft Patterns & Practices チームで公開して、 [The Transient Fault Handling Application Block](https://msdn.microsoft.com/library/dn440719.aspx)します。 このライブラリは、EF と組み合わせて使用するエラスティック スケール クライアント ライブラリで使用します。 ただし、ここで調整したコンストラクターを使用してすべての新しい接続試行が行われるようにするには、一時的エラーの後に新しいコンストラクターが使用されることが保証される位置にすべての一時的な例外から制御が返されるようにします。 そうでないと、正しいシャードへの接続が保証されず、シャード マップに対する変更が発生したときに接続が保持される保証がなくなります。 

次のコード サンプルは、SQL の再試行ポリシーを使用して、新しい周囲方法を示しています。 **DbContext** サブクラスのコンス トラクター。 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

**SqlDatabaseUtils.SqlRetryPolicy** 上記のコードでは、として定義されている、 **SqlDatabaseTransientErrorDetectionStrategy** 10、および 5 秒間の再試行回数の再試行までの時間まで待機します。 この方法は、EF とユーザーによって開始されたトランザクション向けのガイダンスに似ています (を参照してください [制限 (EF6 以降) の実行戦略の再試行と](http://msdn.microsoft.com/data/dn307226)です。 トランザクションを開き直す場合、または (示されているように) エラスティック データベース クライアント ライブラリを使用する適切なコンストラクターからコンテキストを再作成する場合のどちらの状況でも、一時的な例外から制御が返されるスコープをアプリケーション プログラムが制御する必要があります。

ここで一時的な例外に進めないスコープを制御する必要性は、組み込みの使用もできなく **SqlAzureExecutionStrategy** EF に付属します。 **SqlAzureExecutionStrategy** は接続を再度開きますが、使用しない **OpenConnectionForKey** したがっての一部として実行されるすべての検証をバイパスし、 **OpenConnectionForKey** 呼び出します。 代わりに、コード サンプルでは、組み込み **DefaultExecutionStrategy** EF にも付属します。 代わりに **SqlAzureExecutionStrategy**, 、一時的な障害処理の再試行ポリシーと組み合わせて正常に動作します。 実行ポリシーを設定、 **ElasticScaleDbConfiguration** クラスです。 使用しないことにしました注 **DefaultSqlExecutionStrategy** を使用することを提案するため **SqlAzureExecutionStrategy** が説明したような動作しないと一時的な例外が発生した場合。 さまざまな再試行ポリシーと EF の詳細については、次を参照してください。 [EF の接続レジリエンシー](http://msdn.microsoft.com/data/dn456835.aspx)します。     

#### コンストラクターの書き直し
上記のコード例では、既定のコンス トラクターを書き直すデータ依存ルーティングを Entity Framework で使用するために、アプリケーションに必要なを示しています。 次の表では、この方法を他のコンストラクター向けに一般化しています。 


現在のコンストラクター  | データの書き換えのコンストラクター | 基本コンストラクター | メモ
---------- | ----------- | ------------|----------
MyContext() |ElasticScaleContext(ShardMap、TKey) |DbContext(DbConnection、bool) |接続は、シャード マップとデータに依存するルーティングのキーの関数である必要があります。 EF による自動的な接続の作成を回避し、代わりにシャード マップを使用して接続を仲介する必要があります。 
MyContext(string)|ElasticScaleContext(ShardMap、TKey) |DbContext(DbConnection、bool) |接続は、シャード マップとデータに依存するルーティングのキーの関数である必要があります。 シャード マップによる検証が回避されるため、固定されたデータベース名または接続文字列は機能しません。 
MyContext(DbCompiledModel) |ElasticScaleContext(ShardMap、TKey、DbCompiledModel) |DbContext(DbConnection、DbCompiledModel、bool) |提供されたモデルで特定のシャード マップとシャーディング キーに接続が作成されます。 コンパイルされたモデルは、基本コンストラクターに渡されます。
MyContext(DbConnection、bool) |ElasticScaleContext(ShardMap、TKey, bool) |DbContext(DbConnection、bool) |接続は、シャード マップとキーから推論する必要があります。 接続を入力として渡すことはできません (入力で既にシャード マップとキーが使用されていた場合を除きます)。 ブール値が渡されます。 
MyContext(string、DbCompiledModel) |ElasticScaleContext(ShardMap、TKey、DbCompiledModel) |DbContext(DbConnection、DbCompiledModel、bool) |接続は、シャード マップとキーから推論する必要があります。 接続を入力として渡すことはできません (入力でシャード マップとキーが使用されていた場合を除きます)。 コンパイルされたモデルが渡されます。 
MyContext(ObjectContext、bool) |ElasticScaleContext(ShardMap、TKey、ObjectContext、bool) |DbContext(ObjectContext、bool) |新しいコンス トラクターでは、入力として渡される ObjectContext が Elastic Scale が管理する接続に再ルーティングされるようにする必要があります。 ObjectContexts の詳しい説明は、このドキュメントの範囲を超えているため省略します。
MyContext(DbConnection、DbCompiledModel、bool) |ElasticScaleContext(ShardMap、TKey、DbCompiledModel、bool)| DbContext(DbConnection、DbCompiledModel、bool); |接続は、シャード マップとキーから推論する必要があります。 接続を入力として渡すことはできません (入力で既にシャード マップとキーが使用されていた場合を除きます)。 モデルとブール値が基本クラス コンストラクターに渡されます。 

## EF の移行を通じたシャード スキーマのデプロイ 

自動スキーマ管理は、Entity Framework によって提供される便利な機能です。 エラスティック データベース ツールを使用したアプリケーションのコンテキストでは、この機能を保持して、データベースがシャード化されたアプリケーションに追加されたときにスキーマを新しく作成されたシャードに自動的にプロビジョニングできるようにします。 基本的なユース ケースは、EF を使用するシャード化されたアプリケーションのためにデータ層の容量を増やすことです。 EF のスキーマ管理機能に依存することで、EF 上に構築されたシャード化されたアプリケーションでのデータベースの管理に伴う労力を減らすことができます。 

EF の移行を通じたスキーマの配置に最適な **接続の開封**します。 これは、エラスティック データベース クライアント API によって提供される、開かれた接続に依存するデータ依存ルーティングのシナリオとは対照的です。 もう 1 つの違いは、一貫性の要件にあります。シャード マップの同時操作から保護するためにすべてのデータ依存ルーティング接続の一貫性を保証するのは望ましいことですが、まだシャード マップに登録されておらず、シャードレットを保持するために割り当てられていない新しいデータベースへの初期スキーマ デプロイメントでは、これは問題になりません。 そこで、このシナリオでは、データ依存ルーティングではなく、通常のデータベース接続に依存することができます。  

これは、EF の移行を通じたスキーマのデプロイメントと、アプリケーションのシャード マップにおけるシャードとしての新しいデータベースの登録とが密接に結び付いたアプローチになります。 これは、次の前提条件に依存します。 

* データベースが既に作成されている。 
* データベースが空で、ユーザー スキーマやユーザー データが含まれない。
* データ依存ルーティングに使用される エラスティック データベース クライアント API を介してデータベースにまだアクセスできない。 

これらの前提条件を通常作成して開かれていない **SqlConnection** スキーマ デプロイメントのための EF の移行を開始します。 次のコード サンプルに、この方法を示します。 

        // Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
        // and kick off EF intialization of the database to deploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext to trigger migrations and schema deployment for the new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query to engage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register the mapping of the tenant to the shard in the shard map. 
            // After this step, data-dependent routing on the shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 
 

このサンプルは、メソッドを示しています。 **RegisterNewShard** をシャード マップ内の、シャードの登録、配置、EF の移行を通じてスキーマをし、シャードにシャーディング キーのマッピングを格納します。 コンス トラクターで利用すること、 **DbContext** サブクラス (**ElasticScaleContext** サンプルで) 入力としての SQL 接続文字列を取得します。 次の例に示すように、このコンストラクターのコードは簡単です。 


        // C'tor to deploy schema and migrations to a new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // We want existence checks so that the schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 
 
基本クラスから派生したコンストラクターのバージョンを使用したことのある読者もいることでしょう。 ただし、このコードでは、接続するときに EF の既定の初期化子が使用されるようにする必要があります。 そこで、接続文字列を使用して基本クラスのコンストラクターを呼び出す前に、静的メソッドに迂回します。 EF の初期化子の設定が競合しないように、シャードの登録を異なるアプリ ドメインまたはプロセスで実行する必要があることに注目してください。 


## 制限事項 

このドキュメントで説明した方法には、いくつかの制限事項があります。 

* 使用する EF アプリケーション **LocalDb** まず elastic database クライアント ライブラリを使用する前に、通常の SQL Server データベースに移行する必要があります。 Elastic Scale によるシャーディングを使用してアプリケーションをスケール変更ではありえません **LocalDb**します。 開発をまだ使用 **LocalDb**します。 

* データベース スキーマの変更を暗示するすべてのアプリケーションに対する変更は、すべてのシャードで EF の移行を介する必要があります。 このドキュメントのサンプル コードには、この方法を示していません。 Update-Database を ConnectionString パラメーターと共に使用してすべてのシャードに対して反復処理を行うことを検討してください。または、Update-Database を -Script オプションと共に使用して保留中の移行の T-SQL スクリプトを抽出し、その T-SQL スクリプトをシャードに適用します。  

* 要求を受け取った場合、要求によって提供されたシャーディング キーで識別される 1 つのシャード内にそのすべてのデータベース処理が含まれると見なされます。 ただし、この仮定が常に正しいとは限りません。 たとえば、シャーディング キーを使用可能にできないことがあります。 クライアント ライブラリは、これに対処する、 **MultiShardQuery** 複数のシャードを照会するための接続の抽象化を実装するクラス。 使用方法の学習、 **MultiShardQuery** EF と組み合わせたでがこのドキュメントの範囲外

## まとめ 

Entity Framework アプリケーションは、Azure SQL Database の Elastic Database ツールのメリットを簡単に活用できます。 このドキュメントで説明した手順で、EF アプリケーションは、データ依存ルーティングのコンス トラクターをリファクタリングをエラスティック データベース クライアント ライブラリの機能を使用できます、 **DbContext** サブクラスは、EF アプリケーションで使用します。 これだけに必要な変更を制限、 **DbContext** クラスが既に存在します。 さらに、EF アプリケーションは、新しいシャードと、必要な EF の移行を伴う手順と、新しいシャードとマッピングのシャード マップへの登録とを組み合わせることによって、自動スキーマ デプロイメントのメリットを引き続き得ることができます。 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
 

