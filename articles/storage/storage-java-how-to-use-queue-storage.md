<properties 
    pageTitle="Java から Queue ストレージを使用する方法 | Microsoft Azure" 
    description="Azure Queue サービスを使用して、キューの作成と削除のほか、メッセージの挿入、取得、および削除を行う方法を説明します。 コード サンプルは Java で記述されています。" 
    services="storage" 
    documentationCenter="java" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="12/01/2015" 
    ms.author="robmcm"/>

# Java から Queue ストレージを使用する方法

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

## 概要

このガイドでは、Azure キュー ストレージ サービスを使用して一般的なシナリオを実行する方法について説明します。 サンプルが使用して Java で記述され、 [Azure Storage SDK for Java][]します。 紹介するシナリオ **挿入**, 、**ピーク**, 、**を取得する**, 、および **を削除する** キュー メッセージだけでなく **を作成する** と **を削除する** キューです。 キューの詳細については、次を参照してください。、 [次のステップ](#NextSteps) セクションです。

注: SDK は、Android デバイスで Azure Storage を使用する開発者向けに用意されています。 詳細については、次を参照してください。、 [Azure Storage SDK for Android][]します。 

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## Java アプリケーションの作成

このガイドで使用するストレージ機能は、Java アプリケーション内でローカルで実行することも、Azure の Web ロールまたは worker ロールで動作するコード内で実行することもできます。

そのためには、Java Development Kit (JDK) をインストールし、Azure サブスクリプションに Azure ストレージ アカウントを作成する必要があります。 これを実行して後、は、開発システムが最小要件およびに示されている依存関係を満たしていることを確認する必要があります。、 [Azure Storage SDK for Java][] GitHub のリポジトリです。 システムがそれらの要件を満たしている場合は、指示に従って、そのリポジトリからシステムに Azure Storage Libraries for Java をダウンロードしてインストールできます。 それらのタスクが完了したら、この記事の例を使用した Java アプリケーションを作成できます。

## キュー ストレージにアクセスするようにアプリケーションを構成する

Azure Storage API を使用してキューにアクセスする Java ファイルの先頭には、次の import ステートメントを追加します。

    // Include the following imports to use queue APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.queue.*;

## Azure のストレージ接続文字列の設定

Azure ストレージ クライアントでは、ストレージ接続文字列を使用して、データ管理サービスにアクセスするためのエンドポイントおよび資格情報を保存します。 クライアント アプリケーションで実行するときに必要ストレージ接続文字列は、次の形式に、ストレージ アカウントの名前を使用して、ストレージ アカウントのプライマリ アクセス キーに表示、 [Azure ポータル](portal.azure.com) の *AccountName* と *AccountKey* 値。 この例では、接続文字列を保持する静的フィールドを宣言する方法を示しています。

    // Define the connection-string with your values.
    public static final String storageConnectionString = 
        "DefaultEndpointsProtocol=http;" + 
        "AccountName=your_storage_account;" + 
        "AccountKey=your_storage_account_key";

Microsoft Azure ロール内で実行されるアプリケーション、この文字列で、サービスの構成ファイルに保存することができます *ServiceConfiguration.cscfg*, への呼び出しでアクセスできると、 **RoleEnvironment.getConfigurationSettings** メソッドです。 接続文字列を取得する例をここでは、 **設定** という名前の要素 *StorageConnectionString* サービス構成ファイルで。

    // Retrieve storage account from connection-string.
    String storageConnectionString = 
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

次のサンプルでは、これら 2 つのメソッドのいずれかを使用してストレージ接続文字列を取得するとします。

## 方法: キューを作成する

A **CloudQueueClient** オブジェクトでは、キューの参照オブジェクトを取得することができます。 次のコード作成、 **CloudQueueClient** オブジェクトです。 (注: 作成する方法もありますがある **CloudStorageAccount** のオブジェクト詳細については、次を参照してください。 **CloudStorageAccount** で、 [Azure Storage Client SDK Reference]。)。

使用して、 **CloudQueueClient** を使用するキューへの参照を取得するオブジェクト。 キューが存在しない場合は作成できます。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
           CloudStorageAccount.parse(storageConnectionString);

       // Create the queue client.
       CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

       // Retrieve a reference to a queue.
       CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Create the queue if it doesn't already exist.
       queue.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: メッセージをキューに追加する

既存のキューにメッセージを挿入する最初の作成新しい **CloudQueueMessage**します。 次を呼び出す、 **addMessage** メソッドです。 A **CloudQueueMessage** 、文字列 (utf-8 形式) またはバイト配列から作成できます。 次のコードでは、キューが存在しない場合は作成し、メッセージ "Hello, World" を挿入します。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Create the queue if it doesn't already exist.
        queue.createIfNotExists();

        // Create a message and add it to the queue.
        CloudQueueMessage message = new CloudQueueMessage("Hello, World");
        queue.addMessage(message);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: 次のメッセージをピークする

キューの先頭にあるメッセージをピークを呼び出して、キューから削除することがなく **peekMessage**します。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");
            
        // Peek at the next message.
        CloudQueueMessage peekedMessage = queue.peekMessage();
            
        // Output the message value.
        if (peekedMessage != null)
        {
          System.out.println(peekedMessage.getMessageContentAsString());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: キューに配置されたメッセージの内容を変更する

キュー内のメッセージの内容をインプレースで変更できます。 メッセージが作業タスクを表している場合は、この機能を使用して、作業タスクの状態を更新できます。 次のコードでは、キュー メッセージを新しい内容に更新し、表示タイムアウトを設定して、60 秒延長します。 これにより、メッセージに関連付けられている作業の状態が保存され、クライアントにメッセージの操作を続行する時間が 1 分与えられます。 この方法を使用すると、キュー メッセージに対する複数の手順から成るワークフローを追跡でき、ハードウェアまたはソフトウェアの問題が原因で処理手順が失敗した場合に最初からやり直す必要がなくなります。 同様に、再試行回数を保持する通常、メッセージが再試行された場合、複数の *n* 、削除するようにします。 こうすることで、処理するたびにアプリケーション エラーをトリガーするメッセージから保護されます。

次のコード サンプルでは、キュー内でメッセージを検索し、内容が "Hello, World" に一致する最初のメッセージを見つけ、メッセージの内容を変更し、終了します。 

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // The maximum number of messages that can be retrieved is 32. 
        final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

        // Loop through the messages in the queue.
        for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
        {
            // Check for a specific string.
            if (message.getMessageContentAsString().equals("Hello, World"))
            {
                // Modify the content of the first matching message.
                message.setMessageContent("Updated contents.");
                // Set it to be visible in 30 seconds.
                EnumSet<MessageUpdateFields> updateFields = 
                    EnumSet.of(MessageUpdateFields.CONTENT,
                    MessageUpdateFields.VISIBILITY);
                // Update the message.
                queue.updateMessage(message, 30, updateFields, null, null);
                break;
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

また、次のコード サンプルでは、キューの最初に参照したメッセージのみを更新します。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage message = queue.retrieveMessage();
            
        if (message != null)
        {
            // Modify the message content.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 60 seconds.
            EnumSet<MessageUpdateFields> updateFields = 
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 60, updateFields, null, null);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: キューの長さを取得する

キュー内のメッセージの概数を取得できます。  **DownloadAttributes** メッセージの数は、キュー内の数を含むメソッドは、現在の値をサービスにキューを要求します。 キュー サービスが要求に応答した後にメッセージが追加または削除される可能性があるため、これらの値は概数にすぎません。  **GetApproximateMessageCount** メソッドへの呼び出しによって取得された最後の値を返す **downloadAttributes**, 、キュー サービスを呼び出さずにします。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Download the approximate message count from the server.
        queue.downloadAttributes();

        // Retrieve the newly cached approximate message count.
        long cachedMessageCount = queue.getApproximateMessageCount();
            
        // Display the queue length.
        System.out.println(String.format("Queue length: %d", cachedMessageCount));
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: 次のメッセージをデキューする

コードでは、2 つの手順でキューからメッセージをデキューします。 呼び出すと **retrieveMessage**, 、キュー内に次のメッセージを取得します。 返されたメッセージ **retrieveMessage** このキューからメッセージを読み取る他のコードで参照できなくなります。 既定では、このメッセージを参照できない状態は 30 秒間続きます。 キューからのメッセージの削除を完了するにも呼び出す必要があります **deleteMessage**します。 このようにメッセージを 2 つの手順で削除することで、ハードウェアまたはソフトウェアの問題が原因でコードによるメッセージの処理が失敗した場合に、コードの別のインスタンスで同じメッセージを取得し、もう一度処理することができます。 コードの呼び出し **deleteMessage** メッセージが処理された直後します。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage retrievedMessage = queue.retrieveMessage();
            
        if (retrievedMessage != null)
        {
            // Process the message in less than 30 seconds, and then delete the message.
            queue.deleteMessage(retrievedMessage);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }


## メッセージのデキュー用の追加オプション

キューからのメッセージの取得をカスタマイズする方法は 2 つあります。 1 つ目の方法では、(最大 32 個の) メッセージのバッチを取得できます。 2 つ目の方法では、コードで各メッセージを完全に処理できるように、非表示タイムアウトの設定を長くまたは短くすることができます。

次のコード例では、 **retrieveMessages** 1 回の呼び出しで 20 個のメッセージを取得します。 使用して各メッセージを処理し、 **の** ループします。 また、各メッセージの非表示タイムアウトを 5 分 (300 秒) に設定します。 5 分間の同時にすべてのメッセージので 5 分が経過するの呼び出し以降に開始 **retrieveMessages**, 、削除されていないすべてのメッセージが再び表示されます。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
        for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
            // Do processing for all messages in less than 5 minutes, 
            // deleting each message after processing.
            queue.deleteMessage(message);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: キューを一覧表示する

現在のキューの一覧を取得する呼び出し、 **CloudQueueClient.listQueues()** のコレクションを返すメソッド **CloudQueue** オブジェクトです。 

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient =
            storageAccount.createCloudQueueClient();

        // Loop through the collection of queues.
        for (CloudQueue queue : queueClient.listQueues())
        {
            // Output each queue name.
            System.out.println(queue.getName());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 方法: キューを削除する

キューおよびキューに含まれるすべてのメッセージを削除する、 **deleteIfExists** メソッドを **CloudQueue** オブジェクトです。

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = 
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Delete the queue if it exists.
        queue.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## 次のステップ

これで、Queue ストレージの基本を学習できました。さらに複雑なストレージ タスクを実行するには、次のリンク先を参照してください。

- [Azure Storage SDK for Java][]
- [Azure ストレージ クライアント SDK リファレンス][]
- [Azure Storage REST API][]
- [Azure のストレージ チーム ブログ][]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Storage Client SDK Reference]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/

