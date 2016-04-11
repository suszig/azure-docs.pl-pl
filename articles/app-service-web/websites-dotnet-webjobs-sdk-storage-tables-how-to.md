<properties 
    pageTitle="Web ジョブ SDK を使用して Azure テーブル ストレージを使用する方法" 
    description="Web ジョブ SDK で Azure テーブル ストレージを使用する方法について説明します。 テーブルを作成し、エンティティをテーブルに追加し、既存のテーブルを読み取ります。" 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/22/2015" 
    ms.author="tdykstra"/>

# Web ジョブ SDK を使用して Azure テーブル ストレージを使用する方法

## 概要

このガイドでは、c# コード サンプルを使用して Azure ストレージ テーブルを読み書きする方法を示す [web ジョブ SDK](websites-dotnet-webjobs-sdk.md) バージョン 1.x します。

このガイドがわかっていると想定しています。 [の Visual Studio で web ジョブ プロジェクトを作成する方法は、ストレージ アカウントにそのポイントを文字列](websites-dotnet-webjobs-sdk-get-started.md)します。
        
一部のコード スニペットの表示、 `Table` である関数で使用される属性 [は手動で呼び出す](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual), 、つまり、トリガー属性を使用してではなく。 

## <a id="ingress"></a> エンティティをテーブルに追加する方法

テーブルにエンティティを追加するには、`Table` 属性を `ICollector<T>` または `IAsyncCollector<T>` パラメーター (`T` は、追加するエンティティのスキーマ) に指定します。 この属性のコンストラクターは、テーブルの名前を指定する文字列パラメーターを受け取ります。 

次のコード サンプルでは追加 `Person` という名前のテーブルにエンティティ *受信*します。

        [NoAutomaticTrigger]
        public static void IngressDemo(
            [Table("Ingress")] ICollector<Person> tableBinding)
        {
            for (int i = 0; i < 100000; i++)
            {
                tableBinding.Add(
                    new Person() { 
                        PartitionKey = "Test", 
                        RowKey = i.ToString(), 
                        Name = "Name" }
                    );
            }
        }

`ICollector` には、`TableEntity` を継承する型または `ITableEntity` を実装する型を用いるのが一般的ですが、必須ではありません。 先ほど `Ingress` メソッドに示したコードは、次の 2 つの `Person` クラスのどちらでも正しく動作します。

        public class Person : TableEntity
        {
            public string Name { get; set; }
        }

        public class Person
        {
            public string PartitionKey { get; set; }
            public string RowKey { get; set; }
            public string Name { get; set; }
        }

Azure Storage API を直接操作する場合は、メソッド シグネチャに `CloudStorageAccount` パラメーターを追加することもできます。

## <a id="monitor"></a> リアルタイム監視

多くの場合、データの受信関数は大量のデータを処理するため、Web ジョブ SDK のダッシュボードはリアルタイムの監視データを提供します。  **Invocation Log** セクションでは、関数がまだ実行されているかどうかを説明します。

![実行中の受信関数](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressrunning.png)

 **Invocation Details** ページは、関数の進行状況 (書き込まれたエンティティの数) をレポート中が実行されていることを中止する機会が与えられます。 

![実行中の受信関数](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingressprogress.png)

関数が終了するときに、 **Invocation Details** ページが書き込まれた行の数を報告します。

![完了した受信関数](./media/websites-dotnet-webjobs-sdk-storage-tables-how-to/ingresssuccess.png)

## <a id="multiple"></a> テーブルから複数のエンティティを読み取る方法

テーブルの読み取りを行うには、`Table` 属性を `IQueryable<T>` パラメーター (`T` は、`TableEntity` を継承する型か、`ITableEntity` を実装する型) に指定します。

次のコード サンプルは、`Ingress` テーブルからすべての行を読み取り、ログに記録します。
 
        public static void ReadTable(
            [Table("Ingress")] IQueryable<Person> tableBinding,
            TextWriter logger)
        {
            var query = from p in tableBinding select p;
            foreach (Person person in query)
            {
                logger.WriteLine("PK:{0}, RK:{1}, Name:{2}", 
                    person.PartitionKey, person.RowKey, person.Name);
            }
        }

### <a id="readone"></a> テーブルから 1 つのエンティティを読み取る方法

1 つのテーブル エンティティにバインドする際に、パーティション キーと行キーを指定できる追加の 2 つのパラメーターを持つ `Table` 属性コンストラクターがあります。

次のコード サンプルは、キュー メッセージで受信したパーティション キー値と行キー値に基づいた `Person` エンティティのテーブル行を読み取ります。  

        public static void ReadTableEntity(
            [QueueTrigger("inputqueue")] Person personInQueue,
            [Table("persontable","{PartitionKey}", "{RowKey}")] Person personInTable,
            TextWriter logger)
        {
            if (personInTable == null)
            {
                logger.WriteLine("Person not found: PK:{0}, RK:{1}",
                        personInQueue.PartitionKey, personInQueue.RowKey);
            }
            else
            {
                logger.WriteLine("Person found: PK:{0}, RK:{1}, Name:{2}",
                        personInTable.PartitionKey, personInTable.RowKey, personInTable.Name);
            }
        }


この例の `Person` クラスは、`ITableEntity` を実装する必要はありません。

## <a id="storageapi"></a> .NET ストレージ API を直接使用して、テーブルを操作する方法

テーブルを操作する際に、`CloudTable` オブジェクトを使用して、より柔軟に `Table` 属性を使用することもできます。

次のコード例では、 `CloudTable` に 1 つのエンティティを追加するオブジェクト、 *受信* テーブルです。 
 
        public static void UseStorageAPI(
            [Table("Ingress")] CloudTable tableBinding,
            TextWriter logger)
        {
            var person = new Person()
                {
                    PartitionKey = "Test",
                    RowKey = "100",
                    Name = "Name"
                };
            TableOperation insertOperation = TableOperation.Insert(person);
            tableBinding.Execute(insertOperation);
        }

使用する方法の詳細についての `CloudTable` オブジェクトは、「 [.NET からテーブル ストレージを使用する方法](../storage-dotnet-how-to-use-tables.md)します。 

## <a id="queues"></a>キューのハウツー記事で紹介されている関連トピック

キュー メッセージによってトリガーされるテーブルの処理方法については、またはテーブル処理に固有ではない web ジョブ SDK のシナリオを参照してください。 [web ジョブ sdk Azure キュー ストレージを使用する方法](websites-dotnet-webjobs-sdk-storage-queues-how-to.md)します。 

その記事では、以下のようなトピックが紹介されています。

* Async 関数
* 複数のインスタンス
* グレースフル シャットダウン
* 関数本体での Web ジョブ SDK 属性の使用
* コード内での SDK 接続文字列の設定
* コードの Web ジョブ SDK コンストラクター パラメーター値の設定
* 手動での関数のトリガー
* ログの書き込み

## <a id="nextsteps"></a> 次のステップ

このガイドでは、Azure テーブルを操作するための一般的なシナリオの処理方法を示すコードのサンプルを提供しました。 Azure web ジョブおよび web ジョブ SDK を使用する方法の詳細については、次を参照してください。 [Azure WebJobs の推奨リソース](http://go.microsoft.com/fwlink/?linkid=390226)します。
 
