<properties
    pageTitle="Service Bus トピックの使用方法 (Ruby) | Microsoft Azure"
    description="Azure での Service Bus のトピックとサブスクリプションの使用方法について学習します。コード サンプルは Ruby アプリケーション向けに作成されています。"
    services="service-bus"
    documentationCenter="ruby"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="12/09/2015"
    ms.author="sethm"/>


# Service Bus トピック/サブスクリプションの使用方法

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

このガイドでは、Ruby アプリケーションから Service Bus のトピックとサブスクリプションを使用する方法について説明します。 ここでは、**トピックとサブスクリプションの作成、サブスクリプション フィルターの作成、トピックへのメッセージの送信**、**サブスクリプションからのメッセージの受信**、**トピックとサブスクリプションの削除**などのシナリオについて説明します。 トピックとサブスクリプションの詳細については、次を参照してください。、 [次のステップ](#next-steps) セクションです。

## Service Bus トピックとサブスクリプション

Service Bus のトピックとサブスクリプションのサポート、* * パブリッシュ/サブスクライブ
メッセージング通信 * * モデルです。 トピックおよびサブスクリプション, を使用する場合
分散アプリケーションのコンポーネントをそのと直接通信することはありません。
互いを代わりにメッセージをやり取りするを果たすトピックを介して、
中継ぎ局です。

![TopicConcepts](./media/service-bus-ruby-how-to-use-topics-subscriptions/sb-topics-01.png)

Service Bus キューとは、すべてのメッセージを処理する場所とは異なり、
1 つのコンシューマー、トピックおよびサブスクリプションの提供、 **一対多** フォーム
この通信を使用して、パブリッシュ/サブスクライブ パターンを使用します。 ことは
トピックへの複数のサブスクリプションを登録します。 メッセージを送信するときに、
トピックは、利用できるようにする各サブスクリプション処理
独立しています。

トピック サブスクリプションのコピーを受け取る仮想キューのようになります
トピックに送信されたメッセージです。 登録することもできます。
使用すると、サブスクリプションごとにトピックに対するフィルター ルール
どのような話題でフィルター処理や制限トピックにメッセージの受信します。
サブスクリプションの使用方法について説明します。

Service Bus トピックとサブスクリプションを処理する使用できます、
非常に多数のユーザー間でのメッセージ数が非常に大きいと
アプリケーション。

## サービス名前空間の作成

Azure の Service Bus キューを使用するには、最初にサービス名前空間を作成する必要があります。 名前空間内でサービス バス リソースをアドレス指定するためのスコープ コンテナーを提供します。
アプリケーション内で Service Bus リソースをアドレス指定するためのスコープ コンテナーを提供します。 には、コマンド ライン インターフェイスを使用して名前空間を作成する必要があります、 [Azure クラシック ポータルの [][] ACS 接続を使用して、名前空間を作成できません。

名前空間を作成するには:

1. Azure PowerShell コンソールを開きます。

2. コマンドを下に示すように入力して、名前空間を作成します。 独自の名前空間値と、アプリケーションと同じリージョンを指定します。

      New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true

      ![名前空間の作成](./media/service-bus-ruby-how-to-use-topics-subscriptions/showcmdcreate.png)

## 名前空間の既定の管理資格情報の取得

新しいキューの作成などの管理操作を実行するのには
名前空間、名前空間の管理資格情報を取得する必要があります。

Service Bus 名前空間を作成するために実行した PowerShell コマンドレットが表示されます。
名前空間を管理に使用できるキーです。 **DefaultKey** 値をコピーします。 クラスターの
このチュートリアルの後半で、コードでこの値を使用します。

      ![Copy key](./media/service-bus-ruby-how-to-use-topics-subscriptions/defaultkey.png)

> [AZURE.NOTE]
> ログオンする場合に、このキーを検索することもできます、
> [Azure クラシック ポータルの [][] に移動し、
> 名前空間の接続情報です。

## Ruby アプリケーションの作成

手順については、次を参照してください。 [Azure Ruby アプリケーションを作成する](/develop/ruby/tutorials/web-app-with-linux-vm/)します。

## Service Bus を使用するようにアプリケーションを構成する

Azure Service Bus を使用するには、Ruby Azure パッケージをダウンロードして使用します。このパッケージには、ストレージ REST サービスと通信するための便利なライブラリのセットが含まれています。

### RubyGems を使用してパッケージを取得する

1. **PowerShell** (Windows)、**ターミナル** (Mac)、**Bash** (Unix) などのコマンド ライン インターフェイスを使用します。

2. コマンド ウィンドウに「gem install azure」と入力して、gem と依存関係をインストールします。

### パッケージをインポートする

任意のテキスト エディターを使用して、ストレージを使用するアプリケーションの Ruby ファイルの先頭に次のコードを追加します。

    require "azure"

## Service Bus 接続の設定

Azure モジュールは、環境変数 **azure \_servicebus\_namespace** と **\_servicebus\_access\_key**
詳細については、名前空間への接続に必要です。 これらの環境変数が設定されていない場合は、**Azure::ServiceBusService** を使用する前に、次のコードを使用して名前空間情報を指定する必要があります。

    Azure.config.sb_namespace = "<your azure service bus namespace>"
    Azure.config.sb_access_key = "<your azure service bus access key>"

作成した値には、URL 全体ではなく、名前空間値を設定してください。 たとえば、"yourexamplenamespace.servicebus.windows.net" ではなく、**"yourexamplenamespace"** を使用します。

## トピックを作成する

**Azure::ServiceBusService** オブジェクトを使用すると、トピックを操作できます。 次のコードでは、**Azure::ServiceBusService** オブジェクトを作成します。 トピックを作成するには、 **create \_topic ()** メソッドです。 次の例では、トピックを作成し、既に存在する場合はエラーを出力します。

    azure_service_bus_service = Azure::ServiceBusService.new
    begin
      topic = azure_service_bus_service.create_queue("test-topic")
    rescue
      puts $!
    end

**Azure::ServiceBus::Topic** オブジェクトに追加のオプションを渡すこともできます。これにより、メッセージの有効期間やキューの最大サイズなどの既定のトピックの設定をオーバーライドできます。 次の例は、キューの最大サイズを 5 GB に、有効期間を 1 分に設定する方法を示しています。

    topic = Azure::ServiceBus::Topic.new("test-topic")
    topic.max_size_in_megabytes = 5120
    topic.default_message_time_to_live = "PT1M"
    
    topic = azure_service_bus_service.create_topic(topic)

## サブスクリプションを作成する

トピック サブスクリプションも、**Azure::ServiceBusService** オブジェクトで作成します。 サブスクリプションを指定し、サブスクリプションの仮想キューに配信するメッセージを制限するフィルターを設定することができます。

サブスクリプションは永続的であり、サブスクリプション、またはサブスクリプションが関連付けられているトピックが削除されるまで存在し続けます。 アプリケーションにサブスクリプションを作成するロジックが含まれている場合は、最初に getSubscription メソッドを使用して、サブスクリプションが既に存在しているかどうかを確認する必要があります。

### 既定の (MatchAll) フィルターを適用したサブスクリプションの作成

**MatchAll** フィルターは、新しいサブスクリプションの作成時にフィルターが指定されていない場合に使用される既定のフィルターです。 **MatchAll** フィルターを使用すると、トピックに発行されたすべてのメッセージがサブスクリプションの仮想キューに置かれます。 次の例では、"all-messages" という名前のサブスクリプションを作成し、既定の **MatchAll** フィルターを使用します。

    subscription = azure_service_bus_service.create_subscription("test-topic",
      "all-messages")

### フィルターを適用したサブスクリプションの作成

トピックに送信されたメッセージのうち、特定のサブスクリプション内に表示されるメッセージを指定するフィルターを定義することもできます。

サブスクリプションでサポートされるフィルターのうち、最も柔軟性の高いものが、SQL92 のサブセットを実装する **Azure::ServiceBus::SqlFilter** です。 SQL フィルターは、トピックに発行されるメッセージのプロパティに対して適用されます。 SQL フィルターで使用できる式の詳細については、確認、 [SqlFilter.SqlExpression](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx) 構文です。

フィルターをサブスクリプションに追加するにを使用して、 **create \_rule ()** のメソッド、 **azure::servicebusservice** オブジェクトです。 このメソッドでは、新しいフィルターを既存のサブスクリプションに追加できます。

既定のフィルターはすべての新しいサブスクリプションに自動的に適用されるので、最初に既定のフィルターを削除する必要があります。削除しないと **MatchAll** は指定される他のすべてのフィルターをオーバーライドします。 既定のルールを削除するを使用して、 **delete \_rule ()** メソッドを **azure::servicebusservice** オブジェクトです。

次の例では、"high-messages"という名前のサブスクリプションを作成と、 **:sqlfilter** のみに、カスタム メッセージを選択する **message \_number** 3 を超えるプロパティ。

    subscription = azure_service_bus_service.create_subscription("test-topic",
      "high-messages")
    azure_service_bus_service.delete_rule("test-topic", "high-messages",
      "$Default")
    
    rule = Azure::ServiceBus::Rule.new("high-messages-rule")
    rule.topic = "test-topic"
    rule.subscription = "high-messages"
    rule.filter = Azure::ServiceBus::SqlFilter.new({
      :sql_expression => "message_number > 3" })
    rule = azure_service_bus_service.create_rule(rule)

同様に、次の例では "low-messages" という名前のサブスクリプションを作成し、**Azure::ServiceBus::SqlFilter** を適用します。このフィルターでは、**message_number** プロパティが 3 以下のメッセージのみが選択されます。

    subscription = azure_service_bus_service.create_subscription("test-topic",
      "low-messages")
    azure_service_bus_service.delete_rule("test-topic", "low-messages",
      "$Default")
    
    rule = Azure::ServiceBus::Rule.new("low-messages-rule")
    rule.topic = "test-topic"
    rule.subscription = "low-messages"
    rule.filter = Azure::ServiceBus::SqlFilter.new({
      :sql_expression => "message_number <= 3" })
    rule = azure_service_bus_service.create_rule(rule)

メッセージが "test-topic" に送信されると、そのメッセージは "all-messages" トピック サブスクリプションにサブスクライブしている受信者に必ず配信され、さらにメッセージのコンテンツに応じて、"high-messages" および "low-messages" トピック サブスクリプションにサブスクライブしている受信者に対して選択的に配信されます。

## メッセージをトピックに送信する

Service Bus トピックにメッセージを送信するアプリケーションを使用する必要があります、 **send \_topic\_message ()** メソッドを **azure::servicebusservice** オブジェクトです。 Service Bus トピックに送信されたメッセージは、**Azure::ServiceBus::BrokeredMessage** オブジェクトのインスタンスです。 **:Brokeredmessage** オブジェクトがある一連の標準的なプロパティ (など **ラベル** と **\_to\_live**) は、カスタム アプリケーションの特定のプロパティの保持に使用するディクショナリ、および文字列データの本体。 アプリケーションは、文字列値を渡すことによって、メッセージの本文を設定することができます、 **send \_topic\_message ()** メソッド、必須の標準プロパティは既定値が設定されます。

次の例では、"test-topic" に 5 件のテスト メッセージを送信する方法を示します。 各メッセージの **message_number** カスタム プロパティの値が、ループの反復回数に応じて変化することに注意してください (これによってメッセージを受信するサブスクリプションが決定されます)。

    5.times do |i|
      message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
        { :message_number => i })
      azure_service_bus_service.send_topic_message("test-topic", message)
    end

Service Bus トピックでは、最大 256 MB までのメッセージをサポートしています (標準とカスタムのアプリケーション プロパティが含まれるヘッダーの最大サイズは 64 MB です)。 トピックで保持されるメッセージ数には上限がありませんが、1 つのトピックで保持できるメッセージの合計サイズには上限があります。 このトピックのサイズはトピックの作成時に定義します。上限は 5 GB です。

## サブスクリプションからメッセージを受信する

使用してサブスクリプションからメッセージを受信、 **receive \_subscription\_message ()** メソッドを **azure::servicebusservice** オブジェクトです。 既定では、メッセージは読み取られて (ピークされて) ロックされますが、サブスクリプションからは削除されません。 読み取りし、設定して、サブスクリプションからメッセージを削除できます、 **\_lock** オプションを **false**します。

既定の動作では、読み取りと削除が 2 段階の操作になるため、メッセージが失われることを許容できないアプリケーションにも対応することができます。 Service Bus は要求を受け取ると、次に読み取られるメッセージを検索して、他のコンシューマーが受信できないようロックしてから、アプリケーションにメッセージを返します。 呼び出して受信処理の第 2 段階を完了したら、アプリケーションがメッセージの処理 (または後で処理するために確実に保存) **delete \_subscription\_message ()** メソッドをパラメーターとして削除するメッセージ。  **Delete \_subscription\_message ()** メソッドは、メッセージが読み取り中としてマークされ、サブスクリプションから削除します。

場合、 **: \_lock** にパラメーターが設定されている **false**, と読み取りと削除、メッセージは最もシンプルなモデルになります、障害発生時のメッセージを処理しない、アプリケーションが許容できるシナリオに最適です。 このことを理解するために、コンシューマーが受信要求を発行した後で、メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。 Service Bus はメッセージを読み取り済みとしてマークするため、アプリケーションが再起動してメッセージの読み取りを再開すると、クラッシュ前に読み取られていたメッセージは見落とされることになります。

次の例は、メッセージの受信方法を使用および処理 **receive \_subscription\_message ()**します。 例では、最初から受信して削除メッセージ"low-messages"サブスクリプションを使用して **: \_lock** に設定 **false**, 、"high-messages"から別のメッセージを受信しを使用してメッセージを削除し、 **delete \_subscription\_message ()**:

    message = azure_service_bus_service.receive_subscription_message(
      "test-topic", "low-messages", { :peek_lock => false })
    message = azure_service_bus_service.receive_subscription_message(
      "test-topic", "high-messages")
    azure_service_bus_service.delete_subscription_message(message)

## アプリケーションのクラッシュと読み取り不能のメッセージを処理する

Service Bus には、アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 受信側アプリケーションが何らかの理由でのメッセージを処理できないかどうかは、呼び出すことができます、 **unlock \_subscription\_message ()** メソッドを **azure::servicebusservice** オブジェクトです。 このメソッドが呼び出されると、Service Bus によりサブスクリプション内のメッセージのロックが解除され、メッセージが再度受信できる状態に変わります。このメッセージは、同じコンシューマー側アプリケーションまたは他のコンシューマー側アプリケーションで受信されます。

サブスクリプション内でロックされているメッセージには、タイムアウトも設定されています。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合は、Service Bus によってメッセージのロックが自動的に解除され、再度受信できる状態に変わります。

前に、メッセージを処理した後、アプリケーションがクラッシュすること、 **delete \_subscription\_message ()** メソッドが呼び出されると、再起動する際に、アプリケーションにメッセージが再配信されます。 一般的に、この動作は **"1 回以上の処理"** と呼ばれます。つまり、すべてのメッセージが 1 回以上処理されますが、特定の状況では、同じメッセージが再配信される可能性があります。 重複処理が許されないシナリオの場合、重複メッセージの配信を扱うロジックをアプリケーションに追加する必要があります。 このロジックを使用して多くの場合、対処は、 **\_id** は一定に保たれる配信が試行されると、メッセージのプロパティです。

## トピックとサブスクリプションを削除する

トピックおよびサブスクリプションは永続的であり、明示的にする必要がありますから削除された、 [Azure クラシック ポータル](https://manage.windowsazure.com) またはプログラムを使用しています。 次の例では、"test-topic" という名前のトピックを削除する方法を示します。

    azure_service_bus_service.delete_topic("test-topic")

トピックを削除すると、そのトピックに登録されたサブスクリプションもすべて削除されます。 サブスクリプションは、個別に削除することもできます。 次のコードでは、"high-messages" という名前のサブスクリプションを "test-topic" トピックから削除する方法を示しています。

    azure_service_bus_service.delete_subscription("test-topic", "high-messages")

## 次のステップ

これで、Service Bus トピックの基本を学習できました。さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

-   参照してください [キュー、トピック、およびサブスクリプション](service-bus-queues-topics-subscriptions.md)します。
-   API リファレンス [SqlFilter](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx)します。
-   参照してください、 [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) GitHub のリポジトリです。


[azure classic portal]: http://manage.windowsazure.com 

