<properties 
    pageTitle="Web ジョブ SDK を使用して Azure キュー ストレージを操作する方法" 
    description="Web ジョブ SDK を使用して Azure キュー ストレージを操作する方法について説明します。キューの作成と削除のほか、キュー メッセージの挿入、ピーク、取得、削除を行います。" 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="12/14/2015" 
    ms.author="tdykstra"/>


# Web ジョブ SDK を使用して Azure キュー ストレージを操作する方法

## 概要

このガイドでは、Azure キュー ストレージ サービスを使用して Azure Web ジョブ SDK バージョン 1.x を操作する方法について説明します。

このガイドがわかっていると想定しています。 [の Visual Studio で web ジョブ プロジェクトを作成する方法は、ストレージ アカウントにそのポイントを文字列](websites-dotnet-webjobs-sdk-get-started.md#configure-storage) または [複数のストレージ アカウント](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs)します。

ほとんどのコード スニペットは関数を作成するコードではなくのみを示して、 `JobHost` この例のようにオブジェクト。

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

このガイドには、次のトピックが含まれています。

-   [キュー メッセージが受信したときに関数をトリガーする方法](#trigger)
    - 文字列のキュー メッセージ
    - POCO キュー メッセージ
    - Async 関数
    - QueueTrigger 属性が連携する種類
    - ポーリング アルゴリズム
    - 複数のインスタンス
    - 並列実行
    - キューまたはキュー メッセージ メタデータの取得
    - グレースフル シャットダウン
-   [キュー メッセージの処理中にキュー メッセージを作成する方法](#createqueue)
    - 文字列のキュー メッセージ
    - POCO キュー メッセージ
    - (非同期関数での) 複数のメッセージの作成
    - キューの属性が連携する種類
    - 関数本体での Web ジョブ SDK 属性の使用
-   [キュー メッセージの処理中に blob を読み書きする方法](#blobs)
    - 文字列のキュー メッセージ
    - POCO キュー メッセージ
    - BLOB の属性が連携する種類
-   [有害なメッセージを処理する方法](#poison)
    - 有害メッセージの自動処理
    - 有害メッセージの手動処理
-   [構成オプションを設定する方法](#config)
    - コード内で SDK の接続文字列を設定する
    - QueueTrigger 設定の構成
    - コードの Web ジョブ SDK コンストラクター パラメーター値の設定
-   [関数を手動でトリガーする方法](#manual)
-   [ログを書き込む方法](#logs)
-   [エラーを処理し、タイムアウトを構成する方法](#errors)
-   [次のステップ](#nextsteps)

## <a id="trigger"></a> キュー メッセージが受信したときに関数をトリガーする方法

キュー メッセージを受信したときに WebJobs SDK が呼び出す関数を記述するには、`QueueTrigger` 属性を使用します。 属性コンストラクターは、ポーリングのためにキューの名前を指定する文字列パラメーター受け取ります。 こともできます [キューの名前を動的に設定](#config)します。

### 文字列のキュー メッセージ

次の例では、キューに文字列メッセージが含まれます。キューメッセージの内容が含まれる `logMessage` と呼ばれる文字列パラメーターに `QueueTrigger` が適用されます。 関数 [ダッシュ ボードにログ メッセージを書き込みます](#logs)します。


        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

それに `文字列`, 、バイト配列の場合は、パラメーターは、 `CloudQueueMessage` オブジェクト、または定義した POCO します。

### POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) キュー メッセージ

次の例では、`BlobName` プロパティを持つ `BlobInformation` オブジェクトの JSON がキュー メッセージに含まれています。 SDK は自動的にオブジェクトを逆シリアル化します。

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

SDK を使用して、 [Newtonsoft.Json NuGet パッケージ](http://www.nuget.org/packages/Newtonsoft.Json) およびメッセージを逆シリアル化します。 キュー メッセージを Web ジョブ SDK を使用しないプログラムで作成する場合は、SDK が解析できる POCO キュー メッセージを作成する次の例のようなコードを記述できます。

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### Async 関数

次の async 関数 [ダッシュ ボードにログを書き込みます](#logs)します。

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Async 関数がかかる場合があります、 [キャンセル トークン](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), の blob をコピーする次の例に示すようにします。 (の詳細については、 `queueTrigger` のプレース ホルダーを参照してください、 [Blob](#blobs) セクションです)。

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName, 
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <a id="qtattributetypes"></a> QueueTrigger 属性が連携する種類

次の種類の `QueueTrigger` を使用できます。

* `string`
* JSON としてシリアル化された POCO の型
* `byte[]`
* `CloudQueueMessage`

### <a id="polling"></a> ポーリング アルゴリズム

SDK はランダムな指数バックオフ アルゴリズムを実装することで、ストレージ トランザクション コストにおけるアイドル状態のキューのポーリングの影響を軽減しています。 メッセージが見つかった場合、SDK は 2 秒間待機した後、別のメッセージをチェックします。 メッセージが見つからない場合は約 4 秒間待機してから再試行します。 その後もキュー メッセージの取得失敗が続けば、待機時間は規定値として 1 分間に設定されている最大待機時間に達するまで増え続けます。 [、最大待機時間は構成可能な](#config)します。

### <a id="instances"></a> 複数のインスタンス

Web アプリが複数のインスタンス上で稼働している場合、継続的な Web ジョブは各マシン上で実行され、各マシンがトリガーを待機して関数の実行を試行します。 Web ジョブ SDK キュー トリガーにより、関数がキュー メッセージを複数回処理するのを自動的に回避できます。関数はべき等として記述する必要はありません。 ただし、1 つのインスタンスのことを確認する場合、関数の実行ホスト web アプリケーションの複数のインスタンスがある場合にも使用すると、 `シングルトン` 属性です。

### <a id="parallel"></a> 並列実行

異なるキューをリッスンする複数の関数を使用している場合、複数のメッセージを同時に受信したとき、SDK では並行してそれらを呼び出します。

1 つのキューに対して複数のメッセージが受信される場合も同様に処理されます。 既定では、SDK は一度にキュー メッセージ 16 個のバッチを取得し、それらを並列処理する関数を実行します。 [バッチ サイズは構成可能な](#config)します。 処理中のメッセージの数がバッチ サイズの半分まで減少すると、SDK は別のバッチを取得し、そのメッセージの処理を開始します。 そのため、1 つの関数につき同時に処理されるメッセージの最大数は、バッチ サイズの 1.5 倍です。 この制限は、`QueueTrigger` 属性を持つ各関数に個別に適用されます。

1 つのキューで受信した複数のメッセージを並列に実行したくない場合は、バッチ サイズを 1 に設定します。 関連項目 **キューの処理の詳細に制御** で [Azure web ジョブ SDK 1.1.0 RTM](/blog/azure-webjobs-sdk-1-1-0-rtm/)します。

### <a id="queuemetadata"></a>キューまたはキュー メッセージ メタデータを取得します。

メソッド シグネチャにパラメーターを追加することで、次のメッセージ プロパティを取得できます。

* `DateTimeOffset` expirationTime
* `DateTimeOffset` insertionTime
* `DateTimeOffset` nextVisibleTime
* `string` queueTrigger (メッセージのテキストが含まれます)
* `string` id
* `string` popReceipt
* `int` dequeueCount

Azure ストレージ API を直接扱う場合は、`CloudStorageAccount` パラメーターも追加できます。

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

### <a id="graceful"></a>正常なシャット ダウン

連続した Web ジョブで実行されている関数は、Web ジョブが中断しそうな場合にオペレーティング システムから通知を受けられる `CancellationToken` パラメーターを受け取ることができます。 この通知を使用すれば、関数が予期せず終了してデータが不整合な状態になることを防止できます。

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

**注:** このダッシュボードではシャットダウンされた関数のステータスや出力が正しく示されていない可能性があります。

詳細については、次を参照してください。 [web ジョブのグレースフル シャット ダウン](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR)します。

## <a id="createqueue"></a> キュー メッセージの処理中にキュー メッセージを作成する方法

新しいキュー メッセージを作成する関数を記述するには、`Queue` 属性を使用します。 ような `QueueTrigger`, のキューの名前を文字列として渡すこともできます [キューの名前を動的に設定](#config)します。

### 文字列のキュー メッセージ

次の非 Async コード サンプルでは、"inputqueue" という名前のキューに受信したキュー メッセージと同じ内容で、"outputqueue" という名前のキューにキュー メッセージを新しく作成します(関数を使用して非同期 `IAsyncCollector < T >` このセクションで後で示すようにします)。


        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) キュー メッセージ

文字列ではなく POCO オブジェクトを含むキュー メッセージを作成するには、出力パラメーターとして POCO 型を `Queue` 属性のコンス トラクターに渡します。

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

SDK はオブジェクトを JSON に自動的にシリアル化します。 オブジェクトが null の場合でもキュー メッセージは常に作成されます。

### (非同期関数での) 複数のメッセージの作成

複数のメッセージを作成するには、出力キューのパラメーターの型を確認 `ICollector < T >` または `IAsyncCollector < T >`, の次の例に示すようにします。

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

`Add` メソッドが呼び出されると、すぐに各キュー メッセージが作成されます。

### キューの属性が連携する種類

次のパラメーターの種類に `Queue` 属性を使用できます。

* `out string` (関数が終了したときに、パラメーター値が null でない場合は、キュー メッセージを作成します)
* `byte[] を` (と同様に動作 `文字列`)
* `out CloudQueueMessage` (`string` と同様に動作)
* `out POCO` (シリアル化可能な型で、関数が終了したときに、パラメーターが null である場合は、null オブジェクトでメッセージを作成します)
* `ICollector`
* `IAsyncCollector`
* `CloudQueue` (Azure Storage API を直接使用して、手動でメッセージを作成します)

### <a id="ibinder"></a>関数の本体で web ジョブ SDK 属性を使用します。

`Queue`、`Blob`、`Table` などの WebJobs SDK 属性を使用する前に関数で何らかの処理を行う必要がある場合は、`IBinder` インターフェイスを使用できます。

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

`Table` と `Blob` 属性で `IBinder` インターフェイスを使用することもできます。

## <a id="blobs"></a> 読み書き可能な blob およびテーブル、キュー メッセージの処理中にする方法

`Blob` と `Table` 属性を持つと、BLOB および Table を読み書きすることができます。 このセクションのサンプルは、BLOB に適用されます。 Blob を作成または更新されたときに、プロセスを開始する方法を示すコード サンプルでは、次を参照してください。 [web ジョブ SDK で Azure blob ストレージを使用する方法](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), 、テーブルを読み書きするコード サンプルでは、「と [web ジョブ SDK で Azure テーブル ストレージを使用する方法](websites-dotnet-webjobs-sdk-storage-tables-how-to.md)します。

### BLOB の操作を開始する文字列キュー メッセージ

文字列を含むキュー メッセージでは、`queueTrigger` は、メッセージの内容を含む `Blob` 属性の `blobPath` パラメーターで使用できるプレースホルダーです。

次の例では、BLOB の読み取りと書き込みに `Stream` オブジェクトを使用しています。 キュー メッセージは、textblobs コンテナーにある BLOB の名前です。 名前に "-new" を加えた BLOB のコピーが同じコンテナー内に作成されます。

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName, 
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

`Blob` 属性コンストラクターが、コンテナーと BLOB 名を指定する  `blobPath` パラメーターを受け取ります。 このプレース ホルダーの詳細については、次を参照してください [web ジョブ SDK で Azure blob ストレージを使用する方法](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),、。

属性が  `Stream` オブジェクトを修飾するともう 1 つのコンス トラクターのパラメーターが `FileAccess` モードを読み取り、書き込み、読み取り/書き込みとして指定します。

次の例では、`CloudBlockBlob` オブジェクトを使用して BLOB を削除します。 キュー メッセージは、BLOB の名前です。

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a id="pocoblobs"></a> POCO [(Plain Old CLR Object](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) キュー メッセージ

キュー メッセージ内に JSON として格納される POCO については、プレースホルダーを使用して、`Queue` 属性の `blobPath` パラメーター内でオブジェクトのプロパティを指定できます。 使用することも [キュー メタデータ プロパティ名](#queuemetadata) プレース ホルダーとして。

次の例では、BLOB を別の拡張子を持つ新しい BLOB にコピーします。 キュー メッセージは、 `BlobName` および `BlobNameWithoutExtension` プロパティを含む `BlobInformation` オブジェクトです。 プロパティの名前は、`Blob` 属性の BLOB パスのプレース ホルダーとして使用されます

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

Blob をオブジェクトにバインドする前に、関数で何らかの処理を行う必要がある場合は、関数の本体で属性を使用することができます [Queue 属性について前に示したよう](#ibinder)します。

### <a id="blobattributetypes"></a> 型を持つ Blob 属性を使用することができます。

`Blob` 属性は、次の種類で使用できます。

* `Stream` (読み取りまたは書き込み、FileAccess コンス トラクターのパラメーターを使用して指定)
* `TextReader`
* `TextWriter`
* `string` (読み取り)
* `out string` (書き込みは、関数を返されたときに、文字列パラメーターが null 以外の場合にのみ BLOB を作成します)
* POCO (読み取り)
* out POCO (書き込みは常に、BLOB を作成し、関数が返されたときに、POCO のパラメーターが null の場合は、null オブジェクトとして作成します)
* `CloudBlobStream` (書き込み)
* `ICloudBlob` (読み取りまたは書き込み)
* `CloudBlockBlob` (読み取りまたは書き込みe)
* `CloudPageBlob` (読み取りまたは書き込み)

## <a id="poison"></a> 有害なメッセージを処理する方法

関数の失敗を引き起こす内容を含むメッセージは*有害メッセージ*と呼ばれます。 関数が失敗してもキュー メッセージは削除されず、最終的には回収されて、このサイクルを繰り返します。 SDK では一定数繰り返し送信されると自動的にそのサイクルを中断します。また手動でも処理できます。

### 有害メッセージの自動処理

SDKでは、キュー メッセージを処理する関数を最大 5 回呼び出します。 5 回目の実行に失敗した場合、メッセージは有害キューに移動されます。 [最大再試行回数は構成可能な](#config)します。

有害キューには *{originalqueuename}*-poison という名前が付けられます。 メッセージのログを取得するか、手動での対処が必要であるという通知を送信することにより有害キューからのメッセージを処理する関数が記述できます。

次の例では、キュー メッセージが存在しない BLOB の名前を含んでいると、`CopyBlob` 関数が失敗します。 その場合、メッセージは copyblobqueue キューから copyblobqueue-poison キューへと移動されます。 その後、`ProcessPoisonMessage` が有害メッセージを記録されます。

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

![有害メッセージ処理のためのコンソール出力](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### 有害メッセージの手動処理

`dequeueCount` という名前の `int` パラメーターを関数に加えることで、処理のためにメッセージが取得された回数が得られます。 関数コードの dequeue 回数を確認し、特定の閾値を超えたときにメッセージを処理する、独自の有害メッセージの処理を実行できます。具体例を次に示します。

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

## <a id="config"></a> 構成オプションを設定する方法

`JobHostConfiguration` 型を使用して次の構成オプションを設定できます。

* コード内で SDK の接続文字列を設定する。
* 最大デキュー回数などの `QueueTrigger` 設定を構成する。
* 構成からキューの名前を取得する。

### <a id="setconnstr"></a>コードで SDK の接続文字列を設定します。

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

### <a id="configqueue"></a>QueueTrigger 設定を構成します。

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

### <a id="setnamesincode"></a>Web ジョブ SDK のコンス トラクターのパラメーター値設定コード

キュー名、BLOB 名、コンテナー、テーブル名をハード コーディングではなく、コードに指定する場合もあります。 たとえば、`QueueTrigger` のキュー名を構成ファイルか環境変数に指定します。

`NameResolver` オブジェクトを `JobHostConfiguration` 型に渡して実行します。 WebJobs SDK 属性コンストラクターのパラメーターにパーセント (%) 記号で囲まれた特殊なプレースホルダーを追加し、`NameResolver` コードでこれらのプレースホルダーの代わりに使用する実際の値を指定します。

たとえば、テスト環境で「logqueuetest」という名前のキューを、実行環境で「logqueueprod」という名前のキューを使用したいとします。 ハードコーディングされたキューの名前ではなく、実際のキューの名前を持つであろう、`appSettings` コレクション内のエントリの名前を指定する必要があります。 `appSettings` キーが logqueue の場合、関数は次の例のようになります。

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

`NameResolver` クラスは、次の例に示すように `appSettings` からキューの名前を取得できます。

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

次の例のように、`NameResolver` クラスを `JobHost` オブジェクトに渡します。

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

**注:** 関数が呼び出されるたびに、キュー、テーブル、BLOB の名前は解決されますが、BLOB コンテナーの名前はアプリケーションの起動時にのみ解決されます。 ジョブの実行中には、BLOB コンテナーの名前を変更することはできません。

## <a id="manual"></a>関数を手動でトリガーする方法

関数を手動でトリガーするには、次の例に示すように `JobHost` オブジェクトで `Call` または `CallAsync` メソッドを使用し、関数では `NoAutomaticTrigger` 属性を使用します。

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

## <a id="logs"></a>ログを書き込む方法

ダッシュボードは次の 2 つの場所でログを表示します。Web ジョブのページと特定の Web ジョブの呼び出しのページです。

![Web ジョブ ページのログ](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![関数の呼び出しページのログ](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

関数または `Main()` メソッドに呼び出すコンソール メソッドの出力は、特定の メソッド呼び出しのページではなく、WebJob のダッシュボード ページに表示されます。 メソッド シグネチャのパラメーターから取得した TextWriter オブジェクトの出力は、メソッド呼び出しのダッシュボード ページに表示されます。

コンソールはシングル スレッドで、多くのジョブ関数が同時に実行されるため、コンソールの出力を特定のメソッド呼び出しにリンクすることはできません。 その理由は、SDK には、各関数の呼び出しに独自のログ書き込みオブジェクトが用意されています。

書き込む [アプリケーション トレース ログ](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), を使用して `Console.Out` (INFO としてマークされたログが作成されます) と `Console.Error` (ERROR としてマークされたログが作成されます)。 使用する方法、 [Trace または TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), 詳細、警告を提供する、Info と Error に加えレベルを"重大"です。 ログをトレースするアプリケーションは、Azure Web アプリの構成方法によって、Web アプリ ログ ファイル、Azure テーブル、Azure BLOB に表示されます。 すべてのコンソール出力と同様に、最新の 100 のアプリケーションのログは関数呼び出しのページではなく、Web ジョブのダッシュボード ページに表示されます。

プログラムがローカルまたはその他の環境で実行されているのではなく、Azure Web ジョブで実行されている場合は、コンソール出力はダッシュボードにのみ表示されます。

高スループットのシナリオ用にダッシュボード ログを無効にします。 既定では、SDK はストレージにログを書き込みます。このアクティビティにより、多数のメッセージの処理時にパフォーマンスが低下する場合があります。 ログを無効にするには、ダッシュボード接続文字列を、次の例に示すように null に設定します。

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = “”;        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

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

Web ジョブ SDK ダッシュボードで特定の関数呼び出しのページに移動し、**[出力切り替え]** をクリックすると、`TextWriter` オブジェクトからの出力が表示されます。

![関数呼び出しのリンクをクリックします](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![関数の呼び出しページのログ](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

Web ジョブのページ (特定の関数呼び出しのページではなく) に移動して、**[出力切り替え]** をクリックすると、WebJobs SDK のダッシュボードでアプリケーション ログの最新 100 件が表示されます。

![[出力切り替え] をクリックします](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

継続的な web ジョブでアプリケーション ログに表示/データ/ジョブ/連続/*{webjobname}*/job_log.txt web アプリのファイル システムにします。

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

Azure BLOB では、アプリケーション ログは次のようになります。
        2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!,
        2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello world!,
        2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,

Azure テーブルでは `Console.Out` および `Console.Error` ログが次のように表示されます。

![テーブル内の INFO ログ](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![テーブル内の ERROR ログ](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

Logger を独自に接続する場合は、「 [この例](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs)します。

## <a id="errors"></a>エラーを処理し、タイムアウトを構成する方法

Web ジョブ SDK にも含まれています、 [タイムアウト](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) 関数を取り消す場合に使用できる属性が指定された時間内を完了します。 指定した期間内にエラーが多すぎるが発生したときにアラートを生成する場合は、使用して、 `ErrorTrigger` 属性です。 ここでは、 [ErrorTrigger 例](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring)します。

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    To = "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors to the Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

また、アプリ設定または環境変数名である構成スイッチを使用して、トリガーできるかどうかを制御する関数を動的に無効または有効にすることもできます。 サンプル コードについては、 `を無効にする` 属性 [web ジョブ SDK のサンプル リポジトリ](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs)します。

## <a id="nextsteps"></a> 次のステップ

このガイドでは、Azure キューを操作するための一般的なシナリオの処理方法を示すコードのサンプルを提供しました。 Azure web ジョブおよび web ジョブ SDK を使用する方法の詳細については、次を参照してください。 [Azure WebJobs の推奨リソース](http://go.microsoft.com/fwlink/?linkid=390226)します。






