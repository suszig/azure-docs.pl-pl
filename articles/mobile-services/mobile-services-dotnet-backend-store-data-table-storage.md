<properties
    pageTitle="テーブル ストレージを使用する .NET バックエンド モバイル サービスの作成 | Azure Mobile Services"
    description=".NET バックエンド モバイル サービスに Azure テーブル ストレージを使用する方法について学習します。"
    services="mobile-services"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="12/11/2015"
    ms.author="glenga"/>

# テーブル ストレージを使用する .NET バックエンド モバイル サービスの作成

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


このトピックでは、.NET バックエンド モバイル サービスで非リレーショナル データ ストアを使用する方法を説明します。 このチュートリアルでは、既定の Azure SQL Database のデータ ストアではなく Azure テーブル ストレージが使用されるように、Azure Mobile Services のクイック スタート プロジェクトを変更します。

このチュートリアルの完了が必要です、 [Get started with Mobile Services] チュートリアルです。 Azure ストレージ アカウントも必要になります。

##.NET バックエンド モバイル サービスでの Azure テーブル ストレージの構成

まず、Azure Storage に接続できるよう、モバイル サービスと .NET バックエンド コード プロジェクトを構成する必要があります。

1.  **ソリューション エクスプ ローラー** Visual Studio では、.NET バックエンド プロジェクトを右クリックして [ **NuGet パッケージの管理**します。

2. 左ペインで選択、 **オンライン** カテゴリで、 **安定しているリリースのみ**, 、検索 **MobileServices**, 、] をクリックして **インストール** 上、 **Microsoft Azure Mobile Services .NET バックエンド Azure Storage 拡張機能** をパッケージ化し、使用許諾契約に同意します。

    ![](./media/mobile-services-dotnet-backend-store-data-table-storage/mobile-add-storage-nuget-package-dotnet.png)

    これによって、Azure ストレージ サービス用のサポートが、モバイル サービス プロジェクトに追加されます。

3. まだストレージ アカウントを作成していない場合は、次を参照してください。 [をストレージ アカウントを作成する方法](../storage-create-storage-account.md)します。

4.  [Azure classic portal], をクリックして **ストレージ**, ストレージ アカウントをクリックし、クリックして **キーの管理**します。

5. 書き留めて、 **ストレージ アカウント名** と **アクセス キー**します。

6. モバイル サービス] をクリックして、 **構成** ] タブで、下へスクロールして **接続文字列** 新しい接続文字列を入力し、 **名前** の `StorageConnectionString` と **値** は、ストレージ アカウント接続文字列で、次の形式です。

        DefaultEndpointsProtocol=https;AccountName=<ACCOUNT_NAME>;AccountKey=<ACCESS_KEY>;

    ![](./media/mobile-services-dotnet-backend-store-data-table-storage/mobile-blob-storage-app-settings.png)

7. 上記の文字列での値を置き換える `<ACCOUNT_NAME>` と `<ACCESS_KEY>` ポータルからの値をクリックし、 **保存**します。

    ストレージ アカウント接続文字列は、暗号化されてアプリ設定に格納されます。 この文字列は、実行時に任意のテーブル コント ローラーで表示できます。

8. Visual Studio のソリューション エクスプ ローラーで、モバイル サービス プロジェクトの Web.config ファイルを開き、次の新しい接続文字列を追加します。

        <add name="StorageConnectionString" connectionString="<STORAGE_CONNECTION_STRING>" />

9. `<STORAGE_CONNECTION_STRING>` プレース ホルダーを、手順 6 で使用した接続文字列で置き換えます。

    モバイル サービスがローカル コンピューター上で実行されるときにこの接続文字列が使用されるため、この文字列を発行前にテストできます。 Azure で実行される場合、モバイル サービスにはポータルで設定された接続文字列の値が使用され、プロジェクト内の接続文字列は無視されます。

## <a name="modify-service"></a>データ型とテーブル コント ローラーの変更

TodoList のクイック スタート プロジェクトは、Entity Framework を使用して SQL Database にアクセスするよう設計されているため、テーブル ストレージで作業できるようにプロジェクトを更新する必要があります。

1. 変更、 **TodoItem** データ型から派生する **StorageData** の代わりに **EntityData**, 、次のようにします。

        public class TodoItem : StorageData
        {
            public string Text { get; set; }
            public bool Complete { get; set; }
        }

    >[AZURE.NOTE] **StorageData** 型形式の文字列である複合キーを必要とする Id プロパティには *partitionId*,、*rowValue*します。

2.  **TodoItemController**, 、次の追加ステートメントを使用します。

        using System.Web.Http.OData.Query;
        using System.Collections.Generic;

3. 置き換える、 **初期化** のメソッド、 **TodoItemController** を次です。

        protected override void Initialize(HttpControllerContext controllerContext)
        {
            base.Initialize(controllerContext);

            // Create a new Azure Storage domain manager using the stored
            // connection string and the name of the table exposed by the controller.
            string connectionStringName = "StorageConnectionString";
            var tableName = controllerContext.ControllerDescriptor.ControllerName.ToLowerInvariant();
            DomainManager = new StorageDomainManager<TodoItem>(connectionStringName,
                tableName, Request, Services);
        }

    これで、ストレージ アカウントの接続文字列を使用して、要求されたコント ローラーの新しいストレージ ドメイン マネージャーが作成されます。

3. 既存 **GetAllTodoItems** メソッドを次のコードです。

        public Task<IEnumerable<TodoItem>> GetAllTodoItems(ODataQueryOptions options)
        {
            // Call QueryAsync, passing the supplied query options.
            return DomainManager.QueryAsync(options);
        }

    SQL データベースとは異なり、このバージョンが IQueryable を返すしません。<TEntity>、結果は、にバインドすることができますが、返しませんが、クエリ内でようにします。

## クライアント アプリの更新

テーブル ストレージを使用して、.NET バックエンドがクイック スタート アプリケーションで使用できるようにするには、クライアント側に変更を 1 つ加える必要があります。 これは、テーブル ストレージ プロバイダーで複合キーが求められるからです。

1. データ アクセス コードを含むクライアント コード ファイルを開き、挿入操作を実行するメソッドを検索します。

2. ID フィールドが明示的に文字列形式 `<partitionID>,<rowValue>` で設定されるように、追加された TodoItem インスタンスを更新します。

    これは、この ID の C# アプリでの設定例で、ここではパーティションの部分は固定されており、行の部分は GUID ベースに設定されています。

         todoItem.Id = string.Format("partition,{0}", Guid.NewGuid());

これで、アプリをテストする準備ができました。

## <a name="test-application"></a>アプリケーションをテストする

1. モバイル サービスの .NET バックエンド  プロジェクトを再発行します (省略可能)。

    .NET バックエンド プロジェクトを Azure に発行する前に、モバイル サービスをローカルでテストすることもできます。 ローカルまたは Azure でテストするかどうかにかかわらず、モバイル サービスでは Azure テーブル ストレージが使用されます。

4. モバイル サービスに接続されている、クイック スタート クライアント アプリを実行します。

    クイック スタート チュートリアルに従って以前に追加した項目は、表示されないことに注意してください。 これは、現時点でテーブル ストアが空だからです。

5. 新しい項目を追加して、データベースの変更を生成します。

    アプリとモバイル サービスは以前と同様に動作しますが、データが格納される場所が SQL Database ではなく非リレーショナル ストアになったという点が異なります。

##次のステップ

ここまでで、テーブル ストレージを .NET バックエンドに使用するのがいかに簡単かを説明したので、次に示すその他のバックエンド ストレージ オプションの使用も検討してみてください。

+ [ハイブリッド接続を使用して内部設置型 SQL Server に接続します。](mobile-services-dotnet-backend-hybrid-connections-get-started.md)</br>ハイブリッド接続により、モバイル サービスに安全にオンプレミスのアセットに接続します。 この方法で、モバイル クライアントが Azure を使用することによってオンプレミスのデータにアクセスできるようにします。 サポートされている資産には、Microsoft SQL Server、MySQL、HTTP Web APIs、およびほとんどのカスタム Web サービスなど、静的 TCP ポートで実行されるすべてのリソースが含まれます。

+ [Mobile Services を使用して Azure ストレージにイメージをアップロードします。](mobile-services-dotnet-backend-windows-store-dotnet-upload-data-blob-storage.md)</br>アプリから Azure Blob ストレージにイメージをアップロードする TodoList サンプル プロジェクトを拡張する方法を示します。

<!-- Anchors. -->
[Create a non-relational store]: #create-store
[Modify data and controllers]: #modify-service
[Test the application]: #test-application


<!-- Images. -->


<!-- URLs. -->
[Get started with Mobile Services]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md
[Azure classic portal]: https://manage.windowsazure.com/
[What is the Table Service]: ../storage-dotnet-how-to-use-tables.md#what-is
[MongoLab Add-on Page]: /gallery/store/mongolab/mongolab


