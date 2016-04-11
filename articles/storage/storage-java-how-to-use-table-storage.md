<properties 
    pageTitle="Java からテーブル ストレージを使用する方法 | Microsoft Azure" 
    description="Azure でテーブル ストレージ サービスを使用する方法について説明します。 コード サンプルは Java で記述されています。" 
    services="storage" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="12/01/2015" 
    ms.author="robmcm"/>


# Java からテーブル ストレージを使用する方法

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

## 概要

このガイドでは、Azure Table ストレージ サービスを使用して一般的なシナリオを実行する方法について説明します。 サンプルが使用して Java で記述され、 [Azure Storage SDK for Java][]します。 紹介するシナリオ **を作成する**, 、**を一覧表示する**, と **を削除する** テーブルだけでなく **を挿入する**, 、**クエリを実行する**, 、**を変更する**, と **を削除する** テーブル内のエンティティです。 テーブルの詳細については、次を参照してください。、 [次のステップ](#NextSteps) セクションです。

注: SDK は、Android デバイスで Azure Storage を使用する開発者向けに用意されています。 詳細については、次を参照してください。、 [Azure Storage SDK for Android][]します。 

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## Java アプリケーションの作成

このガイドで使用するストレージ機能は、Java アプリケーション内でローカルで実行することも、Azure の Web ロールまたは worker ロールで動作するコード内で実行することもできます。

そのためには、Java Development Kit (JDK) をインストールし、Azure サブスクリプションに Azure ストレージ アカウントを作成する必要があります。 これを実行して後、は、開発システムが最小要件およびに示されている依存関係を満たしていることを確認する必要があります。、 [Azure Storage SDK for Java][] GitHub のリポジトリです。 システムがそれらの要件を満たしている場合は、指示に従って、そのリポジトリからシステムに Azure Storage Libraries for Java をダウンロードしてインストールできます。 それらのタスクが完了したら、この記事の例を使用した Java アプリケーションを作成できます。

## テーブル ストレージにアクセスするようにアプリケーションを構成する

Microsoft Azure ストレージ API を使用してテーブルにアクセスする Java ファイルの先頭には、次の import ステートメントを追加します。

    // Include the following imports to use table APIs
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.table.*;
    import com.microsoft.azure.storage.table.TableQuery.*;

## Azure のストレージ接続文字列の設定

Azure ストレージ クライアントでは、ストレージ接続文字列を使用して、データ管理サービスにアクセスするためのエンドポイントおよび資格情報を保存します。 クライアント アプリケーションで実行するときに必要ストレージ接続文字列は、次の形式に、ストレージ アカウントの名前を使用して、ストレージ アカウントのプライマリ アクセス キーに表示、 [Azure ポータル](portal.azure.com) の *AccountName* と *AccountKey* 値。 この例では、接続文字列を保持する静的フィールドを宣言する方法を示しています。

    // Define the connection-string with your values.
    public static final String storageConnectionString = 
        "DefaultEndpointsProtocol=http;" + 
        "AccountName=your_storage_account;" + 
        "AccountKey=your_storage_account_key";

Microsoft Azure ロール内で実行されるアプリケーション、この文字列で、サービスの構成ファイルに保存することができます *ServiceConfiguration.cscfg*, への呼び出しでアクセスできると、 **RoleEnvironment.getConfigurationSettings** メソッドです。 接続文字列を取得する例をここでは、 **設定** という名前の要素 *StorageConnectionString* サービス構成ファイルで。

    // Retrieve storage account from connection-string.
    String storageConnectionString = 
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

次のサンプルでは、これら 2 つのメソッドのいずれかを使用してストレージ接続文字列を取得するとします。

## 方法: テーブルを作成する

A **CloudTableClient** オブジェクトでは、テーブルの参照オブジェクトを取得できます。
参照オブジェクトを取得できます。 次のコード作成、 **CloudTableClient** オブジェクト
新規作成に使用して **CloudTable** "people"という名前のテーブルを表すオブジェクト。 (注: 作成する方法もありますがある **CloudStorageAccount** のオブジェクト詳細については、次を参照してください。 **CloudStorageAccount** で、 [Azure Storage Client SDK Reference]。)。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create the table if it doesn't exist.
       String tableName = "people";
       CloudTable cloudTable = new CloudTable(tableName,tableClient);
       cloudTable.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: テーブルを一覧表示する

テーブルの一覧を取得する、 **CloudTableClient.listTables()** 、反復可能なテーブル名のリストを取得します。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Loop through the collection of table names.
        for (String table : tableClient.listTables())
        {
          // Output each table name.
          System.out.println(table);
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: エンティティをテーブルに追加する

エンティティが実装するカスタム クラスを使用して Java オブジェクトにマップ **TableEntity**します。 便宜上、 **TableServiceEntity** クラスが実装する **TableEntity** し、リフレクションを使用してプロパティをプロパティの名前が付いた getter および setter メソッドにマップします。 エンティティをテーブルに追加するには、最初に、エンティティのプロパティを定義するクラスを作成します。 次のコードは、ユーザーの名を行キーとして、姓をパーティション キーとしてそれぞれ使用するエンティティ クラスを定義します。 エンティティのパーティション キーと行キーの組み合わせで、テーブル内のエンティティを一意に識別します。 同じパーティション キーを持つエンティティは、異なるパーティション キーを持つエンティティよりも迅速に照会できます。

    public class CustomerEntity extends TableServiceEntity {
        public CustomerEntity(String lastName, String firstName) {
            this.partitionKey = lastName;
            this.rowKey = firstName;
        }

        public CustomerEntity() { }

        String email;
        String phoneNumber;
        
        public String getEmail() {
            return this.email;
        }
        
        public void setEmail(String email) {
            this.email = email;
        }
        
        public String getPhoneNumber() {
            return this.phoneNumber;
        }
        
        public void setPhoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
        }
    }

エンティティに関連するテーブル操作が必要な **TableOperation** オブジェクトです。 このオブジェクトで実行でき、エンティティに対して実行する操作を定義する、 **CloudTable** オブジェクトです。 次のコードの新しいインスタンスを作成する、 **CustomerEntity** 顧客データを格納するクラス。 次のコード呼び出し **TableOperation.insertOrReplace** を作成する、 **TableOperation** オブジェクトをテーブルにエンティティを挿入し、新しい **CustomerEntity** 関連付けします。 最後に、コードが呼び出され、 **実行** メソッドを **CloudTable** "people"テーブルと新しいを指定するオブジェクト **TableOperation**, 、その後は、新しいユーザー エンティティを"people"テーブルに挿入するかが既に存在する場合にエンティティを置換するには、ストレージ サービスに要求を送信します。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();
            
        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");
            
        // Create a new customer entity.
        CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
        customer1.setEmail("Walter@contoso.com");
        customer1.setPhoneNumber("425-555-0101");
            
        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer1);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: エンティティのバッチを挿入する

1 回の書き込み操作でエンティティのバッチをテーブル サービスに挿入できます。 次のコード作成、 **TableBatchOperation** オブジェクト、3 つの挿入操作を追加します。 新しいエンティティ オブジェクトを作成し、その値を設定しを呼び出すことによって各挿入操作を追加、 **挿入** メソッドを **TableBatchOperation** にエンティティを新しい挿入操作に関連付けるオブジェクト。 コードの呼び出し **実行** 上、 **CloudTable** "people"テーブルを指定するオブジェクトと **TableBatchOperation** オブジェクトで、テーブル操作のバッチを単一の要求でストレージ サービスに送信します。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Define a batch operation.
        TableBatchOperation batchOperation = new TableBatchOperation();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a customer entity to add to the table.
        CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
        customer.setEmail("Jeff@contoso.com");
        customer.setPhoneNumber("425-555-0104");
        batchOperation.insertOrReplace(customer);

       // Create another customer entity to add to the table.
       CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
       customer2.setEmail("Ben@contoso.com");
       customer2.setPhoneNumber("425-555-0102");
       batchOperation.insertOrReplace(customer2);

       // Create a third customer entity to add to the table.
       CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
       customer3.setEmail("Denise@contoso.com");
       customer3.setPhoneNumber("425-555-0103");
       batchOperation.insertOrReplace(customer3);

       // Execute the batch of operations on the "people" table.
       cloudTable.execute(batchOperation);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

バッチ操作に関しては、次の事項にも留意してください。

- 最大 100 個の挿入、削除、マージ、置換、挿入/マージ、挿入/置換の操作を任意の組み合わせで 1 つのバッチで実行できます。
- 取得をバッチ操作として実行することもできますが、バッチ内の唯一の操作であることが必要です。
- 1 つのバッチ操作に含まれるすべてのエンティティのパーティション キーが同じである必要があります。
- バッチ操作のデータ ペイロードは 4 MB に制限されています。

## 方法: パーティション内のすべてのエンティティを取得する

テーブルに対してパーティション内のエンティティを照会するには、使用することができます、 **TableQuery**します。 呼び出す **TableQuery.from** を指定した結果の型を返す特定のテーブルに対するクエリを作成します。 次のコードは、'Smith' がパーティション キーであるエンティティに対してフィルターを指定します。 **TableQuery.generateFilterCondition** クエリのフィルターを作成するヘルパー メソッドです。 呼び出す **、** によって返される参照で、 **TableQuery.from** クエリにフィルターを適用するメソッドです。 呼び出して、クエリの実行時に **実行** 上、 **CloudTable** オブジェクトを返します、 **反復子** で、 **CustomerEntity** 指定された型の結果します。 使用して、 **反復子** で返されるの結果を処理する for each ループです。 このコードは、クエリ結果の各エントリのフィールドをコンソールに出力します。

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";

        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();
            
       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY, 
           QueryComparisons.EQUAL,
           "Smith");

       // Specify a partition query, using "Smith" as the partition key filter.
       TableQuery<CustomerEntity> partitionQuery =
           TableQuery.from(CustomerEntity.class)
           .where(partitionFilter);

        // Loop through the results, displaying information about the entity.
        for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() + 
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: パーティション内の一定範囲のエンティティを取得する

パーティション内の一部のエンティティのみ照会する場合は、フィルター内で比較演算子を使用して範囲を指定できます。 次のコードは、2 つのフィルターを組み合わせて、行キー (名) がアルファベットの "E" までの文字で始まる、"Smith" というパーティション内のすべてのエントリを取得します。 その後で、クエリ結果が出力されます。 このガイドのバッチ挿入に関するセクションでテーブルに追加したエンティティを使用すると、この場合は 2 つのエンティティ (Ben Smith と Denise Smith) だけが返されます。Jeff Smith は返されません。

    try
    {
        // Define constants for filters.
        final String PARTITION_KEY = "PartitionKey";
        final String ROW_KEY = "RowKey";
        final String TIMESTAMP = "Timestamp";
            
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the table client.
       CloudTableClient tableClient = storageAccount.createCloudTableClient();

       // Create a cloud table object for the table.
       CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a filter condition where the partition key is "Smith".
        String partitionFilter = TableQuery.generateFilterCondition(
           PARTITION_KEY, 
           QueryComparisons.EQUAL,
           "Smith");

        // Create a filter condition where the row key is less than the letter "E".
        String rowFilter = TableQuery.generateFilterCondition(
           ROW_KEY, 
           QueryComparisons.LESS_THAN,
           "E");

        // Combine the two conditions into a filter expression.
        String combinedFilter = TableQuery.combineFilters(partitionFilter, 
            Operators.AND, rowFilter);

        // Specify a range query, using "Smith" as the partition key,
        // with the row key being up to the letter "E".
        TableQuery<CustomerEntity> rangeQuery =
           TableQuery.from(CustomerEntity.class)
           .where(combinedFilter);

        // Loop through the results, displaying information about the entity
        for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
            System.out.println(entity.getPartitionKey() +
                " " + entity.getRowKey() +
                "\t" + entity.getEmail() +
                "\t" + entity.getPhoneNumber());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: 単一のエンティティを取得する

単一の特定のエンティティを取得するクエリを記述することができます。 次のコード呼び出し **TableOperation.retrieve** パーティション キーと行キー、顧客を指定するパラメーター"Jeff Smith"を作成する代わりに、 **TableQuery** と同じようにフィルターを使用します。 この取得操作が実行されると、1 つのコレクションではなく、1 つのエンティティのみ返されます。  **GetResultAsType** メソッドは、結果の割り当てのターゲット型にキャスト、 **CustomerEntity** オブジェクトです。 この型がクエリに指定した型と互換性がない場合は、例外がスローされます。 パーティション キーおよび行キーが正確に一致するエンティティがない場合は、null 値が返されます。 クエリでパーティション キーと行キーの両方を指定することが、Table サービスから単一のエンティティを取得するための最速の方法です。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
        TableOperation retrieveSmithJeff = 
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

       // Submit the operation to the table service and get the specific entity.
       CustomerEntity specificEntity =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();
            
        // Output the entity.
        if (specificEntity != null)
        {
            System.out.println(specificEntity.getPartitionKey() +
                " " + specificEntity.getRowKey() +
                "\t" + specificEntity.getEmail() +
                "\t" + specificEntity.getPhoneNumber());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: エンティティを変更する

エンティティを変更するには、そのエンティティをテーブル サービスから取得し、エンティティ オブジェクトに変更を加えて、その変更を置換またはマージ操作でテーブル サービスに戻して保存します。 次のコードは、既存のユーザーの電話番号を変更します。 呼び出す代わりに **TableOperation.insert** を挿入するように、このコードを呼び出す **TableOperation.replace**します。  **CloudTable.execute** メソッドはテーブル サービス呼び出しし、変更しない限り、別のアプリケーションに、時間でこのアプリケーションで取得した後、このエンティティが置き換えられます。 別のアプリケーションが変更を加えた場合は、例外がスローされるので、このエンティティを取得して変更し、もう一度保存する必要があります。 このオプティミスティック同時実行制御の再試行パターンは、分散したストレージ システムでは一般的です。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = 
           TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Submit the operation to the table service and get the specific entity.
        CustomerEntity specificEntity =
          cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Specify a new phone number.
        specificEntity.setPhoneNumber("425-555-0105");

        // Create an operation to replace the entity.
        TableOperation replaceEntity = TableOperation.replace(specificEntity);

        // Submit the operation to the table service.
        cloudTable.execute(replaceEntity);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: エンティティ プロパティのサブセットを照会する

テーブルに対するクエリでは、ごくわずかのプロパティだけをエンティティから取得できます。 プロジェクションと呼ばれるこの方法では、帯域幅の使用が削減され、クエリのパフォーマンスが向上します。特に、大量のエンティティがある場合に役立ちます。 次のコードでクエリを使用して、 **選択** をテーブルにエンティティの電子メール アドレスだけを返すメソッド。 結果はコレクションへの射影 **文字列** を利用して、 **EntityResolver**, 、サーバーから返されるエンティティの型変換が行われます。 このプロジェクションの詳細については、 [ブログの投稿][]します。 プロジェクションはローカル ストレージ エミュレーターではサポートされていません。したがって、このコードはテーブル サービスのアカウントを使用している場合にのみ機能します。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Define a projection query that retrieves only the Email property
        TableQuery<CustomerEntity> projectionQuery = 
           TableQuery.from(CustomerEntity.class)
           .select(new String[] {"Email"});

        // Define a Entity resolver to project the entity to the Email value.
        EntityResolver<String> emailResolver = new EntityResolver<String>() {
            @Override
            public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
                return properties.get("Email").getValueAsString();
            }
        };

        // Loop through the results, displaying the Email values.
        for (String projectedString : 
            cloudTable.execute(projectionQuery, emailResolver)) {
                System.out.println(projectedString);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: エンティティを挿入または置換する

エントリをテーブルに追加するときは、多くの場合、そのエントリがテーブル内に既に存在しているかどうかを把握していません。 エンティティの挿入または置換操作では、エンティティが存在しない場合にそのエンティティを挿入し、エンティティが存在する場合はその既存のエンティティを置き換えるという操作を 1 つの要求で処理することができます。 これまでの例に対して、次のコードは "Walter Harp" のエンティティを挿入または置換します。 このコードを呼び出す新しいエンティティを作成した後、 **TableOperation.insertOrReplace** メソッドです。 このコードを呼び出して **実行** 上、 **CloudTable** オブジェクト、テーブルと、挿入または置換テーブル操作をパラメーターとして。 エンティティの一部のみ更新する、 **TableOperation.insertOrMerge** メソッドを代わりに使用できます。 挿入または置換はローカル ストレージ エミュレーターではサポートされていません。したがって、このコードはテーブル サービスのアカウントを使用している場合にのみ機能します。 挿入または置換、挿入またはマージこの詳細を確認できます [ブログの投稿][]します。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create a new customer entity.
        CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
        customer5.setEmail("Walter@contoso.com");
        customer5.setPhoneNumber("425-555-0106");

        // Create an operation to add the new customer to the people table.
        TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

        // Submit the operation to the table service.
        cloudTable.execute(insertCustomer5);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: エンティティを削除する

エンティティは、取得後に簡単に削除できます。 エンティティを取得した後で呼び出す **TableOperation.delete** 、エンティティを削除するとします。 まず **実行** 上、 **CloudTable** オブジェクトです。 次のコードは、ユーザー エンティティを取得して削除します。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Create a cloud table object for the table.
        CloudTable cloudTable = tableClient.getTableReference("people");

        // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
        TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

        // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
        CustomerEntity entitySmithJeff =
            cloudTable.execute(retrieveSmithJeff).getResultAsType();

        // Create an operation to delete the entity.
        TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

        // Submit the delete operation to the table service.
        cloudTable.execute(deleteSmithJeff);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: テーブルを削除する

最後に、次のコードは、ストレージ アカウントからテーブルを削除します。 削除されたテーブルは、削除後の一定期間 (通常は 40 秒未満) は再作成できなくなります。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the table client.
        CloudTableClient tableClient = storageAccount.createCloudTableClient();

        // Delete the table and all its data if it exists.
        CloudTable cloudTable = new CloudTable("people",tableClient);
        cloudTable.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 次のステップ

これで、テーブル ストレージの基本を学習できました。さらに複雑なストレージ タスクを実行する方法については、次のリンク先を参照してください。

- [Azure Storage SDK for Java][]
- [Azure ストレージ クライアント SDK リファレンス][]
- [Azure Storage REST API][]
- [Azure のストレージ チーム ブログ][]

詳細については、「関連項目、 [Java デベロッパー センター](/develop/java/)します。


[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Storage Client SDK Reference]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[blog post]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
 
