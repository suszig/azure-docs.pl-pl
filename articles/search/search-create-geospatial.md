<properties 
    pageTitle="Azure Search を使用した地理空間検索アプリの作成 |Microsoft Azure |ホスト型クラウド検索サービス" 
    description="Bing と Azure Search では、Microsoft Azure でホスト型クラウド検索サービスを使用して地理空間検索アプリを作成します。" 
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

# Azure Search を使用した地理空間検索アプリの作成

このチュートリアルでは、Azure Search および Bing Maps を使用して地理空間検索を Web アプリケーションに追加する方法を説明します。 地理空間検索を使用すると、ある地点から特定の距離内にある検索対象を見つけることができます (現在位置から 5 KM 以内にあるすべてのレストランを検索するなど)。 Azure Search の地理空間機能は、一般的に使用されているマッピング技術をサポートしています。 たとえば、不動産業のアプリで多角形の範囲内にある売家を多角形を使用して表す必要がある場合は、OData または単純な検索構文を使用して簡単に実現できます。

詳細については、に関する Channel 9 のビデオを見る [Azure Search と地理空間データ](http://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data)します。

![][7]

アプリケーションを作成するには、Bing のマッピング サービスを活用して、CSV ファイルからロードした住所をジオコーディングし、生成されたデータを Search インデックスに保管します。 

このチュートリアルは、 [Azure Search – Adventure Works Demo](http://azuresearchadventureworksdemo.codeplex.com)します。 このデモをまだ実行していない場合は、まず、このデモでインデックスを作成したり Web アプリから Azure Search API を呼び出したりしてみてください。  

<a id="sub-1"></a>
## 前提条件

+   Visual Studio 2012 以上および ASP.NET MVC 4 と SQL Server がインストールされていること。 ソフトウェアがインストールされていない場合は、無料の Express エディションをダウンロードすることができます: [Visual Studio 2013 Express](http://www.visualstudio.com/products/visual-studio-express-vs.aspx) と [Microsoft SQL Server 2014 Express](http://msdn.microsoft.com/evalcenter/dn434042.aspx)します。
+   Azure Search サービス。 Search サービス名および管理キーが必要です。 参照してください [ポータルで Azure Search サービスの作成](search-create-service-portal.md) 詳細です。
+   Bing マップ サービスおよびこのサービスを利用するためのキー。 次のセクションで手順について説明します。
+   [CodePlex の azure Search GeoSearch Sample](https://azuresearchgeospatial.codeplex.com/)します。 [ソース] タブをクリックして **ダウンロード** ソリューションの zip ファイルを取得します。 

    ![][12]

このソリューションには次の 2 つのプロジェクトが含まれています。

+   **StoreIndexer** Azure Search インデックスを作成してデータをロードします。
+   **AdventureWorksWebGeo** Azure Search インデックスを照会する MVC4 ベースのアプリケーションは、Bing map 上、店舗の場所を表示します。

[AZURE です。含める [このチュートリアルを完了する Azure のアカウントを作成する必要があります:](../../includes/free-trial-note.md)]

<a id="sub-2"></a>
## Bing Maps

ここでは、次の 2 つのことに Bing Maps API を使用します。

+ **住所のジオコーディング:** 住所 (市、状態、zip) がある、データであって、期待しても、住所の経度および緯度の座標地理空間検索を行うことができるようにします。 この座標を取得するために、Bing Maps DataFlow API を使用して、住所を一括送信してジオコーディングします。 ここでは Bing の無料体験アカウントを使用するため 1 回につき 50 個の住所に制限されますが、このチュートリアルではこれで十分です。

+ **Bing Maps:** 、アプリの実行時に Bing Maps を使用して、Bing マップ上に店舗の場所を表示します。

### Bing Maps のアカウントの作成

1. 移動して、 [Bing Maps ポータル](https://www.bingmapsportal.com/) 新しいアカウントを作成します。 詳細な情報を入力してアカウントを作成します。

2. アカウントを作成すると、選択 **キーの作成または表示** し、キーを作成する詳細を入力します。  このデモを選択できます **Trial Key**します。

3. クリックして **送信**します。 Bing マップ アプリケーション用、キーの詳細情報が表示されます。 このキーを後で使用するのでメモしておいてください。

<a id="sub-3"></a>
## Bing Maps DataFlow API を使用して C# で住所をジオコーディングする

このステップでは、世界各地にあるさまざまな自転車店舗の住所を Bing Maps DataFlow API を使用してジオコーディングします。 

このデータは、先ほどダウンロードしたソースの store_locations.csv という CSV ファイルにあります。 このファイルをテキスト エディターまたは Excel で開くと、各店舗の ID 列、店名、および住所があることを確認できます。

コードを見ながら、どのように動作するのか説明していきます。

1. Visual Studio で AdventureWorksGeo ソリューションを開き、プロジェクトを展開 **StoreIndexer** ソリューション エクスプ ローラーで Program.cs を開きます。 インデックスの作成で既にカバーされているので、 [Azure Search – Adventure Works Demo](http://azuresearchadventureworksdemo.codeplex.com/), 、Program.cs で動作する方法についての説明は省略します。

2. 移動して、 **Main** 機能、呼び出していることに注意してください。 **ApplyStoreData**します。  この関数に移動してコードを見てみましょう。  

3. **ApplyStoreData** System.Data.DataTable に"store_locations.csv"と呼ばれる CSV ファイルからデータを読み込みます。  

    このファイルには、Azure Search にロードする住所を含め、全店舗の情報が入っています。  セットを作成してこのファイル内の各行を反復処理して **indexOperations** Azure Search インデックスに挿入する (で以前に作成、 **CreateStoresIndex()** 関数)。  

    慎重にした後で確認する、ことがわかりますが、 **GeoPt** 各店舗の緯度と経度を格納するフィールドが空です。 次のステップに進みましょう、 **Main** 関数です。

5. 関数に移動 **ExtractAddressInfoToXML()**します。 この関数は、store_locations.csv ファイルから住所情報を抽出し、Bing Maps がジオコーディングできる形式で XML ファイルにロードします。 ファイルが作成されると、送信されて処理が Bing Maps DataFlow を関数を呼び出して **GeoCoding.CreateJob**します。

6. ジオコーディングの処理に時間がかかるために呼び出すループがない **GeoCoding.CheckStatus** ジョブが完全かどうかをぞれぞれ 10 秒です。 呼び出して結果をダウンロードが完了すると、 **GeoCoding.DownloadResults** アドレス クラスにします。

7. 最後のステップでは、ジオコーディングされた住所を取り出し、Azure Search に送信します。 開いてこれを行う方法について詳しく見ていきましょう、 **UpdateStoreData** 関数です。

  **UpdateStoreData** アクションを使用して **@search.action: マージ** ジオコーディングされた経度と緯度を使用して Edm.GeographyPoint 型のロケーション フィールドを更新するには、Bing Maps から先ほどダウンロードしたことを調整します。 これは、"stores" インデックスのドキュメントの一意キーである storeId を検索し、その新しいデータを既存のドキュメントにマージすることで更新しています。

8. アプリケーションを実行する前に、Azure Search および Bing Maps API の情報を追加します。このためには、App.config を開き、"SearchServiceName"、"SearchServiceApiKey"、"BingMapsAPI" の値を Azure Search サービスおよび Bing Maps API の該当する各値に更新します。 Search サービス名については、"mysearch.search.windows.net" というサービスであれば、"mysearch" と入力します。

9. 右クリックし、 **StoreIndexer** プロジェクト **デバッグ** | **新しいインスタンスを開始** を実行します。

<a id="sub-4"></a>
## Azure Search および Bing Maps を使用して MVC4 アプリケーションにマッピングを追加する

このステップでは、店舗の検索をロードして Bing Map 上にプロットする検索アプリケーションをビルドして Web ブラウザーで実行します。

1.  Visual Studio で、AdventureWorksGeo.sln という名前の Azure Search デモ ソリューションを開きます。 
    
2.  右クリック **AdventureWorksWebGeo** し、ソリューション エクスプ ローラーで **スタートアップ プロジェクトとして設定**します。
    
3.  このプロジェクトの Web.config を開き、"SearchServiceName"、"SearchServiceApiKey"、"BingMapsAPI" の値を、使用する Azure Search サービスおよび Bing Maps API の該当する値に更新します。 Search サービス名については、"mysearch.search.windows.net" というサービスであれば、"mysearch" と入力します。

4.  Web.config を保存します。
    
5.  キーを押して **f5 キーを押して** 、プロジェクトを起動します。 手順に従います。 [トラブルシューティング](#err-mvc) ビルド エラーが出た場合は、ステップです。

各店舗がマップ上に点として表示されます。 いずれかの店舗をクリックすると、その店舗の詳細をまとめたポップアップが表示されます。 この情報はすべて、前のステップで作成した "stores" という Azure Search インデックスから得られた情報です。 

<a id="sub-5"></a>
## AdventureWorksWebGeo を探索する

プロジェクト **AdventureWorksWebGeo** は、ASP.NET MVC 4 を使用して、Azure Search geosearch を活用するマッピング アプリケーションをビルドすると対話する方法です。 このセクションでは、このアプリケーションのコードの個々の部分を見ながら何が行われているのかを説明します。

1.  ソリューション エクスプ ローラーで、 **AdventureWorksWebGeo** | **コント ローラー** して HomeController.cs を開きます。  **Index()** アプリケーションが起動し、インデックス ページが読み込まれるときに呼び出されます。 この関数では、Bing Maps API が Web.config からロードされ、ViewBag.BingAPI として Index ビューに渡されます。

2.  Index.cshtml を開いて **ビュー** | **ホーム**します。

3.  このファイルは、Bing Maps を Web アプリケーションに追加する一般的な方法に従っていますが、次のように、注目すべき箇所がいくつかあります。

+   コントローラーから渡された ViewBag を使用し、資格情報 '@ViewBag.BingAPI' を使用して、マップの資格情報を読み込みます。 

+   マップが読み込まれると、JQuery $.post が homecontroller **検索** 参照することで、関数:/検索

+    **検索** 関数は、受信され、プッシュピンとして Bing Map に追加されて、店舗の場所を取得します。 

4.  下の HomeController.cs を開いて **コント ローラー** を見て、 **検索** 関数です。 _storeSearch.Search(lat, lon, 10000) への呼び出しがどのように行われるのかに注目してください。 これにより、クエリが実行されて、指定の緯度 (lat) および経度 (lon) から 10,000 KM 内にあるすべての店舗が検索されます。 このクエリの結果が処理され、Index ビューに戻されると、Bing Map 上に表示されるプッシュピンとして処理されます。

これでデモは終了です。 Azure Search を使用してマップ ベースの ASP.NET MVC4 アプリケーションを構築する前に理解しておく必要がある主な操作をすべて説明しました。


<a id="err-mvc"></a>
## 「ファイルまたはアセンブリ 'System.Web.Mvc' が読み込めませんでした」の解決方法

AdventureWorksWeb をビルドするときに、「ファイルまたはアセンブリ 'System.Web.Mvc, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'、またはその依存関係の 1 つが読み込めませんでした」というエラーが出た場合は、次の手順を試してエラーを解決してください。

1. パッケージ マネージャー コンソールを開く: **ツール** | **NuGet パッケージ マネージャー** | **パッケージ マネージャー コンソール**
2. PM> プロンプトに、「Update-package -reinstall Microsoft.AspNet.Mvc」と入力します。
3. ファイルを再ロードするように求められたら選択 **すべてはい**します。
4. ソリューションをリビルドして実行してください **f5 キーを押して** 再度します。

<a id="next-steps"></a>
## 次のステップ

追加で行う自習として、このマッピング アプリケーションにさらに機能を追加することを検討してください。 たとえば、次のようなものを追加できます。

+ 特定の店舗を検索できるようにする検索ボックス。

+ 国や郡でフィルタリングできるようにするファセット。  

+ ユーザーが描画する選択領域。これにより、マップ上に領域を描画して検索領域を指定できるようにします。 そして、地理的交差 API を使用して、この領域を Azure Search でフィルタリングし、マップ上にプロットする。


<!--Anchors-->
[Prerequisites]: #sub-1
[Bing Maps]: #sub-2
[Geocode Addresses in C# using Bing Maps DataFlow API]: #sub-3
[Add Mapping to an MVC4 Application using Azure Search and Bing Maps]: #sub-4
[Explore AdventureWorksWebGeo]: #sub-5
[Next steps]: #next-steps


<!--Image references-->
[7]: ./media/search-create-geospatial/AzureSearch-geo1-App.PNG
[12]: ./media/search-create-geospatial/AzureSearch_Create2_CodeplexDownload.PNG 
