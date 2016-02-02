<properties 
    pageTitle="Azure Web ジョブ SDK とは" 
    description="Azure Web ジョブ SDK の紹介です。この SDK の特徴、一般的な用途、コード サンプルについて説明します。" 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/14/2015" 
    ms.author="tdykstra"/>


# Azure Web ジョブ SDK とは

## <a id="overview"></a>概要

この記事では、Web ジョブ SDK の特徴を紹介すると共に、一般的な用途を確認し、コードでの使い方の概要を示します。

[WebJobs](websites-webjobs-resources.md) 機能での Azure App Service web アプリ、API アプリ、モバイル アプリと同じコンテキストでプログラムまたはスクリプトを実行することができます。 目的、 [web ジョブ SDK](websites-webjobs-resources.md) は、画像処理、キュー処理、rss 情報集約ファイル メンテナンスなど、web ジョブが実行できる一般的なタスク用に記述するコードを簡略化して、電子メールを送信します。 WebJobs SDK には、Azure Storage や Service Bus の操作、タスクのスケジューリング、エラー処理など、一般的な用途に対応した各種の機能が組み込まれています。 さらに、拡張できる設計となっておりは、 [拡張機能のオープン ソース リポジトリ](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview)します。

Web ジョブ SDK には次のコンポーネントが含まれています。

* **NuGet パッケージ**。 Visual Studio コンソール アプリケーション プロジェクトに追加する NuGet パッケージは、Web ジョブ SDK の属性でメソッドを修飾することによって、コードで使用するフレームワークを提供します。

* **ダッシュボード**。 Web ジョブ SDK の一部は、Azure App Service に含まれており、NuGet パッケージを使用するプログラム向けに豊富な監視と診断の機能を提供します。 これらの監視および診断機能を使用するためにコードを記述する必要はありません。

## <a id="scenarios"></a>シナリオ

ここでは、Azure Web ジョブ SDK を使用してより簡単に処理できる標準的なシナリオをいくつか取り上げます。

* 画像処理または CPU 負荷の高い作業。 Web サイトの一般的な機能は、画像やビデオをアップロードする機能です。 アップロード後にコンテンツを操作する必要が生じ、その作業の間ユーザーを待たせたくない場合がよくあります。

* キューの処理。 Web フロントエンドがバックエンド サービスと通信する一般的な方法は、キューを使用することです。 Web サイトは、処理を完了する必要がある場合に、メッセージをキューにプッシュします。 バックエンド サービスは、キューからメッセージをプルし、処理を完了します。 イメージ処理には、キューを使用できます。 たとえば、ユーザーがいくつかのファイルをアップロードした後、それらをバックエンドで取り出して処理できるように、ファイル名をキュー メッセージに格納します。 また、キューを使用してサイトの応答性を改善できます。 たとえば、SQL データベースに直接書き込む代わりに、キューに書き込んで、完了したことをユーザーに通知します。その間、バックエンド サービスでは、待ち時間の長いリレーショナル データベースの作業を処理することができます。 キューのイメージ プロセスを使用した処理の例は、次を参照してください。、 [web ジョブ SDK が入門チュートリアル](websites-dotnet-webjobs-sdk-get-started.md)します。

* RSS 情報集約。 RSS フィードのリストを維持するサイトがある場合は、フィードからのすべての記事をバックグラウンド プロセスにプルすることができます。

* ログ ファイルの集計やクリーンアップなどのファイルのメンテナンス。 分析ジョブを実行するために、いくつかのサイトによって作成されたログ ファイルや別々の期間に作成されたログ ファイルを組み合わせる作業が必要になる場合があります。 また、使用していないログ ファイルをクリーンアップするために毎週実行するタスクをスケジュール設定する場合もあります。

* Azure テーブルへの取り込み。 格納したファイルと BLOB を解析して、データをテーブルに格納したいと思っても、 その取り込み用の関数を作るには、大量のコードの記述が必要になることがあります (場合によっては数百万行)。Web ジョブ SDK を利用すれば、この機能を簡単に実装できます。 また、この SDK なら、テーブルに書き込まれた行の数など、進行状況インジケーターのリアルタイム監視も実現できます。

* など、バック グラウンド スレッドで実行するその他の実行時間の長いタスク [電子メールを送信する](https://github.com/victorhurdugaci/AzureWebJobsSamples/tree/master/SendEmailOnFailure)します。

* 毎晩のバックアップ操作の実行など、スケジュールに従って実行するタスク。

これらのシナリオの多くでは、Web アプリを複数の VM で実行するように拡張することを検討できます (複数の WebJobs を同時に実行するなど)。 一部のシナリオでは、これにより同じデータが複数回処理されることになりますが、WebJobs SDK の組み込みのキュー、BLOB、および Service Bus のトリガーを使用した場合、これは問題にはなりません。 SDK では、関数は各メッセージまたは BLOB に対して 1 回のみ処理されます。

また、Web ジョブ SDK では、一般的なエラー処理シナリオを簡単に処理できます。 関数が失敗したときに通知を送信するようにアラートを設定できます。また、タイムアウトを設定して、指定した時間制限内に関数が完了しない場合に、その関数を自動的にキャンセルすることができます。

## <a id="code"></a> コード サンプル

Azure Storage と組み合わせて動作する標準的なタスクを処理するコードは、シンプルです。 コンソール アプリケーションの `Main` を作成する方法、 `JobHost` 記述したメソッドへの呼び出しを調整するオブジェクト。 Web ジョブ SDK フレームワークは、メソッド内で使用した Web ジョブ SDK 属性に基づいて、そのメソッドを呼び出すタイミングと、使用するパラメーター値を認識します。 SDK には、関数が呼び出される条件を指定する*トリガー*と、メソッドのパラメーターに対して情報を入出力する方法を指定する*バインダー*が用意されています。

たとえば、 [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md) 属性により、キューにメッセージを受信し、メッセージが自動的に逆シリアル化された場合は、メッセージの形式は、バイト配列またはカスタム型の JSON はときに、呼び出される関数。  [BlobTrigger](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) 属性が Azure ストレージ アカウントに新しい blob が作成されるたびに、プロセスをトリガーします。

次に示すのは、キューをポーリングして、受信したキュー メッセージごとに BLOB を作成する単純なプログラムです。

        public static void Main()
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }
    
        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)
        {
            writer.WriteLine(inputText);
        }

`JobHost` オブジェクトは、バック グラウンドの関数セット用のコンテナーです。  `JobHost` オブジェクトは、関数は、それらをトリガーするイベントの監視を監視し、トリガー イベントが発生したときに、その関数を実行します。 呼び出す、 `JobHost` メソッドをコンテナー プロセスを現在のスレッドまたはバック グラウンド スレッドで実行するかどうかを指定します。 この例で、 `RunAndBlock` メソッド上でプロセス実行継続的に、現在のスレッド。

`ProcessQueueMessage` この例では、 `QueueTrigger` 属性の場合は、新規キュー メッセージの受信がその関数をトリガーします。  `JobHost` オブジェクトは、指定されたキュー (このサンプルでは"webjobsqueue") で新規キュー メッセージを監視し、呼び出しが見つかった場合 `ProcessQueueMessage`します。

`QueueTrigger` 属性のバインド、 `inputText` パラメーターをキュー メッセージの値。 および `Blob` 属性のバインド、 `TextWriter` オブジェクトを"containername"という名前のコンテナー内の"blobname"という名前の blob です。

        public static void ProcessQueueMessage([QueueTrigger("webjobsqueue")]] string inputText, 
            [Blob("containername/blobname")]TextWriter writer)

その後、関数はこれらのパラメーターを使用して、キュー メッセージの値を BLOB に書き込みます。

        writer.WriteLine(inputText);

Web ジョブ SDK のトリガー機能とバインダー機能は、記述する必要のあるコードを大幅に簡素化します。 キュー、BLOB、ファイルの処理、またはスケジュールされたタスクの開始に必要な低レベルのコードは、Web ジョブ SDK フレームワークによって実行されます。 たとえば、このフレームワークは、まだ存在していないキューの作成、キューのオープン、キュー メッセージの読み取り、処理の完了時のキュー メッセージの削除、まだ存在しない BLOB コンテナーの作成、BLOB への書き込みなどを処理します。

次のコード例は、1 つの web ジョブでさまざまなトリガーを示しています。 `QueueTrigger`, 、`FileTrigger`, 、`WebHookTrigger`, 、および `ErrorTrigger`します。

```
    public class Functions
    {
        public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        TextWriter log)
        {
            log.WriteLine(message);
        }

        public static void ProcessFileAndUploadToBlob(
            [FileTrigger(@"import\{name}", "*.*", autoDelete: true)] Stream file,
            [Blob(@"processed/{name}", FileAccess.Write)] Stream output,
            string name,
            TextWriter log)
        {
            output = file;
            file.Close();
            log.WriteLine(string.Format("Processed input file '{0}'!", name));
        }

        [Singleton]
        public static void ProcessWebHookA([WebHookTrigger] string body, TextWriter log)
        {
            log.WriteLine(string.Format("WebHookA invoked! Body: {0}", body));
        }

        public static void ProcessGitHubWebHook([WebHookTrigger] string body, TextWriter log)
        {
            dynamic issueEvent = JObject.Parse(body);
            log.WriteLine(string.Format("GitHub WebHook invoked! ('{0}', '{1}')",
                issueEvent.issue.title, issueEvent.action));
        }

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
    }
```

## <a id="schedule"></a> スケジュール設定

`TimerTrigger` 属性では、スケジュールに従って実行する関数をトリガーします。 Web ジョブをスケジュールするには、web ジョブ SDK を使用して web ジョブの Azure またはスケジュールの個々 の関数を使用して全体として `TimerTrigger`します。 コード サンプルを次に示します。

```
public class Functions
{
    public static void ProcessTimer([TimerTrigger("*/15 * * * * *", RunOnStartup = true)]
    TimerInfo info, [Queue("queue")] out string message)
    {
        message = info.FormatNextOccurrences(1);
    }
}
```

サンプル コードの詳細を参照してください。 [TimerSamples.cs](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/ExtensionsSample/Samples/TimerSamples.cs) GitHub.com 上の azure web ジョブ sdk 拡張リポジトリにします。

## 機能拡張

Web ジョブ SDK を使用すると、組み込みの機能に制限されずに、カスタムのトリガーやバインダーを記述できるようになります。 たとえば、キャッシュ イベントや定期的なスケジュール向けのトリガーを記述できます。  [オープン ソース リポジトリ](https://github.com/Azure/azure-webjobs-sdk-extensions) が含まれています、 [web ジョブ SDK の拡張機能の詳細なガイド](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) とするために役立つサンプル コードは、独自のトリガーおよびバインダーを記述を開始します。

## <a id="workerrole"></a>Web ジョブ以外で web ジョブ SDK を使用してください。

Web ジョブ SDK を使用するプログラムは標準的なコンソール アプリケーションであるため、場所を問わず実行できます。Web ジョブとして実行する必要はありません。 ローカルの開発用コンピューターでプログラムをテストし、運用段階では、好みに応じてクラウド サービスの worker ロールまたは Windows サービスで実行することができます。

ただし、ダッシュボードは Azure App Service Web アプリの拡張機能としてのみ使用可能です。 Web ジョブ以外で実行する際にもダッシュボードを使いたい場合は、Web ジョブ SDK ダッシュボードの接続文字列が参照するストレージ アカウントを使用するように Web アプリを構成できます。そうすれば、その Web アプリの Web ジョブ ダッシュボードに、別の場所で実行されているプログラムから関数の実行に関するデータが表示されます。 URL https://を使用して、ダッシュ ボードに接続できます*{webappname}*.scm.azurewebsites.net/azurejobs/#/functions します。 詳細については、次を参照してください。 [web ジョブ SDK を使用したローカル開発用ダッシュ ボードの取得](http://blogs.msdn.com/b/jmstall/archive/2014/01/27/getting-a-dashboard-for-local-development-with-the-webjobs-sdk.aspx), 、ブログの投稿にいない接続文字列名が示されているに注意してください。

## <a id="nostorage"></a>ダッシュ ボードの機能

Web ジョブ SDK には、トリガーやバインダーを使用しない場合でも、次のようないくつかの利点があります。

* ダッシュボードから関数を呼び出すことができる。
* ダッシュボードから関数を再生することができる。
* ダッシュ ボードで、特定の web ジョブ (アプリケーション ログ、Console.Out、Console.Error、トレースなどを使用して記述します。) にリンクされているログを表示することができます。 またはそれらを生成した特定の関数の呼び出しにリンクされている (を使用して書き込まれるログ、 `TextWriter` 、SDK がパラメーターとして関数に渡すオブジェクト)。

詳細については、次を参照してください [関数を手動でトリガーする方法](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#manual) と [ログを書き込む方法](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#logs)。

## <a id="nextsteps"></a>次のステップ

Web ジョブ SDK の詳細については、次を参照してください。 [Azure WebJobs の推奨リソース](http://go.microsoft.com/fwlink/?linkid=390226)します。

Web ジョブ SDK の最新の機能強化については、次を参照してください。、 [リリース ノート](https://github.com/Azure/azure-webjobs-sdk/wiki/Release-Notes)します。






