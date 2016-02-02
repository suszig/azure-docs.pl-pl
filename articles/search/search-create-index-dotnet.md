<properties
    pageTitle=".NET を使用して Azure Search インデックスの作成 |Microsoft Azure |ホスト型クラウド検索サービス"
    description="Azure Search .NET SDK またはライブラリを使用して、コードでインデックスを作成します。"
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="11/09/2015"
    ms.author="heidist"/>


# .NET を使用した Azure Search インデックスの作成

> [AZURE.SELECTOR]
- [Overview](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST](search-create-index-rest-api.md)


この記事には、使用してインデックスを作成する方法がでは、 [Azure Search .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)します。 次の内容のサブセットである、 [.NET アプリケーションから Azure Search の使用方法](search-howto-dotnet-sdk.md)します。 エンド ツー エンドの手順については、元の記事を参照してください。

インデックスの作成の前提条件を含めるを使用して行う通常の検索サービスとの接続を確立したこと、 `SearchServiceClient`します。 スムーズに再デプロイできるように、同じ名前のインデックスが既に存在する場合はそれを削除することをお勧めします。

"Hotels"という名前のインデックスにある場合を構築できますメソッドの次のようになります。

    private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
    {
        if (serviceClient.Indexes.Exists("hotels"))
        {
            serviceClient.Indexes.Delete("hotels");
        }
    }

このメソッドを使用して、指定された `SearchServiceClient` 確認するには、インデックスが存在する場合は、そして管理者であればそれを削除します。

新しい "hotels" インデックスを作成するには、次のようなメソッドを作成します。

    private static void CreateHotelsIndex(SearchServiceClient serviceClient)
    {
        var definition = new Index()
        {
            Name = "hotels",
            Fields = new[]
            {
                new Field("hotelId", DataType.String)                       { IsKey = true },
                new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true },
                new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true },
                new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
                new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
                new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
                new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
            }
        };
    
        serviceClient.Indexes.Create(definition);
    }

このメソッドが、新たに作成 `インデックス` オブジェクトのリストで `フィールド` 、新しいインデックスのスキーマを定義するオブジェクト。 各フィールドには、名前、データ型、および検索動作を定義するいくつかの属性があります。 フィールドに加えて、スコアリング プロファイル、サジェスター、または CORS オプションを Index に追加することもできます (簡潔さを優先し、サンプルではこれらは省略されています)。 上の詳細については、Index オブジェクトと、SDK の参照を使用してその構成要素を見つけることができます [MSDN](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.index_members.aspx), などでも、 [Azure Search REST API リファレンス](https://msdn.microsoft.com/library/azure/dn798935.aspx)します。




