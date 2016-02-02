<properties
    pageTitle="Windows 上で JDBC を含む Java を使用して、SQL Database に接続する"
    description="Azure SQL Database への接続に使用できる Java コード サンプルについて説明します。サンプルは JDBC を使用し、Windows クライアント コンピューター上で実行されます。"
    services="sql-database"
    documentationCenter=""
    authors="LuisBosquez"
    manager="jeffreyg"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="12/08/2015"
    ms.author="lbosq"/>



# Windows 上で JDBC を含む Java を使用して、SQL Database に接続する

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


このトピックでは、Azure SQL Database への接続に使用できる Java コード サンプルについて説明します。 Java サンプルは、Java Development Kit (JDK) バージョン 1.8 に依存します。 サンプルは、JDBC ドライバーを使用して、Azure SQL Database に接続されます。


## 前提条件

### ドライバーとライブラリ

- [Microsoft JDBC Driver for SQL Server - SQL JDBC 4](http://www.microsoft.com/download/details.aspx?displaylang=en&id=11774)します。
- 実行する任意のオペレーティング システム プラットフォーム [Java Development Kit 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)します。

### SQL Database

参照してください、 [開始ページ](sql-database-get-started.md) にデータベースを作成する方法について説明します。

### SQL テーブル

このトピックの Java コード例は、次のようなテスト テーブルが Azure SQL Database のデータベース内に既に存在していることを前提としています。




    CREATE TABLE Person
    (
        id         INT    PRIMARY KEY    IDENTITY(1,1),
        firstName  VARCHAR(32),
        lastName   VARCHAR(32),
        age        INT
    );

## 手順 1. 接続文字列を取得する

[AZURE.INCLUDE [sql-database-include-connection-string-jdbc-20-portalshots](../../includes/sql-database-include-connection-string-jdbc-20-portalshots.md)]
> [AZURE.NOTE] JTDS JDBC ドライバーを使用している場合は、接続文字列の URL に "ssl=require" を追加し、JVM の次のオプションを設定する必要があります。"-Djsse.enableCBCProtection=false"。 この JVM オプションはセキュリティの脆弱性を修正するプログラムを無効にするため、このオプションを設定する前に、どのようなリスクがあるかを必ず理解しておいてください。


## 手順 2: Java コード サンプルをコンパイルします。

Java サンプル コードの全体について説明します。 後続のセクションには小さい Java セグメントをコピー アンド ペーストする場所を示すコメントが含まれています。 このセクションのサンプルはコメントの近くでコピー アンド ペーストしなくてもコンパイルされ、実行されますが、接続して、終了するのみになります。 コメントを次に示します。


1. `2 つの行をテーブルに挿入します。`
2. `トランザクションと更新をコミットします。`
3. `テーブルから行を選択します。`


次に Java コード サンプルの全体について説明します。 このサンプルには、 `メイン` の関数、 `SQLDatabaseTest` クラスです。


    import java.sql.*;
    import com.microsoft.sqlserver.jdbc.*;
    
    public class SQLDatabaseTest {
    
        public static void main(String[] args) {
            String connectionString =
                "jdbc:sqlserver://your_server.database.windows.net:1433;"
                + "database=your_database;"
                + "user=your_user@your_server;"
                + "password=your_password;"
                + "encrypt=true;"
                + "trustServerCertificate=false;"
                + "hostNameInCertificate=*.database.windows.net;"
                + "loginTimeout=30;";
    
            // Declare the JDBC objects.
            Connection connection = null;
            Statement statement = null;
            ResultSet resultSet = null;
            PreparedStatement prepsInsertPerson = null;
            PreparedStatement prepsUpdateAge = null;
    
            try {
                connection = DriverManager.getConnection(connectionString);
    
                // INSERT two rows into the table.
                // ...
    
                // TRANSACTION and commit for an UPDATE.
                // ...
    
                // SELECT rows from the table.
                // ...
            }
            catch (Exception e) {
                e.printStackTrace();
            }
            finally {
                // Close the connections after the data has been handled.
                if (prepsInsertPerson != null) try { prepsInsertPerson.close(); } catch(Exception e) {}
                if (prepsUpdateAge != null) try { prepsUpdateAge.close(); } catch(Exception e) {}
                if (resultSet != null) try { resultSet.close(); } catch(Exception e) {}
                if (statement != null) try { statement.close(); } catch(Exception e) {}
                if (connection != null) try { connection.close(); } catch(Exception e) {}
            }
        }
    }

もちろん、実際に以前の Java コード サンプルを実行するには、実際の値を接続文字列に入力して、プレース ホルダーを置き換える必要があります。


- your_server
- your_database
- your_user
- your_password


## 手順 3. 行を挿入する

この Java セグメントは、TRANSACT-SQL の INSERT ステートメントを発行して、Person テーブルに 2 つの行を挿入します。 一般的な順序は次のとおりです。


1. 生成、 `PreparedStatement` オブジェクトを使用して、 `接続` オブジェクトです。
 - パラメーターが含まれて `Statement.RETURN_GENERATED_KEYS` 後に自動的に生成された値を取得できるように、 **id** キーの値。
2. 呼び出す、 `実行` メソッドを `PreparedStatement` オブジェクトです。
3. 使用してプライマリ キーに自動的に生成された数値を取得、 `PreparedStatement` オブジェクトです。
 - これは Person テーブルの **id** 列の AUTO_INCREMENT 仕様に関連しています。


この短い Java セグメント化、コメントが表示されているプライマリ コード サンプルをコピー アンド ペースト `//テーブルに 2 つの行を挿入します。`します。


    // Create and execute an INSERT SQL prepared statement.
    String insertSql = "INSERT INTO Person (firstName, lastName, age) VALUES "
        + "('Bill', 'Gates', 59), "
        + "('Steve', 'Ballmer', 59);";
    
    prepsInsertPerson = connection.prepareStatement(
        insertSql,
        Statement.RETURN_GENERATED_KEYS);
    prepsInsertPerson.execute();
    // Retrieve the generated key from the insert.
    resultSet = prepsInsertPerson.getGeneratedKeys();
    // Iterate through the set of generated keys.
    while (resultSet.next()) {
        System.out.println("Generated: " + resultSet.getString(1));
    }

## 手順 4. トランザクションをコミットする

次の Java コード セグメントを増やすには TRANSACT-SQL UPDATE ステートメントが発行される、 `年齢` person テーブルのすべての行の値。 一般的な順序は次のとおりです。


1. `SetAutoCommit` を更新プログラムがデータベースに自動的にコミットされることを防ぐために呼び出されます。
2. `ExecuteUpdate` メソッドが呼び出され、トランザクションのコンテキストで更新を実行します。
3. `コミット` 明示的にトランザクションをコミットするメソッドが呼び出されます。


この短い Java セグメント化、コメントが表示されているプライマリ コード サンプルをコピー アンド ペースト `//トランザクションと更新をコミットします。`します。


    // Set AutoCommit value to false to execute a single transaction at a time.
    connection.setAutoCommit(false);
    
    // Write the SQL Update instruction and get the PreparedStatement object.
    String transactionSql = "UPDATE Person SET Person.age = Person.age + 1;";
    prepsUpdateAge = connection.prepareStatement(transactionSql);
    
    // Execute the statement.
    prepsUpdateAge.executeUpdate();
    
    //Commit the transaction.
    connection.commit();
    
    // Return the AutoCommit value to true.
    connection.setAutoCommit(true);

## 手順 5. クエリを実行する

この Java セグメントは、TRANSACT-SQL SELECT ステートメントを実行して、Person テーブルから更新されたすべての行を表示します。 一般的な順序は次のとおりです。


1. 生成、 `ステートメント` オブジェクトを使用して、 `接続` オブジェクトです。
2. 生成、 `ResultSet` オブジェクトを使用して、 `ステートメント` オブジェクトです。
3. 呼び出しをループ `resultSet.next` 、返されたすべての行を反復処理します。


この短い Java セグメント化、コメントが表示されているプライマリ コード サンプルをコピー アンド ペースト `/]、[テーブルから行を選択します。`します。


    // Create and execute a SELECT SQL statement.
    String selectSql = "SELECT firstName, lastName, age FROM dbo.Person";
    statement = connection.createStatement();
    resultSet = statement.executeQuery(selectSql);
    
    // Iterate through the result set and print the attributes.
    while (resultSet.next()) {
        System.out.println(resultSet.getString(2) + " "
            + resultSet.getString(3));
    }

## 次のステップ

詳細については、次を参照してください。、 [Java デベロッパー センター](/develop/java/)します。





