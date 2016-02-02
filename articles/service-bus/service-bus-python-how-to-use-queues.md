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

この記事では、Service Bus キューの使用方法について説明します。 サンプルは Python で記述され使用する、 [Python Azure パッケージの][]します。 紹介するシナリオは、**キューの作成、メッセージの送受信**、**キューの削除**です。

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]
> [AZURE.NOTE] Python をインストールするか、 [Python Azure パッケージの][], を参照してください、 [Python インストール ガイド](../python-how-to-install.md)します。

## キューを作成する

**ServiceBusService** オブジェクトを使用すると、キューを操作できます。 プログラムを使用して Service Bus にアクセスするすべての Python ファイルの先頭付近に次のコードを追加します。

```
from azure.servicebus import ServiceBusService, Message, Queue
```

次のコードでは、**ServiceBusService** オブジェクトを作成します。 置換 `mynamespace`, 、`sharedaccesskeyname`, 、および `sharedaccesskey` 名前空間、共有アクセス署名 (SAS) キーの名前と値を使用します。

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

**create_queue** also supports additional options, which enable you to override default queue settings such as message time to live (TTL) or maximum queue size. The following example sets the maximum queue size to 5GB, and the TTL value to 1 minute:
```
queue_options Queue() =
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue ("taskqueue"、queue_options)
```

## Send messages to a queue

To send a message to a Service Bus queue, your application calls the **send\_queue\_message** method on the **ServiceBusService** object.

The following example demonstrates how to send a test message to the queue named *taskqueue using* **send\_queue\_message**:
```
msg = Message(b'Test Message')
bus_service.send_queue_message"taskqueue"(msg)
```

Service Bus queues support a maximum message size of 256 KB (the header, which includes the standard and custom application properties, can have a maximum size of 64 KB). There is no limit on the number of messages held in a queue but there is a cap on the total size of the messages held by a queue. This queue size is defined at creation time, with an upper limit of 5 GB. For more information about quotas, see [Azure Queues and Service Bus queues][].

## Receive messages from a queue

Messages are received from a queue using the **receive\_queue\_message** method on the **ServiceBusService** object:
```
msg = bus_service.receive_queue_message (:peek_lock"taskqueue"= False)
print(msg.body)
```

Messages are deleted from the queue as they are read when the parameter **peek\_lock** is set to **False**. You can read (peek) and lock the message without deleting it from the queue by setting the parameter **peek\_lock** to **True**.

The behavior of reading and deleting the message as part of the receive operation is the simplest model, and works best for scenarios in which an application can tolerate not processing a message in the event of a failure. To understand this, consider a scenario in which the consumer issues the receive request and then crashes before processing it. Because Service Bus will have marked the message as being consumed, then when the application restarts and begins consuming messages again, it will have missed the message that was consumed prior to the crash.

If the **peek\_lock** parameter is set to **True**, the receive becomes a two stage operation, which makes it possible to support applications that cannot tolerate missing messages. When Service Bus receives a request, it finds the next message to be consumed, locks it to prevent other consumers receiving it, and then returns it to the application. After the application finishes processing the message (or stores it reliably for future processing), it completes the second stage of the receive process by calling the **delete** method on the **Message** object. The **delete** method will mark the message as being consumed and remove it from the queue.
```
msg = bus_service.receive_queue_message (:peek_lock"taskqueue"= True)
print(msg.body)

msg.delete()
```

## アプリケーションのクラッシュと読み取り不能のメッセージを処理する方法

Service Bus には、アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 受信側のアプリケーションがなんらかの理由によってメッセージを処理できない場合には、**Message** オブジェクトの **unlock** メソッドを呼び出すことができます。 このメソッドが呼び出されると、Service Bus によってキュー内のメッセージのロックが解除され、メッセージが再度受信できる状態に変わります。メッセージを受信するアプリケーションは、以前と同じものでも、別のものでもかまいません。

キュー内でロックされているメッセージにはタイムアウトも設定されています。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合には、Service Bus によりメッセージのロックが自動的に解除され、再度受信できる状態に変わります。

メッセージが処理された後、**delete** メソッドが呼び出される前にアプリケーションがクラッシュした場合は、アプリケーションが再起動する際にメッセージが再配信されます。 一般的に、この動作は **"1 回以上の処理"** と呼ばれます。つまり、すべてのメッセージが 1 回以上処理されますが、特定の状況では、同じメッセージが再配信される可能性があります。 重複処理が許されないシナリオの場合、重複メッセージの配信を扱うロジックをアプリケーションに追加する必要があります。 通常、この問題はメッセージの **MessageId** プロパティを使用して対処します。このプロパティは配信が試行された後も同じ値を保持します。

## 次のステップ

これで、Service Bus キューの基本を学習できました。さらに詳細な情報が必要な場合は、次のリンク先をご覧ください。

-   参照してください [キュー、トピック、およびサブスクリプションの][]します。


[azure classic portal]: http://manage.windowsazure.com 
[python azure package]: https://pypi.python.org/pypi/azure 
[queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md 
[azure queues and service bus queues]: service-bus-azure-and-service-bus-queues-compared-contrasted.md#capacity-and-quotas 

