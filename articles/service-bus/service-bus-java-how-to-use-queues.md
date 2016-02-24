<properties
    pageTitle="Java での Service Bus キューの使用方法 | Microsoft Azure"
    description="Azure での Service Bus キューの使用方法を学習します。 コード サンプルは Java で記述されています。"
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    />

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/07/2015"
    ms.author="sethm"/>

# Service Bus キューの使用方法

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

この記事では、Service Bus キューの使用方法について説明します。 サンプルは
使用して Java で記述され、 [Azure SDK for Java][]します。 この
紹介するシナリオ **キューの作成**, 、**を送受信します。
メッセージ**, 、および **キューの削除**します。

[AZURE.INCLUDE [service-bus-java-how-to-create-queue](../../includes/service-bus-java-how-to-create-queue.md)]

## Service Bus を使用するようにアプリケーションを構成する

インストールされていることを確認、 [Azure SDK for Java][] このサンプルをビルドする前にします。 Eclipse を使用している場合は、インストール、 [Azure Toolkit for Eclipse][] 、Azure SDK for Java が含まれます。 追加することができますし、 **Microsoft Azure Libraries for Java** をプロジェクトに。

![](media/service-bus-java-how-to-use-queues/eclipselibs.png)

次の `import` ステートメントを Java ファイルの先頭に追加します。

```
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## キューを作成する

Service Bus キューの管理操作は、
**ServiceBusContract** クラスです。 A **ServiceBusContract** オブジェクトは、
管理用のトークン アクセス許可をカプセル化した適切な構成
それを管理するアクセス許可を持つ SAS トークンと **ServiceBusContract** クラスは、
Azure との唯一の通信ポイントです。

 **ServiceBusService** クラスを作成、列挙、メソッドを提供
およびキューを削除します。 次の例に、どのように、 **ServiceBusService** オブジェクト
"HowToSample"名前空間の名前が"TestQueue"キューの作成に使用できます。

        Configuration config =
            ServiceBusConfiguration.configureWithSASAuthentication(
                    "HowToSample",
                    "RootManageSharedAccessKey",
                    "SAS_key_value",
                    ".servicebus.windows.net"
                    );

    ServiceBusContract service = ServiceBusService.create(config);
    QueueInfo queueInfo = new QueueInfo("TestQueue");
    try
    {
        CreateQueueResult result = service.createQueue(queueInfo);
    }
    catch (ServiceException e)
    {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

メソッドがあります **QueueInfo** するキューのプロパティを使用できます。
調整 (例: する既定の有効期間 (TTL) 値を設定するには
適用、キューに送信されるメッセージ)。 次の例では、
という名前のキューを作成する `TestQueue` 、最大サイズは 5 GB。

    long maxSizeInMegabytes = 5120;
    QueueInfo queueInfo = new QueueInfo("TestQueue");
    queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateQueueResult result = service.createQueue(queueInfo);

使用できる、 **listQueues** メソッドを **ServiceBusContract**
オブジェクト内で指定した名前のキューが既に存在するかどうかをチェックするには
サービス名前空間。

## メッセージをキューに送信する

Service Bus キューにメッセージを送信するアプリケーションを取得します。
**ServiceBusContract** オブジェクトです。 次のコードを送信する方法を示しています、
メッセージを `TestQueue` で以前に作成したキュー、 `HowToSample` 名前空間。

    try
    {
        BrokeredMessage message = new BrokeredMessage("MyMessage");
        service.sendQueueMessage("TestQueue", message);
    }
    catch (ServiceException e)
    {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

メッセージには、送信、Service Bus から受信したキューのインスタンス、 [BrokeredMessage][] クラスです。 [BrokeredMessage][] オブジェクトがある一連の標準的なプロパティ (よう [ラベル](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) と [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx))、ディクショナリ
カスタム アプリケーション固有のプロパティおよび任意のアプリケーション データの本体を保持するために使用されます。 アプリケーションは、のコンス トラクターにシリアル化可能なオブジェクトを渡すことによって、メッセージの本文を設定することができます、 [BrokeredMessage][], 、オブジェクトをシリアル化する、適切なシリアライザーを使用しているとします。 または、提供、 **java。IO です。InputStream** オブジェクトです。

以下の例では、前に示したコード スニペットで取得した
`TestQueue` **MessageSender** 前のコード スニペットで取得しました。

    for (int i=0; i<5; i++)
    {
         // Create message, passing a string message for the body.
         BrokeredMessage message = new BrokeredMessage("Test message " + i);
         // Set an additional app-specific property.
         message.setProperty("MyProperty", i);
         // Send message to the queue
         service.sendQueueMessage("TestQueue", message);
    }

Service Bus キューでは、最大 256 KB までのメッセージをサポートしています
(標準とカスタムのアプリケーション プロパティが含まれるヘッダーは、
最大サイズは 64 kb です)。 キューで保持されるメッセージ数には上限がありませんが、
キュー 1 つあたりが保持できるメッセージの合計サイズには上限
があります。 このキューのサイズはキューの作成時に定義します。
上限は 5 GB です。

## キューからメッセージを受信する

キューからメッセージを受信する主な方法は、
**ServiceBusContract** オブジェクトです。 メッセージは 2 つの
さまざまなモード: **ReceiveAndDelete** と **PeekLock**します。

使用する場合、 **ReceiveAndDelete** モードでは、受信はシングル ショット
受信が 1 回ずつの動作になります。つまり、Service Bus は
メッセージ キューにメッセージが読み取り中としてマークされを返します
そのアプリケーションにします。 **ReceiveAndDelete** モード (既定値
モード) は最もシンプルなモデルとシナリオに最適な
メッセージを処理しないアプリケーションが許容できる、
失敗しました。 これを理解するシナリオを検討してください. コンシューマー
受信要求を発行し、処理する前にクラッシュします。
Service Bus はマークするため、メッセージの読み取りとして
アプリケーションが再起動して、メッセージの消費を開始するときに、
クラッシュ前に読み取られていたメッセージは見落とされます。

 **PeekLock** モードでは、受信は、2 段階の操作になります
メッセージが失われることが許容できないアプリケーションに対応することができます
。 Service Bus が要求を受け取ると、次に読み取られるメッセージを検索して、
他のコンシューマーが受信できないようロックしてから、
アプリケーションにメッセージを返します。 アプリケーションが
メッセージの処理を終えた後 (または後で処理するために確実に保存した後)、
呼び出して受信処理の第 2 段階を完了 **削除**
を完了します。 Service Bus が確認、 **削除** を呼び出すと、その
メッセージが読み取り中としてマークし、キューから削除します。

次の例では、メッセージを受信する方法について説明し、
使用して処理 **PeekLock** モード (既定モードではありません)。 次の例では、
以下は、無限ループが到着するごとにメッセージを処理しています。
"TestQueue":

        try
    {
        ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
        opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

        while(true)  {
             ReceiveQueueMessageResult resultQM =
                    service.receiveQueueMessage("TestQueue", opts);
            BrokeredMessage message = resultQM.getValue();
            if (message != null && message.getMessageId() != null)
            {
                System.out.println("MessageID: " + message.getMessageId());
                // Display the queue message.
                System.out.print("From queue: ");
                byte[] b = new byte[200];
                String s = null;
                int numRead = message.getBody().read(b);
                while (-1 != numRead)
                {
                    s = new String(b);
                    s = s.trim();
                    System.out.print(s);
                    numRead = message.getBody().read(b);
                }
                System.out.println();
                System.out.println("Custom Property: " +
                    message.getProperty("MyProperty"));
                // Remove message from queue.
                System.out.println("Deleting this message.");
                //service.deleteMessage(message);
            }  
            else  
            {
                System.out.println("Finishing up - no more messages.");
                break;
                // Added to handle no more messages.
                // Could instead wait for more messages to be added.
            }
        }
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }
    catch (Exception e) {
        System.out.print("Generic exception encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

## アプリケーションのクラッシュと読み取り不能のメッセージを処理する方法

Service Bus には、
アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 場合は、
受信側のアプリケーションがなんらかの理由によってメッセージを処理できない場合には、
呼び出すことができます、 **unlockMessage** 受信したメッセージに対してメソッド
(の代わりに、 **deleteMessage** メソッド)。 このメソッドが呼び出されると、Service Bus
キュー内でメッセージのロックを解除し、利用可能であることを
メッセージが再度受信できる状態に変わります。メッセージを受信するアプリケーションは、
以前と同じものでも、別のものでもかまいません。

キュー内でロックされているメッセージにはタイムアウトも設定されています。
アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合には、
メッセージのロックが自動的に解除され、
再度受信できる状態に変わります
。

メッセージが処理された後、
前に、 **deleteMessage** し、要求を発行すると、メッセージ
アプリケーションが再起動する際にメッセージが再配信されます。 一般的に、この動作は
呼ばれる **には、少なくとも 1 回処理**, 、つまり、各メッセージになります
1 回以上処理されますが、特定の状況では、同じメッセージが
再配信される可能性があります。 重複処理が許されないシナリオの場合、
アプリケーション開発者は重複メッセージの配信を扱うロジックを
アプリケーションに追加する必要があります。 通常、この問題は
使用して、 **getMessageId** はメッセージのメソッド
配信が試行された後も同じ値を保持します。

## 次のステップ

これをサービス バス キューの基本を学習できました。、を参照してください。 [キュー、トピック、およびサブスクリプション][] の詳細。

詳細については、次を参照してください。、 [Java デベロッパー センター](/develop/java/)します。


  [Azure SDK for Java]: http://azure.microsoft.com/develop/java/
  [Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

