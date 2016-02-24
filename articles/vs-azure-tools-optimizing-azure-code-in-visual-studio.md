<properties
   pageTitle="Visual Studio での Azure コードの最適化| Microsoft Azure"
   description="Visual Studio の Azure コード最適化ツールにより、コードの堅牢性とパフォーマンスを向上させる方法について説明します。"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="09/08/2015"
   ms.author="tarcher" />

# Azure コードの最適化

Microsoft Azure を使用するアプリケーションをプログラミングする場合、クラウド環境でのアプリケーションのスケーラビリティ、動作、パフォーマンスに関する問題の回避に役立つコーディングの手法がいくつかあります。 Microsoft では、このような一般的に発生する問題の一部を認識および特定し、その解決を支援する Azure コード分析ツールを提供しています。 このツールは、NuGet を使用して Visual Studio でダウンロードできます。

## Azure コード分析規則

Azure コード分析ツールでは、パフォーマンスに影響する既知の問題を検出したときに、以下の規則を使用して Azure コードに自動的にフラグを設定します。 検出された問題は警告またはコンパイラ エラーとして表示されます。 多くの場合、警告またはエラーを解決するためのコード修正や提案事項は電球アイコンで示されます。

## 既定 (インプロセス) のセッション状態モードを使用しない

### ID

AP0000

### 説明

クラウド アプリケーションの既定 (インプロセス) のセッション状態モードを使用すると、セッション状態が失われる可能性があります。

アイデアやでフィードバックを共有してください [Azure コード分析のフィードバック](http://go.microsoft.com/fwlink/?LinkId=403771)します。

### 理由

既定では、web.config ファイルで指定されているセッション状態モードはインプロセスです。 また、構成ファイルに指定されたエントリがない場合、セッション状態モードは既定でインプロセスに設定されます。 インプロセス モードでは、Web サーバー上のメモリにセッション状態が格納されます。 インスタンスが再起動されたときや、負荷分散またはフェールオーバーのサポートに新しいインスタンスが使用されたときに、Web サーバー上のメモリに格納されているセッション状態は保存されません。 この場合、クラウドでアプリケーションのスケーラビリティを確保できなくなります。

ASP.NET セッション状態では、セッション状態データのさまざまなストレージ オプション (InProc、StateServer、SQLServer、Custom、Off) がサポートされます。 など、外部セッション状態ストアでデータをホストするカスタム モードを使用することをお勧め [Redis の Azure セッション状態プロバイダー](http://go.microsoft.com/fwlink/?LinkId=401521)します。

### 解決策

推奨されるソリューションの 1 つとして、Managed Cache Service でセッション状態を保存します。 使用する方法について説明 [Redis の Azure セッション状態プロバイダー](http://go.microsoft.com/fwlink/?LinkId=401521) セッションの状態を保存します。 また、クラウドでアプリケーションのスケーラビリティを確保するために、他の場所にセッション状態を保存することもできます。 詳細については、代わりのソリューションをお読みください [セッション状態モード](https://msdn.microsoft.com/library/ms178586)します。

## Run メソッドを非同期にしない

### ID

AP1000

### 説明

非同期のメソッドを作成する (よう [await](https://msdn.microsoft.com/library/hh156528.aspx)) の外部、 [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) メソッドから非同期メソッドを呼び出すと [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)します。 宣言、 [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) メソッドを async により再起動ループに入るには、ワーカー ロールです。

アイデアやでフィードバックを共有してください [Azure コード分析のフィードバック](http://go.microsoft.com/fwlink/?LinkId=403771)します。

### 理由

内部で非同期メソッドを呼び出して、 [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) メソッドにより、クラウド サービスのランタイム ワーカー ロールを再利用します。 ワーカー ロールの開始時にすべてのプログラムの実行内で行われて、 [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) メソッドです。 終了、 [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) メソッドは、worker ロールを再起動します。 worker ロールのランタイムは、非同期メソッドを検出すると、非同期メソッドの後のすべての操作をディスパッチしてから制御を戻します。 これにより、ワーカー ロールを終了する、 [[[[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) メソッドと再起動します。 実行の次の反復では、worker ロールが非同期メソッドを再度検出して再起動することで、worker ロールももう一度再利用されます。

### 解決策

以外のすべての非同期操作を置き、 [Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) メソッドです。 内部からリファクターされた非同期メソッドを呼び出して、 [[Run()](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx)](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) RunAsync () .wait などのメソッドです。 Azure コード分析ツールにより、この問題を解決できます。

次のコード スニペットは、この問題のコード修正を示しています。

```
public override void Run()
{
    RunAsync().Wait();
}

public async Task RunAsync()
{
    //Asynchronous operations code logic
    // This is a sample worker implementation. Replace with your logic.

    Trace.TraceInformation("WorkerRole1 entry point called");

    HttpClient client = new HttpClient();

    Task<string> urlString = client.GetStringAsync("http://msdn.microsoft.com");

    while (true)
    {
        Thread.Sleep(10000);
        Trace.TraceInformation("Working");

        string stream = await urlString;
    }

}
```

## Service Bus の Shared Access Signature 認証を使用する

### ID

AP2000

### 説明

認証に Shared Access Signature (SAS) を使用します。 Service Bus の認証に、Access Control Service (ACS) は使用されなくなります。

アイデアやでフィードバックを共有してください [Azure コード分析のフィードバック](http://go.microsoft.com/fwlink/?LinkId=403771)します。

### 理由

セキュリティを強化するために、Azure Active Directory では、ACS 認証に代わって SAS 認証が使用されるようになります。 参照してください [Azure Active Directory は ACS の未来](http://blogs.technet.com/b/ad/archive/2013/06/22/azure-active-directory-is-the-future-of-acs.aspx) については、移行計画にします。

### 解決策

アプリで SAS 認証を使用します。 次の例は、既存の SAS トークンを使用して Service Bus の名前空間またはエンティティにアクセスする方法を示しています。

```
MessagingFactory listenMF = MessagingFactory.Create(endpoints, new StaticSASTokenProvider(subscriptionToken));
SubscriptionClient sc = listenMF.CreateSubscriptionClient(topicPath, subscriptionName);
BrokeredMessage receivedMessage = sc.Receive();
```

詳細については、次のトピックを参照してください。

- 概要については、次を参照してください [Service Bus での共有アクセス署名認証。](https://msdn.microsoft.com/library/dn170477.aspx)

- [Service Bus での共有アクセス署名認証の使用方法](https://msdn.microsoft.com/library/dn205161.aspx)

- サンプル プロジェクトについては、次を参照してください [Service Bus サブスクリプションで使用する共有アクセス署名 (SAS) 認証。](http://code.msdn.microsoft.com/windowsazure/Using-Shared-Access-e605b37c)

## "受信ループ" を回避するために OnMessage メソッドの使用を検討する

### ID

AP2002

### 説明

「受信ループ」に入らないようにするを呼び出す、 **OnMessage** メソッドは、通話を超えるメッセージを受信するための優れたソリューション、 **受信** メソッドです。 ただし、使用する場合、 **受信** 既定以外のサーバー待機時間を指定するメソッドとするは、サーバー待機時間は 1 分以上になっていることを確認します。

アイデアやでフィードバックを共有してください [Azure コード分析のフィードバック](http://go.microsoft.com/fwlink/?LinkId=403771)します。

### 理由

呼び出すときに **OnMessage**, 、クライアントが常にキューまたはサブスクリプションをポーリングする内部メッセージ ポンプを起動します。 このメッセージ ポンプには、メッセージを受信する呼び出しを発行する無限ループが含まれています。 呼び出しがタイムアウトすると、新しい呼び出しが発行されます。 タイムアウト間隔は、の値によって決まりますが、 [OperationTimeout](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx) のプロパティ、 [MessagingFactory](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagingfactory.aspx)使用されています。

使用する利点 **OnMessage** と比較して **受信** ユーザーが手動でメッセージをポーリング、例外を処理、並列で複数のメッセージ処理およびメッセージの完了を持っていないことができます。

呼び出した場合 **受信** の既定値を使用しなくても、必ず、 *ServerWaitTime* 値は 1 分以上です。 設定 *ServerWaitTime* 1 分以上により、このサーバーは、メッセージが完全に受信される前にタイムアウトします。

### 解決策

推奨される使用法については、以下のコード例をご覧ください。 詳細については、次を参照してください。 [Onmessage メソッド)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.onmessage.aspx)と [Receive メソッド (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.receive.aspx)します。

Azure のメッセージング インフラストラクチャのパフォーマンスを向上させるには、デザイン パターンを参照してください。 [非同期メッセージングの基本](https://msdn.microsoft.com/library/dn589781.aspx)します。

使用する例を次に示します **OnMessage** メッセージを受信します。

```
void ReceiveMessages()
{
    // Initialize message pump options.
    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = true; // Indicates if the message-pump should call complete on messages after the callback has completed processing.
    options.MaxConcurrentCalls = 1; // Indicates the maximum number of concurrent calls to the callback the pump should initiate.
    options.ExceptionReceived += LogErrors; // Enables you to get notified of any errors encountered by the message pump.

    // Start receiving messages.
    QueueClient client = QueueClient.Create("myQueue");
    client.OnMessage((receivedMessage) => // Initiates the message pump and callback is invoked for each message that is recieved, calling close on the client will stop the pump.
    {
        // Process the message.
    }, options);
    Console.WriteLine("Press any key to exit.");
    Console.ReadKey();
```

使用する例を次に示します **受信** 既定のサーバー待機時間。

```
string connectionString =  
CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

QueueClient Client =  
    QueueClient.CreateFromConnectionString(connectionString, "TestQueue");

while (true)  
{   
   BrokeredMessage message = Client.Receive();
   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
```

使用する例を次に示します **受信** 既定以外のサーバー待機時間。

```
while (true)  
{   
   BrokeredMessage message = Client.Receive(new TimeSpan(0,1,0));

   if (message != null)
   {
      try  
      {
         Console.WriteLine("Body: " + message.GetBody<string>());
         Console.WriteLine("MessageID: " + message.MessageId);
         Console.WriteLine("Test Property: " +  
            message.Properties["TestProperty"]);

         // Remove message from queue
         message.Complete();
      }

      catch (Exception)
      {
         // Indicate a problem, unlock message in queue
         message.Abandon();
      }
   }
}
```
## Service Bus の非同期メソッドの使用を検討する

### ID

AP2003

### 説明

仲介型メッセージングでパフォーマンスを向上させるには、Service Bus の非同期メソッドを使用します。

アイデアやでフィードバックを共有してください [Azure コード分析のフィードバック](http://go.microsoft.com/fwlink/?LinkId=403771)します。

### 理由

各呼び出しの実行時にメイン スレッドはブロックされないため、非同期メソッドを使用することで、アプリケーション プログラムの同時実行が可能になります。 Service Bus メッセージング メソッドを使用すると、操作 (送信、受信、削除など) の実行に時間がかかります。 この時間には、要求と応答の待機時間だけでなく、Service Bus サービスによる操作の処理時間も含まれます。 時間あたりの操作数を増やすには、操作を同時に実行する必要があります。 詳細についてを参照してください [パフォーマンスの向上を使用して Service Bus 仲介型メッセージングのベスト プラクティス](https://msdn.microsoft.com/library/azure/hh528527.aspx)します。

### 解決策

参照してください [QueueClient クラス (Microsoft.ServiceBus.Messaging)](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.queueclient.aspx) 推奨される非同期メソッドを使用する方法についてです。

Azure のメッセージング インフラストラクチャのパフォーマンスを向上させるには、デザイン パターンを参照してください。 [非同期メッセージングの基本](https://msdn.microsoft.com/library/dn589781.aspx)します。

## Service Bus のキューとトピックのパーティション分割を検討する

### ID

AP2004

### 説明

Service Bus メッセージングでパフォーマンスを向上させるには、Service Bus のキューとトピックをパーティション分割します。

アイデアやでフィードバックを共有してください [Azure コード分析のフィードバック](http://go.microsoft.com/fwlink/?LinkId=403771)します。

### 理由

Service Bus のキューとトピックをパーティション分割すると、パフォーマンスのスループットとサービスの可用性が向上します。これは、パーティション分割されたキューまたはトピックの全体的なスループットが、1 つのメッセージ ブローカーまたはメッセージング ストアのパフォーマンスによって制限されなくなったためです。 また、メッセージング ストアが一時的に停止しても、パーティション分割されたキューまたはトピックが使用できなくなることはありません。 詳細については、次を参照してください。 [メッセージング エンティティのパーティション分割](https://msdn.microsoft.com/library/azure/dn520246.aspx)します。

### 解決策

次のコード スニペットは、メッセージング エンティティをパーティション分割する方法を示しています。

```
// Create partitioned topic.
NamespaceManager ns = NamespaceManager.CreateFromConnectionString(myConnectionString);
TopicDescription td = new TopicDescription(TopicName);
td.EnablePartitioning = true;
ns.CreateTopic(td);
```

詳細については、次を参照してください [パーティション分割されている Service Bus キューおよびトピック |。Microsoft Azure のブログ](http://azure.microsoft.com/blog/2013/10/29/partitioned-service-bus-queues-and-topics/) とチェック アウト、 [Microsoft Azure Service Bus パーティション分割されたキュー](https://code.msdn.microsoft.com/windowsazure/Service-Bus-Partitioned-7dfd3f1f) サンプルです。

## SharedAccessStartTime を設定しない

### ID

AP3001

### 説明

共有アクセス ポリシーを即座に開始するために、現在の時刻に設定された SharedAccessStartTime を使用しないようにします。 このプロパティを設定する必要があるのは、共有アクセス ポリシーを後で開始する場合だけです。

アイデアやでフィードバックを共有してください [Azure コード分析のフィードバック](http://go.microsoft.com/fwlink/?LinkId=403771)します。

### 理由

クロックの同期によって、データセンター間でわずかな時間差が生じます。 たとえば、論理的に考えると、DateTime.Now などのメソッドを使用して、ストレージの SAS ポリシーの開始時刻を現在の時刻として設定した場合、SAS ポリシーが即座に有効になることになります。 ただし、データセンター間のわずかな時間差によって、時刻が開始時刻よりもわずかに遅れているデータセンターもあれば、進んでいるデータセンターもあるため、問題が発生する場合があります。 結果として、SAS ポリシーの有効期間の設定が短すぎると、短時間で (または即座に) 有効期限が切れる可能性があります。

Azure storage での共有アクセス署名を使用する詳細については、次を参照してください。 [概要テーブル SAS (Shared Access Signature)、キュー SAS および Blob SAS - Microsoft Azure Storage チーム ブログの更新サイトのホーム - MSDN ブログ](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)します。

### 解決策

共有アクセス ポリシーの開始時刻を設定するステートメントを削除します。 Azure コード分析ツールにより、この問題を解決できます。 セキュリティ管理の詳細については、デザイン パターンを参照してください [バレット キー パターン](https://msdn.microsoft.com/library/dn568102.aspx)します。

次のコード スニペットは、この問題のコード修正を示しています。

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

## 共有アクセス ポリシーの有効期限は 5 分より長くする必要がある

### ID

AP3002

### 説明

"時刻のずれ" と呼ばれる状態により、さまざまな場所にあるデータセンター間でクロックの時間差が 5 分になる場合があります。 SAS ポリシー トークンが予定よりも早く期限切れになるのを防ぐには、5 分より長い有効期限を設定します。

アイデアやでフィードバックを共有してください [Azure コード分析のフィードバック](http://go.microsoft.com/fwlink/?LinkId=403771)します。

### 理由

世界中のさまざまな場所にあるデータセンターは、クロック信号によって同期されます。 さまざまな場所へのクロック信号の伝達に時間がかかるため、すべてが同期されていると思われていても、地理的に異なる場所にあるデータセンター間で時間の差異が生じている可能性があります。 この時間差は、共有アクセス ポリシーの開始時刻と有効期限の間隔に影響する場合があります。 したがって、共有アクセス ポリシーが即座に有効になるようにするには、開始時刻を指定しないでください。 また、早期タイムアウトを防ぐために、有効期限が 5 分を超えていることを確認します。

Azure storage での共有アクセス署名の使用に関する詳細については、次を参照してください。 [概要テーブル SAS (Shared Access Signature)、キュー SAS および Blob SAS - Microsoft Azure Storage チーム ブログの更新サイトのホーム - MSDN ブログ](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)します。

### 解決策

セキュリティ管理の詳細については、デザイン パターンを参照してください。 [バレット キー パターン](https://msdn.microsoft.com/library/dn568102.aspx)します。

共有アクセス ポリシーの開始時刻を指定しない場合の例を次に示します。

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
   SharedAccessExpiryTime = DateTime.UtcNow.AddHours(10),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

ポリシーの有効期間が 5 分より長い共有アクセス ポリシーの開始時刻を指定する例を次に示します。

```
// The shared access policy provides  
// read/write access to the container for 10 hours.
blobPermissions.SharedAccessPolicies.Add("mypolicy", new SharedAccessBlobPolicy()
{
   // To ensure SAS is valid immediately, don’t set start time.
   // This way, you can avoid failures caused by small clock differences.
  SharedAccessStartTime = new DateTime(2014,1,20),   
 SharedAccessExpiryTime = new DateTime(2014, 1, 21),
   Permissions = SharedAccessBlobPermissions.Write |
      SharedAccessBlobPermissions.Read
});
```

詳細については、次を参照してください。 [を作成し、共有アクセス署名を使用して](https://msdn.microsoft.com/library/azure/jj721951.aspx)します。

## CloudConfigurationManager を使用する

### ID

AP4000

### 説明

使用して、 [ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) クラス プロジェクトなど、Azure の web サイトと Azure mobile services の実行時の問題は発生しません。 ベスト プラクティスとして、それがクラウドを使用することをお勧め[ConfigurationManager](https://msdn.microsoft.com/library/system.configuration.configurationmanager(v=vs.110).aspx) のすべての Azure クラウド アプリケーションの構成管理の統一方法として。

アイデアやでフィードバックを共有してください [Azure コード分析のフィードバック](http://go.microsoft.com/fwlink/?LinkId=403771)します。

### 理由

CloudConfigurationManager は、アプリケーション環境に適した構成ファイルを読み取ります。

[CloudConfigurationManager](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)

### 解決策

使用するようコードをリファクタリング、 [CloudConfigurationManager クラス](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.cloudconfigurationmanager.aspx)します。 この問題のコード修正は、Azure コード分析ツールによって提供されます。

次のコード スニペットは、この問題のコード修正を示しています。 *Views\\Home\\AllDates.cshtml*

`var settings = ConfigurationManager.AppSettings["mySettings"];`

を以下に置き換えることができます。

`var settings = CloudConfigurationManager.GetSetting("mySettings");`

App.config ファイルまたは Web.config ファイルに構成設定を保存する方法の例を次に示します。 設定は、構成ファイルの appSettings セクションに追加します。 前のコード例の Web.config ファイルを次に示します。

```
<appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="mySettings" value="[put_your_setting_here]"/>
  </appSettings>  
```

## ハードコーディングされた接続文字列を使用しない

### ID

AP4001

### 説明

ハードコーディングされた接続文字列を使用しており、それらを後で更新する必要がある場合、ソース コードを変更し、アプリケーションを再コンパイルする必要があります。 ただし、構成ファイルに接続文字列を保存すると、構成ファイルを更新するだけで、接続文字列を後で変更できます。

アイデアやでフィードバックを共有してください [Azure コード分析のフィードバック](http://go.microsoft.com/fwlink/?LinkId=403771)します。

### 理由

接続文字列をすばやく変更する必要があるときに問題が生じるため、接続文字列のハードコーディングは望ましくありません。 また、プロジェクトをソース管理にチェックインする必要がある場合、ソース コードで文字列が表示される可能性があるため、ハードコーディングされた接続文字列ではセキュリティの脆弱性が生じます。

### 解決策

構成ファイルまたは Azure 環境に接続文字列を保存します。

- スタンドアロン アプリケーションでは、app.config を使用して接続文字列の設定を保存します。

- IIS がホストする Web アプリケーションでは、web.config を使用して接続文字列を保存します。

- ASP.NET vNext アプリケーションでは、configuration.json を使用して接続文字列を保存します。

Web.config または app.config などの構成ファイルの使用方法の詳細については、次を参照してください。 [ASP.NET Web の構成ガイドライン](https://msdn.microsoft.com/library/vstudio/ff400235(v=vs.100).aspx)します。 Azure の環境変数の機能については、次を参照してください。 [Azure の Web サイト: アプリケーション文字列と接続文字列は機能](http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)です。 ソース管理への接続文字列を格納する方法の詳細については、次を参照してください。 [のソース コード リポジトリに格納されているファイルの接続文字列などの機密情報を含めるのは避ける](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)します。

## 診断構成ファイルを使用する

### ID

AP5000

### 説明

Microsoft.WindowsAzure.Diagnostics プログラミング API を使用するなどして、コードで診断設定を構成するのではなく、diagnostics.wadcfg ファイル  (Azure SDK 2.5 を使用する場合は diagnostics.wadcfgx) で診断設定を構成します。 これにより、コードを再コンパイルしなくても診断設定を変更できます。

アイデアやでフィードバックを共有してください [Azure コード分析のフィードバック](http://go.microsoft.com/fwlink/?LinkId=403771)します。

### 理由

Azure SDK 2.5 (Azure 診断 1.3 を使用) より前では、Azure 診断 (WAD) を複数の方法 (ストレージの構成 BLOB への追加、命令型コード、宣言型の構成、または既定の構成の使用) で構成できました。 ただし、推奨される診断構成方法は、アプリケーション プロジェクトで XML 構成ファイル (diagnostics.wadcfg、または SDK 2.5 以降では diagnositcs.wadcfgx) を使用する方法です。 この方法では、diagnostics.wadcfg ファイルで構成をすべて定義し、このファイルを自由に更新して再デプロイできます。 混在させると、diagnostics.wadcfg 構成ファイルのプログラムを使用して設定の構成の方法で、 [DiagnosticMonitor](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.diagnosticmonitor.aspx)または [RoleInstanceDiagnosticManager](https://msdn.microsoft.com/library/microsoft.windowsazure.diagnostics.management.roleinstancediagnosticmanager.aspx)クラスは、混乱を招くことができます。 参照してください [Azure 診断構成の変更の初期化または](https://msdn.microsoft.com/library/azure/hh411537.aspx) の詳細。

WAD 1.3 (Azure SDK 2.5 に付属) 以降では、コードを使用して診断を構成することはできなくなりました。 そのため、構成を提供できるのは診断拡張機能の適用時または更新時だけとなります。

### 解決策

診断構成デザイナーを使用して、診断設定を診断構成ファイル (diagnositcs.wadcfg、または SDK 2.5 以降では diagnositcs.wadcfgx) に移動します。 インストールすることは推奨も [Azure SDK 2.5](http://go.microsoft.com/fwlink/?LinkId=513188) 、最新の診断機能を使用します。

1. 構成するロールのショートカット メニューで [プロパティ] をクリックし、[構成] タブをクリックします。

1.  **診断** セクションで、ことを確認して、 **を有効にする診断** ] チェック ボックスをオンします。

1. 選択、 **構成** ] ボタンをクリックします。

  ![[診断の有効化] オプションへのアクセス](./media/vs-azure-tools-optimizing-azure-code-in-visual-studio/IC796660.png)

  参照してください [Azure クラウド サービスおよび仮想マシンの診断の構成](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) の詳細。


## DbContext オブジェクトを静的として宣言しない

### ID

AP6000

### 説明

メモリを節約するために、DBContext オブジェクトを静的として宣言しないようにします。

アイデアやでフィードバックを共有してください [Azure コード分析のフィードバック](http://go.microsoft.com/fwlink/?LinkId=403771)します。

### 理由

DBContext オブジェクトでは、各呼び出しのクエリ結果が保持されます。 静的 DBContext オブジェクトは、アプリケーション ドメインがアンロードされるまで破棄されません。 そのため、静的 DBContext オブジェクトが大量のメモリを消費する可能性があります。

### 解決策

DBContext をローカル変数または非静的インスタンス フィールドとして宣言し、これをタスクで使用して、使用後に破棄されるようにします。

次の MVC コントローラー クラスの例は、DBContext オブジェクトの使用方法を示しています。

```
public class BlogsController : Controller
    {
        //BloggingContext is a subclass to DbContext        
        private BloggingContext db = new BloggingContext();
        // GET: Blogs
        public ActionResult Index()
        {
            //business logics…
            return View();
        }
        protected override void Dispose(bool disposing)
        {
            if (disposing)
            {
                db.Dispose();
            }
            base.Dispose(disposing);
        }
    }
```

## 次のステップ

Optimzing と Azure アプリケーションのトラブルシューティングの詳細については、次を参照してください。 [Visual Studio を使用して Azure App Service で web アプリケーションのトラブルシューティング](web-sites-dotnet-troubleshoot-visual-studio.md)します。

