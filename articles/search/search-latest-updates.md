<properties 
    pageTitle="Azure Search 関連の最新の更新履歴情報 | Microsoft Azure | ホスト型クラウド検索サービス" 
    description="サービスの最新の更新履歴情報を記述する Azure Search のリリース ノート" 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="11/04/2015" 
    ms.author="heidist"/>

#Azure Search 関連の最新の更新履歴情報#

Azure Search は、Microsoft Azure のホスト型クラウド検索サービスです。 一般提供のサポートされる構成についての 99.9% の可用性サービス レベル アグリーメント (SLA) を提供することが、 [API の 2015年-02-28 バージョン](https://msdn.microsoft.com/library/azure/dn798935.aspx)です。

##機能のバージョン管理とロール アウト

機能は個別にまたは作業負荷とアプリケーションを通じて、リリース、 [REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx), 、[.NET SDK](http://go.microsoft.com/fwlink/?LinkId=528216), 、またはサービスのダッシュ ボード、 [Azure Classic Portal](https://portal.azure.com)します。

.NET ライブラリおよび REST API の両方に、複数のバージョンがあります。 新機能がロール アウトされた後も、従来の API は引き続き動作します。 お客様がアクセスできる [Search サービスのバージョン](https://msdn.microsoft.com/library/azure/dn864560.aspx) をバージョン管理ポリシーの詳細について参照してください。


##API バージョン 2015-02-28-Preview
**再リリース: 2015 年 9 月**

このバージョンを新規追加 [Lucene クエリ構文でサポート](https://msdn.microsoft.com/library/azure/mt589323.aspx) に対して使用できる、 [Azure Search サービス REST API のプレビュー バージョン](search-api-2015-02-28-preview.md)です。 新しい構文を使用するには、Search Documents 操作で `queryType`を指定する必要があります。

さらに、次の機能の両方のプレビューでは終了移され、MSDN の公式の API の一部であるようになりました。
- [自然言語プロセッサ](search-language-support.md)
- 検索、提案、および参照クエリの POST

##.NET SDK 0.10.0-Preview
**リリース日: 2015 年 8 月**

これは、.NET クライアント ライブラリ Microsoft.Azure.Search.dll の、2 番目のイテレーションです。
 このバージョンでは、.NET クラスを使用してインデクサーを作成、管理、および使用できるようになりました。 また、Azure SQL インデクサーの場合、地理ポイントのインデックス作成が新しくサポートされます。


- [Indexers クラス](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.indexer.aspx)
- [DataSource クラス](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.datasource.aspx)

##.NET SDK 0.9.6-Preview
**リリース日: 2015 年 3 月 5日**

これは、Azure Search 用 .NET SDK の最初のパブリック リリースです。 これには、以下の 2 つの名前空間を持つ、クライアント ライブラリ Microsoft.Azure.Search.dll が含まれています。

- [Microsoft.Azure.Search](https://msdn.microsoft.com/library/azure/microsoft.azure.search.aspx)
- [Microsoft.Azure.Search.Models](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.aspx)

以下については除外されています。

- [インデクサー](http://go.microsoft.com/fwlink/p/?LinkId=528173) (この機能は、0.10.0-preview バージョンでは除外されなく)
- [管理 REST API](https://msdn.microsoft.com/library/azure/dn832684.aspx)
- [2015-02-28-preview](search-api-2015-02-28-Preview.md) 機能 (現在、プレビュー専用機能で構成されている Microsoft 自然言語プロセッサと `moreLikeThis`)。

参照してください [.NET で Azure Search を使用する方法](http://go.microsoft.com/fwlink/p/?LinkId=528088) については、SDK のインストールおよび使用します。

##API バージョン 2015-02-28-Preview
**リリース日: 2015 年 4 月 22日**

- インデクサーは、fieldMapping コンストラクトをサポートするようになりました。これは実際のフィールド名が外部データベースと Azure Search インデックスとの間で異なる場合に、フィールド割り当てを提供します。 参照してください [インデクサー](search-api-indexers-2015-02-28-Preview.md) の `2015-02-28-preview` インデクサーの資料についてのバージョン.

- さらにこのプレビューの更新では、インデクサーはフィールドの型変換をサポートするようになったため、ソース フィールドが JSON 配列を表すと仮定して、SQL テーブル内の文字列フィールドを、検索インデックス内の文字列コレクション フィールドにマッピングできます。


**リリース日: 2015 年 3 月 5 日**

- [Microsoft 自然言語プロセッサ](search-api-2015-02-28-Preview.md) 詳細の言語および Office と Bing でサポートされるすべての言語の広範な語幹検索のサポートを表示します。

- [moreLikeThis =](search-api-2015-02-28-Preview.md) 排他の検索パラメーターを相互には `search=`, 、クエリの代替実行パスをトリガーします。 `moreLikeThis=` は、検索用語の入力に基づく `search=` のフルテキスト検索ではなく、指定されたドキュメントに類似のドキュメントを、検索可能フィールドの比較によって検索します。

##API バージョン 2015-02-28
**リリース日: 2015 年 3 月 5日**

- [インデクサー](http://go.microsoft.com/fwlink/p/?LinkID=528210) 新しい機能であり、Azure SQL Database、Azure DocumentDB と Azure Vm 上の SQL Server 上のデータ ソースからのインデックス作成を大幅に簡略化します。

- [サジェスター](https://msdn.microsoft.com/library/azure/dn798936.aspx) 挿入辞一致のサポートを追加して、以前の実装 (での一致のみプレフィックス) より制限の先行入力クエリ サポートを置き換えています。 この実装では、語中のどの位置にある一致でも検出できます。さらに、あいまい一致もサポートしています。

- [タグ ブースト](http://go.microsoft.com/fwlink/p/?LinkId=528212) によってスコアリング プロファイルのための新しいシナリオが実現します。 具体的には、持続データ (ショッピングの嗜好など) を活用し、パーソナライズされた情報に基づいて、個々のユーザーに対する検索結果の精度を向上させることができます。 

参照してください [Azure Search は一般公開されました](http://go.microsoft.com/fwlink/p/?LinkId=528211) サービスの発表、Azure ブログのこれらすべての機能について説明しています。

##API バージョン 2014-10-20-Preview
**リリース日: 2014 年 11 月、2015 年 1 月**

- [Lucene 言語アナライザー](search-api-2014-10-20-preview.md) Lucene と共に配布されるカスタム言語アナライザーの多言語のサポートを提供するようになりました。 

- スコアリング プロファイルを含め、インデックス作成のツールのサポートが導入された、 [Azure Classic Portal](https://portal.azure.com)します。

##API バージョン 2014-07-31-Preview
**リリース日: 2014 年 8 月 21日**

このバージョンは、次のコア機能を提供する、Azure Search のパブリック プレビュー リリースでした。

- インデックスとドキュメントの操作のための REST API。 この API バージョンの大半は、2015-02-28 バージョンでも変更されていません。 バージョンのドキュメントを `2014-07-31-Preview` ご覧 [Azure Search Service REST API バージョン 2014年-07-31-preview](search-api-2014-07-31-preview.md)します。

- 検索結果のチューニングのためのスコアリング プロファイル。 スコアリング プロファイルは、検索スコアの計算に使用する条件を追加します。 この機能のドキュメントは掲載されて [Azure Search Service スコア付けプロファイル REST API バージョン 2014年-07-31-preview](search-api-scoring-profiles-2014-07-31-preview.md)します。

- 地理空間サポートは当初より利用可能です。これは最初から Azure Search の一部であった `Edm.GeographyPoint` データ型により提供されます。

- プレビュー バージョンでのプロビジョニング、 [Azure Classic Portal](https://portal.azure.com )します。 Azure Search は、新しいポータルでのみ使用できるいくつかのサービスの 1 つでした。

##管理 API バージョン 2015-08-19
**リリース日: 2015 年 9 月 11日**

[管理 REST API](https://msdn.microsoft.com/library/azure/dn832684.aspx) 次の更新プログラムが含まれています。

- checkNameAvailability は、所与のサービス名が既に使用されていないか確認します。
- レプリカの範囲は以前は 1 ～ 6 でしたが、現在は 1 ～ 12 です。
- SKU プロパティはプロパティ バッグからサービス ペイロードの最上位に移動されました。
- 検索サービスの作成操作の要求本文が更新され、SKU 設定の再配置が追加されました。

##管理 API バージョン 2015-02-28
**リリース日: 2015 年 3 月 5日**

[管理 REST API](search-management-api-2014-02-28.md) Azure Search の一般提供リリースに属している、管理 API の最初のバージョンを示します。 以前のプレビューとこのプレビューでは、機能の相違はありません。

##管理 API バージョン 2014-07-31-Preview
**リリース日: 2014 年 10 月**

プレビュー リリースの [管理 REST API](search-management-api-2014-07-31-preview.md) プログラムを使用してサービスの管理をサポートするようになりました。 これは、サービス REST API とは独立してバージョン管理されます。


 
