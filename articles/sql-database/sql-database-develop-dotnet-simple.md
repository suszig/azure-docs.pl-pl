<properties
    pageTitle=".NET (C#) を使用して SQL Database に接続する"
    description="このクイック スタートのコード サンプルを使用して、C# で最新のアプリケーションを構築し、Azure SQL Database を使用してクラウドの強力なリレーショナル データベースでバックアップします。"
    services="sql-database"
    documentationCenter=""
    authors="tobbox"
    manager="jeffreyg"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="12/08/2015"
    ms.author="tobiast"/>



# .NET (c#) からの SQL Database 使用

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


## 前提条件

### .NET Framework

.NET framework は、Windows にプレインストールされています。 Linux と Mac OS X から .NET Framework をダウンロードすることができます、 [Mono プロジェクト](http://www.mono-project.com/)します。

### SQL Database

参照してください、 [開始ページ](sql-database-get-started.md) にサンプル データベースを作成する方法について説明します。 ガイドに従って、**AdventureWorks データベースのテンプレート**を作成することが重要です。 以下に示す例は、**AdventureWorks スキーマ** とのみ動作します。

## 手順 1: 接続文字列を取得します。

[AZURE.INCLUDE [sql-database-include-connection-string-dotnet-20-portalshots](../../includes/sql-database-include-connection-string-dotnet-20-portalshots.md)]

## 手順 2: 接続

[System.Data.SqlClient.SqlConnection クラス](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) SQL データベースに接続するために使用します。


```
using System.Data.SqlClient;

class Sample
{
  static void Main()
  {
      using(var conn = new SqlConnection("Server=tcp:yourserver.database.windows.net,1433;Database=yourdatabase;User ID=yourlogin@yourserver;Password={yourpassword};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"))
      {
          conn.Open();
      }
  }
}
```

## 手順 3. クエリを実行する

[System.Data.SqlClient.SqlCommand](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx) と [SqlDataReader](https://msdn.microsoft.com/library/system.data.sqlclient.sqldatareader.aspx) クラスを使用して、結果 SQL Database に対するクエリのセットを取得します。 注 System.Data.SqlClient もサポートしているオフラインへのデータの取得 [System.Data.DataSet](https://msdn.microsoft.com/library/system.data.dataset.aspx)します。

```
using System;
using System.Data.SqlClient;

class Sample
{
    static void Main()
    {
      using(var conn = new SqlConnection("Server=tcp:yourserver.database.windows.net,1433;Database=yourdatabase;User ID=yourlogin@yourserver;Password={yourpassword};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"))
        {
            var cmd = conn.CreateCommand();
            cmd.CommandText = @"
                    SELECT
                        c.CustomerID
                        ,c.CompanyName
                        ,COUNT(soh.SalesOrderID) AS OrderCount
                    FROM SalesLT.Customer AS c
                    LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID
                    GROUP BY c.CustomerID, c.CompanyName
                    ORDER BY OrderCount DESC;";

            conn.Open();

            using(var reader = cmd.ExecuteReader())
            {
                while(reader.Read())
                {
                    Console.WriteLine("ID: {0} Name: {1} Order Count: {2}", reader.GetInt32(0), reader.GetString(1), reader.GetInt32(2));
                }
            }                   
        }
    }
}
```

## 手順 4. 行を挿入する

実行する方法を参照してください、この例では、 [挿入](https://msdn.microsoft.com/library/ms174335.aspx) ステートメントからアプリケーションを保護するためのパラメーターを渡すを安全に [SQL インジェクション](https://technet.microsoft.com/library/ms161953(v=sql.105).aspx) 脆弱性、および自動生成された取得 [主キー](https://msdn.microsoft.com/library/ms179610.aspx) 値。

```
using System;
using System.Data.SqlClient;

class Sample
{
    static void Main()
    {
        using(var conn = new SqlConnection("Server=tcp:yourserver.database.windows.net,1433;Database=yourdatabase;User ID=yourlogin@yourserver;Password={yourpassword};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"))
        {
            var cmd = conn.CreateCommand();
            cmd.CommandText = @"
                INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate)
                OUTPUT INSERTED.ProductID
                VALUES (@Name, @Number, @Cost, @Price, CURRENT_TIMESTAMP)";

            cmd.Parameters.AddWithValue("@Name", "SQL Server Express");
            cmd.Parameters.AddWithValue("@Number", "SQLEXPRESS1");
            cmd.Parameters.AddWithValue("@Cost", 0);
            cmd.Parameters.AddWithValue("@Price", 0);

            conn.Open();

            int insertedProductId = (int)cmd.ExecuteScalar();

            Console.WriteLine("Product ID {0} inserted.", insertedProductId);
        }
    }
}
```





