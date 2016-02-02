<properties
    pageTitle="キュー ストレージと Visual Studio 接続済みサービスの概要 (クラウド サービス) | Microsoft Azure"
    description="Visual Studio 接続済みサービスを使用してストレージ アカウントに接続した後、Visual Studio のクラウド サービス プロジェクトで Azure キュー ストレージの使用を開始する方法について説明します。"
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


# Azure キュー ストレージと Visual Studio 接続済みサービスの概要 (Cloud Services プロジェクト)

## 概要

Visual Studio を使用してクラウド サービス プロジェクトで Azure ストレージ アカウントを参照または作成した後は、Visual Studio で Azure キュー ストレージを使用して開始する方法を説明 **[接続済みサービス** ダイアログ。

コードでキューを作成する方法を説明します。 また、キュー メッセージの追加、変更、読み取り、削除などの基本的なキュー操作を実行する方法についても説明します。 サンプルは c# コードと使用の記述、 [.NET 用 Azure ストレージ クライアント ライブラリ](https://msdn.microsoft.com/library/azure/dn261237.aspx)します。

**接続済みサービスの追加** 操作によって、プロジェクト内の Azure ストレージにアクセスする適切な NuGet パッケージがインストールされ、プロジェクトの構成ファイルに、ストレージ アカウントの接続文字列が追加されます。

 - 参照してください [.NET からキュー ストレージを使用する方法](storage-dotnet-how-to-use-queues.md) コードでキューを操作する方法についてです。
 - 参照してください [Storage のドキュメント](https://azure.microsoft.com/documentation/services/storage/) Azure ストレージに関する一般的な情報です。
 - 参照してください [クラウド サービスのドキュメント](http://azure.microsoft.com/documentation/services/cloud-services/) Azure クラウド サービスに関する一般的な情報です。
 - 参照してください [ASP.NET](http://www.asp.net) ASP.NET アプリケーションのプログラミングの詳細についてです。


Azure Queue ストレージは、HTTP または HTTPS を使用した認証された呼び出しを介して世界中のどこからでもアクセスできる大量のメッセージを格納するためのサービスです。 キューの 1 つのメッセージの最大サイズは 64 KB で、1 つのキューには、ストレージ アカウントの合計容量の上限に達するまで、数百万のメッセージを格納できます。


## コードでキューにアクセスする

Visual Studio Cloud Services プロジェクトでキューにアクセスするには、Azure キュー ストレージにアクセスする C# ソース ファイルに、次の項目を含める必要があります。

1. C# ファイル冒頭の名前空間宣言に、次の **using** ステートメントが含まれていることをご確認ください。

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. ストレージ アカウント情報を表す **CloudStorageAccount** オブジェクトを取得します。 次のコードを使用して、Azure サービス構成からストレージ接続文字列とストレージ アカウント情報を取得できます。

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. ストレージ アカウント内のキュー オブジェクトを参照する **CloudQueueClient** オブジェクトを取得します。

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. 特定のキューを参照する **CloudQueue** オブジェクトを取得します。

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");



**注:** 上記のコードはすべて、以下の例に示すコードの前に使用してください。

## コードでキューを作成する

コードでキューが作成するには、**CreateIfNotExists** に呼び出しを追加します。

    // Get a reference to a CloudQueue object with the variable name 'messageQueue'
    // as described in the "Access queues in code" section.
    
    // Create the CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## メッセージをキューに追加する

既存のキューにメッセージを挿入するには、新しい **CloudQueueMessage** オブジェクトを作成した後、**AddMessage** メソッドを呼び出します。

**CloudQueueMessage** オブジェクトは、文字列 (UTF-8 形式) またはバイト配列から作成できます。

次の例では、'Hello, World' というメッセージを挿入します。

    // Get a reference to a CloudQueue object with the variable name 'messageQueue' as described in
    // the "Access queues in code" section.
    
    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## キュー内のメッセージを読み取る

**PeekMessage** メソッドを呼び出すと、キューの先頭にあるメッセージをキューから削除せずにピークできます。

    // Get a reference to a CloudQueue object with the variable name 'messageQueue'
    // as described in the "Access queues in code" section.
    
    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## キュー内のメッセージを読み取って削除する

コードでは、2 つの手順でキューからメッセージを削除 (デキュー) できます。

1. **GetMessage** を呼び出すと、キュー内の次のメッセージが取得されます。 **GetMessage** から返されたメッセージは、このキューからメッセージを読み取る他のコードから参照できなくなります。 既定では、このメッセージを参照できない状態は 30 秒間続きます。
2.  また、キューからのメッセージの削除を完了するには、**DeleteMessage** を呼び出します。

このようにメッセージを 2 つの手順で削除することで、ハードウェアまたはソフトウェアの問題が原因でコードによるメッセージの処理が失敗した場合に、コードの別のインスタンスで同じメッセージを取得し、もう一度処理することができます。 次のコードでは、メッセージが処理された直後に **DeleteMessage** を呼び出します。

    // Get a reference to a CloudQueue object with the variable name 'messageQueue'
    // as described in the "Access queues in code" section.
    
    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();
    
    // Process the message in less than 30 seconds
    
    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);

## オプションを追加してキュー メッセージを処理および削除する

キューからのメッセージの取得をカスタマイズする方法は 2 つあります。

 - メッセージのバッチを取得できます (最大 32 個)。
 - コードを増減できるように、長いまたは短いの非表示タイムアウトを設定できます。
設定を長くまたは短くすることができます。 次のコード例では、
**GetMessages** 1 回の呼び出しで 20 個のメッセージを取得します。 その後、for ループを使用して、
各メッセージを使用して、 **foreach** ループします。 また、各メッセージの
非表示タイムアウトを 5 分に設定します。 この 5 分の
非表示期間は、すべてのメッセージに対して同時に開始します。そのため、
呼び出し **GetMessages**, 、削除されていないすべてのメッセージ
メッセージが再び表示されます。

次に例を示します。

    // Get a reference to a CloudQueue object with the variable name 'messageQueue'
    // as described in the "Access queues in code" section.
    
    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
    
        // Then delete the message after processing
        messageQueue.DeleteMessage(message);
    
    }

## キューの長さを取得する

キュー内のメッセージの概数を取得できます。 この
**FetchAttributes** メソッド キュー サービスに要求
ようキュー サービスに要求します。  **ApproximateMethodCount**
プロパティは、キュー サービスを呼び出さずに、
**FetchAttributes** 呼び出さずに、キュー サービスのメソッドです。

    // Get a reference to a CloudQueue object with the variable name 'messageQueue'
    // as described in the "Access queues in code" section.
    
    // Fetch the queue attributes.
    messageQueue.FetchAttributes();
    
    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;
    
    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## Async-Await パターンを一般的な Azure Queue API で使用する

この例では、Async-Await パターンを一般的な Azure Queue API で使用する方法を示します。 このサンプルは、特定のメソッドの非同期バージョンをそれぞれ呼び出しています。これは、各メソッドの **Async** 接尾辞によって確認できます。 非同期のメソッドを使用するとき、async-await パターンは、呼び出しが完了するまでローカルでの実行を中断します。 この動作により、現在のスレッドで別の作業を実行できるようになるため、パフォーマンスのボトルネックを回避し、アプリケーションの全体的な応答性が向上させることができます。 .NET での Async-await パターンの使用の詳細については、[Async と Await (c# および Visual Basic)] を参照してください (https://msdn.microsoft.com/library/hh191443.aspx)

    // Get a reference to a CloudQueue object with the variable name 'messageQueue'
    // as described in the "Access queues in code" section.
    
    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");
    
    // Add the message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");
    
    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);
    
    // Delete the message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## キューを削除する

キューおよびキューに格納されているすべてのメッセージを削除するには、キュー オブジェクトの **Delete** メソッドを呼び出します。

    // Get a reference to a CloudQueue object with the variable name 'messageQueue'
    // as described in the "Access queues in code" section.
    
    // Delete the queue.
    messageQueue.Delete();

## 次のステップ

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]





