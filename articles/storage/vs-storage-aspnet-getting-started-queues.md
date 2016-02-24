<properties
    pageTitle="キュー ストレージと Visual Studio 接続済みサービスの概要 (ASP.NET) | Microsoft Azure"
    description="Visual Studio 接続済みサービスを使用してストレージ アカウントに接続した後、Visual Studio の ASP.NET プロジェクトで Azure Queue ストレージの使用を開始する方法について説明します。"
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

# Azure キュー ストレージと Visual Studio 接続済みサービスの概要

## 概要

この記事で説明する方法を作成または Visual Studio を使用して ASP.NET プロジェクトで Azure ストレージ アカウントを参照した後は、Visual Studio で Azure キュー ストレージを使用して開始 **[接続済みサービス** ダイアログ。

ストレージ アカウントで Azure キューを作成し、アクセスする方法を紹介します。 また、キュー メッセージの追加、変更、読み取り、削除などの基本的なキュー操作を実行する方法についても説明します。 サンプルは c# コードと使用の記述、 [.NET 用 Azure ストレージ クライアント ライブラリ](https://msdn.microsoft.com/library/azure/dn261237.aspx)します。 ASP.NET の詳細については、次を参照してください。 [ASP.NET](http://www.asp.net)します。

Azure キュー ストレージは、HTTP または HTTPS を使用した認証された呼び出しを介して世界中のどこからでもアクセスできる大量のメッセージを格納するためのサービスです。 キューの 1 つのメッセージの最大サイズは 64 KB で、1 つのキューには、ストレージ アカウントの合計容量の上限に達するまで、数百万のメッセージを格納できます。

## コードでキューにアクセスする

ASP.NET プロジェクトでキューにアクセスするには、Azure キュー ストレージにアクセスする C# ソース ファイルに、次の項目を含める必要があります。

1. C# ファイルの上部にある名前空間の宣言には、これらが含まれているかどうかを確認 **を使用して** ステートメントです。

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. 取得、 **CloudStorageAccount** をストレージ アカウント情報を表すオブジェクト。 次のコードを使用して、Azure サービス構成からストレージ接続文字列とストレージ アカウント情報を取得できます。

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. 取得、 **CloudQueueClient** queueobjects、ストレージ アカウントを参照するオブジェクト。  

        // Create the queueclient.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. 取得、 **CloudQueue** 特定のキューを参照するオブジェクト。

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**注** の次の例に示すすべてのコードの前に上記のコードを使用します。

## コードでキューを作成する

コードで Azure キューを作成するへの呼び出しを追加 **CreateIfNotExists** 上記のコードにします。

    // Create the CloudQuecClient  if it does not exist
    messageQueue.CreateIfNotExists();

## メッセージをキューに追加する

既存のキューにメッセージを挿入するには、新しい作成 **CloudQueueMessage** オブジェクトを呼び出し、 **AddMessage** メソッドです。

A **CloudQueueMessage** オブジェクトは、文字列 (utf-8 形式) またはバイト配列から作成できます。

次の例では、'Hello, World' というメッセージを挿入します。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## キュー内のメッセージを読み取る

PeekMessage() メソッドを呼び出すと、キューの先頭にあるメッセージをキューから削除せずにピークできます。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## キュー内のメッセージを読み取って削除する

コードでは、2 つの手順でキューからメッセージを削除 (デキュー) できます。
1. GetMessage() を呼び出すと、キュー内の次のメッセージが取得されます。 GetMessage() から返されたメッセージは、このキューからメッセージを読み取る他のコードから参照できなくなります。 既定では、このメッセージを参照できない状態は 30 秒間続きます。
2.  キューからのメッセージの削除を完了するには、呼び出す **DeleteMessage**します。

このようにメッセージを 2 つの手順で削除することで、ハードウェアまたはソフトウェアの問題が原因でコードによるメッセージの処理が失敗した場合に、コードの別のインスタンスで同じメッセージを取得し、もう一度処理することができます。 次のコード呼び出し **DeleteMessage** メッセージが処理された直後します。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## 追加オプションを利用してメッセージをデキューする

キューからのメッセージの取得をカスタマイズする方法は 2 つあります。
1 つ目の方法では、(最大 32 個の) メッセージのバッチを取得できます。 2 つ目の方法では、
コードで各メッセージを完全に処理できるように、非表示タイムアウトの
設定を長くまたは短くすることができます。 次のコード例では、
**GetMessages** 1 回の呼び出しで 20 個のメッセージを取得します。 その後、for ループを使用して、
各メッセージを使用して、 **foreach** ループします。 また、各メッセージの
非表示タイムアウトを 5 分に設定します。 この 5 分の
非表示期間は、すべてのメッセージに対して同時に開始します。そのため、
呼び出し **GetMessages**, 、削除されていないすべてのメッセージ
メッセージが再び表示されます。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## キューの長さを取得する

キュー内のメッセージの概数を取得できます。 この
**FetchAttributes** メソッドは要求をキュー サービス
ようキュー サービスに要求します。  **ApproximateMethodCount**
プロパティは、キュー サービスを呼び出さずに、
**FetchAttributes** キュー サービスを呼び出さずのメソッドです。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## Async-Await パターンを一般的なキュー API で使用する

この例では、Async-Await パターンを一般的なキュー API で使用する方法を示します。 このサンプルは、特定のメソッドの非同期バージョンをそれぞれ呼び出しています。これは、各メソッドの Async 接尾辞によって確認できます。 非同期のメソッドを使用するとき、async-await パターンは、呼び出しが完了するまでローカルでの実行を中断します。 この動作により、現在のスレッドで別の作業を実行できるようになるため、パフォーマンスのボトルネックを回避し、アプリケーションの全体的な応答性が向上させることができます。 .NET での Async-await パターンの使用の詳細については、[Async と Await (c# および Visual Basic)] を参照してください (https://msdn.microsoft.com/library/hh191443.aspx)

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## キューを削除する

キューおよびキューに含まれているすべてのメッセージを削除するには、
**削除** 、キュー オブジェクトに対してメソッドです。

    // Get a reference to the CloudQueue object named 'messageQueue' as described in "Access a queue in code"

    // Delete the queue.
    messageQueue.Delete();



## 次のステップ

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

