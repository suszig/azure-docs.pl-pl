<properties 
    pageTitle="Azure App Service API アプリ トリガー" 
    description="この記事では、API アプリにトリガーを実装する方法について説明します。" 
    services="app-service\logic" 
    documentationCenter=".net" 
    authors="guangyang"
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-logic" 
    ms.workload="na" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/15/2015" 
    ms.author="guayan"/>

# Azure App Service API アプリ トリガー

## 概要

この記事では、API アプリ トリガーを実装して、ロジック アプリからそれらを使用する方法について説明します。

初めて使用する場合は、 [API apps](app-service-api-apps-why-best-platform.md) で [Azure App Service](../app-service/app-service-value-prop-what-is.md), 、マルチパートのシリーズを読むことをお勧め [API アプリの作成](app-service-dotnet-create-api-app.md)

すべてのコード スニペットでは、このトピックのコピーからさらに、 [FileWatcher API アプリのサンプル コード](http://go.microsoft.com/fwlink/?LinkId=534802)します。 

次の nuget パッケージをビルドして実行するには、この記事のコードをダウンロードする必要があります注: [http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/)します。

## API アプリ トリガーとは

API アプリでイベントを発生させることは一般的なシナリオなので、API アプリのクライアントはイベントに応答して適切な処理を行うことができます。 このシナリオをサポートする REST API ベースのメカニズムは API アプリ トリガーと呼ばれます。 

たとえばと、クライアント コードを使用して、 [Twitter Connector API アプリ](../app-service-logic/app-service-logic-connector-twitter.md) 、コードは、特定の単語を含む新しいツイートに基づいて処理を実行する必要があるとします。 この場合、このニーズを満たすために、ポーリング トリガーかプッシュ トリガーを設定する場合があります。

## ポーリング トリガーとプッシュ トリガー

現在、2 種類のトリガーがサポートされています。

- ポーリング トリガー - 発生したイベントの通知に対して、クライアントが API アプリをポーリングします。 
- プッシュ トリガー - イベントの発生時に、クライアントは API アプリから通知を受け取ります。 

### ポーリング トリガー

ポーリング トリガーは、標準の REST API として実装され、そのクライアント (ロジック アプリなど) が通知を取得するために API をポーリングすることを想定しています。 クライアントは状態を保持できますが、ポーリング トリガー自体はステートレスです。 

要求パケットと応答パケットに関する次の情報は、ポーリング トリガー コントラクトのいくつかの重要な側面を示しています。

- 要求
    - HTTP メソッド: GET
    - パラメーター
        - triggerState - このオプション パラメーターでは、指定された状態に基づいて通知が返されたかどうかをポーリング トリガーが適切に判断できるように、クライアントが自身の状態を指定できます。
        - API 固有のパラメーター
- Response
    - ステータス コード **200** - 要求が有効であり、トリガーからの通知があります。 通知の内容は応答本文です。 応答内の "Retry-After" ヘッダーは、後続の要求呼び出しで追加の通知データを取得する必要があることを示しています。
    - ステータス コード **202** - 要求有効ですが、トリガーからの新しい通知はありません。
    - ステータス コード **4 xx** -要求が無効です。 クライアントは要求を再試行できません。
    - ステータス コード **5 xx** -内部サーバー エラーまたは一時的な問題要求の結果が発生しました。 クライアントは要求を再試行する必要があります。

次のコード スニペットは、ポーリング トリガーを実装する方法の例を示します。

    // Implement a poll trigger.
    [HttpGet]
    [Route("api/files/poll/TouchedFiles")]
    public HttpResponseMessage TouchedFilesPollTrigger(
        // triggerState is a UTC timestamp
        string triggerState,
        // Additional parameters
        string searchPattern = "*")
    {
        // Check to see whether there is any file touched after the timestamp.
        var lastTriggerTimeUtc = DateTime.Parse(triggerState).ToUniversalTime();
        var touchedFiles = Directory.EnumerateFiles(rootPath, searchPattern, SearchOption.AllDirectories)
            .Select(f => FileInfoWrapper.FromFileInfo(new FileInfo(f)))
            .Where(fi => fi.LastAccessTimeUtc > lastTriggerTimeUtc);

        // If there are files touched after the timestamp, return their information.
        if (touchedFiles != null && touchedFiles.Count() != 0)
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventTriggered(new { files = touchedFiles });
        }
        // If there are no files touched after the timestamp, tell the caller to poll again after 1 mintue.
        else
        {
            // Extension method provided by the AppService service SDK.
            return this.Request.EventWaitPoll(new TimeSpan(0, 1, 0));
        }
    }

このプッシュ トリガーをテストするには、次の手順に従います。

1. API アプリの認証設定をデプロイ **パブリック匿名**します。
2. 呼び出す、 **タッチ** 操作、ファイルにタッチします。 次の図は、Postman を介した要求の例を示します。
   ![Postman を使用したタッチ操作を呼び出し](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
3. ポーリング トリガーを呼び出す、 **triggerState** パラメーターは、手順 2. の前にタイムスタンプに設定します。 次の図は、Postman を介した要求の例を示します。
   ![Postman を使用したポーリング トリガーを呼び出し](./media/app-service-api-dotnet-triggers/callpolltriggerfrompostman.PNG)

### プッシュ トリガー

プッシュ トリガーは、特定のイベントの発生時に通知を受け取るように登録されているクライアントに通知をプッシュする標準の REST API として実装されます。

要求パケットと応答パケットに関する次の情報は、プッシュ トリガー コントラクトのいくつかの重要な側面を示しています。

- 要求
    - HTTP メソッド: PUT
    - パラメーター
        - triggerId: 必須 - プッシュ トリガーの登録を表す不明瞭な文字列 (GUID など)。
        - callbackUrl: 必須 - イベントの発生時に呼び出されるコールバックの URL。 呼び出しは、単純な POST HTTP 呼び出しです。
        - API 固有のパラメーター
- Response
    - ステータス コード **200** -成功したクライアントの登録を要求します。
    - ステータス コード **4 xx** -要求が無効です。 クライアントは要求を再試行できません。
    - ステータス コード **5 xx** -内部サーバー エラーまたは一時的な問題要求の結果が発生しました。 クライアントは要求を再試行する必要があります。
- コールバック
    - HTTP メソッド: POST
    - 要求本文: 通知の内容です。

次のコード スニペットは、プッシュ トリガーを実装する方法の例を示します。

    // Implement a push trigger.
    [HttpPut]
    [Route("api/files/push/TouchedFiles/{triggerId}")]
    public HttpResponseMessage TouchedFilesPushTrigger(
        // triggerId is an opaque string.
        string triggerId,
        // A helper class provided by the AppService service SDK.
        // Here it defines the input of the push trigger is a string and the output to the callback is a FileInfoWrapper object.
        [FromBody]TriggerInput<string, FileInfoWrapper> triggerInput)
    {
        // Register the trigger to some trigger store.
        triggerStore.RegisterTrigger(triggerId, rootPath, triggerInput);

        // Extension method provided by the AppService service SDK indicating the registration is completed.
        return this.Request.PushTriggerRegistered(triggerInput.GetCallback());
    }

    // A simple in-memory trigger store.
    public class InMemoryTriggerStore
    {
        private static InMemoryTriggerStore instance;

        private IDictionary<string, FileSystemWatcher> _store;

        private InMemoryTriggerStore()
        {
            _store = new Dictionary<string, FileSystemWatcher>();
        }

        public static InMemoryTriggerStore Instance
        {
            get
            {
                if (instance == null)
                {
                    instance = new InMemoryTriggerStore();
                }
                return instance;
            }
        }

        // Register a push trigger.
        public void RegisterTrigger(string triggerId, string rootPath,
            TriggerInput<string, FileInfoWrapper> triggerInput)
        {
            // Use FileSystemWatcher to listen to file change event.
            var filter = string.IsNullOrEmpty(triggerInput.inputs) ? "*" : triggerInput.inputs;
            var watcher = new FileSystemWatcher(rootPath, filter);
            watcher.IncludeSubdirectories = true;
            watcher.EnableRaisingEvents = true;
            watcher.NotifyFilter = NotifyFilters.LastAccess;

            // When some file is changed, fire the push trigger.
            watcher.Changed +=
                (sender, e) => watcher_Changed(sender, e,
                    Runtime.FromAppSettings(),
                    triggerInput.GetCallback());

            // Assoicate the FileSystemWatcher object with the triggerId.
            _store[triggerId] = watcher;

        }

        // Fire the assoicated push trigger when some file is changed.
        void watcher_Changed(object sender, FileSystemEventArgs e,
            // AppService runtime object needed to invoke the callback.
            Runtime runtime,
            // The callback to invoke.
            ClientTriggerCallback<FileInfoWrapper> callback)
        {
            // Helper method provided by AppService service SDK to invoke a push trigger callback.
            callback.InvokeAsync(runtime, FileInfoWrapper.FromFileInfo(new FileInfo(e.FullPath)));
        }
    }

このプッシュ トリガーをテストするには、次の手順に従います。

1. API アプリの認証設定をデプロイ **パブリック匿名**します。
2. 参照 [http://requestb.in/](http://requestb.in/) コールバック URL として機能する RequestBin を作成します。
3. GUID として使用してプッシュ トリガーを呼び出す **triggerId** RequestBin url を **callbackUrl**します。
   ![Postman を使用したプッシュ トリガーを呼び出し](./media/app-service-api-dotnet-triggers/callpushtriggerfrompostman.PNG)
4. 呼び出す、 **タッチ** 操作、ファイルにタッチします。 次の図は、Postman を介した要求の例を示します。
   ![Postman を使用したタッチ操作を呼び出し](./media/app-service-api-dotnet-triggers/calltouchfilefrompostman.PNG)
5. プロパティの出力でプッシュ トリガーのコールバックが呼び出されることを確認する RequestBin をチェックします。
   ![Postman を使用したポーリング トリガーの呼び出し](./media/app-service-api-dotnet-triggers/pushtriggercallbackinrequestbin.PNG)

### API 定義でのトリガーの記述

移動し、トリガーを実装して API アプリを Azure に配置した後、 **API の定義** UI では、これは、API アプリの Swagger 2.0 API 定義によって生じますトリガーが自動的に認識されることを Azure プレビュー ポータルのブレードが表示されます。

![[API の定義] ブレード](./media/app-service-api-dotnet-triggers/apidefinitionblade.PNG)

クリックすると、 **Swagger のダウンロード** ] ボタンをクリックし、JSON ファイルを開き、次のような結果が表示されます。

    "/api/files/poll/TouchedFiles": {
      "get": {
        "operationId": "Files_TouchedFilesPollTrigger",
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    },
    "/api/files/push/TouchedFiles/{triggerId}": {
      "put": {
        "operationId": "Files_TouchedFilesPushTrigger",
        ...
        "x-ms-scheduler-trigger": "push"
      }
    }

拡張機能プロパティ **x-ms のスケジューラのトリガー** はトリガーの API の定義に記載されている方法と場合、ゲートウェイ経由で API の定義を要求するときに、API アプリ ゲートウェイによって自動的に追加、次の条件のいずれかに要求します。 (このプロパティは手動で追加することもできます)。

- ポーリング トリガー
    - HTTP メソッドが場合 **取得**します。
    - 場合、 **operationId** プロパティには、文字列が含まれています。 **トリガー**します。
    - 場合、 **パラメーター** プロパティには持つパラメーターが含まれています、 **名前** プロパティに設定 **triggerState**します。
- プッシュ トリガー
    - HTTP メソッドが場合 **配置**します。
    - 場合、 **operationId** プロパティには、文字列が含まれています。 **トリガー**します。
    - 場合、 **パラメーター** プロパティには持つパラメーターが含まれています、 **名前** プロパティに設定 **triggerId**します。

## ロジック アプリでの API アプリ トリガーの使用

### ロジック アプリ デザイナーでの API アプリ トリガーの一覧表示と構成

API アプリと同じリソース グループにロジック アプリを作成すると、それをクリックするだけで、デザイナー キャンバスに追加できます。 次の図では、これを示します。

![ロジック アプリ デザイナーにおけるトリガー](./media/app-service-api-dotnet-triggers/triggersinlogicappdesigner.PNG)

![ロジック アプリ デザイナーでのポーリング トリガーの構成](./media/app-service-api-dotnet-triggers/configurepolltriggerinlogicappdesigner.PNG)

![ロジック アプリ デザイナーでのプッシュ トリガーの構成](./media/app-service-api-dotnet-triggers/configurepushtriggerinlogicappdesigner.PNG)

## ロジック アプリ向けの API アプリ トリガーの最適化

API アプリにトリガーを追加した後は、ロジック アプリ内で API アプリを使用するときのエクスペリエンスを改善するためにできることがいくつかあります。

たとえば、 **triggerState** ポーリング トリガーのパラメーターは、ロジック アプリで次のように設定する必要があります。 この式は、ロジック アプリからのトリガーの最後の呼び出しを評価して、その値を返します。  

    @coalesce(triggers()?.outputs?.body?['triggerState'], '')

注: 前の式で使用される関数の詳細については、ドキュメントを参照して、 [ロジック アプリ ワークフローの定義言語](https://msdn.microsoft.com/library/azure/dn948512.aspx)です。

ロジック アプリ ユーザーは、上記の式を提供する必要があります、 **triggerState** 、トリガーの使用中のパラメーターです。 拡張プロパティを使用してロジック アプリ デザイナーによって事前設定してこの値を設定することは **x ms スケジューラ推奨**します。   **X ms 視界** の値に拡張プロパティを設定することができます *内部* パラメーター自体がデザイナーに表示されないようにします。  次のスニペットは、そのことを示しています。

    "/api/Messages/poll": {
      "get": {
        "operationId": "Messages_NewMessageTrigger",
        "parameters": [
          {
            "name": "triggerState",
            "in": "query",
            "required": true,
            "x-ms-visibility": "internal",
            "x-ms-scheduler-recommendation": "@coalesce(triggers()?.outputs?.body?['triggerState'], '')",
            "type": "string"
          }
        ]
        ...
        "x-ms-scheduler-trigger": "poll"
      }
    }

プッシュ トリガーの場合、 **triggerId** パラメーターは、ロジック アプリを一意に識別する必要があります。 ベスト プラクティスとして、次の式を使用して、このプロパティをワークフローの名前に設定することをお勧めします。

    @workflow().name

使用して、 **x ms スケジューラ推奨** と **x ms 視界** API の定義の拡張プロパティ、API アプリを自動的にそのユーザーに対してこの式を設定するロジック アプリ デザイナーに伝えることができます。

        "parameters":[  
          {  
            "name":"triggerId",
            "in":"path",
            "required":true,
            "x-ms-visibility":"internal",
            "x-ms-scheduler-recommendation":"@workflow().name",
            "type":"string"
          },


### API の定義での拡張機能プロパティの追加

拡張プロパティなどの追加のメタデータ情報は、 **x ms スケジューラ推奨** と **x の ms 表示** -の 2 つの方法のいずれかで API 定義に追加できます。 静的または動的です。

静的なメタデータを直接編集できます、 */metadata/apiDefinition.swagger.json* 、プロジェクト内のファイルし、プロパティを手動で追加します。

動的メタデータを使用する API アプリの場合は、SwaggerConfig.cs ファイルを編集して、これらの式を追加できる操作フィルターを追加できます。

    GlobalConfiguration.Configuration 
        .EnableSwagger(c =>
            {
                ...
                c.OperationFilter<TriggerStateFilter>();
                ...
            }


このクラスを実装して、動的メタデータのシナリオを容易にする方法の例を次に示します。

    // Add extension properties on the triggerState parameter
    public class TriggerStateFilter : IOperationFilter
    {

        public void Apply(Operation operation, SchemaRegistry schemaRegistry, System.Web.Http.Description.ApiDescription apiDescription)
        {
            if (operation.operationId.IndexOf("Trigger", StringComparison.InvariantCultureIgnoreCase) >= 0)
            {
                // this is a possible trigger
                var triggerStateParam = operation.parameters.FirstOrDefault(x => x.name.Equals("triggerState"));
                if (triggerStateParam != null)
                {
                    if (triggerStateParam.vendorExtensions == null)
                    {
                        triggerStateParam.vendorExtensions = new Dictionary<string, object>();
                    }

                    // add 2 vendor extensions
                    // x-ms-visibility: set to 'internal' to signify this is an internal field
                    // x-ms-scheduler-recommendation: set to a value that logic app can use
                    triggerStateParam.vendorExtensions.Add("x-ms-visibility", "internal");
                    triggerStateParam.vendorExtensions.Add("x-ms-scheduler-recommendation",
                                                           "@coalesce(triggers()?.outputs?.body?['triggerState'], '')");
                }
            }
        }
    }
 

