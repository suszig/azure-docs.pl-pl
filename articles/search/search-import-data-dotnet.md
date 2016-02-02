<properties
    pageTitle=".NET を使用して Azure Search にデータをインポート |Microsoft Azure |ホスト型クラウド検索サービス"
    description=".NET SDK または .NET ライブラリを使用して Azure Search インデックスにデータをアップロードする方法"
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="11/09/2015"
    ms.author="heidist"/>


# .NET を使用して Azure Search にデータをインポートします。

> [AZURE.SELECTOR]
- [Overview](search-what-is-data-import.md)
- [Portal](search-import-data-portal.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)
- [Indexers](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)


ここで説明を使用して、インデックスを設定する方法について、 [Azure Search .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)します。 次の内容のサブセットである、 [.NET アプリケーションから Azure Search の使用方法](search-howto-dotnet-sdk.md)します。 エンド ツー エンドの手順については、元の記事を参照してください。

データをインポートする前提条件として、インデックスが事前に作成されている必要があります。

"hotels" という名前のインデックスがあると仮定すると、データをインポートするためのメソッドを次のように作成できます。

次の手順で `Main` は新しく作成されたインデックスを設定します。 この処理は次のメソッドで行います。

    private static void UploadDocuments(SearchIndexClient indexClient)
    {
        var documents =
            new Hotel[]
            {
                new Hotel()
                { 
                    HotelId = "1058-441", 
                    HotelName = "Fancy Stay", 
                    BaseRate = 199.0, 
                    Category = "Luxury", 
                    Tags = new[] { "pool", "view", "concierge" }, 
                    ParkingIncluded = false, 
                    LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero), 
                    Rating = 5, 
                    Location = GeographyPoint.Create(47.678581, -122.131577)
                },
                new Hotel()
                { 
                    HotelId = "666-437", 
                    HotelName = "Roach Motel",
                    BaseRate = 79.99,
                    Category = "Budget",
                    Tags = new[] { "motel", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                    Rating = 1,
                    Location = GeographyPoint.Create(49.678581, -122.131577)
                },
                new Hotel() 
                { 
                    HotelId = "970-501", 
                    HotelName = "Econo-Stay",
                    BaseRate = 129.99,
                    Category = "Budget",
                    Tags = new[] { "pool", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4,
                    Location = GeographyPoint.Create(46.678581, -122.131577)
                },
                new Hotel()
                { 
                    HotelId = "956-532", 
                    HotelName = "Express Rooms",
                    BaseRate = 129.99,
                    Category = "Budget",
                    Tags = new[] { "wifi", "budget" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1995, 7, 1, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4,
                    Location = GeographyPoint.Create(48.678581, -122.131577)
                },
                new Hotel() 
                { 
                    HotelId = "566-518", 
                    HotelName = "Surprisingly Expensive Suites",
                    BaseRate = 279.99,
                    Category = "Luxury",
                    ParkingIncluded = false
                }
            };
    
        try
        {
            indexClient.Documents.Index(IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc))));
        }
        catch (IndexBatchException e)
        {
            // Sometimes when your Search service is under load, indexing will fail for some of the documents in
            // the batch. Depending on your application, you can take compensating actions like delaying and
            // retrying. For this simple demo, we just log the failed document keys and continue.
            Console.WriteLine(
                "Failed to index some of the documents: {0}",
                String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
        }
    
        // Wait a while for indexing to complete.
        Thread.Sleep(2000);
    }

このメソッドには 4 つの部分があります。 1 つ目の配列を作成する `ホテル` 、インデックスにアップロードする入力データとして使用するオブジェクト。 このデータは、わかりやすくするためハードコーディングされています。 実際のアプリケーションでは、通常、データは SQL Database などの外部データ ソースから取得されます。

2 番目の部分を作成、 `IndexAction` 各 `ホテル`, 、し、新しいものをまとめてグループ `IndexBatch`します。 バッチがによって Azure Search インデックスにアップロードし、 `Documents.Index` メソッドです。
> [AZURE.NOTE] この例では、単にドキュメントをアップロードします。 既存のドキュメントまたはドキュメントの削除に変更をマージする場合は、作成、 `IndexAction` で対応する `IndexActionType`します。 指定する必要はありません `IndexActionType` この例では、既定値はため `アップロード`します。

このメソッドの 3 番目の部分は、インデックス作成の重要なエラー ケースを処理する catch ブロックです。 Azure Search サービスをバッチ内のドキュメントの一部にインデックスが失敗した場合、 `IndexBatchException` によってスローされた `Documents.Index`します。 サービスの負荷が高いときにドキュメントのインデックスを作成すると、これが発生する場合があります。 **コードでこのケースを明示的に処理することを強くお勧めします。**しばらく待ってから失敗したドキュメントのインデックス作成を再試行したり、サンプルと同じようにログに記録してから続けることができます。または、アプリケーションのデータ整合性要件に応じて他の処理を行うこともできます。

最後に、メソッドは 2 秒間遅延します。 インデックスの作成は Azure Search サービスで非同期的に行われるので、サンプル アプリケーションは短い時間待機して、確実にドキュメントを検索に使用できるようにする必要があります。 通常、このような遅延は、デモ、テスト、およびサンプル アプリケーションでのみ必要です。





