<properties 
    pageTitle="Service Bus トピックの使用方法 (Python) | Microsoft Azure" 
    description="Python から Azure Service Bus のトピックとサブスクリプションを使用する方法を説明します。" 
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

# Service Bus のトピックとサブスクリプションの使用方法

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

この記事では、Service Bus のトピックとサブスクリプションの使用方法について説明します。 サンプルは Python で記述され使用する、 [Python Azure パッケージ][]します。 紹介するシナリオ **トピックとサブスクリプションの作成**, 、**サブスクリプション フィルターの作成**, 、**トピックにメッセージを送信する**, 、**サブスクリプションからメッセージを受信**, 、および **トピックとサブスクリプションの削除**します。 トピックとサブスクリプションの詳細については、次を参照してください。、 [次のステップ](#next-steps) セクションです。

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

**注:** Python をインストールする必要がある場合、または [Python Azure パッケージ][], を参照してください、 [Python インストール ガイド](../python-how-to-install.md)します。

## トピックを作成する

 **ServiceBusService** オブジェクトでは、トピックを操作することができます。 プログラムを使用して Service Bus にアクセスするすべての Python ファイルの先頭付近に、次のコードを追加します。

```
from azure.servicebus import ServiceBusService, Message, Topic, Rule, DEFAULT_RULE_NAME
```

次のコード作成、 **ServiceBusService** オブジェクトです。 `mynamespace`、`sharedaccesskeyname`、`sharedaccesskey` の部分は、実際の名前空間、Shared Access Signature (SAS) キー名、キー値に置き換えます。

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

SAS キー名の値を取得し、値を [Azure クラシック ポータル][] **接続情報** ウィンドウです。

```
bus_service.create_topic('mytopic')
```

**create \_topic** もメッセージの有効期間や最大トピック サイズなどの既定のトピックの設定を上書きすることを有効にする追加のオプションをサポートしています。 次の例では、最大トピック サイズを 5 GB に、有効期間 (TTL) を 1 分に設定します。

```
topic_options = Topic()
topic_options.max_size_in_megabytes = '5120'
topic_options.default_message_time_to_live = 'PT1M'

bus_service.create_topic('mytopic', topic_options)
```

## サブスクリプションを作成する

トピックへのサブスクリプションもで作成、 **ServiceBusService** オブジェクトです。 サブスクリプションを指定し、サブスクリプションの仮想キューに配信するメッセージを制限するフィルターを設定することができます。

> [AZURE.NOTE] サブスクリプションは、永続的なまたは先の [購読している、削除は、トピックのいずれかのされるまで存在し続けます。

### 既定の (MatchAll) フィルターを適用したサブスクリプションの作成

 **MatchAll** フィルターは、新しいサブスクリプションが作成されるときにフィルターが指定されていない場合に使用される既定のフィルターです。 ときに、 **MatchAll** フィルターを使用して、トピックに発行されたすべてのメッセージがサブスクリプションの仮想キューに置かれます。 次の例は、"allmessages"という名前のサブスクリプションを作成し、既定値を使用して **MatchAll**
フィルターを使用します。

```
bus_service.create_subscription('mytopic', 'AllMessages')
```

### フィルターを適用したサブスクリプションの作成

トピックに送信されたメッセージのうち、特定のトピック サブスクリプション内に表示されるメッセージを指定するフィルターを定義することもできます。

サブスクリプションでサポートされるフィルターの最も柔軟性の高い型は、 **SqlFilter**, 、SQL92 のサブセットを実装します。 SQL フィルターは、トピックに発行されるメッセージのプロパティに対して適用されます。 SQL フィルターで使用できる式の詳細については、次を参照してください。、 [SqlFilter.SqlExpression][] 構文です。

フィルターをサブスクリプションに追加するにを使用して、 **create \_rule** のメソッド、 **ServiceBusService** オブジェクトです。 このメソッドによって、新しいフィルターを既存のサブスクリプションに追加できます。

> [AZURE.NOTE] 既定のフィルターはすべての新しいサブスクリプションに自動的に適用されるため、既定のフィルターをまず削除する必要がありますか **MatchAll** その他のフィルターを指定することよりも優先されます。 既定のルールを削除するを使用して、 **delete \_rule** のメソッド、 **ServiceBusService** オブジェクトです。

という名前のサブスクリプションを作成する例を次 `HighMessages` で、 **SqlFilter** のみに、カスタム メッセージを選択する **messagenumber** 3 を超えるプロパティ。

```
bus_service.create_subscription('mytopic', 'HighMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber > 3'

bus_service.create_rule('mytopic', 'HighMessages', 'HighMessageFilter', rule)
bus_service.delete_rule('mytopic', 'HighMessages', DEFAULT_RULE_NAME)
```

同様に、次の例がという名前のサブスクリプションを作成 `LowMessages` で、 **SqlFilter** のみを持つメッセージを選択する、 **messagenumber** 3 のプロパティが。

```
bus_service.create_subscription('mytopic', 'LowMessages')

rule = Rule()
rule.filter_type = 'SqlFilter'
rule.filter_expression = 'messagenumber <= 3'

bus_service.create_rule('mytopic', 'LowMessages', 'LowMessageFilter', rule)
bus_service.delete_rule('mytopic', 'LowMessages', DEFAULT_RULE_NAME)
```

ここで、メッセージが送信される場合に `mytopic` をサブスクライブした受信者に必ず配信、 **AllMessages** トピック サブスクリプションにサブスクライブされた受信者に対して選択的に配信されると、 **HighMessages** と **LowMessages** (メッセージの内容に応じてトピックのサブスクリプション。

## メッセージをトピックに送信する

Service Bus トピックにメッセージを送信するアプリケーションを使用する必要があります、 **send \_topic\_message** のメソッド、 **ServiceBusService** オブジェクトです。

次の例では、`mytopic` トピックに 5 件のテスト メッセージを送信する方法を示しています。 なお、 **messagenumber** 、ループの反復処理で各メッセージのプロパティの値が変化 ([受信するサブスクリプションが決定されます)。

```
for i in range(5):
    msg = Message('Msg {0}'.format(i).encode('utf-8'), custom_properties={'messagenumber':i})
    bus_service.send_topic_message('mytopic', msg)
```

Service Bus トピックでは、最大 256 MB までのメッセージをサポートしています (標準とカスタムのアプリケーション プロパティが含まれるヘッダーの最大サイズは 64 MB です)。 トピックで保持されるメッセージ数には上限がありませんが、1 つのトピックで保持できるメッセージの合計サイズには上限があります。 このトピックのサイズはトピックの作成時に定義します。上限は 5 GB です。 クォータの詳細については、次を参照してください。 [Azure キューと Service Bus キュー][]します。

## サブスクリプションからメッセージを受信する

使用してサブスクリプションからメッセージを受信、 **\_subscription\_message** メソッドを **ServiceBusService** オブジェクト。

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=False)
print(msg.body)
```

読み取られるときに、メッセージがサブスクリプションから削除されますパラメーター **\_lock** に設定されている **False**します。 (ピークして) を参照し、パラメーターを設定して、キューから削除することがなく、メッセージをロックできます **\_lock** に **True**します。

受信操作の中で行われるメッセージの読み取りと削除の動作は、最もシンプルなモデルであり、障害発生時にアプリケーション側でメッセージを処理しないことを許容できるシナリオに最適です。 このことを理解するために、コンシューマーが受信要求を発行した後で、メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。 Service Bus はメッセージを読み取り済みとしてマークするため、アプリケーションが再起動してメッセージの読み取りを再開すると、クラッシュ前に読み取られていたメッセージは見落とされることになります。

場合、 **\_lock** にパラメーターが設定されている **True**, 、受信メッセージが失われることが許容できないアプリケーションをサポートすることが 2 段階の操作になります。 Service Bus は要求を受け取ると、次に読み取られるメッセージを検索して、他のコンシューマーが受信できないようロックしてから、アプリケーションにメッセージを返します。 呼び出して受信処理の第 2 段階を完了したら、アプリケーションがメッセージの処理 (または後で処理するために確実に保存) **削除** メソッドを **メッセージ** オブジェクトです。  **削除** メソッドに、メッセージを読み取り中としてマークし、サブスクリプションから削除します。

```
msg = bus_service.receive_subscription_message('mytopic', 'LowMessages', peek_lock=True)
print(msg.body)

msg.delete()
```

## アプリケーションのクラッシュと読み取り不能のメッセージを処理する方法

Service Bus には、アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 受信側アプリケーションが何らかの理由でのメッセージを処理できないかどうかは、呼び出すことができます、 **のロックを解除** メソッドを **メッセージ** オブジェクトです。 このメソッドが呼び出されると、Service Bus によってサブスクリプション内のメッセージのロックが解除され、メッセージが再度受信できる状態に変わります。このメッセージは、同じコンシューマー側アプリケーションまたは他のコンシューマー側アプリケーションで受信されます。

サブスクリプション内でロックされているメッセージにはタイムアウトも設定されています。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合には、Service Bus によりメッセージのロックが自動的に解除され、再度受信できる状態に変わります。

前に、メッセージを処理した後、アプリケーションがクラッシュすること、 **削除** メソッドが呼び出されると、再起動する際にそのメッセージがアプリケーションに再配信されます。 一般的に、 **1 回の処理には、少なくとも**, 、つまり、各メッセージが 1 回以上処理されますが、特定の状況で、同じメッセージが再配信されます。 重複処理が許されないシナリオの場合、重複メッセージの配信を扱うロジックをアプリケーションに追加する必要があります。 これは、多くの場合を使用して、 **MessageId** は一定に保たれる配信が試行されると、メッセージのプロパティです。

## トピックとサブスクリプションを削除する

トピックおよびサブスクリプションは永続的であり、明示的にする必要がありますから削除された、 [Azure クラシック ポータル][] またはプログラムを使用しています。 次の例では、`mytopic` という名前のトピックを削除する方法を示しています。

```
bus_service.delete_topic('mytopic')
```

トピックを削除すると、そのトピックに登録されたサブスクリプションもすべて削除されます。 サブスクリプションは、個別に削除することもできます。 次のコードでは、`HighMessages` という名前のサブスクリプションを `mytopic` トピックから削除する方法を示しています。

```
bus_service.delete_subscription('mytopic', 'HighMessages')
```

## 次のステップ

これで、Service Bus トピックの基本を学習できました。さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

-   参照してください [キュー、トピック、およびサブスクリプション][]します。
-   参照を [SqlFilter.SqlExpression][]します。

[Azure classic portal]: http://manage.windowsazure.com
[Python Azure package]: https://pypi.python.org/pypi/azure  
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Azure Queues and Service Bus queues]: service-bus-azure-and-service-bus-queues-compared-contrasted.md#capacity-and-quotas 

