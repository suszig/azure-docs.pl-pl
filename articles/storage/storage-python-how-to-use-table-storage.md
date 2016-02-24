<properties
    pageTitle="Python から Table ストレージを使用する方法 | Microsoft Azure"
    description="Python から Table サービスを使用して、テーブルを作成、削除する方法、テーブルのエンティティを挿入、照会する方法について説明します。"
    services="storage"
    documentationCenter="python"
    authors="emgerner-msft"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="12/11/2015"
    ms.author="emgerner"/>


# Python から Table ストレージを使用する方法

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]

## 概要

このガイドでは、Azure Table ストレージ サービスを使用して一般的なシナリオを実行する方法について説明します。 サンプルは Python で記述され使用する、 [Python Azure Storage のパッケージ][]します。 管理対象のシナリオは、作成および削除します。
挿入して、テーブル内のエンティティのクエリを実行するだけでなく、テーブルです。

[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.NOTE] Python をインストールする必要がある場合、または [Python Azure パッケージ][], を参照してください、 [Python インストール ガイド](../python-how-to-install.md)します。


## テーブルの作成

 **TableService** オブジェクトでは、テーブル サービスを使用することができます。 この
次のコードを作成、 **TableService** オブジェクトです。 付近のコードを追加します。
プログラムを使用して Azure Storage にアクセスするすべての Python ファイルの先頭付近に、次のコードを追加します。

    from azure.storage.table import TableService, Entity

次のコード作成、 **TableService** ストレージ アカウント名とアカウント キーを使用してオブジェクトです。  'myaccount' と 'mykey' の部分は、実際のアカウントとキーに置き換えてください。

    table_service = TableService(account_name='myaccount', account_key='mykey')

    table_service.create_table('tasktable')

## エンティティをテーブルに追加する

エンティティを追加するには、最初に、エンティティの
プロパティ名と値を定義するディクショナリを作成します。 すべてのエンティティに必要なに注意してください。
指定 **PartitionKey** と **RowKey**します。 これらはエンティティの一意の識別子であり、
エンティティの識別子。 これらの値をはるかに照会します。
その他のプロパティを照会するよりも高速です。 システムを使用して **PartitionKey** に
多くのストレージ ノードにテーブルのエンティティを自動的に配布します。
同じであるエンティティ **PartitionKey** 同じノードに格納されます。 **RowKey** パーティション内のエンティティの一意の id を指定します。
一意の ID です。

エンティティをテーブルに追加するには、
 **insert \_entity** メソッドです。

    task = {'PartitionKey': 'tasksSeattle', 'RowKey': '1', 'description' : 'Take out the trash', 'priority' : 200}
    table_service.insert_entity('tasktable', task)

インスタンスを渡すことも、 **エンティティ** クラスを **insert \_entity** メソッドです。

    task = Entity()
    task.PartitionKey = 'tasksSeattle'
    task.RowKey = '2'
    task.description = 'Wash the car'
    task.priority = 100
    table_service.insert_entity('tasktable', task)

## エンティティを更新する

次のコードは、既存のエンティティの以前のバージョンを更新されたバージョンに置き換える方法を
示しています。

    task = {'description' : 'Take out the garbage', 'priority' : 250}
    table_service.update_entity('tasktable', 'tasksSeattle', '1', task)

更新されるエンティティが存在しない場合、更新操作は失敗します。
操作は失敗します。 エンティティを格納する場合
使用する前に存在していたかどうかに関係なく **insert \_or\_replace_entity**します。
次の例では、最初の呼び出しで既存のエンティティを置き換えます。 2 番目の呼び出しは、エンティティが、指定したため、新しいエンティティが挿入されます **PartitionKey** と **RowKey** テーブル内に存在します。

    task = {'description' : 'Take out the garbage again', 'priority' : 250}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '1', task)

    task = {'description' : 'Buy detergent', 'priority' : 300}
    table_service.insert_or_replace_entity('tasktable', 'tasksSeattle', '3', task)

## エンティティのグループを変更する

状況によって、複数の操作をバッチとして送信し、
サーバーによるアトミック処理を行うことが合理的である場合があります。 これを実現するには、
使用して、 **begin \_batch** メソッドを **TableService** しを呼び出すと、
一連の操作を通常どおり呼び出します。 バッチを送信するときは、
バッチを呼び出す **commit \_batch**します。 バッチでエンティティを変更するには、すべてのエンティティが同じパーティションに含まれている必要があります。 次の例では、2 つのエンティティをバッチでまとめて追加します。

    task10 = {'PartitionKey': 'tasksSeattle', 'RowKey': '10', 'description' : 'Go grocery shopping', 'priority' : 400}
    task11 = {'PartitionKey': 'tasksSeattle', 'RowKey': '11', 'description' : 'Clean the bathroom', 'priority' : 100}
    table_service.begin_batch()
    table_service.insert_entity('tasktable', task10)
    table_service.insert_entity('tasktable', task11)
    table_service.commit_batch()

## エンティティを照会する

テーブル内のエンティティを照会する、 **get \_entity** メソッド
渡す **PartitionKey** と **RowKey**します。

    task = table_service.get_entity('tasktable', 'tasksSeattle', '1')
    print(task.description)
    print(task.priority)

## エンティティのセットを照会する

この例は、に基づいて、Seattle 内のすべてのタスクを検索 **PartitionKey**します。

    tasks = table_service.query_entities('tasktable', "PartitionKey eq 'tasksSeattle'")
    for task in tasks:
        print(task.description)
        print(task.priority)

## エンティティ プロパティのサブセットを照会する

テーブルに対するクエリでは、ごくわずかのプロパティだけをエンティティから取得できます。
呼ばれるこの方法 *投影*, 、帯域幅が削減され、向上させることができます
クエリのパフォーマンスが向上します。特に、大量のエンティティがある場合に役立ちます。 使用して、 **選択**
パラメーターを提供するプロパティの名前を渡します
クライアント。

次のコードでクエリには、説明のみが返されます。
テーブル内のエンティティです。

[AZURE.NOTE] 次のスニペットはクラウドに対してのみ機能します。
記憶域サービス。 これは、記憶域がサポートされていません
Microsoft Azure ストレージ エミュレーターが起動されます。

    tasks = table_service.query_entities('tasktable', "PartitionKey eq 'tasksSeattle'", 'description')
    for task in tasks:
        print(task.description)

## エンティティを削除する

パーティション キーと行キーを使用してエンティティを削除できます。

    table_service.delete_entity('tasktable', 'tasksSeattle', '1')

## テーブルを削除する

次のコードは、ストレージ アカウントからテーブルを削除します。

    table_service.delete_table('tasktable')

## 次のステップ

これで、テーブル ストレージの基本を説明した次のリンクします。
さらに複雑なストレージ タスクについて説明します。

-   MSDN リファレンス  [Azure Storage][] を参照してください。
-   参照してください、 [Azure Storage チームのブログ][]します。

詳細については、「関連項目、 [Python デベロッパー センター](/develop/python/)します。

[Azure Storage Team blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Python Azure package]: https://pypi.python.org/pypi/azure
[Python Azure Storage package]: https://pypi.python.org/pypi/azure-storage

