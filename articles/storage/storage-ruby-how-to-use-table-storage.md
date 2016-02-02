<properties
    pageTitle="Ruby から Azure Table Storage  を使用する方法 | Microsoft Azure"
    description="Azure で Azure Table Storage を使用する方法について説明します。コード サンプルは Ruby API を使用して記述されています。"
    services="storage"
    documentationCenter="ruby"
    authors="tfitzmac"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="09/23/2015"
    ms.author="tomfitz"/>



# Ruby から Azure Table Storage を使用する方法

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

## 概要

このガイドでは、Azure Table サービスを使用して一般的なシナリオを実行する方法について説明します。 サンプルは Ruby API を使用して記述されています。 紹介するシナリオは、**テーブルの作成と削除、テーブルのエンティティの挿入とクエリ実行**などです。

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## Ruby アプリケーションの作成

手順については、Ruby アプリケーションを作成する方法
参照してください [Azure で Ruby アプリケーションの作成](/develop/ruby/tutorials/web-app-with-linux-vm/)します。

## アプリケーションのストレージへのアクセスの構成

Azure Storage を使用するには、Ruby azure パッケージをダウンロードして使用する必要があります。このパッケージには、Storage REST サービスと通信するための便利なライブラリのセットが含まれています。

### RubyGems を使用してパッケージを取得する

1. **PowerShell** (Windows)、**ターミナル** (Mac)、**Bash** (Unix) などのコマンド ライン インターフェイスを使用します。

2. コマンド ウィンドウに「**gem install azure**」と入力して、gem と依存関係をインストールします。

### パッケージをインポートする

任意のテキスト エディターを使用して、Storage を使用する Ruby ファイルの先頭に次のコードを追加します。

    require "azure"

## Azure Storage の接続文字列の設定

Azure モジュールでは、環境変数を読み取ります **azure \_storage\_account** と **azure \_storage\_access\_key** については、Azure ストレージ アカウントへの接続に必要です。 これらの環境変数が設定されていない場合は、**Azure::TableService** を使用する前に、次のコードを使用してアカウント情報を指定する必要があります。

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your azure storage access key>"

これらの値を取得するには、次の手順を実行します。

1. ログイン、 [Azure ポータル](portal.azure.com)します。

2. ストレージ アカウントに移動します。

3. **[設定]**ブレードで、[**[キー]** を選択します。

4. 所望するアクセス キー値をコピーします。

## テーブルの作成

**Azure::TableService** オブジェクトを使用すると、テーブルとエンティティを操作できます。 テーブルを作成するには、 **create \_table ()** メソッドです。 次の例では、テーブルを作成し、既に存在している場合はエラーを出力します。

    azure_table_service = Azure::TableService.new
    begin
      azure_table_service.create_table("testtable")
    rescue
      puts $!
    end

## エンティティをテーブルに追加する

エンティティを追加するには、エンティティのプロパティを定義するハッシュ オブジェクトを最初に作成します。 すべてのエンティティについて、**PartitionKey** と **RowKey** を指定する必要があることに注意してください。 これらはエンティティの一意の識別子であり、他のエンティティのプロパティよりはるかに高速に照会できる値です。 Azure Storage では、テーブルのエンティティを多数のストレージ ノードに自動的に配布するために **PartitionKey** を使用します。 **PartitionKey** が同じエンティティは同じノードに格納されます。 **RowKey** は、エンティティが属するパーティション内のエンティティの一意の ID です。

    entity = { "content" => "test entity",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.insert_entity("testtable", entity)

## エンティティを更新する

既存のエンティティを更新するには、複数のメソッドがあります。

* **update \_entity ():** 置換することで既存のエンティティを更新します。
* **merge \_entity ():** 新しいプロパティ値を既存のエンティティに結合することによって既存のエンティティを更新します。
* **insert \_or\_merge\_entity ():** 置換することで既存のエンティティを更新します。 エンティティが存在しない場合は、新しいエンティティが挿入されます。
* **insert \_or\_replace\_entity ():** 新しいプロパティ値を既存のエンティティに結合することによって既存のエンティティを更新します。 エンティティが存在しない場合は、新しいエンティティが挿入されます。

次の例は、更新、エンティティを使用して **update \_entity ()**:

    entity = { "content" => "test entity with updated content",
      :PartitionKey => "test-partition-key", :RowKey => "1" }
    azure_table_service.update_entity("testtable", entity)

**Update \_entity ()** と **merge \_entity ()**, 、更新操作は失敗し、更新するエンティティが存在しない場合。 したがってで既に存在するかどうかに関係なくエンティティを格納する場合を使用して **insert \_or\_replace\_entity ()** または **insert \_or\_merge\_entity ()**します。

## エンティティのグループを操作する

状況によって、複数の操作をバッチとして送信し、サーバーによるアトミック処理を行うことが合理的である場合があります。 これを実現するを作成、 **バッチ** オブジェクトを使用して、 **execute \_batch ()** メソッドを **TableService**します。 次の例では、RowKey が 2 および 3 である 2 つのエンティティをバッチで送信する方法を示します。 これは、同じ PartitionKey を持つエンティティでのみ機能することに注意してください。

    azure_table_service = Azure::TableService.new
    batch = Azure::Storage::Table::Batch.new("testtable",
      "test-partition-key") do
      insert "2", { "content" => "new content 2" }
      insert "3", { "content" => "new content 3" }
    end
    results = azure_table_service.execute_batch(batch)

## エンティティを照会する

テーブル内のエンティティを照会する、 **get \_entity ()** メソッドは、テーブル名を渡すことによって **PartitionKey** と **RowKey**します。

    result = azure_table_service.get_entity("testtable", "test-partition-key",
      "1")

## エンティティのセットを照会する

テーブル内のエンティティのセットを照会するには、クエリ ハッシュ オブジェクトを作成しを使用して、 **query \_entities ()** メソッドです。 次の例では、同じ **PartitionKey** を持つエンティティをすべて取得します。

    query = { :filter => "PartitionKey eq 'test-partition-key'" }
    result, token = azure_table_service.query_entities("testtable", query)

> [AZURE.NOTE] 結果セットが大きすぎて単一のクエリで返すことができない場合は、継続トークンが返されます。この継続トークンを使用して、後続のページを取得できます。

## エンティティ プロパティのサブセットを照会する

テーブルに対するクエリでは、ごくわずかのプロパティだけをエンティティから取得できます。 "プロジェクション" と呼ばれるこの方法では、帯域幅の使用が削減され、クエリのパフォーマンスが向上します。特に、大量のエンティティがある場合に役立ちます。 select 句を使用して、クライアントに渡すプロパティの名前を指定します。

    query = { :filter => "PartitionKey eq 'test-partition-key'",
      :select => ["content"] }
    result, token = azure_table_service.query_entities("testtable", query)

## エンティティを削除する

エンティティを削除するには、 **delete \_entity ()** メソッドです。 目的のエンティティを含んでいるテーブルの名前と、エンティティの PartitionKey および RowKey を渡す必要があります。

        azure_table_service.delete_entity("testtable", "test-partition-key", "1")

## テーブルを削除する

テーブルを削除するには、 **delete\_table()** メソッドとパスを削除するテーブルの名前。

        azure_table_service.delete_table("testtable")

## 次のステップ

さらに複雑なストレージ タスクの詳細については、次のリンク先をご覧ください。

- [Azure ストレージ チーム ブログ](http://blogs.msdn.com/b/windowsazurestorage/)
- [Azure SDK for Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) GitHub のリポジトリ





