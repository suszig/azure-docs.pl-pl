<properties
    pageTitle="一過性の接続障害から復旧するための対策 | Microsoft Azure"
    description="接続エラーなど、Azure SQL Database とやり取りする際に発生する一過性の障害をトラブルシューティング、診断、防止するための対策。"
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="msmets"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="12/10/2015"
    ms.author="daleche"/>


# SQL Database とのやり取りで生じた一過性の障害と接続エラーのトラブルシューティング


このトピックでは、クライアント プログラムが Azure SQL Database とやり取りする際に発生する接続エラーと一過性の障害を防止、トラブルシューティング、診断、軽減する方法について説明します。


<a id="i-transient-faults" name="i-transient-faults"></a>

## 一過性の障害


一過性の障害とは、根本的な原因が短時間で自然に解決するエラーです。 一過性の障害を起こす偶発的原因として、Azure システムが、各種ワークロードの負荷分散を行うために行うハードウェア リソースの瞬間的切り替えがあります。 この再構成の進行中、Azure SQL Database への接続が失われる可能性があります。


クライアント プログラムで ADO.NET を使用している場合、プログラムはについて述べることが一時的な障害のスローによって、 **SqlException**します。  **数** プロパティは、トピックの先頭付近にある一時的なエラーの一覧と比較できます。
[SQL Database クライアント プログラムのエラー メッセージ](sql-database-develop-error-messages.md)します。


### 接続とコマンド


接続の試行中に一過性のエラーが発生した場合、数秒待って接続を再試行する必要があります。


SQL クエリ コマンドの実行中に一過性のエラーが発生した場合、そのコマンドをすぐに再試行することは避けてください。 ある程度の時間差を伴って接続が新たに確立されます。 その後でコマンドを再試行してください。


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

## 一過性の障害のための再試行ロジック


一過性の障害が多少なりとも生じるクライアント プログラムは、再試行ロジックを組み込むことで堅牢性を高めることができます。


Azure SQL Database との通信にサード パーティのミドルウェアを使用する場合、一過性の障害に対する再試行ロジックがそのミドルウェアに備わっているかどうかをベンダーに確認してください。


### 再試行の原則


- エラーが一過性の障害である場合、再度、接続を開いてみる。


- 一過性の障害で失敗した SQL SELECT ステートメントをそのまま再試行することはしない。
 - 新しい接続を確立してから、SELECT を再試行してください。


- SQL UPDATE ステートメントが一過性の障害で失敗した場合、新たに接続を確立したうえで UPDATE を再試行する。
 - データベース トランザクション全体が完了するかトランザクション全体がロールバックされたことを再試行ロジックで確認する必要があります。


#### 再試行に関するその他の注意点


- 業務終了時刻に自動的に起動されて翌朝にかけて完了するようなバッチ プログラムは、再試行間隔をある程度長めにしても問題ありません。


- 人間の心理として、待ち時間があまり長いと途中でキャンセルされる可能性があります。ユーザー インターフェイス プログラムはその点を考慮するようにしてください。
 - ただし、数秒おきに再試行するようなやり方は避けてください。そのような方法を用いると、大量の要求でシステムが処理しきれなくなる可能性があります。


### 再試行の間隔を長くする



最初に再試行する前に、5 秒間待つことをお勧めします。 5 秒未満で再試行すると、クラウド サービスに過度の負荷がかかるおそれがあります。 再試行するたびに、待ち時間を比例して、最大 60 秒まで長くする必要があります。

詳細については、 *ブロック期間* ADO.NET を使用しているクライアントで使用できる、 [SQL サーバー接続プール (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)します。

加えて、最大再試行回数を設定し、プログラムが自動的に終了するように配慮する必要があります。


### 再試行ロジックを含んだコード サンプル


さまざまなプログラミング言語での再試行ロジックが使われているコード サンプルは次のリンクからアクセスできます。

- [クイック スタート コード サンプル](sql-database-develop-quick-start-client-code-samples.md)


<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

## 再試行ロジックのテスト


再試行ロジックをテストするには、プログラムの実行中に修復可能なエラーをシミュレートする (人為的に発生させる) 必要があります。


### ネットワークから切断することによるテスト


再試行ロジックをテストする手段として、プログラムの実行中にクライアント コンピューターをネットワークから切断する方法が挙げられます。 エラーになります。
- **SqlException.Number** 11001 =
- メッセージ:「そのようなホストは不明です」


最初の再試行のときに、プログラムでスペルミスを修正してから接続を試みてください。


具体的には、コンピューターをネットワークから切断した後でプログラムを起動します。 プログラムでは、その結果、プログラムを実行時のパラメーターが認識されます。
1. エラーのリストに対して一時的に 11001 を追加し、一過性と見なします。
2. 通常と同様に初回接続を試みます。
3. エラーが捕捉された後、11001 をリストから削除します。
4. コンピューターをネットワークに接続するユーザーを示すメッセージが表示されます。
 - いずれかを使用してさらに実行を一時停止、 **Console.ReadLine** メソッドまたは [ok] ボタンを含むダイアログ。 コンピューターをネットワークに接続した後、ユーザーが Enter キーを押します。
5. 再度接続を試みます。


### 接続時に間違った綴りのデータベース名を使用することによるテスト


意図的に間違ったユーザー名を使って初回接続を試みます。 エラーになります。
- **SqlException.Number** 18456 を =
- メッセージ:"Login ユーザー 'WRONG_MyUserName' に失敗しました。"


最初の再試行のときに、プログラムでスペルミスを修正してから接続を試みてください。


実用的にするには、プログラムがその結果、プログラムを実行時のパラメーターを認識して可能性があります。
1. エラーのリストに対して一時的に 18456 を追加し、一過性と見なします。
2. 意図的に 'WRONG_' をユーザー名に追加します。
3. エラーが捕捉された後、18456 をリストから削除します。
4. ユーザー名から 'WRONG_' を削除します。
5. 再度接続を試みます。


<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## 接続: 接続文字列


Azure SQL Database に接続するために必要な接続文字列は、Microsoft SQL Server に接続するための文字列とは少し異なります。 データベースの接続文字列をコピーすることができます、 [Azure ポータル](http://portal.azure.com/)します。


[AZURE.INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]



### 接続再試行用の .NET SqlConnection パラメーター


クライアント プログラムが接続する場合に Azure SQL Database に .NET Framework クラスを使用して **System.Data.SqlClient.SqlConnection**, 、.NET 4.5.1 を使用する必要がありますまたはそれ以降は、その接続の再試行機能を活用できるようにします。 この機能の詳細については [ここ](http://go.microsoft.com/fwlink/?linkid=393996)します。


<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


ビルドするときに、 [接続文字列](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) の **SqlConnection** オブジェクトに、次のパラメーターの間で値を調整する必要があります。

- ConnectRetryCount &nbsp;&nbsp;*(既定値は 0 です。範囲は 0 ～ 255 です)*
- ConnectRetryInterval &nbsp;&nbsp;*(既定値は 1 秒です。範囲は 1 ～ 60 です)*
- Connection Timeout &nbsp;&nbsp;*(既定値は 15 秒です。範囲は 0 ～ 2147483647 です)*


具体的には、選択した値で次の等式が成り立つ必要があります。

- Connection Timeout = ConnectRetryCount * ConnectionRetryInterval

たとえば、回数が 3、間隔が 10 秒、タイムアウトが 29 秒のみの場合、29 < 3 * 10 となり、接続時に 3 回目と最後の試行には十分な時間が与えらません。


#### 接続とコマンド


 **ConnectRetryCount** と **ConnectRetryInterval** パラメーターにより、 **SqlConnection** オブジェクトは、通知またはプログラムに制御を返すようなプログラムを発行しないせず接続操作を再試行してください。 再試行は次の状況で発生することがあります。

- mySqlConnection.Open メソッドの呼び出し
- mySqlConnection.Execute メソッドの呼び出し

これには注意が必要です。 一時的な障害が発生した場合、 *クエリ* が実行されている、 **SqlConnection** オブジェクトは接続操作を再試行していないと、確実には、クエリを再試行しません。 ただし、 **SqlConnection** 非常に高速に実行されるクエリを送信する前に接続を確認します。 簡単なチェックが接続の問題を検出した場合 **SqlConnection** 接続操作を再試行します。 再試行に成功すると、実行するクエリが送信されます。


#### ConnectRetryCount をアプリケーションの再試行ロジックと組み合わせて使用する必要があるかどうか

アプリケーションにカスタムの堅牢な再試行ロジックが組み込まれていると仮定します。 このロジックでは、接続操作が 4 回再試行されます。 追加する場合 **ConnectRetryInterval** と **ConnectRetryCount** = 3 を接続文字列は 4 * 3 = 12 に再試行回数が増加されます再試行します。 このように何回も再試行するのは、適切ではない可能性があります。



<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

## 接続: IP アドレス


SQL Database サーバーは、クライアント プログラムのホストとなるコンピューターの IP アドレスからの通信を許可するように構成する必要があります。 ファイアウォールの設定を編集することによって、これを行う、 [Azure ポータル](http://portal.azure.com/)します。


IP アドレスの構成を怠った場合、必要な IP アドレスを示した親切なエラー メッセージがプログラムで表示されます。


[AZURE.INCLUDE [sql-database-include-ip-address-22-v12portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]


詳細については、次を参照してください。
[方法: SQL データベースのファイアウォール設定の構成](sql-database-configure-firewall-settings.md)


<a id="c-connection-ports" name="c-connection-ports"></a>

## 接続: ポート


通常、必要な設定は、クライアント プログラムのホストとなるコンピューターのポート 1433 の送信方向を開放するだけです。


たとえば、クライアント プログラムが Windows コンピューターでホストされている場合、そのホストの Windows ファイアウォールでポート 1433 を開放することができます。


1. コントロール パネルを開く
2. &gt; [すべてのコントロール パネル項目]
3. &gt; [Windows ファイアウォール]
4. &gt; [詳細設定]
5. &gt; [送信の規則]
6. &gt; [操作]
7. &gt; [新しい規則]


かどうかは、Azure の仮想マシン (VM) で、クライアント プログラムがホストされている、お読みください:<br/>[1433 for ADO.NET 4.5 と SQL Database V12 における 1433年以外のポート](sql-database-develop-direct-route-ports-adonet-v12.md)します。


ポートと IP アドレスの構成に関する背景情報を参照してください。
[Azure SQL Database ファイアウォール](sql-database-firewall-configure.md)


<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

## 接続: ADO.NET 4.5


プログラムのような ADO.NET のクラスを使用している場合 **System.Data.SqlClient.SqlConnection** を Azure SQL Database に接続することをお勧め .NET Framework 4.5 以降のバージョンを使用することです。


ADO.NET 4.5。
- サポート TDS 7.4 プロトコルを追加します。 これには、4.0 の機能以外の接続の強化が含まれます。
- 接続プールをサポートします。 加えて、プログラムに割り当てた接続オブジェクトが正常に動作しているかどうかを効率的に検証することが可能です。


接続プールから取得した接続オブジェクトを使用するとき、すぐに使用しないのであれば、プログラムで一時的に接続を閉じるようお勧めします。 接続を再度開いたとしても、新しい接続の作成に伴う処理負荷はわずかです。


最新の ADO.NET にアップグレードする ADO.NET 4.0 を使用しているかを前の手順をお勧めします。
- 第 2015 年 7 月時点ですることができます [ADO.NET 4.6 をダウンロード](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx)します。


<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## 診断: ユーティリティから接続できるかどうかをテストする


プログラムから Azure SQL Database に接続できないときの診断方法として 1 つ考えられるのは、ユーティリティ プログラムを使用して接続する方法です。 診断対象のプログラムと同じライブラリを使用して接続するユーティリティがあれば理想的です。


任意の Windows コンピューターでは、これらのユーティリティを実行できます。
- SQL Server Management Studio (ssms.exe) に対して、ADO.NET を使用して接続します。
- 使用して接続する sqlcmd.exe [ODBC](http://msdn.microsoft.com/library/jj730308.aspx)します。


接続後、短い SQL SELECT クエリが正しく動作するかどうかをテストしてください。


<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

## 診断: 開放ポートを確認する


ポートの問題から接続に失敗している可能性があるとします。 ポートの構成に関するレポート作成に対応したユーティリティをご使用のコンピューターから実行してください。


Linux で次のユーティリティは便利になります。
- `netstat -nap`
- `nmap -sS -O 127.0.0.1`
 - (IP アドレスを指定する値の例を変更します)。


Windows では、 [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) ユーティリティが役立つ可能性があります。 以下の例では、Azure SQL Database サーバー上のポートの状況と、ノート PC 上で動作しているポートとを照会しています。


```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

## 診断: エラーのログを記録する


断続的な問題は、過去数日から数週間にわたる一般的なパターンを検出することによって診断できる場合が多々あります。


診断には、クライアントで発生したエラーのログが役立ちます。 Azure SQL Database が内部的に記録するエラー データとそれらのログ エントリを相互に関連付けることも可能です。


Enterprise Library 6 (EntLib60) には、ログ記録を支援する .NET マネージ クラスが用意されています。
- [5-ログ記録と同じくらい簡単: ログ記録アプリケーション ブロックを使用します。](http://msdn.microsoft.com/library/dn440731.aspx)


<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

## 診断: エラーの発生をシステム ログで調べる


以下に示したのは、エラーや各種情報のログを照会する Transact-SQL SELECT ステートメントの例です。


| ログのクエリ | 説明 |
| :-- | :-- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |  [Sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) ビューが一時的な障害や接続障害が発生するものも含めて、個々 のイベントに関する情報を提供しています<br/><br/>。理想的に関連付けることができます、 **start_time** または **end_time** クライアント プログラムに問題が発生した場合についての情報の値<br/><br/>**。ヒント:** に接続する必要があります、 **マスター** これを実行するデータベースです。 |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |  [Sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) ビューには、追加の診断用のイベントの種類の集計数が提供しています<br/><br/>**。ヒント:** に接続する必要があります、 **マスター** これを実行するデータベースです。 |


### 診断: SQL Database のログから問題のイベントを検索する


Azure SQL Database のログで問題のイベントに関するエントリを検索することができます。 内に、次の TRANSACT-SQL SELECT ステートメントを実行してください、 **マスター** データベース。


```
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```


#### sys.fn_xe_telemetry_blob_target_read_file から返される行の例


次に示したのは、クエリから返される行の例です。 ここに示した行では null 値が表示されていますが、null 以外の場合も多くあります。


```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## Enterprise Library 6


Enterprise Library 6 (EntLib60) は、.NET クラスのフレームワークです。クラウド サービス (Azure SQL Database サービスもその一つ) に対する堅牢なクライアントをこのフレームワークを使って実装することができます。 最初にアクセスして、EntLib60 が利用できる各領域に対する専用のトピックを見つけることができます。
- [エンタープライズ ライブラリ 6 – 2013年 4 月](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)


一時的な障害を処理するための再試行ロジックは、EntLib60 が利用できる 1 つの領域を示します。
- [4-忍耐力、すべての成功の秘訣: 一時的な障害処理アプリケーション ブロックを使用します。](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)


短い c# コードを使用するサンプル EntLib60、再試行ロジック内から入手できます。
- [コード サンプル: SQL データベースに接続するための c# での Enterprise Library 6 からロジックを再試行してください。](sql-database-develop-entlib-csharp-retry-windows.md)


> [AZURE.NOTE] EntLib60 のソース コードは使用可能なパブリック [ダウンロード](http://go.microsoft.com/fwlink/p/?LinkID=290898)します。 EntLib に対して機能の追加や保守目的での更新を行う予定はありません。


### 一過性の障害と再試行に関連した EntLib60 のクラス


再試行ロジックで特に利用する機会の多い EntLib60 のクラスは次のとおりです。 これらはすべて、または名前空間でさらに **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

*名前空間に **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*

- **RetryPolicy** クラス
 - **ExecuteAction** メソッド


- **ExponentialBackoff** クラス


- **SqlDatabaseTransientErrorDetectionStrategy** クラス


- **ReliableSqlConnection** クラス
 - **ExecuteCommand** メソッド


名前空間に **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

- **AlwaysTransientErrorDetectionStrategy** クラス

- **NeverTransientErrorDetectionStrategy** クラス


EntLib60 に関する情報は以下のリンクから入手できます。

- 無料 [ダウンロードを予約します Microsoft Enterprise Library、第 2 版の開発者向けガイド。](http://www.microsoft.com/download/details.aspx?id=41145)

- ベスト プラクティス: [一般的なガイダンスを再試行](best-practices-retry-general.md) が再試行ロジックの優れたについて詳しく説明します。

- ダウンロードを NuGet [エンタープライズ ライブラリ - 一時的な障害処理アプリケーション ブロック 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)


### EntLib60: Logging ブロック


- Logging ブロックは、きわめて柔軟性に優れた構成可能なソリューションです。
 - ログ メッセージをさまざまな場所に作成して保存する。
 - メッセージを分類したりフィルタリングしたりする。
 - デバッグやトレース、監査要件、全般的なログ要件に利用できるコンテキスト情報を収集する。


- Logging ブロックは、ログ出力先が備えるログ機能を抽象化したものです。対象となるログ ストアの場所や種類に関係なく、アプリケーション コードの一貫性を確保することができます。


詳細は、以下を参照してください。
[5-ログ記録と同じくらい簡単: ログ記録アプリケーション ブロックを使用します。](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)


### EntLib60 IsTransient メソッドのソース コード


次から、 **SqlDatabaseTransientErrorDetectionStrategy** クラスでの c# ソース コードを **IsTransient** メソッドです。 どのようなエラーが一過性で、再試行する価値があるかは、このソース コードを見るとはっきりわかります (2013 年 4 月時点)。

さまざまな **//comment** 読みやすさを強調するためにこのコピーからの行が削除されました。


```
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```


## 詳細情報


- [SQL Server の接続プーリング (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx)


- [*再試行* 汎用の再試行で記述されたライブラリは Apache 2.0 ライセンスは、 **Python**, 、あらゆるものに再試行動作を追加するタスクを簡略化します。](https://pypi.python.org/pypi/retrying)

