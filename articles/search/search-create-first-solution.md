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

+   [Azure サブスクリプション](../includes/free-trial-note.md)します。 試用版サブスクリプションにサインアップする準備完了でない場合は、このチュートリアルをスキップし、無効にする [Try Azure App Service](https://tryappservice.azure.com/) 代わりにします。 この代替オプションの場合、Azure Search で ASP.NET Web アプリを無料で使用でき (セッションごとに 1 時間)、サブスクリプションは不要です。
+   Visual Studio 2012 以上および ASP.NET MVC 4 と SQL Server がインストールされていること。 ソフトウェアがインストールされていない場合は、無料の Express エディションをダウンロードすることができます: [Visual Studio 2013 Express](http://www.visualstudio.com/products/visual-studio-express-vs.aspx) と [Microsoft SQL Server 2014 Express](http://msdn.microsoft.com/evalcenter/dn434042.aspx)します。
+   Azure Search サービス。 Search サービス名および管理キーが必要です。 参照してください [ポータルで Azure Search サービスの作成](search-create-service-portal.md) 詳細です。
+   [Codeplex の adventure Works Azure Search デモ プロジェクト](http://go.microsoft.com/fwlink/p/?LinkID=510972)します。 [SOURCE CODE] タブで、**[Download]** をクリックしてソリューションの zip ファイルを取得してください。

    ![][12]


このソリューションには次の 2 つのプロジェクトが含まれています。

+   **CatalogIndex** で Azure Search インデックスが作成され、このプロジェクトで使用する SQL Server データベースからデータが読み込まれます。
+   **AdventureWorksWeb** は、Azure Search インデックスを照会する MVC4 ベースのアプリケーションです。 このチュートリアルは、ASP.NET MVC の実務知識を持つ読者を想定しています。

<a id="sub-2"></a>
## Azure Search インデックスの作成

この手順では、**CatalogIndex** を使用して、AdventureWorks SQL Server データベースのデータを基に "カタログ" と呼ばれる新しい Azure Search インデックスを作成します。

1.  Visual Studio で、**AdventureWorksCatalog.sln** という名前の Azure Search デモ ソリューションを開きます。
2.  ソリューション エクスプローラーで **[CatalogIndexer]** を右クリックし、**[スタートアップ プロジェクトに設定]** を選択します。こうすると、**F5** キーを押した場合に **AdventureWorksWeb** プロジェクトではなく、このアプリケーションが実行されます。
3.  このプロジェクトの **App.config** を開き、"SearchServiceName" および "SearchServiceApiKey" の値を、使用する Azure Search サービスの該当する値に更新します。 Search サービス名については、"mysearch.search.windows.net" というサービスであれば、"mysearch" と入力します。
4.  App.config には、オプションで "SourceSqlConnectionString" のエントリを含めることができます。その場合、SQL Server 2014 Express LocalDB (Server=(LocalDB)\v11.0) の使用が前提となります。 エディションが異なる SQL Server を使用している場合は、それに応じてサーバー名を更新してください。 たとえば、ローカルの既定のインスタンスがある場合、(local) または localhost を使用できます。
5.  **App.config** を保存します。
6.  **F5** キーを押して、プロジェクトを起動します。

コマンド プロンプトが開き、"インデックスを作成しています..." というメッセージが表示されます。

しばらくすると、次のメッセージが表示されて完了します。"完了しました。キーを押して <enter> を続行します"。

   ![][8]

**Enter** キーを押してアプリケーションを閉じます。 これで Azure Search インデックスが正常に作成できました。
> [AZURE.NOTE] "invalid value for key 'attachdbfilename' (キー 'attachdbfilename' の値が無効です)" というメッセージが含まれるエラー、または他のデータベース接続エラーが表示された場合は、UAC 競合が発生している可能性があります。 このデモの目的は、次の手順を実行してこれらのエラーを回避することにあります。 
> 認証されたユーザーにアクセスを提供する新規または既存フォルダー (Temp など) にソリューションをコピーします。 
> **[管理者として実行]** を使用して、Visual Studio を開始します。 
> ソリューションを開き、ビルドして、**F5** を押してインデックスを作成します。

インデックスの作成とドキュメントのアップロードを検証するには、Search サービス ダッシュ ボード、 [Azure Classic Portal](https://portal.azure.com)します。 [使用状況] のインデックスは 1 つずつ増えます。データベース内の 1 製品につき 1 個のドキュメントがあるため、合計 294 個のドキュメントがあります。

**[インデックス]** タイルをクリックしてインデックス リストを表示します。 インデックス リストがスライドして現れ、新しいインデックスとドキュメント数が表示されます。 無料の価格レベルでは、使用できるインデックスが最大で 3 つであることに注意してください。 既に 3 つのインデックスが存在する場合は、新しいインデックスのための領域を解放するために、1 つのインデックスを削除する必要があります。

   ![][9]


<a id="sub-3"></a>
## CatalogIndexer の探索

**CatalogIndexer** プロジェクトの機能を理解するために、もう少し詳しく見ていきましょう。

1.  開いている **Program.cs** へ移動、ソリューション エクスプ ローラーから、 `Main (args)` 関数です。

    この関数と呼ばれる Uri を構築する方法に注意してください `_serviceURI` 、特定の Azure Search サービスに基づいており、という名前の HttpClient を作成し、 `_httpClient` を使用する、API キーをこの Search サービスを認証します。

2.  `ChangeEnumeratorSql` と `変更セット` 、SQL Server の AdventureWorks データベースにある Products テーブルからデータを列挙するために使用します。

3.  このテーブルから収集されたデータに基づく `ApplyChanges` がこのデータを Azure Search インデックスに適用するのには呼び出されます。

4.  移動 `ApplyChanges` 同じファイルにします。 既に存在する場合に、この関数が、インデックスを削除する方法に注意してください (`DeleteCatalogIndex`) し、"catalog"という名前の新しいインデックスを作成 (`CreateCatalogIndex`)。

5.  移動、 `CreateCatalogIndex` 機能、SQL Server の Products テーブルの列に一致するスキーマに対するインデックスの作成方法に注意してください。 各フィールドには、型 (つまり、 `Edm.String` または `Edm.Double`) に使用されるこれらのフィールドを定義する属性とします。 参照してください、 [Azure Search REST API のドキュメント](http://msdn.microsoft.com/library/azure/dn798935.aspx) これらの属性の詳細についてです。

6.  戻り、 `ApplyChanges` 関数です。 この関数がすべての列挙された変更でデータをループ処理方法に注意してください。 `変更セット`します。 変更を 1 つずつ適用するのではなく、1000 個単位のグループにバッチ化してから Search サービスに適用します。 この方法は、ドキュメントを 1 つずつ適用するよりはるかに効率的です。

ここまでは、SQL Server のリレーショナル データを使用してインデックスを作成および取り込む方法を見てきましたが、次は、検索データを使用した製品カタログの構築方法について説明しましょう。

<a id="sub-4"></a>
## Azure Search を使用した MVC4 アプリケーションの構築

この手順では、Web ブラウザーで検索アプリケーションを構築し、実行します。

1.  Visual Studio で、**AdventureWorksCatalog.sln** という名前の Azure Search デモ ソリューションを開きます。

2.  ソリューション エクスプローラーで **AdventureWorksWeb** を右クリックし、**[スタートアップ プロジェクトに設定]** を選択します。

3.  このプロジェクトの **Web.config** を開き、"SearchServiceName" の値と "SearchServiceApiKey" の値を、使用する Azure Search サービスの該当する値に更新します。 Search サービス名については、"mysearch.search.windows.net" というサービスであれば、"mysearch" と入力します。

4.  **Web.config** を保存します。

5.  **F5** キーを押して、プロジェクトを起動します。 手順に従います。 [トラブルシューティング](#err-mvc) ビルド エラーが出た場合は、ステップです。

    ![][10]

6.  検索ボックスは空のままにし、**[検索]** をクリックすると、294 個すべての製品が表示されます。

7.  左側にファセットのリストがあります。 いずれかのファセットをクリックしてみてください。 検索が再び実行されますが、今回は、選択したファセットが追加されて結果がフィルタリングされます。 最初の行にブレークポイントを追加することも、 **HomeController.cs** `検索` 各行 (f11) する関数。

7.  "mountain bikes" などの検索語句を入力します。 入力すると、クエリの候補がドロップダウン リストで表示されます。

    ![][11]

次に、これまでの説明の復習として、このデモで表示されるスクリーンショットを最初から順に示します。 これまでのセクションでは、ファセット ナビゲーション (左側)、ページ上部に表示されるドキュメント数、提案について説明しました。さらに、関連性、リスト、または価格でソートするソートのオプションも取り上げました。

   ![][7]


<a id="sub-5"></a>
## AdventureWorksWeb の探索

プロジェクト AdventureWorksWeb の内容を調べることで、ASP.NET MVC 4 を使用して Web アプリケーションから Azure Search を利用する方法がわかります。 このセクションでは、このアプリケーションのコードの個々の部分を見ながら何が行われているのかを説明します。

1.  ソリューション エクスプローラーで、**AdventureWorksWeb** | **Controller** を展開して **HomeController.cs** を開きます。

    これは、インデックス ビューからの操作を管理する MVC コントローラーです。 コント ローラーの上部にあるへの参照が表示されます `CatalogSearch _catalogSearch` 、Azure Search サービスへの HttpClient 接続オブジェクトを作成します。 そのためコード `CatalogSearch` にある **CatalogSeach.cs**

2. **HomeController.cs** の内部には、主要な関数として次の 2 つがあります。

    **Search**: ユーザーが検索ボタンをクリックするかファセットを選択すると、この関数が呼び出されて結果が取得され、その結果がインデックス ビューに戻されて表示されます。

    **Suggest**: ユーザーが検索ボックスに入力を開始すると、この関数が呼び出され、Azure Search インデックスの内容に基づいて候補のリストが返されます。

これら 2 つの関数について、もう少し詳しく見ていきましょう。

3.  **HomeController.cs** `検索` 関数への呼び出しがわかります `_catalogSearch.Search`, 、Azure Search サービスへの接続オブジェクトが含まれています。 **CatalogSearch.cs** に含まれている Search 関数を呼び出すと、この関数がこれらのパラメーターを利用して Azure Search クエリをビルドすることがわかります。 クエリの結果がという名前の JSON オブジェクトに格納されている `結果` に渡されると、 `インデックス` ビューの結果は解析され、web ページに表示できます。

4.  [ビュー | ホーム] から **Index.cshtml** を開くと、これらの結果を解析するために使用されるコードが表示されます。

5.  まだ実行中のアプリケーションがある場合は停止し、[ビュー | ホーム] で **Index.cshtml** ファイルを開きます。 このファイルの最後に、使用する JavaScript 関数が表示されます `JQuery $(function ())`します。 この関数は、ページが読み込まれたときに呼び出されます。 JQuery オートコンプリート関数を使用し、この関数を "q" という ID で、検索テキスト ボックスからのコールバックとしてリンクします。 テキスト ボックスに入力されるたびに、この自動補完の関数が呼び出され、今度は、入力された内容によって /home/suggest が呼び出されます。 `/提案` 、関数への参照は、 **HomeController.cs** と呼ばれる `Suggest`します。

6.  **HomeController.cs** を開いて、Suggest 関数に移動します。 このコードは、Search 関数を使用するとよく似て、 `_catalogSearch` オブジェクト関数を呼び出すには **CatalogSearch.cs** と呼ばれる `候補を表示する`します。 検索クエリを作成するのではなく、 `Suggest` 関数への呼び出しは、 [Suggestions API](http://msdn.microsoft.com/library/azure/dn798936.aspx)します。 この API は、テキスト ボックスに入力された語句を使用して、想定される候補のリストを作成します。 値は **Index.cshtml** ファイルに返され、入力に先行して表示されるオプションとして、検索ボックスに自動的に一覧表示されます。

ここで、候補を作成するフィールドを Azure Search がどのように認識するのか、考えてみましょう。 その答えは、インデックスを作成した時点にあります。  `CreateCatalogIndex`  Program.cs ファイル内の関数の **CatalogIndexer** という名前の属性があるプロジェクトで、 `提案`します。 この属性に設定するときに `True`, をしているため Azure Search 使用できることをフィールドとして候補を取得します。

以下のことを試してみましょう。

7.  もう一度、アプリケーションをビルドして、**F5** キーを押して実行します。

8.  検索ボックスに "Road" という言葉を入力します。 すると間もなくテキスト ボックスの下部に、ユーザーが選択すると思われる項目の候補が一覧表示されます。

ブレークポイントを追加することができます、 `Suggest` 内で機能 **HomeController.cs** および候補リストを構築するために行われた各呼び出しのステップ (F11) 実行します。

これでデモは終了です。 Azure Search を使用して ASP.NET MVC4 アプリケーションを構築する前に理解しておく必要がある主な操作をすべて説明しました。


<a id="err-mvc"></a>
## 「ファイルまたはアセンブリ 'System.Web.Mvc' が読み込めませんでした」の解決方法

AdventureWorksWeb をビルドするときに、「ファイルまたはアセンブリ 'System.Web.Mvc, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'、またはその依存関係の 1 つが読み込めませんでした」というエラーが出た場合は、次の手順を試してエラーを解決してください。

1. **[ツール]**、**[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順にクリックし、パッケージ マネージャー コンソールを開きます。
2. PM> プロンプトに、「Update-package -reinstall Microsoft.AspNet.Mvc」と入力します。
3. ファイルを再ロードするように求められたら、**[すべてはい]** を選択します。
4. ソリューションを再ビルドし、**F5** キーをもう一度押します。


<a id="next-steps"></a>
## 次のステップ

追加で行う自習として、Search 結果のいずれかをユーザーがクリックしたときに開く [詳細] ページを追加することを検討してください。 準備として以下のことができます。

+   については、 [参照 API](http://msdn.microsoft.com/library/azure/dn798929.aspx) (たとえば、productID を渡すことができます) 特定のドキュメントを返すための Azure Search のクエリを作成することができます。
+   **HomeController.cs** ファイルに Details という新しい関数を追加します。 この検索の結果を受け取り、結果を表示するために、対応する **Details.cshtml** ビューを追加します。
+   この追加のコード サンプルと地理空間検索のビデオ: [Channel 9 - Azure Search と地理空間データ](http://channel9.msdn.com/Shows/Data-Exposed/Azure-Search-and-Geospatial-Data) と [CodePlex: Azure Search GeoSearch Sample](http://azuresearchgeospatial.codeplex.com)

確認することも、 [Azure Search REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) MSDN にします。





[prerequisites]: #sub-1 
[create an azure search index]: #sub-2 
[explore catalogindexer]: #sub-3 
[build an mvc4 application using azure search]: #sub-4 
[explore adventureworksweb]: #sub-5 
[next steps]: #next-steps 
[7]: ./media/search-create-first-solution/AzureSearch_Create1_WebApp.PNG 
[8]: ./media/search-create-first-solution/AzureSearch_Create1_ConsoleMsg.PNG 
[9]: ./media/search-create-first-solution/AzureSearch_Create1_DashboardIndexes.PNG 
[10]: ./media/search-create-first-solution/AzureSearch_Create1_WebAppEmpty.PNG 
[11]: ./media/search-create-first-solution/AzureSearch_Create1_Suggestions.PNG 
[12]: ./media/search-create-first-solution/AzureSearch_Create1_CodeplexDownload.PNG 

