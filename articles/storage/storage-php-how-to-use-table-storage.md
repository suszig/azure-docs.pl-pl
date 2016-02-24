<properties
    pageTitle="PHP から Table ストレージを使用する方法 | Microsoft Azure"
    description="PHP から Table サービスを使用して、テーブルを作成および削除する方法、テーブルのエンティティを挿入、削除、照会する方法について説明します。"
    services="storage"
    documentationCenter="php"
    authors="tfitzmac"
    manager="adinah"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="09/23/2015"
    ms.author="tomfitz"/>


# PHP から Table ストレージを使用する方法

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

## 概要

このガイドでは、Azure Table サービスを使用して一般的なシナリオを実行する方法について説明します。 サンプルは PHP を使用で記述され、 [Azure SDK for PHP][download]します。 紹介するシナリオ **を作成して、テーブルを削除して挿入、削除、およびテーブル内のエンティティを照会する**です。 Azure テーブル サービスの詳細については、次を参照してください。、 [次のステップ](#NextSteps) セクションです。

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## PHP アプリケーションの作成

Azure Table サービスにアクセスする PHP アプリケーションを作成するための要件は、コード内から Azure SDK for PHP のクラスを参照することのみです。 アプリケーションの作成には、メモ帳などの任意の開発ツールを使用できます。

このガイドで使用する Table サービス機能は、PHP アプリケーション内からローカルで呼び出すことも、Azure の Web ロール、worker ロール、または Web サイト上で実行されるコード内で呼び出すこともできます。

## Azure クライアント ライブラリの入手

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## Table サービスにアクセスするようにアプリケーションを構成する

Azure Table サービス API を使用するには、次の要件があります。

1. 使用してオートローダー ファイルを参照、 [require_once][require_once] ステートメントと
2. 使用する可能性のあるクラスを参照する

次の例では、オートローダー ファイルと参照を含める方法、 **ServicesBuilder** クラスです。

> [AZURE.NOTE] この例 (および、この記事では、その他の例) は、Composer を使用して Azure 向け PHP クライアント ライブラリをインストールしたと仮定します。 ライブラリを手動でまたは PEAR パッケージとしてインストールした場合は、<code>WindowsAzure.php</code> オートローダー ファイルを参照する必要があります。

    require_once 'vendor\autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


下のすべてのサンプルに `require_once` ステートメントが入っていますが、サンプルの実行に必要なクラスのみが参照されます。

## Azure のストレージ接続文字列の設定

Azure Table サービス クライアントをインスタンス化するには、まず有効な接続文字列が必要です。 Table サービスの接続文字列の形式は次のとおりです。

ライブ サービスにアクセスする場合:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

エミュレーター ストレージにアクセスする場合:

    UseDevelopmentStorage=true


いずれの Azure サービス クライアントを作成するには、使用する必要があります、 **ServicesBuilder** クラスです。 そのための方法は次のとおりです。

* 接続文字列を直接渡す
* 使用して、 **CloudConfigurationManager (CCM)** を接続文字列の複数の外部ソースを確認します。
    * 既定では 1 つの外部ソース (環境変数) のみサポートされています。
    * 新しいソースを追加するには拡張することによって、 **ConnectionStringSource** クラス

ここで概説している例では、接続文字列を直接渡します。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);


## テーブルの作成

A **TableRestProxy** オブジェクトでは、テーブルを作成することができます、 **createTable** メソッドです。 テーブルの作成時、Table サービスのタイムアウトを設定できます (Table サービスのタイムアウトの詳細については、次を参照してください [。テーブル サービス操作のタイムアウト設定][table-service-timeouts].)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Create table.
        $tableRestProxy->createTable("mytable");
    }
    catch(ServiceException $e){
        $code = $e->getCode();
        $error_message = $e->getMessage();
        // Handle exception based on error codes and messages.
        // Error codes and messages can be found here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
    }

制限に関する詳細については、テーブル名は、次を参照してください。 [テーブル サービス データ モデルについて][table-data-model]します。

## エンティティをテーブルに追加する

テーブルにエンティティを追加するには、新しい作成 **エンティティ** オブジェクトに渡すと **TableRestProxy insertEntity]-> [**します。 エンティティの作成時には `PartitionKey` と `RowKey` を指定する必要があることに注意してください。 これらにはエンティティの一意の識別子であり、他のエンティティのプロパティよりはるかに高速に照会できる値です。 システムでは `PartitionKey` が使用されて多くのストレージ ノードにテーブルのエンティティが自動的に配布されます。 `PartitionKey` が同じエンティティは同じノードで格納されています (同じノードで格納されている複数のエンティティに対する処理は、異なるノードにまたがって格納されているエンティティに対する処理よりもパフォーマンスは高くなります)。`RowKey` は特定のパーティション内のエンティティの一意の ID です。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\Table\Models\Entity;
    use WindowsAzure\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $entity = new Entity();
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate",
                         EdmType::DATETIME,
                         new DateTime("2012-11-05T08:15:00-08:00"));
    $entity->addProperty("Location", EdmType::STRING, "Home");

    try{
        $tableRestProxy->insertEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
    }

テーブルのプロパティと型については、次を参照してください。 [テーブル サービス データ モデルについて][table-data-model]します。

 **TableRestProxy** クラスは、エンティティを挿入する 2 つのメソッドを提供しています: **insertOrMergeEntity** と **insertOrReplaceEntity**します。 これらのメソッドを使用するのには、新しい作成 **エンティティ** し、いずれかのメソッドのパラメーターとして渡します。 各メソッドは、渡されたエンティティが存在しない場合に、そのエンティティを挿入します。 エンティティが既に存在する場合 **insertOrMergeEntity** プロパティが存在しない場合は、プロパティ値を更新し、新しいプロパティを追加] に存在しない場合 **insertOrReplaceEntity** は既存のエンティティを完全に置き換えます。 次の例は、使用する方法を示しています。 **insertOrMergeEntity**します。 `PartitionKey` が "tasksSeattle" で `RowKey` が "1" であるエンティティがまだ存在しない場合は挿入されます。 ただし、既に挿入されている場合 (前の例を参照)、`DueDate` プロパティが更新され、`Status` プロパティが追加されます。 `Description` プロパティと `Location` プロパティも更新されますが、値は実際には変更されないままになります。 これら後者の 2 つのプロパティは例に示しているように追加されますが、ターゲット エンティティに存在しているため、それらの既存の値は変更されないままになります。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\Table\Models\Entity;
    use WindowsAzure\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    //Create new entity.
    $entity = new Entity();

    // PartitionKey and RowKey are required.
    $entity->setPartitionKey("tasksSeattle");
    $entity->setRowKey("1");

    // If entity exists, existing properties are updated with new values and
    // new properties are added. Missing properties are unchanged.
    $entity->addProperty("Description", null, "Take out the trash.");
    $entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
    $entity->addProperty("Location", EdmType::STRING, "Home");
    $entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

    try {
        // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
        // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
        $tableRestProxy->insertOrMergeEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## 単一のエンティティを取得する

 **TableRestProxy getEntity]-> [** メソッドでは、クエリすることで、単一のエンティティを取得できます。 その `PartitionKey` と `RowKey`です。 パーティション キーの下の例では `tasksSeattle` と行キー `1` に渡される、 **getEntity** メソッドです。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entity = $result->getEntity();

    echo $entity->getPartitionKey().":".$entity->getRowKey();

## パーティション内のすべてのエンティティを取得する

エンティティのクエリは、フィルターを使用して作成されます (詳細については、次を参照してください。 [クエリを実行するテーブルとエンティティ][filters])。 パーティション内のすべてのエンティティを取得するには、使用、フィルター"PartitionKey eq *partition_name*"です。 次の例では、すべてのエンティティを取得する方法、 `tasksSeattle` パーティションにフィルターを渡すことによって、 **queryEntities** メソッドです。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "PartitionKey eq 'tasksSeattle'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## パーティション内のエンティティのサブセットを取得する

前の例で示している同じパターンを使用してパーティション内のエンティティのサブセットを取得できます。 取得するエンティティのサブセットを使用するフィルターによって決まります (詳細については、次を参照してください。 [テーブルおよびエンティティを照会する][filters])。次の例は、フィルターを使用して、特定のエンティティをすべて取得する方法を示しています。 `Location` と `DueDate` で指定した日付より前です。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

    try {
        $result = $tableRestProxy->queryEntities("mytable", $filter);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    $entities = $result->getEntities();

    foreach($entities as $entity){
        echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
    }

## エンティティ プロパティのサブセットを取得する

クエリを使用してエンティティのプロパティのサブセットを取得できます。 呼ばれるこの方法 *投影*, が帯域幅を削減され、特に、大量のエンティティのクエリ パフォーマンスを向上させることができます。 取得するプロパティを指定するプロパティの名前を渡す、 **クエリ addSelectField]-> [** メソッドです。 このメソッドを複数回呼び出して、ほかのプロパティを追加できます。 実行した後 **TableRestProxy queryEntities]-> [**, 、返されるエンティティは、選択したプロパティのみを持ちます。 (テーブル エンティティのサブセットが返されるようにする場合は、前のクエリで示したようにフィルターを使用します)。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\Table\Models\QueryEntitiesOptions;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $options = new QueryEntitiesOptions();
    $options->addSelectField("Description");

    try {
        $result = $tableRestProxy->queryEntities("mytable", $options);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

    // All entities in the table are returned, regardless of whether
    // they have the Description field.
    // To limit the results returned, use a filter.
    $entities = $result->getEntities();

    foreach($entities as $entity){
        $description = $entity->getProperty("Description")->getValue();
        echo $description."<br />";
    }

## エンティティを更新する

使用して、既存のエンティティを更新することができます、 **エンティティ setProperty]-> [** と **エンティティ addProperty]-> [** メソッドおよび、通話を **TableRestProxy updateEntity]-> [**します。 次の例では、エンティティを取得してから、1 つのプロパティの変更、別のプロパティの削除、新しいプロパティの追加を行っています。 その値を設定してプロパティを削除するには、注意してください **null**します。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\Table\Models\Entity;
    use WindowsAzure\Table\Models\EdmType;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

    $entity = $result->getEntity();

    $entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

    $entity->setPropertyValue("Location", null); //Removed Location.

    $entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

    try {
        $tableRestProxy->updateEntity("mytable", $entity);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## エンティティを削除する

エンティティを削除するテーブル名およびエンティティを渡す `PartitionKey` と `RowKey` に、 **TableRestProxy deleteEntity]-> [** メソッドです。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete entity.
        $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

同時実行制御チェックを使用して削除するエンティティの Etag を設定できます、 **DeleteEntityOptions setEtag]-> [** メソッドを渡して、 **DeleteEntityOptions** オブジェクトを **deleteEntity** 4 番目のパラメーターとして。

## バッチ テーブル処理

 **TableRestProxy バッチ]-> [** メソッドでは、1 つの要求で複数の操作を実行することができます。 ここに示すパターンでは、追加の操作を **BatchRequest** オブジェクトと成功して、 **BatchRequest** オブジェクトを **TableRestProxy バッチ]-> [** メソッドです。 するための操作を追加する、 **BatchRequest** オブジェクトは、次の方法のいずれかを複数回呼び出します。

* **addInsertEntity** (insertEntity 処理を追加)
* **addUpdateEntity** (updateEntity 処理を追加)
* **addMergeEntity** (mergeEntity 処理を追加)
* **addInsertOrReplaceEntity** (insertOrReplaceEntity 処理を追加)
* **addInsertOrMergeEntity** (insertOrMergeEntity 処理を追加)
* **addDeleteEntity** (deleteEntity 処理を追加)

次の例を実行する方法を示しています。 **insertEntity** と **deleteEntity** 1 つの要求での操作。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;
    use WindowsAzure\Table\Models\Entity;
    use WindowsAzure\Table\Models\EdmType;
    use WindowsAzure\Table\Models\BatchOperations;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    // Create list of batch operation.
    $operations = new BatchOperations();

    $entity1 = new Entity();
    $entity1->setPartitionKey("tasksSeattle");
    $entity1->setRowKey("2");
    $entity1->addProperty("Description", null, "Clean roof gutters.");
    $entity1->addProperty("DueDate",
                          EdmType::DATETIME,
                          new DateTime("2012-11-05T08:15:00-08:00"));
    $entity1->addProperty("Location", EdmType::STRING, "Home");

    // Add operation to list of batch operations.
    $operations->addInsertEntity("mytable", $entity1);

    // Add operation to list of batch operations.
    $operations->addDeleteEntity("mytable", "tasksSeattle", "1");

    try {
        $tableRestProxy->batch($operations);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

テーブル バッチ処理の詳細については、次を参照してください。 [エンティティ グループ トランザクションを実行する][entity-group-transactions]です。

## テーブルを削除する

最後に、テーブルを削除するテーブル名を渡す、 **TableRestProxy deleteTable]-> [** メソッドです。

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use WindowsAzure\Common\ServiceException;

    // Create table REST proxy.
    $tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

    try {
        // Delete table.
        $tableRestProxy->deleteTable("mytable");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179438.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## 次のステップ

これで、Azure Table サービスの基本を学習できました。さらに複雑なストレージ タスクについては、次のリンク先を参照してください。

- 参照してください、 [Azure Storage チームのブログ](http://blogs.msdn.com/b/windowsazurestorage/)

詳細については、「関連項目、 [PHP デベロッパー センター](/develop/php/)します。

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx

