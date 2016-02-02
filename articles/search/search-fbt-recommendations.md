<properties
   pageTitle="Azure Search "Frequently Bought Together" recommendations | Microsoft Azure | Apache Mahout | Azure Machine Learning"
   description="Frequently Bought Together、新規のか、購入に関する推奨事項を Apache Mahout または Azure Machine Learning を使用して Azure Search アプリケーションに追加するためのサンプル コード"
   services="search"
   documentationCenter=""
   authors="liamca"
   manager="pablocas"
   editor=""
   tags="machine learning"/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="11/12/2015"
   ms.author="liamca"/>


# Azure Search の "よく一緒に購入されている商品" によるレコメンデーション

オンラインで買い物をしたときに、"よく一緒に購入されている商品" リストや "この商品を購入したお客様はこのような商品も購入しています" リスト、または "おすすめの新商品" リストに気付いたことがあるでしょう。これらのリストには、購入を検討するようにその他の商品またはサービスが提示されています。 通常、このデータは、多くの顧客の購入履歴に基づいてオンライン小売業者が集めたものか、単に消費者の好みに関する独自のデータになります。 検索で関連する消費者向けのレコメンデーションを提示する機能は、商用アプリケーションだけでなく、マルチメディア コンテンツや情報を提供するサイトにとっても効果的です。

Azure Search アプリケーションでは、追加のコードを記述することでレコメンデーションを実装できます。このコードにより、データ内のパターンや関係を特定して、ユーザーに関連するレコメンデーションに変換するために使用されるデータとサービスが取り入れられます。

[このサンプルでどのように](https://github.com/liamca/azure-search-recommendations), を使用して、 [Apache Mahout]() の推奨事項エンジン。

この記事の残りの部分では、映画のレンタルに関するサンプル データを使用して "よく一緒にレンタルされている商品" のレコメンデーションを追加するサンプル コードについて説明します。

![1](./media/search-fbt-recommendations/product_recommendations.png)

## レコメンデーションとは

*レコメンデーション*とは、既存の検索ユーザーの行動 (Web ログなど) に基づいてカタログからより多くの商品を表示させる手法で、商品の推奨やコンバージョンの向上に使用されます。

リコメンデーション エンジンは多くの場合、またはデジタルのストアから直接データを収集して過去の顧客のアクティビティを使用してトレーニングします。 このサンプルでは、推奨事項のデータを作成するのに Apache Mahout を使用します。

一般的な推奨事項は次のとおりです。
- 頻繁に Bought 流れ: これを購入した顧客も購入します。
- 項目の推奨事項をお客様: も購入するように、顧客

## Azure Search インデックスの作成

- AzureSearchMovieRecommendations.sln ソリューションを開き、ImportAzureSearchIndexData を既定のプロジェクトとして設定します。
- ImportAzureSearchIndexData 内の Program.cs ファイルを開き、お使いの Azure Search サービスを指すように SearchServiceName と SearchApiKey を変更します。
- Http://grouplens.org/datasets/hetrec-2011/から hetrec2011 movielens-2k.zip をダウンロードし、\ImportAzureSearchIndexData\data にファイル Movies.dat と user_ratedmovies.dat をコピー
- プロジェクトを実行し、インデックスを作成して映画データを読み込みます。
- 最後に、アプリケーションでテスト検索を実行します。

## 映画を検索するための簡単な HTML アプリケーションの作成

使用すると、Azure Search インデックスのクエリは、完成した JavaScript web アプリケーションにあります。 
\WebSite\starter-template-complete

最初からデモを処理する場合は、元の CSS を参照してください。
\WebSite\starter-template

\WebSite\starter-template-complete 内の search.js ファイルを開き、Azure Search サービスの詳細で apiKey と azureSearchService を更新します。

Chrome などのブラウザーでこのファイルを開き、検索ボックスで入力すると映画が表示されます。

## Mahout を使用したレコメンデーションの作成を実行するためのコマンド

- data\movie_usage.txt ファイルを Azure Blob Storage にアップロードします。
- (リモート デスクトップを有効にして) HDInsight インスタンスを作成し、(Azure クラシック ポータルから利用できる) リモート デスクトップからコンピューターに接続します。
- HDInsight コンピューターから、"Hadoop コマンド ライン" を開きます。
- C:\apps\dist の Mahout bin ディレクトリに移動します。 地雷次のようが、Mahout の最新のバージョンを入手できます。
    C:\apps\dist\mahout-1.0.0.2.3.3.0-2992\bin
- [CONTAINER] と [STORAGEACT] を Azure Storage の詳細 (movie_usage.txt ファイルを配置した場所) に置き換えて、次のコマンド ラインを実行します。

    mahout itemsimilarity -s SIMILARITY_COSINE --input "wasb://[CONTAINER]@[STORAGEACT].blob.core.windows.net/movie_usage.txt" --output "wasb://[CONTAINER]@[STORAGEACT].blob.core.windows.net/output/" --tempDir "wasb://[CONTAINER]@[STORAGEACT].blob.core.windows.net/temp" -m 5

完了するには、さまざまな分かかりますが、このとき、ストレージ コンテナーが映画のリコメンデーションを含めることが次のファイルを含める必要があります。
/movies/output/part-r-00000

このファイルには、[Item ID of Movie]、[Item ID of Recommendations related to this Movie]、[Similarity Percentage] という 3 つの列があります。

## Mahout から Azure Search へのデータのインポート

Azure Search インデックスを作成したプログラムがという名前のフィールドを作成 `の推奨事項` のコレクション (コンマで区切られた一連の文字列に似ています)。 ここでは、前の手順で作成されたデータをこの Azure Search インデックスとマージします。

- Visual Studio の AzureSearchMovieRecommendations.sln ソリューションから、MahoutOutputLoader 内の Program.cs ファイルを開きます。
- お使いの Azure Search サービスの詳細で SearchServiceName と SearchApiKey を更新します。
- Mahout の製品レコメンデーション ファイルを格納する Azure ストレージ アカウントの詳細で StorageApiKey と StorageAccountName を更新します。
- アプリケーションを実行してデータをマージします。

## レコメンデーションの視覚化

この時点で、Web アプリケーションに戻り、いずれかの映画をクリックしてレコメンデーションを表示できます。

このイメージをクリックしたときにレコメンデーションが返されるしくみを確認したい場合は、Search.js ファイルを開き、openMovieDetails() 関数を確認してください。

## クレジット

GroupLens (http://grouplens.org/datasets/hetrec-2011/) によってデータが提供されました

このデータのライセンスの詳細については、このページを参照してください: http://files.grouplens.org/datasets/hetrec2011/hetrec2011-movielens-readme.txt






