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

必要となるのは、バックエンド システムで特定のイベントが発生した場合に、ユーザーのモバイル アプリケーションにプッシュ通知が送信されるようにすることです。 例:  iPhone で銀行の銀行取引アプリケーションいる銀行の顧客が通知を受ける借方が自分のアカウントまたは場所を Windows Phone で予算承認のアプリを持つ金融部門の従業員が通知を受ける承認要求を受け取ったときに、イントラネットの環境から一定金額以上行われるとします。

こうした場合、銀行口座や承認処理はバックエンド システムで実行されている可能性が高く、バックエンド システムからユーザーにプッシュを開始する必要があります。 このようなバックエンド システムが複数ある場合は、そのすべてにおいて、イベントによって通知がトリガーされたときにプッシュを実装する同じ種類のロジックを構築する必要があります。 ここで複雑なのは、エンド ユーザーがさまざまな通知をサブスクライブしている場合に、複数のバックエンド システムを単一のプッシュ システムと統合する必要があることです。さらに、たとえばイントラネット モバイル アプリの場合などは、複数のモバイル アプリケーションがあり、その中の 1 つのモバイル アプリケーションがこのような複数のバックエンド システムから通知を受信する可能性があります。 バックエンド システムでは、プッシュのセマンティクスやテクノロジに対応していない、あるいは対応する必要がないため、従来の一般的なソリューションでは、バックエンド システムに対して特定のイベントをポーリングし、クライアントにプッシュ メッセージを送信するコンポーネントを導入していました。
ここでは、Azure Service Bus を使用した、より優れたソリューションであるトピック/サブスクリプション モデルについて説明します。これにより、複雑さを軽減し、スケーラブルなソリューションを実現することができます。

次のセクションでは、このソリューションの全般的なアーキテクチャを紹介します (概論として、複数のモバイル アプリを前提としていますが、モバイル アプリが 1 つしかない場合にも同様に該当します)。

## アーキテクチャ

![][1]

このアーキテクチャの図で重要な要素は、トピック/サブスクリプションによるプログラミング モデルを提供する Azure Service Bus (詳細については、「 [Service Bus Pub/Sub programming])。 この場合は、モバイル バックエンドは、受信側 (通常 [Azure Mobile Service], 、モバイル アプリにプッシュを開始) はバックエンド システムから直接メッセージを受信せずが代わりにによって提供される中間抽象化レイヤーがある [Azure Service Bus] モバイル バックエンドを 1 つまたは複数のバックエンド システムからメッセージを受信を有効にします。 Service Bus のトピックは、各バックエンド システム (会計、人事、財務など) に作成する必要があります。Service Bus のトピックは基本的に、プッシュ通知としてメッセージの送信を開始する特定の「トピック」です。 バックエンド システムは、これらのトピックにメッセージを送信します。 Service Bus のサブスクリプションを作成することで、モバイル バックエンドがこのような 1 つまたは複数のトピックをサブスクライブできます。 これにより、モバイル バックエンドは対応するバックエンド システムから通知を受信することができます。 モバイル バックエンドは継続的にサブスクリプションのメッセージをリッスンし、メッセージを受信すると、Notification Hubs に通知として送信します。 その後、Notification Hubs は最終的にモバイル アプリにメッセージを配信します。 主要なコンポーネントをまとめると、以下のようになります。

1. バックエンド システム (LoB　またはレガシ システム)
    - Service Bus のトピックを作成
    - メッセージを送信
2. モバイル バックエンド
    - サービスのサブスクリプションを作成
    - (バックエンド システムから) メッセージを受信
    - (Azure Notification Hubs 経由で) クライアントに通知を送信
3. モバイル アプリケーション
    - 通知を受信して表示

###メリット:

1. 受信者 (Notification Hubs を経由したモバイル アプリケーションまたはサービス) と送信者 (バックエンド システム) を分離することで、最小限の変更によって追加のバックエンド システムを統合できます。
2. また、複数のモバイル アプリ 1 つまたは複数のバックエンド システムからイベントを受信するシナリオを実現できます。  

## サンプル:

###前提条件
概念と一般的な作成および構成手順を理解するために、以下のチュートリアルを完了する必要があります。

1. [Service Bus Pub/Sub programming] -サービス バス トピック/サブスクリプションでの作業の詳細を説明このトピック/サブスクリプションを格納する名前空間を作成する方法からメッセージを送受信する方法です。
2. [Notification Hubs - Windows Universal tutorial] -これは、Windows ストア アプリの設定を登録し、通知を受信する通知ハブを使用する方法を説明します。

###サンプル コード

完全なサンプル コードについては、「 [Notification Hub Samples]します。 このサンプルは、3 つのコンポーネントに分割されています。

1. **EnterprisePushBackendSystem**

    a. このプロジェクトを使用して、 *WindowsAzure.ServiceBus* Nuget パッケージに基づくと [Service Bus Pub/Sub programming]します。

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

    c. `CreateTopic` 使用される Service Bus のトピックを作成するメッセージを送信します。

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

    d. `SendMessage` この Service Bus トピックにメッセージを送信に使用されます。 ここでは、サンプルで使用するために、一連のランダムなメッセージを定期的にトピックに送信します。 通常は、イベントが発生した場合にバックエンド システムがメッセージを送信します。

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

    a. このプロジェクトを使用して、 *WindowsAzure.ServiceBus* と *Microsoft.Web.WebJobs.Publish* Nuget パッケージ化しに基づく [Service Bus Pub/Sub programming]します。

    b. これは、c# コンソール アプリとして実行する、 [Azure WebJob] LoB またはバックエンド システムからのメッセージをリッスンするように継続的に実行する必要があるためです。 これは、モバイル バックエンドの一部になります。

        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

            // Create the subscription which will receive messages
            CreateSubscription(connectionString);

            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }

    c. `CreateSubscription` 使用のトピックの Service Bus サブスクリプションを作成するバックエンド システムがメッセージを送信する場所です。 ビジネス シナリオによっては、このコンポーネントは対応するトピックへの 1 つまたは複数のサブスクリプション (一部は人事部門のシステムからメッセージを受信し、一部は財務部門のシステムからメッセージを受信するなど)　を作成します。

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

    e. これは、として公開するため、 **web ジョブ**, Visual Studio でソリューションを右クリックし、選択、 **web ジョブとして発行**

    ![][2]

    f. 発行プロファイルを選択し、新しい Azure web サイトを作成し、web サイトがあると、既に存在していないこの web ジョブをホストする場合 **発行**します。

    ![][3]

    g. で、「連続実行」にサインインするときにジョブを構成する、 [Azure Classic Portal] 、次のような結果を表示する必要があります。

    ![][4]


3. **EnterprisePushMobileApp**

    a. この Windows ストア アプリケーションでは、モバイル　バックエンドの一部として実行されている WebJobs からトースト通知を受信して表示します。 これに基づいて [Notification Hubs - Windows Universal tutorial]します。  

    b. アプリケーションでトースト通知の受信が有効になっていることを確認します。

    c. アプリの登録コードが呼び出される以下の Notification Hubs が起動時を確認します (交換した後、 *HubName* と *DefaultListenSharedAccessSignature*:

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
2. 実行、 **EnterprisePushMobileApp** Windows ストア アプリを開始します。
3. 実行、 **EnterprisePushBackendSystem** メッセージをコンソール アプリケーションでは、LoB バックエンドをシミュレートして、送信を開始し、次のように表示されるトースト通知を表示する必要があります。

    ![][5]

4. メッセージは、最初に WebJobs の Service Bus のサブスクリプションによって監視されていた Service Bus のトピックに送信されます。 メッセージを受信すると、通知が作成されてモバイル アプリに送信されます。 ログのリンクをクリックすると、処理を確認する web ジョブのログを調べればわかる [Azure Classic Portal] Web ジョブの。

    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Notification Hub Samples]: https://github.com/Azure/azure-notificationhubs-samples
[Azure Mobile Service]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Service Bus Pub/Sub programming]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[Notification Hubs - Windows Universal tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Azure Classic Portal]: https://manage.windowsazure.com/


