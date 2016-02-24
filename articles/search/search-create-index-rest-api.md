<properties
    pageTitle="REST API を使用した Azure Search インデックスの作成 | Microsoft Azure | ホスト型クラウド検索サービス"
    description="Azure Search と HTTP REST API を使用して、コードでインデックスを作成します。"
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="11/17/2015"
    ms.author="heidist"/>

# REST API を使用した Azure Search インデックスの作成
> [AZURE.SELECTOR]
- [概要](search-what-is-an-index.md)
- [ポータル](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [REST ()](search-create-index-rest-api.md)

この記事には、使用してインデックスを作成する方法がでは、 [Azure Search REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx)します。 次の内容の一部は [Create Index (Azure Search REST API)](https://msdn.microsoft.com/library/azure/dn798941.aspx)します。 詳しいコンテキストについては、元の記事を参照してください。

インデックスを作成するうえでの前提条件として、(通常は httpClient を介して) 検索サービスへの接続が既に確立されていることが挙げられます。

## インデックス スキーマの定義

REST API を使用してインデックスを作成するには、Azure Search の URL エンドポイントに POST 要求を発行します。 インデックスの構造は、要求の本文で JSON を使用して定義します。

**要求ヘッダーを要求して**:

次の例では、サービスの URL エンドポイントを使用する必要があります。具体的には、サービス名と適切な API バージョン (このドキュメントが書かれた時点で最新の API バージョンは "2015-02-28") に注意してください。 要求ヘッダーにもする必要があるしたときに、サービスのプライマリ管理者キーを提供 [サービスを作成する](https://msdn.microsoft.com/library/azure/dn798941.aspx/)です。

    POST https://[servicename].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/JSON
    api-key:[primary admin key]


**要求本文**:

これは、インデックス名 ("hotels"、ここでは) を識別するだけでなく、 [名、型、およびフィールドの属性](https://msdn.microsoft.com/library/azure/dn798941.aspx)します。

    {
        "name": "hotels",  
        "fields": [
            {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
            {"name": "baseRate", "type": "Edm.Double"},
            {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
            {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, analyzer:"fr.lucene"},
            {"name": "hotelName", "type": "Edm.String"},
            {"name": "category", "type": "Edm.String"},
            {"name": "tags", "type": "Collection(Edm.String)"},
            {"name": "parkingIncluded", "type": "Edm.Boolean"},
            {"name": "smokingAllowed", "type": "Edm.Boolean"},
            {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
            {"name": "rating", "type": "Edm.Int32"},
            {"name": "location", "type": "Edm.GeographyPoint"}
        ]
    }

要求が成功した場合、状態コード "201 Created" が返ります。 REST API を使用してインデックスを作成する方法については、次を参照してください [このページ](https://msdn.microsoft.com/library/azure/dn798941.aspx)します。

