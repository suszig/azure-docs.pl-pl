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

このチュートリアルの完了を必要と、[モバイル サービスの開始を取得] チュートリアルです。 Azure ストレージ アカウントも必要になります。

## .NET バックエンド モバイル サービスでの Azure テーブル ストレージの構成

まず、Azure Storage に接続できるよう、モバイル サービスと .NET バックエンド コード プロジェクトを構成する必要があります。

1. Visual Studio の**ソリューション エクスプローラー**で、.NET バックエンド プロジェクトを右クリックし、[**NuGet パッケージの管理**] をクリックします。

2. 左側のウィンドウで、[**オンライン**] カテゴリ、[**安定しているリリースのみ**] の順にクリックし、[**MobileServices**] を検索します。[**Microsoft Azure Mobile Services .NET バックエンド Azure Storage 拡張機能**] パッケージの [**インストール**] をクリックして、使用許諾契約に同意します。

    ![](./media/mobile-services-dotnet-backend-store-data-table-storage/mobile-add-storage-nuget-package-dotnet.png)

    これによって、Azure ストレージ サービス用のサポートが、モバイル サービス プロジェクトに追加されます。

3. まだストレージ アカウントを作成していない場合は、次を参照してください。 [をストレージ アカウントを作成する方法](../storage-create-storage-account.md)します。

4. [Azure クラシック ポータル]、[クリックして **ストレージ**, ストレージ アカウントをクリックし、クリックして **キーの管理**します。

5. **Storage アカウント名**と**アクセス キー**をメモします。

6. モバイル サービス] をクリックして、 **構成** ] タブで、下へスクロールして **接続文字列** 新しい接続文字列を入力し、 **名** の `StorageConnectionString` と **値** は、ストレージ アカウント接続文字列で、次の形式です。

        DefaultEndpointsProtocol=https;AccountName=<ACCOUNT_NAME>;AccountKey=<ACCESS_KEY>;

    ![](./media/mobile-services-dotnet-backend-store-data-table-storage/mobile-blob-storage-app-settings.png)

7. 上記の文字列での値を置き換える `< ACCOUNT_NAME >` と `< ACCESS_KEY >` ポータルからの値をクリックし、 **保存**します。

    ストレージ アカウント接続文字列は、暗号化されてアプリ設定に格納されます。 この文字列は、実行時に任意のテーブル コント ローラーで表示できます。

8. Visual Studio のソリューション エクスプ ローラーで、モバイル サービス プロジェクトの Web.config ファイルを開き、次の新しい接続文字列を追加します。

        <add name="StorageConnectionString" connectionString="<STORAGE_CONNECTION_STRING>" />

9. 置き換える、 `< STORAGE_CONNECTION_STRING >` 手順 6 で接続文字列のプレース ホルダーです。

    モバイル サービスがローカル コンピューター上で実行されるときにこの接続文字列が使用されるため、この文字列を発行前にテストできます。 Azure で実行される場合、モバイル サービスにはポータルで設定された接続文字列の値が使用され、プロジェクト内の接続文字列は無視されます。

## <a name="modify-service"></a>データ型とテーブル コント ローラーを変更します。

TodoList のクイック スタート プロジェクトは、Entity Framework を使用して SQL Database にアクセスするよう設計されているため、テーブル ストレージで作業できるようにプロジェクトを更新する必要があります。

1. **TodoItem** データ型を、**EntityData** からではなく、**StorageData** から派生するよう、次のように変更します。

        public class TodoItem : StorageData
        {
            public string Text { get; set; }
            public bool Complete { get; set; }
        }

    >[AZURE.NOTE]**StorageData** 型には ID プロパティが含まれ、このプロパティには *partitionId*,*rowValue* という形式の文字列の複合キーを入力する必要があります。

2. **TodoItemController** で、次の using ステートメントを追加します。

        using System.Web.Http.OData.Query;
        using System.Collections.Generic;

3. **TodoItemController** の **Initialize** メソッドを次のコードで置き換えます。

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

3. 既存の **GetAllTodoItems** メソッドを次のコードで置き換えます。

        public Task<IEnumerable<TodoItem>> GetAllTodoItems(ODataQueryOptions options)
        {
            // Call QueryAsync, passing the supplied query options.
            return DomainManager.QueryAsync(options);
        }

    SQL データベースとは異なり、このバージョンが IQueryable を返すしない<TEntity>, であるために、結果をバインドすることができますが、返しませんが、クエリ内で、します。

## クライアント アプリの更新

テーブル ストレージを使用して、.NET バックエンドがクイック スタート アプリケーションで使用できるようにするには、クライアント側に変更を 1 つ加える必要があります。 これは、テーブル ストレージ プロバイダーで複合キーが求められるからです。

1. データ アクセス コードを含むクライアント コード ファイルを開き、挿入操作を実行するメソッドを検索します。

2. 文字列の形式で Id フィールドを明示的に設定に追加される TodoItem インスタンスを更新 `< partitionID > < rowValue >`します。

    これは、この ID の C# アプリでの設定例で、ここではパーティションの部分は固定されており、行の部分は GUID ベースに設定されています。

         todoItem.Id = string.Format("partition,{0}", Guid.NewGuid());


これで、アプリをテストする準備ができました。

## <a name="test-application"></a>アプリケーションをテストします。

1. モバイル サービスの .NET バックエンド  プロジェクトを再発行します (省略可能)。

    .NET バックエンド プロジェクトを Azure に発行する前に、モバイル サービスをローカルでテストすることもできます。 ローカルまたは Azure でテストするかどうかにかかわらず、モバイル サービスでは Azure テーブル ストレージが使用されます。

4. モバイル サービスに接続されている、クイック スタート クライアント アプリを実行します。

    クイック スタート チュートリアルに従って以前に追加した項目は、表示されないことに注意してください。 これは、現時点でテーブル ストアが空だからです。

5. 新しい項目を追加して、データベースの変更を生成します。

    アプリとモバイル サービスは以前と同様に動作しますが、データが格納される場所が SQL Database ではなく非リレーショナル ストアになったという点が異なります。

## 次のステップ

ここまでで、テーブル ストレージを .NET バックエンドに使用するのがいかに簡単かを説明したので、次に示すその他のバックエンド ストレージ オプションの使用も検討してみてください。

+ [ハイブリッド接続を使用して内部設置型 SQL Server に接続](mobile-services-dotnet-backend-hybrid-connections-get-started.md)</br>ハイブリッド接続により、モバイル サービスをオンプレミスのアセットに安全に接続します。この方法で、モバイル クライアントが Azure を使用することによってオンプレミスのデータにアクセスできるようにします。サポートされている資産には、Microsoft SQL Server、MySQL、HTTP Web APIs、およびほとんどのカスタム Web サービスなど、静的 TCP ポートで実行されるすべてのリソースが含まれます。

+ [Mobile Services を使用して Azure ストレージにイメージのアップロード](mobile-services-dotnet-backend-windows-store-dotnet-upload-data-blob-storage.md)</br>アプリから Azure Blob ストレージにイメージをアップロードする TodoList サンプル プロジェクトを拡張する方法を示します。







[create a non-relational store]: #create-store 
[modify data and controllers]: #modify-service 
[test the application]: #test-application 
[get started with mobile services]: mobile-services-dotnet-backend-windows-store-dotnet-get-started.md 
[azure classic portal]: https://manage.windowsazure.com/ 
[what is the table service]: ../storage-dotnet-how-to-use-tables.md#what-is 
[mongolab add-on page]: /gallery/store/mongolab/mongolab 

