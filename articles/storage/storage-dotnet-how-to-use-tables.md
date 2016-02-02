<properties
    pageTitle=".NET から テーブル ストレージを使用する方法 | Microsoft Azure"
    description="Microsoft Azure Table ストレージを使用してテーブルを作成および削除する方法、さらにテーブル内のエンティティを挿入および照会する方法について説明します。"
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="adinah"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="08/04/2015"
    ms.author="tamram"/>



# .NET からテーブル ストレージを使用する方法

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

## 概要

この記事を使用して一般的なシナリオを実行する方法を示す、
Azure テーブル ストレージ サービスです。 サンプルは c \# コードで記述します。
Azure .NET 用 ストレージ クライアント ライブラリを利用しています。 紹介するシナリオは、テーブルの作成と
削除、テーブル エンティティの操作などです。

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-configure-connection-string-include](../../includes/storage-configure-connection-string-include.md)]

## プログラムでテーブル ストレージにアクセスする

[AZURE.INCLUDE [storage-dotnet-obtain-assembly](../../includes/storage-dotnet-obtain-assembly.md)]

### 名前空間宣言

プログラムを使用してオートスケーリング アプリケーション ブロックにアクセスする
プログラムを使用して Azure Storage にアクセスするのには、です。

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;

参照してください、 `Microsoft.WindowsAzure.Storage.dll` アセンブリ。

[AZURE.INCLUDE [storage-dotnet-retrieve-conn-string](../../includes/storage-dotnet-retrieve-conn-string.md)]

## テーブルの作成

A **CloudTableClient** オブジェクトでは、テーブルの参照オブジェクトを取得できます。
参照オブジェクトを取得できます。 次のコード作成、 **CloudTableClient** オブジェクト
これを使用して新しいテーブルを作成します。 この記事ですべてのコードを想定しています
作成するアプリケーションは、Azure クラウド サービス プロジェクトと
Azure アプリケーションのサービス構成に格納されているストレージ接続文字列を使用することを前提としています。

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));
    
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    
    // Create the table if it doesn't exist.
    CloudTable table = tableClient.GetTableReference("people");
    table.CreateIfNotExists();

## エンティティをテーブルに追加する

エンティティから派生したカスタム クラスを使用して c \# オブジェクト
**TableEntity**します。 エンティティをテーブルに追加するには、
エンティティのプロパティを定義するクラスを作成します。 次のコードは、
ユーザーの名を行キーとして、姓をパーティション キーとしてそれぞれ使用する
エンティティ クラスを定義します。 エンティティのパーティション キーと
行キーの組み合わせで、テーブル内のエンティティを一意に識別します。 同じパーティション キーを持つエンティティは、
異なるパーティション キーを持つエンティティよりも迅速に照会できます。
パーティション キーが、多様なパーティション キーを使用して並列操作の拡張性が向上できます。 テーブル サービスに保存するかを任意のプロパティの
プロパティは両方を公開するサポートされている型のパブリック プロパティである必要があります `取得` と `設定`します。
また、エンティティ型で、パラメーターのないコンストラクターを*必ず*公開する必要があります。

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

使用してエンティティに関連するテーブル操作が実行されて、 **CloudTable**
「"テーブルを作成する」で作成したオブジェクトです。 実行する操作は、
によって表される、 **TableOperation** オブジェクトです。 次のコード例では、まず **CloudTable** オブジェクトを作成し、次に **CustomerEntity** オブジェクトを作成します。 その後、操作を準備するために、ユーザー エンティティをテーブルに挿入する **TableOperation** オブジェクトを作成します。 最後に、**CloudTable.Execute** を呼び出して操作を実行します。

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("StorageConnectionString"));
    
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    
    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";
    
    // Create the TableOperation object that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);
    
    // Execute the insert operation.
    table.Execute(insertOperation);

## エンティティのバッチを挿入する

1 回の書き込み操作でエンティティのバッチをテーブルに
操作に渡すことはできません。 バッチ操作に関しては、次の事項にも
留意してください。

-  更新、削除、および挿入を同じ 1 回のバッチ操作で実行できます。
-  1 つのバッチ操作には、最大 100 個のエンティティを含めることができます。
-  1 つのバッチ操作に含まれるすべてのエンティティのパーティション キーが
    同じである必要があります。
-  クエリをバッチ操作として実行することもできますが、バッチ内の唯一の操作である必要があります。


次のコード例は、2 つのエンティティ オブジェクトを作成して、
**TableBatchOperation** を使用して、 **挿入** メソッドです。 その後、**CloudTable.Execute** を呼び出して操作を実行します。

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));
    
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    
    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
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
    table.ExecuteBatch(batchOperation);

## パーティション内のすべてのエンティティを取得する

テーブルに対してパーティション内のすべてのエンティティを照会する場合は、**TableQuery** オブジェクトを使用します。
次のコード例は、'Smith' がパーティション キーであるエンティティに
対してフィルターを指定します。 この例は、クエリ結果の各エンティティの
フィールドをコンソールに出力します。

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));
    
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    
    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    
    // Print the fields for each customer.
    foreach (CustomerEntity entity in table.ExecuteQuery(query))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## パーティション内の一定範囲のエンティティを取得する

パーティション内の一部のエンティティのみを照会する場合は、
パーティション キー フィルターと行キー フィルターを組み合わせて範囲を指定できます。 次のコード例は、
2 つのフィルターを使用して、行キー (名) がアルファベットの "E" より
前の文字で始まる、"Smith" というパーティション内のすべてのエンティティを
取得し、クエリ結果を出力します。

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));
    
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    
    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
    // Create the table query.
    TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));
    
    // Loop through the results, displaying information about the entity.
    foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
    {
        Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
    }

## 単一のエンティティを取得する

単一の特定のエンティティを取得するクエリを記述することができます。 この
次のコードでは使用 **TableOperation** "Ben Smith"というユーザーを指定します。
このメソッドが戻るエンティティではなく、
コレクション、および戻り値で **TableResult.Result** は、 **CustomerEntity** オブジェクトです。
クエリでパーティション キーと行キーの両方を指定することが、テーブル サービスから
単一のエンティティを取得するための最速の方法です。

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));
    
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    
    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    
    // Execute the retrieve operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);
    
    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## エンティティを置換する

エンティティを更新するテーブル サービスから取得し、変更、
エンティティ オブジェクトと、変更をテーブル サービスに戻して保存します。 この
既存のユーザーの電話番号を変更します。 このコードでは、
呼び出す **挿入**, 、このコードでは
**置換**します。 これにより、サーバーでエンティティが完全に置換されます。
ただし、エンティティを取得した後にサーバーでエンティティが変更された場合は、
操作が失敗します。 このエラーは、取得と更新の間にアプリケーションの
別のコンポーネントによって行われた変更が意図せず上書きされるのを防ぐための
ものです。 このエラーを正しく処理するには、
もう一度エンティティを取得し、変更を加えて (その変更がまだ有効な場合)、
別の **置換** 操作します。 次のセクションでは、
この動作をオーバーライドする方法を説明します。

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));
    
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    
    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    
    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);
    
    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;
    
    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-0105";
    
       // Create the InsertOrReplace TableOperation.
       TableOperation updateOperation = TableOperation.Replace(updateEntity);
    
       // Execute the operation.
       table.Execute(updateOperation);
    
       Console.WriteLine("Entity updated.");
    }
    
    else
       Console.WriteLine("Entity could not be retrieved.");

## エンティティの挿入または置換を行う

**置換** からエンティティが変更されている操作は失敗
変更されていると失敗します。 さらに、
最初の順序でサーバーからエンティティ、 **置換** 操作を成功させます。
しかし、サーバーにエンティティが存在するかどうかわからないが、
それに格納されている現在の値は、関連します。 ただし、更新する必要があります。
ある場合もあります。 これを実現するには、使用、 **InsertOrReplace**
操作に渡すことはできません。 この操作は、最終更新日時に関係なく、エンティティが
存在しない場合は挿入し、存在する場合は置換します。 [
次のコード例は、Ben Smith のユーザー エンティティを取得していますを使用してサーバーに保存し、 **InsertOrReplace**します。 そのため、
行われた操作ができるように、取得と更新の間でエンティティ
上書きされます。

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));
    
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    
    // Create the CloudTable object that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    
    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);
    
    // Assign the result to a CustomerEntity object.
    CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;
    
    if (updateEntity != null)
    {
       // Change the phone number.
       updateEntity.PhoneNumber = "425-555-1234";
    
       // Create the InsertOrReplace TableOperation.
       TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(updateEntity);
    
       // Execute the operation.
       table.Execute(insertOrReplaceOperation);
    
       Console.WriteLine("Entity was updated.");
    }
    
    else
       Console.WriteLine("Entity could not be retrieved.");

## エンティティ プロパティのサブセットを照会する

テーブル クエリでは、エンティティのすべてのプロパティではなくごくわずかのプロパティだけをエンティティから取得できます。 プロジェクションと呼ばれるこの方法では、帯域幅の使用が削減され、クエリのパフォーマンスが向上します。特に、大量のエンティティがある場合に役立ちます。 次のコードのクエリは、
テーブル内のエンティティの電子メール アドレスだけを
テーブルに挿入します。 クエリを使用して、これは、 **DynamicTableEntity** と
また **EntityResolver**します。 学びますプロジェクションの詳細について、 [Introducing Upsert とクエリ プロジェクションのブログの投稿][]します。 プロジェクションはローカル ストレージ エミュレーターではサポートされていません。したがって、このコードは Table サービスのアカウントを使用している場合にのみ機能します。

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));
    
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    
    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
    // Define the query, and select only the Email property.
    TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });
    
    // Define an entity resolver to work with the entity after retrieval.
    EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;
    
    foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
    {
        Console.WriteLine(projectedEmail);
    }

## エンティティを削除する

同じパターンを使用して、それを取得した後、簡単にエンティティを削除することができます。
同じパターンを使用します。 次のコードは、
コードでは、ユーザー エンティティを取得して削除します。

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));
    
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    
    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    
    // Execute the operation.
    TableResult retrievedResult = table.Execute(retrieveOperation);
    
    // Assign the result to a CustomerEntity.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;
    
    // Create the Delete TableOperation.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
    
       // Execute the operation.
       table.Execute(deleteOperation);
    
       Console.WriteLine("Entity deleted.");
    }
    
    else
       Console.WriteLine("Could not retrieve the entity.");

## テーブルを削除する

最後に、次のコード例は、ストレージ アカウントからテーブルを削除します。 A
削除されたテーブルはの再作成することはできません、
再作成できなくなります。

    // Retrieve the storage account from the connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));
    
    // Create the table client.
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    
    // Create the CloudTable that represents the "people" table.
    CloudTable table = tableClient.GetTableReference("people");
    
    // Delete the table it if exists.
    table.DeleteIfExists();

## ページ内のエンティティを非同期的に取得する

多数のエンティティを読み取る場合、すべてが返されるまで待機するのではなく取得後に処理または表示するには、セグメント化されたクエリを使用してエンティティを取得できます。 この例は、大きな結果のセットが返されるのを待機している間に実行がブロックされないように、Async-Await パターンを使用して結果をページに返す方法を示しています。 .NET での Async-await パターンの使用の詳細については、次を参照してください。 [Async と Await (c# および Visual Basic) を使用した非同期プログラミング](https://msdn.microsoft.com/library/hh191443.aspx)します。

    // Initialize a default TableQuery to retrieve all the entities in the table.
    TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();
    
    // Initialize the continuation token to null to start from the beginning of the table.
    TableContinuationToken continuationToken = null;
    
    do
    {
        // Retrieve a segment (up to 1,000 entities).
        TableQuerySegment<CustomerEntity> tableQueryResult =
            await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);
    
        // Assign the new continuation token to tell the service where to
        // continue on the next iteration (or null if it has reached the end).
        continuationToken = tableQueryResult.ContinuationToken;
    
        // Print the number of rows retrieved.
        Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);
    
    // Loop until a null continuation token is received, indicating the end of the table.
    } while(continuationToken != null);

## 次のステップ

これで、テーブル ストレージの基本を学習できました。
さらに複雑なストレージ タスクについて説明します。

- 利用可能な API の詳細については、テーブル サービスのリファレンス ドキュメントを参照してください。
    - [参照を .NET 用ストレージ クライアント ライブラリ](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST API リファレンス](http://msdn.microsoft.com/library/azure/dd179355)
- Azure Storage、を使用して、作業に記述するコードを簡略化する方法を学習 [Azure web ジョブ SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- Azure でデータを格納するための追加のオプションについては、他の機能ガイドも参照してください。
    - 使用 [Blob ストレージ](storage-dotnet-how-to-use-blobs.md) 非構造化データを格納します。
    - 使用 [SQL Database](sql-database-dotnet-how-to-use.md) リレーショナル データを格納します。


[download and install the azure sdk for .net]: /develop/net/ 
[creating an azure project in visual studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx 
[blob5]: ./media/storage-dotnet-how-to-use-table-storage/blob5.png 
[blob6]: ./media/storage-dotnet-how-to-use-table-storage/blob6.png 
[blob7]: ./media/storage-dotnet-how-to-use-table-storage/blob7.png 
[blob8]: ./media/storage-dotnet-how-to-use-table-storage/blob8.png 
[blob9]: ./media/storage-dotnet-how-to-use-table-storage/blob9.png 
[introducing upsert and query projection blog post]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx 
[.net client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409 
[azure storage team blog]: http://blogs.msdn.com/b/windowsazurestorage/ 
[configure azure storage connection strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx 
[odata]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2 
[edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2 
[spatial]: http://nuget.org/packages/System.Spatial/5.0.2 
[how to: programmatically access table storage]: #tablestorage 

