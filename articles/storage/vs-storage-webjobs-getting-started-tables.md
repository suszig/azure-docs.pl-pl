<properties
    pageTitle="Azure Storage と Visual Studio 接続済みサービスの概要 (Web ジョブ プロジェクト)"
    description="Visual Studio 接続済みサービスを使用してストレージ アカウントに接続した後、Visual Studio の Azure Web ジョブ プロジェクトで Azure テーブル ストレージの使用を開始する方法について説明します。"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/16/2015"
    ms.author="tarcher"/>

# Azure Storage の使用 (Azure Web ジョブ プロジェクト)

## 概要

この記事では、Azure テーブル ストレージ サービスを使用して Azure Web ジョブ SDK バージョン 1.x を操作する方法について説明します。 コード サンプルを使用して、 [web ジョブ SDK](websites-dotnet-webjobs-sdk.md) バージョン 1.x します。

Azure テーブル ストレージ サービスを使用すると、大量の構造化データを格納できるようになります。 このサービスは、Azure クラウドの内部および外部からの認証された呼び出しを受け付ける NoSQL データストアです。 Azure のテーブルは、構造化された非リレーショナル データを格納するのに最適です。  参照してください [.NET からテーブル ストレージを使用する方法](storage-dotnet-how-to-use-tables.md/#create-a-table ".NET からテーブル ストレージを使用する方法") の詳細。


一部のコード スニペットの表示、 **テーブル** である関数で使用される属性 [は手動で呼び出す](vs-storage-webjobs-getting-started-blobs.md#manual), 、つまり、トリガー属性を使用してではなく。

## エンティティをテーブルに追加する方法

エンティティをテーブルに追加するには、使用、 **テーブル** 属性を **ICollector<T>** または **IAsyncCollector<T>** パラメーター、 **T** を追加するエンティティのスキーマを指定します。 この属性のコンストラクターは、テーブルの名前を指定する文字列パラメーターを受け取ります。

次のコード サンプルでは追加 **人** という名前のテーブルにエンティティ *受信*します。

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

通常、型を用いる **ICollector** から派生した **TableEntity** または実装する **ITableEntity**, 、する必要はありませんが、します。 次のいずれかの **人** クラスの動作を前に示したコード **受信** メソッドです。

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

Azure storage API を直接操作するかどうかを追加する、 **CloudStorageAccount** メソッド シグネチャのパラメーターです。

## リアルタイム監視

多くの場合、データの受信関数は大量のデータを処理するため、Web ジョブ SDK のダッシュボードはリアルタイムの監視データを提供します。  **Invocation Log** セクションでは、関数がまだ実行されているかどうかを説明します。

![実行中の受信関数](./media/vs-storage-webjobs-getting-started-tables/ingressrunning.png)

 **Invocation Details** ページは、関数の進行状況 (書き込まれたエンティティの数) をレポート中が実行されていることを中止する機会が与えられます。

![実行中の受信関数](./media/vs-storage-webjobs-getting-started-tables/ingressprogress.png)

関数が終了するときに、 **Invocation Details** ページが書き込まれた行の数を報告します。

![完了した受信関数](./media/vs-storage-webjobs-getting-started-tables/ingresssuccess.png)

## テーブルから複数のエンティティを読み取る方法

テーブルの読み取りを行う、 **テーブル** 属性を **IQueryable<T>** パラメーター入力場所 **T** から派生 **TableEntity** または実装する **ITableEntity**します。

次のコード サンプルは、読み取り、すべての行をログに記録、 **受信** テーブル。

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

### テーブルから 1 つのエンティティを読み取る方法

 **テーブル** 1 つのテーブル エンティティにバインドするときに、パーティション キーと行キーを指定できる 2 つのパラメーターを持つ属性のコンス トラクターです。

次のコード サンプルのテーブル行を読み取り、 **人** エンティティのパーティション キーと行キー値に基づいてキュー メッセージで受信します。  

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


 **人** のこの例のクラスが実装する必要はありません **ITableEntity**します。

## .NET ストレージ API を直接使用して、テーブルを操作する方法

使用することも、 **テーブル** 属性を **CloudTable** オブジェクトをより柔軟にテーブルを操作します。

次のコード例では、 **CloudTable** に 1 つのエンティティを追加するオブジェクト、 *受信* テーブルです。

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

使用する方法の詳細についての **CloudTable** オブジェクトは、「 [.NET からテーブル ストレージを使用する方法](../storage-dotnet-how-to-use-tables.md)します。

## キューのハウツー記事で紹介されている関連トピック

キュー メッセージによってトリガーされるテーブルの処理方法については、またはテーブル処理に固有ではない web ジョブ SDK のシナリオを参照してください。 [web ジョブ sdk Azure キュー ストレージを使用する方法](vs-storage-webjobs-getting-started-queues.md)します。



## 次のステップ

この記事では、Azure テーブルを操作するための一般的なシナリオの処理方法を示すコードのサンプルを提供しました。 Azure web ジョブおよび web ジョブ SDK を使用する方法の詳細については、次を参照してください。 [Azure WebJobs の推奨リソース](http://go.microsoft.com/fwlink/?linkid=390226)します。

