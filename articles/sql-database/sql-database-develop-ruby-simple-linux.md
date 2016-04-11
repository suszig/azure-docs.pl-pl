<properties
    pageTitle="Ubuntu 上の Ruby と TinyTDS を使用した SQL Database への接続"
    description="Azure SQL Database に接続するために、Ubuntu Linux クライアントとして実行できる Ruby のコード サンプルを提供します。"
    services="sql-database"
    documentationCenter=""
    authors="ajlam"
    manager="jeffreyg"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="12/08/2015"
    ms.author="andrela"/>


# Ubuntu Linux 上の Ruby を使用した SQL Database への接続

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]

このトピックでは、Ubuntu Linux クライアント コンピューター上で実行される Ruby のコード サンプルを示し、Azure SQL Database のデータベースに接続する方法について説明します。

## 前提条件

### 必要なモジュールのインストール

端末を開き、FreeTDS をコンピューターにインストールします (まだインストールしていない場合)。

    sudo apt-get --assume-yes update
    sudo apt-get --assume-yes install freetds-dev freetds-bin

コンピューターで FreeTDS を構成してから、Ruby をコンピューターにインストールします (まだインストールしていない場合)。

    sudo apt-get install libgdbm-dev libncurses5-dev automake libtool bison libffi-dev
    curl -L https://get.rvm.io | bash -s stable

署名に関する問題がある場合は、次のコマンドを実行してください。

    command curl -sSL https://rvm.io/mpapis.asc | gpg --import -

署名に関する問題がない場合は、次のコマンドを実行してください。  

    source ~/.rvm/scripts/rvm
    rvm install 2.1.2
    rvm use 2.1.2 --default
    ruby -v

バージョン 2.1.2 または Ruby の VM が実行されていることを確認します。

次に、TinyTDS をインストールします。

    gem install tiny_tds

### SQL Database

参照してください、 [開始ページ](sql-database-get-started.md) にサンプル データベースを作成する方法について説明します。  作成するガイドを実行する重要である、 **AdventureWorks データベースのテンプレート**します。 のみ以下に示す例を使用、 **AdventureWorks スキーマ**します。


## 手順 1. 接続の詳細を取得する

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## 手順 2. 接続する

 [Tinytds::client](https://github.com/rails-sqlserver/tiny_tds) 関数を使用して、SQL データベースに接続します。

    require 'tiny_tds'
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',
    host: 'yourserver.database.windows.net', port: 1433,
    database: 'AdventureWorks', azure:true

## 手順 3: クエリを実行します。

 [::Result](https://github.com/rails-sqlserver/tiny_tds) 関数を使用して、SQL Database に対するクエリから設定の結果を取得します。 この関数は、クエリを受け入れ、結果セットを返します。 結果セットが反復処理を使用して [することにより行う |row|](https://github.com/rails-sqlserver/tiny_tds)します。

    require 'tiny_tds'  
    print 'test'     
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',
    host: 'yourserver.database.windows.net', port: 1433,
    database: 'AdventureWorks', azure:true
    results = client.execute("select * from SalesLT.Product")
    results.each do |row|
    puts row
    end

## 手順 4: 行を挿入します。

実行する方法を参照してください、この例では、 [挿入](https://msdn.microsoft.com/library/ms174335.aspx) ステートメントからアプリケーションを保護するためのパラメーターを渡すを安全に [SQL インジェクション](https://technet.microsoft.com/library/ms161953(v=sql.105).aspx) 脆弱性、および自動生成された取得 [主キー](https://msdn.microsoft.com/library/ms179610.aspx) 値。  

Azure で TinyTDS を使用するには、いくつかの `SET` ステートメントを実行して現在のセッションが特定の情報を処理する方法を変更することをお勧めします。 推奨 `SET` ステートメントは、コード サンプルに用意されています。 たとえば、`SET ANSI_NULL_DFLT_ON` は、列の NULL 値が許容されることが明示的に宣言されていない場合でも、作成された新しい列が NULL 値を持つことを許可します。

Microsoft SQL Server に合うように [datetime](http://msdn.microsoft.com/library/ms187819.aspx) を使用して、書式設定、 [strftime](http://ruby-doc.org/core-2.2.0/Time.html#method-i-strftime) 関数に対応する日付時刻形式にキャストします。

    require 'tiny_tds'
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',
    host: 'yourserver.database.windows.net', port: 1433,
    database: 'AdventureWorks', azure:true
    results = client.execute("SET ANSI_NULLS ON")
    results = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")
    results = client.execute("SET ANSI_NULL_DFLT_ON ON")
    results = client.execute("SET IMPLICIT_TRANSACTIONS OFF")
    results = client.execute("SET ANSI_PADDING ON")
    results = client.execute("SET QUOTED_IDENTIFIER ON")
    results = client.execute("SET ANSI_WARNINGS ON")
    results = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")
    require 'date'
    t = Time.now
    curr_date = t.strftime("%Y-%m-%d %H:%M:%S.%L")
    results = client.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate)
    OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, '#{curr_date}' )")
    results.each do |row|
    puts row
    end

