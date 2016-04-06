<properties
   pageTitle="再試行サービス固有のガイダンス | Microsoft Azure"
   description="再試行メカニズムを設定するためのサービス固有のガイダンスです。"
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="masimms"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2015"
   ms.author="masashin"/>

# 再試行サービス固有のガイダンス

![](media/best-practices-retry-service-specific/pnp-logo.png)

## 概要

ほとんどの Azure サービスとクライアント SDK には、再試行メカニズムが組み込まれています。 しかし、再試行メカニズムは、サービスごとにさまざまな特性や要件があるため一定ではなく、それぞれの再試行メカニズムは特定のサービスに合わせて調整されます。 このガイドでは、主要な Azure サービスの再試行メカニズム機能の概要と、そのサービスに合わせて再試行メカニズムを使用、適合、または拡張するために役立つ情報を記載しています。

一時的な障害を処理およびサービスとリソースに対する接続と操作の再試行に関する一般的なガイダンスは、次を参照してください。 [再試行ガイダンス](best-practices-retry-general.md)します。

次の表は、このガイダンスで説明されている Azure サービスの再試行機能をまとめています。

| **サービス**                           | **再試行機能**                  | **ポリシーの構成**     | **Scope**                                        | **テレメトリ機能** |
|---------------------------------------|-----------------------------------------|------------------------------|--------------------------------------------------|------------------------
| **[AzureStorage](#azure-storage-retry-guidelines)**                      | クライアントでネイティブ                        | プログラムによる                 | クライアントと個々の操作                 | TraceSource            |
| **[Entity Framework による SQL Database](#sql-database-using-entity-framework-6-retry-guidelines)** | クライアントでネイティブ                        | プログラムによる                 | AppDomain ごとにグローバル                             | なし                   |
| **[ADO.NET を使用した SQL データベース](#sql-database-using-ado-net-retry-guidelines)**         | Topaz*                                  | 宣言型およびプログラムによる | 1 つのステートメントまたはコードのブロック              | カスタム                 |
| **[Service Bus](#service-bus-retry-guidelines)**                       | クライアントでネイティブ                        | プログラムによる                 | 名前空間マネージャー、メッセージング ファクトリ、およびクライアント | ETW                    |
| **[キャッシュ](#cache-redis-retry-guidelines)**                             | クライアントでネイティブ                        | プログラムによる                 | クライアント                                           | TextWriter             |
| **[DocumentDB](#documentdb-pre-release-retry-guidelines)**                        | サービスでネイティブ                       | 構成不可             | グローバル                                           | TraceSource            |
| **[Search](#search-retry-guidelines)**                            | Topaz* (カスタム検出戦略を実装) | 宣言型およびプログラムによる | コードのブロック                                   | カスタム                 |
| **[Active Directory](#azure-active-directory-retry-guidelines)**                  | Topaz* (カスタム検出戦略を実装) | 宣言型およびプログラムによる | コードのブロック                                   | カスタム                 |
*Topaz: <a href="http://msdn.microsoft.com/library/dn440719.aspx">エンタープライズ ライブラリ 6.0</a> に含まれる一時的エラー処理アプリケーション ブロックの表示名。 このガイダンスで説明されているとおり、ほとんどの種類のサービスに対して、カスタム検出戦略を Topaz と一緒に使用できます。 Topaz の既定の戦略のセクションで示した [一時的なエラー処理アプリケーション ブロック (Topaz) 戦略](#transient-fault-handling-application-block-topaz-strategies) このガイダンスの末尾にします。 このブロック (Topaz) は、現在のところオープン ソース フレームワークであり、Microsoft により直接サポートされていないことに注意してください。

> [AZURE.NOTE] ほとんどの Azure の組み込み再試行メカニズムは現在方法には、さまざまな種類のエラーに対して異なる再試行ポリシー適用なしまたは機能以外にも例外が再試行ポリシーに含まれます。 したがって、この記事の執筆時点で選択できる最良のガイダンスは、最適な平均的パフォーマンスと可用性を提供するポリシーを構成することです。 ポリシーを微調整する 1 つの方法は、ログ ファイルを分析して、発生する一時的エラーの種類を判別することです。 たとえば、エラーの多くがネットワーク接続問題に関連している場合、最初の再試行を長時間待つのではなく、すぐに再試行することができます。

## Microsoft Azure Storage の再試行ガイドライン

Microsoft Azure Storage サービスには、テーブル、Blob Storage、ファイル、およびストレージ キューが含まれています。

### 再試行メカニズム

再試行は、個々の REST 操作レベルで実行され、クライアント API 実装の不可欠な部分です。 SDK を使用してクライアントのストレージを実装するクラス、 [IExtendedRetryPolicy インターフェイス](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.aspx)します。

このインターフェイスにはさまざまな実装があります。 ストレージ クライアントは、ポリシーを、テーブル、BLOB、およびキューにアクセスするために特別に設計されたものの中から選択できます。 各実装は、基本的に、再試行間隔や他の詳細を定義する、それぞれに異なる再試行戦略を使用します。

組み込みクラスは、Linear (一定遅延) と、ランダム化された再試行間隔が指定される Exponential をサポートします。 別のプロセスがより高いレベルで再試行を処理している場合に使用する、再試行なしポリシーもあります。 ただし、組み込みクラスによって提供されていない特定の要件がある場合は、独自の再試行クラスを実装できます。

代替再試行では、読み取りアクセス geo 冗長ストレージ (RA-GRS) を使用しており、要求の結果が再試行可能エラーになる場合は、プライマリとセカンダリのストレージ サービス場所での切り替えが行われます。 参照してください [Azure Storage 冗長オプション](http://msdn.microsoft.com/library/azure/dn727290.aspx) の詳細。

### ポリシーの構成 (Microsoft Azure Storage)

再試行ポリシーは、プログラムにより構成されます。 一般的なプロシージャが作成および設定は、 **TableRequestOptions**, 、**BlobRequestOptions**, 、**FileRequestOptions**, 、または **QueueRequestOptions** インスタンス。

```csharp
TableRequestOptions interactiveRequestOption = new TableRequestOptions()
{
  RetryPolicy = new LinearRetry(TimeSpan.FromMilliseconds(500), 3),
  // For Read-access geo-redundant storage, use PrimaryThenSecondary.
  // Otherwise set this to PrimaryOnly.
  LocationMode = LocationMode.PrimaryThenSecondary,
  // Maximum execution time based on the business use case. Maximum value up to 10 seconds.
  MaximumExecutionTime = TimeSpan.FromSeconds(2)
};
```

要求オプション インスタンスは、クライアントに対して設定することができ、そのクライアントからのすべての操作は、指定された要求オプションを使用します。

```csharp
client.DefaultRequestOptions = interactiveRequestOption;
var stats = await client.GetServiceStatsAsync();
```

クライアント要求オプションは、要求オプション クラスの設定済みインスタンスを操作メソッドのパラメーターとして渡すことによって、オーバーライドできます。

```csharp
var stats = await client.GetServiceStatsAsync(interactiveRequestOption, operationContext: null);
```

使用する、 **OperationContext** インスタンス、操作が完了したときと、再試行が行われたときに実行するコードを指定します。 このコードは、ログとテレメトリで使用する、操作に関する情報を収集できます。

    // Set up notifications for an operation
    var context = new OperationContext();
    context.ClientRequestID = "some request id";
    context.Retrying += (sender, args) =>
    {
      /* Collect retry information */
    };
    context.RequestCompleted += (sender, args) =>
    {
      /* Collect operation completion information */
    };
    var stats = await client.GetServiceStatsAsync(null, context);

拡張再試行ポリシーを返すというエラーが再試行に適しているかどうかを示す、だけでなく、 **RetryContext** 再試行回数、前回の要求の結果の数をプライマリまたはセカンダリの場所に、次の再試行が行われるかどうかを示すオブジェクトです (詳細については、次の表を参照してください)。 プロパティ、 **RetryContext** オブジェクトを使用して、決定の場合で、再試行を行う。 詳細については、次を参照してください。 [IExtendedRetryPolicy.Evaluate メソッド](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.evaluate.aspx)します。

次の表は、組み込み再試行ポリシーの既定の設定を示しています。

| **Context**              | **設定**                                                 | **既定値**                  | **意味**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|--------------------------|-------------------------------------------------------------|------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| テーブル/Blob/ファイル<br />QueueRequestOptions | MaximumExecutionTime<br /><br />ServerTimeout<br /><br /><br /><br /><br />LocationMode<br /><br /><br /><br /><br /><br /><br />RetryPolicy | 120 秒<br /><br />なし<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br /><br />ExponentialPolicy | 要求の最大実行時間 (考えられるすべての再試行が含まれます)。<br />要求に対するサーバー タイムアウト間隔 (値は秒単位に丸められます)。 指定しない場合、サーバーに対するすべての要求に既定値が使用されます。 通常、最善のオプションは、サーバーの既定値が使用されるように、この設定を省略するです。<br />ストレージ アカウントは作成時の読み取りアクセス geo 冗長ストレージ (RA-GRS) のレプリケーション オプション場合は、要求を受け取る場所を示すために場所モードを使用できます。 たとえば場合、 **PrimaryThenSecondary** を指定すると、要求は常に、プライマリの場所に送信されます。 要求が失敗した場合は、2 次拠点へ送信されます。<br />各オプションの詳細については、以下を参照してください。 |
| Exponential ポリシー                      | maxAttempt<br />deltaBackoff<br /><br /><br />MinBackoff<br /><br />MaxBackoff               | 3<br />4 秒<br /><br /><br />3 秒<br /><br />30 秒   | 再試行回数。<br />再試行のバックオフ間隔。 ランダムに要素を含む、この期間の倍数は、その後の再試行の試行中に使用されます。<br />deltaBackoff から計算されたすべての再試行間隔に追加されます。 この値は変更できません。<br />MaxBackoff は、計算された再試行間隔が MaxBackoff より大きい場合に使用されます。 この値は変更できません。                                                                                                                                                                                                                                                                                                                                                                       |
| Linear ポリシー                           | maxAttempt<br />deltaBackoff                                     | 3<br />30 秒                                 | 再試行回数。<br />再試行のバックオフ間隔。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |

### 再試行使用のガイダンス
ストレージ クライアント API を使用して Microsoft Azure Storage サービスにアクセスする場合は、次のガイドラインを検討します。

* Microsoft.WindowsAzure.Storage.RetryPolicies 名前空間からの組み込み再試行ポリシーを使用します (これらのポリシーが要件に適している場合)。 ほとんどの場合、これらのポリシーで十分対応できます。
* 使用して、 **ExponentialRetry** バッチ操作、バック グラウンド タスク、または非対話型のシナリオでのポリシー。 これらのシナリオでは、一般に、サービスを復旧するためにより多くの時間を確保できます。結果として、操作は最終的に成功する可能性が高くなります。
* 指定することを検討してください、 **MaximumExecutionTime** のプロパティ、 **RequestOptions** 合計実行時間を制限するタイムアウト値を選択する場合、型と操作のサイズを考慮パラメーター。
* カスタム再試行を実装する必要がある場合は、ストレージ クライアント クラスを囲むラッパーは作成しないでください。 代わりに、機能を使用して、既存のポリシーの拡張、 **IExtendedRetryPolicy** インターフェイスです。
* 読み取りアクセス geo 冗長ストレージ (RA-GRS) を使用している場合は、使用、 **LocationMode** ストアの読み取り専用のコピーを再試行回数が、セカンダリにアクセスするかを指定するには、主要なアクセスが失敗する必要があります。 ただし、このオプションを使用するときは、プライマリ ストアからのレプリケーションがまだ完了していない場合に、古くなった可能性があるデータを用いてアプリケーションが正常に動作できることを確認する必要があります。

再試行操作について次の設定から始めることを検討します。 これらは汎用の設定であり、操作を監視して、独自のシナリオに合うように値を微調整する必要があります。  

| **Context**          | **サンプルのターゲット E2E<br />最大待機時間** | **再試行ポリシー** | **[設定]**            | **値**  | **動作のしくみ**                                                            |
|----------------------|-----------------------------------|------------------|-------------------------|-------------|-----------------------------------------------------------------------------|
| 対話型、UI、<br />またはフォア グラウンド | 2 秒                         | Linear           | maxAttempt<br />deltaBackoff | 3<br />500 ミリ秒    | 試行 1-500 ミリ秒の遅延<br />試行 2-500 ミリ秒の遅延<br />試行 3-500 ミリ秒の遅延  |
| 背景<br />またはバッチ            | 30 秒                        | Exponential      | maxAttempt<br />deltaBackoff | 5<br />4 秒 | 試行 1 - 最大 3 秒の遅延<br />試行 2 - 最大 7 秒の遅延<br />試行 3 - 最大 15 秒の遅延 |

## テレメトリ

再試行回数がログに記録する、 **TraceSource**します。 構成する必要があります、 **TraceListener** 、イベントをキャプチャし、適切な宛先ログに書き込んだりできます。 使用することができます、 **TextWriterTraceListener** または **XmlWriterTraceListener** ログ ファイルにデータを書き込む、 **EventLogTraceListener** Windows イベント ログに書き込む、または **EventProviderTraceListener** トレース データを ETW サブシステムに書き込むことです。 バッファーの自動フラッシュと、ログに記録するイベントの詳細度 (たとえば、エラー、警告、情報、および冗長など) を構成することもできます。 詳細については、次を参照してください。 [クライアント側 .NET ストレージ クライアント ライブラリによるログ](http://msdn.microsoft.com/library/azure/dn782839.aspx)します。

操作が受け取ることができます、 **OperationContext** インスタンスを公開、 **"再試行中"** カスタム テレメトリ ロジックをアタッチするために使用するイベントです。 詳細については、次を参照してください。 [OperationContext.Retrying イベント](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.operationcontext.retrying.aspx)です。

## 例 (Microsoft Azure Storage)

次のコード例は、2 つ作成する方法を示しています。 **TableRequestOptions** 異なる再試行設定でのインスタンスは対話型の要求の 1 つ、のいずれかのバック グラウンドの要求。 次にこの例では、これら 2 つの再試行ポリシーをクライアントに対して設定して、それらのポリシーがすべての要求に適用されるようにします。さらに、対話型戦略を特定の要求に対して設定して、クライアントに適用された既定の設定をオーバーライドできるようにします。

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.RetryPolicies;
using Microsoft.WindowsAzure.Storage.Table;

namespace RetryCodeSamples
{
    class AzureStorageCodeSamples
    {
        private const string connectionString = "UseDevelopmentStorage=true";

        public async static Task Samples()
        {
            var storageAccount = CloudStorageAccount.Parse(connectionString);

            TableRequestOptions interactiveRequestOption = new TableRequestOptions()
            {
                RetryPolicy = new LinearRetry(TimeSpan.FromMilliseconds(500), 3),
                // For Read-access geo-redundant storage, use PrimaryThenSecondary.
                // Otherwise set this to PrimaryOnly.
                LocationMode = LocationMode.PrimaryThenSecondary,
                // Maximum execution time based on the business use case. Maximum value up to 10 seconds.
                MaximumExecutionTime = TimeSpan.FromSeconds(2)
            };

            TableRequestOptions backgroundRequestOption = new TableRequestOptions()
            {
                // Client has a default exponential retry policy with 4 sec delay and 3 retry attempts
                // Retry delays will be approximately 3 sec, 7 sec, and 15 sec
                MaximumExecutionTime = TimeSpan.FromSeconds(30),
                // PrimaryThenSecondary in case of Read-access geo-redundant storage, else set this to PrimaryOnly
                LocationMode = LocationMode.PrimaryThenSecondary
            };

            var client = storageAccount.CreateCloudTableClient();
            // Client has a default exponential retry policy with 4 sec delay and 3 retry attempts
            // Retry delays will be approximately 3 sec, 7 sec, and 15 sec
            // ServerTimeout and MaximumExecutionTime are not set

            {
                // Set properties for the client (used on all requests unless overridden)
                // Different exponential policy parameters for background scenarios
                client.DefaultRequestOptions = backgroundRequestOption;
                // Linear policy for interactive scenarios
                client.DefaultRequestOptions = interactiveRequestOption;
            }

            {
                // set properties for a specific request
                var stats = await client.GetServiceStatsAsync(interactiveRequestOption, operationContext: null);
            }

            {
                // Set up notifications for an operation
                var context = new OperationContext();
                context.ClientRequestID = "some request id";
                context.Retrying += (sender, args) =>
                {
                    /* Collect retry information */
                };
                context.RequestCompleted += (sender, args) =>
                {
                    /* Collect operation completion information */
                };
                var stats = await client.GetServiceStatsAsync(null, context);
            }
        }
    }
}
```

## 詳細情報

- [Azure Storage Client Library Retry Policy Recommendations (Microsoft Azure Storage クライアント ライブラリの再試行ポリシーに対する推奨事項)](http://azure.microsoft.com/blog/2014/05/22/azure-storage-client-library-retry-policy-recommendations/)
- [Storage Client Library 2.0 – Implementing Retry Policies (Storage Client Library 2.0 – 再試行ポリシーの実装)](http://gauravmantri.com/2012/12/30/storage-client-library-2-0-implementing-retry-policies/)

## Entity Framework 6 を使用して SQL Database にアクセスする場合の再試行ガイドライン

SQL Database は、多様なサイズで利用できるホステッド SQL データベースです。これは Standard (共有) サービスと Premium (非共有) サービスのどちらとしてでも使用できます。 Entity Framework は、.NET 開発者がドメイン固有オブジェクトを使用してリレーショナル データを操作できるようにする、オブジェクトリレーショナル マッパーです。 これにより、開発者が通常は記述する必要のあるデータアクセス コードの大部分が不要になります。

## 再試行メカニズム

Entity Framework 6.0 を使用して SQL データベースにアクセスするとき、再試行サポートは提供されておりと呼ばれるメカニズムを通じて高い [接続回復/再試行ロジック](http://msdn.microsoft.com/data/dn456835.aspx)します。 仕様全体はで使用できる、 [.NET Entity Framework wiki](https://entityframework.codeplex.com/wikipage?title=Connection%20Resiliency%20Spec) codeplex します。 再試行メカニズムの主な機能は、次のとおりです。

* 主要な抽象化は、 **IDbExecutionStrategy** インターフェイスです。 このインターフェイスは、次の事柄を実行します。
  * 同期および非同期の定義 **Execute*** メソッドです。
  * 既定の戦略として、直接使用できるクラス、またはデータベース コンテキストに基づいて構成できるクラスを定義します。このクラスは、プロバイダー名にマップされるかまたはプロバイダー名とサーバー名にマップされます。 コンテキストに基づいて構成された場合、再試行は個々のデータベース操作のレベルで行われます。特定の 1 コンテキスト操作に対して複数の再試行が行われる場合もあります。
  * 失敗した接続をいつ、どのように再試行するかを定義します。
* いくつかの組み込み実装が含まれています、 **IDbExecutionStrategy** インターフェイス。
  * 既定値 - 再試行なし。
  * SQL Database の既定値 (自動) - 再試行なし。ただし例外を検査し、SQL Database 戦略を使用するという提案でそれらをラップします。
  * SQL Database の既定値 - Exponential (基本クラスからの継承) に、SQL Database の検出ロジックを加えたもの。
* ランダム化を含む指数バックオフ戦略を実装します。
* 組み込み再試行クラスは、ステートフルですが、スレッド セーフではありません。 ただし、このクラスは現在の操作が完了した後に再利用できます。
* 指定した再試行回数を超えた場合、結果は新しい例外でラップされます。 これは現在の例外をバブルアップしません。

## ポリシーの構成 (Entity Framework 6 と SQL Database の組み合わせ)

再試行サポートは、Entity Framework 6.0 以降を使用して SQL Database にアクセスする場合に提供されます。 再試行ポリシーは、プログラムにより構成されます。 構成を操作ごとに変更することはできません。

コンテキストに基づいて既定値として戦略を構成する場合は、新しい戦略をオンデマンドで作成する機能を指定します。 次のコードは、再試行構成クラスを拡張するを作成する方法、 **DbConfiguration** 基本クラスです。

```csharp
public class BloggingContextConfiguration : DbConfiguration
{
  public BlogConfiguration()
  {
    // Set up the execution strategy for SQL Database (exponential) with 5 retries and 4 sec delay
    this.SetExecutionStrategy(
         "System.Data.SqlClient", () => new SqlAzureExecutionStrategy(5, TimeSpan.FromSeconds(4)));
  }
}
```

できますし、これを指定するすべての操作を使用して既定の再試行戦略として、 **SetConfiguration** のメソッド、 **DbConfiguration** アプリケーションの起動時をインスタンス化します。 既定では、EF は構成クラスを自動的に検出して使用します。

    DbConfiguration.SetConfiguration(new BloggingContextConfiguration());

コンテキスト クラスに注釈を付けることによって、コンテキストに対して再試行構成クラスを指定できます、 **DbConfigurationType** 属性です。 ただし、構成クラスが 1 つしかない場合、EF はコンテキストに注釈を付けずにそれを使用します。

    [DbConfigurationType(typeof(BloggingContextConfiguration))]
    public class BloggingContext : DbContext
    { ...

特定の操作に対して異なる再試行戦略を使用して、または特定の操作に対する再試行を無効にする必要がある場合を中断またはにフラグを設定することで、戦略をスワップできる構成クラスを作成することができます、 **CallContext**します。 構成クラスでは、このフラグを使用して、戦略を切り替えたり、指定した戦略を無効にして既定の戦略を使用したりできます。 詳細については、次を参照してください。 [実行戦略の中断](http://msdn.microsoft.com/dn307226#transactions_workarounds) ] ページで、再試行実行戦略 (EF6 以降) での制限事項にします。

個々の操作に特定の再試行戦略を使用する別の手法として、必要な戦略クラスのインスタンスを作成し、パラメーターにより目的の設定を指定することができます。 起動の **ExecuteAsync** メソッドです。

    var executionStrategy = new SqlAzureExecutionStrategy(5, TimeSpan.FromSeconds(4));
    var blogs = await executionStrategy.ExecuteAsync(
        async () =>
        {
            using (var db = new BloggingContext("Blogs"))
            {
                // Acquire some values asynchronously and return them
            }
        },
        new CancellationToken()
    );

使用する最も簡単な方法、 **DbConfiguration** クラスと同じアセンブリで検索するには、 **DbContext** クラスです。 ただし、異なるシナリオ (対話型やバックグラウンドでの再試行戦略が異なるなど) で同じコンテキストが必要な場合には、これは適しません。 異なるコンテキストを別の Appdomain で実行する場合は、構成ファイル内で構成クラスを指定するために組み込みサポートを使用するか、またはコードを使用して構成クラスを明示的に設定することができます。 異なる複数のコンテキストを同じ AppDomain 内で実行する必要がある場合には、カスタム ソリューションが必要です。

詳細については、次を参照してください。 [コード ベースの構成 (EF6 以降)](http://msdn.microsoft.com/data/jj680699.aspx)します。

次の表は、EF6 を使用している場合の、組み込み再試行ポリシーの既定の設定を示しています。

![](media/best-practices-retry-service-specific/RetryServiceSpecificGuidanceTable4.png)
## 再試行使用のガイダンス

EF6 を使用して SQL Database にアクセスする場合は、次のガイドラインを検討します。

* 適切なサービス オプション (Shared または Premium) を選択します。 共有インスタンスは、共有サーバーの他のテナントによる使用状況により、通常よりも長い接続遅延や調整の影響を受ける可能性があります。 予測可能なパフォーマンスと信頼性の高い低待機時間での操作が必要な場合は、Premium オプションを選択することを検討してください。
* 固定間隔戦略を Azure SQL Database で使用することは推奨されていません。 代わりに、指数バックオフ戦略を使用します。サービスがオーバーロードする可能性があり、遅延が長くなれば回復するための時間をより多くとることができるからです。
* 接続を定義するときは、接続タイムアウトとコマンド タイムアウトに適切な値を選択します。 タイムアウトは、ビジネス ロジック設計と十分なテストの両方に基づいた値にします。 この値は、時間の経過によるデータの量やビジネス プロセスの変化に応じて、変更することが必要になる場合があります。 タイムアウトが短すぎると、データベースがビジー状態の場合に、接続が途中でエラーになる可能性があります。 タイムアウトが長すぎると、接続エラーを検出するまで長く待ちすぎて、再試行ロジックが正常に機能しなくなる可能性があります。 コンテキストの保存時に実行されるコマンドの数は簡単には特定できないとしても、タイムアウトの値は、エンドツーエンド待機時間の構成要素です。 既定のタイムアウトを変更するには設定して、 **CommandTimeout** のプロパティ、 **DbContext** インスタンス。
* Entity Framework は、構成ファイルで定義されている再試行構成をサポートします。 ただし、Azure 上で最大の柔軟性を実現するために、アプリケーション内でプログラムを使用して構成を作成することを検討してください。 再試行ポリシーの特定のパラメーター (再試行数や再試行間隔など) は、サービス構成ファイルに格納して、実行時に適切なポリシーを作成するために使用することができます。 これにより、アプリケーションを再起動する必要なしに設定を変更できます。

再試行操作について次の設定から始めることを検討します。 再試行間の遅延を指定することはできません (固定されており、指数のシーケンスとして生成されます)。 カスタム再試行戦略を作成しない限り、ここに示すとおり、指定できるのは最大値のみです。 これらは汎用の設定であり、操作を監視して、独自のシナリオに合うように値を微調整する必要があります。

| **Context**          | **サンプルのターゲット E2E<br />最大待機時間** | **再試行ポリシー** | **[設定]**           | **値**   | **動作のしくみ**                                                                                                            |
|----------------------|-----------------------------------|--------------------|------------------------|--------------|-----------------------------------------------------------------------------------------------------------------------------|
| 対話型、UI、<br />またはフォア グラウンド | 2 秒                         | Exponential        | MaxRetryCount<br />MaxDelay | 3<br />750 ミリ秒     | 試行 1-0 秒の遅延<br />試行 2-750 ミリ秒の遅延<br />試行 3-750 ミリ秒の遅延                                                   |
| 背景<br /> またはバッチ            | 30 秒                        | Exponential        | MaxRetryCount<br />MaxDelay | 5<br />12 秒 | 試行 1-0 秒の遅延<br />試行 2 - 最大 1 秒の遅延<br />試行 3 - 最大 3 秒の遅延<br />試行 4 - 最大 7 秒の遅延<br />試行 5-12 秒の遅延 |

> [AZURE.NOTE] エンド ツー エンドの待機時間のターゲットには、サービスへの接続の既定のタイムアウトがあるとします。 接続タイムアウトにより長い時間を指定する場合、エンドツーエンド待機時間は、すべての再試行についてこの追加時間分だけ延長されます。

## 例 (Entity Framework 6 による SQL Database アクセス)

次のコード例は、Entity Framework を使用する単純なデータ アクセス ソリューションを定義します。 という名前のクラスのインスタンスを定義することで、特定の再試行戦略を設定して **BlogConfiguration** まで **DbConfiguration**します。

```csharp
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.Entity.SqlServer;
using System.Threading.Tasks;

namespace RetryCodeSamples
{
    public class BlogConfiguration : DbConfiguration
    {
        public BlogConfiguration()
        {
            // Set up the execution strategy for SQL Database (exponential) with 5 retries and 12 sec delay.
            // These values could be loaded from configuration rather than being hard-coded.
            this.SetExecutionStrategy(
                    "System.Data.SqlClient", () => new SqlAzureExecutionStrategy(5, TimeSpan.FromSeconds(12)));
        }
    }

    // Specify the configuration type if more than one has been defined.
    // [DbConfigurationType(typeof(BlogConfiguration))]
    public class BloggingContext : DbContext
    {
        // Definition of content goes here.
    }

    class EF6CodeSamples
    {
        public async static Task Samples()
        {
            // Execution strategy configured by DbConfiguration subclass, discovered automatically or
            // or explicitly indicated through configuration or with an attribute. Default is no retries.
            using (var db = new BloggingContext("Blogs"))
            {
                // Add, edit, delete blog items here, then:
                await db.SaveChangesAsync();
            }
        }
    }
}
```

Entity Framework の再試行メカニズムの使用例を参照して [接続回復/再試行ロジック](http://msdn.microsoft.com/data/dn456835.aspx)します。

## 詳細情報

* [Microsoft Azure SQL Database Performance and Elasticity Guide (Microsoft Azure SQL Database のパフォーマンスと弾力性に関するガイド)](http://social.technet.microsoft.com/wiki/contents/articles/3507.windows-azure-sql-database-performance-and-elasticity-guide.aspx)

## ADO.NET の再試行ガイドラインを使用する SQL Database

SQL Database は、多様なサイズで利用できるホステッド SQL データベースです。これは Standard (共有) サービスと Premium (非共有) サービスのどちらとしてでも使用できます。

### 再試行メカニズム

ADO.NET を使用してアクセスする際には、SQL Database には再試行の組み込みサポートはありません。 ただし、要求からのリターン コードを使用して、要求が失敗した理由を判別することができます。 ページ [Azure SQL Database の調整](http://msdn.microsoft.com/library/dn338079.aspx) 調整して、特定の状況に応じて、リターン コードの接続をようにする方法について説明し、これらを処理し、操作を再試行する方法です。

一時的なエラー処理アプリケーション ブロック (Topaz) を使用するには、Nuget パッケージ EnterpriseLibrary.TransientFaultHandling.Data (クラス **SqlAzureTransientErrorDetectionStrategy**) SQL Database の再試行メカニズムを実装します。

ブロックにも用意されています、 **ReliableSqlConnection** は古い ADO.NET 1.0 API を実装するクラス (**IDbConnection** の代わりに **DbConnection**) し、再試行と接続管理を内部的に実行します。 便利である一方、このブロックは再試行を伴う操作の呼び出しにさまざまな一連のメソッドを使用することが求められるため、単なる直接の置き換えにはなりません。 Azure サービスの実装と使用の際に推奨されている、非同期実行はサポートしません。 さらに、このクラスは ADO.NET 1.0 を使用しているので、ADO.NET に対する最新の機能強化と更新を活用できません。

### ポリシーの構成 (ADO.NET による SQL Database アクセス)

一時的エラー処理アプリケーション ブロックは、ファイル ベースの構成とプログラムによる構成の両方をサポートします。 一般に、最大の柔軟性を実現するために、プログラムによる構成を使用する必要があります (詳細については、次のセクションの注を参照してください)。 アプリケーションの起動時に 1 回実行されると、次のコードを作成し、設定、 **RetryManager** 再試行戦略を Azure SQL Database の使用に適した 4 つのリストをします。 既定の戦略も設定、 **RetryManager**します。 これらは、接続またはコマンドの作成時に別の戦略が指定されない場合に、接続とコマンドに使用される戦略です。

```csharp
RetryManager.SetDefault(new RetryManager(
    new List<RetryStrategy> { new ExponentialBackoff(name: "default", retryCount: 3,
                                                    minBackoff:     TimeSpan.FromMilliseconds(100),
                                                    maxBackoff:     TimeSpan.FromSeconds(30),
                                                    deltaBackoff:   TimeSpan.FromSeconds(1),
                                                    firstFastRetry: true),
                            new ExponentialBackoff(name: "default sql connection", retryCount: 3,
                                                    minBackoff:     TimeSpan.FromMilliseconds(100),
                                                    maxBackoff:     TimeSpan.FromSeconds(30),
                                                    deltaBackoff:   TimeSpan.FromSeconds(1),
                                                    firstFastRetry: true),
                            new ExponentialBackoff(name: "default sql command", retryCount: 3,
                                                    minBackoff:     TimeSpan.FromMilliseconds(100),
                                                    maxBackoff:     TimeSpan.FromSeconds(30),
                                                    deltaBackoff:   TimeSpan.FromSeconds(1),
                                                    firstFastRetry: true),
                            new ExponentialBackoff(name: "alt sql", retryCount: 5,
                                                    minBackoff:     TimeSpan.FromMilliseconds(100),
                                                    maxBackoff:     TimeSpan.FromSeconds(30),
                                                    deltaBackoff:   TimeSpan.FromSeconds(1),
                                                    firstFastRetry: true), },
    "default",
    new Dictionary<string, string> {
        {
        RetryManagerSqlExtensions.DefaultStrategyConnectionTechnologyName, "default sql connection"
        },
        {
        RetryManagerSqlExtensions.DefaultStrategyCommandTechnologyName, "default sql command"}
        }));
```

使用する方法については、Azure SQL データベースにアクセスすると、構成済みの再試行ポリシーを参照してください、 [例](#examples-sql-database-using-ado-net-) 以下のセクションです。

Transient Fault Handling Application Block の既定の戦略のセクションで示した [一時的なエラー処理アプリケーション ブロック (Topaz) 戦略](#transient-fault-handling-application-block-topaz-strategies) このガイダンスの末尾にします。

### 再試行使用のガイダンス

ADO.NET を使用して SQL Database にアクセスする場合は、次のガイドラインを検討します。

* 適切なサービス オプション (Shared または Premium) を選択します。 共有インスタンスは、共有サーバーの他のテナントによる使用状況により、通常よりも長い接続遅延や調整の影響を受ける可能性があります。 予測可能性の高いパフォーマンスと信頼性の高い低待機時間での操作が必要な場合は、Premium オプションを選択することを検討してください。
* データの不整合の原因となる非べき等操作を避けるために、再試行は必ず適切なレベルまたはスコープで実行します。 理想的には、すべての操作はべき等にして、不整合を発生させずに繰り返し実行できるようにする必要があります。 これが当てはまらない場合、再試行は、操作が失敗した場合に、関連するすべての変更を元に戻すことができるレベルまたはスコープで (たとえば 1 トランザクションのスコープ内で) 実行する必要があります。 詳細については、次を参照してください。 [クラウド サービスの基本データ アクセス層 – 一時的な障害処理](http://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx#Idempotent_Guarantee)します。
* 固定間隔戦略は、非常に短い間隔でごくわずかな回数の再試行が実行されるのみという対話型のシナリオを除き、Azure SQL Database での使用は推奨されていません。 代わりに、ほとんどのシナリオで、指数バックオフ戦略を使用することを考慮してください。
* 接続を定義するときは、接続タイムアウトとコマンド タイムアウトに適切な値を選択します。 タイムアウトが短すぎると、データベースがビジー状態の場合に、接続が途中でエラーになる可能性があります。 タイムアウトが長すぎると、接続エラーを検出するまで長く待ちすぎて、再試行ロジックが正常に機能しなくなる可能性があります。 タイムアウトの値は、エンドツーエンド待機時間の構成要素です。これはすべての再試行向けの再試行ポリシーに指定される再試行遅延に、事実上追加されます。
* 指数バックオフ再試行ロジックを使用している場合でも、一定回数の再試行が実行された後は接続を閉じ、新しい接続で操作を再試行します。 同じ接続で同じ操作を複数回再試行することは、接続問題を生じさせる要因となる場合があります。 この方法の例は、次を参照してください。 [クラウド サービスの基本データ アクセス層 – 一時的な障害処理](http://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx)します。
* 接続プールが使用中であれば (既定値)、接続を閉じてから再び開いた後であっても、同じ接続がプールから選択される可能性があります。 大文字と小文字の場合は、問題を解決する手法を呼び出すことです、 **ClearPool** のメソッド、 **SqlConnection** ない再利用できるように接続をマークするクラス。 ただし、これは数回の接続試行が失敗し、問題がある接続に関連した SQL タイムアウト (エラー コード -2) などの、特定クラスの一時的エラーを検出した場合にのみ実行してください。
* データ アクセス コードとして開始されたトランザクションを使用している場合 **TransactionScope** インスタンス、再試行ロジックが、接続を再度開き、新しいトランザクション スコープを開始する必要があります。 この理由から、再試行可能コード ブロックは、トランザクションのスコープ全体をカバーしている必要があります。
* 一時的エラー処理アプリケーション ブロックは、構成ファイル内ですべて定義されている再試行構成をサポートします。 ただし、Azure 上で最大の柔軟性を実現するために、アプリケーション内でプログラムを使用して構成を作成することを検討してください。 再試行ポリシーの特定のパラメーター (再試行数や再試行間隔など) は、サービス構成ファイルに格納して、実行時に適切なポリシーを作成するために使用することができます。 これにより、アプリケーションを再起動する必要なしに設定を変更できます。

再試行操作について次の設定から始めることを検討します。 これらは汎用の設定であり、操作を監視して、独自のシナリオに合うように値を微調整する必要があります。

| **Context**          | **サンプルのターゲット E2E<br />最大待機時間** | **再試行戦略** | **[設定]**                                                          | **値**                 | **動作のしくみ**                                                                                                              |
|----------------------|-----------------------------------|--------------------|-----------------------------------------------------------------------|----------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| 対話型、UI、<br />またはフォア グラウンド | 2 秒                             | FixedInterval      | 再試行の回数<br />再試行の間隔<br />最初の高速再試行                           | 3<br />500 ミリ秒<br />true              | 試行 1-0 秒の遅延<br />試行 2-500 ミリ秒の遅延<br />試行 3-500 ミリ秒の遅延                                                     |
| 背景<br />またはバッチ            | 30 秒                            | ExponentialBackoff | 再試行の回数<br />最小バックオフ<br />最大バックオフ<br />デルタ バックオフ<br />最初の高速再試行 | 5<br />0 秒<br />60 秒<br />2 秒<br />false | 試行 1-0 秒の遅延<br />試行 2 - 最大 2 秒の遅延<br />試行 3 - 最大 6 秒の遅延<br />試行 4 - 最大 14 秒の遅延<br />試行 5 - 最大 30 秒の遅延 |

> [AZURE.NOTE] エンド ツー エンドの待機時間のターゲットには、サービスへの接続の既定のタイムアウトがあるとします。 接続タイムアウトにより長い時間を指定する場合、エンドツーエンド待機時間は、すべての再試行についてこの追加時間分だけ延長されます。

### 例 (ADO.NET による SQL Database アクセス)

このセクションでは、Transient Fault Handling Application Block を使用して、構成した再試行ポリシーのセットを使用して Azure SQL データベースにアクセスする方法を説明、 **RetryManager** (前のセクションで示すように [ポリシー構成](#policy-configuration-sql-database-using-ado-net-)します。 ブロックを使用する最も簡単な手段が、 **ReliableSqlConnection** クラス、または拡張メソッドをなどを呼び出すこと **OpenWithRetry** 接続 (を参照してください [The Transient Fault Handling Application Block](http://msdn.microsoft.com/library/hh680934.aspx) の詳細)。

ただし、一時的エラー処理アプリケーション ブロックの現在のバージョンでは、これらの方法は SQL Database に対する非同期操作をネイティブにサポートしていません。 推奨される手法では、SQL Database などの Azure サービスにアクセスするには、非同期技法のみを使用することが求められます。このため、一時的エラー処理アプリケーション ブロックを SQL Database と一緒に使用するには、次の技法について考慮する必要があります。

バージョン 5 の C# 言語では、簡略化された非同期サポートを使用して、ブロック (Topaz) により提供される非同期バージョンのメソッドを作成することができます。 たとえば、次のコードは、の非同期バージョンを作成する方法、 **ExecuteReaderWithRetry** 拡張メソッド。 元のコードへの変更や追加が強調表示されます。 Topaz のソース コードは GitHub で [一時的なエラー処理アプリケーション ブロック ("Topaz")](http://topaz.codeplex.com/SourceControl/latest)します。

```csharp
public async static Task<SqlDataReader> ExecuteReaderWithRetryAsync(this SqlCommand command, RetryPolicy cmdRetryPolicy,
RetryPolicy conRetryPolicy)
{
    GuardConnectionIsNotNull(command);

    // Check if retry policy was specified, if not, use the default retry policy.
    return await (cmdRetryPolicy ?? RetryPolicy.NoRetry).ExecuteAsync(async () =>
    {
        var hasOpenConnection = await EnsureValidConnectionAsync(command, conRetryPolicy).ConfigureAwait(false);

        try
        {
            return await command.ExecuteReaderAsync().ConfigureAwait(false);
        }
        catch (Exception)
        {
            if (hasOpenConnection && command.Connection != null && command.Connection.State == ConnectionState.Open)
            {
                command.Connection.Close();
            }

            throw;
        }
    }).ConfigureAwait(false);
}
```

この新しい非同期拡張メソッドは、ブロック (Topaz) に含まれている同期バーションと同じ方法で使用できます。

```csharp
var sqlCommand = sqlConnection.CreateCommand();
sqlCommand.CommandText = "[some query]";

var retryPolicy =
    RetryManager.Instance.GetRetryPolicy<SqlDatabaseTransientErrorDetectionStrategy>("alt sql");
using (var reader = await sqlCommand.ExecuteReaderWithRetryAsync(retryPolicy))
{
    // Do something with the values
}
```

ただし、この方法は、個々の操作またはコマンドのみを扱い、正しく定義されたトランザクション境界が存在する可能性があるステートメントのブロックは扱いません。 さらに、接続プールから障害がある接続を削除して、それらが後続の試行で選択されないようにするといった状況も扱いません。 これらの問題を解決する非同期の例を参照して [クラウド サービスの基本データ アクセス層 – 一時的な障害処理](http://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx#Timeouts_amp_Connection_Management)します。 このメソッドは、任意のシーケンスのデータベース命令を再試行するだけでなく、接続プールをクリアして無効な接続を削除し、プロセス全体をインストルメント化します。 この例で示すコードは同期ですが、非同期コードに変換するのは比較的簡単です。

### 詳細

一時的エラー処理アプリケーション ブロックの使用の詳細については、次を参照してください。

* [Using the Transient Fault Handling Application Block with SQL Azure (SQL Azure での一時的エラー処理アプリケーション ブロックの使用)](http://msdn.microsoft.com/library/hh680899.aspx)
* [Perseverance, Secret of All Triumphs: Using the Transient Fault Handling Application Block (忍耐力、すべての勝利の秘訣: 一時的エラー処理アプリケーション ブロックの使用)](http://msdn.microsoft.com/library/dn440719.aspx)
* [Cloud Service Fundamentals Data Access Layer – Transient Fault Handling (クラウド サービスの基本データアクセス層 – 一時的エラー処理)](http://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx)

SQL Database を最大限に活用することに関する一般的なガイダンスについては、次を参照してください。

* [Microsoft Azure SQL Database Performance and Elasticity Guide (Microsoft Azure SQL Database のパフォーマンスと弾力性に関するガイド)](http://social.technet.microsoft.com/wiki/contents/articles/3507.windows-azure-sql-database-performance-and-elasticity-guide.aspx)
* [Minimizing Connection Pool errors in SQL Azure (SQL Azure での接続プール エラーの最小化)](http://blogs.msdn.com/b/adonet/archive/2011/11/05/minimizing-connection-pool-errors-in-sql-azure.aspx)

## Service Bus の再試行ガイドライン

Service Bus は、クラウド メッセージング プラットフォームであり、クラウドまたはオンプレミスでホストされているアプリケーションのコンポーネントに対して、向上したスケーラビリティと回復性を備えた疎結合のメッセージ交換を提供します。

### 再試行メカニズム

Service Bus の実装を使用して再試行を実装して、 [RetryPolicy](http://msdn.microsoft.com/library/microsoft.servicebus.retrypolicy.aspx) 基本クラスです。 すべてのサービス バス クライアントを公開、 **RetryPolicy** の実装のいずれかに設定できるプロパティ、 **RetryPolicy** 基本クラスです。 組み込み実装は、次のとおりです。

*  [RetryExponential クラス](http://msdn.microsoft.com/library/microsoft.servicebus.retryexponential.aspx)します。 この、バックオフ間隔、再試行回数を制御するプロパティが公開され、 **TerminationTimeBuffer** の操作が完了する時間の合計を制限するために使用されているプロパティ。
*  [NoRetry クラス](http://msdn.microsoft.com/library/microsoft.servicebus.noretry.aspx)します。 これは、再試行がバッチまたは複数ステップの操作の一部として別のプロセスにより管理されている場合などの、Service Bus API レベルでの再試行が不要な場合に使用されます。

記載されている、Service Bus アクションは、例外の範囲を返すことができます [付録: メッセージング例外](http://msdn.microsoft.com/library/hh418082.aspx)します。 このリストには、操作の再試行が適切であると例外が示す場合の関連情報が記載されています。 たとえば、 [ServerBusyException](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.serverbusyexception.aspx) ことを示します、クライアントが一定時間の待機を操作を再試行する必要があります。 発生、 **ServerBusyException** も Service Bus 計算された再試行遅延にさらに 10 秒の遅延を追加する別のモードに切り替えが発生します。 このモードは、短時間が経過した後にリセットされます。

Service Bus 公開から返される例外は、 **IsTransient** プロパティを示すかどうか、クライアントは、操作を再試行してください。 組み込み **RetryExponential** ポリシーが依存、 **IsTransient** プロパティに、 **MessagingException** クラスで、すべての Service Bus 例外の基本クラスです。 カスタム実装を作成する場合、 **RetryPolicy** 基本クラスの例外の種類の組み合わせを使用することがおよび **IsTransient** 経由で詳細に制御を提供するプロパティは、操作を再試行してください。 検出など、 **QuotaExceededException** に送信されるメッセージを再試行する前に、キューを排出するアクションを実行するとします。

### ポリシーの構成 (Service Bus)

再試行ポリシーは、プログラムで、設定して、既定のポリシーとして設定することができます、 **NamespaceManager** および、 **MessagingFactory**, 、または各メッセージング クライアントに対して個別にします。 設定するメッセージ セッションの既定の再試行ポリシーを設定する、 **RetryPolicy** の **NamespaceManager**します。

    namespaceManager.Settings.RetryPolicy = new RetryExponential(minBackoff: TimeSpan.FromSeconds(0.1),
                                                                 maxBackoff: TimeSpan.FromSeconds(30),
                                                                 deltaBackoff: TimeSpan.FromSeconds(2),
                                                                 terminationTimeBuffer: TimeSpan.FromSeconds(5),
                                                                 maxRetryCount: 3);

このコードでは、わかりやすくするために名前付きパラメーターを使用していることに注意してください。 ただし、どのパラメーターも省略可能ではないので、名前は省略することもできます。

    namespaceManager.Settings.RetryPolicy = new RetryExponential(TimeSpan.FromSeconds(0.1),
                     TimeSpan.FromSeconds(30), TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(5), 3);

メッセージング ファクトリから作成されたすべてのクライアントの既定の再試行ポリシーを設定を設定する、 **RetryPolicy** の **MessagingFactory**します。

    messagingFactory.RetryPolicy = new RetryExponential(minBackoff: TimeSpan.FromSeconds(0.1),
                                                        maxBackoff: TimeSpan.FromSeconds(30),
                                                        deltaBackoff: TimeSpan.FromSeconds(2),
                                                        terminationTimeBuffer: TimeSpan.FromSeconds(5),
                                                        maxRetryCount: 3);

メッセージング クライアントに対して再試行ポリシーを設定したり、既定のポリシーを上書きするを設定する、 **RetryPolicy** プロパティが必要なポリシー クラスのインスタンスを使用します。

```csharp
client.RetryPolicy = new RetryExponential(minBackoff: TimeSpan.FromSeconds(0.1),
                                            maxBackoff: TimeSpan.FromSeconds(30),
                                            deltaBackoff: TimeSpan.FromSeconds(2),
                                            terminationTimeBuffer: TimeSpan.FromSeconds(5),
                                            maxRetryCount: 3);
```

再試行ポリシーは、個々の操作レベルで設定することはできません。 これはメッセージング クライアントのすべての操作に適用されます。
次の表は、組み込み再試行ポリシーの既定の設定を示しています。

![](media/best-practices-retry-service-specific/RetryServiceSpecificGuidanceTable7.png)

### 再試行使用のガイダンス

Service Bus を使用する場合は、次のガイドラインについて検討します。

* 組み込みの使用時に **RetryExponential** 実装では、ポリシー、フォールバック操作がサーバー ビジー例外に、自動的に反応を実装しないでが適切な再試行モードに切り替わります。
* Service Bus は、組み合わせ名前空間という機能をサポートします。これは、プライマリ名前空間内のキューでエラーが発生した場合の、別の名前空間内にあるバックアップ キューへの自動フェールオーバーを実装します。 セカンダリ キューからのメッセージは、プライマリ キューが回復したらそこに送り戻すことができます。 この機能は、一時的なエラーに対処するために役立ちます。 詳細については、次を参照してください。 [非同期メッセージング パターンと高可用性](http://msdn.microsoft.com/library/azure/dn292562.aspx)します。

再試行操作について次の設定から始めることを検討します。 これらは汎用の設定であり、操作を監視して、独自のシナリオに合うように値を微調整する必要があります。


![](media/best-practices-retry-service-specific/RetryServiceSpecificGuidanceTable8.png)

### テレメトリ

Service Bus を使用して ETW イベントとしての再試行の記録、 **EventSource**します。 アタッチする必要があります、 **EventListener** イベントをキャプチャしてパフォーマンス ビューアーに表示または適切な宛先ログに書き込んだりするには、イベント ソースにします。 使って、 [セマンティック ログ アプリケーション ブロック](http://msdn.microsoft.com/library/dn775006.aspx) これを行う。 再試行イベントは、次の形式です。

```text
Microsoft-ServiceBus-Client/RetryPolicyIteration
ThreadID="14,500"
FormattedMessage="[TrackingId:] RetryExponential: Operation Get:https://retry-guidance-tests.servicebus.windows.net/TestQueue/?api-version=2014-05 at iteration 0 is retrying after 00:00:00.1000000 sleep because of Microsoft.ServiceBus.Messaging.MessagingCommunicationException: The remote name could not be resolved: 'retry-guidance-tests.servicebus.windows.net'.TrackingId:6a26f99c-dc6d-422e-8565-f89fdd0d4fe3, TimeStamp:9/5/2014 10:00:13 PM."
trackingId=""
policyType="RetryExponential"
operation="Get:https://retry-guidance-tests.servicebus.windows.net/TestQueue/?api-version=2014-05"
iteration="0"
iterationSleep="00:00:00.1000000"
lastExceptionType="Microsoft.ServiceBus.Messaging.MessagingCommunicationException"
exceptionMessage="The remote name could not be resolved: 'retry-guidance-tests.servicebus.windows.net'.TrackingId:6a26f99c-dc6d-422e-8565-f89fdd0d4fe3,TimeStamp:9/5/2014 10:00:13 PM"
```

### 例 (Service Bus)

次のコード例は、以下に対する再試行ポリシーの設定方法を示しています。

* 名前空間マネージャー。 ポリシーは、そのマネージャー上のすべての操作に適用されます。個々の操作向けにオーバーライドすることはできません。
* メッセージング ファクトリ。 ポリシーは、このファクトリから作成されたすべてのクライアントに適用されます。個々のクライアントの作成時にオーバーライドすることはできません。
* 個々のメッセージング クライアント クライアントの作成後に、そのクライアントに再試行ポリシーを設定することができます。 ポリシーは、そのクライアント上のすべての操作に適用されます。

```csharp
using System;
using System.Threading.Tasks;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Messaging;

namespace RetryCodeSamples
{
    class ServiceBusCodeSamples
    {
        private const string connectionString =
            @"Endpoint=sb://[my-namespace].servicebus.windows.net/;
                SharedAccessKeyName=RootManageSharedAccessKey;
                SharedAccessKey=C99..........Mk=";

        public async static Task Samples()
        {
            const string QueueName = "TestQueue";

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.Http;

            var namespaceManager = NamespaceManager.CreateFromConnectionString(connectionString);

            // The namespace manager will have a default exponential policy with 10 retry attempts
            // and a 3 second delay delta.
            // Retry delays will be approximately 0 sec, 3 sec, 9 sec, 25 sec and the fixed 30 sec,
            // with an extra 10 sec added when receiving a ServiceBusyException.

            {
                // Set different values for the retry policy, used for all operations on the namespace manager.
                namespaceManager.Settings.RetryPolicy =
                    new RetryExponential(
                        minBackoff: TimeSpan.FromSeconds(0),
                        maxBackoff: TimeSpan.FromSeconds(30),
                        deltaBackoff: TimeSpan.FromSeconds(1.75),
                        terminationTimeBuffer: TimeSpan.FromSeconds(4),
                        maxRetryCount: 3);

                // Policies cannot be specified on a per-operation basis.
                if (!await namespaceManager.QueueExistsAsync(QueueName))
                {
                    await namespaceManager.CreateQueueAsync(QueueName);
                }
            }


            var messagingFactory = MessagingFactory.Create(
                namespaceManager.Address, namespaceManager.Settings.TokenProvider);
            // The messaging factory will have a default exponential policy with 10 retry attempts
            // and a 3 second delay delta.
            // Retry delays will be approximately 0 sec, 3 sec, 9 sec, 25 sec and the fixed 30 sec,
            // with an extra 10 sec added when receiving a ServiceBusyException.

            {
                // Set different values for the retry policy, used for clients created from it.
                messagingFactory.RetryPolicy =
                    new RetryExponential(
                        minBackoff: TimeSpan.FromSeconds(1),
                        maxBackoff: TimeSpan.FromSeconds(30),
                        deltaBackoff: TimeSpan.FromSeconds(2),
                        terminationTimeBuffer: TimeSpan.FromSeconds(5),
                        maxRetryCount: 3);


                // Policies cannot be specified on a per-operation basis.
                var session = await messagingFactory.AcceptMessageSessionAsync();
            }


            {
                var client = messagingFactory.CreateQueueClient(QueueName);
                // The client inherits the policy from the factory that created it.


                // Set different values for the retry policy on the client.
                client.RetryPolicy =
                    new RetryExponential(
                        minBackoff: TimeSpan.FromSeconds(0.1),
                        maxBackoff: TimeSpan.FromSeconds(30),
                        deltaBackoff: TimeSpan.FromSeconds(2),
                        terminationTimeBuffer: TimeSpan.FromSeconds(5),
                        maxRetryCount: 3);


                // Policies cannot be specified on a per-operation basis.
                var session = await client.AcceptMessageSessionAsync();
            }
        }
    }
}
```

## 詳細情報

* [非同期メッセージング パターンと高可用性](http://msdn.microsoft.com/library/azure/dn292562.aspx)

## Cache (Redis) の再試行ガイドライン

Azure Redis Cache は、一般的なオープン ソース Redis Cache に基づく、高速データ アクセスと低待機時間のキャッシュ サービスです。 これは安全で、Microsoft により管理されており、Azure の任意のアプリケーションからアクセスできます。

このセクションのガイダンスは、StackExchange.Redis クライアントを使用したキャッシュへのアクセスに基づいています。 その他の適切なクライアントの一覧を参照して、 [Redis web サイト](http://redis.io/clients), 、この異なる再試行メカニズムがあります。

StackExchange.Redis クライアントは、1 つの接続で多重化を使用することに注意してください。 推奨される使用法は、アプリケーションの起動時にこのクライアントのインスタンスを作成し、そのインスタンスをキャッシュに対するすべての操作に使用するというものです。 このため、キャッシュへの接続が行われるのは 1 回限りであるので、このセクションのすべてのガイダンスは、その初期接続の再試行ポリシーに関連するものとなっており、キャッシュにアクセスする各操作に対するものではありません。

### 再試行メカニズム

StackExchange.Redis クライアントは、一連のオプションで構成される接続マネージャー クラスを使用します。 これらのオプションには、 **ConnectRetry** キャッシュに失敗した接続を再試行する回数を指定するプロパティです。 ただし、再試行ポリシーは初期接続アクションに対してのみ使用され、再試行間の待機はしません。

### ポリシーの構成 (Azure Redis Cache)

再試行ポリシーは、キャッシュに接続する前に、クライアントにオプションを設定することで、プログラムによって構成されます。 インスタンスを作成することでそのため、 **ConfigurationOptions** クラス、そのプロパティを設定して、 **接続** メソッドです。

```csharp
var options = new ConfigurationOptions { EndPoints = { "localhost" },
                                            ConnectRetry = 3,
                                            ConnectTimeout = 2000 };
ConnectionMultiplexer redis = ConnectionMultiplexer.Connect(options, writer);
```

 **ConnectTimeout** プロパティは、ミリ秒単位で、最大待機時間を指定)、再試行間の遅延ではありません。

または、文字列としてオプションを指定し、これを渡す、 **接続** メソッドです。

```csharp
    var options = "localhost,connectRetry=3,connectTimeout=2000";
    ConnectionMultiplexer redis = ConnectionMultiplexer.Connect(options, writer);
```

キャッシュに接続する場合は、オプションを直接指定することもできます。

```csharp
var conn = ConnectionMultiplexer.Connect("redis0:6380,redis1:6380,connectRetry=3");
```

次の表は、組み込み再試行ポリシーの既定の設定を示しています。

| **Context**          | **設定**                             | **既定値**<br />(v 1.0.331)           | **意味**                                                                                                                                                                                                   |
|----------------------|-----------------------------------------|-----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 構成オプション | ConnectRetry<br /><br />ConnectTimeout<br /><br />Synctimeout を加算 | 3<br /><br />最大 5,000 ミリ秒に synctimeout を加算<br />1,000 | 繰り返す回数の合計は、最初の接続操作中に試行を接続します。<br />タイムアウト (ミリ秒) のでは、操作を接続します。 再試行を間遅延ではありません。<br />同期操作を許可する時間 (ミリ秒)。 |

> [AZURE.NOTE] 操作のエンド ツー エンドの待機時間に synctimeout を加算が貢献しています。 ただし、一般に、同期操作の使用は推奨されません。 詳細については、次を参照してください。 [パイプラインと Multiplexers](http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/PipelinesMultiplexers.md)します。

## 再試行使用のガイダンス

Azure Redis Cache を使用する場合は、次のガイドラインについて検討します。

* StackExchange Redis クライアントは、その独自の再試行を管理します。ただし、アプリケーションの初回の起動時にキャッシュへの接続を確立するときのみです。 この接続を確立するために接続タイムアウトと再試行の回数を構成することができますが、再試行ポリシーは、キャッシュに対する操作には適用されません。
* 再試行メカニズムには、再試行間の遅延はありません。 指定した接続がタイムアウトすると、指定した回数だけ、失敗した接続を再試行するのみです。
* 多数の再試行を使用するのではなく、元のデータ ソースにアクセスすることによってフォールバックすることを検討してください。

## テレメトリ

使用して、接続 (は他の操作ではなく) に関する情報を収集する、 **TextWriter**します。

```csharp
var writer = new StringWriter();
...
ConnectionMultiplexer redis = ConnectionMultiplexer.Connect(options, writer);
```

これが生成する出力の例を次に示します。

```text
localhost:6379,connectTimeout=2000,connectRetry=3
1 unique nodes specified
Requesting tie-break from localhost:6379 > __Booksleeve_TieBreak...
Allowing endpoints 00:00:02 to respond...
localhost:6379 faulted: SocketFailure on PING
localhost:6379 failed to nominate (Faulted)
> UnableToResolvePhysicalConnection on GET
No masters detected
localhost:6379: Standalone v2.0.0, master; keep-alive: 00:01:00; int: Connecting; sub: Connecting; not in use: DidNotRespond
localhost:6379: int ops=0, qu=0, qs=0, qc=1, wr=0, sync=1, socks=2; sub ops=0, qu=0, qs=0, qc=0, wr=0, socks=2
Circular op-count snapshot; int: 0 (0.00 ops/s; spans 10s); sub: 0 (0.00 ops/s; spans 10s)
Sync timeouts: 0; fire and forget: 0; last heartbeat: -1s ago
resetting failing connections to retry...
retrying; attempts left: 2...
...
```

## 例 (Azure Redis Cache)

次のコード例は、アプリケーションの起動時に StackExchange.Redis クライアントを初期化して Azure Redis Cache にアクセスするときに、接続タイムアウト設定と再試行回数を構成する方法を示しています。 接続タイムアウトとは、キャッシュへの接続で許容される待機期間のことであり、再試行間の遅延のことではありません。

この例のインスタンスを使用して構成を設定する方法を示しています、 **ConfigurationOptions**します。

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using StackExchange.Redis;

namespace RetryCodeSamples
{
    class CacheRedisCodeSamples
    {
        public async static Task Samples()
        {
            var writer = new StringWriter();

            {
                try
                {
                    // Using object-based configuration.
                    var options = new ConfigurationOptions
                                        {
                                            EndPoints = { "localhost" },
                                            ConnectRetry = 3,
                                            ConnectTimeout = 2000  // The maximum waiting time (ms), not the delay for retries.
                                        };
                    ConnectionMultiplexer redis = ConnectionMultiplexer.Connect(options, writer);

                    // Store a reference to the multiplexer for use in the application.
                }
                catch
                {
                    Console.WriteLine(writer.ToString());
                    throw;
                }
            }
        }
    }
}
```

この例は、オプションを文字列として指定することで、構成を設定する方法を示しています。

```csharp
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using StackExchange.Redis;

namespace RetryCodeSamples
{
    class CacheRedisCodeSamples
    {
        public async static Task Samples()
        {
            var writer = new StringWriter();

            {
                try
                {
                    // Using string-based configuration.
                    var options = "localhost,connectRetry=3,connectTimeout=2000";
                    ConnectionMultiplexer redis = ConnectionMultiplexer.Connect(options, writer);

                    // Store a reference to the multiplexer for use in the application.
                }
                catch
                {
                    Console.WriteLine(writer.ToString());
                    throw;
                }
            }
        }
    }
}
```

例については、次を参照してください。 [構成](http://github.com/StackExchange/StackExchange.Redis/blob/master/Docs/Configuration.md#configuration) プロジェクト web サイトにします。

## 詳細情報

* [Redis の Web サイト](http://redis.io/)

## DocumentDB (プレリリース) の再試行ガイドライン

DocumentDB は、スキーマのない JSON データ モデルに対する、高度なクエリとインデックス作成機能を持つ、完全に管理されたサービスとしてのドキュメント データベースです。 これは構成可能で信頼性の高いパフォーマンスと、ネイティブの JavaScript トランザクション処理を提供し、クラウド用にエラスティックなスケーラビリティを備えて作成されています。

## 再試行メカニズム

プレリリース バージョンの DocumentDB クライアントには、内部に構成できない再試行メカニズムが組み込まれています (ただし今後のリリースで変更される可能性があります)。 この再試行メカニズムの既定の設定は、それを使用するコンテキストによって異なります。 一部の操作は、ハード コーディングされたパラメーターを持つ、指数バックオフ戦略を使用します。 他のユーザーのみを指定再試行の回数を実行し、[再試行の待ち時間を使用して、 [DocumentClientException](http://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx) サービスから返されるインスタンス。 遅延が指定されていない場合は、5 秒の遅延が使用されます。

## ポリシーの構成 (DocumentDB)

なし。 再試行を実装するために使用されるすべてのクラスは、内部クラスです。 再試行パラメーターは、定数であるか、またはパラメーターを使用してクラス コンストラクターに設定されるかのいずれかです。

次の表は、組み込み再試行ポリシーの既定の設定を示しています。

| **Context**            | **Settings**                                      | **値** | **動作のしくみ**                                                                                                                                               |
|------------------------|---------------------------------------------------|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 再試行ポリシー (内部) | MaxRetryAttemptsOnQuery<br /><br />MaxRetryAttemptsOnRequest | 3<br /><br />0        | ドキュメント クエリの再試行回数。 この値は変更できません。<br />他の要求の再試行の回数。 この値は変更できません。 |

## 再試行使用のガイダンス

DocumentDB を使用する場合は、次のガイドラインについて検討します。

* 既定の再試行ポリシーは変更できません。
* 既定の設定の詳細については、「TBD」を参照してください。

## テレメトリ

再試行回数が、.NET により、構造化されていないトレース メッセージとしてログインしている **TraceSource**します。 構成する必要があります、 **TraceListener** 、イベントをキャプチャし、適切な宛先ログに書き込んだりできます。

## Search 再試行のガイドライン

Azure Search は、Web サイトまたはアプリケーションへの強力で高度な検索機能の追加、検索結果のすばやく簡単な調整、および微調整された豊富な順位付けモデルの構築を行うために使用できます。

### 再試行メカニズム

標準的な使用では HTTP 要求を用いるので、Search には組み込み再試行メカニズムはありません。 再試行を実装するには、REST クライアントの汎用実装を使用して、サービスからの応答に基づいて、操作を再試行するタイミングおよび条件を決定することができます。 詳細については、セクションを参照してください。 [一般的な REST および再試行のガイドライン](#general-rest-and-retry-guidelines) このガイドで後述します。

### 再試行使用のガイダンス

Azure Search を使用する場合は、次のガイドラインについて検討します。

* エラーの種類を判断するには、サービスによって返される状態コードを使用します。 状態コードが定義されている [HTTP 状態コード (Azure Search)](http://msdn.microsoft.com/library/dn798925.aspx)します。 状態コード 503 (サービス利用不可) は、サービスが過負荷であり、要求をすぐには処理できないことを示します。 適切なアクションは、サービスが回復するための時間が経過するのを待ち、それから操作を再試行することです。 遅延間隔があまりに短いと、その後に再試行しても、利用できない状態を長引かせてしまう可能性があります。
* セクションを参照して [一般的な REST および再試行のガイドライン](#general-rest-and-retry-guidelines) REST 操作の再試行に関する一般情報については、このガイドで後述します。

## 詳細情報

* [Azure Search REST API](http://msdn.microsoft.com/library/dn798935.aspx)

## Azure Active Directory の再試行ガイドライン

Azure Active Directory (AD) は、コアなディレクトリ サービス、拡張 ID 制御、セキュリティ、およびアプリケーション アクセス管理を結合した、包括的な ID 管理とアクセス管理のクラウド ソリューションです。 Azure AD は、一元化されたポリシーとルールに基づいてアプリケーションへのアクセス制御を実現するための、ID 管理プラットフォームを開発者に提供します。

### 再試行メカニズム

Active Directory Authentication Library (ADAL) には、Azure Active Directory 用の組み込み再試行メカニズムはありません。 Active Directory によって返される例外用のカスタム検出メカニズムが含まれる再試行戦略を実装するには、一時的エラー処理アプリケーション ブロックを使用することができます。

### ポリシーの構成 (Azure Active Directory)

作成する Azure Active Directory で Transient Fault Handling Application Block を使用する場合、 **RetryPolicy** インスタンスを使用する検出戦略を定義するクラスに基づいています。

```csharp
var policy = new RetryPolicy<AdalDetectionStrategy>(new ExponentialBackoff(retryCount: 5,
                                                                     minBackoff: TimeSpan.FromSeconds(0),
                                                                     maxBackoff: TimeSpan.FromSeconds(60),
                                                                     deltaBackoff: TimeSpan.FromSeconds(2)));
```

次に呼び出し、 **ExecuteAction** または **ExecuteAsync** を実行する操作を渡して、再試行ポリシーのメソッドです。

```csharp
var result = await policy.ExecuteAsync(() => authContext.AcquireTokenAsync(resourceId, clientId, uc));
```

検出戦略クラスは、障害が発生したときに例外を受け取りますが、それが一時的なエラーまたは永続的なエラーのどちらの可能性があるかを確認する必要があります。 通常、これを行うために、例外の種類と状態コードを調べます。 たとえば、「サービス利用不可」応答は、再試行を行う必要があることを示します。 Transient Fault Handling Application Block には、ADAL クライアントでの使用に適した検出戦略クラスが含まれていませんが、カスタム検出戦略の例で提供されて、 [例](#examples-azure-active-directory-) 以下のセクションです。 カスタム検出戦略の使用は、ブロック (Topaz) で提供される戦略の使用と異なりません。

Transient Fault Handling Application Block の既定の戦略のセクションで示した [一時的なエラー処理アプリケーション ブロック (Topaz) 戦略](#transient-fault-handling-application-block-topaz-strategies) このガイダンスの末尾にします。

## 再試行使用のガイダンス

Azure Active Directory を使用する場合は、次のガイドラインについて検討します。

* Azure Active Directory 用の REST API を使用している場合、結果が 5xx の範囲内のエラー (500 内部サーバー エラー、502 無効なゲートウェイ、503 サービス利用不可、および 504 ゲートウェイ タイムアウトなど) である場合にのみ操作を再試行する必要があります。 その他のエラーの場合は、再試行しないでください。
* Active Directory Authentication Library (ADAL) を使用している場合は、HTTP コードは簡単にはアクセスできません。 ADAL 固有の例外のプロパティをチェックするロジックが含まれる、カスタム検出戦略を作成する必要があります。 参照してください、 [例](#examples-azure-active-directory-) 以下のセクションです。
* Azure Active Directory を使用するバッチのシナリオでは、指数バックオフ ポリシーの使用が推奨されています。

再試行操作について次の設定から始めることを検討します。 これらは汎用の設定であり、操作を監視して、独自のシナリオに合うように値を微調整する必要があります。


| **Context**          | **サンプルのターゲット E2E<br />最大待機時間** | **再試行戦略** | **[設定]**                                                          | **値**                 | **動作のしくみ**                                                                                                              |
|----------------------|----------------------------------------------|--------------------|-----------------------------------------------------------------------|----------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| 対話型、UI、<br />またはフォア グラウンド | 2 秒                                        | FixedInterval      | 再試行の回数<br />再試行の間隔<br />最初の高速再試行                           | 3<br />500 ミリ秒<br />true              | 試行 1-0 秒の遅延<br />試行 2-500 ミリ秒の遅延<br />試行 3-500 ミリ秒の遅延                                                     |
| バック グラウンドまたは<br />batch (バッチ)            | 60 秒                                       | ExponentialBackoff | 再試行の回数<br />最小バックオフ<br />最大バックオフ<br />デルタ バックオフ<br />最初の高速再試行 | 5<br />0 秒<br />60 秒<br />2 秒<br />false | 試行 1-0 秒の遅延<br />試行 2 - 最大 2 秒の遅延<br />試行 3 - 最大 6 秒の遅延<br />試行 4 - 最大 14 秒の遅延<br />試行 5 - 最大 30 秒の遅延 |

## 例 (Azure Active Directory)

次のコード例は、一時的エラー処理アプリケーション ブロック (Topaz) を使用して、ADAL のクライアントでの使用に適したカスタムの一時的エラー検出戦略を定義する方法を示しています。 コードが、新たに作成 **RetryPolicy** 型のカスタム検出戦略に基づいて **AdalDetectionStrategy**, コード リストで定義されている、します。 Topaz のカスタム検出戦略を実装、 **ITransientErrorDetectionStrategy** インターフェイスし、再試行を試す場合は true を返すか、 **false** かどうか、エラーが一時的でないと、再試行は試行されません。

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling;

    namespace RetryCodeSamples
    {
        class ActiveDirectoryCodeSamples
        {
            public async static Task Samples()
            {
                var authority = "[some authority]";
                var resourceId = “[some resource id]”;
                var clientId = “[some client id]”;

                var authContext = new AuthenticationContext(authority);

                var uc = new UserCredential(“[user]", "[password]");

                // Use Topaz with a custom detection strategy to manage retries.
                var policy =
                    new RetryPolicy<AdalDetectionStrategy>(
                        new ExponentialBackoff(
                            retryCount: 5,
                            minBackoff: TimeSpan.FromSeconds(0),
                            maxBackoff: TimeSpan.FromSeconds(60),
                            deltaBackoff: TimeSpan.FromSeconds(2)));

                var result = await policy.ExecuteAsync(() => authContext.AcquireTokenAsync(resourceId, clientId, uc));

                // Get the access token
                var accessToken = result.AccessToken;

                // Use the result, probably to authorize an API call.
            }
        }

        // TODO: This is sample code that needs validation from the WAAD team!
        // based on existing detection strategies
        public class AdalDetectionStrategy : ITransientErrorDetectionStrategy
        {
            private static readonly WebExceptionStatus[] webExceptionStatus =
                new[]
                {
                    WebExceptionStatus.ConnectionClosed,
                    WebExceptionStatus.Timeout,
                    WebExceptionStatus.RequestCanceled
                };

            private static readonly HttpStatusCode[] httpStatusCodes =
                new[]
                {
                    HttpStatusCode.InternalServerError,
                    HttpStatusCode.GatewayTimeout,
                    HttpStatusCode.ServiceUnavailable,
                    HttpStatusCode.RequestTimeout
                };

            public bool IsTransient(Exception ex)
            {
                var adalException = ex as AdalException;
                if (adalException == null)
                {
                    return false;
                }

                if (adalException.ErrorCode == AdalError.ServiceUnavailable)
                {
                    return true;
                }

                var innerWebException = adalException.InnerException as WebException;
                if (innerWebException != null)
                {
                    if (webExceptionStatus.Contains(innerWebException.Status))
                    {
                        return true;
                    }

                    if (innerWebException.Status == WebExceptionStatus.ProtocolError)
                    {
                        var response = innerWebException.Response as HttpWebResponse;
                        return response != null && httpStatusCodes.Contains(response.StatusCode);
                    }
                }

                return false;
            }
        }
    }

Active Directory Graph API 操作の再試行と、返されるエラー コードについては、次を参照してください。

* [コード サンプル: 再試行ロジック](http://msdn.microsoft.com/library/azure/dn448547.aspx)
* [Azure AD Graph のエラー コード](http://msdn.microsoft.com/library/azure/hh974480.aspx)

## 詳細情報

* [カスタム検出戦略を実装する](http://msdn.microsoft.com/library/hh680940.aspx) (Topaz)
* [再試行戦略のカスタム実装](http://msdn.microsoft.com/library/hh680943.aspx) (Topaz)
* [トークン発行と再試行ガイドライン](http://msdn.microsoft.com/library/azure/dn168916.aspx)

## 一般的な REST および再試行のガイドライン

Azure またはサード パーティ提供のサービスにアクセスする場合は、次の事柄を考慮してください。

* 再試行の管理に体系的な方法を使用し (再利用可能コードとするなど)、すべてのクライアントとすべてのソリューションの間で一貫性のある方式を適用できるようにします。
* 対象となるサービスまたはクライアントに組み込み再試行メカニズムがない場合の再試行を管理するために、一時的エラー処理アプリケーション ブロックなどの再試行フレームワークを使用することを検討します。 これは、一貫性のある再試行動作を実装するのに役立ち、ターゲットのサービスに適切な既定の再試行戦略を提供することができます。 ただし、一時的なエラーを示す例外に依存しない、非標準動作を持つサービスのカスタム再試行コードを作成する必要がありますか、使用する場合、 **再試行応答** 再試行動作を管理する応答します。
* 一時的なエラー検出ロジックは、REST 呼び出しの実行に使用する、実際のクライアント API によって異なります。 新しいなどの一部のクライアント **HttpClient** クラスでは、不成功 HTTP 状態コードで完了した要求に対する例外をスローしません。 これによりパフォーマンスは向上しますが、一時的エラー処理アプリケーション ブロックは使用できなくなります。 この場合、REST API への呼び出しを、非成功 HTTP 状態コードに対して例外を生成するコードでラップすることができます。こうすると、ブロック (Topaz) で処理できるようになります。 別の方法として、再試行を実施する別のメカニズムを使用することもできます。
* サービスから返される HTTP 状態コードは、エラーが一時的なものであるかどうかを知るのに役立ちます。 状態コードにアクセスするか、または同等の例外の種類を判別するには、クライアントまたは再試行フレームワークによって生成される例外を調べることが必要になる場合があります。 一般に、次の HTTP コードは、再試行が適切であることを示します。
  * 408 要求タイムアウト
  * 500 内部サーバー エラー
  * 502 無効なゲートウェイ
  * 503 サービス利用不可
  * 504 ゲートウェイ タイムアウト
* 再試行ロジックを例外に基づくものにしている場合、次のものは一般に、接続が確立できなかった一時的なエラーを示しています。
  * WebExceptionStatus.ConnectionClosed
  * WebExceptionStatus.ConnectFailure
  * WebExceptionStatus.Timeout
  * WebExceptionStatus.RequestCanceled
* サービス使用不可状態の場合は、サービスは、後に再試行する前に適切な遅延を示す可能性があります、 **Retry-after 後** 応答ヘッダーまたは別のカスタム ヘッダー (DocumentDB サービス) と同じです。 サービスは、カスタム ヘッダーとして、または応答の内容に埋め込んで、追加情報を送信することもあります。 一時的エラー処理アプリケーション ブロックは、標準またはカスタムの "retry-after" ヘッダーを使用することはできません。
* 408 要求タイムアウトを除き、クライアント エラー (4xx の範囲内のエラー) を表す状態コードに対しては再試行しないでください。
* 再試行戦略および再試行メカニズムは、多様なネットワーク状態やさまざまなシステム負荷などの幅広い条件下で十分にテストします。

## 再試行戦略

次に示すのは、標準的な種類の再試行戦略間隔です。

* **指数**: 再試行ポリシーをランダムな指数バックオフ アプローチを使用して、再試行の間隔を決定する再試行回数を指定します。 次に例を示します。

        var random = new Random();

        var delta = (int)((Math.Pow(2.0, currentRetryCount) - 1.0) *
                    random.Next((int)(this.deltaBackoff.TotalMilliseconds * 0.8),
                    (int)(this.deltaBackoff.TotalMilliseconds * 1.2)));
        var interval = (int)Math.Min(checked(this.minBackoff.TotalMilliseconds + delta),
                       this.maxBackoff.TotalMilliseconds);
        retryInterval = TimeSpan.FromMilliseconds(interval);

* **増分**: 指定回数の再試行回数と再試行の間隔ごとに時間の再試行戦略。 次に例を示します。

        retryInterval = TimeSpan.FromMilliseconds(this.initialInterval.TotalMilliseconds +
                       (this.increment.TotalMilliseconds * currentRetryCount));

* **LinearRetry**: 指定回数の再試行、再試行間の一定の時間を指定した期間を使用してを実行する再試行ポリシーです。 次に例を示します。

        retryInterval = this.deltaBackoff;

## 詳細情報

* [Circuit Breaker Pattern (サーキット ブレーカーのパターン)](http://msdn.microsoft.com/library/dn589784.aspx)

## 一時的エラー処理アプリケーション ブロック (Topaz) の戦略

一時的エラー処理アプリケーション ブロックには、次の既定の戦略があります。

| **戦略**            | **設定**                                         | **既定値**           | **意味**                                                                                                                                                                                                                                                                                 |
|-------------------------|-----------------------------------------------------|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Exponential**         | retryCount<br />minBackoff<br /><br />maxBackoff<br /><br />deltaBackoff<br /><br />fastFirstRetry   | 10<br />1 秒<br /><br />30 秒<br /><br />10 秒<br /><br />true | 再試行の回数。<br />最小バックオフ時間。 この値または計算されたバックオフの中で高い方は、再試行遅延として使用されます。<br />最小バックオフ時間。 この値または計算されたバックオフの下限は、再試行遅延として使用されます。<br />再試行間の指数遅延のランダムなデルタを計算するために使用する値。<br />最初の再試行を試みるかどうかをすぐに行うされます。 |
| **Incremental**         | retryCount<br />initialInterval<br />インクリメント<br /><br />fastFirstRetry<br />| 10<br />1 秒<br />1 秒<br /><br />true   | 再試行の回数。<br />初期間隔が適用される最初の再試行します。<br />再試行間の累進遅延を計算するために使用する増分時間値。<br />最初の再試行を試みるかどうかをすぐに行うされます。                                          |
| **Linear (固定間隔)** | retryCount<br />retryInterval<br />fastFirstRetry<br />             | 10<br />1 秒<br />true            | 再試行の回数。<br />再試行の遅延です。<br />最初の再試行を試みるかどうかをすぐに行うされます。                                                                                                                                                                              |
一時的エラー処理アプリケーション ブロックの使用例については、このガイダンスで前述されている、ADO.NET と Azure Active Directory を使用する Azure SQL Database についての「例」のセクションを参照してください。


