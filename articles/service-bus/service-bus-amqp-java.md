<properties 
   pageTitle="AMQP 1.0 での Service Bus と Java | Microsoft Azure"
   description="AMQP で Java から Service Bus を使用します。"
   services="service-bus"
   documentationCenter="na"
   authors="sethmanheim"
   manager="timlt"
   editor="tysonn" /> 
<tags 
   ms.service="service-bus"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2015"
   ms.author="sethm" />


# AMQP 1.0 で Java から Service Bus を使用する

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Java Message Service (JMS) は、Java プラットフォーム上でメッセージ指向ミドルウェアを操作するための標準 API です。 Azure Service Bus は、Apache Qpid プロジェクトで開発された AMQP 1.0 ベースの JMS クライアント ライブラリでテストされています。 このライブラリは JMS 1.1 API 全体をサポートしており、AMQP 1.0 に準拠するすべてのメッセージング サービスで使用できます。 このシナリオは Service Bus for Windows Server (オンプレミスの Service Bus) でもサポートされています。 詳細については、次を参照してください。 [Service Bus for Windows サーバー [] での AMQP][]します。

## Apache Qpid AMQP 1.0 JMS クライアント ライブラリをダウンロードする

Apache Qpid JMS AMQP 1.0 クライアント ライブラリの最新バージョンのダウンロードについては、次を参照してください。 [http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html](http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html)します。

Service Bus を使用する JMS アプリケーションをビルドおよび実行するときは、次の 4 つの JAR ファイルを Apache Qpid JMS AMQP 1.0 ディストリビューション アーカイブから Java CLASSPATH に追加する必要があります。

-   geronimo-jms\_1.1\_spec-[version].jar

-   qpid-amqp-1-0-client-[version].jar

-   qpid-amqp-1-0-client-jms-[version].jar

-   qpid-amqp-1-0-common-[version].jar

## JMS から Service Bus のキュー、トピック、サブスクリプションを使用する

### Java Naming and Directory Interface (JNDI)

JMS では、Java Naming and Directory Interface (JNDI) を使用して論理名と物理名が関連付けられます。 2 種類の JMS オブジェクト、**ConnectionFactory** および **Destination** は JNDI を使用して解決されます。 JNDI で使用されるプロバイダー モデルでは、さまざまなディレクトリ サービスに接続して、名前解決のタスクを処理できます。 Apache Qpid JMS AMQP 1.0 ライブラリには、テキスト ファイルを使用して構成される単純なプロパティ ファイルベースの JNDI Provider が付属しています。

Qpid Properties File JNDI Provider は、次の形式のプロパティ ファイルを使用して構成されます。

```
# servicebus.properties – sample JNDI configuration

# Register a ConnectionFactory in JNDI using the form:
# connectionfactory.[jndi_name] = [ConnectionURL]
connectionfactory.SBCONNECTIONFACTORY = amqps://[username]:[password]@[namespace].servicebus.windows.net

# Register some queues in JNDI using the form
# queue.[jndi_name] = [physical_name]
# topic.[jndi_name] = [physical_name]
topic.TOPIC = topic1
queue.QUEUE = queue1
```

#### 接続ファクトリを構成する

Qpid Properties File JNDI Provider で **ConnectionFactory** の定義に使用するエントリは、次のような形式になります。

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

ここで `[jndi \_name]` と `[ConnectionURL]` は次の意味があります。

| 名前| 意味| | | | |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------|---|---|---|---|
| `[jndi \_name]`| 接続ファクトリの論理名。この名前は、JNDI を使用して Java アプリケーションで解決 `IntialContext.lookup()` メソッドです。| | | | |
| `[ConnectionURL]`| AMQP ブローカーに必要な情報を JMS ライブラリに渡すための URL。| | | | |

接続 URL の形式は次のとおりです。

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

Where `[namespace]`, `[username]`, and `[password]` have the following meanings:

| Name          | Meaning                                                                        |   |   |   |   |
|---------------|--------------------------------------------------------------------------------|---|---|---|---|
| `[namespace]` | The Service Bus namespace obtained from the [Azure classic portal][].                      |   |   |   |   |
| `[username]`  | The Service Bus issuer name obtained from the [Azure classic portal][].                    |   |   |   |   |
| `[password]`  | URL-encoded form of the Service Bus issuer key obtained from the [Azure classic portal][]. |   |   |   |   |

> [AZURE.NOTE] You must URL-encode the password manually. A useful URL encoding utility is available at [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp).

For example, if the information obtained from the portal is as follows:

| Namespace:   | test.servicebus.windows.net                  |
|--------------|----------------------------------------------|
| Issuer name: | owner                                        |
| Issuer key:  | abcdefg |

Then in order to define a **ConnectionFactory** object named `SBCONNECTIONFACTORY`, the configuration string would be as follows:
```
connectionfactory します。SBCONNECTIONFACTORY amqps://owner:abcdefg@test.servicebus.windows.net =
```

#### Configuring destinations

The entry that defines a destination in the Qpid Properties File JNDI Provider is of the following format:
```
キューです。[jndi_name] = [physical_name]
トピックです。[jndi_name] = [physical_name]
```

Where `[jndi\_name]` and `[physical\_name]` have the following meanings:

| Name              | Meaning                                                                                                                                  |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| `[jndi\_name]`    | The logical name of the destination. This is name is resolved in the Java application by using the JNDI `IntialContext.lookup()` method. |
| `[physical\name]` | The name of the Service Bus entity to which the application sends or receives messages.                                                  |

Note the following:

- The `[physical\name]` value can be a Service Bus queue or topic.
- When receiving from a Service Bus topic subscription, the physical name specified in JNDI should be the name of the topic. The subscription name is provided when the durable subscription is created in the JMS application code.
- It is also possible to treat a Service Bus topic subscription as a JMS Queue. There are several advantages to this approach: the same receiver code can be used for queues and topic subscriptions, and all the address information (the topic and subscription names) is externalized in the properties file.
- To treat a Service Bus topic subscription as a JMS Queue, the entry in properties file should be of the form: `queue.[jndi\_name] = [topic\_name]/Subscriptions/[subscription\_name]`.|

To define a logical JMS destination named "TOPIC" that maps to a Service Bus topic named "topic1," the entry in the properties file would be as follows:
```
トピックです。トピック topic1 =
```

### Sending messages using JMS

The following code shows how to send a message to a Service Bus topic. It is assumed that `SBCONNECTIONFACTORY` and `TOPIC` are defined in a **servicebus.properties** configuration file as described in the previous section.
```
ハッシュ テーブル<String, String> env 新しいハッシュ テーブルを =<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties");

InitialContext コンテキスト = 新しい InitialContext(env) です。

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY") です。
トピックのトピック (トピック) context.lookup("TOPIC"); を =
Connection connection = cf.createConnection();
セッション = connection.createSession (false、Session.AUTO_ACKNOWLEDGE) です。
MessageProducer プロデューサー = session.createProducer(topic) です。
テキスト メッセージがメッセージ = session.createTextMessage ("This is テキスト文字列") です。 
producer.send(message) です。
```

### Receiving messages using JMS

The following code shows `how` to receive a message from a Service Bus topic subscription. It is assumed that `SBCONNECTIONFACTORY` and TOPIC are defined in a **servicebus.properties** configuration file as described in the previous section. It is also assumed that the subscription name is `subscription1`.
```
ハッシュ テーブル<String, String> env 新しいハッシュ テーブルを =<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties");

InitialContext コンテキスト = 新しい InitialContext(env) です。

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY") です。
トピックのトピック (トピック) context.lookup("TOPIC"); を =
Connection connection = cf.createConnection();
セッション = connection.createSession (false、Session.AUTO_ACKNOWLEDGE) です。
TopicSubscriber サブスクライバー = session.createDurableSubscriber (トピック、"subscription1") です。
connection.start() です。
メッセージのメッセージ = messageConsumer.receive() です。
```

### Guidelines for building robust applications

The JMS specification defines how the exception contract of the API methods and application code should be written to handle such exceptions. Here are some other points to consider regarding exception handling:

-   Register an **ExceptionListener** with the JMS connection using **connection.setExceptionListener**. This enables a client to be notified of a problem asynchronously. This notification is particularly important for connections that only consume messages, as they would have no other way to learn that their connection has failed. The **ExceptionListener** is called if there is a problem with the underlying AMQP connection, session, or link. In this situation, the application program should recreate the **JMS Connection**, **Session**, **MessageProducer** and **MessageConsumer** objects from scratch.

-   To verify that a message has been successfully sent from a **MessageProducer** to a Service Bus entity, ensure that the application has been configured with the **qpid.sync\_publish** system property set. You can do this by starting the program with the **-Dqpid.sync\_publish=true** Java VM option set on the command line when starting the application. Setting this option configures the library to not return from the send call until confirmation has been received that the message has been accepted by Service Bus. If a problem occurs during the send operation, a **JMSException** is raised. There are two possible causes: 
    1. If the problem is due to Service Bus rejecting the particular message being sent, then a **MessageRejectedException** exception will be raised. This error is either transitory, or due to some problem with the message. The recommended course of action is to make several attempts to retry the operation with some back-off logic. If the problem persists, then the message should be abandoned with an error logged locally. There is no need to recreate the **JMS Connection**, **Session**, or **MessageProducer** objects in this situation. 
    2. If the problem is due to Service Bus closing the AMQP Link, then an **InvalidDestinationException** exception will be raised. This can be due to a transient problem, or due to the message entity being deleted. In either case, the **JMS Connection**, **Session**, and **MessageProducer** objects should be recreated. If the error condition was transient, then this operation will eventually be successful. If the entity has been deleted, the failure will be permanent.

## Messaging between .NET and JMS

### Message bodies

JMS defines five different message types: **BytesMessage**, **MapMessage**, **ObjectMessage**, **StreamMessage**, and **TextMessage**. The Service Bus .NET API has a single message type, [BrokeredMessage][].

#### JMS to Service Bus .NET API

The following sections show how to consume messages of each of the JMS message types from .NET. An **ObjectMessage** example has not been included, as the body of an **ObjectMessage** contains a serializable object in the Java programming language, which is not interpretable by a .NET application.

##### BytesMessage

The following code shows how to consume the body of a **BytesMessage** object by using the Service Bus .NET APIs.
```
ストリームのストリーム = メッセージです。GetBody<Stream>();
int streamLength = (int) のストリーム。長さです。

バイト byteArray = 新しいバイト [streamLength] です。
ストリーム。(ByteArray、0、streamLength) を読み取る。

Console.WriteLine ("の長さ ="+ streamLength) です。
(int i = 0; は < ストリームします。長さです。i++)
{
  Console.Write ("["+ (sbyte) byteArray [i] +"]") です。
{
```

##### MapMessage

The following code shows how to consume the body of a **MapMessage** object by using the Service Bus .NET APIs. This code iterates through the elements of the map, displaying the name and value of each element.
```
ディクショナリ<String, Object> ディクショナリ = メッセージです。GetBody<Dictionary<String, Object>> ();

foreach (ディクショナリの文字列 mapItemName します。キー)
{
  オブジェクトの mapItemValue = null;
  場合 (ディクショナリ。TryGetValue (mapItemValue アウト mapItemName))
  {
    Console.WriteLine (mapItemName +":"+ mapItemValue) です。
  {
{
```

##### StreamMessage

The following code shows how to consume the body of a **StreamMessage** object by using the Service Bus .NET APIs. This code lists each of the items from the stream, together with their types.
```
リスト<Object> リスト = メッセージです。GetBody<List<Object>> ();

foreach (オブジェクトのリスト内の項目)
{
  Console.WriteLine (項目 +"("+ 項目。GetType() +")") です。
{
```

##### TextMessage

The following code shows how to consume the body of a **TextMessage** object by using the Service Bus .NET APIs. This code displays the text string contained in the body of the message.
```
Console.WriteLine ("テキスト:"+ メッセージです。GetBody<String>()) です。
```

#### Service Bus .NET APIs to JMS

The following sections show how a .NET application can create a message that is received in JMS in each of the different JMS message types. An **ObjectMessage** example has not been included, as the body of an **ObjectMessage** contains a serializable object in the Java programming language, which is not interpretable by a .NET application.

##### BytesMessage

The following code shows how to create a [BrokeredMessage][] object in .NET that is received by a JMS client as a **BytesMessage**.
```
バイトのバイト数 = {33、12、45、33、12、45、33、12、45、33、12、45};
メッセージ = 新しい BrokeredMessage(bytes) です。
```

##### StreamMessage

The following code shows how to create a [BrokeredMessage][] object in .NET that is received by a JMS client as a **StreamMessage**.
```
リスト<Object> リスト新しいリストを =<Object>();
リストです。追加 (「文字列の 1」) です。
リストです。追加 (「文字列 2」) です。
リストです。追加 (文字列"String 3") です。
リストです。Add((double)3.14159) です。
メッセージ = 新しい BrokeredMessage(list) です。
```

##### TextMessage

The following code shows how to consume the body of a **TextMessage** using the Service Bus .NET API. This code displays the text string contained in the body of the message.
```
メッセージ = 新しい BrokeredMessage が ("this is テキスト文字列") です。
```

### Application properties

####JMS to Service Bus .NET APIs

JMS messages support application properties of the following types: **boolean**, **byte**, **short**, **int**, **long**, **float**, **double**, and **String**. The following Java code shows how to set properties on a message by using each of these property types.
```
message.setBooleanProperty ("TestBoolean"は true)。 
message.setByteProperty ("TestByte"、33 (バイト単位)) です。 
message.setDoubleProperty ("TestDouble"、3.14159 D)。 
message.setFloatProperty ("TestFloat"、3.13159F) です。 
message.setIntProperty ("TestInt"、100)。 
message.setStringProperty ("TestString"、"Service Bus") です。
```

In the Service Bus .NET APIs, message application properties are carried in the **Properties** collection of [BrokeredMessage][]. The following code shows how to read the application properties of a message received from a JMS client.
```
場合 (メッセージです。Properties.Keys.Count > 0)
{
  foreach (メッセージの文字列名。Properties.Keys)
  {
    オブジェクトの値 = メッセージです。[Name]; のプロパティ
    Console.WriteLine (名前 +":"+ 値 +"("+ 値。GetType() +")") です。
  {
  Console.WriteLine();
{
```

The following table shows how the JMS property types map to the .NET property types.

| JMS Property Type | .NET Property Type |
|-------------------|--------------------|
| Byte              | sbyte              |
| Integer           | int                |
| Float             | float              |
| Double            | double             |
| Boolean           | bool               |
| String            | string             |

The [BrokeredMessage][] type supports application properties of the following types: **byte**, **sbyte**, **char**, **short**, **ushort**, **int**, **uint**, **long**, **ulong**, **float**, **double**, **decimal**, **bool**, **Guid**, **string**, **Uri**, **DateTime**, **DateTimeOffset**, and **TimeSpan**. The following .NET code shows how to set properties on a [BrokeredMessage][] object using each of these property types.
```
メッセージです。プロパティ ["TestByte"] = 128; (バイト単位)
メッセージです。プロパティ ["TestSbyte"] = (sbyte) ~ 22 日です。
メッセージです。プロパティ ["TestChar"] = 型 (char) 'X';
メッセージです。プロパティ ["TestShort"] = (短い)-12345 です。
メッセージです。プロパティ ["TestUshort"] = (ushort) 12345 です。
メッセージです。プロパティ ["TestInt"] = (int)-100 です。
メッセージです。プロパティ ["TestUint"] = (uint) 100 です。
メッセージです。プロパティ ["TestLong"] = (long)-12345 です。
メッセージです。プロパティ ["TestUlong"] = (ulong) 12345 です。
メッセージです。プロパティ ["TestFloat"] = (float) 3.14159 です。
メッセージです。プロパティ ["TestDouble"] = (double) 3.14159 です。
メッセージです。プロパティ ["TestDecimal"] = 3.14159 の (10 進数)。
メッセージです。プロパティ ["TestBoolean"] = true;
メッセージです。プロパティ ["TestGuid"] = Guid.NewGuid() です。
メッセージです。プロパティ ["TestString"] ="Service Bus"です。
メッセージです。プロパティ ["TestUri"] = 新しい Uri ("http://www.bing.com") です。
メッセージです。プロパティ ["TestDateTime"] = DateTime.Now です。
メッセージです。プロパティ ["TestDateTimeOffSet"] = DateTimeOffset.Now です。
メッセージです。プロパティ ["TestTimeSpan"] = TimeSpan.FromMinutes(60) です。
```

The following Java code shows how to read the application properties of a message received from a Service Bus .NET client.
```
列挙体の propertyNames = message.getPropertyNames() です。 
(propertyNames.hasMoreElements()) 中 
{ 
  文字列名 = (文字列) propertyNames.nextElement() です。 
  オブジェクトの値 = message.getObjectProperty(name) です。 
  System.out.println (名前 +":"+ 値 +"("+ value.getClass() +")") です。 
{
```

次の表は、.NET プロパティの型と JMS プロパティの型の対応を示しています。

| .NET プロパティの型| JMS プロパティの型| メモ|
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| byte| UnsignedByte| -|
| sbyte| Byte| -|
| char| 文字| -|
| short| ショート| -|
| ushort| UnsignedShort| -|
| int| 整数| -|
| uint| UnsignedInteger| -|
| long| Long| -|
| ulong| UnsignedLong| -|
| float| Float| -|
| double| Double| -|
| 小数点| BigDecimal| -|
| bool| Boolean| -|
| Guid| UUID| -|
| string| String| -|
| DateTime| 日付| -|
| DateTimeOffset| DescribedType| AMQP 型にマップされる DateTimeOffset.UtcTicks:<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>|
| TimeSpan| DescribedType| AMQP 型にマップされる Timespan.Ticks:<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type>|
| Uri| DescribedType| AMQP 型にマップされる Uri.AbsoluteUri:<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>|

### 標準ヘッダー

以下に表示する方法、JMS 標準ヘッダーと [BrokeredMessage][] AMQP 1.0 を使用して標準的なプロパティがマップされます。

#### JMS から Service Bus .NET API へ

| JMS| Service Bus .NET| メモ|
|------------------|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| JMSCorrelationID| Message.CorrelationID| -|
| JMSDeliveryMode| 現在使用できません。| Service Bus は持続性のあるメッセージ; のみをサポートします。たとえば、DeliveryMode.PERSISTENT、指定されている内容に関係なく。|
| JMSDestination| Message.To| -|
| JMSExpiration| メッセージです。TimeToLive| 変換|
| JMSMessageID| Message.MessageID| 既定では、JMSMessageID は AMQP メッセージにバイナリ形式でエンコードされます。バイナリ message-id を受信すると、.NET クライアント ライブラリはバイトの Unicode 値に基づいて文字列表現に変換します。文字列メッセージ ID を使用するように JMS ライブラリを切り替えるには、"binary-messageid=false" 文字列を JNDI ConnectionURL のクエリ パラメーターに追加します。例:"amqps://[username]:[password]@[namespace].servicebus.windows.net でしょうか。.servicebus.windows.net? binary-messageid = false"です。|
| JMSPriority| 現在使用できません。| Service Bus はメッセージの優先度をサポートしていません。|
| JMSRedelivered| 現在使用できません。| -|
| JMSReplyTo| メッセージです。ReplyTo| -|
| JMSTimestamp| Message.EnqueuedTimeUtc| 変換|
| JMSType| Message.Properties ["jms type"]| -|

#### Service Bus .NET API から JMS へ

| Service Bus .NET| JMS| メモ|
|-------------------------|------------------|-------------------------|
| ContentType| -| 現在使用できません。|
| CorrelationId| JMSCorrelationID| -|
| EnqueuedTimeUtc| JMSTimestamp| 変換|
| ラベル| 該当なし| 現在使用できません。|
| MessageId| JMSMessageID| -|
| ReplyTo| JMSReplyTo| -|
| ReplyToSessionId| 該当なし| 現在使用できません。|
| ScheduledEnqueueTimeUtc| 該当なし| 現在使用できません。|
| SessionId| 該当なし| 現在使用できません。|
| TimeToLive| JMSExpiration| 変換|
| To| JMSDestination| -|

## サポートされていない機能および制限

Service Bus で AMQP 1.0 を介して JMS を使用する場合は、次の制限があります。

-   セッションごとに作成できる **MessageProducer** または **MessageConsumer** は 1 つのみです。 アプリケーションで複数の **MessageProducer** オブジェクトまたは **MessageConsumer** オブジェクトを作成する場合は、それぞれに専用のセッションを作成してください。

-   揮発性トピック サブスクリプションは現在サポートされていません。

-   **MessageSelector** オブジェクトはサポートされません。

-   **TemporaryQueue** や **TemporaryTopic** などの一時的な送信先と、それらを使用する **QueueRequestor** API と **TopicRequestor** API はサポートされません。

-   トランザクション セッションはサポートされません。

-   分散トランザクションはサポートされません。

## 次のステップ

さらに詳しい情報については、 次のリンク先を参照してください。

- [Service Bus AMQP の概要]
- [Service Bus for Windows Server での AMQP]


[amqp in service bus for windows server]: https://msdn.microsoft.com/library/dn574799.aspx 
[brokeredmessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx 
[service bus amqp overview]: service-bus-amqp-overview.md 
[azure classic portal]: http://manage.windowsazure.com 

