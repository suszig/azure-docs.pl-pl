<properties
    pageTitle="Azure Notification Hubs - エンタープライズ環境のプッシュ アーキテクチャ"
    description="エンタープライズ環境での Azure Notification Hubs の使用に関するガイダンス"
    services="notification-hubs"
    documentationCenter=""
    authors="wesmc7777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/20/2015" 
    ms.author="wesmc"/>


# エンタープライズ環境のプッシュ アーキテクチャに関するガイダンス

今日の企業における重要事項は、外部のエンド ユーザーや社内の従業員向けのモバイル アプリケーションを作成することへと徐々に変化してきました。 社内にはメインフレームや LoB アプリケーションといった既存のバックエンド システムがありますが、これらをモバイル アプリケーション アーキテクチャへと統合する必要が出てきています。 このガイドでは、一般的なシナリオ向けのソリューションを推奨しながら、この統合を行うために最適な方法について説明します。

必要となるのは、バックエンド システムで特定のイベントが発生した場合に、ユーザーのモバイル アプリケーションにプッシュ通知が送信されるようにすることです。 たとえば、iPhone で銀行取引アプリを利用している銀行の顧客が、口座から一定金額以上の引き落としがあった際に通知を受信したいと考えている場合、または、イントラネットの環境において、Windows Phone で予算承認のアプリを利用している財務部門の従業員が、承認要求を受け取った際に通知を受信したいと考えている場合などがあります。

こうした場合、銀行口座や承認処理はバックエンド システムで実行されている可能性が高く、バックエンド システムからユーザーにプッシュを開始する必要があります。 このようなバックエンド システムが複数ある場合は、そのすべてにおいて、イベントによって通知がトリガーされたときにプッシュを実装する同じ種類のロジックを構築する必要があります。 ここで複雑なのは、エンド ユーザーがさまざまな通知をサブスクライブしている場合に、複数のバックエンド システムを単一のプッシュ システムと統合する必要があることです。さらに、たとえばイントラネット モバイル アプリの場合などは、複数のモバイル アプリケーションがあり、その中の 1 つのモバイル アプリケーションがこのような複数のバックエンド システムから通知を受信する可能性があります。 バックエンド システムでは、プッシュのセマンティクスやテクノロジに対応していない、あるいは対応する必要がないため、従来の一般的なソリューションでは、バックエンド システムに対して特定のイベントをポーリングし、クライアントにプッシュ メッセージを送信するコンポーネントを導入していました。
ここでは、Azure Service Bus を使用した、より優れたソリューションであるトピック/サブスクリプション モデルについて説明します。これにより、複雑さを軽減し、スケーラブルなソリューションを実現することができます。

次のセクションでは、このソリューションの全般的なアーキテクチャを紹介します (概論として、複数のモバイル アプリを前提としていますが、モバイル アプリが 1 つしかない場合にも同様に該当します)。

## アーキテクチャ

![][1]

このアーキテクチャの図で重要な要素は、トピック/サブスクリプションによるプログラミング モデル (詳細については、[Service Bus Pub/sub プログラミング] にある) を提供する Azure Service Bus です。 ここでは、モバイル バックエンドは、受信側 (通常 [Azure モバイル サービス]、モバイル アプリにプッシュを開始) は、バックエンド システムから直接メッセージを受信せず、代わりに [Azure Service Bus] によって提供される中間抽象化レイヤーがある 1 つまたは複数のバックエンド システムからメッセージを受信するモバイル バックエンドを有効にします。 Service Bus のトピックは、各バックエンド システム (会計、人事、財務など) に作成する必要があります。Service Bus のトピックは基本的に、プッシュ通知としてメッセージの送信を開始する特定の「トピック」です。 バックエンド システムは、これらのトピックにメッセージを送信します。 Service Bus のサブスクリプションを作成することで、モバイル バックエンドがこのような 1 つまたは複数のトピックをサブスクライブできます。 これにより、モバイル バックエンドは対応するバックエンド システムから通知を受信することができます。 モバイル バックエンドは継続的にサブスクリプションのメッセージをリッスンし、メッセージを受信すると、Notification Hubs に通知として送信します。 その後、Notification Hubs は最終的にモバイル アプリにメッセージを配信します。 主要なコンポーネントをまとめると、以下のようになります。

1. バックエンド システム (LoB　またはレガシ システム)
    - Service Bus のトピックを作成
    - メッセージを送信
2. モバイル バックエンド
    - サービスのサブスクリプションを作成
    - (バックエンド システムから) メッセージを受信
    - (Azure Notification Hubs 経由で) クライアントに通知を送信
3. モバイル アプリケーション
    - 通知を受信して表示

### メリット:

1. 受信者 (Notification Hubs を経由したモバイル アプリケーションまたはサービス) と送信者 (バックエンド システム) を分離することで、最小限の変更によって追加のバックエンド システムを統合できます。
2. また、複数のモバイル アプリ 1 つまたは複数のバックエンド システムからイベントを受信するシナリオを実現できます。

## サンプル:

### 前提条件

概念と一般的な作成および構成手順を理解するために、以下のチュートリアルを完了する必要があります。

1. [Service Bus Pub/sub プログラミング]-詳細なサービス バス トピック/サブスクリプションでの作業について説明してトピック/サブスクリプションを格納する名前空間を作成する方法からメッセージを送受信する方法です。
2. [通知ハブ - Windows Universal チュートリアル -] を登録し、通知を受信通知ハブを使用して Windows ストア アプリを設定する方法について説明します。

### サンプル コード

完全なサンプル コードは、[通知ハブのサンプル] から入手できます。 このサンプルは、3 つのコンポーネントに分割されています。

1. **EnterprisePushBackendSystem**

 a. このプロジェクトを使用して、 *WindowsAzure.ServiceBus* Nuget パッケージとは、[Service Bus トピックとサブスクリプションのプログラミング] です。

 b. この単純な C# コンソール アプリでは、モバイル アプリへのメッセージの配信を開始する LoB システムをシミュレートします。

     static void Main(string[] args)
     {
         string connectionString =
             CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
    
         // Create the topic where we will send notifications
         CreateTopic(connectionString);
    
         // Send message
         SendMessage(connectionString);
     }

 c. `CreateTopic` メッセージを送信する Service Bus トピックの作成に使用します。

     public static void CreateTopic(string connectionString)
     {
         // Create the topic if it does not exist already
    
         var namespaceManager =
             NamespaceManager.CreateFromConnectionString(connectionString);
    
         if (!namespaceManager.TopicExists(sampleTopic))
         {
             namespaceManager.CreateTopic(sampleTopic);
         }
     }

 d. `SendMessage` この Service Bus トピックにメッセージを送信するために使用します。 ここでは、サンプルで使用するために、一連のランダムなメッセージを定期的にトピックに送信します。 通常は、イベントが発生した場合にバックエンド システムがメッセージを送信します。

     public static void SendMessage(string connectionString)
     {
         TopicClient client =
             TopicClient.CreateFromConnectionString(connectionString, sampleTopic);
    
         // Sends random messages every 10 seconds to the topic
         string[] messages =
         {
             "Employee Id '{0}' has joined.",
             "Employee Id '{0}' has left.",
             "Employee Id '{0}' has switched to a different team."
         };
    
         while (true)
         {
             Random rnd = new Random();
             string employeeId = rnd.Next(10000, 99999).ToString();
             string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);
    
             // Send Notification
             BrokeredMessage message = new BrokeredMessage(notification);
             client.Send(message);
    
             Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);
    
             System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
         }
     }

2. **ReceiveAndSendNotification**

 a. このプロジェクトを使用して、 *WindowsAzure.ServiceBus* と *Microsoft.Web.WebJobs.Publish* Nuget のパッケージ化し、[Service Bus Pub/sub プログラミング] に基づきます。

 b. これは、c# コンソール アプリを LoB またはバックエンド システムからのメッセージをリッスンするように継続的に実行する必要があるため、[Azure web ジョブとして] を実行します。 これは、モバイル バックエンドの一部になります。

     static void Main(string[] args)
     {
         string connectionString =
                  CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
    
         // Create the subscription which will receive messages
         CreateSubscription(connectionString);
    
         // Receive message
         ReceiveMessageAndSendNotification(connectionString);
     }

 c. `CreateSubscription` バックエンド システムがメッセージを送信する場合、トピックの Service Bus サブスクリプションの作成に使用します。 ビジネス シナリオによっては、このコンポーネントは対応するトピックへの 1 つまたは複数のサブスクリプション (一部は人事部門のシステムからメッセージを受信し、一部は財務部門のシステムからメッセージを受信するなど)　を作成します。

     static void CreateSubscription(string connectionString)
     {
         // Create the subscription if it does not exist already
         var namespaceManager =
             NamespaceManager.CreateFromConnectionString(connectionString);
    
         if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
         {
             namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
         }
     }

 d. ReceiveMessageAndSendNotification は、サブスクリプションを使用してトピックからメッセージを読み取り、読み取りが正常に実行された場合に、Azure Notification Hubs を使用してモバイル アプリケーションに送信する通知 (サンプル シナリオでは Windows ネイティブのトースト通知) を作成するために使用します。

     static void ReceiveMessageAndSendNotification(string connectionString)
     {
         // Initialize the Notification Hub
         string hubConnectionString = CloudConfigurationManager.GetSetting
                 ("Microsoft.NotificationHub.ConnectionString");
         hub = NotificationHubClient.CreateClientFromConnectionString
                 (hubConnectionString, "enterprisepushservicehub");
    
         SubscriptionClient Client =
             SubscriptionClient.CreateFromConnectionString
                     (connectionString, sampleTopic, sampleSubscription);
    
         Client.Receive();
    
         // Continuously process messages received from the subscription
         while (true)
         {
             BrokeredMessage message = Client.Receive();
             var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";
    
             if (message != null)
             {
                 try
                 {
                     Console.WriteLine(message.MessageId);
                     Console.WriteLine(message.SequenceNumber);
                     string messageBody = message.GetBody<string>();
                     Console.WriteLine("Body: " + messageBody + "\n");
    
                     toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                     SendNotificationAsync(toastMessage);
    
                     // Remove message from subscription
                     message.Complete();
                 }
                 catch (Exception)
                 {
                     // Indicate a problem, unlock message in subscription
                     message.Abandon();
                 }
             }
         }
     }
     static async void SendNotificationAsync(string message)
     {
         await hub.SendWindowsNativeNotificationAsync(message);
     }

 e. これを **WebJobs** として発行するには、Visual Studio でソリューションを右クリックして **[WebJob として発行]** を選択します。

 ![][2]

 f. 発行プロファイルを選択し、この WebJobs をホストする Azure Websites が既に存在していない場合には新規作成してから **[発行]** を選択します。

 ![][3]

 g. [Azure クラシック ポータル] にログインするときに、次のようなものを確認する必要がありますように「連続実行」するジョブを構成します。

 ![][4]

3. **EnterprisePushMobileApp**

 a. この Windows ストア アプリケーションでは、モバイル　バックエンドの一部として実行されている WebJobs からトースト通知を受信して表示します。 これは、[通知ハブ - Windows Universal チュートリアル] に基づいています。

 b. アプリケーションでトースト通知の受信が有効になっていることを確認します。

 c. (*HubName* および *DefaultListenSharedAccessSignature* を置換してから) アプリの起動時に、以下の Notification Hubs の登録コードが呼び出されることを確認します。

     private async void InitNotificationsAsync()
     {
         var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
    
         var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
         var result = await hub.RegisterNativeAsync(channel.Uri);
    
         // Displays the registration ID so you know it was successful
         if (result.RegistrationId != null)
         {
             var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
             dialog.Commands.Add(new UICommand("OK"));
             await dialog.ShowAsync();
         }
     }


### サンプルの実行:

1. WebJobs が正常に実行されていて、「連続実行」するように設定されていることを確認します。
2. **EnterprisePushMobileApp** を実行して、Windows ストア アプリを開始します。
3. LoB バックエンドをシミュレートする **EnterprisePushBackendSystem** コンソール アプリケーションを実行して、メッセージの送信を開始すると、以下のようなトースト通知が表示されます。

    ![][5]

4. メッセージは、最初に WebJobs の Service Bus のサブスクリプションによって監視されていた Service Bus のトピックに送信されます。 メッセージを受信すると、通知が作成されてモバイル アプリに送信されます。 Web ジョブの [Azure クラシック ポータル] ログ リンクをクリックすると、処理を確認する web ジョブのログを確認することができます。

    ![][6]




[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png 
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png 
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png 
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png 
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png 
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png 
[notification hub samples]: https://github.com/Azure/azure-notificationhubs-samples 
[azure mobile service]: http://azure.microsoft.com/documentation/services/mobile-services/ 
[azure service bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/ 
[service bus pub/sub programming]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/ 
[azure webjob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/ 
[notification hubs - windows universal tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/ 
[azure classic portal]: https://manage.windowsazure.com/ 

