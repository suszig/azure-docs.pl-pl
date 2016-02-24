<properties 
    pageTitle="DocumentDB .NET SDK | Microsoft Azure" 
    description="リリース日、提供終了日、DocumentDB .NET SDK の各バージョン間の変更など、.NET SDK に関するあらゆる詳細を提供します。" 
    services="documentdb" 
    documentationCenter=".net" 
    authors="ryancrawcour" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="11/16/2015" 
    ms.author="ryancraw"/>

# DocumentDB SDK

> [AZURE.SELECTOR]
- [.NET SDK](documentdb-sdk-dotnet.md)
- [Node.js SDK](documentdb-sdk-node.md)
- [Java SDK](documentdb-sdk-java.md)
- [Python SDK](documentdb-sdk-python.md)

##DocumentDB .NET SDK

<table>
<tr><td>**ダウンロード**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>
<tr><td>**ドキュメント**</td><td>[.NET SDK Reference Documentation](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>
<tr><td>**サンプル**</td><td>[.NET Code Samples](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)</td></tr>
<tr><td>**開始**</td><td>[Get started with the DocumentDB .NET SDK](documentdb-get-started.md)</td></tr>
<tr><td>**現在に Framework がサポートされています。**</td><td>[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## リリース ノート

### <a name="1.5.2"/>[1.5.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.2)
  - ページング、条件付きの式、および範囲比較用の新しい演算子を使用できるように LINQ を拡張しました。
    - LINQ で SELECT TOP 動作を有効にする Take 演算子
    - 文字列の範囲の比較を有効にする CompareTo 演算子
    - 条件付き (?) および合体演算子 (??)
  - **[固定]** ArgumentOutOfRangeException を含むモデルのプロジェクションを組み合わせる際に linq クエリで Where です。  [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="1.5.1"/>[1.5.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.1)
 - **[固定]** LINQ プロバイダーがプロジェクションが想定されていませんし、選択を生成 Select は、最後の式ではない場合は、* 不適切です。  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="1.5.0"/>[1.5.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.0)
 - 実装された Upsert、追加された UpsertXXXAsync メソッド
 - すべての要求のパフォーマンス改善
 - LINQ プロバイダーによる文字列の条件、結合、CompareTo メソッドのサポート
 - **[固定]** LINQ プロバイダー ・ IEnumerable と配列と同じ SQL を生成するリストのメソッドの実装が含まれています
 - **[固定]** BackoffRetryUtility を使用して同じ HttpRequestMessage もう一度再試行時に新しいレコードを作成する代わりに
 - **[廃止]** UriFactory.CreateCollection--> UriFactory.CreateDocumentCollection を使用してください
 
### <a name="1.4.1"/>[1.4.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.1)
 - **[固定]** NL-NL などなどの非 en カルチャ情報を使用する場合、問題をローカライズします。 
 
### <a name="1.4.0"/>[1.4.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.0)
  - ID ベースのルーティング
    - ID ベースのリソース リンクの構築に役立つ新しい UriFactory ヘルパー
    - URI に取り入れる DocumentClient の新しいオーバーロード
  - 地理空間の LINQ で IsValid() と IsValidDetailed() を追加
  - LINQ プロバイダー サポートの機能強化
    - **数値演算** -Abs、Acos、Asin、Atan、Ceiling、Exp、Floor、ログ、Log10、Pow、ラウンド、記号、Sin、Sqrt、Tan、Cos、Truncate
    - **文字列** -Concat、含む、EndsWith、IndexOf、カウント、ToLower、TrimStart、置換、TrimEnd、StartsWith、SubString、ToUpper を取り消すと、
    - **配列** -Concat、カウントを含む、
    - **IN** 演算子

### <a name="1.3.0"/>[1.3.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.3.0)
  - インデックス作成ポリシーを変更するためのサポートを追加
    - DocumentClient の新しい ReplaceDocumentCollectionAsync メソッド
    - 対応する ResourceResponse 新しい IndexTransformationProgress プロパティ<T> インデックス ポリシーの変更のパーセントの進行状況の追跡
    - DocumentCollection.IndexingPolicy は変化可能になりました
  - 空間インデックス作成とクエリのサポートを追加
    - 点や多角形など、空間型のシリアル化/逆シリアル化するための新しい Microsoft.Azure.Documents.Spatial 名前空間
    - DocumentDB に格納されている GeoJSON データにインデックスを作成するための新しい SpatialIndex クラス
  - **[固定]** : Linq 式から生成された SQL クエリを間違っている [#38](https://github.com/Azure/azure-documentdb-net/issues/38)

### <a name="1.2.0"/>[1.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.2.0)
- Newtonsoft.Json v5.0.7 の依存関係 
- Order By 対応になりました
  - LINQ プロバイダーによる OrderBy() または OrderByDescending() のサポート
  - Order By をサポートするための IndexingPolicy 
  
        **NB: Possible breaking change** 
  
        If you have existing code that provisions collections with a custom indexing policy, then your existing code will need to be updated to support the new IndexingPolicy class. If you have no custom indexing policy, then this change does not affect you.

### <a name="1.1.0"/>[1.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.1.0)
- 新しい HashPartitionResolver クラス、RangePartitionResolver クラス、IPartitionResolver を使用し、データをパーティショニング
- DataContract シリアル化
- LINQ プロバイダーの GUID サポート
- LINQ の UDF サポート

### <a name="1.0.0"/>[1.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.0.0)
- GA SDK

> [AZURE.NOTE]
プレビューと GA の間で NuGet パッケージ名の変更がありました。 移動して **Microsoft.Azure.Documents.Client** に **Microsoft.Azure.DocumentDB**
<br/>


### <a name="0.9.x-preview"/>[0.9.x-preview](https://www.nuget.org/packages/Microsoft.Azure.Documents.Client)
- プレビュー SDK [廃止]

## リリース日と提供終了日
Microsoft は、通知には、少なくとも **12 か月間** 新しいサポートされるバージョンへの移行を促進するため、SDK の破棄を前もってお客様にします。

新機能および機能と最適化が現在の SDK にのみ追加、そのためには、アップグレードすること勧め常に、最新の SDK バージョンにできるだけ早くできるだけします。 

提供終了の SDK で DocumentDB に要求した場合、サービスにより却下されます。

> [AZURE.WARNING]
Azure DocumentDB SDK for .NET のバージョンより前のすべてのバージョン **1.0.0** に終了 **2016 年 2 月 29 日**します。 
 
<br/>
 
| バージョン | リリース日 | 提供終了日 
| ---     | ---          | ---
| [1.5.2](#1.5.2) | 2015 年 12 月 14 日 |---
| [1.5.1](#1.5.1) | 2015 年 11 月 23 日 |---
| [1.5.0](#1.5.0) | 2015 年 10 月 5 日 |---
| [1.4.1](#1.4.1) | 2015 年 8 月 25 日 |---
| [1.4.0](#1.4.0) | 2015 年 8 月 13 日 |---
| [1.3.0](#1.3.0) | 2015 年 8 月 5 日 |---
| [1.2.0](#1.2.0) | 2015 年 7 月 6 日 |---
| [1.1.0](#1.1.0) | 2015 年 4 月 30 日 |---
| [1.0.0](#1.0.0) | 2015 年 4 月 8 日 |---
| [0.9.3-prelease](#0.9.x-preview) | 2015 年 3 月 12 日 | 2016 年 2 月 29 日
| [0.9.2-prelease](#0.9.x-preview) | 2015 年 1 月、 | 2016 年 2 月 29 日
| [.9.1-prelease](#0.9.x-preview) | 2014 年 10 月 13 日 | 2016 年 2 月 29 日
| [0.9.0-prelease](#0.9.x-preview) | 2014 年 8 月 21 日 | 2016 年 2 月 29 日

## FAQ
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## 関連項目

DocumentDB の詳細については、次を参照してください。 [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) サービス ページです。 

