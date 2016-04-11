<properties
    pageTitle="Table ストレージを使用する方法 (C++) | Microsoft Azure"
    description="Azure で Table ストレージ サービスを使用する方法について説明します。 サンプルは C++ で記述されています。"
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="adinah"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/23/2015"
    ms.author="tamram"/>

# C++ から Table ストレージを使用する方法

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

## 概要  
このガイドでは、Azure テーブル ストレージ サービスを使用して一般的なシナリオを実行する方法について説明します。 サンプルは C++ および使用で記述され、 [C++ 用 Azure ストレージ クライアント ライブラリ](https://github.com/Azure/azure-storage-cpp/blob/v1.0.0/README.md)します。 紹介するシナリオ **を作成して、テーブルを削除して** と **テーブル エンティティの操作**します。

>[AZURE.NOTE] このガイドは、以降のバージョン 1.0.0 の C++ の場合、Azure ストレージ クライアント ライブラリを対象います。 推奨されるバージョンは、これはストレージ クライアント ライブラリ 1.0.0、 [NuGet](http://www.nuget.org/packages/wastorage) または [GitHub](https://github.com/)します。

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## C++ アプリケーションの作成  
このガイドでは、C++ アプリケーション内で実行できるストレージ機能を使用します。 そのためには、C++ 用 Azure ストレージ クライアント ライブラリをインストールし、Azure サブスクリプションに Azure ストレージ アカウントを作成する必要があります。  

C++ 用 Azure ストレージ クライアント ライブラリをインストールする場合、次の方法を使用できます。

-   **Linux:** に記載されている手順に従って、 [C++ README 用 Azure ストレージ クライアント ライブラリ](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) ページです。  
-   **Windows:** Visual Studio で、[ **ツール > NuGet パッケージ マネージャー > パッケージ マネージャー コンソール**します。 次のコマンドを入力、 [NuGet パッケージ マネージャー コンソール](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) Enter キーを押します。  

        Install-Package wastorage

## テーブル ストレージにアクセスするようにアプリケーションを構成する  
Azure Storage API を使用してテーブルにアクセスする C++ ファイルの先頭には、次の include ステートメントを追加します。  

    #include "was/storage_account.h"
    #include "was/table.h"

## Azure Storage 接続文字列の設定  
Azure ストレージ クライアントでは、ストレージ接続文字列を使用して、データ管理サービスにアクセスするためのエンドポイントおよび資格情報を保存します。 クライアント アプリケーションを実行する場合は、次の形式でストレージ接続文字列を入力する必要があります。 示されたストレージ アカウントのストレージ アカウントとストレージ アクセス キーの名前を使用して、 [Azure ポータル](portal.azure.com) の *AccountName* と *AccountKey* 値。 詳細については、ストレージ アカウントとアクセス キーは、次を参照してください。 [は Azure ストレージ アカウント](storage-create-storage-account.md)します。 この例では、接続文字列を保持する静的フィールドを宣言する方法を示しています。  

    // Define the connection string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

ローカルの Windows ベース コンピューターで、アプリケーションをテストするには、Azure を使用することができます [ストレージ エミュレーター](storage-use-emulator.md) と共にインストールされた、 [Azure SDK](http://azure.microsoft.com/downloads/)します。 ストレージ エミュレーターは、ローカルの開発マシンで、Azure BLOB、Queue、および Table サービスをシミュレートするユーティリティです。 次の例では、ローカルのストレージ エミュレーターに接続文字列を保持する静的フィールドを宣言する方法を示しています。  

    // Define the connection string with Azure storage emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Azure ストレージ エミュレーターを開始する場合、 **開始** クリックするか Windows キーを押します。 入力を開始 **Azure ストレージ エミュレーター**, 、し、[ **Microsoft Azure ストレージ エミュレーター** アプリケーションの一覧からです。  

次のサンプルでは、これら 2 つのメソッドのいずれかを使用してストレージ接続文字列を取得するとします。  

## 接続文字列の取得  
使用することができます、 **cloud_storage_account** をストレージ アカウント情報を表すクラス。 ストレージ接続文字列からストレージ アカウント情報を取得するには、parse メソッド使用します。

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

次への参照を取得、 **cloud_table_client** クラスではテーブルとテーブル ストレージ サービス内に格納されたエンティティの参照オブジェクトを取得できます。 次のコード作成、 **cloud_table_client** 前に取得したストレージ アカウント オブジェクトを使用してオブジェクト。  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

## テーブルを作成する
A **cloud_table_client** オブジェクトでは、テーブルとエンティティの参照オブジェクトを取得することができます。 次のコード作成、 **cloud_table_client** オブジェクトを使用して、新しいテーブルを作成します。

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();  

## エンティティをテーブルに追加する
テーブルにエンティティを追加するには、新しい作成 **table_entity** オブジェクトに渡すと **table_operation::insert_entity**します。 次のコードは、ユーザーの名を行キーとして、姓をパーティション キーとしてそれぞれ使用します。 エンティティのパーティション キーと行キーの組み合わせで、テーブル内のエンティティを一意に識別します。 同じパーティション キーを持つエンティティは、異なるパーティション キーを持つエンティティよりも迅速に照会できます。一方、多様なパーティション キーを使用すると、並列操作の拡張性が向上します。 詳細については、次を参照してください。 [Microsoft Azure ストレージのパフォーマンスとスケーラビリティのチェックリスト](storage-performance-checklist.md)します。

次のコードの新しいインスタンスを作成する **table_entity** 顧客データを格納します。 次のコード呼び出し **table_operation::insert_entity** を作成する、 **table_operation** オブジェクトをテーブルにエンティティを挿入し、新しいテーブル エンティティを関連付けます。 最後に、コードは execute メソッドを呼び出して、 **cloud_table** オブジェクトです。 新しい **table_operation** "people"テーブルに新しいユーザー エンティティを挿入するテーブル サービスに要求を送信します。  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Retrieve a reference to a table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table if it doesn't exist.
    table.create_if_not_exists();

    // Create a new customer entity.
    azure::storage::table_entity customer1(U("Harp"), U("Walter"));

    azure::storage::table_entity::properties_type& properties = customer1.properties();
    properties.reserve(2);
    properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

    properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

    // Create the table operation that inserts the customer entity.
    azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

    // Execute the insert operation.
    azure::storage::table_result insert_result = table.execute(insert_operation);

## エンティティのバッチを挿入する
1 回の書き込み操作でエンティティのバッチをテーブル サービスに挿入できます。 次のコード作成、 **table_batch_operation** オブジェクト、および 3 つの挿入操作に追加されます。 各挿入操作を追加して、新しいエンティティ オブジェクトを作成し、その値を設定しての insert メソッドを呼び出して、 **table_batch_operation** にエンティティを新しい挿入操作に関連付けるオブジェクト。 次に、 **cloud_table.execute** を呼び出して、操作を実行します。  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define a batch operation.
    azure::storage::table_batch_operation batch_operation;

    // Create a customer entity and add it to the table.
    azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

    azure::storage::table_entity::properties_type& properties1 = customer1.properties();
    properties1.reserve(2);
    properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
    properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

    // Create another customer entity and add it to the table.
    azure::storage::table_entity customer2(U("Smith"), U("Ben"));

    azure::storage::table_entity::properties_type& properties2 = customer2.properties();
    properties2.reserve(2);
    properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
    properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

    // Create a third customer entity to add to the table.
    azure::storage::table_entity customer3(U("Smith"), U("Denise"));

    azure::storage::table_entity::properties_type& properties3 = customer3.properties();
    properties3.reserve(2);
    properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
    properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

    // Add customer entities to the batch insert operation.
    batch_operation.insert_or_replace_entity(customer1);
    batch_operation.insert_or_replace_entity(customer2);
    batch_operation.insert_or_replace_entity(customer3);

    // Execute the batch operation.
    std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);

バッチ操作に関しては、次の事項にも留意してください。  

-   最大 100 個の挿入、削除、マージ、置換、挿入/マージ、挿入/置換の操作を任意の組み合わせで 1 つのバッチで実行できます。  
-   取得をバッチ操作として実行することもできますが、バッチ内の唯一の操作であることが必要です。  
-   1 つのバッチ操作に含まれるすべてのエンティティのパーティション キーが同じである必要があります。  
-   バッチ操作のデータ ペイロードは 4 MB に制限されています。  

## パーティション内のすべてのエンティティを取得する
テーブルに対してパーティション内のすべてのエンティティを照会する、 **table_query** オブジェクトです。 次のコード例は、'Smith' がパーティション キーであるエンティティに対してフィルターを指定します。 この例は、クエリ結果の各エンティティのフィールドをコンソールに出力します。  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Print the fields for each customer.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

この例のクエリでは、フィルター条件に一致するすべてのエンティティが表示されます。 テーブルの規模が大きく、そのテーブル エンティティを頻繁にダウンロードする必要がある場合は、Azure Storage BLOB にデータを保存することをお勧めします。

## パーティション内の一定範囲のエンティティを取得する
パーティション内の一部のエンティティのみを照会する場合は、パーティション キー フィルターと行キー フィルターを組み合わせて範囲を指定できます。 次のコード例は、2 つのフィルターを使用して、行キー (名) がアルファベットの 'E' より前の文字で始まる、'Smith' というパーティション内のすべてのエンティティを取得し、クエリ結果を出力します。  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create the table query.
    azure::storage::table_query query;

    query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
        azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
        azure::storage::query_comparison_operator::equal, U("Smith")),
        azure::storage::query_logical_operator::op_and,
        azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Loop through the results, displaying information about the entity.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        const azure::storage::table_entity::properties_type& properties = it->properties();

        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
            << U(", Property1: ") << properties.at(U("Email")).string_value()
            << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
    }  

## 単一のエンティティを取得する
単一の特定のエンティティを取得するクエリを記述することができます。 次のコードでは **table_operation::retrieve_entity** 、顧客 ' Jeff Smith' を指定します。 このメソッドは、コレクションではなく、1 つのエンティティを返し、返される値は **table_result**します。 クエリでパーティション キーと行キーの両方を指定することが、Table サービスから単一のエンティティを取得するための最速の方法です。  

    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Output the entity.
    azure::storage::table_entity entity = retrieve_result.entity();
    const azure::storage::table_entity::properties_type& properties = entity.properties();

    std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;

## エンティティを置換する
エンティティを置換するには、そのエンティティを Table サービスから取得し、エンティティ オブジェクトを変更して、変更を Table サービスに戻して保存します。 次のコードは、既存のユーザーの電話番号と電子メール アドレスを変更します。 呼び出す代わりに **table_operation::insert_entity**, 、このコードを実行して **table_operation::replace_entity**します。 これにより、サーバーでエンティティが完全に置換されます。ただし、エンティティを取得した後にサーバーでエンティティが変更された場合は、操作が失敗します。 このエラーは、取得と更新の間にアプリケーションの別のコンポーネントによって行われた変更が意図せず上書きされるのを防ぐためのものです。 このエラーの適切な処理エンティティを再度取得、(まだ有効である) 場合は、変更を行い、他を実行するには **table_operation::replace_entity** 操作します。 次のセクションでは、この動作をオーバーライドする方法を説明します。  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Replace an entity.
    azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
    properties_to_replace.reserve(2);

    // Specify a new phone number.
    properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

    // Specify a new email address.
    properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

    // Create an operation to replace the entity.
    azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result replace_result = table.execute(replace_operation);

## エンティティの挿入または置換を行う
**table_operation::replace_entity** サーバーから取得した後にエンティティが変更されている操作は失敗します。 さらに、必要があります、サーバーからエンティティを先に取得するため **table_operation::replace_entity** を成功させます。 しかし、サーバーにエンティティが存在するかどうかわからないが、現在格納されている値は不適切であるため、すべての値を上書きする必要がある場合もあります。 これを実現するには、使用、 **table_operation::insert_or_replace_entity** 操作します。 この操作は、最終更新日時に関係なく、エンティティが存在しない場合は挿入し、存在する場合は置換します。 次のコード例に Jeff Smith のユーザー エンティティを取得していますを使用してサーバーに保存し、 **table_operation::insert_or_replace_entity**します。 そのため、取得操作と更新操作の間に行われたエンティティの更新は上書きされます。  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Insert-or-replace an entity.
    azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
    azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

    properties_to_insert_or_replace.reserve(2);

    // Specify a phone number.
    properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

    // Specify an email address.
    properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

    // Create an operation to insert-or-replace the entity.
    azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

    // Submit the operation to the Table service.
    azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);

## エンティティ プロパティのサブセットを照会する  
テーブルに対するクエリでは、ごくわずかのプロパティだけをエンティティから取得できます。 次のコードでクエリを使用して、 **table_query::set_select_columns** をテーブルにエンティティの電子メール アドレスだけを返すメソッド。  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Define the query, and select only the Email property.
    azure::storage::table_query query;
    std::vector<utility::string_t> columns;

    columns.push_back(U("Email"));
    query.set_select_columns(columns);

    // Execute the query.
    azure::storage::table_query_iterator it = table.execute_query(query);

    // Display the results.
    azure::storage::table_query_iterator end_of_results;
    for (; it != end_of_results; ++it)
    {
        std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

        const azure::storage::table_entity::properties_type& properties = it->properties();
        for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
        {
            std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
        }

        std::wcout << std::endl;
    }

>[AZURE.NOTE] エンティティからいくつかのプロパティを照会する方がすべてのプロパティを取得するよりも効率的です。

## エンティティを削除する
エンティティは、取得後に簡単に削除できます。 エンティティを取得した後で呼び出す **table_operation::delete_entity** 、エンティティを削除するとします。 まず、 **cloud_table.execute** メソッドです。 次のコードは、"Jeff" の行キーと "Smith" のパーティション キーを持つエンティティを取得して削除します。  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);  

## テーブルを削除する
最後に、次のコード例は、ストレージ アカウントからテーブルを削除します。 削除されたテーブルは、削除後の一定期間は再作成できなくなります。  

    // Retrieve the storage account from the connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the table client.
    azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

    // Create a cloud table object for the table.
    azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
    azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

    // Create an operation to delete the entity.
    azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

    // Submit the delete operation to the Table service.
    azure::storage::table_result delete_result = table.execute(delete_operation);

## 次のステップ
これで、Table ストレージの基本を学習できました。Azure Storage の詳細については、次のリンク先を参照してください。  

-   [C++ から BLOB ストレージを使用する方法](storage-c-plus-plus-how-to-use-blobs.md)
-   [C++ から Queue ストレージを使用する方法](storage-c-plus-plus-how-to-use-queues.md)
-   [C++ での Azure Storage のリソース一覧の取得](storage-c-plus-plus-enumeration.md)
-   [C++ 用ストレージ クライアント ライブラリ リファレンス](http://azure.github.io/azure-storage-cpp)
-   [Azure Storage のドキュメント](http://azure.microsoft.com/documentation/services/storage/)


