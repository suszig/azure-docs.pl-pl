<properties
    pageTitle="キュー ストレージと Visual Studio 接続済みサービスの概要 (ASP.NET 5) | Microsoft Azure"
    description="Visual Studio の ASP.NET 5 プロジェクトで Azure キュー ストレージの使用を開始する方法"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/16/2015"
    ms.author="tarcher"/>

# キュー ストレージと Visual Studio 接続済みサービスの概要 (ASP.NET 5)

##概要

Visual Studio を使用して ASP.NET 5 プロジェクトで Azure ストレージ アカウントを参照または作成した後は、Visual Studio で Azure テーブル ストレージを使用して開始する方法を説明 **[接続済みサービス** ダイアログ。  **[接続済みサービス** 操作が、プロジェクト内の Azure ストレージにアクセスする適切な NuGet パッケージをインストールし、プロジェクトの構成ファイルをストレージ アカウントの接続文字列を追加します。

Azure キュー ストレージは、HTTP または HTTPS を使用した認証された呼び出しを介して世界中のどこからでもアクセスできる大量のメッセージを格納するためのサービスです。 キューの 1 つのメッセージの最大サイズは 64 キロバイト (KB) で、1 つのキューには、ストレージ アカウントの合計容量の上限に達するまで、数百万のメッセージを格納できます。

最初に、ストレージ アカウント内に Azure キューを作成する必要があります。 コードでキューを作成する方法を説明します。 また、キュー メッセージの追加、変更、読み取り、削除などの基本的なキュー操作を実行する方法についても説明します。 例は C\# のコードで記述され、Azure .NET 用ストレージ クライアント ライブラリを利用しています。 ASP.NET の詳細については、次を参照してください。 [ASP.NET](http://www.asp.net)します。

**注:** ASP.NET 5 で Azure ストレージへの呼び出しを実行する Api の一部は非同期です。 参照してください [Async および Await における非同期プログラミング](http://msdn.microsoft.com/library/hh191443.aspx) の詳細。 次のコードでは、非同期のプログラミング方法を使用していることを前提としています。

- 参照してください [.NET からキュー ストレージを使用する方法](storage-dotnet-how-to-use-queues.md) プログラムによるキューの操作の詳細についてです。
- 参照してください [Storage のドキュメント](https://azure.microsoft.com/documentation/services/storage/) Azure ストレージに関する一般的な情報です。
- 参照してください [クラウド サービスのドキュメント](http://azure.microsoft.com/documentation/services/cloud-services/) Azure クラウド サービスに関する一般的な情報です。
- 参照してください [ASP.NET](http://www.asp.net) ASP.NET アプリケーションのプログラミングの詳細についてです。





##コードでキューにアクセスする

ASP.NET 5 プロジェクトでキューにアクセスするには、Azure キュー ストレージにアクセスする C# ソース ファイルに、次の項目を含める必要があります。

1. C# ファイルの上部にある名前空間の宣言には、これらが含まれているかどうかを確認 **を使用して** ステートメントです。

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. 取得、 **CloudStorageAccount** をストレージ アカウント情報を表すオブジェクト。 次のコードを使用して、Azure サービス構成からストレージ接続文字列とストレージ アカウント情報を取得できます。

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. 取得、 **CloudQueueClient** 、ストレージ アカウント内のキュー オブジェクトを参照するオブジェクト。  

        // Create the table client.
        CloudQuecClient queueClient = storageAccount.CreateCloudTableClient();

4. 取得、 **CloudQueue** 特定のキューを参照するオブジェクト。

        // Get a reference to a table named "messageQueue"
        CloudTable messageQueue = queueClient.GetQueueReference("messageQueue");


**注:** の次の例に示すすべてのコードの前に上記のコードを使用します。

###コードでキューを作成する

コードで Azure キューを作成するへの呼び出しを追加 **CreateIfNotExistsAsync**します。

    // Create the CloudTable if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

##メッセージをキューに追加する

既存のキューにメッセージを挿入するには、新しい作成 **CloudQueueMessage** オブジェクトを呼び出し、 **AddMessageAsync** メソッドです。

A **CloudQueueMessage** オブジェクトは、文字列 (utf-8 形式) またはバイト配列から作成できます。

次の例では、'Hello, World' というメッセージを挿入します。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

##キュー内のメッセージを読み取る

キューの先頭にあるメッセージをピークを呼び出して、キューから削除することがなく、 **PeekMessageAsync** メソッドです。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code".

    // Display the message.
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


##キュー内のメッセージを読み取って削除する

コードでは、2 つの手順でキューからメッセージを削除 (デキュー) できます。
1. 呼び出す **GetMessageAsync** をキューに次のメッセージを取得します。 返されたメッセージ **GetMessageAsync** このキューからメッセージを読み取る他のコードで参照できなくなります。 既定では、このメッセージを参照できない状態は 30 秒間続きます。
2.  キューからのメッセージの削除を完了するには、呼び出す **DeleteMessageAsync**します。

このようにメッセージを 2 つの手順で削除することで、ハードウェアまたはソフトウェアの問題が原因でコードによるメッセージの処理が失敗した場合に、コードの別のインスタンスで同じメッセージを取得し、もう一度処理することができます。 次のコード呼び出し **DeleteMessageAsync** メッセージが処理された直後します。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code".

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process the message in less than 30 seconds.

    // Then delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## 追加オプションを利用してメッセージをデキューする

キューからのメッセージの取得をカスタマイズする方法は 2 つあります。
1 つ目の方法では、(最大 32 個の) メッセージのバッチを取得できます。 2 つ目の方法では、
コードで各メッセージを完全に処理できるように、非表示タイムアウトの
設定を長くまたは短くすることができます。 次のコード例では、
**GetMessages** 1 回の呼び出しで 20 個のメッセージを取得します。 その後、for ループを使用して、
各メッセージを使用して、 **foreach** ループします。 また、各メッセージの
メッセージごとに 5 分間にタイムアウトしました。 5 分が開始することに注意してください。
同時に、すべてのメッセージのため 5 分が経過した後
呼び出し **GetMessages**, 、削除されていないすべてのメッセージ
再び表示されます。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code".

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## キューの長さを取得する

キュー内のメッセージの概数を取得できます。 この
**FetchAttributes** メソッド キュー サービスに要求
ようキュー サービスに要求します。  **ApproximateMethodCount**
プロパティは、キュー サービスを呼び出さずに、
**FetchAttributes** 呼び出さずに、キュー サービスのメソッドです。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## Async-Await パターンを一般的なキュー API で使用する

この例では、Async-Await パターンを一般的なキュー API で使用する方法を示します。 サンプルでは、特定のメソッドの非同期バージョンをそれぞれ呼び出します。 これは、各メソッドの Async 接尾辞によって確認できます。 非同期のメソッドを使用するとき、Async-Await パターンは、呼び出しが完了するまでローカルでの実行を中断します。 この動作により、現在のスレッドで別の作業を実行できるようになるため、パフォーマンスのボトルネックを回避し、アプリケーションの全体的な応答性が向上させることができます。 .NET での Async-await パターンの使用に関する詳細については、[Async と Await (c# および Visual Basic)] を参照してください (https://msdn.microsoft.com/library/hh191443.aspx)

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code".

    // Create a message to put in the queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## キューを削除する

キューおよびキューに含まれているすべてのメッセージを削除するには、
**削除** 、キュー オブジェクトに対してメソッドです。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code".

    // Delete the queue.
    messageQueue.Delete();



##次のステップ

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

