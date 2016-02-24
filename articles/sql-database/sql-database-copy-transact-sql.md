<properties 
    pageTitle="Transact-SQL を使用した Azure SQL Database のコピーの作成" 
    description="Transact-SQL を使用した Azure SQL Database のコピーの作成" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jeffreyg"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="12/01/2015"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# Transact-SQL を使用した Azure SQL Database のコピーの作成

**1 つのデータベース**

> [AZURE.SELECTOR]
- [Azure ポータル](sql-database-copy.md)
- [PowerShell](sql-database-copy-powershell.md)
- [SQL](sql-database-copy-transact-sql.md)



次の手順は、Transact-SQL を使って SQL データベースをコピーする方法を示しています。 データベース コピー操作では、使用して新しいデータベースに SQL データベースをコピー、 [CREATE DATABASE]() ステートメントです。 コピーは、同じサーバーか別のサーバーで作成するデータベースのスナップショット バックアップです。


> [AZURE.NOTE] Azure SQL データベースは自動的に作成され、すべてのユーザー データベースを復元するバックアップを保持します。 詳細については、「 [ビジネス継続性の概要](sql-database-business-continuity.md)します。


コピー プロセスが完了すると、新しいデータベースは、コピー元のデータベースに依存せずに完全に機能するデータベースになります。 コピーの完了時点で、新しいデータベースのトランザクションはコピー元のデータベースと同じになります。 データベース コピーのサービス レベルとパフォーマンス レベル (価格レベル) はコピー元のデータベースと同じになります。 コピーの完了後、コピーは完全に機能する独立したデータベースになります。 ログイン、ユーザー、アクセス許可は非依存で管理できます。


データベースを同じ論理サーバーにコピーすると、両方のデータベースで同じログインを利用できます。 データベースをコピーするために使用するセキュリティ プリンシパルが、新しいデータベースのデータベース所有者 (DBO) になります。 すべてのデータベース ユーザー、アクセス許可、セキュリティ識別子 (SID) がデータベースのコピーにコピーされます。


この記事の手順を完了するには、次のものが必要です。

- Azure サブスクリプション。 をクリックする必要がある場合、Azure サブスクリプションだけで **無料評価版** これの上部にあるページをこの記事を完了する通します。
- Azure SQL Database。 SQL データベースがない、1 つ作成この記事の手順: [最初の Azure SQL Database を作成する](sql-database-get-started.md)です。
- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms174173.aspx)します。 SSMS を持っていない場合、またはこの記事で説明する機能が利用できない [最新バージョンをダウンロード](https://msdn.microsoft.com/library/mt238290.aspx)します。




## SQL データベースのコピー

サーバーレベル プリンシパル ログインか、コピーするデータベースを作成したときのログインを使用して、マスター データベースにログオンします。 サーバーレベル プリンシパルではないログインでデータベースをコピーするには、dbmanager ロールのメンバーである必要があります。 ログインとサーバーへの接続の詳細については、Azure SQL Database でのデータベース、ログイン、ユーザーの管理に関するページと、Azure SQL Database 開発の操作方法に関するトピックをそれぞれ参照してください。

CREATE DATABASE ステートメントを使用して、コピー元データベースのコピーを開始します。 このステートメントを実行すると、データベースのコピー処理が始まります。 データベースのコピーは非同期的な処理であるため、CREATE DATABASE ステートメントは、データベースのコピーが完了する前に復帰します。


### 同じサーバーへの SQL データベースのコピー

サーバーレベル プリンシパル ログインか、コピーするデータベースを作成したときのログインを使用して、マスター データベースにログオンします。 サーバーレベル プリンシパルではないログインでデータベースをコピーするには、dbmanager ロールのメンバーである必要があります。

このコマンドは、Database1 を、同じサーバー上の Database2 という名前の新しいデータベースにコピーします。 データベースのサイズに応じて、コピー操作の完了に時間がかかる場合があります。

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### 別のサーバーへの SQL データベースのコピー

コピー先サーバー (新しいデータベースを作成する Azure SQL Database サーバー) のマスター データベースにログオンします。 コピー元の Azure SQL Database サーバー上のコピー元データベースのデータベース所有者 (DBO) と同じ名前とパスワードを持つログインを使用します。 コピー先サーバーでのログインも、dbmanager ロールのメンバーであるか、サーバーレベル プリンシパル ログインである必要があります。

このコマンドは、server1 上の Database1 を、server2 上の Database2 という新しいデータベースにコピーします。 データベースのサイズに応じて、コピー操作の完了に時間がかかる場合があります。


    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;
    

## コピー操作の進行状況の監視

sys.databases ビューと sys.dm_database_copies ビューを照会して、コピー操作を監視します。 コピー操作の進行中は、新しいデータベースの sys.databases ビューの state_desc 列が COPYING に設定されます。


- コピー操作に失敗すると、新しいデータベースの sys.databases ビューの state_desc 列が SUSPECT に設定されます。 この場合は、新しいデータベースに対して DROP ステートメントを実行した後でもう一度やり直してください。
- コピー操作に成功すると、新しいデータベースの sys.databases ビューの state_desc 列が ONLINE に設定されます。 この場合、コピー操作は完了しています。新しいデータベースは通常のデータベースであり、コピー元データベースから独立して変更することができます。



## 次のステップ


- 進行中のコピーをキャンセルする場合は、実行、 [DROP DATABASE](https://msdn.microsoft.com/library/ms178613.aspx) ステートメントを新しいデータベースに対して。 また、コピー元のデータベースに対して DROP DATABASE ステートメントを実行することによっても、コピー操作を取り消すことができます。
- 新しいデータベースが移行先サーバーでオンラインになってを使用して、 [ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) ユーザーを新しいデータベースを移行先サーバーでログインを再マップするステートメントです。 新しいデータベースのすべてのユーザーは、コピー元データベースで保持していたアクセス許可を保持します。 データベースのコピーを開始したユーザーが新しいデータベースのデータベース所有者になり、新しいセキュリティ識別子 (SID) が割り当てられます。 コピーが成功した後、他のユーザーが再マップされるまでは、コピーを開始したログイン、つまりデータベース所有者 (DBO) のみが新しいデータベースにログオンできます。




## その他のリソース

- [ビジネス継続性の概要](sql-database-business-continuity.md)
- [障害復旧訓練](sql-database-disaster-recovery-drills.md)
- [SQL Database のドキュメント](https://azure.microsoft.com/documentation/services/sql-database/)

