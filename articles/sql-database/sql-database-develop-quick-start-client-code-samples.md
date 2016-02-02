<properties 
    pageTitle="SQL Database のクライアント クイック スタート コード サンプル | Microsoft Azure" 
    description="Linux での Node.js、Mac OS での Python、Java、Windows、Enterprise Library など、Azure SQL Database クライアント向けのコード サンプルとドライバーを提供します。"
    services="sql-database" 
    documentationCenter="" 
    authors="MightyPen" 
    manager="jeffreyg" 
    editor=""/>


<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/29/2015" 
    ms.author="genemi"/>



# SQL Database のクライアント クイック スタート コード サンプル

このトピックでは、Azure SQL Database への接続に使用するクイック スタート コード サンプルへのリンクを示します。


- 短いサンプルで接続し、クエリを実行します。
- 再試行サンプル接続しクエリが、エラーが発生しましたが、として分類される場合は自動的に再試行を [* 一時的な障害 *](sql-database-develop-error-messages.md#bkmk_connection_errors) (接続タイムアウトなど)。


サンプルは、次のような内容です。


- さまざまなプログラミング言語。
- クライアント プログラムを実行できるオペレーティング システムとしての Windows、Linux、Mac OS。
- 必要なすべての接続ドライバをダウンロードするためのリンク。
- 短いクイック スタート コード サンプル。
- 自動化された再試行ロジックの形式での、一時的なエラーの処理が含まれているより長いサンプル。
- リレーショナル結果セットをオブジェクトに変換するコード サンプル指向の形式。


> [AZURE.NOTE] 現在、他の言語のコード サンプルも準備中であり、それらのリンクはこのトピックに追加される予定です。


## Linux 上のクライアント

このセクションでは、Linux 上で実行されるクライアント プログラムのコード サンプルのトピックへのリンクを提供します。


| 言語| 短いサンプル| 再試行のサンプル| オブジェクトにリレーショナル|
| :-- | :-- | :-- | :-- |
| Node.js| [面倒です](sql-database-develop-nodejs-simple-linux.md)| .| .|
| Python| [FreeTDS、pymssql](sql-database-develop-python-simple-unbutu-linux.md)| .| .|
| Ruby| [FreeTDS、TinyTDS](sql-database-develop-ruby-simple-linux.md)| .| .|


## Mac OS 上のクライアント

このセクションでは、Mac OS 上で実行されるクライアント プログラムのコード サンプルのトピックへのリンクを提供します。


| 言語| 短いサンプル| 再試行のサンプル| オブジェクトにリレーショナル|
| :-- | :-- | :-- | :-- |
| Python| [pymssql](sql-database-develop-python-simple-mac-osx.md)| .| .|
| Ruby| [Homebrew<br/>FreeTDS、TinyTDS](sql-database-develop-ruby-simple-mac-osx.md)| .| .|


## Windows 上のクライアント

このセクションでは、Windows 上で実行されるクライアント プログラムのコード サンプルのトピックへのリンクを提供します。


| 言語| 短いサンプル| 再試行のサンプル| オブジェクトにリレーショナル|
| :-- | :-- | :-- | :-- |
| C#| [ADO.NET](sql-database-develop-dotnet-simple.md)| [ADO.NET custom](sql-database-develop-csharp-retry-windows.md)<br/><br/>[Enterprise Library](sql-database-develop-entlib-csharp-retry-windows.md)| (Entity Framework)|
| C++| [ODBC ドライバー](http://msdn.microsoft.com/library/azure/hh974312.aspx)| .| .|
| Java| [Java。JDK、JDBC。Insert、Transaction を選択します。](sql-database-develop-java-simple-windows.md)| .| .|
| Node.js| [msnodesql](sql-database-develop-nodejs-simple-windows.md)| .| .|
| PHP| [ODBC](sql-database-develop-php-simple-windows.md)| [ODBC (カスタム)](sql-database-develop-php-retry-windows.md)| .|
| Python| [pymssql](sql-database-develop-python-simple-windows.md)| .| .|


## 関連項目

- [Sdk と多数の言語とプラットフォーム向けのツールのダウンロードします。](http://azure.microsoft.com/downloads/#cmd-line-tools)
- [SQL Database と SQL Server の接続ライブラリ](sql-database-libraries.md)
- [一時的なエラーの数値コードの一覧](sql-database-develop-error-messages.md#bkmk_connection_errors)<br/>& nbsp;
- [Azure SQL Database 開発: 操作方法に関するトピック](http://msdn.microsoft.com/library/azure/ee621787.aspx)
- [SQL データベースへの接続: リンク、ベスト プラクティスと設計のガイドライン](sql-database-connect-central-recommendations.md)
- [最初の Azure SQL Database を作成します。](sql-database-get-started.md)
- [Entity Framework 6 ここでは、GitHub の EF 7](http://entityframework.codeplex.com/)






