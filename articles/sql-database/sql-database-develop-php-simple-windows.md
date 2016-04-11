<properties
    pageTitle="Windows 上で PHP を使用して SQL Database に接続する"
    description="Windows クライアントから、Azure SQL Database に接続して、クライアントが必要とするソフトウェア コンポーネントへのリンクを提供するサンプル PHP プログラムを示します。"
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jeffreyg"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="12/08/2015"
    ms.author="meetb"/>


# Windows 上で PHP を使用して SQL Database に接続する


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


このトピックでは、Windows 上で実行される PHP で記述されたクライアント アプリケーションから Azure SQL Database に接続する方法について説明します。


[AZURE.INCLUDE [sql-database-develop-includes-prerequisites-php-windows](../../includes/sql-database-develop-includes-prerequisites-php-windows.md)]

### SQL Database

参照してください、 [開始ページ](sql-database-get-started.md) にサンプル データベースを作成する方法について説明します。  作成するガイドを実行する重要である、 **AdventureWorks データベースのテンプレート**します。 のみ以下に示す例を使用、 **AdventureWorks スキーマ**します。


## 手順 1. 接続の詳細を取得する

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]


## 手順 2: 接続


これは、 **OpenConnection** すべて次の関数の先頭付近にある関数が呼び出されます。


    function OpenConnection()
    {
        try
        {
            $serverName = "tcp:myserver.database.windows.net,1433";
            $connectionOptions = array("Database"=>"AdventureWorks",
                "Uid"=>"MyUser", "PWD"=>"MyPassword");
            $conn = sqlsrv_connect($serverName, $connectionOptions);
            if($conn == false)
                die(FormatErrors(sqlsrv_errors()));
        }
        catch(Exception $e)
        {
            echo("Error!");
        }
    }


## 手順 3: クエリを実行します。

 [Sqlsrv_query()](http://php.net/manual/en/function.sqlsrv-query.php) 関数を使用して、結果 SQL Database に対するクエリのセットを取得します。 この関数は本質的には任意のクエリと接続オブジェクトを受け入れるしの使用を反復処理できる結果セットを返します [sqlsrv_fetch_array()](http://php.net/manual/en/function.sqlsrv-fetch-array.php)します。

    function ReadData()
    {
        try
        {
            $conn = OpenConnection();
            $tsql = "SELECT [CompanyName] FROM SalesLT.Customer";
            $getProducts = sqlsrv_query($conn, $tsql);
            if ($getProducts == FALSE)
                die(FormatErrors(sqlsrv_errors()));
            $productCount = 0;
            while($row = sqlsrv_fetch_array($getProducts, SQLSRV_FETCH_ASSOC))
            {
                echo($row['CompanyName']);
                echo("<br/>");
                $productCount++;
            }
            sqlsrv_free_stmt($getProducts);
            sqlsrv_close($conn);
        }
        catch(Exception $e)
        {
            echo("Error!");
        }
    }


## 手順 4: 行を挿入します。

実行する方法を参照してください、この例では、 [挿入](https://msdn.microsoft.com/library/ms174335.aspx) ステートメントからアプリケーションを保護するためのパラメーターを渡すを安全に [SQL インジェクション](https://technet.microsoft.com/library/ms161953(v=sql.105).aspx) 脆弱性、および自動生成された取得 [主キー](https://msdn.microsoft.com/library/ms179610.aspx) 値。  


    function InsertData()
    {
        try
        {
            $conn = OpenConnection();

            $tsql = "INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT            INSERTED.ProductID VALUES ('SQL Server 1', 'SQL Server 2', 0, 0, getdate())";
            //Insert query
            $insertReview = sqlsrv_query($conn, $tsql);
            if($insertReview == FALSE)
                die(FormatErrors( sqlsrv_errors()));
            echo "Product Key inserted is :";
            while($row = sqlsrv_fetch_array($insertReview, SQLSRV_FETCH_ASSOC))
            {   
                echo($row['ProductID']);
            }
            sqlsrv_free_stmt($insertReview);
            sqlsrv_close($conn);
        }
        catch(Exception $e)
        {
            echo("Error!");
        }
    }

## 手順 5: トランザクションをロールバックします。


このコード例は、以下のトランザクションの使用について示します。

- トランザクションの開始

- データ行の挿入、別のデータ行の更新

- 挿入と更新に成功した場合はトランザクションをコミット。そのどちらか一方に失敗した場合はトランザクションをロールバック


    function Transactions()
    {
        try
        {
            $conn = OpenConnection();

            if (sqlsrv_begin_transaction($conn) == FALSE)
                die(FormatErrors(sqlsrv_errors()));

            $tsql1 = "INSERT INTO SalesLT.SalesOrderDetail (SalesOrderID,OrderQty,ProductID,UnitPrice)
            VALUES (71774, 22, 709, 33)";
            $stmt1 = sqlsrv_query($conn, $tsql1);

            /* Set up and execute the second query. */
            $tsql2 = "UPDATE SalesLT.SalesOrderDetail SET OrderQty = (OrderQty + 1) WHERE ProductID = 709";
            $stmt2 = sqlsrv_query( $conn, $tsql2);

            /* If both queries were successful, commit the transaction. */
            /* Otherwise, rollback the transaction. */
            if($stmt1 && $stmt2)
            {
                   sqlsrv_commit($conn);
                   echo("Transaction was commited");
            }
            else
            {
                sqlsrv_rollback($conn);
                echo "Transaction was rolled back.\n";
            }
            /* Free statement and connection resources. */
            sqlsrv_free_stmt( $stmt1);
            sqlsrv_free_stmt( $stmt2);
        }
        catch(Exception $e)
        {
            echo("Error!");
        }
    }


## 次のステップ


PHP のインストールと使用に関する詳細については、次を参照してください。 [PHP で SQL Server データベースをアクセスする](http://technet.microsoft.com/library/cc793139.aspx)です。

