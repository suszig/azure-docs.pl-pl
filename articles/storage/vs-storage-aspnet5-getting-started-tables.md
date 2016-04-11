<properties
    pageTitle="テーブル ストレージと Visual Studio 接続済みサービスの概要 (ASP.NET 5) | Microsoft Azure"
    description="Visual Studio 接続済みサービスを使用してストレージ アカウントに接続した後で、Visual Studio の ASP.NET 5 プロジェクトで Azure テーブル ストレージの使用を開始する方法について説明します。"
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

# Azure テーブル ストレージと Visual Studio 接続済みサービスの概要

## 概要

この記事で説明する方法を作成または Visual Studio を使用して ASP.NET 5 プロジェクトで Azure ストレージ アカウントを参照した後は、Visual Studio で Azure テーブル ストレージを使用して開始 **[接続済みサービス** ダイアログ。

Azure テーブル ストレージ サービスを使用すると、大量の構造化データを格納できるようになります。 このサービスは、Azure クラウドの内部および外部からの認証された呼び出しを受け付ける NoSQL データストアです。 Azure のテーブルは、構造化された非リレーショナル データを格納するのに最適です。

 **[接続済みサービス** 操作が、プロジェクト内の Azure ストレージにアクセスする適切な NuGet パッケージをインストールし、プロジェクトの構成ファイルをストレージ アカウントの接続文字列を追加します。

Azure テーブル ストレージの使用に関する一般的な情報を参照してください。 [.NET からテーブル ストレージを使用する方法](storage-dotnet-how-to-use-tables.md)します。

最初に、ストレージ アカウント内にテーブルを作成する必要があります。 コードで Azure テーブルを作成する方法を説明します。 テーブルおよびエンティティの基本的な操作についても説明します (テーブル エンティティの追加、変更、読み取りなど)。 例は C\# のコードで記述され、Azure .NET 用ストレージ クライアント ライブラリを利用しています。

**注** -ASP.NET 5 で Azure storage への呼び出しを実行する Api の一部は非同期です。 参照してください [Async および Await を使用した非同期プログラミング](http://msdn.microsoft.com/library/hh191443.aspx) の詳細。 次のコードでは、非同期のプログラミング方法を使用していることを前提としています。

## コードでテーブルにアクセスする

ASP.NET 5 プロジェクト内のテーブルにアクセスするには、Azure テーブル ストレージにアクセスする C# ソース ファイルに、次の項目を含める必要があります。

1. C# ファイルの上部にある名前空間の宣言には、これらが含まれているかどうかを確認 **を使用して** ステートメントです。

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. 取得、 **CloudStorageAccount** をストレージ アカウント情報を表すオブジェクト。 次のコードを使用して、Azure サービス構成からストレージ接続文字列とストレージ アカウント情報を取得できます。

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **注** -すべてのコードの前に上記のコードを次のサンプルで使用しています。

3. 取得、 **CloudTableClient** 、ストレージ アカウント内のテーブル オブジェクトを参照するオブジェクト。  

        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

4. 取得、 **CloudTable** 、特定のテーブルとエンティティを参照するオブジェクトを参照します。

        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## コードでテーブルを作成する

Azure のテーブルを作成するへの呼び出しを追加 **CreateIfNotExistsAsync()**します。

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## エンティティをテーブルに追加する

エンティティをテーブルに追加するには、エンティティのプロパティを定義するクラスを作成します。 次のコードと呼ばれるエンティティ クラスを定義する **CustomerEntity** 行キーとして、姓、名、ユーザーのパーティション キーとして使用します。

    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }

        public string PhoneNumber { get; set; }
    }

エンティティに関連するテーブル操作が実行を使用して、 **CloudTable** 「でテーブルにアクセス コードです」で作成したオブジェクト。  **TableOperation** オブジェクトは、実行する操作を表します。 次のコード例を作成する方法を示しています、 **CloudTable** オブジェクトと **CustomerEntity** オブジェクトです。 操作を準備する、 **TableOperation** テーブルに顧客エンティティを挿入するを作成します。 最後に、CloudTable.ExecuteAsync を呼び出して操作を実行します。

    // Get a reference to the CloudTable object named 'peopleTable' as described in "Access a table in code"

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## エンティティのバッチを挿入する

1 回の書き込み操作で複数のエンティティをテーブルに挿入できます。 次のコード例は、2 つのエンティティ オブジェクト ("Jeff Smith"と"Ben Smith") を作成に追加されます、 **TableBatchOperation** オブジェクトを使用して、 **挿入** メソッド、CloudTable.ExecuteBatchAsync を呼び出して、操作を開始します。

    // Get a reference to a CloudTable object named 'peopleTable' as described in "Access a table in code"

    // Create the batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a customer entity and add it to the table.
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";
    customer1.PhoneNumber = "425-555-0104";

    // Create another customer entity and add it to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    customer2.PhoneNumber = "425-555-0102";

    // Add both customer entities to the batch insert operation.
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);

    // Execute the batch operation.
    await peopleTable.ExecuteBatchAsync(batchOperation);

## パーティション内のすべてのエンティティを取得する
パーティション内のエンティティのすべてのテーブルを照会する、 **TableQuery** オブジェクトです。 次のコード例は、'Smith' がパーティション キーであるエンティティに対してフィルターを指定します。 この例は、クエリ結果の各エンティティのフィールドをコンソールに出力します。

    // Get a reference to a CloudTable object named 'peopleTable' as described in "Access a table in code"

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

    return View();


## 単一のエンティティを取得する
単一の特定のエンティティを取得するクエリを記述することができます。 次のコードでは、 **TableOperation** "Ben Smith"という名前のユーザーを指定するオブジェクト。 このメソッドは、単一のエンティティではなくコレクション、および返された値を返します **TableResult.Result** は、 **CustomerEntity** オブジェクトです。 単一のエンティティを取得する最も簡単な方法は、クエリでパーティションと行キーの両方を指定する、 **テーブル** サービスです。

    // Get a reference to a CloudTableobject named 'peopleTable' as described in "Access a table in code"

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## エンティティを削除する
エンティティは、検索して削除できます。 次のコードは、"Ben Smith" という名前のユーザー エンティティを検索し、見つかったら削除します。

    // Get a reference to a CloudTableobject named 'peopleTable' as described in "Access a table in code"

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## 次のステップ

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]

