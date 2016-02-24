<properties 
    pageTitle="Service Bus キューの使用方法 (Python) | Microsoft Azure" 
    description="Python から Azure Service Bus キューを使用する方法を説明します。" 
    services="service-bus" 
    documentationCenter="python" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="10/08/2015" 
    ms.author="sethm"/>


# Service Bus キューの使用方法

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

この記事では、Service Bus キューの使用方法について説明します。 サンプルは Python で記述され使用する、 [Python Azure パッケージ][]します。 紹介するシナリオ **キューの作成、メッセージを送受信する**, 、および **キューの削除**します。

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

> [AZURE.NOTE] Python をインストールするか、 [Python Azure パッケージ][], を参照してください、 [Python インストール ガイド](../python-how-to-install.md)します。

## キューを作成する

 **ServiceBusService** オブジェクトを使用するキューを操作します。 プログラムを使用して Service Bus にアクセスするすべての Python ファイルの先頭付近に次のコードを追加します。

```
from azure.servicebus import ServiceBusService, Message, Queue
```

次のコード作成、 **ServiceBusService** オブジェクトです。 `mynamespace`、`sharedaccesskeyname`、`sharedaccesskey` の部分は、実際の名前空間、Shared Access Signature (SAS) キー名、キー値に置き換えます。

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

SAS キーの名前と値の値は記載されて、 [Azure クラシック ポータル][] 接続情報、または Visual Studio で **プロパティ** ウィンドウに示すように、前のセクション) は、サーバー エクスプ ローラーで Service Bus 名前空間を選択するとします。

```
bus_service.create_queue('taskqueue')
```

**create_queue** もメッセージの time to live (TTL) や最大キュー サイズなどの既定のキュー設定を上書きすることを有効にする追加のオプションをサポートしています。 次の例では、最大キュー サイズを 5 GB に設定し、TTL 値を 1 分に設定しています。

```
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## メッセージをキューに送信する

Service Bus のキューでは、アプリケーション呼び出しにメッセージを送信する、 **\_queue\_message** メソッドを **ServiceBusService** オブジェクトです。

次の例では、という名前のキューにテスト メッセージを送信する方法 *taskqueue を使用して* **\_queue\_message**:

```
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Service Bus キューでは、最大 256 KB までのメッセージをサポートしています (標準とカスタムのアプリケーション プロパティが含まれるヘッダーの最大サイズは 64 KB です)。 キューで保持されるメッセージ数には上限がありませんが、キュー 1 つあたりが保持できるメッセージの合計サイズには上限があります。 このキュー サイズは作成時に定義され、上限は 5 GB です。 クォータの詳細については、次を参照してください。 [Azure キューと Service Bus キュー][]します。

## キューからメッセージを受信する

使用してキューからメッセージを受信、 **receive \_queue\_message** メソッドを **ServiceBusService** オブジェクト。

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

読み取られるときに、メッセージがキューから削除されますパラメーター **\_lock** に設定されている **False**します。 (ピークして) を参照し、パラメーターを設定して、キューから削除することがなく、メッセージをロックできます **\_lock** に **True**します。

受信操作の中で行われるメッセージの読み取りと削除の動作は、最もシンプルなモデルであり、障害発生時にアプリケーション側でメッセージを処理しないことを許容できるシナリオに最適です。 このことを理解するために、コンシューマーが受信要求を発行した後で、メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。 Service Bus はメッセージを読み取り済みとしてマークするため、アプリケーションが再起動してメッセージの読み取りを再開すると、クラッシュ前に読み取られていたメッセージは見落とされることになります。

場合、 **\_lock** にパラメーターが設定されている **True**, 、受信メッセージが失われることが許容できないアプリケーションをサポートすることが 2 段階の操作になります。 Service Bus は要求を受け取ると、次に読み取られるメッセージを検索して、他のコンシューマーが受信できないようロックしてから、アプリケーションにメッセージを返します。 呼び出して受信処理の第 2 段階を完了したら、アプリケーションがメッセージの処理 (または後で処理するために確実に保存)、 **削除** メソッドを **メッセージ** オブジェクトです。  **削除** メソッドは、メッセージが読み取り中としてマークされ、キューから削除します。

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## アプリケーションのクラッシュと読み取り不能のメッセージを処理する方法

Service Bus には、アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 受信側アプリケーションが何らかの理由でのメッセージを処理できないかどうかは、呼び出すことができます、 **のロックを解除** メソッドを **メッセージ** オブジェクトです。 このメソッドが呼び出されると、Service Bus によってキュー内のメッセージのロックが解除され、メッセージが再度受信できる状態に変わります。メッセージを受信するアプリケーションは、以前と同じものでも、別のものでもかまいません。

キュー内でロックされているメッセージにはタイムアウトも設定されています。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合には、Service Bus によりメッセージのロックが自動的に解除され、再度受信できる状態に変わります。

前に、メッセージを処理した後、アプリケーションがクラッシュすること、 **削除** メソッドが呼び出されると、再起動する際にそのメッセージがアプリケーションに再配信されます。 一般的に、 **1 回の処理には、少なくとも**, 、つまり、各メッセージが 1 回以上処理されますが、特定の状況で、同じメッセージが再配信されます。 重複処理が許されないシナリオの場合、重複メッセージの配信を扱うロジックをアプリケーションに追加する必要があります。 これは、多くの場合を使用して、 **MessageId** は一定に保たれる配信が試行されると、メッセージのプロパティです。

## 次のステップ

これで、Service Bus キューの基本を学習できました。さらに詳細な情報が必要な場合は、次のリンク先をご覧ください。

-   参照してください [キュー、トピック、およびサブスクリプション][]します。

[Azure classic portal]: http://manage.windowsazure.com
[Python Azure package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[Azure Queues and Service Bus queues]: service-bus-azure-and-service-bus-queues-compared-contrasted.md#capacity-and-quotas
 

