<properties 
    pageTitle="Azure Search を使用して最初の検索ソリューションを作成する方法 | Microsoft Azure | ホスト型クラウド検索サービス" 
    description="Microsoft Azure のホスト型のクラウド検索サービスである Azure Search を利用し、最初の検索ソリューションを作成します。" 
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

# Azure Search による最初の検索ソリューションの作成

ここでは、Adventure Works という自転車製造会社向けの検索アプリケーションをサンプルとして説明します。 このチュートリアルを完了すると、ファセット ナビゲーション、検索結果に対する数種類のソート オプション、入力に先行したクエリ候補の表示など、さまざまな検索機能を備えたオンラインの製品カタログが作成されます。

   ![][7]

このデモでは、以下の演習を通じて Azure Search の使用法を説明します。

+   Azure Search インデックスを作成する。
+   SQL Server データベースから Azure Search インデックスにドキュメント (データ) を読み込む。
+   Azure Search を利用して以下の操作を行う ASP.NET MVC4 アプリケーションを構築する。
    +   検索し、結果を Azure Search インデックスに表示する
    +   検索語の入力中、入力に先立って [検索] ボックスに候補を提示する
    +   ファセットを使用した検索結果のフィルタリング


<a id="sub-1"></a>
## 前提条件

+    [Azure サブスクリプション](../includes/free-trial-note.md)します。 試用版サブスクリプションにサインアップする準備完了でない場合は、このチュートリアルをスキップし、無効にする [Try Azure App Service](https://tryappservice.azure.com/) 代わりにします。 この代替オプションの場合、Azure Search で ASP.NET Web アプリを無料で使用でき (セッションごとに 1 時間)、サブスクリプションは不要です。
+   Visual Studio 2012 以上および ASP.NET MVC 4 と SQL Server がインストールされていること。 ソフトウェアがインストールされていない場合は、無料の Express エディションをダウンロードすることができます: [Visual Studio 2013 Express](http://www.visualstudio.com/products/visual-studio-express-vs.aspx) と [Microsoft SQL Server 2014 Express](http://msdn.microsoft.com/evalcenter/dn434042.aspx)します。
+   Azure Search サービス。 Search サービス名および管理キーが必要です。 参照してください [ポータルで Azure Search サービスの作成](search-create-service-portal.md) 詳細です。
+   [Codeplex の adventure Works Azure Search デモ プロジェクト](http://go.microsoft.com/fwlink/p/?LinkID=510972)します。 [ソース] タブをクリックして **ダウンロード** ソリューションの zip ファイルを取得します。 

    ![][12]


このソリューションには次の 2 つのプロジェクトが含まれています。

+   **CatalogIndex** Azure Search インデックスを作成し、プロジェクトに含まれている SQL Server データベースからデータを読み込みます。
+   **AdventureWorksWeb** は Azure Search インデックスを照会する MVC4 ベースのアプリケーションです。 このチュートリアルは、ASP.NET MVC の実務知識を持つ読者を想定しています。

<a id="sub-2"></a>
## Azure Search インデックスの作成

この手順では使用して **CatalogIndex** "catalog"という名前のインデックスが、AdventureWorks SQL Server データベースのデータに基づく新しい Azure Search を作成します。

1.  Visual Studio で、という名前の Azure Search デモ ソリューションを開く **AdventureWorksCatalog.sln**します。  
2.  右クリック **CatalogIndexer** ソリューション エクスプ ローラーし、[ **スタートアップ プロジェクトとして設定** このアプリケーションを実行できるようになく **AdventureWorksWeb** キーを押したときに、プロジェクト **f5 キーを押して**します。
3.  開いている **App.config** このプロジェクトし、"SearchServiceName"および"SearchServiceApiKey"、Azure Search サービスの値に更新します。 Search サービス名については、"mysearch.search.windows.net" というサービスであれば、"mysearch" と入力します。
4.  App.config には、オプションで "SourceSqlConnectionString" のエントリを含めることができます。その場合、SQL Server 2014 Express LocalDB (Server=(LocalDB)\v11.0) の使用が前提となります。 エディションが異なる SQL Server を使用している場合は、それに応じてサーバー名を更新してください。 たとえば、ローカルの既定のインスタンスがある場合、(local) または localhost を使用できます。
5.  保存 **App.config**します。
6.  キーを押して **f5 キーを押して** 、プロジェクトを起動します。

コマンド プロンプトが開き、"インデックスを作成しています..." というメッセージが表示されます。

しばらくすると、次のメッセージが表示されて完了します。"完了しました。  押すキー <enter> 続行する"。

   ![][8]

キーを押して **Enter** アプリケーションを終了します。 これで Azure Search インデックスが正常に作成できました。 

> [AZURE.NOTE] 「キー 'attachdbfilename' の無効な値」が含まれる、またはその他のデータベース接続エラーが発生した場合に、UAC 競合が発生する実行可能性があります。 このデモの目的は、次の手順を実行してこれらのエラーを回避することにあります。 
> 認証されたユーザーにアクセスを提供する新規または既存フォルダー (Temp など) にソリューションをコピーします。 
> 使用 **管理者として実行** Visual Studio を開始します。 
> ソリューションを開き、ビルドして、およびキーを押します **f5 キーを押して** インデックスを作成します。

インデックスの作成とドキュメントのアップロードを検証するには、Search サービス ダッシュ ボード、 [Azure Classic Portal](https://portal.azure.com)します。 [使用状況] のインデックスは 1 つずつ増えます。データベース内の 1 製品につき 1 個のドキュメントがあるため、合計 294 個のドキュメントがあります。

をクリックして、 **インデックス** タイル インデックスの一覧を表示します。 インデックス リストがスライドして現れ、新しいインデックスとドキュメント数が表示されます。 無料の価格レベルでは、使用できるインデックスが最大で 3 つであることに注意してください。 既に 3 つのインデックスが存在する場合は、新しいインデックスのための領域を解放するために、1 つのインデックスを削除する必要があります。

   ![][9]


<a id="sub-3"></a>
## CatalogIndexer の探索

詳しく見ていきましょう、 **CatalogIndexer** そのしくみを理解するプロジェクト。

1.  開いている **Program.cs** へ移動、ソリューション エクスプ ローラーから、 `Main(string[] args)` 関数です。

    この関数は、使用する特定の Azure Search サービスに基づいて `_serviceURI` と呼ばれる Uri を構築し、API キーを利用してこの Search サービスを認証するために `_httpClient` という名前の HttpClient を作成していることがわかります。

2.  `ChangeEnumeratorSql` と `ChangeSet` は、SQL Server の AdventureWorks データベースにある Products テーブルのデータを列挙するために使用されます。 

3.  このテーブルから収集されたデータに基づいて `ApplyChanges` が呼び出され、このデータが Azure Search インデックスに適用されます。

4.  同じファイルの `ApplyChanges` に移動します。 この関数によって、既存のインデックスが削除された後 (`DeleteCatalogIndex`)、"catalog" という名前の新しいインデックスが作成される (`CreateCatalogIndex`) ことに注目してください。  

5.  `CreateCatalogIndex` 関数に進みます。ここでは、SQL Server の Products テーブルの列に一致するスキーマを使用してインデックスが作成されることがわかります。 各フィールドには、これらのフィールドの用途を定義する属性と、型 (`Edm.String` または `Edm.Double`) があります。 参照してください、 [Azure Search REST API のドキュメント](http://msdn.microsoft.com/library/azure/dn798935.aspx) これらの属性の詳細についてです。

6.  `ApplyChanges` 関数に戻ります。 この関数は、`ChangeSet` で列挙された変更のすべてのデータをループ処理します。 変更を 1 つずつ適用するのではなく、1000 個単位のグループにバッチ化してから Search サービスに適用します。 この方法は、ドキュメントを 1 つずつ適用するよりはるかに効率的です。

ここまでは、SQL Server のリレーショナル データを使用してインデックスを作成および取り込む方法を見てきましたが、次は、検索データを使用した製品カタログの構築方法について説明しましょう。

<a id="sub-4"></a>
## Azure Search を使用した MVC4 アプリケーションの構築

この手順では、Web ブラウザーで検索アプリケーションを構築し、実行します。

1.  Visual Studio で、という名前の Azure Search デモ ソリューションを開く **AdventureWorksCatalog.sln**します。  

2.  右クリック **AdventureWorksWeb** し、ソリューション エクスプ ローラーで **スタートアップ プロジェクトとして設定**します。

3.  開いている **Web.config** このプロジェクトし、"SearchServiceName"および"SearchServiceApiKey"、Azure Search サービスの値に更新します。 Search サービス名については、"mysearch.search.windows.net" というサービスであれば、"mysearch" と入力します。

4.  保存 **Web.config**します。

5.  キーを押して **f5 キーを押して** 、プロジェクトを起動します。 手順に従います。 [トラブルシューティング](#err-mvc) ビルド エラーが出た場合は、ステップです。 

    ![][10]

6.  検索ボックスを空のままにし、クリックして **検索** 294 個すべての製品にします。

7.  左側にファセットのリストがあります。 いずれかのファセットをクリックしてみてください。 検索が再び実行されますが、今回は、選択したファセットが追加されて結果がフィルタリングされます。 最初の行にブレークポイントを追加することも、 **HomeController.cs** `Search` 各行 (f11) する関数。

7.  "mountain bikes" などの検索語句を入力します。 入力すると、クエリの候補がドロップダウン リストで表示されます。

    ![][11]

次に、これまでの説明の復習として、このデモで表示されるスクリーンショットを最初から順に示します。 これまでのセクションでは、ファセット ナビゲーション (左側)、ページ上部に表示されるドキュメント数、提案について説明しました。さらに、関連性、リスト、または価格でソートするソートのオプションも取り上げました。

   ![][7]


<a id="sub-5"></a>
## AdventureWorksWeb の探索

プロジェクト AdventureWorksWeb の内容を調べることで、ASP.NET MVC 4 を使用して Web アプリケーションから Azure Search を利用する方法がわかります。 このセクションでは、このアプリケーションのコードの個々の部分を見ながら何が行われているのかを説明します。

1.  ソリューション エクスプ ローラーで、 **AdventureWorksWeb** | **コント ローラー** 開き **HomeController.cs**

    これは、インデックス ビューからの操作を管理する MVC コントローラーです。  このコントローラーの上部に `CatalogSearch _catalogSearch` への参照がありますが、これによって、使用する Azure Search サービスへの HttpClient 接続オブジェクトが作成されます。 そのためコード `CatalogSearch` にある **CatalogSeach.cs**

2. 内で **HomeController.cs**, 、2 つのメイン関数があります。

    **検索**: この関数を呼び出して結果を取得し、表示される [Index] ビューに戻さをユーザーが検索ボタンをクリックするかファセットを選択、します。

    **提案**: ように、ユーザーは、検索ボックスに入力は、この関数は、Azure Search インデックスの内容に基づいて候補の一覧を返します。

これら 2 つの関数について、もう少し詳しく見ていきましょう。  

3.   **HomeController.cs** `Search` 関数への呼び出しがわかります `_catalogSearch.Search`, 、Azure Search サービスへの接続オブジェクトが含まれています。 ある検索関数を呼び出すと **CatalogSearch.cs**, 、Azure Search クエリをビルドするこれらのパラメーターを利用することを確認できます。 クエリの結果は `result` という名前の JSON オブジェクトに格納され、`Index` ビューに戻されます。ここで結果が解析され、Web ページに表示されます。

4.  開いている **Index.cshtml** [表示] |自宅、これらの結果を解析するために使用するコードが表示されます。

5.  まだ実行中、およびオープンである場合は、アプリケーションを停止 **Index.cshtml** ビューの下にあるファイル |ホームです。  このファイルの最後に、`JQuery $(function ())` を使用する JavaScript 関数があることがわかります。 この関数は、ページが読み込まれたときに呼び出されます。 JQuery オートコンプリート関数を使用し、この関数を "q" という ID で、検索テキスト ボックスからのコールバックとしてリンクします。 テキスト ボックスに入力されるたびに、この自動補完の関数が呼び出され、今度は、入力された内容によって /home/suggest が呼び出されます。  `/home/suggest` 関数への参照 **HomeController.cs** と呼ばれる `Suggest`です。

6.  開いている **HomeController.cs** 、Suggest 関数に移動します。 このコードは、Search 関数を使用するとよく似て、 `_catalogSearch` オブジェクト関数を呼び出すには **CatalogSearch.cs** と呼ばれる `Suggest`です。 検索クエリを作成するのではなく、 `Suggest` 関数への呼び出しは、 [Suggestions API](http://msdn.microsoft.com/library/azure/dn798936.aspx)します。 この API は、テキスト ボックスに入力された語句を使用して、想定される候補のリストを作成します。 値が返される、 **Index.cshtml** 先行入力のオプションと、検索ボックスに自動的に示されているファイル。

ここで、候補を作成するフィールドを Azure Search がどのように認識するのか、考えてみましょう。 その答えは、インデックスを作成した時点にあります。  `CreateCatalogIndex`  Program.cs ファイル内の関数の **CatalogIndexer** という名前の属性があるプロジェクトで、 `Suggestions`です。  この属性を `True` に設定すると、Azure Search が候補を取得するフィールドとして常にこれを使用できるようになります。

以下のことを試してみましょう。  

7.  もう一度アプリケーションをビルドして、キーを押します **f5 キーを押して** アプリケーションを実行します。

8.  検索ボックスに "Road" という言葉を入力します。  すると間もなくテキスト ボックスの下部に、ユーザーが選択すると思われる項目の候補が一覧表示されます。  

ブレークポイントを追加することができます、 `Suggest` 内で機能 **HomeController.cs** および候補リストを構築するために行われた各呼び出しのステップ (F11) 実行します。

これでデモは終了です。 Azure Search を使用して ASP.NET MVC4 アプリケーションを構築する前に理解しておく必要がある主な操作をすべて説明しました。


<a id="err-mvc"></a>
## 「ファイルまたはアセンブリ 'System.Web.Mvc' が読み込めませんでした」の解決方法

AdventureWorksWeb をビルドするときに、「ファイルまたはアセンブリ 'System.Web.Mvc, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'、またはその依存関係の 1 つが読み込めませんでした」というエラーが出た場合は、次の手順を試してエラーを解決してください。

1. パッケージ マネージャー コンソールを開く: **ツール** | **NuGet パッケージ マネージャー** | **パッケージ マネージャー コンソール**
2. PM> プロンプトに、「Update-package -reinstall Microsoft.AspNet.Mvc」と入力します。
3. ファイルを再ロードするように求められたら選択 **すべてはい**します。
4. ソリューションをリビルドして実行してください **f5 キーを押して** 再度します。


<a id="next-steps"></a>
## 次のステップ

追加で行う自習として、Search 結果のいずれかをユーザーがクリックしたときに開く [詳細] ページを追加することを検討してください。 準備として以下のことができます。

+   については、 [参照 API](http://msdn.microsoft.com/library/azure/dn798929.aspx) (たとえば、productID を渡すことができます) 特定のドキュメントを返すための Azure Search のクエリを作成することができます。
+   新しい関数を追加、 **HomeController.cs** ファイルに Details といいます。 対応する追加 **Details.cshtml** この検索の結果を受信し、結果を表示するビュー。
+   この追加のコード サンプルと地理空間検索のビデオ: [Channel 9 - Azure Search と地理空間データ](http://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data) と [CodePlex: Azure Search ジオサーチのサンプル](http://azuresearchgeospatial.codeplex.com)

確認することも、 [Azure Search REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) MSDN にします。


<!--Anchors-->
[Prerequisites]: #sub-1
[Create an Azure Search index]: #sub-2
[Explore CatalogIndexer]: #sub-3
[Build an MVC4 Application using Azure Search]: #sub-4
[Explore AdventureWorksWeb]: #sub-5
[Next steps]: #next-steps


<!--Image references-->
[7]: ./media/search-create-first-solution/AzureSearch_Create1_WebApp.PNG
[8]: ./media/search-create-first-solution/AzureSearch_Create1_ConsoleMsg.PNG
[9]: ./media/search-create-first-solution/AzureSearch_Create1_DashboardIndexes.PNG
[10]: ./media/search-create-first-solution/AzureSearch_Create1_WebAppEmpty.PNG
[11]: ./media/search-create-first-solution/AzureSearch_Create1_Suggestions.PNG
[12]: ./media/search-create-first-solution/AzureSearch_Create1_CodeplexDownload.PNG

