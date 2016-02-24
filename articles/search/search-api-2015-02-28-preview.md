<properties
   pageTitle="Azure Search サービス REST API バージョン 2015-02-28-Preview | Microsoft Azure | ホスト型クラウド検索サービス"
   description="Azure Search サービス REST API バージョン 2015-02-28-Preview には、Lucene クエリ構文や moreLikeThis 検索などの試験的機能が含まれています。"
   services="search"
   documentationCenter="na"
   authors="HeidiSteen"
   manager="mblythe"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="search"
   ms.date="11/04/2015"
   ms.author="heidist"/>

# Azure Search サービス REST API: バージョン 2015-02-28-Preview

Azure Search は Microsoft Azure のホスト型クラウド検索サービスです。 この記事は、`api-version=2015-02-28-Preview` のリファレンス ドキュメントです。 このプレビューは、現在一般公開バージョンを拡張 [api バージョン 2015年-02-28 =](https://msdn.microsoft.com/library/dn798935.aspx), 、以下の試験的機能を提供することで。

- [Lucene クエリ構文](https://msdn.microsoft.com/library/azure/mt589323.aspx) の実装、 [Lucene クエリ パーサー](https://lucene.apache.org/core/4_10_0/queryparser/org/apache/lucene/queryparser/classic/package-summary.html), 、これで queryType パラメーターを使用して指定できます [検索操作](#SearchDocs)します。
- `moreLikeThis` で使用されるクエリ queparameter [検索操作](#SearchDocs) は、他のドキュメントを検索のもう 1 つの特定のドキュメントに関連します。

`2015-02-28-Preview` のいくつかの追加機能は、別のドキュメントに記載されています。 学習した内容は次のとおりです。

- [スコアリング プロファイル](search-api-scoring-profiles-2015-02-28-preview.md)
- [インデクサー](search-api-indexers-2015-02-28-preview.md)

Azure Search サービスは複数のバージョンで使用できます。 参照してください [Search サービスのバージョン](http://msdn.microsoft.com/library/azure/dn864560.aspx) 詳細です。

##このドキュメントの API

Azure Search サービス API は、API 操作の 2 つの URL 構文をサポートしています: 単純なと OData (を参照してください [OData (Azure Search API) のサポート](http://msdn.microsoft.com/library/azure/dn798932.aspx) 詳細)。 簡単な構文の一覧を次に示します。

[インデックスの作成](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[インデックスの更新](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[インデックスの取得](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[インデックスの一覧取得](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Get Index Statistics (Azure Search API) (インデックス統計の取得 (Azure Search API))](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[インデックスの削除](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[インデックス内のデータの追加、削除、更新](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[ドキュメントの検索](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[ドキュメントの参照](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Count Documents (Azure Search) (ドキュメントのカウント (Azure Search))](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[検索候補](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

________________________________________
<a name="IndexOps"></a>
## インデックス操作

特定のインデックス リソースに対して簡単な HTTP 要求 (POST、GET、PUT、DELETE) を実行することにより、Azure Search サービスでインデックスを作成および管理できます。 インデックスを作成するには、最初にインデックス スキーマが記述されている JSON ドキュメントを POST します。 スキーマでは、インデックスのフィールド、データ型、使用可能な方法 (たとえば、フルテキスト検索、フィルター、並べ替え、ファセット) が定義されています。 また、インデックスの動作を構成するスコアリング プロファイル、サジェスター、および他の属性も定義されています。

次の例では、2 つの言語で Description フィールドが定義されているホテル情報の検索に使用されるスキーマを示します。 フィールドの使用方法を属性でどのように制御しているのかに注意してください。 たとえば、`hotelId` はドキュメント キー (`"key": true`) として使用され、フルテキスト検索からは除外されます (`"searchable": false`)。

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

インデックスが作成された後、インデックスを設定するドキュメントをアップロードします。 参照してください [更新プログラムのドキュメントの追加または](#AddOrUpdateDocuments) この次の手順にします。

Azure Search でのインデックス処理紹介ビデオについては、次を参照してください。、 [Azure Search に関する Channel 9 Cloud Cover のエピソード](http://go.microsoft.com/fwlink/p/?LinkId=511509)します。


<a name="CreateIndex"></a>
## インデックスの作成

インデックスは Azure Search においてドキュメントを整理および検索するための主要な手段であり、テーブルがデータベースのレコードを整理する方法と似ています。 各インデックスにはインデックス スキーマ (フィールド名、データ型、プロパティ) にすべてが準拠するドキュメントのコレクションがありますが、インデックスでは他の検索動作を定義する追加の構造 (サジェスター、スコアリング プロファイル、CORS オプション) も指定されています。

HTTP POST または PUT 要求を使用して、Azure Search サービス内に新しいインデックスを作成できます。 要求の本文は、インデックスおよび構成の情報を指定する JSON スキーマです。

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

または、PUT を使用して、URI でインデックス名を指定することもできます。 インデックスが存在しない場合は、作成されます。

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

インデックスを作成すると、検索操作で格納および使用されるドキュメントの構造が決まります。 インデックスを設定するのは、別の操作です。 この手順で使用することができます、 [インデクサー](https://msdn.microsoft.com/library/azure/mt183328.aspx) (サポートされているデータ ソースで使用できる)、または [追加、更新、またはドキュメントの削除](https://msdn.microsoft.com/library/azure/dn798930.aspx) 操作します。 ドキュメントが POST されると、逆インデックスが生成されます。

**注**: 許可されるインデックスの最大数は価格レベルによって異なります。 Free サービスの場合、最大 3 個のインデックスが許可されます。 Standard サービスでは、Search サービスごとに 50 個のインデックスが許可されます。 参照してください [制限および制約](http://msdn.microsoft.com/library/azure/dn798934.aspx) 詳細です。

**要求**

HTTPS はすべてのサービス要求に必要です。  **Create Index** 要求は POST または PUT メソッドを使用して作成できます。 POST を使用するときは、インデックス スキーマの定義とともに、要求本文でインデックスの名前を指定する必要があります。 PUT の場合、インデックス名は URL の一部です。 インデックスが存在しない場合は作成されます。 既に存在する場合は、新しい定義に更新されます。

インデックスの名前は小文字にします。文字または数字で始めます。スラッシュとドットは使用できません。文字数の制限は 128 文字です。 インデックスの名前は文字または数字で始め、残りの部分には文字、数字、ダッシュを使用できます。ダッシュは連続することができません。

`api-version` は必須です。 参照してください [Search サービスのバージョン](http://msdn.microsoft.com/library/azure/dn864560.aspx) の使用可能なバージョンの一覧です。

**要求ヘッダー**

次の一覧は、必須と任意の要求ヘッダーについてまとめたものです。

- `Content-Type`: 必須。 これを `application/json` に設定します
- `api-key`: 必須。 `api-key` は、
- Search サービスに対する要求の認証に使用されます。 これはサービスに固有の文字列値です。  **Create Index** 要求を含める必要があります、 `api-key` ヘッダーには (クエリ キー) ではなく管理者キーに設定します。

要求 URL を作成するにはサービス名も必要です。 サービス名と `api-key` はどちらも、Azure クラシック ポータルのサービス ダッシュボードから取得できます。 参照してください [ポータルで Azure Search サービスの作成](search-create-service-portal.md) のページのナビゲーション ヘルプについてです。

<a name="RequestData"></a>
**要求本文の構文**

要求の本文にはスキーマ定義が含まれ、スキーマ定義には、そのインデックスにフィードされるドキュメント内のデータ フィールドのリスト、データ型、属性、およびクエリ時に一致するドキュメントのスコア付けに使用されるスコアリング プロファイルのオプションのリストが含まれます。

POST 要求の場合、要求本文でインデックスの名前を指定する必要があることに注意してください。

インデックスに存在できるキー フィールドは 1 つだけです。 文字列フィールドでなければなりません。 このフィールドは、インデックス内に格納される各ドキュメントの一意の識別子を表します。

インデックスの主要部分には次のものが含まれます。

- `name`
- このインデックスにフィードされる `fields`。そのフィールドで許可されるアクションを定義する名前、データ型、プロパティを含みます。
- オート コンプリートまたは先行入力クエリに使用される `suggesters`。
- カスタム検索スコア ランキングに使用される `scoringProfiles`。 参照してください [スコア付けプロファイルを追加する](https://msdn.microsoft.com/library/azure/dn798928.aspx) 詳細です。
- 既定のスコアリング動作を上書きするために使用される `defaultScoringProfile`。
- インデックスに対するクロス オリジン クエリを可能にする `corsOptions`。

要求ペイロードを構築するための構文は次のとおりです。 サンプルの要求はこのトピックをさらに進むと登場します。

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,
          "analyzer": "name of text analyzer"
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**インデックスの属性**

インデックスを作成するときに、以下の属性を設定できます。 スコアリングおよびスコアリング プロファイルについての詳細については、「 [スコアリング プロファイルの追加](https://msdn.microsoft.com/library/azure/dn798928.aspx)します。

`name` - フィールドの名前を設定します。

`type` - フィールドのデータ型を設定します。 参照してください [サポートされるデータ型](#DataTypes) サポートされる型のリスト。

`searchable` - フィールドをフルテキスト検索可能としてマークします。 これは、インデックス処理中に単語の区切りなどの分析が行われることを意味します。 `searchable` フィールドに "sunny day" のような値を設定した場合、個別トークン "sunny" と "day" に内部的に分割されます。 これにより、これらの語句をフルテキスト検索できます。 `Edm.String` 型または `Collection(Edm.String)` 型のフィールドは、既定で `searchable` になります。 他の型のフィールドは、`searchable` にすることはできません。

  - **注**: `searchable` フィールドは、Azure Search はフルテキスト検索のためのフィールド値の追加のトークン化されたバージョンを格納するために、インデックスに余分な領域を消費します。 インデックスの領域を節約する必要があり、フィールドを検索に含める必要がない場合は、`searchable` を `false` に設定してください。

`filterable` -で参照されるフィールドをできるように `$filter` クエリ。 `filterable` 異なる `searchable` 文字列の処理方法にします。 `filterable` である `Edm.String` 型または `Collection(Edm.String)` 型のフィールドは、単語分割を行われないため、比較は完全一致のみになります。 たとえば、そのようなフィールド `f` に "sunny day" を設定した場合、`$filter=f eq 'sunny'` では一致が見つかりませんが、`$filter=f eq 'sunny day'` では見つかります。 既定では、すべてのフィールドが `filterable` です。

`sortable` - 既定ではシステムは結果をスコア順に並べ替えますが、多くの場合、ユーザーはドキュメントのフィールドで並べ替えることを望みます。 `Collection(Edm.String)` 型のフィールドを `sortable` にすることはできません。 他のすべてのフィールドは、既定で `sortable` になります。

`facetable` - 通常、カテゴリ別のヒット カウントを含む検索結果のプレゼンテーションで使用されます (たとえば、デジタル カメラを検索し、ブランド別、メガピクセル別、価格別などでヒットを表示する場合)。 このオプションは、`Edm.GeographyPoint` 型のフィールドでは使用できません。 他のすべてのフィールドは、既定で `facetable` になります。

  - **注**: 型のフィールド `Edm.String` いる `filterable`, 、`sortable`, 、または `facetable` は最大で 32 KB の長さ。 これは、このようなフィールドは 1 つの検索語句として扱われ、Azure Search での語句の最大長は 32 KB であるためです。 これより多くのテキストを単一の文字列フィールドに格納する必要がある場合は、インデックスの定義で明示的に `filterable`、`sortable`、および `facetable` を `false` に設定する必要があります。

  - **注**: フィールドが none に設定する上記の属性のかどうかは `true` (`searchable`, 、`filterable`, 、`sortable`,  、または`facetable`)、フィールドは実質的に逆インデックスから除外します。 このオプションは、クエリでは使用されませんが、検索結果には必要なフィールドに便利です。 このようなフィールドをインデックスから除外と、パフォーマンスが向上します。

`suggestions` - 以前のバージョンの API には `suggestions` プロパティが含まれていました。 このブール型プロパティは現在は非推奨になっており、`2015-02-28` または `2015-02-28-Preview` では使用できません。 使用してください、 [サジェスター API](#Suggesters) 代わりにします。 `2014-07-31` バージョンでは、`suggestions` プロパティは、`Edm.String` 型または `Collection(Edm.String)` 型のフィールドに対し、フィールドを先行入力のオート コンプリートに使用できるかどうかを指定するために使用されていました。  `suggestions` が `false` 既定では、インデックスに余分な領域を必要とするため、有効にした場合を参照してください [プレビューから Azure Search の一般リリースへの移行](search-transition-from-preview.md) 手順については、新しい API への移行方法をします。

`key` - インデックス内のドキュメントに対する一意の識別子を含んでいるものとしてフィールドをマークします。 正確に 1 つのフィールドを `key` として選択する必要があり、そのフィールドは `Edm.String` 型でなければなりません。 キーのフィールドを直接使用してドキュメント参照を使用することができます、 [参照 API](#LookupAPI)します。

`retrievable` - 検索結果でフィールドを返すことができるかどうかを設定します。  これは、フィールド (たとえば、マージン) をフィルター、並べ替え、またはスコアリングのメカニズムとして使用するけれども、エンド ユーザーに対しては表示したくない場合に役立ちます。 `key` フィールドに対してはこの属性が `true` である必要があります。

`analyzer` - フィールドに使用するテキスト アナライザーの名前を設定します。 使用できる値セットを参照してください。 [言語サポート](#LanguageSupport)します。 このオプションは、`searchable` フィールドでのみ使用できます。 フィールドのアナライザーを選択した後は変更できません。

`suggesters` - 検索モードおよび検索候補の内容のソースであるフィールドを設定します。 参照してください [サジェスター](#Suggesters) 詳細です。

`scoringProfiles` - 検索結果での項目の順序を変更できるカスタム スコアリング動作を定義します。 スコアリング プロファイルは、フィールドの重みと関数で構成されます。 参照してください [スコアリング プロファイルの追加](https://msdn.microsoft.com/library/azure/dn798928.aspx) 詳細については、スコアリング プロファイルで使用される属性です。

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**言語のサポート**

検索可能なフィールドで最も頻繁に行われる分析は、単語の分割、テキストの正規化、語句の除外などです。 使用して既定では、Azure Search の検索可能フィールドの分析、 [Apache Lucene 標準アナライザー](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) を次の要素のテキストに分割、["Unicode Text Segmentation"](http://unicode.org/reports/tr29/) ルール。 さらに、標準アナライザーはすべての文字を小文字形式に変換します。 インデックス付きドキュメントと検索語句の両方について、インデックス作成とクエリ処理の間に分析が行われます。

Azure Search は、さまざまな言語をサポートしています。 各言語には、特定の言語の特性が考慮されている標準以外のテキスト アナライザーが必要です。 Azure Search では 2 種類のアナライザーが用意されています。

- 35 個のアナライザーは Lucene によって提供されます。
- 50 個のアナライザーは、Office および Bing で使用されるマイクロソフト独自の自然言語処理技術によって提供されます。

開発者によっては、使い慣れた簡単なオープン ソース ソリューションである Lucene を好む場合があります。 Lucene のアナライザーは高速です。しかし、マイクロソフトのアナライザーには高度な機能があります。たとえば、レンマ化、単語複混合化 (ドイツ語、デンマーク語、オランダ語、スウェーデン語、ノルウェー語、エストニア語、フィンランド語、ハンガリー語、スロバキア語などの言語で)、エンティティ認識 (URL、電子メール、日付、数値) などです。 可能であれば、マイクロソフトと Lucene の両方のアナライザーを比較し、より適したものを選んでください。

***undefined***

英語用 Lucene アナライザーは標準アナライザーを拡張します。 単語から所有格を (末尾の's) を削除して、に従ってステミングを適用 [Porter Stemming アルゴリズム](http://tartarus.org/~martin/PorterStemmer/), 、英語を削除および [のストップ ワードを](http://en.wikipedia.org/wiki/Stop_words)します。

これに対し、マイクロソフトのアナライザーは、ステミングではなくレンマ化を実行します。 効率的な屈折と不規則な単語のフォームを処理できる関連性の高い検索結果にどのような結果になります (watch モジュール 7 の [Azure 検索 MVA プレゼンテーション](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) 詳細については)。

言語によっては、マイクロソフトのアナライザーでのインデックス作成には、平均して、Lucene の同等機能の 2 ～ 3 倍の時間がかかります。 平均的なサイズのクエリの場合、検索のパフォーマンスはさほど影響はありません。

***undefined***

インデックス定義の各フィールドについて、言語とベンダーを指定するアナライザー名を `analyzer` プロパティに設定できます。 そのフィールドに対してインデックスの作成および検索を行う場合は、同じアナライザーが適用されます。
たとえば、英語、フランス語、スペイン語によるホテルの説明を含む個別のフィールドを同じインデックスに同時に作成できます。 使用して、 ['searchFields' クエリ パラメーター](#SearchQueryParameters) 、クエリで検索するには、どの言語固有フィールドを指定します。 含むクエリの例を確認することができます、 `analyzer` プロパティ [ドキュメントの検索](#SearchDocs)します。 

***undefined***

サポートされている言語と、Lucene およびマイクロソフトのアナライザーの名前を以下に一覧します。

<table style="font-size:12">
    <tr>
        <th>言語</th>
        <th>マイクロソフトのアナライザーの名前</th>
        <th>Lucene のアナライザーの名前</th>
    </tr>
    <tr>
        <td>アラビア語</td>
        <td>ar.microsoft</td>
        <td>ar.lucene</td>      
    </tr>
    <tr>
        <td>アルメニア語</td>
        <td></td>
        <td>hy.lucene</td>
    </tr>
    <tr>
        <td>バングラ語</td>
        <td>bn.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>バスク語</td>
        <td></td>
        <td>eu.lucene</td>
    </tr>
    <tr>
        <td>ブルガリア語</td>
        <td>bg.microsoft</td>
        <td>bg.lucene</td>
    </tr>
    <tr>
        <td>カタルニア語</td>
        <td>ca.microsoft</td>
        <td>ca.lucene</td>          
    </tr>
    <tr>
        <td>中国語 (簡体字)</td>
        <td>zh-Hans.microsoft</td>
        <td>zh-Hans.lucene</td>     
    </tr>
    <tr>
        <td>中国語 (繁体字)</td>
        <td>zh-Hant.microsoft</td>
        <td>zh-Hant.lucene</td>     
    <tr>
    <tr>
        <td>クロアチア語</td>
        <td>hr.microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>チェコ語</td>
        <td>cs.microsoft</td>
        <td>cs.lucene</td>      
    </tr>    
    <tr>
        <td>デンマーク語</td>
        <td>da.microsoft</td>
        <td>da.lucene</td>      
    </tr>    
    <tr>
        <td>オランダ語</td>
        <td>nl.microsoft</td>
        <td>nl.lucene</td>  
    </tr>    
    <tr>
        <td>英語</td>        
        <td>en.microsoft</td>
        <td>en.lucene</td>      
    </tr>
    <tr>
        <td>エストニア語</td>
        <td>et.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>フィンランド語</td>
        <td>fi.microsoft</td>
        <td>fi.lucene</td>      
    </tr>    
    <tr>
        <td>フランス語</td>
        <td>fr.microsoft</td>
        <td>fr.lucene</td>      
    </tr>
    <tr>
        <td>ガリシア語</td>
        <td></td>
        <td>gl.lucene</td>      
    </tr>
    <tr>
        <td>ドイツ語</td>
        <td>de.microsoft</td>
        <td>de.lucene</td>      
    </tr>
    <tr>
        <td>ギリシャ語</td>
        <td>el.microsoft</td>
        <td>el.lucene</td>      
    </tr>
    <tr>
        <td>グジャラート語</td>
        <td>gu.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>ヘブライ語</td>
        <td>he.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>ヒンディー語</td>
        <td>hi.microsoft</td>
        <td>hi.lucene</td>      
    </tr>
    <tr>
        <td>ハンガリー語</td>      
        <td>hu.microsoft</td>
        <td>hu.lucene</td>
    </tr>
    <tr>
        <td>アイスランド語</td>
        <td>is.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>インドネシア語</td>
        <td>id.microsoft</td>
        <td>id.lucene</td>      
    </tr>
    <tr>
        <td>アイルランド語</td>
        <td></td>
        <td>ga.lucene</td>
    </tr>
    <tr>
        <td>イタリア語</td>
        <td>it.microsoft</td>
        <td>it.lucene</td>      
    </tr>
    <tr>
        <td>日本語</td>
        <td>ja.microsoft</td>
        <td>ja.lucene</td>
        
    </tr>
    <tr>
        <td>Kannada</td>
        <td>ka.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Korean</td>
        <td></td>
        <td>ko.lucene</td>
    </tr>
    <tr>
        <td>Latvian</td>        
        <td>lv.microsoft</td>
        <td>lv.lucene</td>  
    </tr>
    <tr>
        <td>Lithuanian</td>
        <td>lt.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malayalam</td>
        <td>ml.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malay (Latin)</td>
        <td>ms.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Marathi</td>
        <td>mr.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Norwegian</td>
        <td>nb.microsoft</td>
        <td>no.lucene</td>      
    </tr>
    <tr>
        <td>Persian</td>
        <td></td>
        <td>fa.lucene</td>      
    </tr>
    <tr>
        <td>Polish</td>
        <td>pl.microsoft</td>
        <td>pl.lucene</td>      
    </tr>
    <tr>
        <td>Portuguese (Brazil)</td>
        <td>pt-Br.microsoft</td>
        <td>pt-Br.lucene</td>       
    </tr>
    <tr>
        <td>Portuguese (Portugal)</td>
        <td>pt-Pt.microsoft</td>        
        <td>pt-Pt.lucene</td>
    </tr>
    <tr>
        <td>Punjabi</td>
        <td>pa.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Romanian</td>
        <td>ro.microsoft</td>
        <td>ro.lucene</td>
    </tr>
    <tr>
        <td>Russian</td>
        <td>ru.microsoft</td>
        <td>ru.lucene</td>  
    </tr>
    <tr>
        <td>Serbian (Cyrillic)</td>
        <td>sr-cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Serbian (Latin)</td>
        <td>sr-latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slovak</td>
        <td>sk.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Slovenian</td>
        <td>sl.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Spanish</td>
        <td>es.microsoft</td>
        <td>es.lucene</td>
    </tr>
    <tr>
        <td>Swedish</td>
        <td>sv.microsoft</td>
        <td>sv.lucene</td>
    </tr>

    <tr>
        <td>Tamil</td>
        <td>ta.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Telugu</td>
        <td>te.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Thai</td>
        <td>th.microsoft</td>
        <td>th.lucene</td>
    </tr>
    <tr>
        <td>Turkish</td>
        <td>tr.microsoft</td>
        <td>tr.lucene</td>      
    </tr>
    <tr>
        <td>Ukrainian</td>
        <td>uk.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Urdu</td>
        <td>ur.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Vietnamese</td>
        <td>vi.microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Additionally Azure Search provides language-agnostic analyzer configurations</td>
    <tr>
        <td>Standard ASCII Folding</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Unicode text segmentation (Standard Tokenizer)</li>
            <li>ASCII folding filter - converts Unicode characters that don't belong to the set of first 127 ASCII characters into their ASCII equivalents. This is useful for removing diacritics.</li>
        </ul>
        </td>
    </tr>
</table>

注釈が付けられた名前を持つすべてのアナライザー <i>lucene</i> により強化されて [Apache Lucene の言語アナライザー](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html)します。 ASCII フォールディング フィルターの詳細についてはご覧 [ここ](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html)します。

**Suggesters**

`suggester` は、検索のオートコンプリートをサポートするために使用されるインデックス内のフィールドを定義します。 部分的な検索文字列に送信する通常、 [Suggestions API](#Suggestions) 、ユーザーが検索クエリを入力すると、API が提案されるフレーズのセットを返します。 インデックスで定義した suggester によって、先行入力検索語句を作成するために使用するフィールドが決められます。 参照してください [サジェスター](#Suggesters) 構成の詳細について。

**スコアリング プロファイル**

`scoringProfile` - 検索結果での項目の順序を変更できるカスタム スコアリング動作を定義します。 スコアリング プロファイルは、フィールドの重みと関数で構成されます。 使用するには、クエリ文字列にプロファイル名を指定します。

既定のスコアリング プロファイルはバックグラウンドで実行され、検索セットの各項目の検索スコアがコンピューティングされます。 内部的な名前のないスコアリング プロファイルを使用できます。 または、クエリ文字列にカスタム プロファイルが指定されていない場合に常に呼び出される既定値として、カスタム プロファイルを使用するように `defaultScoringProfile` を設定します。

参照してください [(Azure Search サービス REST API) 検索インデックスにスコア付けプロファイルを追加](search-api-scoring-profiles-2015-02-28.md) 詳細です。

**CORS のオプション**

ブラウザーがすべてのクロス オリジン要求を禁止するので、既定ではクライアント側 Javascript はどの API も呼び出すことができません。 インデックスへのクロス オリジン クエリを許可するには、`corsOptions` 属性を設定することによって CORS (クロス オリジン リソース共有) を有効にします。 セキュリティ上の理由から、CORS をサポートするのはクエリ API だけであることに注意してください。 CORS に対しては以下のオプションを設定できます。

- `allowedOrigins` (必須): インデックスへのアクセスが許可されるオリジンのリストです。 つまり、これらのオリジンから提供されるすべての Javascript コードは、インデックスのクエリを許可されます (正しい API キーを提供する場合)。 各オリジンの標準的な形式は `protocol://fully-qualified-domain-name:port` ですが、多くの場合ポートは省略されます。 参照してください [今回](http://go.microsoft.com/fwlink/?LinkId=330822) 詳細です。
 - すべてのオリジンにアクセスを許可する場合は、`allowedOrigins` 配列の唯一の要素として `*` を指定します。 なお **この検索サービスの運用方法は推奨されません。**ただし、開発やデバッグの目的では役に立つ場合があります。
- `maxAgeInSeconds` (省略可能): ブラウザーはこの値を使用して、CORS プレフライト応答をキャッシュする期間 (秒) を決定します。 負ではない整数を指定する必要があります。 この値が大きいほどパフォーマンスはよくなりますが、CORS ポリシーの変更が有効になるまでの時間は長くなります。 これを設定しないと、既定の期間として 5 分が使用されます。

<a name="CreateUpdateIndexExample"></a>
**要求本文の例**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, analyzer="fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Response**

要求が成功した場合:要求成功の場合: 201 作成されました。

既定では、応答本文には作成されたインデックス定義の JSON が含まれます。 `Prefer` 要求ヘッダーが `return=minimal` に設定されている場合、応答本文は空で、成功の状態コードは「201 作成されました」ではなく「204 コンテンツがありません」になります。 これは、インデックスの作成に PUT または POST のどちらが使用された場合でも同じです。

**解説**

現時点では、インデックス スキーマ更新のサポートは限定的です。 フィールドの種類の変更といったインデックスの再作成を必要とするスキーマ更新は、現在はサポートされていません。 既存のフィールドを変更または削除することはできませんが、新しいフィールドはいつでも既存のインデックスに追加できます。 新しいフィールドを追加すると、インデックス内のすべての既存ドキュメントでそのフィールドに null 値が自動的に設定されます。 新しいドキュメントがインデックスに追加されるまで、追加の記憶域は使用されません。

<a name="Suggesters"></a>
##Suggesters

Azure Search の検索候補機能は、先行入力またはオート コンプリート クエリ機能であり、検索ボックスに入力された部分文字列に反応して、可能性のある検索用語の一覧を提供します。 商用 Web 検索エンジンを使用したときに、クエリ候補機能にお気づきではないでしょうか。Bing に「.NET」と入力すると、「.NET 4.5」、「.NET Framework 3.5」などの用語のリストが生成されます。 サービス REST API を使用する場合、カスタム Azure Search アプリケーションに検索候補機能を実装するには、次のことが必要です。

- 追加することで、検索候補を有効にする、 **サジェスター** し、名前、検索モード、フィールドの一覧を入力に先行して、インデックスの構築が呼び出されます。 たとえば、ソース フィールドとして「cityName」を指定した場合、「Sea」の部分検索文字列を入力すると、「Seattle」、「Seaside」、「Seatac」 (3 つとも実際の都市名) がユーザーにクエリ候補として提示されます。

- 呼び出して修正候補を呼び出し、 [Suggestions API](#Suggestions) アプリケーション コードにします。 通常、ユーザーが検索クエリを入力している間に、検索文字列の一部がサービスに送信され、この API から提案されるフレーズのセットが返されます。

構成する方法を説明する **サジェスター**します。 参照してください、 [Suggestions API](#Suggestions) suggester の使用方法の詳細。

**使用法**

`Suggesters` は、インデックスに作成され、あいまいな用語やフレーズではなく、特定のドキュメントを提案するために使用した場合に、最も効果的に機能します。 最適な候補フィールドは、タイトル、名前、および項目を識別できる他の比較的短い語句です。 あまり効果的ではないのは、カテゴリやタグなどの反復的なフィールドまたは説明やコメントなどの非常に長いフィールドです。

インデックス定義の一部として、単一のサジェスターを `suggesters` コレクションに追加できます。 suggester を定義するプロパティは次のとおりです。

- `name`: サジェスターの名前。 `suggest` API を呼び出すときは、サジェスターの名前を使用します。
- `searchMode`: 候補語句の検索に使用する方法。 現在サポートされている唯一のモードは `analyzingInfixMatching` です。文の先頭または中間にあるフレーズの柔軟なマッチングを実行します。
- `sourceFields`: 検索候補の内容のソースである 1 つまたは複数のフィールドのリスト。 `Edm.String` 型および `Collection(Edm.String)` 型のフィールドだけを、検索候補のソースにできます。 カスタム言語アナライザーが設定されていないフィールドのみを使用できます。

**Suggester の例**

Suggester は、インデックスの一部です。 現在のバージョンで `suggesters` コレクションに存在できる suggester は、fields コレクションと `scoringProfiles` と共に 1 つだけです。

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [AZURE.NOTE]  Azure Search のパブリック プレビュー バージョンを使用した場合 `suggesters` 古いブール型プロパティが置き換えられます (`"suggestions": false`) は短い文字列 (3 ~ 25 文字) のプレフィックス候補のみをサポートします。 その置き換えである `suggesters` は、フィールドの内容の先頭または途中で一致する語句を検索し、検索文字列の誤りに対する許容範囲が優れた、挿入辞一致をサポートします。 一般公開版のリリース以降は、これが、Suggestions API の唯一の実装です。 `api-version=2014-07-31-Preview` で導入された以前の `suggestions` プロパティは引き続きそのバージョンで動作しますが、Azure Search の `2015-02-28` 以降のバージョンでは動作しません。

<a name="UpdateIndex"></a>
## インデックスの更新

Azure Search 内の既存のインデックスを、HTTP PUT 要求を使用して更新できます。 更新には、既存のスキーマへの新しいフィールドの追加、CORS オプションの変更、スコアリング プロファイルの変更が含まれます。 参照してください [スコアリング プロファイルの追加](https://msdn.microsoft.com/library/azure/dn798928.aspx) の詳細。 更新するインデックスの名前を要求 URI で指定します。

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**重要:** インデックス スキーマ更新のサポートは、検索インデックスを再構築を必要としない操作に制限します。 フィールドの種類の変更といったインデックスの再作成を必要とするスキーマ更新は、現在はサポートされていません。 新しいフィールドはいつでも追加できますが、既存のフィールドを変更または削除することはできません。 同じことが `suggesters` にも当てはまります。 フィールドを追加するときに新しいフィールドをサジェスターに追加できますが、`suggesters` からフィールドを削除したり、既存のフィールドを `suggesters` に追加したりすることはできません。

新しいフィールドをインデックスに追加すると、インデックス内のすべての既存ドキュメントでそのフィールドに null 値が自動的に設定されます。 新しいドキュメントがインデックスに追加されるまで、追加の記憶域は使用されません。

**要求**

HTTPS はすべてのサービス要求に必要です。  **Update Index** HTTP PUT を使用して要求を作成します。 PUT の場合、インデックス名は URL の一部です。 インデックスが存在しない場合は作成されます。 インデックスが既に存在する場合は、新しい定義に更新されます。

インデックスの名前は小文字にします。文字または数字で始めます。スラッシュとドットは使用できません。文字数の制限は 128 文字です。 インデックスの名前は文字または数字で始め、残りの部分には文字、数字、ダッシュを使用できます。ダッシュは連続することができません。

`api-version=[string]` (必須)。 プレビュー バージョンは `api-version=2015-02-28-Preview` です。 参照してください [Search サービスのバージョン](http://msdn.microsoft.com/library/azure/dn864560.aspx) の詳細および代替バージョンです。

**要求ヘッダー**

次の一覧は、必須と任意の要求ヘッダーについてまとめたものです。

- `Content-Type`: 必須。 これを `application/json` に設定します
- `api-key`: 必須。 `api-key` は Search サービスに対する要求の認証に使用されます。 これはサービスに固有の文字列値です。  **Update Index** 要求を含める必要があります、 `api-key` ヘッダーには (クエリ キー) ではなく管理者キーに設定します。

要求 URL を作成するにはサービス名も必要です。 サービス名と `api-key` は、Azure クラシック ポータルのサービス ダッシュボードから取得できます。 参照してください [ポータルで Azure Search サービスの作成](search-create-service-portal.md) のページのナビゲーション ヘルプについてです。

**要求本文の構文**

既存のインデックスを更新する場合、本文には、元のスキーマ定義に加えて、追加する新しいフィールドと、ある場合は変更後のスコアリング プロファイル、サジェスター、および CORS オプションが、含まれる必要があります。 スコアリング プロファイルおよび CORS オプションを変更しない場合は、インデックスが作成されたときの元の値を含める必要があります。 一般に、更新に使用する最善のパターンは、GET でインデックス定義を取得し、変更した後、PUT で更新するというものです。

利便性を考えて、インデックスの作成に使用されるスキーマ構文をここに再掲します。 参照してください [Create Index](#CreateIndex) 詳細です。

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,
          "analyzer": "name of text analyzer"
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Response**

要求が成功した場合: 204 コンテンツがありません。

既定では、応答本文は空になります。 ただし、`Prefer` 要求ヘッダーを `return=representation` に設定した場合は、応答本文には更新されたインデックス定義の JSON が含まれます。 この場合、成功の状態コードは "200 OK" になります。

<a name="ListIndexes"></a>
## インデックスの一覧取得

 **リスト インデックス** 操作インデックスの一覧現在、Azure Search サービスを返します。

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**要求**

HTTPS はすべてのサービス要求に必要です。  **リスト インデックス** 要求は、GET メソッドを使用して作成できます。

`api-version=[string]` (必須)。 プレビュー バージョンは `api-version=2015-02-28-Preview` です。 参照してください [Search サービスのバージョン](http://msdn.microsoft.com/library/azure/dn864560.aspx) の詳細および代替バージョンです。

**要求ヘッダー**

次の一覧は、必須と任意の要求ヘッダーについてまとめたものです。

- `api-key`: 必須。 `api-key` は Search サービスに対する要求の認証に使用されます。 これはサービスに固有の文字列値です。  **リスト インデックス** 要求を含める必要があります、 `api-key` (クエリ キー) ではなく管理者キーに設定します。

要求 URL を作成するにはサービス名も必要です。 サービス名と `api-key` は、Azure クラシック ポータルのサービス ダッシュボードから取得できます。 参照してください [ポータルで Azure Search サービスの作成](search-create-service-portal.md) のページのナビゲーション ヘルプについてです。

**要求本文**

ありません。

**Response**

状態コード: 応答の成功に対して「200 OK」が返されます。

次は応答本文の例です。

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

フィルター処理で応答を興味のあるプロパティだけに絞り込むことができます。 たとえば、インデックス名の一覧だけを必要とする場合、OData `$select` クエリ オプションを使用します。

    GET /indexes?api-version=2015-02-28-Preview&$select=name

この場合は、上記の例の応答は次のように表示されます。

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

これは、Search サービスにインデックスがたくさんある場合、帯域幅を節約する便利な方法となります。

<a name="GetIndex"></a>
## インデックスの取得

 **Get Index** 操作は、Azure Search からインデックスの定義を取得します。

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**要求**

サービス要求には HTTPS が必要です。  **Get Index** 要求は、GET メソッドを使用して作成できます。

要求 URI の [index name] には、インデックス コレクションから返すインデックスを指定します。

`api-version=[string]` (必須)。 プレビュー バージョンは `api-version=2015-02-28-Preview` です。 参照してください [Search サービスのバージョン](http://msdn.microsoft.com/library/azure/dn864560.aspx) の詳細および代替バージョンです。

**要求ヘッダー**

次の一覧は、必須と任意の要求ヘッダーについてまとめたものです。

- `api-key`: `api-key` は Search サービスに対する要求の認証に使用されます。 これはサービスに固有の文字列値です。  **Get Index** 要求を含める必要があります、 `api-key` (クエリ キー) ではなく管理者キーに設定します。

要求 URL を作成するにはサービス名も必要です。 サービス名と `api-key` は、Azure クラシック ポータルのサービス ダッシュボードから取得できます。 参照してください [ポータルで Azure Search サービスの作成](search-create-service-portal.md) のページのナビゲーション ヘルプについてです。

**要求本文**

ありません。

**Response**

状態コード: 応答の成功に対して「200 OK」が返されます。

JSON の例を参照してくださいで [を作成して、インデックスの更新](#CreateUpdateIndexExample) 応答ペイロードの例についてです。

<a name="DeleteIndex"></a>
## インデックスの削除

 **インデックスの削除** 操作は、Azure Search サービスからインデックスおよび関連するドキュメントを削除します。 インデックス名は、Azure クラシック ポータルのサービス ダッシュボードまたは API から取得できます。 参照してください [リスト インデックス](#ListIndexes) 詳細です。

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**要求**

サービス要求には HTTPS が必要です。  **インデックスの削除** 要求は DELETE メソッドを使用して作成できます。

要求 URI の [index name] には、インデックス コレクションから削除するインデックスを指定します。

`api-version=[string]` (必須)。 プレビュー バージョンは `api-version=2015-02-28-Preview` です。 参照してください [Search サービスのバージョン](http://msdn.microsoft.com/library/azure/dn864560.aspx) の詳細および代替バージョンです。

**要求ヘッダー**

次の一覧は、必須と任意の要求ヘッダーについてまとめたものです。

- `api-key`: 必須。 `api-key` は Search サービスに対する要求の認証に使用されます。 これはサービス URL に固有の文字列値です。  **インデックスの削除** 要求を含める必要があります、 `api-key` ヘッダーには (クエリ キー) ではなく管理者キーに設定します。

要求 URL を作成するにはサービス名も必要です。 サービス名と `api-key` は、Azure クラシック ポータルのサービス ダッシュボードから取得できます。 参照してください [ポータルで Azure Search サービスの作成](search-create-service-portal.md) のページのナビゲーション ヘルプについてです。

**要求本文**

ありません。

**Response**

状態コード: 応答の成功に対して「204 コンテンツがありません」が返されます。

<a name="GetIndexStats"></a>
## Get Index Statistics (Azure Search API) (インデックス統計の取得 (Azure Search API))

 **Get Index Statistics** 操作が Azure Search から現在のインデックスとストレージの使用状況に対するドキュメントの数を返します。

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

**要求**

すべてのサービス要求には HTTPS が必要です。  **Get Index Statistics** 要求は、GET メソッドを使用して作成できます。

要求 URI の [index name] で、指定したインデックスに関するインデックスの統計情報を返すサービスがわかります。

`api-version=[string]` (必須)。 プレビュー バージョンは `api-version=2015-02-28-Preview` です。 参照してください [Search サービスのバージョン](http://msdn.microsoft.com/library/azure/dn864560.aspx) の詳細および代替バージョンです。


**要求ヘッダー**

次の一覧は、必須と任意の要求ヘッダーについてまとめたものです。

- `api-key`: `api-key` は Search サービスに対する要求の認証に使用されます。 これはサービスに固有の文字列値です。  **Get Index Statistics** 要求を含める必要があります、 `api-key` (クエリ キー) ではなく管理者キーに設定します。

要求 URL を作成するにはサービス名も必要です。 サービス名と `api-key` は、Azure クラシック ポータルのサービス ダッシュボードから取得できます。 参照してください [ポータルで Azure Search サービスの作成](search-create-service-portal.md) のページのナビゲーション ヘルプについてです。

**要求本文**

ありません。

**Response**

状態コード: 応答の成功に対して「200 OK」が返されます。

応答本文の形式は次のとおりです。

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

________________________________________
<a name="DocOps"></a>
## ドキュメントの操作

Azure Search では、インデックスはクラウドに格納され、サービスにアップロードされた JSON ドキュメントを使用して設定されます。 アップロードされたすべてのドキュメントが、検索データのコーパスを構成します。 ドキュメントにはフィールドが含まれ、その一部はアップロード時に検索語句にトークン化されます。 Azure Search API の `/docs` URL セグメントは、インデックス内のドキュメントのコレクションを表します。 ドキュメントのアップロード、マージ、削除、クエリなど、コレクションに対して実行される操作はすべて単一インデックスのコンテキストで行われるので、これらの操作の URL は常に特定のインデックス名に対する `/indexes/[index name]/docs` で始まります。

アプリケーション コードでは、Azure Search にアップロードする JSON ドキュメントを生成する必要がありますか、または使用することができます、 [インデクサー](https://msdn.microsoft.com/library/dn946891.aspx) をデータ ソースが Azure SQL Database または DocumentDB の場合は、ドキュメントを読み込みます。 通常、指定した 1 つのデータセットからインデックスが設定されます。

検索する各項目に対して 1 つのドキュメントを用意するように計画する必要があります。 たとえば、映画レンタル アプリケーションでは映画ごとに 1 つのドキュメントを使用し、店舗アプリケーションでは SKU ごとに 1 つのドキュメントを使用し、オンライン コースウェア アプリケーションではコースごとに 1 つのドキュメントを使用し、調査会社ではリポジトリ内の論文ごとに 1 つのドキュメントを使用する、という具合です。

ドキュメントは、1 つまたは複数のフィールドで構成されます。 フィールドには、Azure Search によって検索語句にトークン化されるテキスト、およびフィルターまたはスコアリング プロファイルで使用できるトークン化されないテキストまたはテキスト以外の値を格納できます。 各フィールドに対してサポートされる名前、データ型、検索機能は、インデックス スキーマによって決まります。 各インデックス スキーマのフィールドの 1 つは ID として指定される必要があり、ドキュメントごとにインデックス内でそのドキュメントを一意に識別する ID フィールドの値が必要です。 他のすべてのドキュメント フィールドは省略可能であり、指定しないと既定で null 値になります。 null 値は、検索インデックスの領域を使用しないことに注意してください。

ドキュメントをアップロードする前に、サービスでインデックスを作成しておく必要があります。 参照してください [Create Index](#CreateIndex) 詳細については、この最初の手順です。

<a name="AddOrUpdateDocuments"></a>
## ドキュメントの追加、更新、削除

HTTP POST を使用して、指定したインデックスのドキュメントのアップロード、マージ、マージまたはアップロード、または削除を行うことができます。 更新の数が多い場合は、ドキュメントのバッチ処理 (バッチごとに最大 1000 ドキュメント、またはバッチごとに約 16 MB) をお勧めします。

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**要求**

HTTPS はすべてのサービス要求に必要です。 HTTP POST を使用して、指定したインデックスのドキュメントのアップロード、マージ、マージまたはアップロード、または削除を行うことができます。

要求 URI には、[index name] を含めて、ドキュメントを投稿するインデックスを指定します。 一度に 1 つのインデックスに対してのみ、ドキュメントを POST できます。

`api-version=[string]` (必須)。 プレビュー バージョンは `api-version=2015-02-28-Preview` です。 参照してください [Search サービスのバージョン](http://msdn.microsoft.com/library/azure/dn864560.aspx) の詳細および代替バージョンです。

**要求ヘッダー**

次の一覧は、必須と任意の要求ヘッダーについてまとめたものです。

- `Content-Type`: 必須。 これを `application/json` に設定します
- `api-key`: 必須。 `api-key` は Search サービスに対する要求の認証に使用されます。 これはサービスに固有の文字列値です。  **Add Documents** 要求を含める必要があります、 `api-key` ヘッダーには (クエリ キー) ではなく管理者キーに設定します。

要求 URL を作成するにはサービス名も必要です。 サービス名と `api-key` は、Azure クラシック ポータルのサービス ダッシュボードから取得できます。 参照してください [ポータルで Azure Search サービスの作成](.search-create-service-portal.md) のページのナビゲーション ヘルプについてです。

**要求本文**

要求の本文には、インデックスを作成する 1 つまたは複数のドキュメントが含まれます。 ドキュメントは、一意のキーによって識別されます。 各ドキュメントは、アクション (upload、merge、mergeOrUpload、delete) と関連付けられています。 アップロード要求には、キー/値ペアのセットとしてドキュメント データが含まれる必要があります。

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

**ドキュメント アクション**

- `upload`: 更新アクションは、ドキュメントが新しい場合は挿入されて存在する場合は更新/置換される "upsert" に似ています。 更新の場合はすべてのフィールドが置換されることに注意してください。
- `merge`: マージは、指定したフィールドで既存のドキュメントを更新します。 ドキュメントが存在しない場合、マージは失敗します。 マージで指定したすべてのフィールドは、ドキュメント内の既存のフィールドを置き換えます。 これには、`Collection(Edm.String)` 型のフィールドも含まれます。 たとえば、ドキュメントにフィールド "tags" があって値が `["budget"]` である場合、"tags" に値 `["economy", "pool"]` を指定してマージを実行すると、"tags" フィールドの最終的な値は `["economy", "pool"]` になります。 では **いない** する `["budget", "economy", "pool"]`です。
- `mergeOrUpload`: 指定したキーを持つドキュメントがインデックスに既に存在する場合は、`merge` と同じように動作します。 ドキュメントが存在しない場合は、新しいドキュメントの `upload` と同じように動作します。
- `delete`: インデックスから指定したドキュメントを削除します。 `delete` 操作ではキー フィールドの値のみを指定できることに注意してください。 他のフィールドを指定すると、HTTP 400 エラーになります。 ドキュメントから個々のフィールドを削除する場合は、代わりに `merge` を使用して、フィールドを明示的に `null` に設定します。

**Response**

状態コード: 成功応答に対して返される「200 OK」は、すべての項目のインデックスが正常に作成されたことを意味します (すべての項目の "status" フィールドが true に設定されることによって示される状態)。

    {
      "value": [
        {
          "key": "unique_key_of_document",
          "status": true,
          "errorMessage": null
        }
      ]
    }  

状態コード: 1 つ以上の項目のインデックスが正常に作成されなかった場合、207 が返されます (インデックスが作成されなかった項目の "status" フィールドが false に設定されることによって示される状態)。

    {
      "value": [
        {
          "key": "unique_key_of_document",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later."
        }
      ]
    }  

`errorMessage` プロパティは、可能な場合はインデックス作成エラーの理由を示します。

**注**: クライアント コードには、頻繁に 207 応答が発生すると、考えられる理由の 1 つはシステムの負荷ことです。 `errorMessage` プロパティを調べることによってこれを確認できます。 これに該当するかどうか、お勧め ***インデックス作成の要求を調整***します。 調整しないでインデックス作成トラフィックが減らない場合、システムは 503 エラーですべての要求を拒否し始めることがあります。

状態コード: 429 は、インデックスあたりのドキュメント数に対するクォータを超過したことを示します。 新しいインデックスを作成するか、またはさらに高い処理能力の限界にアップグレードする必要があります。

**例:**

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
          "hotelName": "Fancy Stay",
          "category": "Luxury",
          "tags": ["pool", "view", "wifi", "concierge"],
          "parkingIncluded": false,
          "smokingAllowed": false,
          "lastRenovationDate": "2010-06-27T00:00:00Z",
          "rating": 5,
          "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
          "@search.action": "upload",
          "hotelId": "2",
          "baseRate": 79.99,
          "description": "Cheapest hotel in town",
          "description_fr": "Hôtel le moins cher en ville",
          "hotelName": "Roach Motel",
          "category": "Budget",
          "tags": ["motel", "budget"],
          "parkingIncluded": true,
          "smokingAllowed": true,
          "lastRenovationDate": "1982-04-28T00:00:00Z",
          "rating": 1,
          "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
________________________________________
<a name="SearchDocs"></a>
## ドキュメントの検索

A **検索** 操作は GET または POST 要求として発行し、一致するドキュメントを選択するための条件を提供するパラメーターを指定します。

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**GET ではなく、POST を使用する場合**

使用すると HTTP GET を呼び出す、 **検索** API、要求 URL の長さが 8 KB を超えることはできないことに留意する必要があります。 これは通常、ほとんどのアプリケーションで十分な長さです。 ただし、一部のアプリケーション、具体的には OData フィルター式では、非常に大きなクエリが生成されます。 これらのアプリケーションについては、HTTP POST の使用をお勧めします。 POST の要求サイズの上限は 17 MB に近いため、最も複雑なクエリでも十分な長さとなります。

**要求**

サービス要求には HTTPS が必要です。  **検索** GET または POST メソッドを使用して要求を作成できます。

要求 URI は、パラメーターと一致するすべてのドキュメントについて、クエリするインデックスを指定します。 パラメーターは、GET 要求の場合、クエリ文字列に指定し、POST 要求の場合は要求本文に指定します。

GET 要求を作成する際のベスト プラクティス、として必ず [URL エンコード](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) REST API を直接呼び出すときに、特定のクエリ パラメーターです。  **検索** 操作、これが含まれます。

- `$filter`
- `facet`
- `highlightPreTag`
- `highlightPostTag`
- `search`
- `moreLikeThis`

URL エンコードは、上記のクエリ パラメーターにのみ推奨されます。 誤ってクエリ文字列全体 (? より後のすべて) を URL エンコードした場合は、要求が中断します。

また、URL エンコードは、GET を使用して REST API を直接呼び出すときにのみ必要です。 URL エンコードは必要ありませんを呼び出すときに **検索** 、POST を使用してを使用する場合や、 [.NET クライアント ライブラリ](https://msdn.microsoft.com/library/dn951165.aspx), 、URL のエンコーディングを処理します。

<a name="SearchQueryParameters"></a>
**クエリ パラメーター**

**検索** はクエリ条件を提供しても検索の動作を指定するいくつかのパラメーターを受け取ります。 呼び出すときに、これらのパラメーターを URL にクエリ文字列を提供する **検索** を呼び出すときは、要求本文に JSON プロパティと、GET を介して **検索** POST を介してします。 いくつかのパラメーターの構文は、GET および POST の間で多少異なります。 その違いは、以下で随時説明しています。

`search=[string]` (省略可能) - 検索するテキスト。 `searchFields` を指定しないと、既定ですべての `searchable` フィールドが検索されます。 検索するときに `searchable` フィールド、検索テキスト自体がトークン化される、複数の語句を空白で区切ることができますので (例: `search=hello world`)。 任意の語句と一致させるには、`*` を使用します (これはブール型フィルターのクエリに便利です)。 このパラメーターを省略することは、`*` に設定するのと同じ効果を持ちます。 参照してください [単純なクエリ構文](https://msdn.microsoft.com/library/dn798920.aspx) 検索構文の詳細についてのです。

  - **注**: 結果なることがあります驚くべきをクエリすると `searchable` フィールドです。 トークナイザーには、アポストロフィ、数値内のコンマなど、英語テキストで一般的なケースを処理するロジックが含まれています。たとえば、英語ではコンマは大きな数の桁区切り記号として使用されるので、`search=123,456` は、個別の語句 123 および 456 ではなく、1 つの語句 123,456 と一致します。 このため、`search` パラメーターで語句を区切るには区切り記号ではなく空白文字を使用することをお勧めします。

`searchMode=any|all` (省略可能、既定値は `any`) - ドキュメントを一致としてカウントするために、任意の検索語句またはすべての検索語句が一致する必要があるかどうか。

`searchFields=[string]` (省略可能) - 指定したテキストを検索するフィールド名のコンマ区切りのリスト。 対象フィールドは、`searchable` としてマークされている必要があります。

`queryType=simple|full` (省略可能、既定は `simple`) - "simple" に設定すると、検索テキストは簡単なクエリ言語 (+、*、"" などの記号を使用できます) を使用して解釈されます。 既定で、各ドキュメント内のすべての検索可能なフィールド (または `searchFields` で指定したフィールド) に対してクエリが評価されます。 クエリの種類を `full`に設定すると、検索テキストは Lucene クエリ言語 (フィールドの指定や重み付けによる検索を使用できます) を使用して解釈されます。 参照してください [単純なクエリ構文](https://msdn.microsoft.com/library/dn798920.aspx) と [Lucene クエリ構文](https://msdn.microsoft.com/library/azure/mt589323.aspx) の詳細については、検索構文です。 
 
> [AZURE.NOTE] 同様の機能を提供する $filter を優先するためのクエリ言語がサポートされていません Lucene の検索の範囲です。

`moreLikeThis=[key]` (省略可能) **重要:** この機能はでのみ使用 `2015-02-28-Preview`します。 このオプションは、テキスト検索パラメーター `search=[string]` を含むクエリでは使用できません。 `moreLikeThis` パラメーターは、ドキュメント キーで指定されているドキュメントに類似したドキュメントを検索します。 `moreLikeThis` で検索要求を行うと、ソース ドキュメント内での語句の頻度と希少性に基づいて検索語句の一覧が生成されます。 その後、これらの語句を使用して要求が行われます。 `searchFields` を使用して検索対象のフィールドが制限されていない場合、既定ですべての `searchable` フィールドの内容が考慮されます。  

`$skip=#` (省略可能) - スキップする検索結果の数。100,000 を超えることはできません。 ドキュメントを順番にスキャンする必要があり、この制限のために `$skip` を使用できない場合は、代わりに完全に順序付けられているキーに対する `$orderby` と範囲クエリでの `$filter` を使用することを検討してください。

> [AZURE.NOTE] 呼び出すときに **検索** POST を使用して、このパラメータの名前は `skip` の代わりに `$skip`します。

`$top=#` (省略可能) - 取得する検索結果の数。 これは、`$skip` と組み合わせて使用して、検索結果のクライアント側のページングを実装できます。

> [AZURE.NOTE] Azure Search を使用して ***サーバー側のページング*** をクエリが多くのドキュメントを一度に取得するを防ぐためにします。 既定のページ サイズは 50 で、最大ページ サイズは 1,000 です。 既定ではつまり、 **検索** を指定しない場合は、最大で 50 個の結果を返す `$top`します。 50 を超える結果がある場合は、応答には最大で 50 個の結果の次のページを取得する情報が含まれます (を参照してください `@odata.nextLink` と `@search.nextPageParameters` で [次の例](#SearchResponse))。 同様に、`$top` に 1,000 を超える値を指定し、結果が 1,000 個より多い場合は、最初の 1,000 個の結果だけが返されます。その際、最大 1,000 個の結果を含む次のページを取得するための情報も含まれます。  

> [AZURE.NOTE] 呼び出すときに **検索** POST を使用して、このパラメータの名前は `top` の代わりに `$top`します。

`$count=true|false` (省略可能、既定値は `false`) - 結果の合計数を取得するかどうか。 この値を `true` に設定すると、パフォーマンスに影響する場合があります。 返されるカウントは概数であることに注意してください。

> [AZURE.NOTE] 呼び出すときに **検索** POST を使用して、このパラメータの名前は `count` の代わりに `$count`します。

`$orderby=[string]` (省略可能) - 結果を並べ替えるための式のコンマ区切りのリスト。 各式は、フィールド名または `geo.distance()` 関数の呼び出しです。 各式に続いて、昇順を示す `asc` または降順を示す `desc` を指定できます。 既定値は昇順です。 結び付きは、ドキュメントの一致スコアによって切り離されます。 `$orderby` を指定しないと、既定の並べ替え順序はドキュメント一致スコアの降順になります。 `$orderby` には 32 句の制限があります。

> [AZURE.NOTE] 呼び出すときに **検索** POST を使用して、このパラメータの名前は `orderby` の代わりに `$orderby`します。

`$select=[string]` (省略可能) - 取得するフィールドのコンマ区切りリスト。 指定しないと、スキーマで取得可能とマークされているすべてのフィールドが含まれます。 このパラメーターを `*` に設定することによって、明示的にすべてのフィールドを要求することもできます。

> [AZURE.NOTE] 呼び出すときに **検索** POST を使用して、このパラメータの名前は `select` の代わりに `$select`します。

`facet=[string]` (0 以上) - ファセットに使用するフィールド。 オプションとして、コンマ区切りの `name:value` ペアとして表された、ファセットをカスタマイズするパラメーターを文字列に含めることができます。 有効なパラメーターは次のとおりです。

- `count` (ファセット語句の最大数、既定値は 10)。 最大値はありませんが、値が大きいとパフォーマンスが低下します。ファセット フィールドに多数の一意の語句が含まれる場合は特にそうです。
  - 例: `facet=category,count:5` はファセット結果の上位 5 カテゴリを取得します。  
  - **注**: 場合、 `count` パラメーターが一意の語句の数より小さい、結果が正しくない可能性があります。 これは、ファセット クエリがシャード間に分散される方法のためです。 `count` を大きくすると、一般に、語句の数の精度は向上しますが、パフォーマンスは低下します。
- `sort` (のいずれかの `count` を並べ替える *降順* カウント `-count` を並べ替える *昇順* カウント `value` を並べ替える *昇順* 、値または `-value` を並べ替える *降順* 値で)
  - 例: `facet=category,count:3,sort:count` は、各都市名のドキュメント数を降順に並べ替えたファセット結果の上位 3 カテゴリを取得します。 たとえば、上位 3 カテゴリが Budget、Motel、Luxury で、Budget が 5 ヒット、Motel が 6 ヒット、Luxury が 4 ヒットである場合、バケットは Motel、Budget、Luxury の順序になります。
  - 例: `facet=rating,sort:-value` では、値の降順で、すべての可能な評価のバケットが生成されます。 たとえば、評価が 1 ～ 5 の場合、各評価に一致したドキュメントの数に関係なく、バケットは 5、4、3、2、1 の順序になります。
- `values` (ファセット エントリ値の動的なセットを指定する、パイプで区切られた数値または `Edm.DateTimeOffset` 値)
  - 例: `facet=baseRate,values:10|20` では、ベース レート 0 以上 10 未満、10 以上 20 未満、20 以上の、3 つのバケットが生成されます。
  - 例: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` では、2010 年 2 月より前に改装されたホテルと、2010 年 2 月 1 日以降に改装されたホテルの、2 つのバケットが生成されます。
- `interval` (数値に対する 0 より大きい整数間隔、または日時値に対する `minute`、`hour`、`day`、`week`、`month`、`quarter`、`year`)
  - 例: `facet=baseRate,interval:100` では、サイズ 100 のベース レート範囲に基づくバケットが生成されます。 たとえば、ベース レートがすべて $60 ～ $600 の範囲である場合、0 ～ 100、100 ～ 200、200 ～ 300、300 ～ 400、400 ～ 500、500 ～ 600 のバケットが生成されます。
  - 例: `facet=lastRenovationDate,interval:year` では、ホテルが改装された各年に対して 1 つのバケットが生成されます。
- **注**: `count` と `sort` 、同じファセット指定で組み合わせることができますとは組み合わせられません `interval` または `values`, 、および `interval` と `values` 一緒に組み合わせることはできません。

> [AZURE.NOTE] 呼び出すときに **検索** POST を使用して、このパラメータの名前は `facets` の代わりに `facet`します。 また、各文字列が個々にファセット式の文字列となる JSON 配列の文字列として指定します。

`$filter=[string]` (省略可能) - 標準の OData 構文で構造化された検索式。 参照してください [OData 式の構文](#ODataExpressionSyntax) の詳細については、Azure Search がサポートする OData 式文法のサブセットです。

> [AZURE.NOTE] 呼び出すときに **検索** POST を使用して、このパラメータの名前は `filter` の代わりに `$filter`します。

`highlight=[string]` (省略可能) - ヒットの強調表示に使用されるコンマで区切られたフィールド名のセット。 `searchable` フィールドのみが、ヒットの強調表示に使用できます。

`highlightPreTag=[string]` (省略可能、既定値は `<em>`) - ヒットの強調表示の前に付加する文字列タグ。 `highlightPostTag` で設定する必要があります。

> [AZURE.NOTE] 呼び出すときに **検索** GET を使用して、URL の予約文字必要があります (たとえば、# ではなく %23)、パーセントでエンコードします。

`highlightPostTag=[string]` (省略可能、既定値は `</em>`) - ヒットの強調表示の後に付加する文字列タグ。 `highlightPreTag` で設定する必要があります。

> [AZURE.NOTE] 呼び出すときに **検索** GET を使用して、URL の予約文字必要があります (たとえば、# ではなく %23)、パーセントでエンコードします。

`scoringProfile=[string]` (省略可能) - 結果の並べ替えを目的として一致するドキュメントのマッチ スコアを評価するためのスコアリング プロファイルの。

`scoringParameter=[string]` (0 以上) - 名前:値の形式を使用して、スコアリング関数で定義されている各パラメーターの値を示します (例: `referencePointParameter`)。 たとえば、スコアリング プロファイルで "mylocation" という名前のパラメーターを持つ関数が定義されている場合、クエリ文字列のオプションは &scoringParameter=mylocation:-122.2,44.8 になります。

> [AZURE.NOTE] 呼び出すときに **検索** POST を使用して、このパラメータの名前は `scoringParameters` の代わりに `scoringParameter`します。 また、各文字列が個々に name:value のペアとなる JSON 配列の文字列として指定します。

`minimumCoverage` (省略可能、既定値は 100) - クエリが成功として報告されるために、検索クエリで照合する必要のあるインデックスの割合を示す 0 ～ 100 の範囲の数値。 既定では、インデックス全体が一致する必要があります。そうでないと、`Search` は HTTP 状態コード 503 を返します。 `minimumCoverage` を設定し、`Search` が成功した場合は、HTTP 200 が返され、応答にはクエリで照合したインデックスの割合を示す `@search.coverage` 値が含められます。

> [AZURE.NOTE] このパラメーターを 100 未満の値に設定は、1 つのみのレプリカのサービスについても検索の可用性を確保するために役立ちます。 ただし、すべての一致するドキュメントが検索結果に含まれると保証されるわけではありません。 アプリケーションにとって可用性よりも検索の再現率が重要である場合は、`minimumCoverage` を既定値の 100 のままにしておくことをお勧めします。

`api-version=[string]` (必須)。 プレビュー バージョンは `api-version=2015-02-28-Preview` です。 参照してください [Search サービスのバージョン](http://msdn.microsoft.com/library/azure/dn864560.aspx) の詳細および代替バージョンです。

注: この操作を `api-version` 呼び出す場合でも、URL にクエリ パラメーターとして指定された **検索** GET または POST にします。

**要求ヘッダー**

次の一覧は、必須と任意の要求ヘッダーについてまとめたものです。

- `api-key`: `api-key` は Search サービスに対する要求の認証に使用されます。 これはサービス URL に固有の文字列値です。  **検索** 要求は、管理者キーまたはクエリ キーのいずれかを指定できます `api-key`します。

要求 URL を作成するにはサービス名も必要です。 サービス名と `api-key` は、Azure クラシック ポータルのサービス ダッシュボードから取得できます。 参照してください [ポータルで Azure Search サービスの作成](search-create-service-portal.md) のページのナビゲーション ヘルプについてです。

**要求本文**

GET の場合: なし。

POST の場合:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

<a name="SearchResponse"></a>
**応答**

状態コード: 応答の成功に対して「200 OK」が返されます。

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if result count exceeds page size and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if result count exceeds page size; Applies to both GET and POST)
    }

**次に例を示します。**

その他の例を見つけることができます、 [Azure Search の OData 式の構文](https://msdn.microsoft.com/library/azure/dn798921.aspx) ページです。

1)  日付で降順に並べ替えられたインデックスを検索します。

    GET/indexes/hotels/docs? 検索 = * & $orderby = lastRenovationDate desc & api バージョン 2015年-02-28-preview を =

    POST/indexes/hotels/docs/search? api バージョン 2015年-02-28-preview を =
    {
      "search":"*"、
      "orderby": ["lastRenovationDate desc"]
    }

2)  ファセット検索で、インデックスを検索し、カテゴリ、レーティング、タグ、および特定の範囲の baseRate の項目をに対するファセットを取得します。

    GET/indexes/hotels/docs でしょうか検索テストとファセットを = = カテゴリ & ファセット = 評価 & ファセット = タグ & ファセット = baseRate、値: 80|150|220 api バージョン 2015-02-28-preview を =。

    POST/indexes/hotels/docs/search? api バージョン 2015年-02-28-preview を =
    {
      "search":"test"、
      「ファセット」: ["category"、「評価」、"tags"、「baseRate, 値: 80|150|220」]
    }

3)  ユーザーがクリックした後は、以前のファセット クエリ結果を絞り込む、フィルターを使用して 3 とカテゴリ"Motel"を評価します。

    GET/indexes/hotels/docs ですか? 検索テストとファセットを = = タグ & ファセット = baseRate、値: 80|150|220 $filter = 評価 eq 3 とカテゴリ eq 'Motel' と api バージョン 2015年-02-28-preview を =

    POST/indexes/hotels/docs/search? api バージョン 2015年-02-28-preview を =
    {
      "search":"test"、
      「ファセット」: ["tags"、「baseRate, 値: 80|150|220」],
      [フィルター]:「評価 eq 3 とカテゴリ eq 'Motel'」
    }

4) ファセット検索では、クエリで返される一意の語句に上限を設定します。 既定値は 10 ですが、`facet` 属性の `count` パラメーターを使用してこの値を増減できます。

    GET/indexes/hotels/docs ですか? 検索テストとファセットを = = 市区町村、数: 5 api バージョン 2015年-02-28-preview を =

    POST/indexes/hotels/docs/search? api バージョン 2015年-02-28-preview を =
    {
      "search":"test"、
      「ファセット」: [「都市, 数: 5」]
    }

5)  特定のフィールド内のインデックスを検索します。たとえば、言語固有フィールド。

    GET/indexes/hotels/docs? 検索 = hôtel & searchFields = description_fr & api バージョン 2015年-02-28-preview を =

    POST/indexes/hotels/docs/search? api バージョン 2015年-02-28-preview を =
    {
      "search":"hôtel"
      "searchFields": ["description_fr"]
    }

6) 複数のフィールドで、インデックスを検索します。 たとえば、複数の言語の検索可能なフィールドをすべて同じインデックスに格納してクエリできます。  英語とフランス語の説明が同じドキュメントに共存している場合、いずれかまたはすべてをクエリ結果で返すことができます。

    GET/indexes/hotels/docs? 検索 = ホテル & searchFields = ネットワークの説明、description_fr api バージョン 2015年-02-28-preview を =

    POST/indexes/hotels/docs/search? api バージョン 2015年-02-28-preview を =
    {
      "search":「ホテル」
      "searchFields": [「説明」、"description_fr"]
    }

クエリできるのは一度に 1 つのインデックスだけであることに注意してください。 一度に 1 つをクエリするのでない限り、言語ごとに複数のインデックスを作成しないでください。

7)  ページング - 項目の最初のページを取得 (ページ サイズは 10)。

    GET/indexes/hotels/docs? 検索 = * & $skip = 0 & $top = 10 と api バージョン 2015年-02-28-preview を =

    POST/indexes/hotels/docs/search? api バージョン 2015年-02-28-preview を =
    {
      "search":"*"、
      [skip]: 0 の場合、
      "top": 10
    }

8)  ページング - 項目の 2 番目のページを取得 (ページ サイズは 10)。

    GET/indexes/hotels/docs? 検索 = * (& a) $skip = 10 & $top = 10 と api バージョン 2015年-02-28-preview を =

    POST/indexes/hotels/docs/search? api バージョン 2015年-02-28-preview を =
    {
      "search":"*"、
      [skip]: 10 日
      "top": 10
    }

9)  特定のフィールド セットを取得します。

    GET/indexes/hotels/docs? 検索 = * & $select = hotelName、説明、および api バージョン 2015年-02-28-preview を =

    POST/indexes/hotels/docs/search? api バージョン 2015年-02-28-preview を =
    {
      "search":"*"、
      "select": ["hotelName"、「説明」]
    }

10)  特定のフィルター式と一致するドキュメントを取得します

    GET /indexes/hotels/docs?$filter=(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/search?api-version=2015-02-28-Preview
    {
      "filter": "(baseRate ge 60 and baseRate lt 300) or hotelName eq 'Fancy Stay'"
    }

11) 検索ヒットを強調表示してフラグメントをインデックスと戻り値

    GET/indexes/hotels/docs? 検索 = 何かと、強調表示 = 説明 & api バージョン 2015年-02-28-preview を =

    POST/indexes/hotels/docs/search? api バージョン 2015年-02-28-preview を =
    {
      "search":「もの」
      "highlight":「説明」
    }

12) インデックスを検索し、並べ替えの参照から離れるほどに近い場所からドキュメントを返す

    GET/indexes/hotels/docs でしょうか検索 = 何か & $orderby=geo.distance (場所、geography'POINT(-122.12315 47.88121)') (& a) api バージョン 2015-02-28-preview を =。

    POST/indexes/hotels/docs/search? api バージョン 2015年-02-28-preview を =
    {
      "search":「もの」
      "orderby": ["geo.distance (場所、geography'POINT(-122.12315 47.88121)')"]
    }

13) 2 つの距離スコアリング関数で"geo"と呼ばれるスコア付けプロファイルがあるものとしてインデックスを検索、「含む」と"lastlocation"パラメーターを定義する 1 つと呼ばれるパラメーターを定義します。

    GET/indexes/hotels/docs ですか? 検索 = 何か & scoringProfile = geo & scoringParameter = 含む:-122.123,44.77233 & scoringParameter = lastLocation:-121.499,44.2113 と api バージョン = 2015年-02-28-プレビュー

    POST/indexes/hotels/docs/search? api バージョン 2015年-02-28-preview を =
    {
      "search":「もの」
      "scoringProfile":"geo"
      "scoringParameters": ["含む:-122.123,44.77233"、"lastLocation:-121.499,44.2113"]
    }

14) ドキュメントを検索インデックスを使用して、 [簡単なクエリ構文](https://msdn.microsoft.com/library/dn798920.aspx)します。 このクエリは、検索可能なフィールドに語句 "comfort" および "location" が含まれていて "motel" が含まれないホテルを返します。

    GET/indexes/hotels/docs? 検索 = 快適性と場所-motel & searchMode = all & api バージョン = 2015年-02-28-プレビュー

    POST/indexes/hotels/docs/search? api バージョン 2015年-02-28-preview を =
    {
      "search":"快適さ + 場所-motel"、
      "searchMode":"all"
    }

上の `searchMode=all` の使用に注意してください。 このパラメーターを含めると、既定の `searchMode=any` が上書きされて、`-motel` が "OR NOT" ではなく "AND NOT" を意味することが保証されます。 `searchMode=all` を指定しないと、検索結果を制限するのではなく拡大する "OR NOT" になり、一部のユーザーにとっては直感に反している可能性があります。

15) ドキュメントを検索インデックスを使用して、 [lucene クエリ構文](http://lucene.apache.org/core/4_10_4/queryparser/org/apache/lucene/queryparser/classic/package-summary.html#Overview)します。 このクエリは、カテゴリ フィールドに "budget" (低料金) が含まれ、すべての検索可能なフィールドに "recently renovated" (最近改修済み) というフレーズが含まれるホテルを返します。 "recently renovated" というフレーズを含むドキュメントは、用語のブースト値 (3) の結果、高いランクになります。

    GET/indexes/hotels/docs ですか? 検索カテゴリの予算を = AND \"recently renovated\"^3 & searchMode = all & api バージョン = 2015年-02-28-preview & querytype = フル

    POST/indexes/hotels/docs/search? api バージョン 2015年-02-28-preview を =
    {
      "search":"カテゴリの予算と \"recently renovated\"^3"
      "queryType":"full"
      "searchMode":"all"
    }

<a name="LookupAPI"></a>
##ドキュメントの参照

 **Lookup Document** 操作は、Azure Search からドキュメントを取得します。 これは、ユーザーが特定の検索結果をクリックして、そのドキュメントに関する詳細を検索するときに役立ちます。

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**要求**

サービス要求には HTTPS が必要です。  **Lookup Document** 要求は次のように作成できます。

    GET /indexes/[index name]/docs/key?[query parameters]

または、従来の OData 構文をキー参照に使用することもできます。

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

要求 URI は [index name] と [key] を含みます。これにより、どのインデックスからどのドキュメントを取得するかが指定されます。 一度に取得できるドキュメントは 1 つだけです。 使用 **検索** を単一の要求で複数のドキュメントを取得します。

**クエリ パラメーター**

`$select=[string]` (省略可能) - 取得するフィールドのコンマ区切りリスト。 指定しない場合、または `*` に設定した場合は、スキーマで取得可能とマークされているすべてのフィールドが含まれます。

`api-version=[string]` (必須)。 プレビュー バージョンは `api-version=2015-02-28-Preview` です。 参照してください [Search サービスのバージョン](http://msdn.microsoft.com/library/azure/dn864560.aspx) の詳細および代替バージョンです。

注: この操作の場合、`api-version` はクエリ パラメーターとして指定します。

**要求ヘッダー**

次の一覧は、必須と任意の要求ヘッダーについてまとめたものです。

- `api-key`: `api-key` は Search サービスに対する要求の認証に使用されます。 これはサービス URL に固有の文字列値です。  **Lookup Document** 要求は、管理者キーまたはクエリ キーのいずれかを指定できます `api-key`します。

要求 URL を作成するにはサービス名も必要です。 サービス名と `api-key` は、Azure クラシック ポータルのサービス ダッシュボードから取得できます。 参照してください [ポータルで Azure Search サービスの作成](search-create-service-portal.md) のページのナビゲーション ヘルプについてです。

**要求本文**

ありません。

**Response**

状態コード: 応答の成功に対して「200 OK」が返されます。

    {
      field_name: field_value (fields matching the default or specified projection)
    }

**例**

キー '2' を持つドキュメントを参照します。

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

OData 構文を使用して、キー '3' を持つドキュメントを参照します。

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>
##Count Documents (Azure Search) (ドキュメントのカウント (Azure Search))

 **Count Documents** 操作は、検索インデックス内のドキュメントの数の数を取得します。 `$count` 構文は、OData プロトコルの一部です。

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**要求**

サービス要求には HTTPS が必要です。  **Count Documents** 要求は、GET メソッドを使用して作成できます。

要求 URI の [index name] では、指定したインデックスのドキュメント コレクション内のすべての項目の数を返すようにサービスに指示します。

`api-version=[string]` (必須)。 プレビュー バージョンは `api-version=2015-02-28-Preview` です。 参照してください [Search サービスのバージョン](http://msdn.microsoft.com/library/azure/dn864560.aspx) の詳細および代替バージョンです。

**要求ヘッダー**

次の一覧は、必須と任意の要求ヘッダーについてまとめたものです。

- `Accept`: この値は `text/plain` に設定する必要があります。
- `api-key`: `api-key` は Search サービスに対する要求の認証に使用されます。 これはサービス URL に固有の文字列値です。  **Count Documents** 要求は、管理者キーまたはクエリ キーのいずれかを指定できます `api-key`します。

要求 URL を作成するにはサービス名も必要です。 サービス名と `api-key` は、Azure クラシック ポータルのサービス ダッシュボードから取得できます。 参照してください [ポータルで Azure Search サービスの作成](search-create-service-portal.md) のページのナビゲーション ヘルプについてです。

**要求本文**

ありません。

**Response**

状態コード: 応答の成功に対して「200 OK」が返されます。

応答本文には、プレーン テキスト形式の整数として、カウント値が含まれます。

<a name="Suggestions"></a>
##検索候補

 **提案** 操作は部分的な検索入力に基づいて検索候補を取得します。 通常は、ユーザーが検索語句を入力するときに、先行入力の候補を提供するために検索ボックスで使用されます。

Suggestion 要求はターゲット ドキュメントの検索候補を示すことを目的としているので、複数の候補ドキュメントが同じ検索入力と一致した場合、検索候補のテキストが繰り返される場合があります。 `$select` を使用して (ドキュメント キーを含む) 他のドキュメント フィールドを取得し、各検索候補のソースであるドキュメントを示すことができます。

A **提案** 操作は GET または POST 要求として発行します。

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**GET ではなく、POST を使用する場合**

使用すると HTTP GET を呼び出す、 **提案** API、要求 URL の長さが 8 KB を超えることはできないことに留意する必要があります。 これは通常、ほとんどのアプリケーションで十分な長さです。 ただし、一部のアプリケーション、具体的には OData フィルター式では、非常に大きなクエリが生成されます。 これらのアプリケーションについては、HTTP POST の使用をお勧めします。 POST の要求サイズの上限は 17 MB に近いため、最も複雑なクエリでも十分な長さとなります。

**要求**

サービス要求には HTTPS が必要です。  **提案** GET または POST メソッドを使用して要求を作成できます。

要求 URI では、クエリするインデックスの名前を指定します。 部分的に入力する検索語のようなパラメーターは、GET 要求の場合はクエリ文字列に指定し、POST 要求の場合は要求本文に指定します。

GET 要求を作成する際のベスト プラクティス、として必ず [URL エンコード](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) REST API を直接呼び出すときに、特定のクエリ パラメーターです。  **提案** 操作、これが含まれます。

- `$filter`
- `highlightPreTag`
- `highlightPostTag`
- `search`

URL エンコードは、上記のクエリ パラメーターにのみ推奨されます。 誤ってクエリ文字列全体 (? より後のすべて) を URL エンコードした場合は、要求が中断します。

また、URL エンコードは、GET を使用して REST API を直接呼び出すときにのみ必要です。 URL エンコードは必要ありませんを呼び出すときに **提案** 、POST を使用してを使用する場合、または、 [.NET クライアント ライブラリ](https://msdn.microsoft.com/library/dn951165.aspx), 、URL のエンコーディングを処理します。

**クエリ パラメーター**

**検索候補** はクエリ条件を提供しても検索の動作を指定するいくつかのパラメーターを受け取ります。 呼び出すときに、これらのパラメーターを URL にクエリ文字列を提供する **提案** を呼び出すときは、要求本文に JSON プロパティと、GET を介して **提案** POST を介してします。 いくつかのパラメーターの構文は、GET および POST の間で多少異なります。 その違いは、以下で随時説明しています。

`search=[string]` - クエリの検索候補を示すために使用する検索テキスト。 1 文字以上 100 文字以下で指定する必要があります。

`highlightPreTag=[string]` (省略可能) - 検索ヒットの前に付加する文字列タグ。 `highlightPostTag` で設定する必要があります。

> [AZURE.NOTE] 呼び出すときに **提案** GET を使用して、URL の予約文字必要があります (たとえば、# ではなく %23)、パーセントでエンコードします。

`highlightPostTag=[string]` (省略可能) - 検索ヒットの後に付加する文字列タグ。 `highlightPreTag` で設定する必要があります。

> [AZURE.NOTE] 呼び出すときに **提案** GET を使用して、URL の予約文字必要があります (たとえば、# ではなく %23)、パーセントでエンコードします。

`suggesterName=[string]` - インデックス定義の一部である `suggesters` コレクションで指定されているサジェスターの名前。 `suggester` は、検索候補のクエリ語句がスキャンされるフィールドを決定します。 参照してください [サジェスター](#Suggesters) 詳細です。

`fuzzy=[boolean]` (省略可能、既定値 = false) - true に設定すると、検索テキストに置き換えられた文字または存在しない文字がある場合であっても、この API は検索候補を探します。 あいまい一致で検索候補を検索すると低速で多くのリソースを消費するため、一部のシナリオではエクスペリエンスがよくなりますが、パフォーマンスは低下します。

`searchFields=[string]` (省略可能) - 指定した検索テキストを検索するフィールド名のコンマ区切りのリスト。 対象のフィールドは、検索候補が有効になっている必要があります。

`$top=#` (省略可能、既定値 = 5) - 取得する検索候補の数。 1 ～ 100 の数値を指定する必要があります。

> [AZURE.NOTE] 呼び出すときに **提案** POST を使用して、このパラメータの名前は `top` の代わりに `$top`します。

`$filter=[string]` (省略可能) - 検索候補と考えられるドキュメントをフィルター処理する式。

> [AZURE.NOTE] 呼び出すときに **提案** POST を使用して、このパラメータの名前は `filter` の代わりに `$filter`します。

`$orderby=[string]` (省略可能) - 結果を並べ替えるための式のコンマ区切りのリスト。 各式は、フィールド名または `geo.distance()` 関数の呼び出しです。 各式に続いて、昇順を示す `asc` または降順を示す `desc` を指定できます。 既定値は昇順です。 `$orderby` には 32 句の制限があります。

> [AZURE.NOTE] 呼び出すときに **提案** POST を使用して、このパラメータの名前は `orderby` の代わりに `$orderby`します。

`$select=[string]` (省略可能) - 取得するフィールドのコンマ区切りリスト。 指定しないと、ドキュメント キーと検索候補テキストのみが返されます。

> [AZURE.NOTE] 呼び出すときに **提案** POST を使用して、このパラメータの名前は `select` の代わりに `$select`します。

`minimumCoverage` (省略可能、既定値は 80) - クエリが成功として報告されるために、検索候補クエリで照合する必要があるインデックスの割合を示す 0 ～ 100 の範囲の数値。 既定では、インデックスの 80 % 以上が一致している必要があります。そうでないと、`Suggest` は HTTP 状態コード 503 を返します。 `minimumCoverage` を設定し、`Suggest` が成功した場合は、HTTP 200 が返され、応答にはクエリで照合したインデックスの割合を示す `@search.coverage` 値が含められます。

> [AZURE.NOTE] このパラメーターを 100 未満の値に設定は、1 つのみのレプリカのサービスについても検索の可用性を確保するために役立ちます。 ただし、すべての一致する候補が結果に含まれると保証されるわけではありません。 アプリケーションにとって可用性よりも再現率が重要である場合は、`minimumCoverage` が既定値の 80 を下回らないようにすることをお勧めします。

`api-version=[string]` (必須)。 プレビュー バージョンは `api-version=2015-02-28-Preview` です。 参照してください [Search サービスのバージョン](http://msdn.microsoft.com/library/azure/dn864560.aspx) の詳細および代替バージョンです。

注: この操作を `api-version` は呼び出す場合でも、URL にクエリ パラメーターとして指定 **提案** GET または POST にします。

**要求ヘッダー**

以下では、必須と任意の要求ヘッダーについて説明します。

- `api-key`: `api-key` は Search サービスに対する要求の認証に使用されます。 これはサービス URL に固有の文字列値です。  **提案** 要求は、管理者キーまたはとしてクエリ キーのいずれかを指定できます、 `api-key`です。

要求 URL を作成するにはサービス名も必要です。 サービス名と `api-key` は、Azure クラシック ポータルのサービス ダッシュボードから取得できます。 参照してください [ポータルで Azure Search サービスの作成](search-create-service-portal.md) のページのナビゲーション ヘルプについてです。

**要求本文**

GET の場合: なし。

POST の場合:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Response**

状態コード: 応答の成功に対して「200 OK」が返されます。

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

予測オプションがフィールドの取得に使用された場合、配列の各要素に含められます。

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**例**

部分的な検索入力が「lux」である場合に、5 個の検索候補を取得します。

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }

