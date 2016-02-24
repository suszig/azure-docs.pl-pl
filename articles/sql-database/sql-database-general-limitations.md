<properties 
   pageTitle="Azure SQL Database の一般的な制限事項とガイドライン"
   description="このページでは、Azure SQL Database の一般的な制限事項および相互運用性とサポートの領域について説明します。"
   services="sql-database"
   documentationCenter="na"
   authors="rothja"
   manager="jeffreyg"
   editor="monicar" />
<tags 
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="11/16/2015"
   ms.author="jroth" />

# Azure SQL Database の一般的な制限事項とガイドライン

このトピックでは、Azure SQL Database の一般的な制限事項とガイドラインについて説明します。 クォータ、リソース管理、およびサポートを完全に理解を参照してください、 [その他のリソース](#additional-guidelines) このトピックの最後です。

## 接続

 - Windows 認証はサポートされません。 参照してください [Azure SQL データベースにおけるデータベースとログインを管理する](sql-database-manage-logins.md)です。 

 - Microsoft Azure SQL Database では、表形式データ ストリーム (TDS) プロトコル クライアント バージョン 7.3 以降をサポートしています。 

 - TCP/IP 接続のみが許可されます。

 - Microsoft Azure SQL Database には動的ポートがなく、ポート 1433 のみを使用するため、SQL Server 2008 の SQL Server Browser はサポートされません。

## SQL Server エージェント/ジョブ

Microsoft Azure SQL Database では、SQL Server エージェントおよびジョブをサポートしていません。 ただし、オンプレミスの SQL Server で SQL Server エージェントを実行し、Microsoft Azure SQL Database に接続することはできます。

## トランザクション

Azure SQL Database では、分散トランザクション (複数のリソースに影響するトランザクション) をサポートしていません。 詳細については、次を参照してください。 [分散トランザクション (ADO.NET)](https://msdn.microsoft.com/library/ms254973.aspx)します。 SQL Database では、フェールオーバー間で現在のデータベースのコミット前のタイムスタンプ値 (DBTS) が保持されない場合があります。

> [AZURE.NOTE] 特定の状況で、トランザクションを自動的に分散トランザクションに昇格することができます。 詳細については、次を参照してください。 [SQL Server と System.Transactions の統合](https://msdn.microsoft.com/library/ms172070.aspx)します。

## SQL Server 照合順序のサポート

Microsoft Azure SQL Database で使用される既定のデータベース照合順序は **SQL_LATIN1_GENERAL_CP1_CI_AS**, ここで、 **LATIN1_GENERAL** 英語 (米国) **CP1** コード ページ 1252、 **CI** 小文字を区別しない、および **AS** アクセントを区別します。 Transact-SQL を使用して、V12 データベースの照合順序を変更できます。 照合順序を設定する方法の詳細については、次を参照してください。 [COLLATE (TRANSACT-SQL)](https://msdn.microsoft.com/library/ms184391.aspx)します。

## 名前付けに関する要件

セキュリティ上の理由により、特定のユーザー名を使用できません。 使用できない名前は次のとおりです。

 - **admin** 
 - **administrator** 
 - **guest** 
 - **root** 
 - **sa** 

すべての新しいオブジェクトの名前は、SQL Server の識別子に関する規則に従う必要があります。 詳細については、次を参照してください。 [識別子](https://msdn.microsoft.com/library/ms175874.aspx)します。

また、ログインおよびユーザー名に \ 文字を含めることはできません (Windows 認証をサポートしていません)。

## 追加のガイドライン

- SQL データベースが特定のリソースのクォータと制限に基づく一般的な制限事項がこの記事で説明されているだけでなく、 **サービス層**します。 サービス階層の概要については、次を参照してください。 [SQL Database のサービス階層](sql-database-service-tiers.md)します。

- その他の SQL Database の制限を参照してください。 [Azure SQL Database のリソース制限](sql-database-resource-limits.md)します。

- セキュリティ関連のガイドラインを参照してください [Azure SQL Database のセキュリティ ガイドラインと制限事項](sql-database-security-guidelines.md)します。

- 他に関連する領域には、Azure SQL Database とオンプレミスの SQL Server バージョン (SQL Server 2014 など) との互換性があります。 Azure SQL Database の最新の V12 バージョンでは、この領域におけるさまざまな機能強化を図っています。 詳細については、次を参照してください。 [SQL Database V12 の新機能](sql-database-v12-whats-new.md)です。

- ドライバの利用可能性および SQL データベースのサポートについては、次を参照してください。 [SQL Database と SQL Server の接続ライブラリ](sql-database-libraries.md)します。

