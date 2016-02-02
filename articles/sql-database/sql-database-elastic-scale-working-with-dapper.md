<properties 
    pageTitle="Dapper でのエラスティック データベース クライアント ライブラリの使用 |Microsoft Azure" 
    description="Dapper でのエラスティック データベース クライアント ライブラリの使用。" 
    services="sql-database" 
    documentationCenter="" 
    manager="jeffreyg" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/04/2015" 
    ms.author="torsteng;sidneyh"/>


# Dapper でのエラスティック データベース クライアント ライブラリの使用

このドキュメントは、開発者がアプリケーションの構築に Dapper 依存を採用することも [エラスティック データベース ツール](sql-database-elastic-scale-introduction.md) シャーディング データ層をスケール アウトを実装するアプリケーションを作成します。 このドキュメントでは、エラスティック データベース ツールと統合するために Dapper ベースのアプリケーションに加える必要がある変更点を示します。 ここでは、Dapper を使用してエラスティック データベース シャード管理とデータ依存ルーティングを構成する方法を重点的に説明します。

**サンプル コード**: [エラスティック データベース ツール for Azure SQL Database - Dapper integration](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f)します。

**Dapper** と **DapperExtensions** を Azure SQL Database のエラスティック データベース クライアント ライブラリに統合することは容易です。 アプリケーションは、データ依存ルーティングの作成を変更する操作と開く操作 new を使用して [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) オブジェクトを使用する、 [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) からの呼び出し、 [クライアント ライブラリ](http://msdn.microsoft.com/library/azure/dn765902.aspx)します。 これにより、アプリケーションの変更が、新しい接続を作成して開く場所だけに制限されます。

## Dapper の概要

**Dapper** はオブジェクト リレーショナル マッパーです。 Dapper は、アプリケーションの .NET オブジェクトをリレーショナル データベースに (またはその逆に) マップします。 サンプル コードの最初の部分は、エラスティック データベース クライアント ライブラリと Dapper ベースのアプリケーションを統合する方法を示します。 サンプル コードの 2 番目の部分は、Dapper と DapperExtensions の両方を使用する場合の統合方法を示します。

Dapper のマッパー機能には、データベース接続に対する拡張メソッドが用意されています。このメソッドによって、データベースの実行または照会のための T-SQL ステートメントの送信が簡略化されます。 たとえば、Dapper を使用すると、**Execute** の呼び出しでの .NET オブジェクトと SQL ステートメントのパラメーター間のマッピングを容易に行うことができます。また、Dapper から **Query** の呼び出しを使用して、SQL クエリの結果を .NET オブジェクトに容易に組み込むことができます。

DapperExtensions を使用する場合に SQL ステートメントを指定する必要はなくなりました。 データベース接続経由で使用する、**GetList** や **Insert** などの拡張メソッドでは SQL ステートメントがバックグラウンドで作成されます。

Dapper と DapperExtensions のもう 1 つの利点は、データベース接続の作成がアプリケーションによって制御されることです。 これにより、データベースに対するシャードレットのマッピングに基づいてデータベース接続を仲介するエラスティック データベース クライアント ライブラリとやり取りできるようになります。

Dapper アセンブリを取得するには、次を参照してください。 [Dapper dot net](http://www.nuget.org/packages/Dapper/)します。 Dapper の拡張機能では、次を参照してください。 [DapperExtensions](http://www.nuget.org/packages/DapperExtensions)します。

## エラスティック データベース クライアント ライブラリの概要

エラスティック データベース クライアント ライブラリでは、*シャードレット*と呼ばれる、アプリケーション データのパーティションを定義して、データベースにマップし、*シャーディング キー*によって識別します。 任意の数のデータベースを使用して、シャードレットをそれらのデータベースに分散できます。 シャーディング キー値とデータベースとの間のマッピングは、ライブラリの API によって提供されるシャード マップによって格納されます。 この機能は、**シャード マップの管理**と呼ばれます。 シャード マップは、シャーディング キーを格納する要求のデータベース接続用のためのブローカーとしても機能します。 この機能は、**データ依存ルーティング**と呼ばれます。

![シャード マップとデータ依存ルーティング][1]

シャード マップ マネージャーは、データベースでの同時シャードレット管理操作の実行中に発生する可能性があるシャードレット データの一貫性のないビューからユーザーを保護します。 これを実現するために、シャード マップは、ライブラリを使用して構築されたアプリケーションのデータベース接続を仲介します。 これにより、シャード管理操作がシャードレットに影響を与える可能性のあるときに、シャード マップ機能によってデータベース接続を自動的に強制終了できます。

Dapper 用の接続を作成する従来の方法を使用する代わりに使用する必要があります、 [OpenConnectionForKey メソッド](http://msdn.microsoft.com/library/azure/dn824099.aspx)します。 これにより、シャード間でのデータの移動時に、すべての検証が行われ、接続が適切に管理されます。

### Dapper の統合の要件

エラスティック データベース クライアント ライブラリと Dapper API の両方を使用する場合は、次のプロパティを維持することを想定します。

* **スケールアウト**: アプリケーションの容量要求に合わせて必要に応じてシャード化されたアプリケーションのデータ層のデータベースを追加または削除します。

-    **Consistency**: Since our application is scaled out using sharding, we need to perform data dependent routing. We want to use the Data dependent routing capabilities of the library to do so. In particular, we want to retain the validation and consistency guarantees provided by connections that are brokered through the shard map manager in order to avoid corruption or wrong query results. This ensures that connections to a given shardlet are rejected or stopped if (for instance) the shardlet is currently moved to a different shard using Split/Merge APIs.

-    **Object Mapping**: We want to retain the convenience of the mappings provided by Dapper to translate between classes in the application and the underlying database structures. 


次のセクションでは、**Dapper** と **DapperExtensions** に基づくこれらのアプリケーションの要件のガイダンスを示します。

## 技術ガイダンス

### Dapper を使用したデータ依存ルーティング

Dapper では、通常、基になるデータベースへの接続を作成して開くのはアプリケーションです。 アプリケーションで型 T が指定されている場合、Dapper はクエリ結果を型 T の .NET コレクションとして返します。Dapper は、T-SQL の結果行から型 T のオブジェクトへのマッピングを実行します。同様に、Dapper は、データ操作言語 (DML) ステートメント用に .NET オブジェクトを SQL 値またはパラメーターにマップします。 Dapper は、標準の拡張メソッドを使用してこの機能を提供しています [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) ADO .NET SQL クライアント ライブラリからのオブジェクト。 れる DDR 用の Elastic Scale Api によって返される SQL 接続も通常 [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) オブジェクトです。 これにより、クライアント ライブラリの DDR API から返される型を経由して Dapper の拡張機能を直接使用できます (この型も単純な SQL クライアント接続であるため)。

これらの手順によって、Dapper 用のエラスティック データベース クライアント ライブラリが仲介する接続を容易に使用できます。

次のコード例 (付属のサンプルからの抜粋) は、適切なシャードへの接続を仲介するために、アプリケーションがライブラリに対してシャーディング キーを提供するアプローチを示しています。

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

呼び出し、 [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API は、既定の作成と、SQL クライアント接続の開始を置き換えます。  [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) データ依存ルーティングに必要な引数を受け取るを呼び出します。

-    The shard map to access the data dependent routing interfaces

-    The sharding key to identify the shardlet

-    The credentials (user name and password) to connect to the shard


シャード マップ オブジェクトにより、特定のシャーディング キーのシャードレットを保持するシャードへの接続が作成されます。 また、エラスティック データベース クライアント API は接続にタグ付けして、その一貫性を保証します。 呼び出し以降に [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) 正規 SQL クライアント接続オブジェクトを後続の呼び出しを返す、 **Execute** Dapper からの拡張メソッドに依存して Dapper の標準の手法です。

クエリの使用方法はまったく同じ – 接続を使用して、最初に開いた [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) クライアント API からです。 次に、通常の Dapper 拡張メソッドを使用して、SQL クエリの結果を .NET オブジェクトにマップします。

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");
    
           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

DDR 接続を含む **using** ブロックのスコープは、tenantId1 が保持されている 1 つのシャードに対する、そのブロック内のすべてのデータベース操作であることに注目してください。 このクエリが返すのは、現在のシャードに格納されているブログのみです。他のシャードに格納されているブログは返されません。

## Dapper と DapperExtensions を使用したデータ依存ルーティング

Dapper には、データベース アプリケーションの開発時にデータベースからさらなる利便性と抽象化を実現する追加の拡張機能のエコシステムが用意されています。 DapperExtensions はその一例です。

アプリケーションで DapperExtensions を使用しても、データベース接続の作成と管理の方法は変わりません。 接続を開く操作は引き続きアプリケーションが担当し、通常の SQL クライアント接続オブジェクトが拡張メソッドによって使用されます。 そこで依存することができます、 [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) 上記で説明しました。 次のコード サンプルに示すとおり、変更されたのは、T-SQL ステートメントを記述する必要がなくなった点のみです。

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

クエリ用のコード サンプルを次に示します。

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### 一時的エラーの処理

Microsoft Patterns & Practices チームで公開して、 [Transient Fault Handling Application Block](http://msdn.microsoft.com/library/hh680934.aspx) アプリケーション開発者が、クラウドで実行されているときに発生する一般的なの一時的なエラー状態を緩和します。詳細については、次を参照してください。 [忍耐力、Secret of All Triumphs: Transient Fault Handling Application Block を使用して](http://msdn.microsoft.com/library/dn440719.aspx)します。

次のコード サンプルでは、一時的エラーのライブラリを使用して一時的エラーを防ぎます。

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

上記のコードの **SqlDatabaseUtils.SqlRetryPolicy** は **SqlDatabaseTransientErrorDetectionStrategy** として定義され、再試行回数が 10 回、再試行間の待機時間が 5 秒に設定されています。 トランザクションを使用する場合は、一時的エラーの発生時に再試行のスコープがトランザクションの先頭に戻ることをご確認ください。

## 制限事項

このドキュメントで説明した方法には、いくつかの制限事項があります。

* このドキュメントのサンプル コードには、複数のシャードにおけるスキーマの管理方法を示していません。
* 要求を受け取った場合、要求によって提供されたシャーディング キーで識別される 1 つのシャード内にそのすべてのデータベース処理が含まれると見なします。 ただし、この仮定が常に正しいとは限りません。たとえば、シャーディング キーを使用可能にできないことがあります。 これに対処する弾力性データベース クライアント ライブラリに含まれる、 [MultiShardQuery クラス](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx)します。 このクラスは、複数のシャードに対してクエリを実行するための接続の抽象化を実装します。 Dapper と組み合わせた MultiShardQuery の使用方法については、このドキュメントの範囲を超えているため省略します。

## まとめ

Dapper と DapperExtensions を使用するアプリケーションは、Azure SQL Database のエラスティック データベース ツールのメリットを簡単に活用できます。 このドキュメントで説明した手順で、それらのアプリケーションをデータ依存ルーティング作成を変更して、新規の開く操作により、ツールの機能を利用できるよう [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) オブジェクトを使用する、 [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) elastic database クライアント ライブラリの呼び出しです。 これにより、アプリケーションに対して必要な変更が、新しい接続を作成して開く場所だけに制限されます。

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]



[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png 

