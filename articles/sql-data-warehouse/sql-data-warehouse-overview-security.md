<properties
   pageTitle="SQL Data Warehouse でのデータベースの保護 | Microsoft Azure"
   description="ソリューション開発のための Azure SQL Data Warehouse でのデータベース保護に関するヒント。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sahaj08"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/15/2015"
   ms.author="sahajs"/>


# SQL Data Warehouse でのデータベース保護

この記事では、Azure SQL Data Warehouse データベースの保護に関する基本事項を説明します。 特にこの記事では、アクセスの制限、データの保護、およびデータベースでのアクティビティの監視を行うためのリソースの概要を説明します。

## 接続のセキュリティ

接続のセキュリティとは、ファイアウォール ルールと接続の暗号化を使用して、データベースへの接続を制限し、保護する方法のことです。

ファイアウォール ルールはサーバーとデータベースの両方で使用され、明示的にホワイト リストに登録されていない IP アドレスからの接続試行を拒否します。 新しいデータベースへの接続を試行するために、アプリケーションまたはクライアント コンピューターのパブリック IP アドレスを許可するには、まず Microsoft Azure クラシック ポータル、REST API、または PowerShell を使用して、サーバーレベルのファイアウォール ルールを作成する必要があります。 ベスト プラクティスとして、可能な限りサーバーのファイアウォールにより許可される IP アドレスの範囲を制限する必要があります。 詳細については、次を参照してください。 [Azure SQL Database ファイアウォールの []][]します。


## 認証

認証とは、データベースへの接続時に ID を証明する方法のことです。 SQL Data Warehouse では現在、ユーザー名とパスワードを使用した SQL 認証がサポートされています。

データベースの論理サーバーを作成したときに、ユーザー名とパスワードによる "サーバー管理" ログインを指定したとします。 これらの資格情報を使用すると、データベース所有者、つまり "dbo" として、そのサーバーにある任意のデータベースを認証できます。

ただし、ベスト プラクティスとして、組織のユーザーは別のアカウントを使用して認証する必要があります。 この方法により、アプリケーションに付与されるアクセス許可を制限でき、アプリケーション コードが SQL インジェクション攻撃に対して脆弱な場合に、悪意のあるアクティビティのリスクを軽減できます。 サーバー ログインに基づいてデータベース ユーザーを作成するには:

最初に、サーバーのマスター データベースにサーバー管理ログインを使用して接続し、新しいサーバー ログインを作成します。

```
-- Connect to master database and create a login
CREATE LOGIN ApplicationLogin WITH PASSWORD = 'strong_password';
```

次に、SQL Data Warhouse データベースにサーバー管理ログインを使用して接続し、作成したばかりのサーバー ログインに基づいてデータベース ユーザーを作成します。

```

-- Connect to SQL DW database and create a database user
CREATE USER ApplicationUser FOR LOGIN ApplicationLogin;
```

SQL Database の認証の詳細については、次を参照してください。 [[] の Azure SQL データベースにおけるデータベースとログインを管理する][]します。


## 承認

承認とは、Azure SQL Data Warehouse データベース内で実行できる事柄を指し、これはアカウントのロール メンバーシップと権限によって制御されます。 ベスト プラクティスとして、必要最低限の特権をユーザーに付与することをお勧めします。 Azure SQL Data Warehouse により、T-SQL 内のロールで簡単に管理できるようになります。

```
EXEC sp_addrolemember 'db_datareader', 'ApplicationUser'; -- allows ApplicationUser to read data
EXEC sp_addrolemember 'db_datawriter', 'ApplicationUser'; -- allows ApplicationUser to write data
```

接続しているサーバー管理者のアカウントは db_owner のメンバーであり、データベース内ですべての操作を実行する権限を持ちます。 スキーマのアップグレードやその他の管理操作をデプロイするために、このアカウントを保存します。 アクセス許可が限定された "ApplicationUser" アカウントを使用して、アプリケーションで必要な最小限の特権により、アプリケーションをデータベースに接続します。

ユーザーが Azure SQL Database で実行できる操作をさらに制限する方法がいくつかあります。

- [データベースの役割][] 以外の db_datareader と db_datawriter を使用してより強力なアプリケーション ユーザー アカウントまたはより権限の小さな管理アカウントを作成します。
- 詳細な [アクセス許可][] 制御できる操作は個々 の列、テーブル、ビュー、プロシージャ、およびデータベース内の他のオブジェクトを使用します。
- [ストアド プロシージャ][] データベースに対して実行できるアクションを制限するために使用できます。

Azure クラシック ポータルまたは Azure リソース マネージャー API を使用したデータベースと論理サーバーの管理は、ポータル ユーザー アカウントのロールの割り当てによって制御されます。 このトピックの詳細については、次を参照してください。 [Azure ポータルの [] で、ロールベースのアクセス制御][]します。



## 暗号化

Azure SQL Data Warehouse は、"静止"状態にあるときに、データを暗号化することによってデータを保護することができますか、データベース ファイルとを使用して、バックアップに格納されている [透過的なデータ暗号化][]します。 データベースを暗号化するには、サーバーの master データベースに接続して以下を実行します。


```

ALTER DATABASE [AdventureWorks] SET ENCRYPTION ON;
```

データベースの設定から、透過的なデータ暗号化を有効することも、 [Azure 旧ポータルの][]します。



## 監査

データベースの監査イベントと追跡イベントは、規制遵守の維持や、疑わしいアクティビティの特定に役立ちます。 SQL Data Warehouse の監査により、Azure Storage アカウントの監査ログにデータベースのイベントを記録できます。 また SQL Data Warehouse の監査を Microsoft Power BI と統合することにより、詳細なレポートと分析が容易になります。 詳細については、次を参照してください。 [SQL Database の監査 [] を使ってみる][]します。



## 次のステップ

他の開発のヒントを参照してください。 [開発の概要 []][]します。







[development overview]: sql-data-warehouse-overview-develop.md 
[azure sql database firewall]: https://msdn.microsoft.com/library/ee621782.aspx 
[database roles]: https://msdn.microsoft.com/library/ms189121.aspx 
[managing databases and logins in azure sql database]: https://msdn.microsoft.com/library/ee336235.aspx 
[permissions]: https://msdn.microsoft.com/library/ms191291.aspx 
[stored procedures]: https://msdn.microsoft.com/library/ms190782.aspx 
[transparent data encryption]: http://go.microsoft.com/fwlink/?LinkId=526242 
[get started with sql database auditing]: sql-database-auditing-get-started.md 
[azure classic portal]: https://portal.azure.com/ 
[role-based access control in azure portal]: http://azure.microsoft.com/en-us/documentation/articles/role-based-access-control-configure.aspx 

