<properties
    pageTitle="キュー ストレージと Visual Studio 接続済みサービスの概要 (Web ジョブ プロジェクト) | Microsoft Azure"
    description="Visual Studio 接続済みサービスを使用してストレージ アカウントに接続した後、Web ジョブ プロジェクトで Azure キュー ストレージの使用を開始する方法について説明します。"
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

# Azure キュー ストレージと Visual Studio 接続済みサービスの概要 (Web ジョブ プロジェクト)

## 概要

この記事で説明する方法を作成または Visual Studio を使用して Azure ストレージ アカウントを参照した後は、Visual Studio の Azure web ジョブ プロジェクトで Azure キュー ストレージを使用して開始  **[接続済みサービス** ] ダイアログ ボックス。 追加すると、ストレージ アカウントを web ジョブ プロジェクトを Visual Studio を使用して **[接続済みサービス** ダイアログ ボックスで、適切な Azure Storage の NuGet パッケージがインストールされている、適切な .NET 参照がプロジェクトに追加およびストレージ アカウントの接続文字列が App.config ファイルで更新されます。  

この記事では、Azure Web ジョブ SDK バージョン 1.x を Azure キュー ストレージ サービスとともに使用する方法を示す C# コード サンプルについて説明します。

Azure キュー ストレージは、HTTP または HTTPS を使用した認証された呼び出しを介して世界中のどこからでもアクセスできる大量のメッセージを格納するためのサービスです。 キューの 1 つのメッセージの最大サイズは 64 KB で、1 つのキューには、ストレージ アカウントの合計容量の上限に達するまで、数百万のメッセージを格納できます。 参照してください [.NET からキュー ストレージを使用する方法](storage-dotnet-how-to-use-queues.md) の詳細。 ASP.NET の詳細については、次を参照してください。 [ASP.NET](http://www.asp.net)します。



## キュー メッセージを受信したときに関数をトリガーする方法

キュー メッセージを受信したときに WebJobs SDK が呼び出す関数を記述するには、 **QueueTrigger** 属性です。 属性コンストラクターは、ポーリングのためにキューの名前を指定する文字列パラメーター受け取ります。 こともできます [キューの名前を動的に設定](how-to-set-configuration-options)します。

### 文字列のキュー メッセージ

次の例では、キューを文字列メッセージを表すので **QueueTrigger** という名前の文字列パラメーターに適用 **logMessage** キュー メッセージの内容が含まれています。 関数 [ダッシュ ボードにログ メッセージを書き込みます](#how-to-write-logs)します。


        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

それに **文字列**, 、バイト配列の場合は、パラメーターは、 **CloudQueueMessage** オブジェクト、または定義した POCO します。

### POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) キュー メッセージ

次の例では、JSON がキュー メッセージに含まれる、 **BlobInformation** オブジェクトを含む、 **BlobName** プロパティです。 SDK は自動的にオブジェクトを逆シリアル化します。

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

SDK を使用して、 [Newtonsoft.Json NuGet パッケージ](http://www.nuget.org/packages/Newtonsoft.Json) およびメッセージを逆シリアル化します。 キュー メッセージを Web ジョブ SDK を使用しないプログラムで作成する場合は、SDK が解析できる POCO キュー メッセージを作成する次の例のようなコードを記述できます。

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### Async 関数

次の async 関数 [ダッシュ ボードにログを書き込みます](#how-to-write-logs)します。

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Async 関数がかかる場合があります、 [キャンセル トークン](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), の blob をコピーする次の例に示すようにします。 (の詳細については、 **queueTrigger** のプレース ホルダーを参照してください、 [Blob](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) セクションです)。

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

## QueueTrigger 属性が連携する種類

使用する **QueueTrigger** 、次の種類。

* **文字列**
* JSON としてシリアル化された POCO の型
* **byte[]**
* **CloudQueueMessage**

## ポーリング アルゴリズム

SDK はランダムな指数バックオフ アルゴリズムを実装することで、ストレージ トランザクション コストにおけるアイドル状態のキューのポーリングの影響を軽減しています。  メッセージが見つかった場合、SDK は 2 秒間待機した後、別のメッセージをチェックします。 メッセージが見つからない場合は約 4 秒間待機してから再試行します。 その後もキュー メッセージの取得失敗が続けば、待機時間は規定値として 1 分間に設定されている最大待機時間に達するまで増え続けます。 [最大待機時間は構成可能な](#how-to-set-configuration-options)です。

## 複数のインスタンス

Web アプリが複数のインスタンス上で稼働している場合、継続的な Web ジョブは各マシン上で実行され、各マシンがトリガーを待機して関数の実行を試行します。 一部のシナリオでは、これによっていくつかの関数が同じデータを 2 回処理する場合があるため、関数をべき等にする (同じ入力データで関数を繰り返し呼び出しても重複した結果を生成しないように記述する) 必要があります。  

## 並列実行

異なるキューをリッスンする複数の関数を使用している場合、複数のメッセージを同時に受信したとき、SDK では並行してそれらを呼び出します。

1 つのキューに対して複数のメッセージが受信される場合も同様に処理されます。 既定では、SDK は一度にキュー メッセージ 16 個のバッチを取得し、それらを並列処理する関数を実行します。 [バッチ サイズは構成可能な](#how-to-set-configuration-options)です。 処理中のメッセージの数がバッチ サイズの半分まで減少すると、SDK は別のバッチを取得し、そのメッセージの処理を開始します。 そのため、1 つの関数につき同時に処理されるメッセージの最大数は、バッチ サイズの 1.5 倍です。 この制限を持つ各関数に個別に適用されます。、 **QueueTrigger** 属性です。 1 つのキューで受信した複数のメッセージを並列に実行したくない場合は、バッチ サイズを 1 に設定します。

## キューまたはキュー メッセージ メタデータの取得

メソッド シグネチャにパラメーターを追加することで、次のメッセージ プロパティを取得できます。

* **DateTimeOffset** expirationTime
* **DateTimeOffset** insertionTime
* **DateTimeOffset** nextVisibleTime
* **文字列** queueTrigger (メッセージのテキストが含まれます)
* **文字列** id
* **文字列** popReceipt
* **int** dequeueCount

Azure storage API を直接操作するかどうか、追加することも、 **CloudStorageAccount** パラメーター。

次の例では、このメタデータをすべて INFO アプリケーション ログに書き込みます。 この例では logMessage と queueTrigger の両方にキュー メッセージの内容が含まれます。

        public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
            DateTimeOffset expirationTime,
            DateTimeOffset insertionTime,
            DateTimeOffset nextVisibleTime,
            string id,
            string popReceipt,
            int dequeueCount,
            string queueTrigger,
            CloudStorageAccount cloudStorageAccount,
            TextWriter logger)
        {
            logger.WriteLine(
                "logMessage={0}\n" +
            "expirationTime={1}\ninsertionTime={2}\n" +
                "nextVisibleTime={3}\n" +
                "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
                "queue endpoint={7} queueTrigger={8}",
                logMessage, expirationTime,
                insertionTime,
                nextVisibleTime, id,
                popReceipt, dequeueCount,
                cloudStorageAccount.QueueEndpoint,
                queueTrigger);
        }

サンプル コードによって書き込まれたサンプル ログを次に示します。

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

## グレースフル シャットダウン

継続的な web ジョブで実行されている関数が受け入れることができる、 **CancellationToken** これは、web ジョブの終了が近づくと、関数に通知するオペレーティング システムによってパラメーター。 この通知を使用すれば、関数が予期せず終了してデータが不整合な状態になることを防止できます。

次の例では、関数内で Web ジョブの終了が迫っているか確認する方法を示します。

    public static void GracefulShutdownDemo(
                [QueueTrigger("inputqueue")] string inputText,
                TextWriter logger,
                CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(1000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }

**注:** ダッシュ ボードが正しく示されていない状態とシャット ダウンされている関数の出力です。

詳細については、次を参照してください。 [web ジョブのグレースフル シャット ダウン](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR)します。   

## キュー メッセージの処理中にキュー メッセージを作成する方法

新しいキュー メッセージを作成する関数を記述する、 **キュー** 属性です。 ような **QueueTrigger**, のキューの名前を文字列として渡すこともできます [キューの名前を動的に設定](#how-to-set-configuration-options)します。

### 文字列のキュー メッセージ

次の非 Async コード サンプルでは、"inputqueue" という名前のキューに受信したキュー メッセージと同じ内容で、"outputqueue" という名前のキューにキュー メッセージを新しく作成します (関数を使用して非同期 **IAsyncCollector<T>** このセクションで後で示すようにします)。


        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) キュー メッセージ

文字列ではなく poco オブジェクトを含むキュー メッセージを作成するへの output パラメーターとして POCO 型を渡す、 **キュー** 属性コンス トラクターです。

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

SDK はオブジェクトを JSON に自動的にシリアル化します。 オブジェクトが null の場合でもキュー メッセージは常に作成されます。

### (非同期関数での) 複数のメッセージの作成

複数のメッセージを作成するには、出力キューのパラメーターの型を確認 **ICollector<T>** または **IAsyncCollector<T>**, の次の例に示すようにします。

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

各キュー メッセージがすぐに作成されたときに、 **追加** メソッドが呼び出されます。

### キューの属性が連携する種類

使用することができます、 **キュー** 次のパラメーター型の属性。

* **文字列をすべて** (関数が終了すると、パラメーター値が null でない場合は、キュー メッセージを作成します)
* **byte[] を** (と同様に動作 **文字列**)
* **out CloudQueueMessage** (と同様に動作 **文字列**)
* **out POCO** (シリアル化可能な型では、メッセージを作成、null オブジェクトで関数が終了すると、パラメーターが null の場合)
* **ICollector**
* **IAsyncCollector**
* **CloudQueue** (メッセージを作成、Azure Storage API を直接使用して手動で)

### 関数本体での Web ジョブ SDK 属性の使用

など、web ジョブ SDK 属性を使用する前に、関数で何らかの処理を行う必要がある場合 **キュー**, 、**Blob**, 、または **テーブル**, 、使用することができます、 **IBinder** インターフェイスです。

次の例では、入力キュー メッセージを取得して同じ内容の新しい出力キュー メッセージを作成します。 出力キュー名は、関数本体のコードによって設定されます。

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

 **IBinder** インターフェイスもと併用して、 **テーブル** と **Blob** 属性です。

## キュー メッセージの処理中に BLOB およびテーブルの読み書きを行う方法

 **Blob** と **テーブル** 属性では、blob およびテーブルを読み書きすることができます。 このセクションのサンプルは、BLOB に適用されます。 Blob を作成または更新されたときに、プロセスを開始する方法を示すコード サンプルでは、次を参照してください。 [web ジョブ SDK で Azure blob ストレージを使用する方法](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), 、テーブルを読み書きするコード サンプルでは、「と [web ジョブ SDK で Azure テーブル ストレージを使用する方法](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)します。

### BLOB の操作を開始する文字列キュー メッセージ

キュー メッセージで、文字列を含む **queueTrigger** で使用できるプレース ホルダーは、 **Blob** 属性の **blobPath** 、メッセージの内容を含むパラメーター。

次の例では使用 **ストリーム** blob を読み書きするオブジェクト。 キュー メッセージは、textblobs コンテナーにある BLOB の名前です。 名前に "-new" を加えた BLOB のコピーが同じコンテナー内に作成されます。

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

 **Blob** 属性のコンス トラクターは、 **blobPath** コンテナーと blob 名を指定するパラメーターです。 このプレース ホルダーの詳細については、次を参照してください [web ジョブ SDK で Azure blob ストレージを使用する方法](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),、。

属性を修飾すると、 **ストリーム** オブジェクト、もう 1 つのコンス トラクターのパラメーターの指定、 **FileAccess** として読み取り、書き込み、または読み取り/書き込みモードです。

次の例では、 **CloudBlockBlob** blob を削除するオブジェクト。 キュー メッセージは、BLOB の名前です。

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) キュー メッセージ

キュー メッセージで、JSON として格納されている POCO、内のオブジェクトのプロパティを指定するプレース ホルダーを使用することができます、 **キュー** 属性の **blobPath** パラメーター。 使用することも [キュー メタデータ プロパティ名](#get-queue-or-queue-message-metadata) プレース ホルダーとして。

次の例では、BLOB を別の拡張子を持つ新しい BLOB にコピーします。 キュー メッセージは、 **BlobInformation** を含むオブジェクトを **BlobName** と **BlobNameWithoutExtension** プロパティです。 プロパティの名前の blob パス内のプレース ホルダーとして使用されます、 **Blob** 属性です。

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

SDK を使用して、 [Newtonsoft.Json NuGet パッケージ](http://www.nuget.org/packages/Newtonsoft.Json) およびメッセージを逆シリアル化します。 キュー メッセージを Web ジョブ SDK を使用しないプログラムで作成する場合は、SDK が解析できる POCO キュー メッセージを作成する次の例のようなコードを記述できます。

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

Blob をオブジェクトにバインドする前に、関数で何らかの処理を行う必要がある場合は、関数の本体で属性を使用することができます [Queue 属性について前に示したよう](#use-webjobs-sdk-attributes-in-the-body-of-a-function)します。

###BLOB 属性と連携して使用できる種類

 **Blob** 属性は、次の種類で使用できます。

* **ストリーム** (読み取りまたは書き込み、FileAccess コンス トラクターのパラメーターを使用して指定)
* **TextReader**
* **TextWriter**
* **文字列** (読み取り)
* **文字列をすべて** (書き込みは、関数が戻るとき、文字列パラメーターが null 以外の場合にのみ blob を作成)
* POCO (読み取り)
* out POCO (書き込みは常に、BLOB を作成し、関数が返されたときに、POCO のパラメーターが null の場合は、null オブジェクトとして作成します)
* **CloudBlobStream** (書き込み)
* **ICloudBlob** (読み取りまたは書き込み)
* **CloudBlockBlob** (読み取りまたは書き込み)
* **CloudPageBlob** (読み取りまたは書き込み)

##有害メッセージの処理方法

関数の失敗を引き起こす内容を含むメッセージと呼ばれる *有害メッセージ*します。 関数が失敗してもキュー メッセージは削除されず、最終的には回収されて、このサイクルを繰り返します。 SDK では一定数繰り返し送信されると自動的にそのサイクルを中断します。また手動でも処理できます。

### 有害メッセージの自動処理

SDKでは、キュー メッセージを処理する関数を最大 5 回呼び出します。 5 回目の実行に失敗した場合、メッセージは有害キューに移動されます。 [最大再試行回数は構成可能な](#how-to-set-configuration-options)です。

有害キューの名前は *{originalqueuename}*-有害です。 メッセージのログを取得するか、手動での対処が必要であるという通知を送信することにより有害キューからのメッセージを処理する関数が記述できます。

次の例では、 **CopyBlob** 関数は、キュー メッセージに存在しない blob の名前が含まれている場合は失敗します。 その場合、メッセージは copyblobqueue キューから copyblobqueue-poison キューへと移動されます。  **ProcessPoisonMessage** 有害なメッセージを記録します。

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

        public static void ProcessPoisonMessage(
            [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
        {
            logger.WriteLine("Failed to copy blob, name=" + blobName);
        }

次の画像は、これらの関数が有害メッセージを処理したときのコンソール出力を表しています。

![有害メッセージ処理のためのコンソール出力](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### 有害メッセージの手動処理

追加することで、処理のメッセージが取得された回数を取得できます、 **int** という名前のパラメーター **dequeueCount** 関数にします。 関数コードの dequeue 回数を確認し、特定の閾値を超えたときにメッセージを処理する、独自の有害メッセージの処理を実行できます。具体例を次に示します。

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed to copy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## 構成オプションの設定方法

使用することができます、 **JobHostConfiguration** 次の構成オプションを設定する型。

* コード内で SDK の接続文字列を設定する。
* 構成 **QueueTrigger** dequeue 回数の最大値などの設定です。
* 構成からキューの名前を取得する。

###コード内で SDK の接続文字列を設定する

コード内で SDK の接続文字列を設定することにより、次の例に示すように、構成ファイルまたは環境変数に独自の接続文字列の名前を使用できます。

        static void Main(string[] args)
        {
            var _storageConn = ConfigurationManager
                .ConnectionStrings["MyStorageConnection"].ConnectionString;

            var _dashboardConn = ConfigurationManager
                .ConnectionStrings["MyDashboardConnection"].ConnectionString;

            var _serviceBusConn = ConfigurationManager
                .ConnectionStrings["MyServiceBusConnection"].ConnectionString;

            JobHostConfiguration config = new JobHostConfiguration();
            config.StorageConnectionString = _storageConn;
            config.DashboardConnectionString = _dashboardConn;
            config.ServiceBusConnectionString = _serviceBusConn;
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### QueueTrigger 設定を構成します。

キュー メッセージの処理に適用される次の設定を構成できます。

- 並列実行のために同時に取得するキュー メッセージの最大数 (既定値は 16)。
- キュー メッセージが有害キューに送られるまでの最大再試行回数 (既定値は 5)。
- キューが空の場合に再度ポーリングを実行するまでの最大待機時間 (既定値は 1 分)。

次の例は、これらの設定の構成方法を示しています。

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### コードの Web ジョブ SDK コンストラクター パラメーター値の設定

キュー名、BLOB 名、コンテナー、テーブル名をハード コーディングではなく、コードに指定する場合もあります。 キュー名を指定するたとえば、 **QueueTrigger** 構成ファイルまたは環境変数です。

それにを渡すことによって、 **NameResolver** オブジェクトを **JobHostConfiguration** 型です。 Web ジョブ SDK 属性コンス トラクターのパラメーターにパーセント (%) 記号で囲まれた特殊なプレース ホルダーを追加し、 **NameResolver** コードは、これらのプレース ホルダーの代わりに使用される実際の値を指定します。

たとえば、テスト環境で「logqueuetest」という名前のキューを、実行環境で「logqueueprod」という名前のキューを使用したいとします。 内のエントリの名前を指定するハード コーディングされたキュー名の代わりに、 **appSettings** 実際のキュー名が保持されるコレクション。 場合、 **appSettings** キーが logqueue である、関数の例を次のようになります。

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

 **NameResolver** クラスからキューの名前を取得できます **appSettings** 次の例で示すようにします。

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

渡す、 **NameResolver** でクラスを **JobHost** オブジェクトの次の例で示すようにします。

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

**注:** キュー、テーブル、および blob の名前が解決された関数が呼び出されるたびに、アプリケーションの起動時にのみ、blob コンテナーの名前は解決されます。 ジョブの実行中には、BLOB コンテナーの名前を変更することはできません。

## 関数を手動でトリガーする方法

関数を手動でトリガーを使用して、 **を呼び出す** または **CallAsync** メソッドを **JobHost** オブジェクトおよび **NoAutomaticTrigger** 関数の属性の次の例で示すようにします。

        public class Program
        {
            static void Main(string[] args)
            {
                JobHost host = new JobHost();
                host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
            }

            [NoAutomaticTrigger]
            public static void CreateQueueMessage(
                TextWriter logger,
                string value,
                [Queue("outputqueue")] out string message)
            {
                message = value;
                logger.WriteLine("Creating queue message: ", message);
            }
        }

## ログを書き込む方法

ダッシュボードは次の 2 つの場所でログを表示します。Web ジョブのページと特定の Web ジョブの呼び出しのページです。

![Web ジョブ ページのログ](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![関数の呼び出しページのログ](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

関数で呼び出すことまたはコンソール メソッドの出力、 **Main()** メソッドは、特定のメソッド呼び出しのページではなく、WebJob のダッシュ ボード] ページが表示されます。 メソッド シグネチャのパラメーターから取得した TextWriter オブジェクトの出力は、メソッド呼び出しのダッシュボード ページに表示されます。

コンソールはシングル スレッドで、多くのジョブ関数が同時に実行されるため、コンソールの出力を特定のメソッド呼び出しにリンクすることはできません。 その理由は、SDK には、各関数の呼び出しに独自のログ書き込みオブジェクトが用意されています。

書き込む [アプリケーション トレース ログ](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), を使用して **Console.Out** (INFO としてマークされたログが作成されます) と **Console.Error** (ERROR としてマークされたログが作成されます)。 使用する方法、 [Trace または TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), 詳細、警告を提供する、Info と Error に加えレベルを"重大"です。 ログをトレースするアプリケーションは、Azure Web アプリの構成方法によって、Web アプリ ログ ファイル、Azure テーブル、Azure BLOB に表示されます。 すべてのコンソール出力と同様に、最新の 100 のアプリケーションのログは関数呼び出しのページではなく、Web ジョブのダッシュボード ページに表示されます。

プログラムがローカルまたはその他の環境で実行されているのではなく、Azure Web ジョブで実行されている場合は、コンソール出力はダッシュボードにのみ表示されます。

ログを無効にすることができます [Dashboard の接続文字列を null に設定する](#how-to-set-configuration-options)です。

次の例では、ログを書き込むいくつかの方法を示しています。

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

Web ジョブ SDK ダッシュ ボードからの出力で、 **TextWriter** オブジェクトの特定のページに移動すると表示されますが関数呼び出しと、をクリックして **出力の切り替え**:

![関数呼び出しのリンクをクリックします](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![関数の呼び出しページのログ](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

Web ジョブ SDK ダッシュ ボードで、コンソールの最新 100 件の出力表示を (関数の呼び出し) 的ではなく web ジョブのページに移動] をクリックすると **出力の切り替え**します。

![[出力切り替え] をクリックします](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

継続的な web ジョブでアプリケーション ログに表示/データ/ジョブ/連続/*{webjobname}*/job_log.txt web アプリのファイル システムにします。

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

Azure BLOB では、アプリケーション ログは次のようになります。
        2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!,
        2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello world!,
        2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,

Azure テーブルで、 **Console.Out** と **Console.Error** ログは次のようになります。

![テーブル内の INFO ログ](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![テーブル内の ERROR ログ](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

##次のステップ

この記事では、Azure キューを操作するための一般的なシナリオの処理方法を示すコードのサンプルを提供しました。 Azure web ジョブおよび web ジョブ SDK を使用する方法の詳細については、次を参照してください。 [Azure WebJobs の推奨リソース](http://go.microsoft.com/fwlink/?linkid=390226)します。

