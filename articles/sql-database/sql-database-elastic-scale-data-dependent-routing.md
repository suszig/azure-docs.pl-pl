<properties 
    pageTitle="データ依存ルーティング | Microsoft Azure" 
    description="データ依存ルーティング (Azure SQL Database のエラスティック データベースの機能) に ShardMapManager を使用する方法" 
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

#データ依存ルーティング

 **ShardMapManager** クラスは、ADO.NET アプリケーション、シャード化環境にデータベース クエリおよびコマンドを適切な物理データベースに直接する機能を提供します。 これと呼ばれる **データ依存型ルーティング** シャード化されたデータベースを使用する場合は、基本的なパターンとします。 データ依存ルーティングを使用したアプリケーションで、特定のクエリまたはトランザクションがそれぞれアクセスするデータベースは、要求ごとに 1 つに制限されています。  

データ依存ルーティングを使用する場合、アプリケーションは、シャード化された環境の各種データ スライスに関連付けられた、さまざまな接続文字列または DB の場所を追跡する必要はありません。 代わりに、 [シャード マップ マネージャー](sql-database-elastic-scale-shard-map-management.md) 、シャード マップとは、アプリケーションの要求のターゲットであるシャーディング キーの値のデータに基づいて、責任を負い、必要なときに、正しいデータベースへの接続を提供します。 (このキーは、通常、 *customer_id*, 、*tenant_id*, 、*date_key*, 、または、データベース要求の基本的なパラメーターであるその他の特定の識別子)。 

## クライアント ライブラリのダウンロード

ライブラリをインストールするには [Elastic Database クライアント ライブラリ](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)します。 

## データ依存ルーティング アプリケーションでの ShardMapManager の使用 

データ依存ルーティングを使用するアプリケーション、 **ShardMapManager** でアプリケーション ドメインごとに 1 回、ファクトリ呼び出しを使用して、初期化中にインスタンス化する必要があります **GetSQLShardMapManager**します。

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

この例では両方の **ShardMapManager** と特定 **ShardMap** それに含まれる初期化されます。 

シャードを操作しないアプリケーションのマップを直接取得するファクトリ メソッドで使用する資格情報、 **ShardMapManager** (上記の例では、 *smmConnectionString*) に単に読み取り専用アクセス許可のある資格情報をする必要があります、 **グローバル シャード マップ** 、接続文字列が参照されているデータベースです。 これらの資格情報は、通常、シャード マップ マネージャーへの接続で使用される資格情報とは異なります。 関連項目 [、エラスティック データベース クライアント ライブラリでの資格情報を使用して](sql-database-elastic-scale-manage-credentials.md)します。 

## データ依存型ルーティングを呼び出す 

メソッド **ShardMap.OpenConnectionForKey (key、connectionString、connectionOptions)** の値に基づいて適切なデータベースへのコマンド発行の準備ができて、ADO.Net 接続を返す、 **キー** パラメーター。 シャード情報は、アプリケーションにキャッシュ、 **ShardMapManager**, ので、これらの要求は通常、データベースの参照に対して含まれません、 **グローバル シャード マップ** データベースです。 

*  **キー** 要求の適切なデータベースを特定するシャード マップへ、パラメーターがルックアップ キーとして使用されます。 

*  **ConnectionString** 、必要な接続のユーザー資格情報のみを渡すために使用します。 データベース名またはサーバー名これに含まれる *connectionString* メソッドは、データベースとサーバーを使用して決定されますので、 **ShardMap**します。 

*  **ConnectionOptions** 列挙型を使用して、検証が発生したかどうか、または接続時ではなくを指定します。 **ConnectionOptions.Validate** をお勧めします。 分割やマージの操作の結果、シャード マップが変更している可能性や、行が他のデータベースに移動している可能性のある環境では、キー値に基づいて行われるデータベースのルックアップ (キャッシュ済み) が引き続き正しいことが検証によって確認されます。 接続要求がアプリケーションに配信される前に、検証により、ターゲット データベースにあるローカルのシャード マップ (グローバル シャード マップではない) に対する簡単なクエリが実行されます。 

ローカルのシャード マップの検証が失敗した (キャッシュが正しくないことが示された) 場合は、シャード マップ マネージャーがグローバル シャード マップに対してクエリを実行し、ルックアップの正しい値を新しく取得します。その後、キャッシュを更新、取得してから適切なデータベース接続に戻ります。 

のみ **ConnectionOptions.None** (検証しない) ことは、アプリケーションがオンラインの間に、シャード マッピングの変更が想定しないときに発生します。 その場合、キャッシュされた値は常に正しいと見なすことができ、ターゲット データベースに対する今後のラウンドトリップ検証呼び出しを安全にスキップすることができます。 これによってトランザクションの待機時間やデータベース トラフィックを軽減することができます。  **ConnectionOptions** シャーディングの変更が予期されるかどうかを示す構成ファイルまたは時間の期間中以外の値で設定することがありますもします。  

これは、シャード マップ マネージャーを使用して、データ依存ルーティングの整数型のキーの値に基づいてを実行するコードの例 **CustomerID**, を使用して、 **ShardMap** という名前のオブジェクト **customerShardMap**します。  

## 例: データ依存型ルーティング 

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connection to the shard for that customer ID
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, 
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
    { 
        // Execute a simple command 
        SqlCommand cmd = conn.CreateCommand(); 
        cmd.CommandText = @"UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID"; 

        cmd.Parameters.AddWithValue("@customerID", customerId); 
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId); 
        cmd.ExecuteNonQuery(); 
    }  

コンス トラクターを使用するのではなく、ことに注意して、 **SqlConnection**, と、その後、 **Open()** 接続オブジェクトを呼び出し、 **OpenConnectionForKey** メソッドを使用して、適切なデータベースに新しい既に開いている接続を配信します。 この方法で接続した場合も引き続き ADO.Net 接続プールの利点を最大限に活用できます。 トランザクションと要求を満たすことができるのが 1 回に 1 つのシャードである限り、ADO.Net を既に使用しているアプリケーションで必要な変更はこれだけです。 

メソッド **OpenConnectionForKeyAsync** もアプリケーションが ADO.Net を使用した非同期プログラミングを使用する場合に使用します。  その動作は、ADO データ依存ルーティングに相当します。Net の **Connection.OpenAsync** メソッドです。

## 一時的な障害処理との統合 

クラウドでのデータ アクセス アプリケーション開発のベスト プラクティスは、データベースへの接続またはクエリでの一時的な障害をアプリケーションで確実に捕捉し、エラーがスローされる前に操作を確実にリトライすることです。 一時的な障害クラウド アプリケーションの処理の詳細については [一時的な障害処理](http://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx)します。 
 
一時的な障害処理は、データ依存ルーティングのパターンと自然に共存できます。 主な要件を含めたすべてのデータ アクセス要求を再試行するには、 **を使用して** ブロック、データ依存ルーティング接続を取得します。 上記の例は、次のように書き換えることができます (変更箇所が強調表示されています)。 

### 例 – 一時的な障害処理機能を伴うデータ依存ルーティング 

<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() = & gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connection to the shard for that customer ID 
        using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId,  
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
        { 
            // Execute a simple command 
            SqlCommand cmd = conn.CreateCommand(); 

            cmd.CommandText = @&quot;UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID&quot;; 

            cmd.Parameters.AddWithValue(&quot;@customerID&quot;, customerId); 
            cmd.Parameters.AddWithValue(&quot;@newPersonID&quot;, newPersonId); 
            cmd.ExecuteNonQuery(); 

            Console.WriteLine(&quot;Update completed&quot;); 
        } 
<span style="background-color:  #FFFF00">    }); </span> 
</code></pre>


一時的な障害処理の実装に必要なパッケージは、エラスティック データベースのサンプル アプリケーションのビルド時に自動的にダウンロードされます。 パッケージからも入手可能とは別に [Enterprise Library - Transient Fault Handling Application Block](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)します。 バージョン 6.0 以降を使用してください。 

## トランザクションの整合性 

トランザクションのプロパティは、シャードにとってローカルなすべての操作で保証されています。 たとえば、データ依存ルーティングを通じて送信されるトランザクションは、接続するターゲット シャードの範囲内で実行されます。 現時点では、複数の接続を 1 つのトランザクションに登録するために提供された機能はありません。したがって、複数のシャードにまたがる操作にもトランザクション上の保証はありません。  

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 

