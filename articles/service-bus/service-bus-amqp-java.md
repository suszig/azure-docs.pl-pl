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

Java Message Service (JMS) は、Java プラットフォーム上でメッセージ指向ミドルウェアを操作するための標準 API です。 Azure Service Bus は、Apache Qpid プロジェクトで開発された AMQP 1.0 ベースの JMS クライアント ライブラリでテストされています。 このライブラリは JMS 1.1 API 全体をサポートしており、AMQP 1.0 に準拠するすべてのメッセージング サービスで使用できます。 このシナリオは Service Bus for Windows Server (オンプレミスの Service Bus) でもサポートされています。 詳細については、次を参照してください。 [Service Bus for Windows Server での AMQP][]します。

## Apache Qpid AMQP 1.0 JMS クライアント ライブラリをダウンロードする

Apache Qpid JMS AMQP 1.0 クライアント ライブラリの最新バージョンのダウンロードについては、次を参照してください。 [http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html](http://people.apache.org/~rgodfrey/qpid-java-amqp-1-0-client-jms.html)します。

Service Bus を使用する JMS アプリケーションをビルドおよび実行するときは、次の 4 つの JAR ファイルを Apache Qpid JMS AMQP 1.0 ディストリビューション アーカイブから Java CLASSPATH に追加する必要があります。

-   geronimo-jms\_1.1\_spec-[version].jar

-   qpid-amqp-1-0-client-[version].jar

-   qpid-amqp-1-0-client-jms-[version].jar

-   qpid-amqp-1-0-common-[version].jar

## JMS から Service Bus のキュー、トピック、サブスクリプションを使用する

### Java Naming and Directory Interface (JNDI)

JMS では、Java Naming and Directory Interface (JNDI) を使用して論理名と物理名が関連付けられます。 JNDI を使用して 2 つの種類の JMS オブジェクトが解決済み: **ConnectionFactory** と **宛先**します。 JNDI で使用されるプロバイダー モデルでは、さまざまなディレクトリ サービスに接続して、名前解決のタスクを処理できます。 Apache Qpid JMS AMQP 1.0 ライブラリには、テキスト ファイルを使用して構成される単純なプロパティ ファイルベースの JNDI Provider が付属しています。

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

定義に使用するエントリ、 **ConnectionFactory** Qpid Properties File JNDI Provider では、次の形式。

```
connectionfactory.[jndi_name] = [ConnectionURL]
```

この `[jndi\_name]` と `[ConnectionURL]` には次の意味があります。

| 名前            | 意味                                                                                                                                    |   |   |   |   |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------|---|---|---|---|
| `[jndi\_name]`    | 接続ファクトリの論理名。 この名前は、Java アプリケーションで JNDI `IntialContext.lookup()` メソッドを使用して解決されます。 |   |   |   |   |
| `[ConnectionURL]` | AMQP ブローカーに必要な情報を JMS ライブラリに渡すための URL。                                                      |   |   |   |   |

接続 URL の形式は次のとおりです。

```
amqps://[username]:[password]@[namespace].servicebus.windows.net
```

この `[namespace]`、`[username]`、`[password]` には次の意味があります。

| 名前          | 意味                                                                        |   |   |   |   |
|---------------|--------------------------------------------------------------------------------|---|---|---|---|
| `[namespace]` | 取得した Service Bus 名前空間、 [Azure クラシック ポータル][]します。                      |   |   |   |   |
| `[username]`  | 取得したサービス バス発行者名、 [Azure クラシック ポータル][]します。                    |   |   |   |   |
| `[password]`  | 取得したサービス バス発行者キーの URL でエンコードされた形式、 [Azure クラシック ポータル][]します。 |   |   |   |   |

> [AZURE.NOTE] URL エンコード パスワード手動で、 便利な URL エンコード ユーティリティは、「 [http://www.w3schools.com/tags/ref_urlencode.asp](http://www.w3schools.com/tags/ref_urlencode.asp)します。

たとえば、次の情報をポータルから取得したとします。

| 名前空間:   | test.servicebus.windows.net                  |
|--------------|----------------------------------------------|
| 発行者名: | owner                                        |
| 発行者キー:  | abcdefg |

定義するためにし、 **ConnectionFactory** という名前のオブジェクト `SBCONNECTIONFACTORY`, 、構成文字列に次のようになります。

```
connectionfactory.SBCONNECTIONFACTORY = amqps://owner:abcdefg@test.servicebus.windows.net
```

#### 送信先を構成する

Qpid Properties File JNDI Provider で送信先を定義するエントリの形式を次に示します。

```
queue.[jndi_name] = [physical_name]
topic.[jndi_name] = [physical_name]
```

この `[jndi\_name]` と `[physical\_name]` には次の意味があります。

| 名前              | 意味                                                                                                                                  |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| `[jndi\_name]`    | 送信先の論理名。 この名前は、Java アプリケーションで JNDI `IntialContext.lookup()` メソッドを使用して解決されます。 |
| `[physical\name]` | アプリケーションでメッセージの送信または受信に使用する Service Bus エンティティの名前。                                                  |

以下の点に注意してください。

- `[physical\name]` 値には、Service Bus のキューまたはトピックを指定できます。
- Service Bus トピック サブスクリプションから受信した場合は、JNDI で指定された物理名がトピックの名前になります。 サブスクリプション名は、持続性の高いサブスクリプションが JMS アプリケーション コードで作成されるときに指定されます。
- Service Bus トピック サブスクリプションを JMS キューとして扱うことも可能です。 この方法には、いくつかの利点があります。1 つ目は、キューとトピック サブスクリプションに同じ受信側コードを使用できること、2 つ目は、すべてのアドレス情報 (トピック名とサブスクリプション名) をプロパティ ファイルで具体化できる点です。
- Service Bus トピック サブスクリプションを JMS キューとして扱うには、プロパティ ファイルのエントリを `queue.[jndi\_name] = [topic\_name]/Subscriptions/[subscription\_name]` 形式にする必要があります。|

"topic1" という Service Bus トピックにマップされる "TOPIC" という論理的な JMS の送信先を定義するには、プロパティ ファイルのエントリを次のように定義します。

```
topic.TOPIC = topic1
```

### JMS を使用してメッセージを送信する

次のコードは、Service Bus トピックにメッセージを送信する方法を示しています。 ように仮定 `SBCONNECTIONFACTORY` と `TOPIC` で定義された、 **servicebus.properties** 前のセクションで説明したような構成ファイル。

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env); 
 
ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
MessageProducer producer = session.createProducer(topic);
TextMessage message = session.createTextMessage("This is a text string"); 
producer.send(message);
```

### JMS を使用してメッセージを受信する

次のコードは、Service Bus トピック サブスクリプションからメッセージを受信する方法を示しています。 ように仮定 `SBCONNECTIONFACTORY` でトピックが定義されていると、 **servicebus.properties** 前のセクションで説明したような構成ファイル。 また、サブスクリプション名が `subscription1` であることを前提としています。

```
Hashtable<String, String> env = new Hashtable<String, String>(); 
env.put(Context.INITIAL_CONTEXT_FACTORY, 
        "org.apache.qpid.amqp_1_0.jms.jndi.PropertiesFileInitialContextFactory"); 
env.put(Context.PROVIDER_URL, "servicebus.properties"); 
 
InitialContext context = new InitialContext(env);

ConnectionFactory cf = (ConnectionFactory) context.lookup("SBCONNECTIONFACTORY");
Topic topic = (Topic) context.lookup("TOPIC");
Connection connection = cf.createConnection();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
TopicSubscriber subscriber = session.createDurableSubscriber(topic, "subscription1");
connection.start();
Message message = messageConsumer.receive();
```

### 堅牢なアプリケーションを構築するためのガイドライン

JMS 仕様には API メソッドの例外コントラクトが定義されており、アプリケーション コードはそれらの例外を処理するように記述する必要があります。 例外処理に関して考慮するその他の点は次のとおりです。

-   登録、 **ExceptionListener** JMS 接続を使用して **connection.setExceptionListener**します。 これにより、クライアントは問題の通知を非同期に受け取ることができます。 メッセージのみを使用する接続の場合、接続が失敗したことを知る他の方法がないため、この通知は特に重要です。  **ExceptionListener** が、基になる AMQP 接続、セッション、またはリンクに問題がある場合に呼び出されます。 この場合、アプリケーション プログラムを再作成する必要があります、 **JMS Connection**, 、**セッション**, 、**MessageProducer** と **MessageConsumer** オブジェクトを最初からです。

-   メッセージが正常に送信されたことを確認する、 **MessageProducer** は Service Bus エンティティで、アプリケーションが構成されていることを確認、 **qpid.sync\_publish** システム プロパティが設定されます。 使用したプログラムを起動して、これを行う、 **-Dqpid.sync\_publish=true** Java VM オプションをアプリケーションの起動時に、コマンドラインで設定します。 このオプションを設定すると、メッセージが Service Bus で受け入れられたという確認を受け取るまで、送信呼び出しから戻らないようにライブラリが構成されます。 送信操作中に問題が発生した場合、 **JMSException** が発生します。 原因として、次の 2 つが考えられます。 
    1. Service Bus に送信する特定のメッセージを拒否するため問題がある場合、 **MessageRejectedException** 例外が発生します。 このエラーは一時的なものである場合と、メッセージに原因となる問題がある場合があります。 推奨される対処方法として、バックオフ ロジックを使用して処理を何度か再試行します。 問題が解決しない場合は、エラーをローカルで記録してメッセージを破棄する必要があります。 再作成する必要はありません、 **JMS Connection**, 、**セッション**, 、または **MessageProducer** このような状況でのオブジェクト。 
    2. サービス バスの AMQP リンクを閉じる原因、問題である場合、 **InvalidDestinationException** 例外が発生します。 これは一時的な問題が原因であるか、メッセージ エンティティが削除されていることが原因である可能性があります。 いずれの場合、 **JMS Connection**, 、**セッション**, 、および **MessageProducer** オブジェクトを再作成する必要があります。 エラー状態が一時的なものである場合、この処理は最終的に正常に完了します。 エンティティが削除されている場合、永続的な失敗となります。

## .NET と JMS 間のメッセージング

### メッセージ本文

JMS が 5 つの異なるメッセージの種類を定義する: **BytesMessage**, 、**MapMessage**, 、**ObjectMessage**, 、**StreamMessage**, 、および **テキスト メッセージが**です。 Service Bus .NET API が 1 つのメッセージの種類 [BrokeredMessage][]します。

#### JMS から Service Bus .NET API へ

次のセクションでは、.NET から各種類の JMS メッセージを使用する方法を示します。  **ObjectMessage** 例の本文として含まれていますがされず、 **ObjectMessage** .NET アプリケーションで解釈は Java プログラミング言語でシリアル化可能なオブジェクトが含まれています。

##### BytesMessage

次のコードの本文を使用する方法を示しています、 **BytesMessage** Service Bus .NET Api を使用してオブジェクトです。

```
Stream stream = message.GetBody<Stream>();
int streamLength = (int)stream.Length;

byte[] byteArray = new byte[streamLength];
stream.Read(byteArray, 0, streamLength);

Console.WriteLine("Length = " + streamLength);
for (int i = 0; i < stream.Length; i++)
{
  Console.Write("[" + (sbyte) byteArray[i] + "]");
}
```

##### MapMessage

次のコードの本文を使用する方法を示しています、 **MapMessage** Service Bus .NET Api を使用してオブジェクトです。 このコードは、マップの要素について、各要素の名前と値を表示する処理を繰り返します。

```
Dictionary<String, Object> dictionary = message.GetBody<Dictionary<String, Object>>();

foreach (String mapItemName in dictionary.Keys)
{
  Object mapItemValue = null;
  if (dictionary.TryGetValue(mapItemName, out mapItemValue))
  {
    Console.WriteLine(mapItemName + ":" + mapItemValue);
  }
}
```

##### StreamMessage

次のコードの本文を使用する方法を示しています、 **StreamMessage** Service Bus .NET Api を使用してオブジェクトです。 このコードは、ストリームの各アイテムとその型を一覧表示します。

```
List<Object> list = message.GetBody<List<Object>>();

foreach (Object item in list)
{
  Console.WriteLine(item + " (" + item.GetType() + ")");
}
```

##### TextMessage

次のコードの本文を使用する方法を示しています、 **テキスト メッセージが** Service Bus .NET Api を使用してオブジェクトです。 このコードは、メッセージの本文に含まれるテキスト文字列を表示します。

```
Console.WriteLine("Text: " + message.GetBody<String>());
```

#### Service Bus .NET API から JMS へ

以下のセクションでは、.NET アプリケーションが、各 JMS メッセージの種類の JMS で受信されるメッセージを作成する方法を示します。  **ObjectMessage** 例の本文として含まれていますがされず、 **ObjectMessage** .NET アプリケーションで解釈は Java プログラミング言語でシリアル化可能なオブジェクトが含まれています。

##### BytesMessage

次のコードを作成する方法を示しています、 [BrokeredMessage][] として JMS クライアントが受信した .NET でのオブジェクト、 **BytesMessage**します。

```
byte[] bytes = { 33, 12, 45, 33, 12, 45, 33, 12, 45, 33, 12, 45 };
message = new BrokeredMessage(bytes);
```

##### StreamMessage

次のコードを作成する方法を示しています、 [BrokeredMessage][] として JMS クライアントが受信した .NET でのオブジェクト、 **StreamMessage**します。

```
List<Object> list = new List<Object>();
list.Add("String 1");
list.Add("String 2");
list.Add("String 3");
list.Add((double)3.14159);
message = new BrokeredMessage(list);
```

##### TextMessage

次のコードの本文を使用する方法を示しています、 **テキスト メッセージが** Service Bus .NET API を使用します。 このコードは、メッセージの本文に含まれるテキスト文字列を表示します。

```
message = new BrokeredMessage("this is a text string");
```

### アプリケーションのプロパティ

####JMS から Service Bus .NET API へ

JMS メッセージは、次の種類のアプリケーションのプロパティをサポート: **ブール**, 、**バイト**, 、**短い**, 、**int**, 、**長い**, 、**float**, 、**二重**, 、および **文字列**します。 次の Java コードは、これらのプロパティの各型を使用してメッセージにプロパティを設定する方法を示しています。

```
message.setBooleanProperty("TestBoolean", true); 
message.setByteProperty("TestByte", (byte) 33); 
message.setDoubleProperty("TestDouble", 3.14159D); 
message.setFloatProperty("TestFloat", 3.13159F); 
message.setIntProperty("TestInt", 100); 
message.setStringProperty("TestString", "Service Bus");
```

Service Bus .NET Api でメッセージ アプリケーションのプロパティが格納されている、 **プロパティ** のコレクション [BrokeredMessage][]します。 次のコードは、JMS クライアントから受信したメッセージのアプリケーションのプロパティを読み取る方法を示しています。

```
if (message.Properties.Keys.Count > 0)
{
  foreach (string name in message.Properties.Keys)
  {
    Object value = message.Properties[name];
    Console.WriteLine(name + ": " + value + " (" + value.GetType() + ")" );
  }
  Console.WriteLine();
}
```

次の表は、JMS プロパティの型と .NET プロパティの型の対応を示しています。

| JMS プロパティの型 | .NET プロパティの型 |
|-------------------|--------------------|
| Byte              | sbyte              |
| Integer           | int                |
| Float             | float              |
| Double            | double             |
| Boolean           | bool               |
| String            | string             |

 [BrokeredMessage][] 型は、次の種類のアプリケーションのプロパティをサポートしています: **バイト**, 、**sbyte**, 、**char**, 、**短い**, 、**ushort**, 、**int**, 、**uint**, 、**長い**, 、**ulong**, 、**float**, 、**二重**, 、**decimal**, 、**bool**, 、**Guid**, 、**文字列**, 、**Uri**, 、**DateTime**, 、**DateTimeOffset**, と **TimeSpan**します。 次の .NET コードのプロパティを設定する方法を示しています、 [BrokeredMessage][] オブジェクトの各種類のプロパティを使用しています。

```
message.Properties["TestByte"] = (byte)128;
message.Properties["TestSbyte"] = (sbyte)-22;
message.Properties["TestChar"] = (char) 'X';
message.Properties["TestShort"] = (short)-12345;
message.Properties["TestUshort"] = (ushort)12345;
message.Properties["TestInt"] = (int)-100;
message.Properties["TestUint"] = (uint)100;
message.Properties["TestLong"] = (long)-12345;
message.Properties["TestUlong"] = (ulong)12345;
message.Properties["TestFloat"] = (float)3.14159;
message.Properties["TestDouble"] = (double)3.14159;
message.Properties["TestDecimal"] = (decimal)3.14159;
message.Properties["TestBoolean"] = true;
message.Properties["TestGuid"] = Guid.NewGuid();
message.Properties["TestString"] = "Service Bus";
message.Properties["TestUri"] = new Uri("http://www.bing.com");
message.Properties["TestDateTime"] = DateTime.Now;
message.Properties["TestDateTimeOffSet"] = DateTimeOffset.Now;
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

次の Java コードは、Service Bus .NET クライアントから受信したメッセージのアプリケーションのプロパティを読み取る方法を示しています。

```
Enumeration propertyNames = message.getPropertyNames(); 
while (propertyNames.hasMoreElements()) 
{ 
  String name = (String) propertyNames.nextElement(); 
  Object value = message.getObjectProperty(name); 
  System.out.println(name + ": " + value + " (" + value.getClass() + ")"); 
}
```

次の表は、.NET プロパティの型と JMS プロパティの型の対応を示しています。

| .NET プロパティの型 | JMS プロパティの型 | メモ                                                                                                                                                               |
|--------------------|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| byte               | UnsignedByte      | -                                                                                                                                                                      |
| sbyte              | Byte              | -                                                                                                                                                                     |
| char               | 文字         | -                                                                                                                                                                     |
| short              | ショート             | -                                                                                                                                                                     |
| ushort             | UnsignedShort     | -                                                                                                                                                                     |
| int                | 整数           | -                                                                                                                                                                     |
| uint               | UnsignedInteger   | -                                                                                                                                                                     |
| long               | Long              | -                                                                                                                                                                     |
| ulong              | UnsignedLong      | -                                                                                                                                                                     |
| float              | Float             | -                                                                                                                                                                     |
| double             | Double            | -                                                                                                                                                                     |
| decimal            | BigDecimal        | -                                                                                                                                                                     |
| bool               | Boolean           | -                                                                                                                                                                     |
| Guid               | UUID              | -                                                                                                                                                                     |
| string             | String            | -                                                                                                                                                                     |
| DateTime           | Date              | -                                                                                                                                                                     |
| DateTimeOffset     | DescribedType     | AMQP 型にマップされる DateTimeOffset.UtcTicks:<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type> |
| TimeSpan           | DescribedType     | AMQP 型にマップされる Timespan.Ticks:<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type>                        |
| Uri                | DescribedType     | AMQP 型にマップされる Uri.AbsoluteUri:<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>                               |

### 標準ヘッダー

以下に表示する方法、JMS 標準ヘッダーと [BrokeredMessage][] AMQP 1.0 を使用して標準的なプロパティがマップされます。

#### JMS から Service Bus .NET API へ

| JMS              | Service Bus .NET               | メモ                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|------------------|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| JMSCorrelationID | Message.CorrelationID          | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSDeliveryMode  | 現在使用できません。        | Service Bus は持続性のあるメッセージ; のみをサポートします。たとえば、DeliveryMode.PERSISTENT、指定されている内容に関係なく。                                                                                                                                                                                                                                                                                                                                                         |
| JMSDestination   | Message.To                     | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSExpiration    | メッセージです。 TimeToLive            | Conversion                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSMessageID     | Message.MessageID              | 既定では、JMSMessageID は AMQP メッセージにバイナリ形式でエンコードされます。 バイナリ message-id を受信すると、.NET クライアント ライブラリはバイトの Unicode 値に基づいて文字列表現に変換します。 文字列メッセージ ID を使用するように JMS ライブラリを切り替えるには、"binary-messageid=false" 文字列を JNDI ConnectionURL のクエリ パラメーターに追加します。 例:"amqps://[username]:[password]@[namespace].servicebus.windows.net でしょうか。 .servicebus.windows.net? binary-messageid = false"です。 |
| JMSPriority      | 現在使用できません。        | Service Bus はメッセージの優先度をサポートしていません。                                                                                                                                                                                                                                                                                                                                                                                                                             |
| JMSRedelivered   | 現在使用できません。        | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| JMSReplyTo       | メッセージです。 ReplyTo               | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| JMSTimestamp     | Message.EnqueuedTimeUtc        | Conversion                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| JMSType          | Message.Properties ["jms type"] | -                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

#### Service Bus .NET API から JMS へ

| Service Bus .NET        | JMS              | メモ                   |
|-------------------------|------------------|-------------------------|
| ContentType             | -                  | 現在使用できません。 |
| CorrelationId           | JMSCorrelationID | -                         |
| EnqueuedTimeUtc         | JMSTimestamp     | Conversion              |
| ラベル                   | 該当なし              | 現在使用できません。 |
| MessageId               | JMSMessageID     | -                         |
| ReplyTo                 | JMSReplyTo       | -                         |
| ReplyToSessionId        | 該当なし              | 現在使用できません。 |
| ScheduledEnqueueTimeUtc | 該当なし              | 現在使用できません。 |
| SessionId               | 該当なし              | 現在使用できません。 |
| TimeToLive              | JMSExpiration    | Conversion              |
| To                      | JMSDestination   | -                         |

## サポートされていない機能および制限

Service Bus で AMQP 1.0 を介して JMS を使用する場合は、次の制限があります。

-   1 つだけ **MessageProducer** または **MessageConsumer** セッションごとに許可されます。 複数作成する場合は、 **MessageProducer** または **MessageConsumer** アプリケーションでは、オブジェクトは、それぞれの専用のセッションを作成します。

-   揮発性トピック サブスクリプションは現在サポートされていません。

-   **MessageSelector** オブジェクトがサポートされていません。

-   一時的な送信先です。たとえば、 **TemporaryQueue** または **TemporaryTopic**, 、されていないと一緒にサポート、 **QueueRequestor** と **TopicRequestor** Api を使用します。

-   トランザクション セッションはサポートされません。

-   分散トランザクションはサポートされません。

## 次のステップ

さらに詳しい情報については、 次のリンク先を参照してください。

- [Service Bus AMQP の概要]
- [Windows Server 用 Service Bus の AMQP]

[AMQP in Service Bus for Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx
[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx

[Service Bus AMQP overview]: service-bus-amqp-overview.md
[Azure classic portal]: http://manage.windowsazure.com


