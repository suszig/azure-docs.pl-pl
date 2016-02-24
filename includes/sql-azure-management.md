
# SQL Server Management Studio を使用した Azure SQL Database の管理 

SQL Server Management Studio (SSMS) を使用して Azure SQL Database の論理サーバーとデータベースを管理できます。 このトピックでは、SSMS を使用した一般的なタスクの手順について説明します。 始める前に、Azure SQL Database 論理サーバーとデータベースが既にある必要があります。 最初に、読み取り [最初の Azure SQL Database を作成する](sql-database-get-started.md) を通します。

Azure SQL Database を操作する際は、常に最新バージョンの SSMS を使用することをお勧めします。 参照してください [SQL Server Management Studio のダウンロード](https://msdn.microsoft.com/en-us/library/mt238290.aspx) それを取得します。 


## SQL Database 論理サーバーに接続する

SQL データベースに接続するには、Azure 上のサーバー名が必要になります。 この情報を入手するには、ポータルへのサインインが必要になる場合があります

1.  サインイン、 [Azure 管理ポータル](http://manage.windowsazure.com)します。

2.  左のウィンドウで **SQL データベース**します。

3.  SQL データベースのホーム ページをクリックして **サーバー** すべてのサブスクリプションに関連付けられているサーバーの一覧を表示するページの上部にあります。 接続先のサーバーの名前を探し、その名前をクリップボードにコピーします。

    次に、SQL データベース ファイアウォールを構成して、
ローカル コンピューターからの接続を許可します。 それには、ローカル コンピューターの IP アドレスをファイアウォールの例外一覧に追加します。

1.  SQL データベースのホーム ページでをクリックして **サーバー** し接続するサーバーをクリックします。

2.  クリックして **構成** 、ページの上部にあります。

3.  [現在のクライアント IP アドレス] の IP アドレスをコピーします。

4.  構成] ページで **使用できる IP アドレス** 3 つのボックスの開始と終了値、ルール名と、範囲の IP アドレスを指定できます。 ルール名としては、コンピューター名などを入力します。 範囲の開始値および終了値としてコンピューターの IP アドレスを両方のボックスに入力し、表示されるチェック ボックスをオンにします。

    ルール名は一意である必要があります。 開発コンピューターで設定している場合は、[IP 範囲の開始] ボックスと [IP 範囲の終了] ボックスの両方に同じ IP アドレスを入力してもかまいません。 それ以外の場合は、組織の他のコンピューターからの接続に対応するために、より広い範囲の IP アドレスの入力が必要になる可能性があります。
 
5. クリックして **保存** ページの下部にあります。

    **注:** 可能性がありますようになるまで、5 分かかる
    5 分程度かかる可能性があります。

    これで Management Studio を使用して SQL データベースに接続する準備ができました。

1.  タスク バーで、次のようにクリックします **開始**, 、指す **すべてのプログラム**, 、] をポイント。
    **Microsoft SQL Server 2014**, 、クリックして **SQL Server
    Management Studio**します。

2.   **サーバーへの接続**, 、完全修飾を指定します。
    サーバー名として *serverName*..database.windows.net が付きます。 Azure では、サーバー名は英数字から成る自動生成文字列です。

3.  選択 **SQL Server 認証**します。

4.   **ログイン** ボックスに、SQL Server 管理者ログインを入力します。
    サーバーの作成時に、ポータルで指定します。

5.   **パスワード** ボックスで指定したパスワードを入力
    サーバーの作成時にポータルです。

8.  クリックして **接続** 接続を確立します。

最新の更新プログラムを適用した SQL Server 2014 SSMS はサポートが拡張され、
Azure SQL データベースの作成や変更などのタスクが対象になります。 さらに、Transact-SQL ステートメントを使用して、
これらのタスクを実行することもできます。 次に示す手順は、
こうしたステートメントの例を紹介します。 サポートされているコマンドの詳細など、
SQL データベースでの Transact-SQL の使用方法の詳細については、
サポートされているを参照してください [TRANSACT-SQL リファレンス (SQL データベース)](http://msdn.microsoft.com/library/bb510741.aspx)します。

## Azure SQL データベースの作成と管理

接続されている、 **マスター** 、データベースを作成できます新規
サーバーに新しいデータベースを作成したり、既存のデータベースを変更または削除したりできます。 以下の手順では、
Management Studio を使用して一般的なデータベース管理タスクを
実行する方法について説明します。 これらのタスクを実行するには、サーバーのセットアップ時に
**マスター** 、サーバー レベル プリンシパル ログインとデータベース
接続している必要があります。

Management Studio でクエリ ウィンドウを開く、データベース フォルダーを開き、展開、 **システム データベース** フォルダーを右クリックし **マスター**, 、クリックして **新しいクエリ**します。

-   使用して、 **CREATE DATABASE** ステートメントを新しいデータベースを作成します。 次に
    詳細についてを参照してください [CREATE DATABASE (SQL データベース)](https://msdn.microsoft.com/library/dn268335.aspx)します。 以下のステートメントがという名前の新しいデータベースを作成 **myTestDB** し、既定の最大サイズが 250 GB の Standard S0 エディションのデータベースがあることを指定します。

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

クリックして **Execute** クエリを実行します。

-   使用して、 **ALTER DATABASE** 既存のデータベースを変更するステートメント
    たとえば、データベースの名前とエディションを変更する場合に
    使用します。 詳細については、次を参照してください。 [ALTER DATABASE (SQL データベース)](https://msdn.microsoft.com/library/ms174269.aspx)します。 この
    以下のステートメントは、前の手順で作成したデータベースの
    エディションを Standard S1 に変更します。

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   使用 **DROP DATABASE** を既存のデータベースを削除するステートメントです。
    詳細については、次を参照してください。 [DROP DATABASE (SQL データベース)](https://msdn.microsoft.com/library/ms178613.aspx)します。 削除の下の文、 **myTestDB** データベースが、次の手順でログインを作成を使用するためにここでは削除されません。

        DROP DATABASE myTestBase;

-   Master データベースには、 **sys.databases** 使用できるビュー
    このビューは、すべてのデータベースに関する詳細を表示するのに使用できます。 既存のデータベースをすべて表示するには、
    次のステートメントを実行します。

        SELECT * FROM sys.databases;

-   SQL データベースで、 **使用** 切り替え用のステートメントはサポートされていません
    データベースを切り替えることはできません。 代わりに、ターゲット データベースに
    直接接続する必要があります。

>[AZURE.NOTE] 作成またはデータベースを変更する TRANSACT-SQL ステートメントの多くは、それ自体のバッチ内で実行する必要があり、他の TRANSACT-SQL ステートメントと共にグループ化することはできません。 詳細については、上記のリンクでステートメント固有の情報を参照してください。

## ログインを作成と管理する

 **マスター** ログインし、どのログインがあるの追跡データベース
アクセス許可があるかがトラッキングされます。 ログインを管理するには、
接続する、 **マスター** サーバー レベルのプリンシパルを持つデータベース
サーバーのセットアップ時に作成したサーバー レベルのプリンシパル ログインを使用 します。
**CREATE LOGIN**, 、**ALTER LOGIN**, 、または **DROP LOGIN** ステートメント
サーバー全体でログインを管理する master データベースに対して
クエリを実行できます。 詳細については、次を参照してください。 [と SQL データベースのログインのデータベースを管理する](http://msdn.microsoft.com/library/azure/ee336235.aspx)です。 


-   使用して、 **CREATE LOGIN** ステートメントを新しいサーバー レベルを作成するには
    使用します。 詳細については、次を参照してください。 [CREATE LOGIN (SQL データベース)](https://msdn.microsoft.com/library/ms189751.aspx)します。 以下のステートメントは、
    呼ばれる **login1**します。 置換 **password1** のパスワードを使用して、
    置き換えてください。

        CREATE LOGIN login1 WITH password='password1';

-   使用して、 **CREATE USER** データベース レベルの付与をステートメント
    使用します。 すべてのログインを作成する必要があります、 **マスター** データベース
    別のデータベースに接続するためのログインに対しては、
    使用してデータベース レベルのアクセス許可を付与する必要があります、 **ユーザーの作成**
    データベース レベルのアクセス許可を付与する必要があります。 詳細については、次を参照してください。 [CREATE USER (SQL データベース)](https://msdn.microsoft.com/library/ms173463.aspx)します。 

-   アクセス許可 login1 を
    データベースへのアクセス許可と呼ばれる **myTestDB**, を次の操作
    実行します。

 1.  表示するオブジェクト エクスプ ローラーを更新する、 **myTestDB** 先ほど作成したデータベースをオブジェクト エクスプ ローラーでサーバー名を右クリックし、をクリックし、 **更新**します。  

     接続を閉じてある場合は、選択して再接続できます **オブジェクト エクスプ ローラーを接続** [ファイル] メニュー。

 2. 右クリック **myTestDB** データベースし、選択 **新しいクエリ**します。

    3.  myTestDB データベースに対して次のステートメントを
        という名前のデータベース ユーザーを作成する **login1User** に対応します。
        サーバー レベル ログイン **login1**します。

            CREATE USER login1User FROM LOGIN login1;

-   使用して、 **sp \_addrolemember** ストアド プロシージャ、ユーザーに付与
    データベースに対する適切なレベルのアクセス許可をユーザー アカウントに付与します。 次に
    詳細についてを参照してください [sp_addrolemember (TRANSACT-SQL)](http://msdn.microsoft.com/library/ms187750.aspx)します。 次のステートメント **login1User**
    追加することによってデータベースへの読み取り専用のアクセス許可 **login1User** に
     **db\_datareader** ロールです。

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   使用して、 **ALTER LOGIN** の既存のログインを変更するステートメント
    ログインのパスワードなど、既存のログインを必要に応じて変更します。 次に
    詳細についてを参照してください [ALTER LOGIN (SQL データベース)](https://msdn.microsoft.com/library/ms189828.aspx)します。  **ALTER LOGIN** に対してステートメントを実行する、 **マスター** データベースです。 そのデータベースに接続されているクエリ ウィンドウに切り替えてください。 

    次のステートメント、 **login1** ログイン パスワードをリセットします。
    置換 **newPassword** 、任意のパスワードを使用して、
    **oldPassword** ログインの現在のパスワードを使用します。

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   使用 **DROP LOGIN** を既存のログインを削除するステートメントです。
    サーバー レベルでログインを削除すると、それに関連付けられている
    データベース ユーザー アカウントも削除されます。 詳細については、
    参照してください [DROP DATABASE (SQL データベース)](https://msdn.microsoft.com/library/ms178613.aspx)します。  **DROP login ステートメント**
    に対してステートメントを実行する、 **マスター** データベースです。 この
    削除の下の文、 **login1** ログインします。

        DROP LOGIN login1;

-   Master データベースには、 **\_logins** ビューがあります
    このビューは、ログインを表示するときに使用します。 既存のログインをすべて表示するには、
    次のステートメントを実行します。

        SELECT * FROM sys.sql_logins;

## 動的管理ビューを使用して SQL データベースを監視する</h2>

SQL データベースでは、個々のデータベースを監視するのに
使用できる動的管理ビューがいくつかサポートされています。 以下に、これらのビューで
取得できるモニター データの種類の例を示します。 次に
すべての種類と詳しい使用例については、「 [SQL Database の監視の動的管理ビューを使用して](https://msdn.microsoft.com/library/azure/ff394114.aspx)します。

-   動的管理ビューをクエリする必要があります **VIEW DATABASE STATE**
    使用します。 付与する、 **VIEW DATABASE STATE** へのアクセス許可、
    特定のデータベースユーザーに付与するには、
    サーバー レベルのプリンシプル ログインで管理するデータベースに接続し、
    データベースに対して次のステートメントを実行します。

        GRANT VIEW DATABASE STATE TO login1User;

-   使用してデータベース サイズの計算、 **sys.dm\_db\_partition\_stats**
    されています。  **Sys.dm\_db\_partition\_stats** ビューには、ページが返されます、。
    データベース内のすべてのパーティションのページと行数情報を返します。
    これらを使ってデータベース サイズを計算できます。 次のクエリはデータベース サイズを
    MB 単位で返します。

        SELECT SUM(reserved_page_count)*8.0/1024
        FROM sys.dm_db_partition_stats;   

-   使用して、 **sys.dm\_exec\_connections** と **sys.dm\_exec\_sessions**
    •データベースに関連付けられている内部タスクと
    現在のユーザー接続に関する情報を取得します。 次のクエリは
    現在の接続に関する情報を返します。

        SELECT
            e.connection_id,
            s.session_id,
            s.login_name,
            s.last_request_end_time,
            s.cpu_time
        FROM
            sys.dm_exec_sessions s
            INNER JOIN sys.dm_exec_connections e
              ON s.session_id = e.session_id;

-   使用して、 **sys.dm\_exec\_query\_stats** 集計を取得するビュー
    キャッシュされたクエリ プランに対する集計パフォーマンス統計を取得します。 次のクエリは
    平均 CPU 時間の上位 5 クエリに関する情報を
    返します。

        SELECT TOP 5 query_stats.query_hash AS "Query Hash",
            SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
            MIN(query_stats.statement_text) AS "Statement Text"
        FROM
            (SELECT QS.*,
            SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
                    - QS.statement_start_offset)/2) + 1) AS statement_text
             FROM sys.dm_exec_query_stats AS QS
             CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
        GROUP BY query_stats.query_hash
        ORDER BY 2 DESC;

