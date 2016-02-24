<properties 
    pageTitle="Queue ストレージを使用する方法 (C++) | Microsoft Azure" 
    description="Azure で Queue ストレージ サービスを使用する方法について説明します。 サンプルは C++ で記述されています。" 
    services="storage" 
    documentationCenter=".net" 
    authors="tamram" 
    manager="adinah" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/23/2015" 
    ms.author="tamram"/>

# C++ から Queue ストレージを使用する方法  

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

## 概要
このガイドでは、Azure キュー ストレージ サービスを使用して一般的なシナリオを実行する方法について説明します。 サンプルは C++ および使用で記述され、 [C++ 用 Azure ストレージ クライアント ライブラリ](https://github.com/Azure/azure-storage-cpp/blob/v1.0.0/README.md)します。  紹介するシナリオ **挿入**, 、**ピーク**, 、**を取得する**, 、および **を削除する** キュー メッセージだけでなく **を作成して、キューの削除**します。  

>[AZURE.NOTE] このガイドは、以降のバージョン 1.0.0 の C++ の場合、Azure ストレージ クライアント ライブラリを対象います。 推奨されるバージョンは、これはストレージ クライアント ライブラリ 1.0.0、 [NuGet](http://www.nuget.org/packages/wastorage) または [GitHub](https://github.com/)します。 

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## C++ アプリケーションの作成  
このガイドでは、C++ アプリケーション内で実行できるストレージ機能を使用します。  

そのためには、C++ 用 Azure ストレージ クライアント ライブラリをインストールし、Azure サブスクリプションに Azure ストレージ アカウントを作成する必要があります。  

C++ 用 Azure ストレージ クライアント ライブラリをインストールする場合、次の方法を使用できます。

-   **Linux:** 記載されている手順に従って、 [C++ README 用 Azure ストレージ クライアント ライブラリ](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) ページです。  
-   **Windows:** Visual Studio で、[ **ツール > NuGet パッケージ マネージャー > パッケージ マネージャー コンソール**します。 次のコマンドを入力、 [NuGet パッケージ マネージャー コンソール](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) とキーを押します **ENTER**します。  

        Install-Package wastorage 
 
## Queue ストレージにアクセスするようにアプリケーションを構成する
Azure Storage API を使用してキューにアクセスする C++ ファイルの先頭には、次の include ステートメントを追加します。  

    #include "was/storage_account.h"
    #include "was/queue.h"

## Azure のストレージ接続文字列の設定

Azure ストレージ クライアントでは、ストレージ接続文字列を使用して、データ管理サービスにアクセスするためのエンドポイントおよび資格情報を保存します。 クライアント アプリケーションで実行するときに入力してください、次の形式でのストレージ接続文字列で示されたストレージ アカウントのストレージ アカウントとストレージ アクセス キーの名前を使用して、 [Azure ポータル](portal.azure.com) の *AccountName* と *AccountKey* 値。 詳細については、ストレージ アカウントとアクセス キーは、次を参照してください。 [Azure ストレージ アカウントについて](storage-create-storage-account.md)します。 この例では、接続文字列を保持する静的フィールドを宣言する方法を示しています。  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

ローカルの Windows コンピューターで、アプリケーションをテストするには、Microsoft Azure を使用することができます [ストレージ エミュレーター](https://msdn.microsoft.com/library/azure/hh403989.aspx)  と共にインストールされた、 [Azure SDK](http://azure.microsoft.com/downloads/)します。 ストレージ エミュレーターは、ローカルの開発マシンで、Azure で使用できる BLOB、Queue、および Table サービスをシミュレートするユーティリティです。 次の例では、ローカルのストレージ エミュレーターに接続文字列を保持する静的フィールドを宣言する方法を示しています。  

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Azure ストレージ エミュレーターを開始するには、選択、 **開始** ボタンまたはキーを押して、 **Windows** キー。 入力を開始 **Azure ストレージ エミュレーター**, を選択して **Microsoft Azure ストレージ エミュレーター** アプリケーションの一覧からです。  

次のサンプルでは、これら 2 つのメソッドのいずれかを使用してストレージ接続文字列を取得するとします。  

## 接続文字列の取得
使用することができます、 **cloud_storage_account** をストレージ アカウント情報を表すクラス。 ストレージ接続文字列からストレージ アカウント情報を取得、使用することができます、 **解析** メソッドです。 

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

## 方法: キューを作成する
A **cloud_queue_client** オブジェクトでは、キューの参照オブジェクトを取得することができます。 次のコード作成、 **cloud_queue_client** オブジェクトです。  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create a queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

使用して、 **cloud_queue_client** を使用するキューへの参照を取得するオブジェクト。 キューが存在しない場合は作成できます。

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();  

## 方法: メッセージをキューに挿入する
既存のキューにメッセージを挿入する最初の作成新しい **cloud_queue_message**します。 次を呼び出す、 **add_message** メソッドです。 A **cloud_queue_message** 文字列にいずれかから作成できる、または **バイト** 配列。 次のコードでは、キューが存在しない場合は作成し、メッセージ "Hello, World" を挿入します。

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();

    // Create a message and add it to the queue.
    azure::storage::cloud_queue_message message1(U("Hello, World"));
    queue.add_message(message1);  

## 方法: 次のメッセージをピークする
キューの先頭にあるメッセージをピークを呼び出して、キューから削除することがなく、 **peek_message** メソッドです。  

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Peek at the next message.
    azure::storage::cloud_queue_message peeked_message = queue.peek_message();

    // Output the message content.
    std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;

## 方法: キューに配置されたメッセージの内容を変更する
キュー内のメッセージの内容をインプレースで変更できます。 メッセージが作業タスクを表している場合は、この機能を使用して、作業タスクの状態を更新できます。 次のコードでは、キュー メッセージを新しい内容に更新し、表示タイムアウトを設定して、60 秒延長します。 これにより、メッセージに関連付けられている作業の状態が保存され、クライアントにメッセージの操作を続行する時間が 1 分与えられます。 この方法を使用すると、キュー メッセージに対する複数の手順から成るワークフローを追跡でき、ハードウェアまたはソフトウェアの問題が原因で処理手順が失敗した場合に最初からやり直す必要がなくなります。 通常は、さらに再試行回数を保持し、メッセージの再試行回数が n 回を超えた場合はメッセージを削除するようにします。 こうすることで、処理するたびにアプリケーション エラーをトリガーするメッセージから保護されます。  

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));
        
    // Get the message from the queue and update the message contents.
    // The visibility timeout "0" means make it visible immediately.
    // The visibility timeout "60" means the client can get another minute to continue 
    // working on the message.
    azure::storage::cloud_queue_message changed_message = queue.get_message();

    changed_message.set_content(U("Changed message"));
    queue.update_message(changed_message, std::chrono::seconds(60), true);

    // Output the message content.
    std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  

## 方法: 次のメッセージをデキューする
コードでは、2 つの手順でキューからメッセージをデキューします。 呼び出すと **get_message**, 、キュー内に次のメッセージを取得します。 返されたメッセージ **get_message** このキューからメッセージを読み取る他のコードで参照できなくなります。 キューからのメッセージの削除を完了するにも呼び出す必要があります **delete_message**します。 このようにメッセージを 2 つの手順で削除することで、ハードウェアまたはソフトウェアの問題が原因でコードによるメッセージの処理が失敗した場合に、コードの別のインスタンスで同じメッセージを取得し、もう一度処理することができます。 コードの呼び出し **delete_message** メッセージが処理された直後します。

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the next message.
    azure::storage::cloud_queue_message dequeued_message = queue.get_message();
    std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

    // Delete the message.
    queue.delete_message(dequeued_message); 

## 方法: 追加オプションを利用してメッセージをデキューする
キューからのメッセージの取得をカスタマイズする方法は 2 つあります。 1 つ目の方法では、(最大 32 個の) メッセージのバッチを取得できます。 2 つ目の方法では、コードで各メッセージを完全に処理できるように、非表示タイムアウトの設定を長くまたは短くすることができます。 次のコード例では、 **get_messages** 1 回の呼び出しで 20 個のメッセージを取得します。 使用して各メッセージを処理し、 **の** ループします。 また、各メッセージの非表示タイムアウトを 5 分に設定します。 同時にすべてのメッセージに対して、5 分間に開始ので 5 分が経過の呼び出し以降に **get_messages**, 、削除されていないすべてのメッセージが再び表示されます。  

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to 
    // 5 minutes (300 seconds).
    azure::storage::queue_request_options options;
    azure::storage::operation_context context;

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);
        
    for (auto it = messages.cbegin(); it != messages.cend(); ++it)
    {
        // Display the contents of the message.
        std::wcout << U("Get: ") << it->content_as_string() << std::endl;
    }

## 方法: キューの長さを取得する
キュー内のメッセージの概数を取得できます。  **Download_attributes** メソッドは、メッセージ数などのキューの属性を取得するキュー サービスを要求します。  **Approximate_message_count** メソッドは、キューにメッセージの概数を取得します。  

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Fetch the queue attributes.
    queue.download_attributes();

    // Retrieve the cached approximate message count.
    int cachedMessageCount = queue.approximate_message_count();

    // Display number of messages.
    std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  

## 方法: キューを削除する
キューおよびキューに含まれるすべてのメッセージを削除する、 **delete_queue_if_exists** 、キュー オブジェクトに対してメソッドです。

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // If the queue exists and delete it.
    queue.delete_queue_if_exists();  

## 次のステップ
これで、Queue ストレージの基本を学習できました。Azure Storage の詳細については、次のリンク先を参照してください。  

-   [C++ から BLOB ストレージを使用する方法](storage-c-plus-plus-how-to-use-blobs.md)
-   [C++ から Table ストレージを使用する方法](storage-c-plus-plus-how-to-use-tables.md)
-   [C++ での Azure Storage のリソース一覧の取得](storage-c-plus-plus-enumeration.md)
-   [C++ 用ストレージ クライアント ライブラリ リファレンス](http://azure.github.io/azure-storage-cpp)
-   [Azure ストレージのドキュメント](http://azure.microsoft.com/documentation/services/storage/)

 

