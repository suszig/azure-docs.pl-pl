<properties 
   pageTitle="AMQP 1.0 での Service Bus と Python | Microsoft Azure"
   description="AMQP で Python から Service Bus を使用します。"
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

# AMQP 1.0 で Python から Service Bus を使用する

[AZURE.INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

Proton-Python は Proton-C にバインドされている Python 言語です。つまり、Proton-Python は、C で実装されているエンジンのラッパーとして実装されています。

## Proton クライアント ライブラリをダウンロードする

PROTON-C および関連するバインド (Python を含む) をダウンロードするにはから [http://qpid.apache.org/download.html](http://qpid.apache.org/download.html)します。 ソース コード形式のファイルがダウンロードされます。 コードをビルドするには、ダウンロードしたパッケージに含まれる指示に従って操作してください。

このドキュメントの執筆時点で、Proton-C での SSL サポートは Linux オペレーティング システムでのみ提供されています。 Azure Service Bus では SSL を使用する必要があるため、Proton-C (および言語バインド) は、現時点では Linux から Service Bus へのアクセスにのみ使用できます。 Windows で Proton-C と SSL を使用できるようにするための作業が進行中です。更新情報を頻繁に確認してください。

## Python を使用した Service Bus のキュー、トピック、およびサブスクリプションの操作

次のコードは、Service Bus メッセージング エンティティからメッセージを送受信する方法を示しています。

### Proton-Python を使用したメッセージの送信

次のコードは、Service Bus メッセージング エンティティにメッセージを送信する方法を示しています。

```
messenger = Messenger()
message = Message()
message.address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"

message.body = u"This is a text string"
messenger.put(message)
messenger.send()
```

### Proton-Python を使用したメッセージの受信

次のコードは、Service Bus メッセージング エンティティからメッセージを受信する方法を示しています。

```
messenger = Messenger()
address = "amqps://[username]:[password]@[namespace].servicebus.windows.net/[entity]"
messenger.subscribe(address)

messenger.start()
messenger.recv(1)
message = Message()
if messenger.incoming:
      messenger.get(message)
messenger.stop()
```

## .NET と Proton-Python 間のメッセージング

### Application properties

#### Proton-Python から Service Bus .NET API へ

Proton-python メッセージは、次の種類のアプリケーションのプロパティをサポートします。 **int**, 、**長い**, 、**float**, 、**uuid**, 、**bool**, 、**文字列**します。 次の Python コードは、これらのプロパティの型を使用してメッセージにプロパティを設定する方法を示しています。

```
message.properties[u"TestString"] = u"This is a string"    
message.properties[u"TestInt"] = 1
message.properties[u"TestLong"] = 1000L
message.properties[u"TestFloat"] = 1.5    
message.properties[u"TestGuid"] = uuid.uuid1()    
```

Service Bus .NET API でメッセージ アプリケーションのプロパティが格納されている、 **プロパティ** のコレクション [BrokeredMessage][]します。 次のコードは、Python クライアントから受信したメッセージのアプリケーションのプロパティを読み取る方法を示しています。

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

次の表は、Python プロパティの型と .NET プロパティの型の対応を示しています。

| Python プロパティの型 | .NET プロパティの型 |
|----------------------|--------------------|
| int                  | int                |
| float                | double             |
| long                 | int64              |
| uuid                 | guid               |
| bool                 | bool               |
| string               | string             |

#### Service Bus .NET API から Proton-Python へ

 [BrokeredMessage][] 型は、次の種類のアプリケーションのプロパティをサポートしています: **バイト**, 、**sbyte**, 、**char**, 、**短い**, 、**ushort**, 、**int**, 、**uint**, 、**長い**, 、**ulong**, 、**float**, 、**二重**, 、**10 進**, 、**bool**, 、**Guid**, 、**文字列**, 、**Uri**, 、**DateTime**, 、**DateTimeOffset**, 、および **TimeSpan**します。 次の .NET コードのプロパティを設定する方法を示しています、 [BrokeredMessage][] オブジェクトの各種類のプロパティを使用しています。

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
message.Properties["TestTimeSpan"] = TimeSpan.FromMinutes(60);
```

次の Python コードは、Service Bus .NET クライアントから受信したメッセージのアプリケーションのプロパティを読み取る方法を示しています。

```
if message.properties != None:
   for k,v in message.properties.items():         
         print "--   %s : %s (%s)" % (k, str(v), type(v))         
```

次の表は、.NET プロパティの型と Python プロパティの型の対応を示しています。

| .NET プロパティの型 | Python プロパティの型 | メモ                                                                                                                                                               |
|--------------------|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| byte               | int                  | -                                                                                                                                                                     |
| sbyte              | int                  | -                                                                                                                                                                     |
| char               | char                 | Proton-python クラス                                                                                                                                                 |
| short              | int                  | -                                                                                                                                                                     |
| ushort             | int                  | -                                                                                                                                                                     |
| int                | int                  | -                                                                                                                                                                     |
| uint               | int                  | -                                                                                                                                                                     |
| long               | int                  | -                                                                                                                                                                     |
| ulong              | long                 | Proton-python クラス                                                                                                                                                 |
| float              | float                | -                                                                                                                                                                     |
| double             | float                | -                                                                                                                                                                     |
| 小数点            | String               | 現在、decimal は Proton ではサポートされません。                                                                                                                     |
| bool               | bool                 | -                                                                                                                                                                     |
| Guid               | uuid                 | Proton-python クラス                                                                                                                                                 |
| 文字列             | 文字列               | -                                                                                                                                                                     |
| DateTime           | timestamp            | Proton-python クラス                                                                                                                                                 |
| DateTimeOffset     | DescribedType        | AMQP 型にマップされる DateTimeOffset.UtcTicks:<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type> |
| TimeSpan           | DescribedType        | AMQP 型にマップされる Timespan.Ticks:<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type>                        |
| Uri                | DescribedType        | AMQP 型にマップされる Uri.AbsoluteUri:<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>                               |

### 標準のプロパティ

次の表は、Proton-python の標準的なメッセージ プロパティの間のマッピングを表示し、 [BrokeredMessage][] 標準のメッセージ プロパティ。

#### Proton-Python から Service Bus .NET API へ

| Proton-Python        | Service Bus .NET         | メモ                                                     |
|----------------------|--------------------------|-----------------------------------------------------------|
| durable              | 該当なし                      | Service Bus は、持続的なメッセージのみをサポートします。               |
| priority             | 該当なし                      | Service Bus は、単一のメッセージの優先度のみをサポートします。      |
| Ttl                  | Message.TimeToLive       | 変換、Proton-Python TTL はミリ秒単位で定義されます。 |
| first\_acquirer      | 該当なし                      | -                                                           |
| delivery\_count      | 該当なし                      | -                                                           |
| ID                   | Message.MessageID        | -                                                           |
| user\_id             | 該当なし                      | -                                                           |
| アドレス              | Message.To               | -                                                           |
| 件名              | Message.Label            | -                                                           |
| reply\_to            | Message.ReplyTo          | -                                                           |
| correlation\_id      | Message.CorrelationID    | -                                                           |
| content\_type        | Message.ContentType      | -                                                           |
| content\_encoding    | 該当なし                      | -                                                           |
| expiry\_time         | 該当なし                      | -                                                           |
| creation\_time       | 該当なし                      | -                                                           |
| group\_id            | Message.SessionId        | -                                                           |
| group\_sequence      | 該当なし                      | -                                                           |
| reply\_to\_group\_id | Message.ReplyToSessionId | -                                                           |
| BlobSink の format               | 該当なし                      | -                                                           |

| Service Bus .NET        | Proton                       | メモ                                                     |
|-------------------------|------------------------------|-----------------------------------------------------------|
| ContentType             | Message.content\_type        | -                                                           |
| CorrelationId           | Message.correlation\_id      | -                                                           |
| EnqueuedTimeUtc         | 該当なし                          | -                                                           |
| ラベル                   | Message.subject              | -                                                           |
| MessageId               | Message.id                   | -                                                           |
| ReplyTo                 | Message.reply\_to            | -                                                           |
| ReplyToSessionId        | Message.reply\_to\_group\_id | -                                                           |
| ScheduledEnqueueTimeUtc | 該当なし                          | -                                                           |
| SessionId               | Message.group\_id            | -                                                           |
| TimeToLive              | Message.ttl                  | 変換、Proton-Python TTL はミリ秒単位で定義されます。 |
| To                      | Message.address              | -                                                           |

## 次のステップ

さらに詳しい情報については、 次のリンク先を参照してください。

- [Service Bus AMQP の概要]
- [Windows Server 用 Service Bus の AMQP]

[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[AMQP in Service Bus for Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx

[Service Bus AMQP overview]: service-bus-amqp-overview.md

