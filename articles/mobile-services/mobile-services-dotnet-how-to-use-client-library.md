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

##概要

このガイドでは、Windows および Xamarin アプリで Azure Mobile Services 用の管理対象クライアント ライブラリを使用する一般的なシナリオの実行方法を示します。 紹介するシナリオは、データの照会、挿入、更新、および削除、ユーザーの認証、エラー処理などです。 モバイル サービスに慣れていない場合は、」を完了するを考慮する必要があります、 [モバイル サービス クイック スタート](mobile-services-dotnet-backend-xamarin-ios-get-started.md) チュートリアルです。

[AZURE.INCLUDE [mobile-services-concepts](../../includes/mobile-services-concepts.md)]

##<a name="setup"></a>セットアップと前提条件

前提条件として、モバイル サービスとテーブルを作成してあるとします。 詳細については、次を参照してください。 [テーブルを作成](http://go.microsoft.com/fwlink/?LinkId=298592)します。 このトピックで使用するコードでは、テーブルの名前は `TodoItem` であり、`Id`、`Text`、および `Complete` という列があります。

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

##<a name="create-client"></a>方法: Mobile Services クライアントを作成する

次のコードは、モバイル サービスにアクセスするために使用される `MobileServiceClient` オブジェクトを生成します。


    MobileServiceClient client = new MobileServiceClient(
        "AppUrl",
        "AppKey"
    );

前のコードの `AppUrl` と `AppKey` を、モバイル サービスの URL とアプリケーション キーで順に置き換えます。 どちらも Microsoft Azure クラシック ポータルで確認できます。モバイル サービスを選択し、[ダッシュボード] をクリックしてください。

>[AZURE.IMPORTANT]アプリケーション キーの目的は、モバイル サービスに対するランダムな要求をフィルターで除外して、アプリケーションと共に配布されます。 このキーは暗号化されないため、セキュリティで保護されていると見なすことはできません。 モバイル サービスのデータへのアクセスを完全に保護するには、アクセスを許可する前にユーザーを認証する必要があります。 詳細については、次を参照してください。 [方法: ユーザーを認証する](#authentication)です。

##<a name="instantiating"></a>方法: テーブル参照を作成する

Mobile Services のテーブル データにアクセスするコードとそのデータを変更するコードは、必ず `MobileServiceTable` オブジェクトで関数を呼び出します。 呼び出して、テーブルへの参照を取得、 [GetTable](https://msdn.microsoft.com/library/azure/jj554275.aspx) メソッドのインスタンスを `MobileServiceClient`, 、次のようにします。

    IMobileServiceTable<TodoItem> todoTable =
        client.GetTable<TodoItem>();

これは型指定されたシリアル化のモデルです。説明を参照してください、 [型指定されていないシリアル化のモデル](#untyped) 以下です。

##<a name="querying"></a>方法: モバイル サービスのデータを照会する

このセクションでは、モバイル サービスにクエリを発行する方法について説明します。これには次の機能が含まれます。

- [返されるデータをフィルター処理する]
- [返されるデータを並べ替える]
- [ページにデータを返す]
- [特定の列を選択する]
- [ID でデータを検索する]

>[AZURE.NOTE] すべての行が返されるようにするのには、サーバー駆動型ページ サイズが適用されます。 これにより、大きなデータ セットの既定の要求がサービスに悪影響を与えないようにします。 50 を超える行を返すを使用して、 `Take` メソッド、」の説明に従って [Return data in pages]します。

### <a name="filtering"></a>方法: 返されるデータをフィルター処理する

次のコードは、クエリに `Where` 句を含めることによってデータをフィルター処理する方法を示しています。 このコードは、`Complete` プロパティが `false` に等しい `todoTable` からすべての項目を返します。 `Where` 関数は、行のフィルタリング述語をテーブルに対するクエリに適用します。

    // This query filters out completed TodoItems and
    // items without a timestamp.
    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .ToListAsync();

ブラウザー開発者ツールなどのメッセージ検査ソフトウェアを使用して、モバイル サービスに送信された要求の URI を表示するか、 [Fiddler]します。 次の要求 URI を確認する場合は、クエリ文字列自体変更を加えていることに注意してください。

    GET /tables/todoitem?$filter=(complete+eq+false) HTTP/1.1
通常、この要求は、サーバー側で次のような SQL クエリに変換されます。

    SELECT *
    FROM TodoItem
    WHERE ISNULL(complete, 0) = 0

`Where` メソッドに渡される関数には、任意の数の条件を設定できます。 たとえば、次のコードがあるとします。

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

前の `where` ステートメントは、null 以外の `Text` を持ち、`Complete` のステータスが false に設定されている項目を検索します。

代わりに、次のように複数の行で記述することもできます。

    List<TodoItem> items = await todoTable
       .Where(todoItem => todoItem.Complete == false)
       .Where(todoItem => todoItem.Text != null)
       .ToListAsync();

この 2 つの方法は等価であり、区別しないで使用できます。  複数の述語を 1 つのクエリに連結する前のオプションが、よりコンパクトでありお勧めです。

`where` 句は、Mobile Services の OData サブセットに変換される操作をサポートします。 これには、関係演算子 (==、! =、<、<=、>、>=)、算術演算子 (+、-、/、*、%)、数の精度 (Math.Floor、Math.Ceiling)、文字列関数 (Length、Substring、Replace、IndexOf、StartsWith、EndsWith)、日付プロパティ (Year、Month、Day、Hour、Minute、Second)、オブジェクトのアクセス プロパティ、これらすべてを組み合わせた式が含まれます。

### <a name="sorting"></a>方法: 返されるデータを並べ替える

次のコードは、クエリに `OrderBy` または `OrderByDescending` 関数を含めることによってデータを並べ替える方法を示しています。 次のコードは、`todoTable` から、`Text` フィールドの値に基づいて昇順に並べ替えられた項目を返します。

    // Sort items in ascending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderBy(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

    // Sort items in descending order by Text field
    MobileServiceTableQuery<TodoItem> query = todoTable
                    .OrderByDescending(todoItem => todoItem.Text)
    List<TodoItem> items = await query.ToListAsync();

### <a name="paging"></a>方法: ページにデータを返す

既定では、サーバーは最初の 50 行のみを返します。 呼び出すことによって返される行の数を増やすことができます、 [Take] メソッドです。 使用 `Take` と共に、 [Skip] メソッドを特定の「ページ」データセット全体のクエリによって返されるを要求します。 次のクエリを実行すると、テーブルの最初の上位 3 つの項目が返されます。

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

使用することも、 [IncludeTotalCount] メソッドがクエリの合計数を取得することを確認する <i>すべて</i> take paging/limit 句が指定されている、返されたレコードを無視して。

    query = query.IncludeTotalCount();

これは、ハードコーディングされたページング値を `Take` メソッドおよび `Skip` メソッドに渡す、簡略化したシナリオです。 実際のアプリケーションでは、ユーザーが前後のページに移動できるように、ページャー コントロールまたは同等の UI と共に上記と同様のクエリを使用することができます。

####.NET バックエンド モバイル サービスのページングに関する考慮事項

.NET バックエンド モバイル サービスの 50 行の制限を無効にする必要がありますも適用する、 [EnableQueryAttribute](https://msdn.microsoft.com/library/system.web.http.odata.enablequeryattribute.aspx) 公開するメソッドを取得し、ページング動作を指定します。 次のコードをメソッドに適用すると、返される最大行数が 1000 行に設定されます。

    [EnableQuery(MaxTop=1000)]


### <a name="selecting"></a>方法: 特定の列を選択する

クエリに `Select` 句を追加し、結果に含める一連のプロパティを指定できます。 たとえば、次のコードでは、1 つのフィールドだけを選択する方法と、複数のフィールドを選択し、フォーマットする方法も示しています。

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

### <a name="lookingup"></a>方法: ID でデータを検索する

`LookupAsync` 関数を使うと、データベースから特定の ID を持つオブジェクトを検索することができます。

    // This query filters out the item with the ID of 37BBF396-11F0-4B39-85C8-B319C729AF6D
    TodoItem item = await todoTable.LookupAsync("37BBF396-11F0-4B39-85C8-B319C729AF6D");

##<a name="inserting"></a>方法: モバイル サービスにデータを挿入する

> [AZURE.NOTE] 呼ばれるメンバーを作成する必要がある参照、削除、または更新操作の型に対して挿入を実行する場合は、 **Id**します。 その理由は、クラスの例 **TodoItem** 名前のメンバーを持つ **Id**します。 更新および削除の操作では、有効な ID 値を常に指定する必要があります。

次のコードは、テーブルに新しい行を挿入する方法を示しています。 パラメーターには、挿入するデータが .NET オブジェクトとして含まれます。

    await todoTable.InsertAsync(todoItem);

`todoTable.InsertAsync` 呼び出しに渡された `todoItem` に一意のカスタム ID 値が含まれていない場合は、ID の値がサーバーによって生成され、クライアントに返される `todoItem` オブジェクトに設定されます。

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


###ID 値の操作

モバイル サービス テーブルの一意のカスタム文字列値をサポートしている **id** 列です。 このため、アプリケーションは、ID にメール アドレスやユーザー名などのカスタム値を使用できます。

文字列 ID には、次のような利点があります。

+ データベースと往復することなく ID が生成されます。
+ 他のテーブルやデータベースのレコードをより簡単にマージできます。
+ ID 値をより適切にアプリケーションのロジックに統合できます。

挿入されたレコードで文字列 ID 値が設定されない場合は、Mobile Services によって ID 用の一意の値が生成されます。 `Guid.NewGuid()` メソッドを使用すれば、クライアント上でまたは .NET モバイル バックエンド サービスで、独自の ID 値を生成することができます。 JavaScript バックエンド モバイル サービスでの Guid の生成の詳細については、次を参照してください。 [方法: 一意の ID 値を生成](mobile-services-how-to-use-server-scripts.md#generate-guids)します。

また、テーブルに整数 ID を使用することもできます。 整数 ID を使用するには、`mobile table create` コマンドで `--integerId` オプションを使用してテーブルを作成する必要があります。 このコマンドは、Azure のコマンド ライン インターフェイス (CLI) で使用されます。 CLI の使用の詳細については、次を参照してください。 [モバイル サービス テーブルの管理用コマンド](../virtual-machines-command-line-tools.md#Mobile_Tables)します。

##<a name="modifying"></a>方法: モバイル サービスのデータを変更する

次のコードは、同じ ID を持つ既存のインスタンスを新しい情報で更新する方法を示しています。 パラメーターには、更新するデータが .NET オブジェクトとして含まれます。

    await todoTable.UpdateAsync(todoItem);


型指定のないデータを挿入するために、次のような Json.NET を活用することもできます。 更新を行う場合は、更新対象のインスタンスをモバイル サービスが識別できるように、ID を指定する必要があることに注意してしてください。 ID は、`InsertAsync` の呼び出しの結果から取得できます。

    JObject jo = new JObject();
    jo.Add("Id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    jo.Add("Text", "Hello World");
    jo.Add("Complete", false);
    var inserted = await table.UpdateAsync(jo);

"ID" 値を設定せずに項目を更新しようとすると、サービスが更新対象を識別できないため、Mobile Services SDK は `ArgumentException` をスローします。


##<a name="deleting"></a>方法: モバイル サービスのデータを削除する

次のコードは、既存のインスタンスを削除する方法を示しています。 インスタンスは、`todoItem` に設定した "Id" フィールドで識別されます。

    await todoTable.DeleteAsync(todoItem);

型指定のないデータを削除するために、次のような Json.NET を活用することもできます。 削除要求を行う場合は、削除対象のインスタンスをモバイル サービスが識別できるように、ID を指定する必要があることに注意してしてください。 削除要求は ID のみを必要とします。他のプロパティはサービスに渡されません。そのいずれかが渡された場合、サービスに無視されます。 `DeleteAsync` の呼び出しの結果も通常は `null` です。 渡す ID は、`InsertAsync` の呼び出しの結果から取得できます。

    JObject jo = new JObject();
    jo.Add("Id", "37BBF396-11F0-4B39-85C8-B319C729AF6D");
    await table.DeleteAsync(jo);

"ID" フィールドを設定せずに項目を削除しようとすると、サービスが削除対象のインスタンスを識別できないため、サービスから `MobileServiceInvalidOperationException` が返されます。 同様に、"ID" フィールドを設定せずに型指定されていない項目を削除しようとした場合も、サービスから `MobileServiceInvalidOperationException` が返されます。

##<a name="#custom-api"></a>方法: カスタム API の呼び出し

カスタム API を使用してカスタム エンドポイントを定義することにより、insert、update、delete、read のいずれの操作にも関連しないサーバー機能を公開することができます。 カスタム API を使用することによって、HTTP メッセージ ヘッダーの読み取りや設定、JSON 以外のメッセージ本文形式の定義など、メッセージングをより柔軟に制御することができます。 モバイル サービスでカスタム API を作成する方法の例は、次を参照してください。 [方法: カスタム API エンドポイントを定義する](mobile-services-dotnet-backend-define-custom-api.md)です。

いずれかを呼び出すことによってカスタム API を呼び出し、 [InvokeApiAsync] クライアント上のメソッドのオーバー ロードします。 たとえば、次のコード行がへの POST 要求を送信、 **completeAll** のモバイル サービスの API。

    var result = await App.MobileService
        .InvokeApiAsync<MarkAllResult>("completeAll",
        System.Net.Http.HttpMethod.Post, null);

注この型指定されたメソッドを呼び出すことをする必要があります、 **MarkAllResult** 戻り値の型を定義します。 型指定および型指定のないメソッドの両方がサポートされます。 型指定でペイロードを送信せず、クエリ パラメーターがなく、また要求ヘッダーを変更しないため、これは簡単な例になります。 現実的な例と、複数の詳細については [InvokeApiAsync], を参照してください [Custom API in Azure Mobile Services Client SDKs]します。

##方法: プッシュ通知に登録する

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

Xamarin アプリではいくつかの追加コードが必要になります。このコードを使用して、iOS または Android アプリで実行されている Xamarin アプリを Apple Push Notification サービス (APNS) と Google Cloud Messaging (GCM) にそれぞれ登録することができます。 詳細については、次を参照してください **アプリにプッシュ通知を追加** ([Xamarin.iOS](partner-xamarin-mobile-services-ios-get-started-push.md#add-push) | [。Xamarin.Android](partner-xamarin-mobile-services-android-get-started-push.md#add-push))。

>[AZURE.NOTE]特定の登録済みユーザーに通知を送信する必要がある場合が、登録前に認証を要求して、特定のタグを登録するユーザーが承認されていることを確認する重要です。 たとえば、ユーザーが他のユーザーの ID であるタグに登録していないことを確認する必要があります。 詳細については、次を参照してください。 [認証されたユーザーにプッシュ通知の送信](mobile-services-dotnet-backend-windows-store-dotnet-push-notifications-app-users.md)します。

##<a name="pull-notifications"></a>方法: Windows アプリで定期的な通知を使用する

Windows は、ライブ タイルを更新する定期的な通知 (プル通知) をサポートします。 定期的な通知を有効にすると、Windows は [スタート] メニューのアプリケーション タイルを更新するカスタム API エンドポイントに定期的にアクセスします。 定期的な通知を使用する必要があります [カスタム API を定義する](mobile-services-javascript-backend-define-custom-api.md) タイル固有の形式で XML データを返します。 詳細については、次を参照してください。 [定期的な通知](https://msdn.microsoft.com/library/windows/apps/hh761461.aspx)します。

次の例からタイル テンプレート データを要求する定期的な通知をオン、 *タイル* カスタム エンドポイント。

    TileUpdateManager.CreateTileUpdaterForApplication().StartPeriodicUpdate(
        new System.Uri(MobileService.ApplicationUri, "/api/tiles"),
        PeriodicUpdateRecurrence.Hour
    );

選択、 [PeriodicUpdateRecurrance](https://msdn.microsoft.com/library/windows/apps/windows.ui.notifications.periodicupdaterecurrence.aspx) 、データの更新の頻度に最も一致する値。

##<a name="optimisticconcurrency"></a>方法: オプティミスティック同時実行制御を使用する

シナリオによっては、複数のクライアントが同じ項目に対して同時に変更を書き込む場合があります。 競合を検知しない場合、それを意図していなくても、最後に行われた書き込みによってそれ以前の更新がすべて上書きされます。 オプティミスティック同時実行制御では、それぞれのトランザクションがコミットでき、そのためリソース ロックが一切使用されないことを前提としています。 オプティミスティック同時実行制御ではトランザクションをコミットする前に、他のトランザクションがそのデータを変更していないことを確認します。 データが変更されている場合、トランザクションのコミットはロール バックされます。

Mobile Services はオプティミスティック同時実行制御をサポートしており、Mobile Services によって作成された各テーブルに定義されている `__version` システム プロパティ列を使用して各項目の変更を追跡します。 レコードが更新されるたびに、Mobile Services はそのレコードの `__version` プロパティを新しい値に設定します。 各更新要求の際に、要求に含まれているレコードの `__version` プロパティが、サーバー上のレコードの同じプロパティと比較されます。 要求で渡されたバージョンがサーバーと一致しない場合、Mobile Services .NET クライアント ライブラリは `MobileServicePreconditionFailedException<T>` をスローします。 例外に含まれている型は、レコードのサーバー側のバージョンを含んでいるサーバーのレコードです。 アプリケーションはこの情報を使用して、サーバーからの正しい `__version` 値で更新要求をもう一度実行して変更をコミットするかどうかを判断できます。

オプティミスティック同時実行制御を有効にするために、アプリケーションはテーブル クラスに `__version` システム プロパティ用の列を定義しています。 次の定義は、その一例です。

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


型指定のないテーブルを使用しているアプリケーションは、テーブルの `SystemProperties` に次のように `Version` フラグを設定することによって、オプティミスティック同時実行制御を有効にします。

    //Enable optimistic concurrency by retrieving __version
    todoTable.SystemProperties |= MobileServiceSystemProperties.Version;


次のコードは、書き込み競合が検出された場合にそれを解決する方法を示しています。 解決をコミットするには、`UpdateAsync()` 呼び出しに正しい `__version` 値が含まれている必要があります。

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


オプティミスティック同時実行制御を使用して、モバイル サービス向けのより完全な例については、 [Optimistic Concurrency Tutorial]します。


##<a name="binding"></a>方法: モバイル サービス データを Windows ユーザー インターフェイスにバインドする

このセクションでは、Windows アプリで UI 要素を使用して、返されたデータ オブジェクトを表示する方法について説明します。 `todoTable` で未完了の項目を照会し、ごくシンプルな一覧で表示するには、次のコード例を実行して、クエリで一覧のソースをバインドします。 `MobileServiceCollection` を使用すると、モバイル サービス対応のバインディング コレクションが作成されます。

    // This query filters out completed TodoItems.
    MobileServiceCollection<TodoItem, TodoItem> items = await todoTable
        .Where(todoItem => todoItem.Complete == false)
        .ToCollectionAsync();

    // itemsControl is an IEnumerable that could be bound to a UI list control
    IEnumerable itemsControl  = items;

    // Bind this to a ListBox
    ListBox lb = new ListBox();
    lb.ItemsSource = items;

マネージ ランタイムの一部のコントロールと呼ばれるインターフェイスをサポートする [ISupportIncrementalLoading](http://msdn.microsoft.com/library/windows/apps/Hh701916)します。 このインターフェイスにより、コントロールはユーザーによるスクロールの際に追加のデータを要求することができます。 このインターフェイスには、`MobileServiceIncrementalLoadingCollection` によるユニバーサル Windows 8.1 アプリ用の組み込みのサポートが用意されており、コントロールからの呼び出しが自動的に処理されます。 Windows アプリで `MobileServiceIncrementalLoadingCollection` を使用するには、次を実行します。

            MobileServiceIncrementalLoadingCollection<TodoItem,TodoItem> items;
        items =  todoTable.Where(todoItem => todoItem.Complete == false)
                    .ToIncrementalLoadingCollection();

        ListBox lb = new ListBox();
        lb.ItemsSource = items;


Windows Phone 8 と "Silverlight" アプリで新しいコレクションを使用するには、`IMobileServiceTableQuery<T>` や `IMobileServiceTable<T>` で `ToCollection` 拡張メソッドを使用します。 実際にデータを読み込むには、`LoadMoreItemsAsync()` を呼び出します。

    MobileServiceCollection<TodoItem, TodoItem> items = todoTable.Where(todoItem => todoItem.Complete==false).ToCollection();
    await items.LoadMoreItemsAsync();

`ToCollectionAsync` または `ToCollection` を呼び出して作成されたコレクションを使用する場合は、UI コントロールにバインドできるコレクションを取得します。 このコレクションはページングに対応しています。つまり、コントロールが "項目をさらに読み込む" ようにコレクションに依頼し、コレクションがそれを実行します。 その時点で、ユーザー コードは関係せず、コントロールはフローを開始します。 ただし、コレクションはネットワークからデータを読み込むため、読み込みが失敗することもあると想定されます。 このようなエラーを処理するには、`MobileServiceIncrementalLoadingCollection` で `OnException` メソッドをオーバーライドし、コントロールが実行する `LoadMoreItemsAsync` の呼び出しの結果として生じる例外を処理する必要があります。

最後に、テーブルには多くのフィールドが存在するものの、コントロールにはその一部のみを表示する必要があるとします。 セクションで、ガイダンスを使用することがあります"[特定の列を選択](#selecting)"上記を UI に表示する特定の列を選択します。

##<a name="authentication"></a>方法: ユーザーを認証する

Mobile Services は、Facebook、Google、Microsoft アカウント、Twitter、Azure Active Directory などのさまざまな外部 ID プロバイダーを使用したアプリケーション ユーザーの認証と承認をサポートします。 テーブルのアクセス許可を設定することにより、特定の操作へのアクセスを認証されたユーザーのみに制限できます。 さらに、認証されたユーザーの ID を使用することにより、サーバー スクリプトで承認ルールを実装することもできます。 詳細については、このチュートリアルを参照してください。 [Add authentication to your app]します。

2 つの認証フローがサポート: _サーバー フロー_ と _クライアント フロー_します。 サーバー フローには、プロバイダーの Web 認証のインターフェイスを利用する、最も簡単な認証方法が用意されています。 クライアント フローでは、プロバイダー固有とデバイス固有の SDK を利用することから、デバイス固有の機能との統合がさらに進みます。

###サーバー フロー
モバイル サービス Windows アプリの認証プロセスを管理するには
アプリケーションを ID プロバイダーに登録する必要があります。 その後、モバイル サービス内で、プロバイダーから提供されたアプリケーション ID とシークレットを構成する必要があります。 詳細については、このチュートリアルを参照してください。 [Add authentication to your app]します。

Id プロバイダーが登録されると、単に呼び出す、 [LoginAsync method] で、 [MobileServiceAuthenticationProvider] プロバイダーの値。 たとえば、次のコードは、Facebook を使用してサーバー フローのサインインを開始します。

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

Facebook 以外の id プロバイダーを使用している場合の値を変更 [MobileServiceAuthenticationProvider] 上、プロバイダーの値にします。

この場合、Mobile Services は、選択されたプロバイダーのサインイン ページを表示し、ID プロバイダーでのサインインが成功した後で Mobile Services 認証トークンを生成することで、OAuth 2.0 認証フローを管理します。  [LoginAsync method] 返します、 [MobileServiceUser], 、両方を提供する、 [userId] 認証されたユーザーの [MobileServiceAuthenticationToken], 、JSON web トークン (JWT) として。 このトークンはキャッシュして、期限が切れるまで再利用することができます。 詳細については、次を参照してください。 [Caching the authentication token]します。

###クライアント フロー

アプリケーションは個別に ID プロバイダーにアクセスして、返されたトークンを認証のために Mobile Services に提供することもできます。 このクライアント フローでは、ユーザーにシングル サインイン エクスペリエンスを提供したり、ID プロバイダーから追加のユーザー データを取得したりすることができます。

####Facebook や Google から取得したトークンを利用したシングル サインイン

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


####Microsoft アカウントと Live SDK を利用したシングル サインイン

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


###<a name="caching"></a>認証トークンをキャッシュする
場合によっては、最初のユーザー認証の後の login メソッドの呼び出しを避けることができます。 使用する [PasswordVault] を最初に現在のユーザー id をキャッシュする Windows ストア アプリのログインにし、それ以降、キャッシュにユーザー id がされているかどうかをチェックします。 キャッシュが空の場合は、ユーザーにログイン プロセスを実行してもらう必要があります。

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


Windows Phone アプリの場合に、暗号化して使用してデータをキャッシュすることが、、 [ProtectedData] クラスし、分離ストレージに機密情報を保存します。

##<a name="errors"></a>方法: エラーを処理する

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

##<a name="untyped"></a>方法: 型指定のないデータを処理する

.NET クライアントは厳密に型指定されたシナリオ向けに設計されます。 ただし、大まかに型指定されたエクスペリエンスの方が便利であることもあります。たとえば、オープン スキーマでオブジェクトを処理する場合です。 このシナリオには次のように対応できます。 クエリでは、LINQ を使用せずに、ワイヤ形式を使用します。

    // Get an untyped table reference
    IMobileServiceTable untypedTodoTable = client.GetTable("TodoItem");

    // Lookup untyped data using OData
    JToken untypedItems = await untypedTodoTable.ReadAsync("$filter=complete eq 0&$orderby=text");

プロパティ バッグのように使用できる JSON 値が返されます。 JToken と Json.NET の詳細については、次を参照してください [Json.NET。](http://json.codeplex.com/)

##<a name="unit-testing"></a>方法: 単体テストを設計する

`MobileServiceClient.GetTable` とクエリによって返される値はインターフェイスです。 そのためテスト目的で簡単にモックを作成することができるので、テスト ロジックを実装する `MyMockTable : IMobileServiceTable<TodoItem>` を作成できます。

##<a name="customizing"></a>方法: クライアントをカスタマイズする

このセクションでは、要求ヘッダーをカスタマイズし、応答内の JSON オブジェクトのシリアル化をカスタマイズする方法について説明します。

### <a name="headers"></a>方法: 要求ヘッダーをカスタマイズする

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

このコードは、新しい追加 **x マイ ヘッダー** 要求や任意にセットのヘッダーに対する、応答コードを使用できません。 実際のシナリオでは、アプリケーションで必要とされるカスタム ロジックに基づいて応答ステータス コードを設定します。

### <a name="serialization"></a>方法: シリアル化をカスタマイズする

Mobile Services クライアント ライブラリは、Json.NET を使用して、クライアントで JSON 応答を .NET オブジェクトに変換します。 メッセージでの .NET 型と JSON との間のこのシリアル化の動作を構成することができます。  [MobileServiceClient](http://msdn.microsoft.com/library/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx) クラスが公開、 `SerializerSettings` 型のプロパティ [JsonSerializerSettings](http://james.newtonking.com/projects/json/help/?topic=html/T_Newtonsoft_Json_JsonSerializerSettings.htm)

このプロパティを使用すれば、次のように、多くの Json.NET プロパティの中からいずれかを設定できます。

    var settings = new JsonSerializerSettings();
    settings.ContractResolver = new CamelCasePropertyNamesContractResolver();
    client.SerializerSettings = settings;

シリアル化中、このプロパティによってすべてのプロパティが小文字に変換されます。

<!-- Anchors. -->
[What is Mobile Services]: #what-is
[Concepts]: #concepts
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #instantiating
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[Look up data by ID]: #lookingup
[How to: Bind data to user interface in a mobile service]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Delete data in a mobile service]: #deleting
[How to: Use Optimistic Concurrency]: #optimisticconcurrency
[How to: Authenticate users]: #authentication
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Query data from a mobile service]: #querying
[How to: Customize the client]: #customizing
[How to: Work with untyped data]: #untyped
[Customize request headers]: #headers
[Customize serialization]: #serialization
[Next steps]: #nextsteps
[Caching the authentication token]: #caching
[How to: Call a custom API]: #custom-api

<!-- Images. -->



<!-- URLs. -->
[Add authentication to your app]: mobile-services-dotnet-backend-windows-universal-dotnet-get-started-users.md
[PasswordVault]: http://msdn.microsoft.com/library/windows/apps/windows.security.credentials.passwordvault.aspx
[ProtectedData]: http://msdn.microsoft.com/library/system.security.cryptography.protecteddata%28VS.95%29.aspx
[LoginAsync method]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceclientextensions.loginasync.aspx
[MobileServiceAuthenticationProvider]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceauthenticationprovider.aspx
[MobileServiceUser]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.aspx
[UserID]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.userid.aspx
[MobileServiceAuthenticationToken]: http://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.mobileservices.mobileserviceuser.mobileserviceauthenticationtoken.aspx
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: ../virtual-machines-command-line-tools.md/#Commands_to_manage_mobile_services
[Optimistic Concurrency Tutorial]: mobile-services-windows-store-dotnet-handle-database-conflicts.md
[MobileServiceClient]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.aspx

[IncludeTotalCount]: http://msdn.microsoft.com/library/windowsazure/dn250560.aspx
[Skip]: http://msdn.microsoft.com/library/windowsazure/dn250573.aspx
[Take]: http://msdn.microsoft.com/library/windowsazure/dn250574.aspx
[Fiddler]: http://www.telerik.com/fiddler
[Custom API in Azure Mobile Services Client SDKs]: http://blogs.msdn.com/b/carlosfigueira/archive/2013/06/19/custom-api-in-azure-mobile-services-client-sdks.aspx
[InvokeApiAsync]: http://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.invokeapiasync.aspx


