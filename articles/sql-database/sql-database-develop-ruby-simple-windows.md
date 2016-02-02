<properties
    pageTitle="Windows 上の Ruby と TinyTDS を使用した SQL Database への接続"
    description="Windows で実行して Azure SQL Database に接続できる Ruby コード サンプルを提供します。"
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jeffreyg"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="12/08/2015"
    ms.author="mebha"/>



# Windows 上で Ruby を使用して SQL Database に接続する

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)]

このトピックでは、Windows 8.1 を実行している Windows コンピューターで実行される Ruby コード サンプルで Azure SQL Database のデータベースに接続する方法を示します。

## 前提条件

### 必要なモジュールのインストール

ターミナルを開き、次をインストールします。

**1) Ruby:** コンピューターに Ruby がインストールされていない場合は、インストールを行ってください。 新規の Ruby ユーザーには、Ruby 2.1.X インストーラーの使用をお勧めします。 これらのインストーラーは、安定した言語を提供すると共に、互換性を有し更新済みであるパッケージ (gem) の広範なリストを提供します。 [Ruby のダウンロード ページに移動](http://rubyinstaller.org/downloads/) し適切な 2.1.x インストーラーをダウンロードします。 たとえば、64 ビット コンピューターをご使用の場合は、**Ruby 2.1.6 (x64)** インストーラーをダウンロードします。
<br/><br/>インストーラーをダウンロードすると後、は、次の操作を行います。


- インストーラーを起動するファイルをダブルクリックします。

- 使用する言語を選択し、利用規約に同意します。

- インストール設定画面で、*[パスに Ruby 実行可能ファイルを追加する]* と *[.rb および .rbw ファイルをこの Ruby インストールに関連付ける]* の横にあるチェックボックスを両方ともオンにします。


**2) キット:** 、からダウンロード キット [RubyInstaller ページ](http://rubyinstaller.org/downloads/)

ダウンロードが完了したら、次の操作を行います。


- ファイルをダブルクリックします。 ファイルを抽出する場所を指定するよう求められます。

- [...] ボタンをクリックし、"C:\DevKit" を選択します。 ほとんどの場合は、最初にこのフォルダーを作成する必要があります。それには、[新しいフォルダーの作成] をクリックします。

- [OK]、[抽出] の順にクリックして、ファイルを抽出します。


ここで、[コマンド ライン プロンプト] を開き、次のコマンドを入力します。

    > chdir C:\DevKit
    > ruby dk.rb init
    > ruby dk.rb install

Ruby と RubyGems が完全に機能する状態になりました。


**3) TinyTDS:** C:\DevKit に進み、端末から次のコマンドを実行します。 これにより、TinyTDS がコンピューターにインストールされます。

    gem inst tiny_tds --pre

### SQL Database

参照してください、 [開始ページ](sql-database-get-started.md) にサンプル データベースを作成する方法について説明します。 ガイドに従って、**AdventureWorks データベースのテンプレート**を作成することが重要です。 以下に示す例は、**AdventureWorks スキーマ** とのみ動作します。


## 手順 1. 接続の詳細を取得する

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## 手順 2: 接続

[Tinytds::client](https://github.com/rails-sqlserver/tiny_tds) 関数を使用して、SQL データベースに接続します。

    require 'tiny_tds'
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',
    host: 'yourserver.database.windows.net', port: 1433,
    database: 'AdventureWorks', azure:true

## 手順 3: クエリを実行します。

次のコードをコピーして、空のファイルに貼り付けます。 test.rb という名前を付けます。 コマンド プロンプトから次のコマンドを入力して、ファイルを実行します。

    ruby test.rb

コード サンプルでは、 [::result](https://github.com/rails-sqlserver/tiny_tds) 関数を使用して、SQL Database に対するクエリから設定の結果を取得します。 この関数は、クエリを受け入れ、結果セットを返します。 結果セットがを使用して反復処理 [することにより行う |row|](https://github.com/rails-sqlserver/tiny_tds).

    require 'tiny_tds'  
    print 'test'     
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',
    host: 'yourserver.database.windows.net', port: 1433,
    database: 'AdventureWorks', azure:true
    results = client.execute("SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC")
    results.each do |row|
    puts row
    end

## 手順 4: 行を挿入します。

実行する方法を参照してください、この例では、 [挿入](https://msdn.microsoft.com/library/ms174335.aspx) ステートメントからアプリケーションを保護するためのパラメーターを渡すを安全に [SQL インジェクション](https://technet.microsoft.com/library/ms161953(v=sql.105).aspx) 脆弱性、および自動生成された取得 [主キー](https://msdn.microsoft.com/library/ms179610.aspx) 値。

Azure で TinyTDS を使用することをお勧めのいくつか実行する `設定` ステートメントを現在のセッションが特定の情報がどのように処理する方法を変更します。 推奨 `設定` ステートメントは、コード サンプルに用意されています。 たとえば、 `設定 ANSI_NULL_DFLT_ON` 列の null 値許容ステータスが明示的に宣言されていない場合でも、null 値を許可するように作成された新しい列が許可されます。

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



