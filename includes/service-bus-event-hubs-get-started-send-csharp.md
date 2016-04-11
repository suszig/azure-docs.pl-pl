## Event Hub へのメッセージ送信

このセクションでは、Windows コンソール アプリを記述して、Event Hub にイベントを送信します。

1. Visual Studio で、新しい Visual c# のデスクトップ アプリを使用してプロジェクトを作成、 **コンソール アプリケーション** プロジェクト テンプレートです。 プロジェクトに名前を **送信者**します。

    ![][7]

2. ソリューション エクスプ ローラーでソリューションを右クリックし、をクリックし、 **... ソリューションの NuGet パッケージの管理**します。 

    [NuGet パッケージの管理] ダイアログ ボックスが表示されます。

3. 検索 `Microsoft Azure Service Bus`, 、] をクリックして **インストール**, 、使用条件に同意します。 

    ![][8]

    これによりパッケージのダウンロードとインストールが実行され、<a href="https://www.nuget.org/packages/WindowsAzure.ServiceBus/">Azure Service Bus ライブラリ NuGet パッケージ</a>への参照が追加されます。

4. 次の追加 `using` の上部にあるステートメント、 **Program.cs** ファイル。

        using System.Threading;
        using Microsoft.ServiceBus.Messaging;

5. 次のフィールドを追加、 **プログラム** を持つ接続文字列と、前のセクションで作成した Event Hub の名前のプレース ホルダーの値に代入クラス **送信** 権限。

        static string eventHubName = "{event hub name}";
        static string connectionString = "{send connection string}";

6. 次のメソッドを追加、 **プログラム** クラス。

        static void SendingRandomMessages()
        {
            var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
            while (true)
            {
                try
                {
                    var message = Guid.NewGuid().ToString();
                    Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                    eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
                }
                catch (Exception exception)
                {
                    Console.ForegroundColor = ConsoleColor.Red;
                    Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                    Console.ResetColor();
                }

                Thread.Sleep(200);
            }
        }

    このメソッドは、200 ミリ秒の遅延時間で Event Hub にイベントを継続的に送信します。

7. 次の行を最後に、追加、 **Main** メソッド。

        Console.WriteLine("Press Ctrl-C to stop the sender process");
        Console.WriteLine("Press Enter to start now");
        Console.ReadLine();
        SendingRandomMessages();


<!-- Images -->
[7]: ./media/service-bus-event-hubs-getstarted/create-sender-csharp1.png
[8]: ./media/service-bus-event-hubs-getstarted/create-sender-csharp2.png

