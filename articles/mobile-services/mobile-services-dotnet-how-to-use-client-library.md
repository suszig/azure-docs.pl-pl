<properties
    pageTitle="Mobile Services 管理対象クライアント ライブラリの操作 (Windows | Xamarin) | Microsoft Azure"
    description="Windows と Xamarin アプリで Azure Mobile Services 向け .NET クライアントを使用する方法について説明します。"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="11/02/2015"
    ms.author="glenga"/>


# Azure Mobile Services 用の管理対象クライアント ライブラリの使用方法

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-client-library](../../includes/mobile-services-selector-client-library.md)]

## 概要

このガイドでは、Windows および Xamarin アプリで Azure Mobile Services 用の管理対象クライアント ライブラリを使用する一般的なシナリオの実行方法を示します。 紹介するシナリオは、データの照会、挿入、更新、および削除、ユーザーの認証、エラー処理などです。 モバイル サービスに慣れていない場合は、」を完了するを考慮する必要があります、 [モバイル サービス クイック スタート](mobile-services-dotnet-backend-xamarin-ios-get-started.md) チュートリアルです。

[AZURE.INCLUDE [mobile-services-concepts](../../includes/mobile-services-concepts.md)]

## <a name="setup"></a>セットアップと前提条件

前提条件として、モバイル サービスとテーブルを作成してあるとします。 詳細については、次を参照してください。 [テーブルを作成](http://go.microsoft.com/fwlink/?LinkId=298592)します。 このトピックで使用するコードでは、テーブルの名前 `TodoItem` し、次の列になります: `Id`, 、`テキスト`, 、および `完了`します。

これに対応する型指定されたクライアント側 .NET 型を次に示します。


    public class TodoItem
    {
        public string Id { get; set; }
    
        [JsonProperty(PropertyName = "text")]
        public string Text { get; set; }
    
        [JsonProperty(PropertyName = "complete")]
        public bool Complete { get; set; }
    }

なお、 [JsonPropertyAttribute](http://www.newtonsoft.com/json/help/html/Properties_T_Newtonsoft_Json_JsonPropertyAttribute.htm) クライアントの種類と、テーブル間の PropertyName マッピングの間のマッピングを定義するために使用します。

JavaScript バックエンド モバイル サービスで動的スキーマを有効にすると、挿入または更新の要求に含まれるオブジェクトに基づいて、Azure Mobile Services で自動的に新しい列が生成されます。 詳細については、次を参照してください。 [動的スキーマ](http://go.microsoft.com/fwlink/?LinkId=296271)します。 .NET バックエンド モバイル サービスでは、テーブルは、プロジェクトのデータ モデルで定義されます。

## <a name="create-client"></a>方法: モバイル サービス クライアントを作成します。

次のコードは、モバイル サービスにアクセスするために使用される `MobileServiceClient` オブジェクトを生成します。


    MobileServiceClient client = new MobileServiceClient(
        "AppUrl",
        "AppKey"
    );

上記のコードで置き換えます `AppUrl` と `AppKey` をモバイル サービスの URL とアプリケーション キーで順にします。 どちらも Microsoft Azure クラシック ポータルで確認できます。モバイル サービスを選択し、[ダッシュボード] をクリックしてください。
>[AZURE.IMPORTANT]アプリケーション キーは、モバイル サービスに対するランダムな要求をフィルターで除外するためのものであり、アプリケーションと共に配布されます。 このキーは暗号化されないため、セキュリティで保護されていると見なすことはできません。 モバイル サービスのデータへのアクセスを完全に保護するには、アクセスを許可する前にユーザーを認証する必要があります。 詳細については、次を参照してください。 [方法: ユーザーを認証する](#authentication)します。

## <a name="instantiating"></a>方法: テーブル参照を作成します。

テーブルのすべての Mobile Services のデータ アクセスまたは変更するコード関数を呼び出します、 `MobileServiceTable` オブジェクトです。 呼び出して、テーブルへの参照を取得、 [GetTable](https://msdn.microsoft.com/library/azure/jj554275.aspx) メソッドのインスタンスを `MobileServiceClient`, 、次のようにします。

    IMobileServiceTable<TodoItem> todoTable =
        client.GetTable<TodoItem>();

これは型指定されたシリアル化のモデルです。説明を参照してください、 [型指定されていないシリアル化のモデル](#untyped) 以下です。

## <a name="querying"></a>方法: モバイル サービスからのデータのクエリ

このセクションでは、モバイル サービスにクエリを発行する方法について説明します。これには次の機能が含まれます。

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

ブラウザー開発者ツールや [Fiddler] などのメッセージ検査ソフトウェアを使用して、モバイル サービスに送信された要求の URI を表示することができます。 次の要求 URI を確認する場合は、クエリ文字列自体変更を加えていることに注意してください。

    GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1

通常、この要求は、サーバー側で次のような SQL クエリに変換されます。

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

`、` 句がされる操作をサポートしている Mobile Services の OData サブセットに変換します。これには、関係演算子が含まれます (= =、! =、 <, <=, >, 、> =)、算術演算子 (+、-、/、*、%)、単精度 (Math.Floor、Math.Ceiling)、文字列関数 (長さ、Substring、Replace、IndexOf、StartsWith、EndsWith)、日付プロパティ (年、月、日、時、分、秒) の番号、オブジェクト、およびこれらのすべてを組み合わせる式のプロパティにアクセスします。

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

既定では、サーバーは最初の 50 行のみを返します。 [Take] メソッドを呼び出すことによって、返された行の数を増やすことができます。 使用 `を` と共に [Skip] メソッドを特定の「ページ」データセット全体のクエリによって返されるを要求します。 次のクエリを実行すると、テーブルの最初の上位 3 つの項目が返されます。

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

#### .NET バックエンド モバイル サービスのページングに関する考慮事項

.NET バックエンド モバイル サービスの 50 行の制限を無効にする必要がありますも適用する、 [EnableQueryAttribute](https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx) 公開するメソッドを取得し、ページング動作を指定します。 次のコードをメソッドに適用すると、返される最大行数が 1000 行に設定されます。

    [EnableQuery(MaxTop=1000)]

### <a name="selecting"></a>方法: 特定の列の選択

追加することで、結果に含める一連のプロパティを指定することができます、 `選択` クエリに句。 たとえば、次のコードでは、1 つのフィールドだけを選択する方法と、複数のフィールドを選択し、フォーマットする方法も示しています。

    // Select one field -- just the Text
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => todoItem.Text);
    List<string> items = await query.ToListAsync();
    
    // Select multiple fields -- both Complete and Text info
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .Select(todoItem => string.Format("{0} -- {1}", todoItem.Text.PadRight(30), todoItem.Complete ? "Now complete!" : "Incomplete!"));
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

## <a name="inserting"></a>方法: モバイル サービスにデータを挿入

> [AZURE.NOTE] 型に対して挿入、検索、削除、更新の各操作を実行する場合は、**ID** と呼ばれるメンバーを作成する必要があります。 そのため、**TodoItem** クラスの例には、名前 **ID** のメンバーがあります。 更新および削除の操作では、有効な ID 値を常に指定する必要があります。

次のコードは、テーブルに新しい行を挿入する方法を示しています。 パラメーターには、挿入するデータが .NET オブジェクトとして含まれます。

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

Mobile Services は、テーブルの **ID** 列で一意のカスタム文字列値をサポートしています。 このため、アプリケーションは、ID にメール アドレスやユーザー名などのカスタム値を使用できます。

文字列 ID には、次のような利点があります。

+ データベースと往復することなく ID が生成されます。
+ 他のテーブルやデータベースのレコードをより簡単にマージできます。
+ ID 値をより適切にアプリケーションのロジックに統合できます。

挿入されたレコードで文字列 ID 値が設定されない場合は、Mobile Services によって ID 用の一意の値が生成されます。 使用することができます、 `Guid.NewGuid()` 、クライアントまたは .NET モバイル バックエンド サービスで独自 ID 値を生成します。 JavaScript バックエンド モバイル サービスでの Guid の生成の詳細については、次を参照してください。 [方法: 一意の ID 値を生成](mobile-services-how-to-use-server-scripts.md#generate-guids)します。

また、テーブルに整数 ID を使用することもできます。 整数 ID を使用するにはしてテーブルを作成する必要があります、 `モバイル テーブル作成` コマンドを使用して、 `--integerId` オプション。 このコマンドは、Azure のコマンド ライン インターフェイス (CLI) で使用されます。 CLI の使用の詳細については、次を参照してください。 [モバイル サービス テーブルの管理用コマンド](../virtual-machines-command-line-tools.md#Mobile_Tables)します。

## <a name="modifying"></a>方法: モバイル サービスでデータを変更します。

次のコードは、同じ ID を持つ既存のインスタンスを新しい情報で更新する方法を示しています。 パラメーターには、更新するデータが .NET オブジェクトとして含まれます。

    await todoTable.UpdateAsync(todoItem);

型指定のないデータを挿入するために、次のような Json.NET を活用することもできます。 更新を行う場合は、更新対象のインスタンスをモバイル サービスが識別できるように、ID を指定する必要があることに注意してしてください。 結果から、ID を取得できます、 `InsertAsync` 呼び出します。

    JObject jo = new JObject();
    jo.Add("Id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.UpdateAsync(jo);

"Id"値を指定せずに項目を更新しようとする場合は、サービスを更新するには、対象のインスタンスを識別できないモバイル サービス SDK をスローための方法はありません、 `ArgumentException`します。


## <a name="deleting"></a>方法: モバイル サービスでデータを削除します。

次のコードは、既存のインスタンスを削除する方法を示しています。 インスタンスに設定した"Id"フィールドで識別される、 `todoItem`します。

    await todoTable.DeleteAsync(todoItem);

型指定のないデータを削除するために、次のような Json.NET を活用することもできます。 削除要求を行う場合は、削除対象のインスタンスをモバイル サービスが識別できるように、ID を指定する必要があることに注意してしてください。 削除要求は ID のみを必要とします。他のプロパティはサービスに渡されません。そのいずれかが渡された場合、サービスに無視されます。 結果、 `DeleteAsync` 呼び出しは、通常 `null` もします。 渡す ID は、の結果から取得できます、 `InsertAsync` 呼び出します。

    JObject jo = new JObject();
    jo.Add("Id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    await table.DeleteAsync(jo);

"Id"フィールドを設定せずに項目を削除しようとする場合は、サービスを削除する対象のインスタンスを識別できない返ってくるための方法はありません、 `MobileServiceInvalidOperationException` サービスからです。 同様に、"Id"フィールドを設定せずに型指定されていない項目を削除しようとすると、もう一度取得、 `MobileServiceInvalidOperationException` サービスからです。

## <a name="#custom-api"></a>方法: カスタム API 呼び出し

カスタム API を使用してカスタム エンドポイントを定義することにより、insert、update、delete、read のいずれの操作にも関連しないサーバー機能を公開することができます。 カスタム API を使用することによって、HTTP メッセージ ヘッダーの読み取りや設定、JSON 以外のメッセージ本文形式の定義など、メッセージングをより柔軟に制御することができます。 モバイル サービスでカスタム API を作成する方法の例は、次を参照してください。 [方法: カスタム API エンドポイントを定義する](mobile-services-dotnet-backend-define-custom-api.md)します。

クライアントで、[InvokeApiAsync] メソッドのオーバー ロードのいずれかには、カスタム API を呼び出します。 たとえば、次のコード行はモバイル サービスで **completeAll** API に POST 要求を送信します。

    var result = await App.MobileService
        .InvokeApiAsync<MarkAllResult>("completeAll",
        System.Net.Http.HttpMethod.Post, null);

これは型指定メソッドの呼び出しのため、返される **MarkAllResult** の型を定義する必要があります。 型指定および型指定のないメソッドの両方がサポートされます。 型指定でペイロードを送信せず、クエリ パラメーターがなく、また要求ヘッダーを変更しないため、これは簡単な例になります。 現実的な例と、複数の詳細については [InvokeApiAsync]、[Azure Mobile Services クライアント Sdk のカスタム API] を参照してください。

## 方法: プッシュ通知に登録する

Mobile Services クライアントでは、Azure Notification Hubs によるプッシュ通知に登録することができます。 登録する場合、プラットフォーム固有のプッシュ通知サービス (PNS) からハンドルを取得します。 この値は、登録を作成するときに、任意のタグと一緒に指定します。 次のコードは、Windows Notification Service (WNS) によるプッシュ通知用の Windows アプリを登録します。

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

この例では、登録で 2 つのタグが含められます。 Windows アプリの詳細については、次を参照してください。 [アプリにプッシュ通知を追加](mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md)します。

Xamarin アプリではいくつかの追加コードが必要になります。このコードを使用して、iOS または Android アプリで実行されている Xamarin アプリを Apple Push Notification サービス (APNS) と Google Cloud Messaging (GCM) にそれぞれ登録することができます。 詳細については、次を参照してください **アプリにプッシュ通知を追加** ([Xamarin.iOS](partner-xamarin-mobile-services-ios-get-started-push.md#add-push) |。 [Xamarin.Android](partner-xamarin-mobile-services-android-get-started-push.md#add-push)).
>[AZURE.NOTE]特定の登録されたユーザーに通知を送信する必要がある場合は、登録前に認証を要求し、特定のタグへの登録をユーザーが許可されていることを確認することが重要です。 たとえば、ユーザーが他のユーザーの ID であるタグに登録していないことを確認する必要があります。 詳細については、次を参照してください。 [認証されたユーザーにプッシュ通知の送信](mobile-services-dotnet-backend-windows-store-dotnet-push-notifications-app-users.md)します。

## <a name="pull-notifications"></a>方法: Windows アプリで定期的な通知の使用

Windows は、ライブ タイルを更新する定期的な通知 (プル通知) をサポートします。 定期的な通知を有効にすると、Windows は [スタート] メニューのアプリケーション タイルを更新するカスタム API エンドポイントに定期的にアクセスします。 定期的な通知を使用する必要があります [カスタム API を定義する](mobile-services-javascript-backend-define-custom-api.md) タイル固有の形式で XML データを返します。 詳細については、次を参照してください。 [定期的な通知](https://msdn.microsoft.com/library/windows/apps/hh761461.aspx)します。

次の例は定期的な通知を有効にし、*tiles* カスタム エンドポイントからタイル テンプレート データを要求します。

    TileUpdateManager.CreateTileUpdaterForApplication().StartPeriodicUpdate(
        new System.Uri(MobileService.ApplicationUri, "/api/tiles"),
        PeriodicUpdateRecurrence.Hour
    );

選択、 [PeriodicUpdateRecurrance](https://msdn.microsoft.com/library/windows/apps/windows.ui.notifications.periodicupdaterecurrence.aspx) 、データの更新の頻度に最も一致する値。

## <a name="optimisticconcurrency"></a>方法: オプティミスティック同時実行制御

シナリオによっては、複数のクライアントが同じ項目に対して同時に変更を書き込む場合があります。 競合を検知しない場合、それを意図していなくても、最後に行われた書き込みによってそれ以前の更新がすべて上書きされます。 オプティミスティック同時実行制御では、それぞれのトランザクションがコミットでき、そのためリソース ロックが一切使用されないことを前提としています。 オプティミスティック同時実行制御ではトランザクションをコミットする前に、他のトランザクションがそのデータを変更していないことを確認します。 データが変更されている場合、トランザクションのコミットはロール バックされます。

モバイル サービスを使用して各項目の変更を追跡することによってオプティミスティック同時実行制御をサポートしている、 `_ _version` モバイル サービスによって作成された各テーブルに対して定義されているシステム プロパティ列。レコードが更新されるたびにモバイル サービスの設定、 `_ _version` を新しい値には、そのレコードのプロパティです。各更新要求時に、 `_ _version` 、要求に含まれるレコードのプロパティは、サーバー上のレコードの同じプロパティと比較します。要求が、サーバーと一致しませんし、Mobile Services .NET クライアント ライブラリをスローと共にバージョンが渡される場合、 `MobileServicePreconditionFailedException < T >`します。例外に含まれている型は、レコードのサーバー側のバージョンを含んでいるサーバーのレコードです。アプリケーションは、更新要求を適切なもう一度実行するかどうかを決定するこの情報を使用してできる `_ _version` 変更をコミットするサーバーの値。

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

オプティミスティック同時実行制御を使用して、モバイル サービス向けのより完全な例は、[オプティミスティック同時実行制御のチュートリアル] を参照してください。


## <a name="binding"></a>方法: モバイル サービスのデータを Windows ユーザー インターフェイスにバインドします。

このセクションでは、Windows アプリで UI 要素を使用して、返されたデータ オブジェクトを表示する方法について説明します。 未完了の項目を照会する `todoTable` 、非常に単純なリストに表示して、クエリで一覧のソースにバインドする、次のコード例を実行することができます。 使用して `MobileServiceCollection` モバイル サービスに対応するバインディングのコレクションを作成します。

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

## <a name="authentication"></a>方法: ユーザーを認証します。

Mobile Services は、Facebook、Google、Microsoft アカウント、Twitter、Azure Active Directory などのさまざまな外部 ID プロバイダーを使用したアプリケーション ユーザーの認証と承認をサポートします。 テーブルのアクセス許可を設定することにより、特定の操作へのアクセスを認証されたユーザーのみに制限できます。 さらに、認証されたユーザーの ID を使用することにより、サーバー スクリプトで承認ルールを実装することもできます。 詳細については、[認証アプリを追加する] このチュートリアルを参照してください。

_サーバー フロー_と_クライアント フロー_という 2 つの認証フローがサポートされます。 サーバー フローには、プロバイダーの Web 認証のインターフェイスを利用する、最も簡単な認証方法が用意されています。 クライアント フローでは、プロバイダー固有とデバイス固有の SDK を利用することから、デバイス固有の機能との統合がさらに進みます。

### サーバー フロー

モバイル サービス Windows アプリの認証プロセスを管理するには
アプリケーションを ID プロバイダーに登録する必要があります。 その後、モバイル サービス内で、プロバイダーから提供されたアプリケーション ID とシークレットを構成する必要があります。 詳細については、[認証アプリを追加する] このチュートリアルを参照してください。

ID プロバイダーを登録した後は、単純にプロバイダーの [MobileServiceAuthenticationProvider] 値で [LoginAsync メソッド] を呼び出すだけです。 たとえば、次のコードは、Facebook を使用してサーバー フローのサインインを開始します。

    private MobileServiceUser user;
    private async System.Threading.Tasks.Task Authenticate()
    {
        while (user == null)
        {
            string message;
            try
            {
                user = await client
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
    
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

Facebook 以外の id プロバイダーを使用している場合は、プロバイダーの値に上の [MobileServiceAuthenticationProvider] 値を変更します。

この場合、Mobile Services は、選択されたプロバイダーのサインイン ページを表示し、ID プロバイダーでのサインインが成功した後で Mobile Services 認証トークンを生成することで、OAuth 2.0 認証フローを管理します。 JSON web トークン (JWT) として [userId] の認証されたユーザーと [MobileServiceAuthenticationToken] を提供する [LoginAsync メソッド] 返します [MobileServiceUser]。 このトークンはキャッシュして、期限が切れるまで再利用することができます。 詳細については、「認証トークンをキャッシュする」を参照してください。

### クライアント フロー

アプリケーションは個別に ID プロバイダーにアクセスして、返されたトークンを認証のために Mobile Services に提供することもできます。 このクライアント フローでは、ユーザーにシングル サインイン エクスペリエンスを提供したり、ID プロバイダーから追加のユーザー データを取得したりすることができます。

#### Facebook や Google から取得したトークンを利用したシングル サインイン

最も単純な形式では、この Facebook や Google のスニペットに示すようにクライアント フローを使用できます。

    var token = new JObject();
    // Replace access_token_value with actual value of your access token obtained
    // using the Facebook or Google SDK.
    token.Add("access_token", "access_token_value");
    
    private MobileServiceUser user;
    private async System.Threading.Tasks.Task Authenticate()
    {
        while (user == null)
        {
            string message;
            try
            {
                // Change MobileServiceAuthenticationProvider.Facebook
                // to MobileServiceAuthenticationProvider.Google if using Google auth.
                user = await client
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
                message =
                    string.Format("You are now logged in - {0}", user.UserId);
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
    
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
    }

#### Microsoft アカウントと Live SDK を利用したシングル サインイン

ユーザーを認証できるようにするには、Microsoft アカウント デベロッパー センターでアプリケーションを登録する必要があります。 この登録をモバイル サービスと接続する必要があります。 手順を完了 [Microsoft アカウント ログインを使用するようアプリを登録](mobile-services-how-to-register-microsoft-authentication.md) を Microsoft アカウントの登録を作成し、モバイル サービスに接続します。 Windows ストア と Windows Phone 8/Silverlight の両方のバージョンのアプリがある場合は、Windows ストア バージョンを最初に登録します。

次のコードでは Live SDK を使用して認証し、返されたトークンを利用してモバイル サービスにサインインします。

    private LiveConnectSession session;
    //private static string clientId = "<microsoft-account-client-id>";
    private async System.Threading.Tasks.Task AuthenticateAsync()
    {
    
        // Get the URL the mobile service.
        var serviceUrl = App.MobileService.ApplicationUri.AbsoluteUri;
    
        // Create the authentication client for Windows Store using the mobile service URL.
        LiveAuthClient liveIdClient = new LiveAuthClient(serviceUrl);
        //// Create the authentication client for Windows Phone using the client ID of the registration.
        //LiveAuthClient liveIdClient = new LiveAuthClient(clientId);
    
        while (session == null)
        {
            // Request the authentication token from the Live authentication service.
            // The wl.basic scope is requested.
            LiveLoginResult result = await liveIdClient.LoginAsync(new string[] { "wl.basic" });
            if (result.Status == LiveConnectSessionStatus.Connected)
            {
                session = result.Session;
    
                // Get information about the logged-in user.
                LiveConnectClient client = new LiveConnectClient(session);
                LiveOperationResult meResult = await client.GetAsync("me");
    
                // Use the Microsoft account auth token to sign in to Mobile Services.
                MobileServiceUser loginResult = await App.MobileService
                    .LoginWithMicrosoftAccountAsync(result.Session.AuthenticationToken);
    
                // Display a personalized sign-in greeting.
                string title = string.Format("Welcome {0}!", meResult.Result["first_name"]);
                var message = string.Format("You are now logged in - {0}", loginResult.UserId);
                var dialog = new MessageDialog(message, title);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
            else
            {
                session = null;
                var dialog = new MessageDialog("You must log in.", "Login Required");
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }
    }

### <a name="caching"></a>認証トークンをキャッシュ

場合によっては、最初のユーザー認証の後の login メソッドの呼び出しを避けることができます。 Windows ストア アプリの [PasswordVault] を使用するには初めてログインする現在のユーザー id をキャッシュして、キャッシュにユーザー id が既にあるかどうかをチェックするたびにその後にします。 キャッシュが空の場合は、ユーザーにログイン プロセスを実行してもらう必要があります。

    // After logging in
    PasswordVault vault = new PasswordVault();
    vault.Add(new PasswordCredential("Facebook", user.UserId, user.MobileServiceAuthenticationToken));
    
    // Log in
    var creds = vault.FindAllByResource("Facebook").FirstOrDefault();
    if (creds != null)
    {
        user = new MobileServiceUser(creds.UserName);
        user.MobileServiceAuthenticationToken = vault.Retrieve("Facebook", creds.UserName).Password;
    }
    else
    {
        // Regular login flow
        user = new MobileServiceuser( await client
            .LoginAsync(MobileServiceAuthenticationProvider.Facebook, token);
        var token = new JObject();
        // Replace access_token_value with actual value of your access token
        token.Add("access_token", "access_token_value");
    }
    
     // Log out
    client.Logout();
    vault.Remove(vault.Retrieve("Facebook", user.UserId));

Windows Phone アプリについてする可能性がありますを暗号化 [ProtectedData] クラスを使用してデータをキャッシュおよび分離ストレージに機密情報を保存します。

## <a name="errors"></a>方法: エラーを処理します。

Mobile Services には、エラーの検出、検証、回避のためのさまざまな方法があります。

たとえば、サーバー スクリプトは、モバイル サービスに登録され、挿入や更新が行われるデータでの広範な操作 (検証やデータの修正を含む) の実行に使用できます。 次のようにデータを検証および変更するサーバー スクリプトを定義し、登録するとします。

    function insert(item, user, request)
    {
       if (item.text.length > 10) {
          request.respond(statusCodes.BAD_REQUEST, { error: "Text cannot exceed 10 characters" });
       } else {
          request.execute();
       }
    }

サーバー側スクリプトは、モバイル サービスに送信された文字列データの長さを検証し、長すぎる文字列 (この場合は 10 文字を超える) を拒否します。

モバイル サービスはサーバー側でデータを検証してエラー応答を送信するため、検証からのエラー応答を処理できるように .NET アプリケーションを更新することができます。

    private async void InsertTodoItem(TodoItem todoItem)
    {
        // This code inserts a new TodoItem into the database. When the operation completes
        // and Mobile Services has assigned an Id, the item is added to the CollectionView
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

## <a name="untyped"></a>方法: 型指定されていないデータを処理

.NET クライアントは厳密に型指定されたシナリオ向けに設計されます。 ただし、大まかに型指定されたエクスペリエンスの方が便利であることもあります。たとえば、オープン スキーマでオブジェクトを処理する場合です。 このシナリオには次のように対応できます。 クエリでは、LINQ を使用せずに、ワイヤ形式を使用します。

    // Get an untyped table reference
    IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");
    
    // Lookup untyped data using OData
    JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");

プロパティ バッグのように使用できる JSON 値が返されます。 JToken と Json.NET の詳細については、を参照してください [Json.NET](http://json.codeplex.com/)。

## <a name="unit-testing"></a>方法: 単体テストを設計します。

によって返される値 `MobileServiceClient.GetTable` クエリは、インターフェイスとします。によりに作成できるので、テスト目的での「モック」簡単に、 `MyMockTable: IMobileServiceTable < TodoItem >` 、テスト ロジックを実装します。

## <a name="customizing"></a>方法: クライアントをカスタマイズします。

このセクションでは、要求ヘッダーをカスタマイズし、応答内の JSON オブジェクトのシリアル化をカスタマイズする方法について説明します。

### <a name="headers"></a>方法: カスタマイズ要求ヘッダー

特定のアプリケーション シナリオをサポートするには、モバイル サービスとの通信をカスタマイズすることが必要な場合があります。 たとえば、すべての送信要求にカスタム ヘッダーを追加したり、応答のステータス コードを変更したりすることが必要な場合がります。 これを行うには、次の例のように、カスタムの DelegatingHandler を指定します。

    public async Task CallClientWithHandler()
    {
        MobileServiceClient client = new MobileServiceClient(
            "AppUrl",
            "AppKey" ,
            new MyHandler()
            );
        IMobileServiceTable<TodoItem> todoTable = client.GetTable<TodoItem>();
        var newItem = new TodoItem { Text = "Hello world", Complete = false };
        await table.InsertAsync(newItem);
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

Mobile Services クライアント ライブラリは、Json.NET を使用して、クライアントで JSON 応答を .NET オブジェクトに変換します。 メッセージでの .NET 型と JSON との間のこのシリアル化の動作を構成することができます。  [MobileServiceClient](http://msdn.microsoft.com/library/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx) クラスが公開、 `SerializerSettings` 型のプロパティ [JsonSerializerSettings](http://james.newtonking.com/projects/json/help/?topic=html/T_Newtonsoft_Json_JsonSerializerSettings.htm)

このプロパティを使用すれば、次のように、多くの Json.NET プロパティの中からいずれかを設定できます。

    var settings = new JsonSerializerSettings();
    settings.ContractResolver = new CamelCasePropertyNamesContractResolver();
    client.SerializerSettings = settings;

シリアル化中、このプロパティによってすべてのプロパティが小文字に変換されます。








[what is mobile services]: #what-is 
[concepts]: #concepts 
[how to: create the mobile services client]: #create-client 
[how to: create a table reference]: #instantiating 
[how to: query data from a mobile service]: #querying 
[filter returned data]: #filtering 
[sort returned data]: #sorting 
[return data in pages]: #paging 
[select specific columns]: #selecting 
[look up data by id]: #lookingup 
[how to: bind data to user interface in a mobile service]: #binding 
[how to: insert data into a mobile service]: #inserting 
[how to: modify data in a mobile service]: #modifying 
[how to: delete data in a mobile service]: #deleting 
[how to: use optimistic concurrency]: #optimisticconcurrency 
[how to: authenticate users]: #authentication 
[how to: handle errors]: #errors 
[how to: design unit tests]: #unit-testing 
[how to: query data from a mobile service]: #querying 
[how to: customize the client]: #customizing 
[how to: work with untyped data]: #untyped 
[customize request headers]: #headers 
[customize serialization]: #serialization 
[next steps]: #nextsteps 
[caching the authentication token]: #caching 
[how to: call a custom api]: #custom-api 
[add authentication to your app]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-users.md 
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
[mobileserviceclient]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx 
[includetotalcount]: http://msdn.microsoft.com/library/windowsazure/dn250560.aspx 
[skip]: http://msdn.microsoft.com/library/windowsazure/dn250573.aspx 
[take]: http://msdn.microsoft.com/library/windowsazure/dn250574.aspx 
[fiddler]: http://www.telerik.com/fiddler 
[custom api in azure mobile services client sdks]: http://blogs.msdn.com/b/carlosfigueira/archive/2013/06/19/custom-api-in-azure-mobile-services-client-sdks.aspx 
[invokeapiasync]: http://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.invokeapiasync.aspx 

