<properties
    pageTitle=".NET からキュー ストレージを使用する方法 | Microsoft Azure"
    description="Microsoft Azure Queue ストレージを使用して、キューの作成と削除のほか、キュー メッセージの挿入、ピーク、取得、および削除を行う方法について説明します。"
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article" 
    ms.date="12/04/2015"
    ms.author="robinsh"/>

# .NET から Queue ストレージを使用する方法

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

## 概要

このガイドでは、Azure BLOB サービスを使用して
一般的なシナリオを実行する方法について説明します。 サンプルは c \# コードで記述します。
Azure .NET 用ストレージ クライアント ライブラリを利用しています。 紹介するシナリオ **挿入**,、
**ピーク**, 、**取得**, 、および **削除** キュー メッセージだけでなく
**作成して、キューの削除**します。

[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-configure-connection-string-include](../../includes/storage-configure-connection-string-include.md)]

## プログラムでキュー ストレージにアクセスする

[AZURE.INCLUDE [storage-dotnet-obtain-assembly](../../includes/storage-dotnet-obtain-assembly.md)]

### 名前空間宣言
プログラムを使用してオートスケーリング アプリケーション ブロックにアクセスする
冒頭部分に、次の名前空間宣言を追加します。

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;

必ず `Microsoft.WindowsAzure.Storage.dll` アセンブリを参照してください。

[AZURE.INCLUDE [storage-dotnet-retrieve-conn-string](../../includes/storage-dotnet-retrieve-conn-string.md)]

## キューを作成する

A **CloudQueueClient** オブジェクトでは、キューの参照オブジェクトを取得することができます。
次のコード作成、 **CloudQueueClient** オブジェクトです。 このガイドの
すべてのコードでは、Azure アプリケーションのサービス構成に格納されている
接続文字列を使用します。 そのほかにも
 **CloudStorageAccount** オブジェクトです。 参照してください [CloudStorageAccount][]
のドキュメントを参照してください。

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

使用して、 **queueClient** キューへの参照を取得するオブジェクト
取得します。 キューが存在しない場合は作成できます。

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

## メッセージをキューに挿入する

既存のキューにメッセージを挿入するには、最初に新しい
**CloudQueueMessage**します。 次を呼び出す、 **AddMessage** メソッドです。 A
**CloudQueueMessage** 、文字列 (utf-8 にいずれかから作成できます
形式) または **バイト** 配列。 次のコードでは、キューが存在しない場合は
作成し、メッセージ '"Hello, World"' を挿入します。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.AddMessage(message);

## 次のメッセージをピークする

キューの先頭にあるメッセージをキューから削除せずにピークするには、
呼び出してキューから、 **PeekMessage** メソッドです。

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Peek at the next message
    CloudQueueMessage peekedMessage = queue.PeekMessage();

    // Display message.
    Console.WriteLine(peekedMessage.AsString);

## キューに配置されたメッセージの内容を変更する

キュー内のメッセージの内容をインプレースで変更できます。 また、
メッセージが作業タスクを表している場合は、この機能を使用して
作業タスクの状態を更新できます。 次のコードでは、キュー メッセージを新しい内容に更新し、
表示タイムアウトを設定して、60 秒延長し
ます。 これにより、メッセージに関連付けられている作業の状態が保存され、
クライアントにメッセージの操作を続行する時間が 1 分与えられます。 クラスターの
キュー メッセージに対する複数の手順から成る
ワークフローを追跡でき、ハードウェアまたはソフトウェアの問題が原因で
処理手順が失敗した場合に最初からやり直す必要がなくなります。 通常は、
さらに再試行回数を保持し、メッセージの再試行回数が
 *n* 、削除するようにします。 こうすることで、
処理するたびにアプリケーション エラーをトリガーするメッセージから保護されます。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the message from the queue and update the message contents.
    CloudQueueMessage message = queue.GetMessage();
    message.SetMessageContent("Updated contents.");
    queue.UpdateMessage(message,
        TimeSpan.FromSeconds(60.0),  // Make it visible for another 60 seconds.
        MessageUpdateFields.Content | MessageUpdateFields.Visibility);

## 次のメッセージをデキューする

コードでは、2 つの手順でキューからメッセージをデキューします。 既定では、
**GetMessage**, 、キュー内に次のメッセージを取得します。 返されたメッセージ
 **GetMessage** メッセージを読み取る他のコードで参照できなくなります
参照できなくなります。 既定では、このメッセージを参照できない状態は 30 秒間続き
ます。 また、キューからのメッセージの削除を完了するには、
呼び出す **DeleteMessage**します。 このようにメッセージを 2 つの手順で削除することで、
ハードウェアまたはソフトウェアの問題が原因でコードによるメッセージの
処理が失敗した場合に、コードの別のインスタンスで同じメッセージを取得し、
もう一度処理することができます。 コードの呼び出し **DeleteMessage** メッセージが表示されたら右
呼び出します。

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the next message
    CloudQueueMessage retrievedMessage = queue.GetMessage();

    //Process the message in less than 30 seconds, and then delete the message
    queue.DeleteMessage(retrievedMessage);

## Async-Await パターンを一般的なキュー ストレージ API で使用する

この例では、Async-Await パターンを一般的なキュー ストレージ API で使用する方法を示します。 によって示されるサンプルは、非同期バージョンのこれらの特定のメソッドを呼び出して、 *Async* 各メソッドのサフィックスです。 非同期のメソッドを使用する場合、async-await パターンは、呼び出しが完了するまでローカルでの実行を中断します。 この動作により、現在のスレッドで別の作業を実行できるようになるため、パフォーマンスのボトルネックを回避し、アプリケーションの全体的な応答性を向上させることができます。 .NET での Async-await パターンの使用の詳細について参照してください [Async と Await (c# および Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create the queue if it doesn't already exist
    if(await queue.CreateIfNotExistsAsync())
    {
        Console.WriteLine("Queue '{0}' Created", queue.Name);
    }
    else
    {
        Console.WriteLine("Queue '{0}' Exists", queue.Name);
    }

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await queue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await queue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

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

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

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
**FetchAttributes** メソッド キュー サービスに要求
ようキュー サービスに要求します。  **ApproximateMessageCount**
プロパティは、キュー サービスを呼び出さずに、
**FetchAttributes** 呼び出さずに、キュー サービスのメソッドです。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Fetch the queue attributes.
    queue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = queue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## キューを削除する

キューおよびキューに含まれているすべてのメッセージを削除するには、
**削除** 、キュー オブジェクトに対してメソッドです。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Delete the queue.
    queue.Delete();

## 次のステップ

これで、キュー ストレージの基本を学習できました。
さらに複雑なストレージ タスクについて学習するには、次のリンク先を参照してください。

- 利用可能な API の詳細については、Queue サービスのリファレンス ドキュメントを参照してください。
    - [.NET 用ストレージ クライアント ライブラリ リファレンス](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST API リファレンス](http://msdn.microsoft.com/library/azure/dd179355)
- Azure Storage を使用して作業を記述するコードを簡略化する方法を学習、 [Azure web ジョブ SDK](../websites-dotnet-webjobs-sdk/)します。
- Azure でデータを格納するための追加のオプションについては、他の機能ガイドも参照してください。
    - 使用 [テーブル ストレージ](storage-dotnet-how-to-use-tables.md) 構造化データを格納します。 
    - 使用 [Blob ストレージ](storage-dotnet-how-to-use-blobs.md) 非構造化データを格納します。
    - 使用 [SQL Database](sql-database-dotnet-how-to-use.md) リレーショナル データを格納します。

  [Download and install the Azure SDK for .NET]: /develop/net/
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
  [CloudStorageAccount]: http://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudstorageaccount_methods.aspx
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configuring Connection Strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
 

