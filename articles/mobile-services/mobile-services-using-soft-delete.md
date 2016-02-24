<properties
    pageTitle="Mobile Services での論理削除の使用方法 (Windows ストア) | Microsoft Azure"
    description="アプリケーション内で Azure Mobile Services の論理削除機能を使用する方法を説明します。"
    documentationCenter=""
    authors="wesmc7777"
    manager="dwrede"
    editor=""
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="12/07/2015"
    ms.author="wesmc"/>

# Mobile Services での論理削除の使用方法

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


##概要

JavaScript または .NET バックエンドで作成されたテーブルは、オプションで論理削除を有効にすることができます。 論理削除を使用する場合、新しい列と呼ばれる *\__deleted* [SQL bit 型] のデータベースに追加されます。 論理削除を有効にすると、削除操作では列をデータベースから物理的に削除しませんが、deleted 列の値が TRUE に設定されます。

論理削除が有効なテーブルのレコードをクエリした場合、既定では、クエリで削除済みの行が返されることはありません。 これらの行を要求するのには、クエリ パラメーターを渡す必要があります *\__includeDeleted=true* で、 [REST クエリ操作](http://msdn.microsoft.com/library/azure/jj677199.aspx)します。 .NET クライアント SDK では、ヘルパー メソッドの `IMobileServiceTable.IncludeDeleted()` を使用することもできます。

.NET バックエンド対応の論理削除サポートは、最初に、Microsoft Azure Mobile Services .NET バックエンドのバージョン 1.0.402 でリリースされました。 最新の NuGet パッケージはここでは、使用可能な [Microsoft Azure Mobile Services .NET バックエンド](http://go.microsoft.com/fwlink/?LinkId=513165)します。


論理削除の使用において潜在するメリットは次のとおりです。

* [オフライン データの Mobile Services 用の同期] 機能を使用する場合、クライアント SDK に自動的に削除されたレコードのクエリを実行し、ローカルのデータベースから削除されます。 論理削除が有効でない場合は、クライアント SDK がローカル ストアからどのレコードを削除するか認識できるように、バックエンドに追加コードを記述する必要があります。 記述しないと、クライアント ローカル ストアとバックエンドの間でこれらの削除済みレコードに関して矛盾が生じるため、クライアント メソッド `PurgeAsync()` を呼び出して、ローカル ストアを消去する必要があります。
* アプリケーションによっては、データを決して物理的に削除しない、または監査の後のみデータを削除するなどのビジネス要件があります。 論理削除機能は、このようなシナリオで役立ちます。
* 論理削除を使用して "削除の取り消し" 機能を実装することにより、誤って削除されたデータを回復できます。
ただし、論理削除済みのレコードはデータベース内の領域を占有するので、スケジュールされたジョブを作成して、論理削除済みのレコードを一定期間ごとに物理的に削除することを検討する必要があります。 この例については、「.NET バックエンドでの論理削除の使用方法」と「JavaScript バックエンドでの論理削除の使用方法」をご覧ください。 また、これらの物理的に削除されたレコードがデバイスのローカル データ ストアに残存しないように、クライアント コードで定期的に `PurgeAsync()` を呼び出す必要があります。





##.NET バックエンドの論理削除を有効にする

.NET バックエンド対応の論理削除サポートは、最初に、Microsoft Azure Mobile Services .NET バックエンドのバージョン 1.0.402 でリリースされました。 最新の NuGet パッケージはここでは、使用可能な [Microsoft Azure Mobile Services .NET バックエンド](http://go.microsoft.com/fwlink/?LinkId=513165)します。

次の手順は、.NET バックエンド モバイル サービスの論理削除を有効にする方法を示しています。

1. Visual Studio で、.NET バックエンド モバイル サービス プロジェクトを開きます。
2. .NET バックエンド プロジェクトを右クリックし、クリックして **NuGet パッケージの管理**します。
3. パッケージ マネージャー ダイアログ ボックスで [ **Nuget.org** [更新プログラムとインストールのバージョン 1.0.402 または以降の [Microsoft Azure Mobile Services .NET バックエンド](http://go.microsoft.com/fwlink/?LinkId=513165) NuGet パッケージの管理。
3. Visual Studio のソリューション エクスプ ローラーで、 **コント ローラー** のノードでは、.NET バックエンド プロジェクトの下にあるし、開く、コント ローラーのソースします。 たとえば、 *TodoItemController.cs*します。
4. コントローラーの `Initialize()` メソッドで、`enableSoftDelete: true` パラメーターを EntityDomainManager コンストラクターに渡します。

        protected override void Initialize(HttpControllerContext controllerContext)
        {
            base.Initialize(controllerContext);
            MobileService1Context context = new MobileService1Context();
            DomainManager = new EntityDomainManager<TodoItem>(context, Request, Services, enableSoftDelete: true);
        }


##JavaScript バックエンドの論理削除を有効にする

モバイル サービス用の新しいテーブルを作成している場合は、テーブル作成ページで論理削除を有効にすることができます。

![][2]

JavaScript バックエンドの既存のテーブルで論理削除を有効にするには、次の手順に従います。

1. [Azure クラシック ポータル]、[モバイル サービスをクリックします。 その後、[データ] タブをクリックします。
2. データ ページで、目的のテーブルをクリックして選択します。 をクリックし、 **論理削除を有効にする** コマンド バーのボタンをクリックします。 テーブルは、論理削除を有効になっているを既に持っている場合は、このボタンは表示されませんが、表示することができます、 *\__deleted* 列をクリックすると、 **参照** または **列** テーブルのタブをクリックします。

    ![][0]

    テーブルの論理削除を無効にする] をクリックして、 **列** ] タブをクリックして、 *\__deleted* 列と **削除** ] ボタンをクリックします。

    ![][1]

## <a name="using-with-dotnet"></a>.NET バックエンドでの論理削除の使用方法


次のスケジュールされたジョブは、1 か月を超えた論理削除済みのレコードを消去します。

    public class SampleJob : ScheduledJob
    {
        private MobileService1Context context;

        protected override void Initialize(ScheduledJobDescriptor scheduledJobDescriptor,
            CancellationToken cancellationToken)
        {
            base.Initialize(scheduledJobDescriptor, cancellationToken);
            context = new MobileService1Context();
        }

        public override Task ExecuteAsync()
        {
            Services.Log.Info("Purging old records");
            var monthAgo = DateTimeOffset.UtcNow.AddDays(-30);

            var toDelete = context.TodoItems.Where(x => x.Deleted == true && x.UpdatedAt <= monthAgo).ToArray();
            context.TodoItems.RemoveRange(toDelete);
            context.SaveChanges();

            return Task.FromResult(true);
        }
    }

.NET バックエンド モバイル サービスでジョブのスケジュールの詳細を参照してください: [JavaScript バックエンド Mobile Services での定期的なジョブのスケジュール](mobile-services-dotnet-backend-schedule-recurring-tasks.md)




##JavaScript バックエンドでの論理削除の使用方法

テーブル スクリプトを使用して、JavaScript バックエンド Mobile Services での論理削除機能に関連するロジックを追加します。

削除の取り消し要求を検出するには、更新テーブル スクリプトで "undelete" プロパティを使用します。

    function update(item, user, request) {
        if (request.undelete) { /* any undelete specific code */; }
    }
削除済みレコードをスクリプトのクエリ結果に含めるには、"includeDeleted" パラメーターを true に設定します。

    tables.getTable('softdelete_scenarios').read({
        includeDeleted: true,
        success: function (results) {
            request.respond(200, results)
        }
    });

HTTP 要求を介して削除済みレコードを取得するには、"__includedeleted=true" クエリ パラメーターを追加します。

    http://youservice.azure-mobile.net/tables/todoitem?__includedeleted=true

### 論理削除済みレコードを消去するスケジュールされたジョブのサンプル

これは、特定の日付より前に更新されたレコードを削除するスケジュールされたジョブのサンプルです。

    function purgedeleted() {
         mssql.query('DELETE FROM softdelete WHERE __deleted=1', {
            success: function(results) {
                console.log(results);
            },
            error: function(err) {
                console.log("error is: " + err);
        }});
    }

JavaScript バックエンド モバイル サービスでスケジュールされたジョブの詳細についてを参照してください: [JavaScript バックエンド Mobile Services での定期的なジョブのスケジュール](mobile-services-schedule-recurring-tasks.md)します。





<!-- Images -->
[0]: ./media/mobile-services-using-soft-delete/enable-soft-delete-button.png
[1]: ./media/mobile-services-using-soft-delete/disable-soft-delete.png
[2]: ./media/mobile-services-using-soft-delete/enable-soft-delete-new-table.png

<!-- URLs. -->
[SQL bit type]: http://msdn.microsoft.com/library/ms177603.aspx
[Offline data Sync for Mobile Services]: mobile-services-windows-store-dotnet-get-started-offline-data.md
[Azure classic portal]: https://manage.windowsazure.com/



