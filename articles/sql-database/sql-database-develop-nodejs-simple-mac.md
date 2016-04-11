<properties
    pageTitle="Mac OS X 上で Tedious を含む Node.js を使用して SQL Database に接続する"
    description="Azure SQL Database への接続に使用できる Node.js コード サンプルについて説明します。 サンプルは、Tedious ドライバーを使用して接続します。"
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jeffreyg"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="12/08/2015"
    ms.author="meetb"/>


# Mac OS X 上で Tedious を含む Node.js を使用して SQL Database に接続する


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]


このトピックでは、Mac OS X 上で実行する Node.js のコード サンプルを示します。このサンプルは、Tedious ドライバーを使用して、Azure SQL Database に接続します。


## 前提条件


インストール **ノード**, コンピューターに既にインストールされている場合を除き、します。


OSX 10.10 Yosemite に node.js をインストールするには、によって、ならば便利で簡単にインストールを事前にコンパイルされたバイナリ パッケージをダウンロードすることができます。 [Nodejs.org に進み](http://nodejs.org/) を最新のパッケージをダウンロード、インストール ボタンをクリックします。

両方をインストールを行うインストール ウィザードに従い、.dmg からパッケージをインストール **ノード** と **npm**, 、npm は node.js 用の他のパッケージのインストールを容易にするノード パッケージ マネージャー。


コンピューターを構成した後 **ノード** と **npm**, 、Node.js プロジェクトの作成を予定しているディレクトリに移動、および次のコマンドを入力します。


    npm init
    npm install tedious


**npm init** ノード プロジェクトを作成します。 プロジェクトの作成中に既定値を保持するには、プロジェクトが作成されるまで Enter キーを押します。 確認したら、 **package.json** プロジェクト ディレクトリ内のファイルです。

### SQL Database

参照してください、 [開始ページ](sql-database-get-started.md) にサンプル データベースを作成する方法について説明します。  作成するガイドを実行する重要である、 **AdventureWorks データベースのテンプレート**します。 のみ以下に示す例を使用、 **AdventureWorks スキーマ**します。

## 手順 1. 接続の詳細を取得する

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## 手順 2: 接続

 [新しい接続](http://pekim.github.io/tedious/api-connection.html) 関数を使用して、SQL データベースに接続します。

    var Connection = require('tedious').Connection;
    var config = {
        userName: 'yourusername',
        password: 'yourpassword',
        server: 'yourserver.database.windows.net',
        // If you are on Microsoft Azure, you need this:
        options: {encrypt: true, database: 'AdventureWorks'}
    };
    var connection = new Connection(config);
    connection.on('connect', function(err) {
    // If no error, then good to proceed.
        console.log("Connected");
    });


## 手順 3: クエリを実行します。


使用してすべての SQL ステートメントが実行される、 [新しい Request()](http://pekim.github.io/tedious/api-request.html) 関数です。 受け取ることができます、ステートメントが select ステートメントなどの行を返す場合は、それらを使用して、 [request.on()](http://pekim.github.io/tedious/api-request.html) 関数です。 行がない場合 [request.on()](http://pekim.github.io/tedious/api-request.html) 関数は、空のリストを返します。


    var Connection = require('tedious').Connection;
    var Request = require('tedious').Request;
    var TYPES = require('tedious').TYPES;
    var config = {
        userName: 'yourusername',
        password: 'yourpassword',
        server: 'yourserver.database.windows.net',
        // When you connect to Azure SQL Database, you need these next options.
        options: {encrypt: true, database: 'AdventureWorks'}
    };
    var connection = new Connection(config);
    connection.on('connect', function(err) {
        // If no error, then good to proceed.
        console.log("Connected");
        executeStatement();
    });


    function executeStatement() {
        request = new Request("SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC;", function(err) {
        if (err) {
            console.log(err);}
        });
        var result = "";
        request.on('row', function(columns) {
            columns.forEach(function(column) {
              if (column.value === null) {
                console.log('NULL');
              } else {
                result+= column.value + " ";
              }
            });
            console.log(result);
            result ="";
        });

        request.on('done', function(rowCount, more) {
        console.log(rowCount + ' rows returned');
        });
        connection.execSql(request);
    }


## 手順 4: 行を挿入します。

実行する方法を参照してください、この例では、 [挿入](https://msdn.microsoft.com/library/ms174335.aspx) ステートメントからアプリケーションを保護するためのパラメーターを渡すを安全に [SQL インジェクション](https://technet.microsoft.com/library/ms161953(v=sql.105).aspx) 脆弱性、および自動生成された取得 [主キー](https://msdn.microsoft.com/library/ms179610.aspx) 値。  


    var Connection = require('tedious').Connection;
    var Request = require('tedious').Request
    var TYPES = require('tedious').TYPES;
    var config = {
        userName: 'yourusername',
        password: 'yourpassword',
        server: 'yourserver.database.windows.net',
        // If you are on Azure SQL Database, you need these next options.
        options: {encrypt: true, database: 'AdventureWorks'}
    };
    var connection = new Connection(config);
    connection.on('connect', function(err) {
        // If no error, then good to proceed.
        console.log("Connected");
        executeStatement1();
    });


    function executeStatement1() {
        request = new Request("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate) OUTPUT INSERTED.ProductID VALUES (@Name, @Number, @Cost, @Price, CURRENT_TIMESTAMP);", function(err) {
         if (err) {
            console.log(err);}
        });
        request.addParameter('Name', TYPES.NVarChar,'SQL Server Express 2014');
        request.addParameter('Number', TYPES.NVarChar , 'SQLEXPRESS2014');
        request.addParameter('Cost', TYPES.Int, 11);
        request.addParameter('Price', TYPES.Int,11);
        request.on('row', function(columns) {
            columns.forEach(function(column) {
              if (column.value === null) {
                console.log('NULL');
              } else {
                console.log("Product id of inserted item is " + column.value);
              }
            });
        });     
        connection.execSql(request);
    }


## 次のステップ

詳細については、次を参照してください。、 [Node.js デベロッパー センター](/develop/nodejs/)します。

