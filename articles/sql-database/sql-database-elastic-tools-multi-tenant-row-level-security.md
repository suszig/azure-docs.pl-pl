<properties 
    pageTitle="弾力性データベース ツールと行レベルのセキュリティを使用したマルチテナント アプリケーション" 
    description="弾力性データベース ツールと行レベルのセキュリティを使用して、Azure SQL Database にマルチテナントのシャードをサポートする拡張性の高いデータ層を持つアプリケーションを作成する方法について説明します。" 
    metaKeywords="azure sql database elastic tools multi tenant row level security rls" 
    services="sql-database" documentationCenter=""  
    manager="jeffreyg" 
    authors="tmullaney"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/03/2015" 
    ms.author="thmullan;torsteng;sidneyh" />

# 弾力性データベース ツールと行レベルのセキュリティを使用したマルチテナント アプリケーション 

[エラスティック データベース ツール](sql-database-elastic-scale-get-started.md) と [行レベル セキュリティ (RLS)](https://msdn.microsoft.com/library/dn765131) 強力な Azure SQL データベースのマルチ テナント アプリケーションのデータ層を柔軟かつ効率的に拡張するための機能セットを提供します。 この記事は、同時に使用するこれらのテクノロジを使用してマルチ テナント シャードをサポートする拡張性の高いデータ層を持つアプリケーションを構築する方法を示しています。 **ADO.NET SqlClient** や **Entity Framework**します。 

* **エラスティック データベース ツール** .NET ライブラリと Azure のサービス テンプレートのセットを使用して、業界標準のシャーディング手法に従ってアプリケーションのデータ層をスケール アウトすることができます。 弾力性データベース クライアント ライブラリを使用してシャードを管理することは、一般的にシャーディングに関連するインフラストラクチャ タスクの多くを自動化および効率化するうえで役立ちます。 

* **行レベルのセキュリティ** クエリを実行しているテナントに属していない行を除外するセキュリティ ポリシーを使用して、同じデータベース内の複数のテナントのデータを格納することができます。 アプリケーションではなくデータベース内に RLS を使ってアクセス ロジックを一元化することにより、アプリケーションのコードベースが大きくなった際にも保守が容易になり、エラーのリスクが小さくなります。 RLS は、最新バージョンを必要と [Azure SQL Database 更新プログラム (V12)](sql-database-preview-whats-new.md)します。 

これらの機能を使いながら、複数のテナントのデータを同じデータベースに格納することにより、コストの削減と効率性の向上というメリットがアプリケーションにもたらされます。 マルチテナントのシャードではテナント間でリソースが等しく分配されることが保証されないため、より厳密なパフォーマンスの保証を必要とする "プレミアム" テナント向けに、分離された単一テナントのシャードを柔軟に提供することもできます。  

要するに、エラスティック データベース クライアント ライブラリの [データ依存ルーティング](sql-database-elastic-scale-data-dependent-routing.md) Api が、シャーディング キー (一般に、"TenantId") を含む正しいシャード データベースにテナントを自動的に接続します。 接続された後、テナントは、データベース内の RLS セキュリティ ポリシーに基づき、その TenantId が含まれる行にのみアクセスできます。 どの行がどのテナントに属しているかを示すための TenantId 列はすべてのテーブルに含まれていると見なされます。 

![アプリのアーキテクチャのブログ][1]

## サンプル プロジェクトのダウンロード

### 前提条件
* Visual Studio (2012 以降) を使用します。 
* 3 つの Azure SQL データベースを作成します。 
* サンプル プロジェクトをダウンロード: [Elastic DB Tools for Azure SQL - Multi-tenant Shards](http://go.microsoft.com/?linkid=9888163)
  * 先頭に、データベースの情報を入力 **Program.cs** 

このプロジェクトの拡張で説明した [Elastic DB Tools for Azure SQL - Entity Framework Integration](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) のマルチ テナント シャード データベースのサポートを追加します。 上の図に示すように、4 つのテナントと 2 つのマルチテナント シャード データベースを含む、ブログや投稿を作成するための簡単なコンソール アプリケーションを構築します。 

アプリケーションをビルドし、実行します。 弾力性データベース ツールのシャード マップ マネージャーが起動され、次のテストが実行されます。 

1. Entity Framework と LINQ を使用して、新しいブログを作成し、各テナントのすべてのブログを表示します。
2. ADO.NET SqlClient を使用して、テナントのすべてのブログを表示します。
3. 不適切なテナントへのブログの挿入を試みて、エラーがスローされることを確認します。  

RLS はシャード データベースでまだ有効になっていないため、これらの各テストで問題点が明らかになります。テナントは、そのテナントに属さないブログを表示できます。また、アプリケーションは、不適切なテナントにブログを挿入できます。 この記事の残りの部分では、RLS によるテナントの分離を適用してこれらの問題を解決する方法について説明します。 次の 2 つの手順が含まれます。 

1. **アプリケーション層**: 常に、接続を開いた後、SESSION_CONTEXT で現在の TenantId を設定するアプリケーション コードを変更します。 サンプル プロジェクトでは、この手順は既に完了しています。 
2. **データ層**: SESSION_CONTEXT に格納されている TenantId に基づいて行のフィルター選択するには、各シャード データベースで、RLS セキュリティ ポリシーを作成します。 この手順は、それぞれのシャード データベースに対して実行する必要があります。そうでないと、マルチテナント シャード内の行がフィルター選択されません。 


## ステップ 1) アプリケーション層: TenantId を SESSION_CONTEXT に設定する

弾力性データベース クライアント ライブラリのデータ依存型ルーティング API を使用してシャード データベースに接続した後、アプリケーションは、RLS セキュリティ ポリシーを使って他のテナントに属している行を除外できるように、その接続を使用している TenantId をデータベースに通知する必要があります。 この情報を渡すことをお勧めはその接続での現在の TenantId を格納する、 [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx)します。 (注: 別の方法として使用することが [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), 、SESSION_CONTEXT はより適切なオプションが使用するには簡単だから既定では、NULL が返されますキーと値のペアをサポートしています)。

### Entity Framework

Entity Framework を使用するアプリケーションでは、最も簡単な方法を設定で説明されている ElasticScaleContext オーバーライド内で SESSION_CONTEXT [データ依存ルーティングに使用する EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md/#data-dependent-routing-using-ef-dbcontext)します。 データ依存型ルーティングで仲介された接続を返す前に、SESSION_CONTEXT の "TenantId" をその接続に指定されている shardingKey に設定する SqlCommand を作成して実行します。 この方法では、SESSION_CONTEXT を設定するコードを 1 回記述するだけで済みます。 

```
// ElasticScaleContext.cs 
// ... 
// C'tor for data dependent routing. This call will open a validated connection routed to the proper 
// shard by the shard map manager. Note that the base class c'tor call will fail for an open connection 
// if migrations need to be done and SQL credentials are used. This is the reason for the  
// separation of c'tors into the DDR case (this c'tor) and the internal c'tor for new shards. 
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(OpenDDRConnection(shardMap, shardingKey, connectionStr), true /* contextOwnsConnection */)
{
}

public static SqlConnection OpenDDRConnection(ShardMap shardMap, T shardingKey, string connectionStr)
{
    // No initialization
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map to broker a validated connection for the given key
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(shardingKey, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", shardingKey);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
} 
// ... 
```

これで、ElasticScaleContext が呼び出されるたびに、SESSION_CONTEXT が指定された TenantId に自動的に設定されます。 

```
// Program.cs 
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))   
    {     
        var query = from b in db.Blogs
                    orderby b.Name
                    select b;
        
        Console.WriteLine("All blogs for TenantId {0}:", tenantId);     
        foreach (var item in query)     
        {       
            Console.WriteLine(item.Name);     
        }   
    } 
}); 
```

### ADO.NET SqlClient 

ADO.NET SqlClient を使用したアプリケーションの場合、推奨される方法は、接続を返す前に SESSION_CONTEXT の "TenantId" を正しい TenantId に自動的に設定する ShardMap.OpenConnectionForKey() を囲むラッパー関数を作成する方法です。 SESSION_CONTEXT が常に設定されることを確認するには、このラッパー関数を使用して接続を開くだけです。

```
// Program.cs
// ...

// Wrapper function for ShardMap.OpenConnectionForKey() that automatically sets SESSION_CONTEXT with the correct
// tenantId before returning a connection. As a best practice, you should only open connections using this 
// method to ensure that SESSION_CONTEXT is always set before executing a query.
public static SqlConnection OpenConnectionForTenant(ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map to broker a validated connection for the given key
        conn = shardMap.OpenConnectionForKey(tenantId, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT to shardingKey to enable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", tenantId);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
}

// ...

// Example query via ADO.NET SqlClient
// If row-level security is enabled, only Tenant 4's blogs will be listed
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (SqlConnection conn = OpenConnectionForTenant(sharding.ShardMap, tenantId4, connStrBldr.ConnectionString))
    {
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SELECT * FROM Blogs";

        Console.WriteLine("--\nAll blogs for TenantId {0} (using ADO.NET SqlClient):", tenantId4);
        SqlDataReader reader = cmd.ExecuteReader();
        while (reader.Read())
        {
            Console.WriteLine("{0}", reader["Name"]);
        }
    }
});

```

## ステップ 2) データ層: 行レベルのセキュリティ ポリシーを作成する

### 各テナントがアクセスできる行をフィルター選択するセキュリティ ポリシーを作成する

アプリケーションでクエリを実行する前に SESSION_CONTEXT が現在の TenantId に設定されるようになったため、RLS セキュリティ ポリシーを使ってクエリをフィルター選択して、別の TenantId を持つ行を除外できます。  

RLS は T-SQL で実装されています。ユーザー定義の関数を使用してアクセス ロジックを定義し、セキュリティ ポリシーを使ってこの関数を任意の数のテーブルにバインドします。 このプロジェクトでは、関数を使用して、(他の SQL ユーザーではなく) アプリケーションがデータベースに接続されていること、および SESSION_CONTEXT に格納されている "TenantId" が特定の行の TenantId と一致することを確認しているのみです。 フィルター述語は、SELECT、UPDATE、および DELETE クエリに対するフィルターを通過するこれらの条件を満たす行を許可します。また、ブロック述語は、これらの条件に違反する行を INSERT または UPDATE から除外します。 SESSION_CONTEXT を設定していない場合は、NULL が返され、行を表示または挿入することはできません。 

RLS を有効にするには、Visual Studio (SSDT)、SSMS、またはプロジェクトに含まれる PowerShell スクリプトを使用してすべてのシャードで次の T-SQL を実行 (を使用している場合、または [弾力性データベース ジョブ](sql-database-elastic-jobs-overview.md), 、それを使用して、すべてのシャードでこの T-SQL の実行を自動化することができます)。 

```
CREATE SCHEMA rls -- separate schema to organize RLS objects 
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)     
    RETURNS TABLE     
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult          
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- the user in your application’s connection string (dbo is only for demo purposes!)         
        AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
GO

CREATE SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts
GO 
```

> [AZURE.TIP] テーブルの数百台でも、述語を追加する必要があるプロジェクトのより複雑な場合は、スキーマ内のすべてのテーブルに述語を追加するセキュリティ ポリシーを自動的に生成するヘルパー ストアド プロシージャを使用できます。 参照してください [ヘルパー スクリプト (ブログ) – すべてのテーブルに行レベル セキュリティを適用](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script)します。  

ここでサンプル アプリケーションを再び実行すると、そのテナントに属している行だけが表示されます。 さらに、アプリケーションは、シャード データベースに現在接続されているテナント以外のテナントに属する行を挿入できず、TenantId が異なる表示可能な行を更新できません。 アプリケーションがどちらかの操作を実行しようとすると、DbUpdateException が生成されます。

後で新しいテーブルを追加する場合は、セキュリティ ポリシーを変更して、新しいテーブルにフィルター述語とブロック述語を追加します。 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable
GO 
```

### 挿入用に TenantId を自動的に設定する既定の制約を追加する 

各テーブルに既定の制約を設定し、行を挿入するときに SESSION_CONTEXT に現在格納されている値を持つ TenantId を自動的に設定できます。 次に例を示します。 

```
-- Create default constraints to auto-populate TenantId with the value of SESSION_CONTEXT for inserts 
ALTER TABLE Blogs     
    ADD CONSTRAINT df_TenantId_Blogs      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO

ALTER TABLE Posts     
    ADD CONSTRAINT df_TenantId_Posts      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO 
```

これで、アプリケーションが行を挿入するときに TenantId を指定する必要はありません。 

```
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        var blog = new Blog { Name = name }; // default constraint sets TenantId automatically     
        db.Blogs.Add(blog);     
        db.SaveChanges();   
    } 
}); 
```

> [AZURE.NOTE] Entity Framework プロジェクトの既定の制約を使用する場合は、EF データ モデルに TenantId 列を含めないことをお勧めします。 これは、Entity Framework のクエリでは既定値が自動的に指定され、T-SQL で作成された SESSION_CONTEXT を使用する既定の制約が上書きされるためです。 たとえば、サンプル プロジェクトで既定の制約を使用するには、DataClasses.cs から TenantId を削除し (さらにパッケージ マネージャー コンソールで Add-Migration を実行し)、T-SQL を使ってフィールドがデータベース テーブルにのみ存在することを確認します。 これにより、データを挿入するときに Entity Framework によって不適切な既定値が自動的に指定されなくなります。 

### (省略可能) すべての行にアクセスするには "superuser" を有効にします。
一部のアプリケーションは、すべてのシャード上のすべてのテナント間でレポート作成を有効にするために、またはテナントの行をデータベース間で移行する分割 / マージ操作をシャードで実行するために、すべての行にアクセスできる "superuser" を作成できます。 これを可能にするには、新しい SQL ユーザー (この例では "superuser") を各シャード データベースに作成してください。 そして、このユーザーがすべての行にアクセスできるようにする新しいの述語関数を使用して、セキュリティ ポリシーを変更してください。

```
-- New predicate function that adds superuser logic
CREATE FUNCTION rls.fn_tenantAccessPredicateWithSuperUser(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult 
        WHERE 
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- note, should not be dbo!
            AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
        ) 
        OR
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('superuser')
        )
GO

-- Atomically swap in the new predicate function on each table
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts
GO
```


### メンテナンス 

* **新しいシャードの追加**: のすべての新しいシャードで RLS を有効にする T-SQL スクリプトを実行する必要があります、それ以外の場合、これらのシャードに対するクエリはフィルターされません。

* **新しいテーブルの追加**: 新しいテーブルを作成するたびに、すべてのシャードのセキュリティ ポリシーにフィルターおよびブロックの述語を追加する必要があります、それ以外の場合、新しいテーブルに対するクエリはフィルターされません。 これを自動化できます、DDL トリガーを使用する」の説明に従って [新しく作成されたテーブル (ブログ) に自動的に行レベルのセキュリティを適用](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx)します。


## 概要 

弾力性データベース ツールと行レベルのセキュリティを組み合わせると、アプリケーションのデータ層をスケール アウトして、マルチテナントのシャードと単一テナントのシャードの両方をサポートできます。 マルチテナントのシャードは、データをより効率的に格納するために使用できます (特に、少数のデータ行を保持するテナントが多数ある場合)。一方、単一テナントのシャードは、より厳密なパフォーマンス要件と分離要件を持つ "プレミアム" テナントをサポートするために使用できます。  詳細については、次を参照してください。、 [弾力性データベース ツールのドキュメント マップ](sql-database-elastic-scale-documentation-map.md) または [行レベルのセキュリティ参照](https://msdn.microsoft.com/library/dn765131) MSDN にします。 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->

 

