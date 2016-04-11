<properties 
    pageTitle="Python から Queue ストレージを使用する方法 | Microsoft Azure" 
    description="Python から Azure Queue サービスを使用して、キューの作成と削除のほか、メッセージの挿入、取得、および削除を行う方法を説明します。" 
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

# Python から Queue ストレージを使用する方法

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

## 概要

このガイドでは、Azure Queue ストレージ サービスを使用して一般的なシナリオを実行する方法について説明します。 サンプルは Python で記述され使用する、 [Python Azure Storage のパッケージ][]します。 紹介するシナリオ **挿入**, 、**ピーク**,、
**取得する**, 、および **を削除する** キュー メッセージだけでなく **を作成し、
キューの削除**します。 キューの詳細については、「[次のステップ][]」セクションを参照してください。

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


> [AZURE.NOTE] Python をインストールする必要がある場合、または [Python Azure パッケージ][], を参照してください、 [Python インストール ガイド](../python-how-to-install.md)します。

## 方法: キューを作成する

 **QueueService** オブジェクトを使用して、キューを操作できます。 次のコード作成、 **QueueService** オブジェクトです。 プログラムを使用して Azure Storage にアクセスするすべての Python ファイルの先頭付近に、次のコードを追加します。

    from azure.storage.queue import QueueService

次のコード作成、 **QueueService** オブジェクトのストレージ アカウント名とアカウント キーを使用します。 'myaccount' と 'mykey' の部分は、実際のアカウントとキーに置き換えてください。

    queue_service = QueueService(account_name='myaccount', account_key='mykey')

    queue_service.create_queue('taskqueue')


## 方法: メッセージをキューに挿入する

キューにメッセージを挿入するには、使用、 **put \_message** メソッド
新しいメッセージを作成し、キューに追加します。

    queue_service.put_message('taskqueue', 'Hello World')


## 方法: 次のメッセージをピークする

キューの先頭にあるメッセージをキューから削除せずにピークするには、
呼び出してキューから、 **peek \_messages** メソッドです。 既定では、
**peek \_messages** 1 つのメッセージをピークします。

    messages = queue_service.peek_messages('taskqueue')
    for message in messages:
        print(message.message_text)


## 方法: 次のメッセージをデキューする

コードでは、2 つの手順でキューからメッセージを削除します。 既定では、
**get \_messages**, 、既定では、キュー内の次のメッセージを取得します。 A
返されたメッセージ **get \_messages** 他のすべての非表示になります
このキューからメッセージを読み取る他のコードから参照できなくなります。 既定では、このメッセージを参照できない
状態は 30 秒間続きます。 キューからのメッセージの削除を完了するには、
呼び出す必要があります **delete \_message**します。 2 段階の手順でメッセージを削除するこの方法では、
ハードウェアまたはソフトウェアの問題が原因でコードによるメッセージの処理が
失敗した場合に、コードの別のインスタンスで同じメッセージを取得し、
もう一度処理することができます。 コードの呼び出し **delete \_message** 右
を呼び出します。

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        print(message.message_text)
        queue_service.delete_message('taskqueue', message.message_id, message.pop_receipt)


## 方法: キューに配置されたメッセージの内容を変更する

キュー内のメッセージの内容をインプレースで変更できます。 また、
メッセージが作業タスクを表している場合は、この機能を使用して
作業タスクの状態を更新できます。 使用して次のコード、 **update \_message**
メソッドを使用してメッセージを更新します。

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        queue_service.update_message('taskqueue', message.message_id, 'Hello World Again', message.pop_receipt, 0)

## 方法: メッセージをデキューするための追加オプション

キューからのメッセージの取得をカスタマイズする方法は 2 つあります。
1 つ目の方法では、(最大 32 個の) メッセージのバッチを取得できます。 2 つ目の方法では、
コードで各メッセージを完全に処理できるように、非表示タイムアウトの
設定を長くまたは短くすることができます。 次のコード例では、
**get \_messages** 1 回の呼び出しで 16 個のメッセージを取得します。 その後、for ループを使用して、
各メッセージを処理します。 また、各メッセージの
非表示タイムアウトを 5 分に設定します。

    messages = queue_service.get_messages('taskqueue', numofmessages=16, visibilitytimeout=5*60)
    for message in messages:
        print(message.message_text)
        queue_service.delete_message('taskqueue', message.message_id, message.pop_receipt)

## 方法: キューの長さを取得する

キュー内のメッセージの概数を取得できます。 この
**get \_queue\_metadata** メソッドは、キュー サービスにメタデータを返すことを要求
キュー、および **x ms-およそ-メッセージのカウント** くださいとして使用する、インデックス、返されたディクショナリ カウントを検索します。
キュー サービスが要求に応答した後にメッセージが追加または削除される可能性があるため、
この結果は概数にすぎません。

    queue_metadata = queue_service.get_queue_metadata('taskqueue')
    count = queue_metadata['x-ms-approximate-messages-count']

## 方法: キューを削除する

キューおよびキューに含まれているすべてのメッセージを削除するには、
**delete \_queue** メソッドです。

    queue_service.delete_queue('taskqueue')

## 次のステップ

これで、キュー ストレージの基本を学習できました。さらに複雑なストレージ タスクを
さらに複雑なストレージ タスクについて学習するには、次のリンク先を参照してください。

-   参照してください、 [Azure ストレージ チーム ブログ][]

詳細については、「関連項目、 [Python デベロッパー センター](/develop/python/)します。

[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Python Azure package]: https://pypi.python.org/pypi/azure
[Python Azure Storage package]: https://pypi.python.org/pypi/azure-storage 
 

