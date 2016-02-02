<properties
    pageTitle="App Service Mobile Apps の管理されたクライアント ライブラリの操作 (Windows | Xamarin) | Microsoft Azure"
    description="Windows と Xamarin アプリで Azure App Service Mobile Apps 向け .NET クライアントを使用する方法について説明します。"
    services="app-service\mobile"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/05/2015" 
    ms.author="glenga"/>


# Azure Mobile Apps 用の管理されたクライアントの使用方法

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]
&nbsp;

[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

## 概要

このガイドでは、Windows および Xamarin アプリで Azure App Service Mobile Apps 用の管理されたクライアント ライブラリを使用する一般的なシナリオの実行方法を示します。 モバイル アプリに慣れていない場合は、完了するを検討してください、 [モバイル アプリのクイック スタート](app-service-mobile-windows-store-dotnet-get-started.md) チュートリアルです。 このガイドでは、クライアント側の管理された SDK に重点を置いています。 モバイル アプリのサーバー側 Sdk の詳細については、次を参照してください。 [.NET バックエンド SDK を使用](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) または [Node.js バックエンド SDK を使用する方法](app-service-mobile-node-backend-how-to-use-server-sdk.md)します。

## リファレンス ドキュメント

クライアント SDK リファレンス ドキュメントは、: [Azure モバイル アプリの .NET クライアント リファレンス](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.aspx)します。

## <a name="setup"></a>セットアップと前提条件

テーブルを 1 つ含むモバイル アプリ バックエンド プロジェクトを既に作成して発行してあるものとします。 このトピックで使用するコードでは、テーブルの名前 `TodoItem` し、次の列になります: `Id`, 、`テキスト`, 、および `完了`します。 これは、を完了したときに作成された同じテーブル [クイック スタート チュートリアル](app-service-mobile-windows-store-dotnet-get-started.md)

これに対応する型指定されたクライアント側 C# 型を次に示します。


    public class TodoItem
    {
        public string Id { get; set; }
    
        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }
    
        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    }

なお、 [JsonPropertyAttribute](http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm) を定義するために使用、 *PropertyName* クライアントの種類と、テーブル間のマッピングします。

モバイル アプリ バックエンドで新しいテーブルを作成する方法については、次を参照してください。 [方法: テーブル コント ローラーを定義する](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller) (.NET バックエンド) または [動的スキーマを使用してテーブルを定義する](app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations) (Node.js バックエンド)。 Node.js バックエンドでは、使用することも、 **簡単テーブル** の設定、 [Azure ポータル](https://portal.azure.com)します。

## <a name="create-client"></a>方法: モバイル アプリ クライアントの作成

次のコード作成、 `MobileServiceClient` モバイル アプリ バックエンドにアクセスするために使用します。

    MobileServiceClient client = new MobileServiceClient("MOBILE_APP_URL");

上記のコードで置き換えます `MOBILE_APP_URL` モバイル アプリ バックエンドの URL を使用して、モバイル アプリ バックエンドでのブレードであるが、 [Azure ポータル](https://portal.azure.com)します。

## <a name="instantiating"></a>方法: テーブル参照を作成します。

関数を呼び出しますのアクセスまたはバックエンド テーブル内のデータを変更するコードはすべて、 `MobileServiceTable` オブジェクトです。 呼び出して、テーブルへの参照を取得、 [GetTable](https://msdn.microsoft.com/library/azure/jj554275.aspx) メソッドのインスタンスを `MobileServiceClient`, 、次のようにします。

    IMobileServiceTable<TodoItem> todoTable =
        client.GetTable<TodoItem>();

これは、型指定されたシリアル化モデルです。 型指定されていないシリアル化モデルもサポートされます。 次のコードは、型指定されていないテーブルへの参照を作成します。

    // Get an untyped table reference
    IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");

型指定されていないクエリでは、基になる OData クエリ文字列を指定する必要があります。

## <a name="querying"></a>方法: モバイル アプリからのデータのクエリ

このセクションでは、モバイル アプリ バックエンドにクエリを発行する方法について説明します。これには次の機能が含まれます。

- [返されるデータをフィルター処理]
- [返されるデータを並べ替える]
- [ページにデータを返す]
- [特定の列を選択]
- [ID でデータを検索]

>[AZURE.NOTE] すべての行が返されるのを防ぐために、サーバー側で設定されたページ サイズが適用されます。 これにより、大きなデータ セットの既定の要求がサービスに悪影響を与えないようにします。 50 を超える行を返すには使用、 `かかる` メソッドを [ページに戻り値のデータ] で説明します。  

### <a name="filtering"></a>方法: 返されるデータをフィルター処理

次のコードを含めることによってデータをフィルター処理する方法を示しています、 `、` クエリ内の句。 すべての項目を返す `todoTable` が `完了` プロパティに等しい `false`します。  `、` 関数は、行のフィルタ リング述語をテーブルに対するクエリを適用します。

    // This query filters out completed TodoItems and
    // items without a timestamp.
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .ToListAsync();

ブラウザー開発者ツールや [Fiddler] などのメッセージ検査ソフトウェアを使用して、バックエンドに送信された要求の URI を表示することができます。 次の要求 URI を確認する場合は、クエリ文字列自体変更を加えていることに注意してください。

    GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1

通常、この要求は、Azure 側で次のような SQL クエリに変換されます。

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0

関数に渡されるには、 `、` メソッドは、任意の数の条件を設定できます。 たとえば、次のコードがあるとします。

    // This query filters out completed TodoItems where Text isn't null
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false
           && todoItem.Text != null)
       .ToListAsync();

このコードは、ほぼ次のように変換できます (前と同じ要求)。

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0
          AND ISNULL(text, 0) = 0

`、` 上記のステートメントにいる項目を検索 `完了` 状態は、null が false に設定 `テキスト`します。

代わりに、次のように複数の行で記述することもできます。

    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .Where(todoItem => todoItem.Text != null)
       .ToListAsync();

この 2 つの方法は等価であり、区別しないで使用できます。 複数の述語を 1 つのクエリに連結する前のオプションが、よりコンパクトでありお勧めです。

`、` 句のある操作をサポートする OData サブセットに変換します。これには、関係演算子が含まれます (= =、! =、 <, <=, >, 、> =)、算術演算子 (+、-、/、*、%)、単精度 (Math.Floor、Math.Ceiling)、文字列関数 (長さ、Substring、Replace、IndexOf、StartsWith、EndsWith)、日付プロパティ (年、月、日、時、分、秒) の番号、オブジェクト、およびこれらのすべてを組み合わせる式のプロパティにアクセスします。

### <a name="sorting"></a>方法: 返されるデータを並べ替える

次のコードを含めることによってデータを並べ替える方法を示しています、 `OrderBy` または `OrderByDescending` クエリ内の関数です。 項目を返します `todoTable` 昇順に並べ替えられた、 `テキスト` フィールドです。

    // Sort items in ascending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderBy(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();
    
    // Sort items in descending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderByDescending(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

### <a name="paging"></a>方法: ページにデータを返す

既定では、バックエンドは最初の 50 行のみを返します。 [Take] メソッドを呼び出すことによって、返された行の数を増やすことができます。 使用 `を` と共に [Skip] メソッドを特定の「ページ」データセット全体のクエリによって返されるを要求します。 次のクエリを実行すると、テーブルの最初の上位 3 つの項目が返されます。

    // Define a filtered query that returns the top 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

次の変更されたクエリは、最初の 3 つの結果をスキップし、その後の 3 つを返します。 ページ サイズが 3 つの項目である場合、これは実質的にデータの 2 番目の "ページ" になります。

    // Define a filtered query that skips the top 3 items and returns the next 3 items.
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Skip(3)
                    .Take(3);
    List<TodoItem> items = await query.ToListAsync();

[IncludeTotalCount] メソッドを使用して、クエリがの合計数を取得することを確認する <i>すべて</i> take paging/limit 句が指定されている、返されたレコードを無視して。

    query = query.IncludeTotalCount();

これは、ハードコーディングされたページング値を渡すことの簡略化されたシナリオ、 `かかる` と `Skip` メソッドです。 実際のアプリケーションでは、ユーザーが前後のページに移動できるように、ページャー コントロールまたは同等の UI と共に上記と同様のクエリを使用することができます。
>[AZURE.NOTE]モバイル アプリ バックエンドで 50 行の制限を無効にする必要がありますも適用する、 [EnableQueryAttribute](https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx) 公開するメソッドを取得し、ページング動作を指定します。 次のコードをメソッドに適用すると、返される最大行数が 1000 行に設定されます。

    [EnableQuery(MaxTop=1000)]

### <a name="selecting"></a>方法: 特定の列の選択

追加することで、結果に含める一連のプロパティを指定することができます、 `選択` クエリに句。 たとえば、次のコードでは、1 つのフィールドだけを選択する方法と、複数のフィールドを選択し、フォーマットする方法も示しています。

    // Select one field -- just the Text
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => todoItem.Text);
    List<string> items = await query.ToListAsync();
    
    // Select multiple fields -- both Complete and Text info
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => string.Format("{0} -- {1}", 
                        todoItem.Text.PadRight(30), todoItem.Complete ? 
                        "Now complete!" : "Incomplete!"));
    List<string> items = await query.ToListAsync();

これまでに説明した関数はいずれも付加的なものであるため、この後も呼び出すようにし、クエリに活用することにします。 次の例も参照してください。

    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Where(todoItem => todoItem.Complete == false)
                    .Select(todoItem => todoItem.Text)
                    .Skip(3).
                    .Take(3);
    List<string> items = await query.ToListAsync();

### <a name="lookingup"></a>方法: ID でデータを検索

`LookupAsync` 関数を使用して、特定の ID を持つデータベースからオブジェクトを検索

    // This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
    TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");

### <a name="lookingup"></a>方法: 型指定されていないクエリを実行します。

型指定されていないテーブル オブジェクトを使用してクエリを実行するときは、次の例のように、OData クエリ文字列を明示的に指定する必要があります。

    // Lookup untyped data using OData
    JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");

プロパティ バッグのように使用できる JSON 値が返されます。 JToken と Json.NET の詳細については、を参照してください [Json.NET](http://json.codeplex.com/)。

## <a name="inserting"></a>方法: モバイル アプリ バックエンドにデータを挿入

クライアントのすべての型には、**Id** という名前のメンバーが含まれる必要があります。その既定値は文字列です。 この **Id** は、オフラインで CRUD 操作を実行するために必要です。 次のコードは、テーブルに新しい行を挿入する方法を示しています。 パラメーターには、挿入するデータが .NET オブジェクトとして含まれます。

    await todoTable.InsertAsync(todoItem);

一意のカスタム ID 値が含まれていないかどうか、 `todoItem` に渡される、 `todoTable.InsertAsync` 呼び出すには、ID は、サーバーによって生成され設定されているため、値、 `todoItem` オブジェクトがクライアントに返されます。

型指定のないデータを挿入するために、次に示すように Json.NET を活用することもできます。

    JObject jo = new JObject();
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

メール アドレスを一意の文字列 ID として使用する例を次に示します。

    JObject jo = new JObject();
    jo.Add("id", "myemail@emaildomain.com");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.InsertAsync(jo);

### ID 値の操作

Mobile Apps は、テーブルの **ID** 列で一意のカスタム文字列値をサポートしています。 このため、アプリケーションは、ID にメール アドレスやユーザー名などのカスタム値を使用できます。

文字列 ID には、次のような利点があります。

+ データベースと往復することなく ID が生成されます。
+ 他のテーブルやデータベースのレコードをより簡単にマージできます。
+ ID 値をより適切にアプリケーションのロジックに統合できます。

挿入されたレコードで文字列 ID 値が設定されない場合は、モバイル アプリ バックエンドによって ID 用の一意の値が生成されます。 使用することができます、 `Guid.NewGuid()` 、クライアント上またはバックエンドで独自 ID 値を生成します。

## <a name="modifying"></a>方法: モバイル アプリ バックエンドでデータを変更します。

次のコードは、同じ ID を持つ既存のインスタンスを新しい情報で更新する方法を示しています。 パラメーターには、更新するデータが .NET オブジェクトとして含まれます。

    await todoTable.UpdateAsync(todoItem);

型指定されていないデータを挿入するには、Json.NET を活用を次のようにかかる場合があります。 
    JObject jo = 新しい JObject() です。
    jo します。("Id"、"37BBF396-11F0-4B39-85C8-B319C729AF6D") を追加します。
    jo します。("Text"、"Hello World") を追加します。
    jo します。追加 (「完了」、false);
    挿入 var = テーブルを待機します。UpdateAsync(jo) です。

更新を行うときは ID を指定する必要があることに注意してください。 バックエンドはそれによって更新するインスタンスを識別します。 結果から、ID を取得できます、 `InsertAsync` 呼び出します。 "Id"値を指定せずに項目を更新しようとすると、 `ArgumentException` が発生します。


## <a name="deleting"></a>方法: モバイル アプリ バックエンドでデータを削除します。

次のコードは、既存のインスタンスを削除する方法を示しています。 インスタンスに設定した"Id"フィールドで識別される、 `todoItem`します。

    await todoTable.DeleteAsync(todoItem);

型指定されていないデータを削除するには、次のような Json.NET を利用できます。

    JObject jo = new JObject();
    jo.Add("Id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    await table.DeleteAsync(jo);

削除要求を行うときは、ID を指定する必要があります。 それ以外のプロパティは、サービスに渡されないか、またはサービスで無視されます。 結果、 `DeleteAsync` 呼び出しは、通常 `null`します。 渡す ID は、の結果から取得できます、 `InsertAsync` 呼び出します。 "Id"フィールドを設定、せずに項目を削除しようとすると、 `MobileServiceInvalidOperationException` バックエンドからが返されます。

## <a name="#custom-api"></a>方法: カスタム API 呼び出し

カスタム API を使用してカスタム エンドポイントを定義することにより、insert、update、delete、read のいずれの操作にも関連しないサーバー機能を公開することができます。 カスタム API を使用することによって、HTTP メッセージ ヘッダーの読み取りや設定、JSON 以外のメッセージ本文形式の定義など、メッセージングをより柔軟に制御することができます。

クライアントで、[InvokeApiAsync] メソッドのオーバー ロードのいずれかには、カスタム API を呼び出します。 たとえば、次のコード行は、バックエンドの **completeAll** API に POST 要求を送信します。

    var result = await App.MobileService
        .InvokeApiAsync<MarkAllResult>("completeAll",
        System.Net.Http.HttpMethod.Post, null);

これは型指定メソッドの呼び出しのため、返される **MarkAllResult** の型を定義する必要があります。 型指定および型指定のないメソッドの両方がサポートされます。 型指定でペイロードを送信せず、クエリ パラメーターがなく、また要求ヘッダーを変更しないため、これは簡単な例になります。 現実的な例と、複数の詳細については [InvokeApiAsync]、[Azure Mobile Services クライアント Sdk のカスタム API] を参照してください。

## 方法: プッシュ通知に登録する

Mobile Apps クライアントでは、Azure Notification Hubs によるプッシュ通知に登録できます。 登録する場合、プラットフォーム固有のプッシュ通知サービス (PNS) からハンドルを取得します。 この値は、登録を作成するときに、任意のタグと一緒に指定します。 次のコードは、Windows Notification Service (WNS) によるプッシュ通知用の Windows アプリを登録します。

        private async void InitNotificationsAsync()
        {
            // Request a push notification channel.
            var channel =
                await PushNotificationChannelManager
                    .CreatePushNotificationChannelForApplicationAsync();
    
            // Register for notifications using the new channel and a tag collection.
            var tags = new List<string>{ "mytag1", "mytag2"};
            await MobileService.GetPush().RegisterNativeAsync(channel.Uri, tags);
        }

この例では、登録で 2 つのタグが含められます。 テンプレート登録に登録する方法を含む、Windows アプリの詳細については、次を参照してください。 [アプリにプッシュ通知を追加](app-service-mobile-windows-store-dotnet-get-started-push.md)します。

Xamarin アプリではいくつかの追加コードが必要になります。このコードを使用して、iOS または Android アプリで実行されているアプリを Apple Push Notification サービス (APNS) と Google Cloud Messaging (GCM) にそれぞれ登録することができます。 詳細については、次を参照してください **アプリにプッシュ通知を追加** ([Xamarin.iOS](partner-xamarin-mobile-services-ios-get-started-push.md#add-push) |。 [Xamarin.Android](partner-xamarin-mobile-services-android-get-started-push.md#add-push)).

## 方法: プッシュ テンプレートを登録してクロス プラットフォーム通知を送信する

テンプレートを登録するには、**MobileService.GetPush().RegisterAsync()** メソッドを使用してクライアント アプリにテンプレートを渡すだけです。

        MobileService.GetPush().RegisterAsync(channel.Uri, newTemplates());

テンプレートは JObject 型となり、次のような JSON 形式で複数のテンプレートを含めることが可能です。

        public JObject newTemplates()
        {
            // single template for Windows Notification Service toast
            var template = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(message)</text></binding></visual></toast>";
    
            var templates = new JObject
            {
                ["generic-message"] = new JObject
                {
                    ["body"] = template,
                    ["headers"] = new JObject
                    {
                        ["X-WNS-Type"] = "wns/toast"
                    },
                    ["tags"] = new JArray()
                },
                ["more-templates"] = new JObject {...}
            };
            return templates;
        }

**RegisterAsync()** メソッドは次のセカンダリ タイルも受け入れます。

        MobileService.GetPush().RegisterAsync(string channelUri, JObject templates, JObject secondaryTiles);

セキュリティのためにタグはすべて取り除かれるので注意してください。 インストールやインストール内のテンプレートには、タグを追加するには、[Azure モバイル アプリの作業を .NET バックエンド サーバー SDK] を参照してください。

これらの登録済みのテンプレートを使用して通知を送信する操作 [通知ハブの Api](https://msdn.microsoft.com/library/azure/dn495101.aspx)します。

## <a name="optimisticconcurrency"></a>方法: オプティミスティック同時実行制御

シナリオによっては、複数のクライアントが同じ項目に対して同時に変更を書き込む場合があります。 競合を検知しない場合、それを意図していなくても、最後に行われた書き込みによってそれ以前の更新がすべて上書きされます。 *オプティミスティック同時実行制御*では、それぞれのトランザクションがコミットでき、そのためリソース ロックが一切使用されないことを前提としています。 オプティミスティック同時実行制御ではトランザクションをコミットする前に、他のトランザクションがそのデータを変更していないことを確認します。 データが変更されている場合、トランザクションのコミットはロール バックされます。

Mobile Apps を使用して各項目の変更を追跡することによってオプティミスティック同時実行制御をサポートしている、 `_ _version` モバイル アプリ バックエンドでは、各テーブルに対して定義されているシステム プロパティ列。レコードが更新されるたびにモバイル アプリの設定、 `_ _version` を新しい値には、そのレコードのプロパティです。各更新要求時に、 `_ _version` 、要求に含まれるレコードのプロパティは、サーバー上のレコードの同じプロパティと比較します。バージョンが渡された場合、要求には、バックエンドと一致しませんし、クライアント ライブラリを生成、 `MobileServicePreconditionFailedException < T >`します。例外に含まれている型は、レコードのサーバー側のバージョンを含んでいるバックエンドのレコードです。アプリケーションは、更新要求を適切なもう一度実行するかどうかを決定するこの情報を使用してできる `_ _version` 変更をコミットするバックエンドからの値。

オプティミスティック同時実行制御を有効にする、アプリケーションはテーブル クラス用の列を定義、 `_ _version` システム プロパティです。 次の定義は、その一例です。

    public class TodoItem
    {
        public string Id { get; set; }
    
        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }
    
        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    
        // *** Enable Optimistic Concurrency *** //
        [JsonProperty(PropertyName = "__version")]
        public byte[] Version { set; get; }
    }

型指定されていないテーブルを使用してアプリケーションを設定してオプティミスティック同時実行制御を有効にする、 `バージョン` フラグを `SystemProperties` 次のようにテーブルのです。

    //Enable optimistic concurrency by retrieving __version
    todoTable.SystemProperties |= MobileServiceSystemProperties.Version;

次のコードは、書き込み競合が検出された場合にそれを解決する方法を示しています。 正しい `_ _version` で値を含める必要がある、 `UpdateAsync()` を解像度をコミットする呼び出しです。

    private async void UpdateToDoItem(TodoItem item)
    {
        MobileServicePreconditionFailedException<TodoItem> exception = null;
    
        try
        {
            //update at the remote table
            await todoTable.UpdateAsync(item);
        }
        catch (MobileServicePreconditionFailedException<TodoItem> writeException)
        {
            exception = writeException;
        }
    
        if (exception != null)
        {
            // Conflict detected, the item has changed since the last query
            // Resolve the conflict between the local and server item
            await ResolveConflict(item, exception.Item);
        }
    }
    
    
    private async Task ResolveConflict(TodoItem localItem, TodoItem serverItem)
    {
        //Ask user to choose the resoltion between versions
        MessageDialog msgDialog = new MessageDialog(String.Format("Server Text: \"{0}\" \nLocal Text: \"{1}\"\n",
                                                    serverItem.Text, localItem.Text),
                                                    "CONFLICT DETECTED - Select a resolution:");
    
        UICommand localBtn = new UICommand("Commit Local Text");
        UICommand ServerBtn = new UICommand("Leave Server Text");
        msgDialog.Commands.Add(localBtn);
        msgDialog.Commands.Add(ServerBtn);
    
        localBtn.Invoked = async (IUICommand command) =>
        {
            // To resolve the conflict, update the version of the
            // item being committed. Otherwise, you will keep
            // catching a MobileServicePreConditionFailedException.
            localItem.Version = serverItem.Version;
    
            // Updating recursively here just in case another
            // change happened while the user was making a decision
            UpdateToDoItem(localItem);
        };
    
        ServerBtn.Invoked = async (IUICommand command) =>
        {
            RefreshTodoItems();
        };
    
        await msgDialog.ShowAsync();
    }

詳細については、次を参照してください。、 [Azure Mobile Apps でのオフライン データ同期](app-service-mobile-offline-data-sync.md)します。


## <a name="binding"></a>方法: バインド モバイル アプリにデータを Windows ユーザー インターフェイス

このセクションでは、Windows アプリで UI 要素を使用して、返されたデータ オブジェクトを表示する方法について説明します。 未完了の項目を照会する `todoTable` 、非常に単純なリストに表示して、クエリで一覧のソースにバインドする、次のコード例を実行することができます。 使用して `MobileServiceCollection` モバイル アプリに対応のバインディング コレクションを作成します。

    // This query filters out completed TodoItems.
    MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
        .Where(todoItem => todoItem.Complete == false)
        .ToCollectionAsync();
    
    // itemsControl is an IEnumerable that could be bound to a UI list control
    IEnumerable itemsControl  = items;
    
    // Bind this to a ListBox
    ListBox lb = new ListBox();
    lb.ItemsSource = items;

マネージ ランタイムの一部のコントロールと呼ばれるインターフェイスをサポートする [ISupportIncrementalLoading](http://msdn.microsoft.com/library/windows/apps/Hh701916)します。 このインターフェイスにより、コントロールはユーザーによるスクロールの際に追加のデータを要求することができます。 このインターフェイスを使用してユニバーサル Windows 8.1 アプリ用のサポートは組み込まれて `MobileServiceIncrementalLoadingCollection`, 、コントロールからの呼び出しが自動的に処理します。 使用する `MobileServiceIncrementalLoadingCollection` Windows アプリで、次の手順します。

            MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
        items =  todoTable.Where(todoItem => todoItem.Complete == false)
                    .ToIncrementalLoadingCollection();
    
        ListBox lb = new ListBox();
        lb.ItemsSource = items;

Windows Phone 8、および"Silverlight"アプリで新しいコレクションを使用する、 `ToCollection` に対して拡張メソッド `IMobileServiceTableQuery < T >` と `IMobileServiceTable < T >`します。実際にデータを読み込む、 `LoadMoreItemsAsync()`します。

    MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
    await items.LoadMoreItemsAsync();

呼び出して作成されたコレクションを使用すると `ToCollectionAsync` または `ToCollection`, 、UI コントロールにバインドできるコレクションを取得します。 このコレクションはページングに対応しています。つまり、コントロールが "項目をさらに読み込む" ようにコレクションに依頼し、コレクションがそれを実行します。 その時点で、ユーザー コードは関係せず、コントロールはフローを開始します。 ただし、コレクションはネットワークからデータを読み込むため、読み込みが失敗することもあると想定されます。 オーバーライドがこのようなエラーを処理するため、 `OnException` メソッドを `MobileServiceIncrementalLoadingCollection` への呼び出しの結果として例外を処理する `LoadMoreItemsAsync` コントロールが実行されます。

最後に、テーブルには多くのフィールドが存在するものの、コントロールにはその一部のみを表示する必要があるとします。 セクションで、ガイダンスを使用することがあります"[特定の列を選択](#selecting)"上記を UI に表示する特定の列を選択します。

## <a name="package-sid"></a>方法: Windows ストアのパッケージ SID の取得

Windows アプリでは、プッシュ通知と特定の認証モードを有効にするにはパッケージ SID が必要です。 この値を取得するには:

1. Visual Studio ソリューション エクスプローラーで、Windows Store アプリ プロジェクトを右クリックし、**[ストア]**、**[アプリケーションをストアと関連付ける]** の順にクリックします。
2. ウィザードで **[次へ]** をクリックし、Microsoft アカウントでサインインし、**[新しいアプリケーション名の予約]** にアプリの名前を入力し、**[予約]** をクリックします。
3. アプリの登録が正常に作成されたら、新しいアプリ名を選択し、**[次へ]** をクリックし、**[関連付け]** をクリックします。 この操作により、必要な Windows ストア登録情報がアプリケーション マニフェストに追加されます。
4. ログイン、 [Windows デベロッパー センター](https://dev.windows.com/en-us/overview) Microsoft アカウントを使用します。 **[マイ アプリ]** で、先ほど作成したアプリ登録をクリックします。
5. **[アプリ管理]**、**[アプリ ID]** の順にクリックし、下にスクロールして **[パッケージ SID]** を探します。

多くの場合、パッケージ SID は URI として使用されます。その場合はスキームとして _ms-app://_ を使用する必要があります。 この値をプレフィックスとして連結して形成されたパッケージ SID のバージョンをメモしておきます。



## <a name="errors"></a>方法: エラーを処理します。

次の例では、バックエンドによって返される例外を処理する方法を示します。

    private async void InsertTodoItem(TodoItem todoItem)
    {
        // This code inserts a new TodoItem into the database. When the operation completes
        // and App Service has assigned an Id, the item is added to the CollectionView
        try
        {
            await todoTable.InsertAsync(todoItem);
            items.Add(todoItem);
        }
        catch (MobileServiceInvalidOperationException e)
        {
            // Handle error
        }
    }

## <a name="unit-testing"></a>方法: 単体テストを設計します。

によって返される値 `MobileServiceClient.GetTable` クエリは、インターフェイスとします。によりに作成できるので、テスト目的での「モック」簡単に、 `MyMockTable: IMobileServiceTable < TodoItem >` 、テスト ロジックを実装します。

## <a name="customizing"></a>方法: クライアントをカスタマイズします。

このセクションでは、要求ヘッダーをカスタマイズし、応答内の JSON オブジェクトのシリアル化をカスタマイズする方法について説明します。

### <a name="headers"></a>方法: カスタマイズ要求ヘッダー

特定のアプリケーション シナリオをサポートするには、モバイル アプリ バックエンドとの通信をカスタマイズすることが必要な場合があります。 たとえば、すべての送信要求にカスタム ヘッダーを追加したり、応答のステータス コードを変更したりすることが必要な場合がります。 ユーザー設定 [DelegatingHandler] を提供することで、次の例に示すように、これを実行できます。

    public async Task CallClientWithHandler()
    {
        MobileServiceClient client = new MobileServiceClient(
            "AppUrl", "", "",
            new MyHandler()
            );
        IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
        var newItem = new TodoItem { Text = "Hello world", Complete = false };
        await todoTable.InsertAsync(newItem);
    }
    
    public class MyHandler : DelegatingHandler
    {
        protected override async Task<HttpResponseMessage> 
            SendAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Add a custom header to the request.
            request.Headers.Add("x-my-header", "my value");
            var response = await base.SendAsync(request, cancellationToken);
            // Set a differnt response status code.
            response.StatusCode = HttpStatusCode.ServiceUnavailable;
            return response;
        }
    }

このコードは、要求内に新しい **x-my-header** ヘッダーを追加し、利用不可に対する応答コードを任意に設定します。 実際のシナリオでは、アプリケーションで必要とされるカスタム ロジックに基づいて応答ステータス コードを設定します。

### <a name="serialization"></a>方法: シリアル化をカスタマイズ

Mobile Apps クライアント ライブラリは、Json.NET を使用して、クライアントで JSON 応答を .NET オブジェクトに変換します。 メッセージでの .NET 型と JSON との間のこのシリアル化の動作を構成することができます。 [MobileServiceClient] クラスの公開、 `SerializerSettings` 型のプロパティ [JsonSerializerSettings](http://james.newtonking.com/projects/json/help/?topic=html/T_Newtonsoft_Json_JsonSerializerSettings.htm)

このプロパティを使用すれば、次のように、多くの Json.NET プロパティの中からいずれかを設定できます。

    var settings = new JsonSerializerSettings();
    settings.ContractResolver = new CamelCasePropertyNamesContractResolver();
    client.SerializerSettings = settings;

シリアル化中、このプロパティによってすべてのプロパティが小文字に変換されます。








[filter returned data]: #filtering 
[sort returned data]: #sorting 
[return data in pages]: #paging 
[select specific columns]: #selecting 
[look up data by id]: #lookingup 
[add authentication to your app]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-users.md 
[work with the .net backend server sdk for azure mobile apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md 
[passwordvault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx 
[protecteddata]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx 
[loginasync method]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceclientextensions.loginasync.aspx 
[mobileserviceauthenticationprovider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider.aspx 
[mobileserviceuser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.aspx 
[userid]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid.aspx 
[mobileserviceauthenticationtoken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken.aspx 
[ascii control codes c0 and c1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set 
[cli to manage mobile services tables]: ../virtual-machines-command-line-tools.md/#Commands_to_manage_mobile_services 
[optimistic concurrency tutorial]: mobile-services-windows-store-dotnet-handle-database-conflicts.md 
[mobileserviceclient]: http://msdn.microsoft.com/library/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx 
[includetotalcount]: http://msdn.microsoft.com/library/windowsazure/dn250560.aspx 
[skip]: http://msdn.microsoft.com/library/windowsazure/dn250573.aspx 
[take]: http://msdn.microsoft.com/library/windowsazure/dn250574.aspx 
[fiddler]: http://www.telerik.com/fiddler 
[custom api in azure mobile services client sdks]: http://blogs.msdn.com/b/carlosfigueira/archive/2013/06/19/custom-api-in-azure-mobile-services-client-sdks.aspx 
[invokeapiasync]: http://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.invokeapiasync.aspx 
[delegatinghandler]: https://msdn.microsoft.com/library/system.net.http.delegatinghandler(v=vs.110).aspx 

