<properties
    pageTitle="Ruby で Service Bus キューを使用する方法 | Microsoft Azure"
    description="Azure での Service Bus キューの使用方法を学習します。コード サンプルは Ruby で記述されています。"
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


# Service Bus キューの使用方法

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

このガイドでは、Service Bus キューの使用方法について説明します。 サンプルは
Ruby で記述され、Azure gem を使用します。 紹介するシナリオは、
説明を含める **キューの作成、メッセージを送受信する**, と
**キューの削除**します。 キューの詳細については、次を参照してください。、 [次のステップ](#next-steps) セクションです。

## Service Bus キューとは

Service Bus キューのサポート、 *仲介型メッセージング* 通信
モデル。 キューを使用すると、分散アプリケーションのコンポーネントが
直接相互通信します。使用してメッセージを交換する代わりに
仲介役として機能するキューです。 メッセージ プロデューサー (送信者) は
キューにメッセージをその処理を続行します。
メッセージ コンシューマー (受信者) は、キューからメッセージを非同期に受信して
キューにし、それを処理します。 メッセージ プロデューサーは、
さらに処理し、送信を続行するために、コンシューマーから
。 キューでは **では、先入れ先出し (FIFO)** メッセージ配信
1 つまたは複数を使用します。 つまり、通常は
れる順序でレシーバーによって受信および処理
キューに追加され、各メッセージを受信し、によってのみ処理
1 つのメッセージ コンシューマー。

![QueueConcepts](./media/service-bus-ruby-how-to-use-queues/sb-queues-08.png)

サービス バス キューは汎用テクノロジであり、
さまざまなシナリオ:

-   [複数層の web および worker ロール間の通信
    Azure application](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md) します。
-   によってホストされるアプリケーションを内部設置型アプリケーションと Azure 間の通信
    [ハイブリッド ソリューション](service-bus-dotnet-hybrid-app-using-service-bus-relay.md)します。
-   分散アプリケーションのコンポーネント間の通信
    異なる組織または部門の内部設置型を実行して、
    組織です。

キューを使用すると、アプリケーションのスケール アウト性と
アーキテクチャの復元性を有効にします。

## サービス名前空間の作成

Azure の Service Bus キューを使用するには、最初にサービス名前空間を作成する必要があります。 サービス名前空間は、アプリケーション内で Service Bus リソースをアドレス指定するためのスコープ コンテナーを提供します。 作成する必要があります、
コマンド ライン インターフェイスを使用して名前空間 Azure クラシック ポータルでは、ACS 接続の名前空間が作成されないためです。

サービス名前空間を作成するには:

1. Azure PowerShell コンソールを開きます。

2. 次のコマンドを入力して、Service Bus の名前空間を作成します。 独自の名前空間値と、アプリケーションと同じリージョンを指定します。

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true

    ![Create Namespace](./media/service-bus-ruby-how-to-use-queues/showcmdcreate.png)
    ```

## 名前空間の管理資格情報の取得

新しいキューの作成などの管理操作を実行するのには
名前空間、名前空間の管理資格情報を取得する必要があります。

Azure service bus 名前空間を作成するために実行した PowerShell コマンドレットが表示されます。
名前空間を管理に使用できるキーです。 **DefaultKey** 値をコピーします。 クラスターの
このチュートリアルの後半で、コードでこの値を使用します。

![キーのコピー](./media/service-bus-ruby-how-to-use-queues/defaultkey.png)
> [AZURE.NOTE] ログオンする場合は、このキーを検索することも、 [Azure クラシック ポータル](http://manage.windowsazure.com/) 、Service Bus 名前空間の接続情報に移動します。

## Ruby アプリケーションの作成

Ruby アプリケーションを作成します。 手順については、次を参照してください。 [Azure Ruby アプリケーションを作成する](/develop/ruby/tutorials/web-app-with-linux-vm/)します。

## Service Bus を使用するようにアプリケーションを構成する

Azure Service Bus を使用するには、Ruby Azure パッケージをダウンロードして使用します。このパッケージには、ストレージ REST サービスと通信するための便利なライブラリのセットが含まれています。

### RubyGems を使用してパッケージを取得する

1. **PowerShell** (Windows)、**ターミナル** (Mac)、**Bash** (Unix) などのコマンド ライン インターフェイスを使用します。

2. コマンド ウィンドウに「gem install azure」と入力して、gem と依存関係をインストールします。

### パッケージをインポートする

任意のテキスト エディターを使用して、ストレージを使用するアプリケーションの Ruby ファイルの先頭に次のコードを追加します。

```
require "azure"
```

## Azure Service Bus 接続の設定

Azure モジュールは、環境変数 **azure \_servicebus\_namespace** と **\_servicebus\_access_key**
Service Bus 名前空間への接続に必要な情報です。 これらの環境変数が設定されていない場合は、**Azure::ServiceBusService** を使用する前に、次のコードを使用して名前空間情報を指定する必要があります。

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

作成した値には、URL 全体ではなく、名前空間値を設定してください。 たとえば、"yourexamplenamespace.servicebus.windows.net" ではなく、**"yourexamplenamespace"** を使用します。

## キューの作成方法

**Azure::ServiceBusService** オブジェクトを使用して、キューを操作できます。 キューを作成するには、**create_queue()** メソッドを使用します。 次の例では、キューを作成するか、すべてのエラーを出力します。

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

**Azure::ServiceBus::Queue** オブジェクトに追加のオプションを渡すこともできます。これにより、メッセージの有効期間やキューの最大サイズなどの既定のキューの設定をオーバーライドできます。 次の例は、キューの最大サイズを 5 GB に、有効期間を 1 分に設定する方法を示しています。

```
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## メッセージをキューに送信する方法

アプリケーションが Service Bus キューにメッセージを送信する、 **send \_queue\_message ()** メソッドを **azure::servicebusservice** オブジェクトです。 メッセージ キューに送信された (およびから受信された) の Service Bus メッセージ **:brokeredmessage** オブジェクト、および標準的なプロパティ セットがあります (など **ラベル** と **\_to\_live**) は、カスタム アプリケーションの特定のプロパティの保持に使用するディクショナリ、および任意のアプリケーション データの本体。 アプリケーションでは、メッセージとして文字列値を渡すことによってメッセージの本文を設定でき、必須の標準プロパティは既定値に設定されます。

次の例を使用して"test-queue"という名前のキューにテスト メッセージを送信する方法を示します **send \_queue\_message ()**:

```
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Service Bus キューでは、最大 256 KB までのメッセージをサポートしています (標準とカスタムのアプリケーション プロパティが含まれるヘッダーの最大サイズは 64 KB です)。 キューで保持されるメッセージ数には上限がありませんが、キュー 1 つあたりが保持できるメッセージの合計サイズには上限があります。 このキュー サイズは作成時に定義され、上限は 5 GB です。

## キューからメッセージを受信する方法

使用してキューからメッセージを受信、 **receive \_queue\_message ()** メソッドを **azure::servicebusservice** オブジェクトです。 既定では、メッセージは読み取られて (ピークされて) ロックされますが、キューからは削除されません。 ただし、**:peek_lock** オプションを **false** に設定すると、読み取ったメッセージをキューから削除できます。

既定の動作では、読み取りと削除が 2 段階の操作になるため、メッセージが失われることを許容できないアプリケーションにも対応することができます。 Service Bus は要求を受け取ると、次に読み取られるメッセージを検索して、他のコンシューマーが受信できないようロックしてから、アプリケーションにメッセージを返します。 呼び出して受信処理の第 2 段階を完了したら、アプリケーションがメッセージの処理 (または後で処理するために確実に保存) **delete \_queue\_message ()** メソッドをパラメーターとして削除するメッセージ。  **Delete \_queue\_message ()** メソッドは、メッセージが読み取り中としてマークされ、キューから削除します。

場合、 **: \_lock** にパラメーターが設定されている **false**, と読み取りと削除、メッセージは最もシンプルなモデルになります、障害発生時のメッセージを処理しない、アプリケーションが許容できるシナリオに最適です。 このことを理解するために、コンシューマーが受信要求を発行した後で、メッセージを処理する前にクラッシュしたというシナリオを考えてみましょう。 Service Bus はメッセージを読み取り済みとしてマークするため、アプリケーションが再起動してメッセージの読み取りを再開すると、クラッシュ前に読み取られていたメッセージは見落とされることになります。

次の例では、受信しを使用してメッセージを処理する方法 **receive \_queue\_message ()**します。 例では、まずで受信して削除メッセージを使用して **: \_lock** 設定 **false**, 、別のメッセージを受信しを使用してメッセージを削除し、 **delete \_queue\_message ()**:

```
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## アプリケーションのクラッシュと読み取り不能のメッセージを処理する方法

Service Bus には、アプリケーションにエラーが発生した場合や、メッセージの処理に問題がある場合に復旧を支援する機能が備わっています。 受信側アプリケーションが何らかの理由でのメッセージを処理できないかどうかは、呼び出すことができます、 **unlock \_queue\_message ()** メソッドを **azure::servicebusservice** オブジェクトです。 このメソッドが呼び出されると、Service Bus によってキュー内のメッセージのロックが解除され、メッセージが再度受信できる状態に変わります。メッセージを受信するアプリケーションは、以前と同じものでも、別のものでもかまいません。

キュー内でロックされているメッセージにはタイムアウトも設定されています。アプリケーションがクラッシュした場合など、ロックがタイムアウトになる前にアプリケーションがメッセージの処理に失敗した場合には、メッセージのロックが自動的に解除され、再度受信できる状態に変わります。

前に、メッセージを処理した後、アプリケーションがクラッシュすること、 **delete \_queue\_message ()** メソッドが呼び出されると、再起動する際にそのメッセージがアプリケーションに再配信されます。 一般的に、この動作は **1 回以上の処理**と呼ばれます。つまり、すべてのメッセージが 1 回以上処理されますが、特定の状況では、同じメッセージが再配信される可能性があります。 重複処理が許されないシナリオの場合、重複メッセージの配信を扱うロジックをアプリケーションに追加する必要があります。 これは、多くの場合を使用して、 **\_id** 配信が試行されると、メッセージのプロパティです。

## 次のステップ

これで、Service Bus キューの基本を学習できました。さらに詳細な情報が必要な場合は、次のリンク先を参照してください。

-   概要 [キュー、トピック、およびサブスクリプション](service-bus-queues-topics-subscriptions.md)します。
-   参照してください、 [Azure SDK for Ruby](https://github.com/Azure/azure-sdk-for-ruby) GitHub のリポジトリです。

この記事で説明した Azure Service Bus キューおよび「比較、 [Azure キュー サービスを使用する方法](/develop/ruby/how-to-guides/queue-service/) 記事を参照してください [Azure キューと Azure Service Bus キューの比較](service-bus-azure-and-service-bus-queues-compared-contrasted.md)






