<properties 
    pageTitle="パーティション分割された Service Bus のキューおよびトピックでの AMQP 1.0 のサポート |Microsoft Azure" 
    description="Advanced Message Queuing Protocol (AMQP) 1.0 を Service Bus のパーティション分割されたキューおよびトピックで使用する方法を学習します。" 
    services="service-bus" 
    documentationCenter=".net" 
    authors="hillaryc" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="11/05/2015" 
    ms.author="hillaryc"/>

# パーティション分割された Service Bus のキューとトピックにおける AMQP 1.0 のサポート 

Azure Service Bus ようになりました Advanced Message Queuing Protocol (**AMQP**) の Service Bus 1.0 **パーティション分割されたキューおよびトピックです。**

**AMQP** はオープン標準のメッセージのさまざまなプログラミング言語を使用してクロスプラット フォーム アプリケーションを開発できるようにするキューのプロトコルです。 詳細については、Service Bus で AMQP サポートは、次を参照してください。 [Service Bus で AMQP 1.0 サポート](service-bus-amqp-overview.md)します。

**キューおよびトピックをパーティション分割されて**, とも呼ばれる、 *パーティション分割されたエンティティ*, 、パーティション分割されていない従来のキューおよびトピックより高い可用性、信頼性、スループットを提供します。 パーティション分割されたエンティティの詳細については、次を参照してください。 [メッセージング エンティティのパーティション分割された](service-bus-partitioning.md)します。

パーティション分割されたキューおよびトピックと通信するための AMQP 1.0 プロトコルの追加により、Service Bus のパーティション分割されたエンティティによって提供される高い可用性、信頼性、スループットを活用できる、相互運用性のあるアプリケーションの構築が可能になります。    

## パーティション分割されたキューでの AMQP の使用

キューは、一時的な切り離し、負荷平準化、負荷分散、および疎結合を必要とするシナリオに便利です。 キューでは、パブリッシャーがキューにメッセージを送信します。コンシューマーは、メッセージが 1 回しか受信できない状況でキューからのメッセージを受信します。 この良い例が、POS 端末が在庫管理システムにデータを直接発行するのではなく、キューにデータを発行する在庫システムです。 その後、在庫管理システムは在庫補充を管理するためにいつでもデータを使用できます。 これには、在庫管理システムを常にオンラインにしておく必要がないなど、いくつかの利点があります。 Service Bus キューの詳細については、次を参照してください [Service Bus キューを使用するアプリケーションの作成。](service-bus-create-queues.md) 

アプリケーションの可用性、信頼性、スループットは、複数のメッセージ ブローカーとメッセージング ストアにパーティション分割されたキューによりさらに向上します。     

### パーティション分割されたキューの作成

使用して、パーティション分割されたキューを作成することができます、 [Azure クラシック ポータル][] と Service Bus SDK。 パーティション分割されたキューを作成する設定、 [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.enablepartitioning.aspx) プロパティを **true** で、 [QueueDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queuedescription.aspx) インスタンス。 次のコードでは Service Bus SDK を使用してパーティション分割されたキューを作成する方法を示します。 
 
```
// Create partitioned queue
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var queueDescription = new QueueDescription("myQueue");
queueDescription.EnablePartitioning = true;
nm.CreateQueue(queueDescription);
```

### AMQP を使用したメッセージの送受信

メッセージを送信し、プロトコルとして設定して AMQP を使用してパーティション分割されたキューからメッセージを受信、 [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) プロパティを [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx) 次のコードに示すようにします。  

```
// Sending and receiving messages to and from a queue
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
var queueClient = QueueClient.CreateFromConnectionString(amqpConnectionString, "myQueue");

BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
queueClient.Send(message);

var receivedMessage = queueClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## パーティション分割されたトピックでの AMQP の使用

トピックは、キューと同様に、一時的な結合の解除、負荷平準化、負荷分散、疎結合を必要とするシナリオで便利です。 キューとは異なり、トピックは同じメッセージのコピーを複数のサブスクライバーにルーティングできます。 トピックでは、パブリッシャーはトピックにメッセージを送信し、コンシューマーはサブスクリプションからメッセージを受信します。 在庫システムの POS の例では、端末がトピックにデータを発行します。 その後、在庫管理システムがサブスクリプションからメッセージを受信します。 さらに、監視システムが別のサブスクリプションから、同じメッセージを受信します。 Service Bus のトピックの詳細については、次を参照してください [Service Bus のトピックとサブスクリプションを使用するアプリケーションの作成。](service-bus-create-topics-subscriptions.md) 

アプリケーションの可用性、信頼性、スループットは、複数のメッセージ ブローカーとメッセージング ストアにパーティション分割されたトピックとそのサブスクリプションにより、さらに向上します。 

### パーティション分割されたトピックの作成

パーティション分割されたトピックの作成は、 [Azure クラシック ポータル][] と Service Bus SDK。 パーティション分割されたトピックを作成する設定、 [EnablePartitioning](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.enablepartitioning.aspx) プロパティを **true** で、 [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) インスタンス。 次のコードでは、Service Bus SDK を使用してパーティション分割されたトピックを作成する方法を示します。
    
```
// Create partitioned topic
var nm = NamespaceManager.CreateFromConnectionString(myConnectionString);
var topicDescription = new TopicDescription("myTopic");
topicDescription.EnablePartitioning = true;
nm.CreateTopic(topicDescription);

var subscriptionDescription = new SubscriptionDescription("myTopic", "mySubscription");
nm.CreateSubscription(subscriptionDescription);
```

### AMQP を使用したメッセージの送受信

メッセージを送信し、設定して、プロトコルとしての AMQP を使用してパーティション分割されたトピック サブスクリプションからメッセージを受信、 [TransportType](https://msdn.microsoft.com/library/azure/microsoft.servicebus.servicebusconnectionstringbuilder.transporttype.aspx) プロパティを [TransportType.Amqp](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.transporttype.aspx), の次のコード スニペットに示すようにします。  

```
// Sending and receiving messages to a topic and from a subscription
var myConnectionStringBuilder = new ServiceBusConnectionStringBuilder(myConnectionString);
myConnectionStringBuilder.TransportType = TransportType.Amqp;
string amqpConnectionString = myConnectionStringBuilder.ToString();
    
var topicClient = TopicClient.CreateFromConnectionString(amqpConnectionString, "myTopic");
BrokeredMessage message = new BrokeredMessage("Hello AMQP");
Console.WriteLine("Sending message {0}...", message.MessageId);
topicClient.Send(message);
    
var subcriptionClient = SubscriptionClient.CreateFromConnectionString(amqpConnectionString, "myTopic", "mySubscription");
var receivedMessage = subcriptionClient.Receive();
Console.WriteLine("Received message: {0}", receivedMessage.GetBody<string>());
receivedMessage.Complete();
```

## 次のステップ

パーティション分割されたメッセージング エンティティの詳細については、次を参照してください。

*    [パーティション分割されたメッセージング エンティティ](service-bus-partitioning.md)
*    [OASIS Advanced Message Queuing Protocol (AMQP) バージョン 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
*    [Service Bus での AMQP 1.0 サポート](service-bus-amqp-overview.md)
*    [Service Bus と AMQP 1.0 で Java Message Service (JMS) API を使用する方法に関するページ](service-bus-java-how-to-use-jms-api-amqp.md)
*    [.NET サービス バス API で AMQP 1.0 を使用する方法](service-bus-dotnet-advanced-message-queuing.md)

[Azure classic portal]: http://manage.windowsazure.com

