<properties 
    pageTitle="API プレビュー バージョン 2014* から API バージョン 2015* への移行  | Microsoft Azure | ホスト型クラウド検索サービス" 
    description="重大な変更、および 2014-07-31-Preview または 2014-10-20-Preview 向けに記述されたコードを Azure Search API バージョン 2015-02-28 に移行する方法を説明します。" 
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


# API バージョン 2014* から API バージョン 2015* へのプレビューの移行

Azure Search がホストされているクラウドの Microsoft Azure でサービスを検索します。 以下のガイダンスは、Azure Search のプレビュー バージョンでカスタム アプリケーションを構築し、現在一般提供されている 2015-02-28 リリースに移行しているユーザー向けです。

プレビューを利用されているユーザーは、次のいずれかのプレビュー バージョンを使用された可能性があります。

- 2014-07-31-Preview
- 2014-10-20-Preview

Azure Search は一般公開されているので、新しいリリースに移行されることをお勧めします。2015-02-28 は、一般提供されている Azure Search リリースの公式 API バージョンです。 このバージョンに記載されています。 [MSDN](https://msdn.microsoft.com/library/azure/dn798933.aspx)します。

私たちも展開し、次のプレビュー バージョンを [2015年-02-28-preview](search-api-2015-02-28-preview.md), 、まだ開発段階に追加された機能を紹介します。 いずれかでプレビュー API についてフィードバックすることができます、 [Azure Search フォーラム](https://social.msdn.microsoft.com/forums/azure/home?forum=azuresearch) または [フィードバック ページ](http://feedback.azure.com/forums/263029-azure-search)します。

### 移行時のチェックリスト

- 重大な変更によって、ご利用のソリューションが影響を受けるかどうかを検討する。
- API バージョンをバンプ `2015年-02-28` ロックされたバージョン。 このバージョンは SLA でサポートされています。 そのため、問題が発生した場合はより迅速に解決できます。
- 構築、デプロイ、テストする。 重大な変更を除き、検索動作では 100% のパリティが必要です。
- 実稼働環境に展開する。
- 将来の機能導入を検討できるように、新機能を評価する。 2015-02-28-preview をテストする場合は、ドライブ、再び移行 Microsoft 自然言語プロセッサまたは `morelikethis`します。

## API バージョン 2015* での重大な変更

この API の最初のリリースには、オート コンプリートまたは入力に先行した候補表示機能が含まれていました。 この機能は便利でしたが、プレフィックスのマッチング機能に限られていたので、検索対象は検索用語の最初の数文字のみで、フィールド内の他の部分は対象になっていませんでした。 この実装と呼ばれるブール型プロパティ `提案` 設定する `true` を特定のフィールドでプレフィックスのマッチングを有効にする必要がある場合。

この元の実装は、新しいを優先して使用しないで `サジェスター` コンス トラクターで定義されている、 [インデックス](https://msdn.microsoft.com/library/azure/dn798941.aspx) 挿入辞一致とあいまい一致機能します。 名称からわかるように、挿入辞一致とあいまい一致が可能になると一致検索機能は強化されます。 挿入辞一致にはプレフィックスも含まれるため、先頭の文字による一致検索も引き続き可能ですが、文字列の残りの部分も一致検索対象に含まれるよう機能が拡張されています。

前の実装 (前述のブール型プロパティ) を中止することにしたのは、新しいソリューションを構築しているユーザーが誤って導入しないようにするためです。この実装は、いずれの 2015 バージョンにも含まれておらず、下位互換性もありません。 使用する場合に `2015年-02-28` または `2015年-02-28-preview` 新しいを使用する必要があります `サジェスター` を先行入力クエリを有効にする構成要素。

## 既存コードの移行

この候補表示プロパティが、唯一の重大な変更です。 移行したことができますこのプロパティを使用しない場合、 `api バージョン` から `2014年-07-31-preview` または `2014年-10-20-preview` と `2015年-02-28`, 、リビルドして、再展開します。 アプリケーションは、以前と同様に動作します。

候補表示機能を実装したカスタム アプリケーションによって、以下を実行できます。

1. すべての NuGet パッケージを更新する。
1. Api バージョンをバンプ `2015年-02-28`します。 以下のコード サンプルを使用する場合、 API バージョンは **AzureSearchHelper** クラスに含まれます。
1. 削除、 `提案 = {true | false}` インデックスを定義する JSON スキーマからの属性です。
1. 構造を追加するインデックスの下部にある `サジェスター` (のように、 [後](#after) セクション)。
1. ご利用サービスに発行できるかを確認する (名前の競合を回避するために、インデックスの名前を変更しなければならない場合があります)。
1. ソリューションをリビルドし、テスト環境にデプロイする。
1. すべてのテスト ケースを実行して、ソリューションが想定どおりに動作することを確認する。
1. 実稼働環境に展開する。

コード例から、 [codeplex の Adventure Works サンプル](https://azuresearchadventureworksdemo.codeplex.com/) 元が `提案` 実装します。 このサンプルを使用して、サンプル コードを使用したコード移行を練習するのをお勧めします。

次のセクションで紹介、 [する前に](#before) と [後](#after) 検索候補の実装です。 置き換えることができます、 **CreateCatalogIndex()** メソッド内のバージョンを [後](#after) セクションを作成し、新しい機能を試してくださいソリューションを配置します。

<a name="before"></a>
### 実装する前

ご覧のとおり、 `提案` は、ブール型プロパティの各フィールドに設定します。 すべての削除、 `提案` 属性です。

        private static void CreateCatalogIndex()
        {
            var definition = new 
            {
                Name = "catalog",
                Fields = new[] 
                { 
                    new { Name = "productID",        Type = "Edm.String",         Key = true,  Searchable = false, Filterable = false, Sortable = false, Facetable = false, Retrievable = true,  Suggestions = false },
                    new { Name = "name",             Type = "Edm.String",         Key = false, Searchable = true,  Filterable = false, Sortable = true,  Facetable = false, Retrievable = true,  Suggestions = true  },
                    new { Name = "productNumber",    Type = "Edm.String",         Key = false, Searchable = true,  Filterable = false, Sortable = false, Facetable = false, Retrievable = true,  Suggestions = true  },
                    new { Name = "color",            Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = true,  Facetable = true,  Retrievable = true,  Suggestions = false },
                    new { Name = "standardCost",     Type = "Edm.Double",         Key = false, Searchable = false, Filterable = false, Sortable = false, Facetable = false, Retrievable = true,  Suggestions = false },
                    new { Name = "listPrice",        Type = "Edm.Double",         Key = false, Searchable = false, Filterable = true,  Sortable = true,  Facetable = true,  Retrievable = true,  Suggestions = false },
                    new { Name = "size",             Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = true,  Facetable = true,  Retrievable = true,  Suggestions = false },
                    new { Name = "weight",           Type = "Edm.Double",         Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = true,  Retrievable = true,  Suggestions = false },
                    new { Name = "sellStartDate",    Type = "Edm.DateTimeOffset", Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = false, Retrievable = false, Suggestions = false },
                    new { Name = "sellEndDate",      Type = "Edm.DateTimeOffset", Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = false, Retrievable = false, Suggestions = false },
                    new { Name = "discontinuedDate", Type = "Edm.DateTimeOffset", Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = false, Retrievable = true,  Suggestions = false },
                    new { Name = "categoryName",     Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = false, Facetable = true,  Retrievable = true,  Suggestions = true  },
                    new { Name = "modelName",        Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = false, Facetable = true,  Retrievable = true,  Suggestions = true  },
                    new { Name = "description",      Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = false, Facetable = false, Retrievable = true,  Suggestions = false }
                }
            };

<a name="after"></a>
### 実装した後

移行されたスキーマ定義を省略、 `提案` プロパティを追加し、 `サジェスター` 下部を構築します。

        private static void CreateCatalogIndex()
        {
            var definition = new 
            {
                Name = "catalog",
                Fields = new[] 
                { 
                    new { Name = "productID",        Type = "Edm.String",         Key = true,  Searchable = false, Filterable = false, Sortable = false, Facetable = false, Retrievable = true },
                    new { Name = "name",             Type = "Edm.String",         Key = false, Searchable = true,  Filterable = false, Sortable = true,  Facetable = false, Retrievable = true },
                    new { Name = "productNumber",    Type = "Edm.String",         Key = false, Searchable = true,  Filterable = false, Sortable = false, Facetable = false, Retrievable = true },
                    new { Name = "color",            Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = true,  Facetable = true,  Retrievable = true },
                    new { Name = "standardCost",     Type = "Edm.Double",         Key = false, Searchable = false, Filterable = false, Sortable = false, Facetable = false, Retrievable = true },
                    new { Name = "listPrice",        Type = "Edm.Double",         Key = false, Searchable = false, Filterable = true,  Sortable = true,  Facetable = true,  Retrievable = true },
                    new { Name = "size",             Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = true,  Facetable = true,  Retrievable = true },
                    new { Name = "weight",           Type = "Edm.Double",         Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = true,  Retrievable = true },
                    new { Name = "sellStartDate",    Type = "Edm.DateTimeOffset", Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = false, Retrievable = false },
                    new { Name = "sellEndDate",      Type = "Edm.DateTimeOffset", Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = false, Retrievable = false },
                    new { Name = "discontinuedDate", Type = "Edm.DateTimeOffset", Key = false, Searchable = false, Filterable = true,  Sortable = false, Facetable = false, Retrievable = true },
                    new { Name = "categoryName",     Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = false, Facetable = true,  Retrievable = true },
                    new { Name = "modelName",        Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = false, Facetable = true,  Retrievable = true },
                    new { Name = "description",      Type = "Edm.String",         Key = false, Searchable = true,  Filterable = true,  Sortable = false, Facetable = false, Retrievable = true }
                },
                suggesters = new[]
                {
                new {
                    name = "sg",
                    searchMode = "analyzingInfixMatching",
                    sourceFields = new []{"name", "productNumber", "categoryName", "modelName", "description"}
                    }
                 }
            };

## 新機能とアプローチの評価

ソリューションを移行し、期待どおりに実行されることを確認できましたら、次に示すリンク先で新機能に関する情報を参照してください。

- [Azure Search は一般公開 (ブログの投稿)](http://go.microsoft.com/fwlink/p/?LinkId=528211)
- [Azure Search には、最新の更新プログラムの新します。](search-latest-updates.md)
- [Azure Search とは何ですか。](search-what-is-azure-search.md)

## 問い合わせ

API バージョン `2015年-02-28` SLA でサポートされています。 サポート オプションとのリンクを使って [このページ](../support/options/) サポート チケットを申請します。






