<properties
    pageTitle="Python から Azure BLOB ストレージを使用する方法 | Microsoft Azure"
    description="Python から Azure BLOB ストレージを使用して、BLOB をアップロード、一覧表示、ダウンロード、削除する方法について説明します。"
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


# Python から Azure BLOB ストレージを使用する方法

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

## 概要

この記事では、BLOB ストレージを使用して一般的なシナリオを実行する方法について説明します。 サンプルは Python で記述され使用する、 [Python Azure ストレージ パッケージ][]します。 紹介するシナリオは、BLOB の アップロード、一覧表示、
ダウンロード、削除などです。

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## コンテナーを作成する

> [AZURE.NOTE] Python をインストールする必要がある場合、または [Python Azure パッケージの][], を参照してください、 [Python インストール ガイド](../python-how-to-install.md)します。

**BlobService** オブジェクトを使用して、コンテナーおよび BLOB を操作できます。 この
次のコードを作成、 **BlobService** オブジェクトです。 次の内容を
プログラムを使用して Azure Storage にアクセスするすべての Python ファイルの先頭。

    from azure.storage.blob import BlobService

次のコードは、ストレージ アカウントの名前とアカウント キーを使用して、**BlobService** オブジェクトを作成します。 'myaccount' と 'mykey' の部分は、実際のアカウントとキーに置き換えてください。

    blob_service = BlobService(account_name='myaccount', account_key='mykey')

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

次のコード サンプルでコンテナーが存在しない場合は、**BlobService** オブジェクトを使用してコンテナーを作成できます。

    blob_service.create_container('mycontainer')

既定では、新しいコンテナーはプライベートであるため、このコンテナーから BLOB をダウンロードするにはストレージ アクセス キーを (前と同じ方法で) 指定する必要があります。 コンテナー内のファイルをすべてのユーザーが利用できるようにする場合は、次のコードを使用して、コンテナーを作成しパブリック アクセス レベルを渡します。

    blob_service.create_container('mycontainer', x_ms_blob_public_access='container')

または、次のコードを使用してコンテナーを作成した後で、コンテナーを変更することもできます。

    blob_service.set_container_acl('mycontainer', x_ms_blob_public_access='container')

この変更後、インターネット上のだれもがパブリック コンテナー内の BLOB を表示できますが、
管理者のみがそれらを変更または削除できます。

## コンテナーに BLOB をアップロードする

データを blob にアップロードするには、 **put \_block\_blob\_from\_path**, 、**put \_block\_blob\_from\_file**, 、**put \_block\_blob\_from\_bytes** または **put \_block\_blob\_from\_text** メソッドです。 これらは、データのサイズが 64 MB を超過した場合に必要なチャンクを実行する高レベル メソッドです。

**put \_block\_blob\_from\_path** 指定されたパスからファイルの内容をアップロードし、 **put \_block\_blob\_from\_file** 既に開かれているファイル/ストリームから内容をアップロードします。 **put \_block\_blob\_from\_bytes** (バイト単位) の配列をアップロードおよび **put \_block\_blob\_from\_text** は指定されたエンコード (既定値は utf-8) を使用して指定されたテキスト値をアップロードします。

次の例では、**sunset.png** ファイルの内容を **myblob** BLOB にアップロードします。

    blob_service.put_block_blob_from_path(
        'mycontainer',
        'myblob',
        'sunset.png',
        x_ms_blob_content_type='image/png'
    )

## コンテナー内の BLOB を一覧表示する

コンテナー内の blob の一覧を表示する、 **list \_blobs** メソッドです。 各
呼び出す **list \_blobs** は結果のセグメントを返します。 すべての結果を取得するには
チェック、 **next\_marker** 呼び出し、結果の **list \_blobs** として再度
必要に応じて新しいストレージ アカウントを作成します。 次のコードの出力、 **名** コンテナー内の各 blob の
出力します。

    blobs = []
    marker = None
    while True:
        batch = blob_service.list_blobs('mycontainer', marker=marker)
        blobs.extend(batch)
        if not batch.next_marker:
            break
        marker = batch.next_marker
    for blob in blobs:
        print(blob.name)

## BLOB をダウンロードする

結果の各セグメントは、最大の blob の変数の数を含めることができます。
5000 です。 場合 **next\_marker** が存在する特定のセグメントにある可能性があります
コンテナー内の複数の blob です。

Blob からデータをダウンロードするには使用 **\_blob\_to\_path**, 、**\_blob\_to\_bytes**, 、**get\_blob\_to\_bytes**, 、または **get \_blob\_to\_text を使用**します。 これらは、データのサイズが 64 MB を超過した場合に必要なチャンクを実行する高レベル メソッドです。

次の例では、使用方法を示します **\_blob\_to\_path** の内容をダウンロード、 **myblob** blob を格納する、 **out-sunset.png** ファイルです。

    blob_service.get_blob_to_path('mycontainer', 'myblob', 'out-sunset.png')

## BLOB を削除する

最後に、BLOB を削除するには、**delete_blob** を呼び出します。

    blob_service.delete_blob('mycontainer', 'myblob')

## 次のステップ

これで、Blob ストレージの基本を説明した次のリンクします。
さらに複雑なストレージ タスクについて学習するには、次のリンク先を参照してください。

- アクセス、 [Azure ストレージ チーム ブログ][]
- [AzCopy コマンド ライン ユーティリティを使用してデータを転送します。](storage-use-azcopy)

詳細については、「関連項目、 [Python デベロッパー センター](/develop/python/)します。


[azure storage team blog]: http://blogs.msdn.com/b/windowsazurestorage/ 
[python azure package]: https://pypi.python.org/pypi/azure 
[python azure storage package]: https://pypi.python.org/pypi/azure-storage 

