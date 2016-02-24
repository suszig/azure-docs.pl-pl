<properties
    pageTitle="Java で Service Bus トピックを使用する方法 | Microsoft Azure"
    description="Azure での Service Bus のトピックとサブスクリプションの使用方法について学習します。 コード サンプルは Java アプリケーション向けに作成されています。"
    services="service-bus"
    documentationCenter="java"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/07/2015"
    ms.author="sethm"/>

# Service Bus のトピックとサブスクリプションの使用方法

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

このガイドでは、Service Bus のトピックとサブスクリプションの使用方法について説明します。 サンプルが使用して Java で記述され、 [Azure SDK for Java][]します。 紹介するシナリオ **トピックとサブスクリプションの作成**, 、**サブスクリプション フィルターの作成**, 、**トピックにメッセージを送信する**, 、**サブスクリプションからメッセージを受信**, と
**トピックとサブスクリプションの削除**します。

[AZURE.INCLUDE [service-bus-java-how-to-create-topic](../../includes/service-bus-java-how-to-create-topic.md)]

## Service Bus を使用するようにアプリケーションを構成する

インストールされていることを確認、 [Azure SDK for Java][] このサンプルをビルドする前にします。 Eclipse を使用している場合は、インストール、 [Azure Toolkit for Eclipse][] 、Azure SDK for Java が含まれます。 追加することができますし、 **Microsoft Azure Libraries for Java** をプロジェクトに。

![](media/service-bus-java-how-to-use-topics-subscriptions/eclipselibs.png)

次の import ステートメントを Java ファイルの先頭に追加します。

    // Include the following imports to use service bus APIs
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.*;
    import com.microsoft.windowsazure.core.*;
    import javax.xml.datatype.*;

Azure Libraries for Java をビルド パスに追加し、プロジェクトのデプロイメント アセンブリに含めます。

## トピックを作成する

サービス バス トピックの管理操作は、
**ServiceBusContract** クラスです。 A **ServiceBusContract** オブジェクトは、
管理用のトークン アクセス許可をカプセル化した適切な構成
それを管理するアクセス許可を持つ SAS トークンと **ServiceBusContract** クラスは、
Azure との唯一の通信ポイントです。

 **ServiceBusService** クラスを作成、列挙、メソッドを提供
および削除のためのメソッドが用意されています。 例を次にどのように、 **ServiceBusService** オブジェクト
という名前のトピックを作成するために使用できる `TestTopic`, と呼ばれる名前空間を持つ `HowToSample`:

    Configuration config =
        ServiceBusConfiguration.configureWithSASAuthentication(
          "HowToSample",
          "RootManageSharedAccessKey",
          "SAS_key_value",
          ".servicebus.windows.net"
          );

    ServiceBusContract service = ServiceBusService.create(config);
    TopicInfo topicInfo = new TopicInfo("TestTopic");
    try  
    {
        CreateTopicResult result = service.createTopic(topicInfo);
    }
    catch (ServiceException e) {
        System.out.print("ServiceException encountered: ");
        System.out.println(e.getMessage());
        System.exit(-1);
    }

メソッドがあります **TopicInfo** 、トピックのプロパティを有効にします。
チューニングする (例: する既定の有効期間 (TTL) 値を設定するには
の値を設定するメソッドなどです)。 次の例では、
という名前のトピックを作成する `TestTopic` 、最大サイズは 5 GB。

    long maxSizeInMegabytes = 5120;  
    TopicInfo topicInfo = new TopicInfo("TestTopic");  
    topicInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
    CreateTopicResult result = service.createTopic(topicInfo);

使用できる、 **listTopics** メソッド
**ServiceBusContract** オブジェクトの場合は、指定した名前のトピックを確認するには
既に存在するかどうか確認できます。

## サブスクリプションを作成する

トピックへのサブスクリプションもで作成、 **ServiceBusService**
クラスから派生していることに注意してください。 サブスクリプションを指定し、
サブスクリプションの仮想キューに渡すメッセージを制限するオプションのフィルターを設定することができます
。

### 既定の (MatchAll) フィルターを適用したサブスクリプションの作成

 **MatchAll** フィルターはフィルターなしの場合に使用される既定のフィルター
に使用される既定のフィルターです。 ときに、 **MatchAll**
フィルターを使用すると、トピックに発行されたすべてのメッセージが
サブスクリプションの仮想キューに置かれます。 次の例では、
サブスクリプションは、"AllMessages"という名前し、既定値を使用して **MatchAll**
フィルターを使用します。

    SubscriptionInfo subInfo = new SubscriptionInfo("AllMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);

### フィルターを適用したサブスクリプションの作成

トピックに送信されたメッセージのうち、
特定のトピック サブスクリプション内に表示されるメッセージに絞り込むフィルターを設定することもできます。

サブスクリプションでサポートされるフィルターのうち、最も柔軟性の高いものが、
[SqlFilter][], 、SQL92 のサブセットを実装します。 SQL フィルターは
トピックに発行されるメッセージのプロパティに対して適用されます。 次に
SQL フィルターで使用できる式の詳細については、
確認、 [SqlFilter.SqlExpression][] 構文です。

次の例は、という名前のサブスクリプションを作成 `HighMessages` で、
[SqlFilter][] のみ、カスタム メッセージを選択するオブジェクト
**MessageNumber** 3 を超えるプロパティ。

    // Create a "HighMessages" filtered subscription  
    SubscriptionInfo subInfo = new SubscriptionInfo("HighMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);
    RuleInfo ruleInfo = new RuleInfo("myRuleGT3");
    ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber > 3");
    CreateRuleResult ruleResult =
        service.createRule("TestTopic", "HighMessages", ruleInfo);
    // Delete the default rule, otherwise the new rule won't be invoked.
    service.deleteRule("TestTopic", "HighMessages", "$Default");

同様に、次の例がという名前のサブスクリプションを作成 `LowMessages` で、 [SqlFilter][] のみを持つメッセージを選択するオブジェクト、 **MessageNumber** 3 のプロパティが。

    // Create a "LowMessages" filtered subscription
    SubscriptionInfo subInfo = new SubscriptionInfo("LowMessages");
    CreateSubscriptionResult result =
        service.createSubscription("TestTopic", subInfo);
    RuleInfo ruleInfo = new RuleInfo("myRuleLE3");
    ruleInfo = ruleInfo.withSqlExpressionFilter("MessageNumber <= 3");
    CreateRuleResult ruleResult =
        service.createRule("TestTopic", "LowMessages", ruleInfo);
    // Delete the default rule, otherwise the new rule won't be invoked.
    service.deleteRule("TestTopic", "LowMessages", "$Default");


メッセージを今すぐ送信と `TestTopic`, は常に
サブスクライブした受信者に配信される、 `AllMessages` 、サブスクリプションにサブスクライブされた受信者に対して選択的に配信されると、 `HighMessages` と `LowMessages` サブスクリプション (に応じて、
。

## メッセージをトピックに送信する

サービス バス トピックにメッセージを送信するには、アプリケーションで
**ServiceBusContract** オブジェクトです。 次のコードを送信する方法を示しています、
メッセージを `TestTopic` トピック内で以前に作成、 `HowToSample` 名前空間。

    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendTopicMessage("TestTopic", message);

サービス バス トピックに送信されたメッセージは、
[BrokeredMessage][] クラスです。 [BrokeredMessage][]* オブジェクトのセットがあります。
標準的なメソッド (よう **setLabel** と **TimeToLive**)、ディクショナリ
アプリケーションに固有のカスタム プロパティの保持に使用するディクショナリが用意されており、
任意のアプリケーション データの本体が格納されます。 アプリケーションでは、
BrokeredMessage のコンストラクターにシリアル化可能なオブジェクトを渡すことによってメッセージの本文を設定できます。
[BrokeredMessage][], 、適切な **DataContractSerializer** されます
オブジェクトをシリアル化します。 この方法に代わって、
**java.io.InputStream** を指定できます。

以下の例では、前に示したコード スニペットで取得した
`TestTopic` **MessageSender** 上のコード スニペットで取得しました。
注方法 **MessageNumber** 各メッセージのプロパティの値
ループの反復回数に応じてどのように変化するかに注目してください (これによって
メッセージを受信するサブスクリプションが決定されます)。

    for (int i=0; i<5; i++)  {
        // Create message, passing a string message for the body
        BrokeredMessage message = new BrokeredMessage("Test message " + i);
        // Set some additional custom app-specific property
        message.setProperty("MessageNumber", i);
        // Send message to the topic
        service.sendTopicMessage("TestTopic", message);
    }

Service Bus トピックでは、最大 256 MB までのメッセージをサポートしています
(標準とカスタムのアプリケーション プロパティが含まれるヘッダーは、
64 MB が最大サイズです)。 キューで保持されるメッセージ数には上限がありませんが、
1 つのトピックで保持できるメッセージの合計サイズには上限
があります。 このトピックのサイズはトピックの作成時に定義します。
上限は 5 GB です。

## サブスクリプションからメッセージを受信する方法

サブスクリプションからメッセージを受信する主な方法は、
**ServiceBusContract** オブジェクトです。 メッセージは 2 つの
さまざまなモード: **ReceiveAndDelete** と **PeekLock**します。

使用する場合、 **ReceiveAndDelete** モードでは、受信はシングル ショット
受信が 1 回ずつの動作になります。つまり、Service Bus は
メッセージに対する読み取り要求を受け取ると、メッセージを読み取り中としてマークし、
想定しています。 **ReceiveAndDelete** モードは最もシンプルなモデルと機能
障害発生時にアプリケーション側でメッセージを
処理しないことを許容できるシナリオに最適です。 このことを理解するために、
コンシューマーが受信要求を発行した後で、
メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。 Service Bus はメッセージを読み取り済みとしてマークするため、
アプリケーションが再起動してメッセージの読み取りを再開すると、
クラッシュ前に読み取られていたメッセージは
見落とされることになります。

 **PeekLock** モードでは、受信は、2 段階の操作になります
メッセージが失われることが許容できないアプリケーションに対応することができます
。 Service Bus が要求を受け取ると、次に読み取られるメッセージを検索して、
他のコンシューマーが受信できないようロックしてから、
アプリケーションにメッセージを返します。 アプリケーションが
メッセージの処理を終えた後 (または後で処理するために確実に保存した後)、
呼び出して受信処理の第 2 段階を完了 **削除**
を完了します。 Service Bus が確認、 **削除** を呼び出すと、その
メッセージが読み取り中としてマークされ、トピックから削除されます。

次の例では、
使用して処理 **PeekLock** モード (既定モードではありません)。 次の例では、
ループを実行し、"HighMessages" サブスクリプション内のメッセージを処理します。メッセージがなくなるとループを終了します (または、新しいメッセージを待機するように設定される場合もあります)。

    try
    {
        ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
        opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

        while(true)  {
            ReceiveSubscriptionMessageResult  resultSubMsg =
                service.receiveSubscriptionMessage("TestTopic", "HighMessages", opts);
            BrokeredMessage message = resultSubMsg.getValue();
            if (message != null && message.getMessageId() != null)
            {
                System.out.println("MessageID: " + message.getMessageId());
                // Display the topic message.
                System.out.print("From topic: ");
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
                    message.getProperty("MessageNumber"));
                // Delete message.
                System.out.println("Deleting this message.");
                service.deleteMessage(message);
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
によってトピック内のメッセージのロックが解除され、
メッセージが再度受信できる状態に変わります。メッセージを受信するアプリケーションは、
以前と同じものでも、別のものでもかまいません。

トピック内でロックされているメッセージにはタイムアウトも設定されています。
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

## トピックとサブスクリプションを削除する

トピックとサブスクリプションを削除する主な方法は、
**ServiceBusContract** オブジェクトです。 トピックを削除すると、そのトピックに登録されたサブスクリプションもすべて
削除されます。 サブスクリプションは、個別に削除することもできます。

    // Delete subscriptions
    service.deleteSubscription("TestTopic", "AllMessages");
    service.deleteSubscription("TestTopic", "HighMessages");
    service.deleteSubscription("TestTopic", "LowMessages");

    // Delete a topic
    service.deleteTopic("TestTopic");

## 次のステップ

これをサービス バス キューの基本を学習できました。、を参照してください。 [Service Bus キュー、トピック、およびサブスクリプション][] の詳細。

  [Azure SDK for Java]: http://azure.microsoft.com/develop/java/
  [Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
  [Azure classic portal]: http://manage.windowsazure.com/
  [Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx 
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
