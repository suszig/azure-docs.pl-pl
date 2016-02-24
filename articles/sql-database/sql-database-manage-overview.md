<properties 
    pageTitle="概要: SQL Database の管理ツール" 
    description="Azure SQL Database を管理するためのツールとオプションを比較します。" 
    services="sql-database" 
    documentationCenter="" 
    authors="TigerMint" 
    manager="" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/15/2015" 
    ms.author="vinsonyu"/>

# 概要: SQL Database の管理ツール

このトピックでは、仕事や分野、ユーザーに合ったツールを選択できるように、SQL データベースを管理するためのツールとオプションを紹介し、比較します。 適切なツールを選択するには、管理対象のデータベースの数、タスク、タスクの実行頻度を考慮します。



## Azure クラシック ポータル


 [Azure Classic Portal](http://portal.azure.com) web ベース クラシック ポータルですることができますを作成、更新、およびデータベースや論理サーバーを削除およびデータベースの利用状況を監視します。 このツールは、Azure を使い始めたばかりの場合、管理対象のデータベースの数が少ない場合、すばやく作業を行う必要がある場合に便利です。 

詳細については、ポータルを使用して次を参照してください。 [SQL データベースの管理 Azure クラシック ポータルを使用して](sql-database-manage-portal.md)します。

## SQL Server Management Studio と Visual Studio の SQL Server Data Tools


SQL Server Management Studio (SSMS) と Visual Studio の SQL Server Data Tools (SSDT) は、クラウドのデータベースに接続し、管理や開発を行うための、コンピューター上で実行するクライアント ツールです。 アプリケーションの開発者が Visual Studio またはその他の統合開発環境 (Ide) に詳しいなら [Visual Studio の SSDT を使用してください](https://msdn.microsoft.com/library/mt204009.aspx)します。 ハイブリッド環境での SQL Server Database の管理など、SSDT ではまだ使用できない高度な SQL 機能が必要な場合は、SSMS を使用してください。

SSMS を使用して Azure SQL データベースを管理する方法について [SQL データベースの管理 SSMS を使用して](sql-database-manage-azure-ssms.md)


## コマンド ライン ツール

PowerShell などのコマンド ライン ツールを使って、データベースやエラスティック データベース プールを管理したり、Azure のリソースのデプロイを自動化したりできます。 Microsoft は、多数のデータベースを管理する場合や運用環境でリソースの変更およびデプロイを自動化する場合にこのツールをお勧めします。 

コマンド ライン ツールを使用して Azure SQL データベースの管理について [PowerShell を使用した SQL データベースの管理](sql-database-command-line-tools.md)
 

