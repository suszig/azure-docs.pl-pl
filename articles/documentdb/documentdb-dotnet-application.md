<properties 
    pageTitle="DocumentDB の ASP.NET MVC チュートリアル: Web アプリケーションの開発 | Microsoft Azure" 
    description="この ASP.NET MVC チュートリアルでは、DocumentDB を使用して MVC Web アプリケーションを作成します。 JSON を格納し、Azure Websites でホストされている ToDo アプリからデータにアクセスします。" 
    keywords="asp.net mvc tutorial, web application development, mvc web application, asp net mvc tutorial step by step"
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
    ms.topic="hero-article" 
    ms.date="11/18/2015" 
    ms.author="ryancraw"/>

#<a name="_Toc395809351"></a>DocumentDB を使用した ASP.NET MVC Web アプリケーションの開発

> [AZURE.SELECTOR]
- [.NET](documentdb-dotnet-application.md)
- [Node.js](documentdb-nodejs-application.md)
- [Java](documentdb-java-application.md)
- [Python](documentdb-python-application.md) 

この記事では、Azure DocumentDB を効果的に活用して、JSON ドキュメントの保存とクエリを行う方法を説明します。ToDo アプリを Azure DocumentDB を使って構築するエンド ツー エンドの手順を説明します。 対象となるタスクは、JSON ドキュメントとして Azure DocumentDB に保存するものとします。

![このチュートリアルで作成された、ToDo リスト MVC Web アプリケーションのスクリーン ショット - ASP NET MVC チュートリアル ステップ バイ ステップ](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image1.png)

このチュートリアルでは、Azure で提供される DocumentDB サービスを使用して、Azure にホストされている ASP.NET MVC Web アプリケーションからデータを保存したりデータにアクセスしたりする方法を説明します。

> [AZURE.TIP] このチュートリアルでは、ASP.NET MVC と Azure Websites を使用した経験があることを前提としています。 ASP.NET に慣れていない場合、または [前提条件となるツール](#_Toc395637760), から完全なサンプル プロジェクトをダウンロードすることをお勧め [GitHub][] このサンプルの手順に従います。 プロジェクトをビルドした後でこの記事を見直すと、プロジェクトのコンテキストのコードについての洞察を得ることができます。

## <a name="_Toc395637760"></a>このデータベース チュートリアルの前提条件

この記事の手順を実行する前に、次のソフトウェアがインストールされていることを確認してください。

- アクティブな Azure アカウント。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト](../../pricing/free-trial/)をご覧ください。
- [Visual Studio 2013](http://www.visualstudio.com/) Update 4 以降。
- Azure SDK for .NET バージョン 2.5.1 または高いで使用できる、 [Microsoft Web Platform Installer][]します。

この記事に掲載されているすべてのスクリーン ショットは、Visual Studio 2013 Update 4 および Azure SDK for .NET Version 2.5.1 で撮影しました。 ご利用のシステムにインストールされているバージョンと異なる場合、画面やオプション設定が一部異なる可能性もありますが、上記の前提条件を満たしていれば、アプリケーションの動作に支障はありません。

## <a name="_Toc395637761"></a>手順 1: DocumentDB データベース アカウントを作成する

最初に、DocumentDB アカウントを作成します。 既にアカウントがある場合にスキップできます [新しい ASP.NET MVC アプリケーション作成](#_Toc395637762)します。

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

[AZURE.INCLUDE [documentdb-keys](../../includes/documentdb-keys.md)]

<br/>
次のセクションで、新しい ASP.NET MVC アプリケーションをゼロから作成する方法について説明します。 

## <a name="_Toc395637762"></a>手順 2: 新しい ASP.NET MVC アプリケーションを作成する

アカウントが用意できたので、新しい ASP.NET プロジェクトを作成します。

1. Visual Studio での **ファイル** ] メニューをポイント **新規**, 、] をクリックし、 **プロジェクト**します。

     **新しいプロジェクト** ] ダイアログ ボックスが表示されます。
2.  **プロジェクトの種類** ] ウィンドウで、展開 **テンプレート**, 、**Visual c#**, 、**Web**, 、し、[ **ASP.NET Web アプリケーション**します。

    ![ASP.NET Web アプリケーション プロジェクトの種類が強調表示されている [新しいプロジェクト] ダイアログ ボックスのスクリーン ショット](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image10.png)

3.  **名前** ボックスに、プロジェクトの名前を入力します。 このチュートリアルでは、"todo" という名前を使用します。 これ以外の名前を使用する場合は、このチュートリアルで todo 名前空間について言及されているすべての場所で、提供されているコード サンプルを、ここでアプリケーションに対して指定した名前に変更する必要があります。 

4. をクリックして **参照** クリックして、プロジェクトを作成するフォルダーに移動する **OK**します。

     **新しい ASP.NET プロジェクト** ] ダイアログ ボックスが表示されます。

    ![MVC アプリケーション テンプレートが強調表示され、[クラウドでのホスト] ボックスがオンになっている、[新しい ASP.NET プロジェクト] ダイアログ ボックスのスクリーン ショット](./media/documentdb-dotnet-application/asp-net-mvc-tutorial-image11.png)

5. [テンプレート] ペインで選択 **MVC**します。

6. Azure でアプリケーションをホストしているように計画する場合は、[ **クラウドでホスト** azure でアプリケーションをホストする右下にします。 これで、アプリケーションをクラウドでホストすること、および Azure Web サイトでホストされるアプリケーションを実行することを選択しました。 このオプションを選択すると、Azure Websites があらかじめ自動的にプロビジョニングされ、最終的に完成したアプリケーションをデプロイする作業が段違いにやりやすくなります。 この他の場所をホストするか、なりますが、Azure を構成し、オフにしたくない場合 **クラウドでホスト**します。

7. クリックして **OK** と、Visual Studio のスキャフォールディング機能によって空の ASP.NET MVC テンプレートです。 

8. これをクラウドでホストすることを選択した場合、少なくとも 1 つの画面が表示され、Azure アカウントにログインして新しい Web サイトの値を指定するよう求められます。 追加の値をすべて指定して続行します。 

    ここでは Azure SQL Database サーバーを使用しないため、ここで [データベース サーバー] を選択していません。後で Azure DocumentDB アカウントを Azure ポータルで作成する予定です。

    選択の詳細については、 **App Service プラン** と **リソース グループ**, を参照してください [Azure App Service プランの詳細な概要](azure-web-sites-web-hosting-plans-in-depth-overview.md)します。

    ![[Microsoft Azure Websites を構成する] ダイアログ ボックスのスクリーン ショット](./media/documentdb-dotnet-application/image11_1.png)

9. Visual Studio によってスケルトン MVC アプリケーションが作成されると、空の ASP.NET アプリケーションをローカルに実行できる状態となります。

    読者の皆さんは、ASP.NET の "Hello World" アプリケーションで体験済みだと思いますので、プロジェクトをローカルに実行する手順は省略します。 早速このプロジェクトに DocumentDB を追加して、アプリケーションを構築しましょう。

## <a name="_Toc395637767"></a>手順 3: DocumentDB を MVC Web アプリケーション プロジェクトに追加する

必要なプラミングを ASP.NET MVC のほとんどができました
このソリューションでは、Azure DocumentDB、MVC web アプリケーションを追加、このチュートリアルの本題さあ、始めましょう。

1. DocumentDB .NET SDK は、NuGet パッケージの形式で配布されています。 Visual Studio で NuGet パッケージの取得でプロジェクトを右クリックして Visual Studio で NuGet パッケージ マネージャーを使用します。 **ソリューション エクスプ ローラー** クリックし、 **NuGet パッケージの管理**します。

    ![[NuGet パッケージの管理] が強調表示されている、ソリューション エクスプローラーでの Web アプリケーション プロジェクトの右クリック オプションのスクリーン ショット](./media/documentdb-dotnet-application/image21.png)

     **NuGet パッケージの管理** ] ダイアログ ボックスが表示されます。

2.  **オンライン検索** ボックスに、入力 ***Azure DocumentDB***します。 
    
    インストールの結果から、 **Microsoft Azure DocumentDB クライアント ライブラリ** パッケージです。 これにより、DocumentDB パッケージだけでなく、依存関係のあるすべてのコンポーネント (Newtonsoft.Json など) がダウンロードされてインストールされます。

    ![Microsoft Azure DocumentDB クライアント ライブラリが強調表示されている [NuGet パッケージの管理] ウィンドウのスクリーン ショット](./media/documentdb-dotnet-application/nuget.png)

    または、パッケージ マネージャー コンソールを使用してパッケージをインストールすることもできます。 [、 **ツール** ] メニューのをクリックして **NuGet パッケージ マネージャー**, 、クリックして **パッケージ マネージャー コンソール**します。 プロンプトで、次のように入力します。

        Install-Package Microsoft.Azure.DocumentDB

3. パッケージがインストールされると、次のように、Microsoft.Azure.Documents.Client と Newtonsoft.Json の 2 つの新しい参照が Visual Studio ソリューションに追加されます。

    ![ソリューション エクスプローラーで JSON データ プロジェクトに追加された 2 つの参照のスクリーン ショット](./media/documentdb-dotnet-application/image22.png)


##<a name="_Toc395637763"></a>手順 4: ASP.NET MVC アプリケーションをセットアップする
 
次に、モデル、ビュー、およびコントローラーをこの MVC アプリケーションに追加します。

- [モデルを追加する](#_Toc395637764)です。
- [コント ローラーを追加](#_Toc395637765)します。
- [ビューの追加](#_Toc395637766)します。


### <a name="_Toc395637764"></a>JSON データ モデルを追加する

最初に作成する、 **M** MVC では、モデルです。 

1.  **ソリューション エクスプ ローラー**, を右クリックし、 **モデル** フォルダー] をクリックして **追加**, 、] をクリックし、 **クラス**します。

     **新しい項目の追加** ] ダイアログ ボックスが表示されます。

2. 新しいクラスを名前 **Item.cs** ] をクリック **追加**します。 

3. この新しい **Item.cs** ファイルに追加し、最後の後に次 *ステートメントを使用して*します。
        
        using Newtonsoft.Json;
    
4. 次のコード 
        
        public class Item
        {
        }

    を、以下のコードに置き換えます。
        
        public class Item
        {
            [JsonProperty(PropertyName="id")]
            public string Id { get; set; }
        
            [JsonProperty(PropertyName="name")]
            public string Name { get; set; }
        
            [JsonProperty(PropertyName = "desc")]
            public string Description { get; set; }
        
            [JsonProperty(PropertyName="isComplete")]
            public bool Completed { get; set; }    
        }

    DocumentDB のすべてのデータは、JSON 形式でネットワーク越しに渡され、保存されます。 オブジェクトをシリアル化または逆シリアル化して JSON.NET によって方法を制御する、 **JsonProperty** 属性、 **項目** クラスを作成しました。 しない **が** このを実行する場合は、プロパティが JSON camelCase 名前付け規則に従うことを確認をします。 
    
    だけでなくを指定するプロパティ名の形式と JSON になりますが、完全名を変更できる、.NET プロパティに対して行ったように、 **説明** プロパティです。 
    

### <a name="_Toc395637765"></a>コントローラーを追加する

以上で、 **M**, 、今すぐ作成してみましょう、 **C** MVC コント ローラー クラスでします。

1.  **ソリューション エクスプ ローラー**, を右クリックし、 **コント ローラー** フォルダーをクリックして **追加**, 、順にクリック **コント ローラー**します。

     **スキャフォールディングの追加** ] ダイアログ ボックスが表示されます。

2. 選択 **MVC 5 コント ローラー - 空** ] をクリックし、 **追加**します。

    ![[MVC 5 コントローラー - 空] オプションが強調表示されている [スキャフォールディングの追加] ダイアログ ボックスのスクリーン ショット](./media/documentdb-dotnet-application/image14.png)

3. 名前を新しいコント ローラーに **ItemController します。**

    ![[コントローラーの追加] ダイアログ ボックスのスクリーン ショット](./media/documentdb-dotnet-application/image15.png)

    ファイルが作成されると、Visual Studio ソリューション次のように、新しい ItemController.cs ファイルと **ソリューション エクスプ ローラー**します。 前に作成した新しい Item.cs ファイルも表示されます。

    ![Visual Studio ソリューションのスクリーン ショット - 新しい ItemController.cs ファイルと Item.cs ファイルが強調表示されているソリューション エクスプローラー](./media/documentdb-dotnet-application/image16.png)

    ItemController.cs は閉じてもかまいません。後でまた使用します。 

### <a name="_Toc395637766"></a>ビューを追加する

これで、作成、 **V** MVC ビューでは。

- [項目のインデックス ビューを追加](#AddItemIndexView)します。
- [[新しい項目のビューを追加する](#AddNewIndexView)です。
- [[アイテムの編集ビューを追加する](#_Toc395888515)です。


#### <a name="AddItemIndexView"></a>項目のインデックス ビューを追加する

1.  **ソリューション エクスプ ローラー**, 、展開、 **ビュー**  フォルダー、空を右クリックして **項目** を追加したときの Visual Studio が作成したフォルダー、 **ItemController** 以前で、をクリックして **追加**, 、] をクリックし、 **ビュー**します。

    ![[ビューの追加] コマンドが強調表示された状態の、Visual Studio で作成された Item フォルダーが示されているソリューション エクスプローラーのスクリーン ショット](./media/documentdb-dotnet-application/image17.png)

2.  **ビューの追加** ] ダイアログ ボックスで、次の操作します。
    -  **ビュー名** ボックスに、入力 ***インデックス***します。
    -  **テンプレート** ボックスで、 ***リスト***します。
    -  **モデル クラス** ボックスで、 ***項目 (todo します。モデル)***します。
    - ままにして、 **データ コンテキスト クラス** ボックスは空白です。 
    - レイアウト ページ ボックスに「 ***~/Views/Shared/_Layout.cshtml***します。
    
    ![[ビューの追加] ダイアログ ボックスのスクリーン ショット](./media/documentdb-dotnet-application/image18.png)

3. これらすべての値を設定すると、クリックして **追加** と、Visual Studio の新しいテンプレート ビューを作成します。 完了すると、作成された cshtml ファイルを開きます。 このファイルは閉じてください。後で再度使用します。

#### <a name="AddNewIndexView"></a>新しい項目を作成するためのビューを追加する

作成方法のような **項目のインデックス** ビューを作成新規作成するための新しいビュー **項目**します。

1.  **ソリューション エクスプ ローラー**, を右クリックし、 **項目** フォルダーを再びクリックして **追加**, 、] をクリックし、 **ビュー**します。

2.  **ビューの追加** ] ダイアログ ボックスで、次の操作します。
    -  **ビュー名** ボックスに、入力 ***作成***します。
    -  **テンプレート** ボックスで、 ***作成***します。
    -  **モデル クラス** ボックスで、 ***項目 (todo します。モデル)***します。
    - ままにして、 **データ コンテキスト クラス** ボックスは空白です。
    - レイアウト ページ ボックスに「 ***~/Views/Shared/_Layout.cshtml***します。
    - クリックして **追加**します。

#### <a name="_Toc395888515"></a>項目を編集するためのビューを追加する

最後に、もう 1 つのビューを編集するため、 **項目** 以前と同じようにします。

1.  **ソリューション エクスプ ローラー**, を右クリックし、 **項目** フォルダーを再びクリックして **追加**, 、] をクリックし、 **ビュー**します。

2.  **ビューの追加** ] ダイアログ ボックスで、次の操作します。
    -  **ビュー名** ボックスに、入力 ***編集***します。
    -  **テンプレート** ボックスで、 ***編集***します。
    -  **モデル クラス** ボックスで、 ***項目 (todo します。モデル)***します。
    - ままにして、 **データ コンテキスト クラス** ボックスは空白です。 
    - レイアウト ページ ボックスに「 ***~/Views/Shared/_Layout.cshtml***します。
    - クリックして **追加**します。

この作業が済んだら、Visual Studio に表示されている cshtml ドキュメントをすべて閉じてください。これらのビューは後で使用します。

## <a name="_Toc395637769"></a>手順 5: DocumentDB を接続する

MVC の標準的な構成要素を準備できたので、次に DocumentDB 用のコードを追加します。 

このセクションでは、次の処理を行うコードを追加します。

- [未完了の項目を一覧表示する](#_Toc395637770)です。
- [項目を追加する](#_Toc395637771)です。
- [項目の編集](#_Toc395637772)します。

### <a name="_Toc395637770"></a>MVC Web アプリケーションの未完了項目の一覧表示

最初に、DocumentDB に接続して使用するためのすべてのロジックを含むクラスを追加します。 このチュートリアルでは、このすべてのロジックを DocumentDBRepository という名前のリポジトリ クラスにカプセル化します。 

1.  **ソリューション エクスプ ローラー**, で、プロジェクトを右クリックし、 **追加**, 、クリックして **クラス**します。 新しいクラスの名前を **DocumentDBRepository** ] をクリック **追加**します。
 
2. 新しく作成された **DocumentDBRepository** クラスにし、次の追加 *ステートメントを使用して* 上、 *名前空間* 宣言
        
        using Microsoft.Azure.Documents; 
        using Microsoft.Azure.Documents.Client; 
        using Microsoft.Azure.Documents.Linq; 
        using System.Configuration;
        using System.Linq.Expressions;
        using System.Threading.Tasks;

    次のコード 

        public class DocumentDBRepository
        {
        }

    を、以下のコードに置き換えます。

        
        public static class DocumentDBRepository<T>
        {
            //Use the Database if it exists, if not create a new Database
            private static Database ReadOrCreateDatabase()
            {
                var db = Client.CreateDatabaseQuery()
                                .Where(d => d.Id == DatabaseId)
                                .AsEnumerable()
                                .FirstOrDefault();
                
                if (db == null)
                {
                    db = Client.CreateDatabaseAsync(new Database { Id = DatabaseId }).Result;
                }
                
                return db;
            }
            
            //Use the DocumentCollection if it exists, if not create a new Collection
            private static DocumentCollection ReadOrCreateCollection(string databaseLink)
            {
                var col = Client.CreateDocumentCollectionQuery(databaseLink)
                                  .Where(c => c.Id == CollectionId)
                                  .AsEnumerable()
                                  .FirstOrDefault();
        
                if (col == null)
                {
                    var collectionSpec = new DocumentCollection { Id = CollectionId };
                    var requestOptions = new RequestOptions {OfferType = "S1" };
                    
                    col = Client.CreateDocumentCollectionAsync(databaseLink, collectionSpec, requestOptions).Result;
                }
                
                return col;
            }
            
            //Expose the "database" value from configuration as a property for internal use
            private static string databaseId;
            private static String DatabaseId
            {
                get
                {
                    if (string.IsNullOrEmpty(databaseId))
                    {
                        databaseId = ConfigurationManager.AppSettings["database"];
                    }
                
                    return databaseId;
                }
            }
            
            //Expose the "collection" value from configuration as a property for internal use
            private static string collectionId;
            private static String CollectionId
            {
                get
                {
                    if (string.IsNullOrEmpty(collectionId))
                    {
                        collectionId = ConfigurationManager.AppSettings["collection"];
                    }
                
                    return collectionId;
                }
            }
            
            //Use the ReadOrCreateDatabase function to get a reference to the database.
            private static Database database;
            private static Database Database
            {
                get
                {
                    if (database == null)
                    {
                        database = ReadOrCreateDatabase();
                    }
                    
                    return database;
                }
            }
            
            //Use the ReadOrCreateCollection function to get a reference to the collection.
            private static DocumentCollection collection;
            private static DocumentCollection Collection
            {
                get
                {
                    if (collection == null)
                    {
                        collection = ReadOrCreateCollection(Database.SelfLink);
                    }
                    
                    return collection;
                }
            }
            
            //This property establishes a new connection to DocumentDB the first time it is used, 
            //and then reuses this instance for the duration of the application avoiding the
            //overhead of instantiating a new instance of DocumentClient with each request
            private static DocumentClient client;
            private static DocumentClient Client
            {
                get
                {
                    if (client == null)
                    {
                        string endpoint = ConfigurationManager.AppSettings["endpoint"];
                        string authKey = ConfigurationManager.AppSettings["authKey"];
                        Uri endpointUri = new Uri(endpoint);
                        client = new DocumentClient(endpointUri, authKey);
                    }
                    
                    return client;
                }
            }
        }

    > [AZURE.TIP] When creating a new DocumentCollection you can supply an optional RequestOptions parameter of OfferType, which allows you to specify the performance level of the new collection. If this parameter is not passed the default offer type will be used. For more on DocumentDB offer types please refer to [DocumentDB Performance Levels](documentdb-performance-levels.md)

3. 構成では、開きからいくつかの値を読み取るので、 **Web.config** 、アプリケーションのファイルを下にある次の行を追加、 `<AppSettings>` セクションです。
    
        <add key="endpoint" value="enter the URI from the Keys blade of the Azure Portal"/>
        <add key="authKey" value="enter the PRIMARY KEY, or the SECONDARY KEY, from the Keys blade of the Azure  Portal"/>
        <add key="database" value="ToDoList"/>
        <add key="collection" value="Items"/>
    
4. 値は、今すぐ更新 *エンドポイント* と *authKey* Azure ポータルの [キー] ブレードを使用します。 使用して、 **URI** endpoint 設定の値として [キー] ブレードから、 **主キー**, 、または **セカンダリ キー** authKey 設定の値として [キー] ブレードからです。


    That takes care of wiring up the DocumentDB repository, now let's add our application logic.

5. 最初に、この todo リスト アプリケーションに、未完了の項目を表示する機能を追加します。  コピーして貼り付け、次のコード スニペット任意の場所内で、 **DocumentDBRepository** クラスです。

        public static IEnumerable<T> GetItems(Expression<Func<T, bool>> predicate) 
        {
            return Client.CreateDocumentQuery<T>(Collection.DocumentsLink) 
                .Where(predicate) 
                .AsEnumerable(); 
        } 

6. 開いている、 **ItemController** 前に追加し、以下の追加お *ステートメントを使用して* 名前空間宣言の上にします。

        using System.Net;
        using System.Threading.Tasks;
        using todo.Models;

    プロジェクトの名前が "todo" ではない場合は、using "todo.Models"; をプロジェクトの名前に合わせて更新する必要があります。

    次のコード

        //GET: Item
        public ActionResult Index()
        {
            return View();
        }

    を、以下のコードに置き換えます。

        public ActionResult Index()
        {
            var items = DocumentDBRepository<Item>.GetItems(d => !d.Completed);
            return View(items);
        }
    
この時点で、エラーなくソリューションをビルドすることは可能です。

行うアプリケーションを実行した場合、 **HomeController** と **インデックス** そのコント ローラーのビューです。 これは、作業の開始時に選択した MVC テンプレート プロジェクトの既定の動作であって、期待していた動作ではありません。 この MVC アプリケーションのルーティングに手を加えて、この動作を変更しましょう。

開いている ***App\_Start\RouteConfig.cs*** で始まる行を見つけます"の既定値:"し、次のように変更します。

        defaults: new { controller = "Item", action = "Index", id = UrlParameter.Optional }

これでいる場合を ASP.NET MVC に伝えますの代わりに、ルーティング動作を制御する URL の値が指定されていない **ホーム**, を使用して **項目** コント ローラーとしてユーザー **インデックス** ビューとします。

アプリケーションを実行する場合に呼び出しますので、 **ItemController** をリポジトリ クラスへのコールおよび GetItems メソッドへのすべての不完全な項目を使用して、 **ビュー**\\**項目**\\**インデックス** 表示します。 

このプロジェクトをビルドして実行すると、次のように表示されます。    

![このデータベース チュートリアルで作成された、ToDo リスト Web アプリケーションのスクリーン ショット](./media/documentdb-dotnet-application/image23.png)

### <a name="_Toc395637771"></a>項目の追加

空のグリッドでは物足りないので、データベースにいくつか項目を追加してみましょう。

DocumentDBRepository および ItemController を DocumentDB にレコードを保持するには、いくつかのコードを追加してみましょう。

1.  次のメソッドを追加、 **DocumentDBRepository** クラスです。

        public static async Task<Document> CreateItemAsync(T item)
        {
            return await Client.CreateDocumentAsync(Collection.SelfLink, item);
        }

    このメソッドは単に、渡されたオブジェクトを受け取って、DocumentDB に保存します。

2. ItemController.cs ファイルを開き、クラス内に次のコード スニペットを追加します。 これがどのように ASP.NET MVC に対して実行する、 **作成** アクション。 このケースでは、前に作成した関連する Create.cshtml ビューを指定します。

        public ActionResult Create()
        { 
            return View(); 
        }

    このコント ローラーから送信された内容を受け入れるにコードがいくつかありますが、 **作成** 表示します。

2. 次のコード ブロックを ItemController.cs クラスに追加します。このコードは、このコントローラーに対するフォームの POST で行う処理を ASP.NET MVC に伝えます。
    
        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> Create([Bind(Include =  "Id,Name,Description,Completed")] Item item)  
        {
            if (ModelState.IsValid)  
            {  
                await DocumentDBRepository<Item>.CreateItemAsync(item);
                return RedirectToAction("Index");  
            }   
            return View(item);   
        }
    このコードは DocumentDBRepository を呼び出し、CreateItemAsync メソッドを使用して新しい todo 項目をデータベースに保存します。 
 
    **セキュリティに関する注意**: **ValidateAntiForgeryToken** 属性は、クロスサイト リクエスト フォージェリ攻撃に対してこのアプリケーションを保護するためにここで使用します。 この属性を追加するだけでなく、偽造防止トークンもビューで処理する必要があります。 詳細については、件名、およびこれを正しく実装する方法の例を参照してください [クロスサイト リクエスト フォージェリの防止][]します。 提供されるソース コード [GitHub][] 場所に完全な実装を持ちます。

    **セキュリティに関する注意**: 使用しても、 **バインド** オーバーポスティング攻撃から保護するためのメソッド パラメーターの属性です。 詳細について参照してください。 [ASP.NET MVC での基本的な CRUD 操作][]します。

データベースに新しい項目を追加するために必要なコードは以上です。


### <a name="_Toc395637772"></a>項目の編集

最後には、私たちがあることをおを編集する機能を追加する **項目** データベースと完了済みとしてマークします。 編集ビューは既に追加されて、プロジェクトにコント ローラーにされ、コードを追加するだけの **DocumentDBRepository** クラスです。

1. 次のコードを追加、 **DocumentDBRepository** クラスです。

        public static T GetItem(Expression<Func<T, bool>> predicate)
        {
            return Client.CreateDocumentQuery<T>(Collection.DocumentsLink)
                        .Where(predicate)
                        .AsEnumerable()
                        .FirstOrDefault();
        }       

        public static async Task<Document> UpdateItemAsync(string id, T item)
        {
            Document doc = GetDocument(id); 
            return await Client.ReplaceDocumentAsync(doc.SelfLink, item);
        }

        private static Document GetDocument(string id) 
        { 
            return Client.CreateDocumentQuery(Collection.DocumentsLink) 
                .Where(d => d.Id == id) 
                .AsEnumerable() 
                .FirstOrDefault(); 
        } 
    
    これらのメソッドの最初の **GetItem** に戻されます DocumentDB から Item を取得、 **ItemController** 後、 **編集** 表示します。
    
    2 番目のメソッドを追加しました置換、 **ドキュメント** のバージョンを使用して DocumentDB 内、 **ドキュメント** から渡された、 **ItemController**します。

2. 次のコードを追加、 **ItemController** クラスです。

        public ActionResult Edit(string id)
        {
            if (string.IsNullOrEmpty(id))
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
                     
            Item item = (Item)DocumentDBRepository<Item>.GetItem(d => d.Id == id);
            
            if (item == null)
            {
                return HttpNotFound();
            }
            
            return View(item);
        }
        
        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<ActionResult> Edit([Bind(Include = "Id,Name,Description,Completed")] Item item)
        {
           if (ModelState.IsValid)
            {
                await DocumentDBRepository<Item>.UpdateItemAsync(item.Id, item);
                return RedirectToAction("Index");
            }

          return View(item);
        }
        
    
    ユーザーがクリックした場合に発生する Http GET を処理する最初のメソッド、 **編集** からのリンク、 **インデックス** 表示します。 このメソッドをフェッチ、 [**ドキュメント**](http://msdn.microsoft.com/library/azure/microsoft.azure.documents.document.aspx) DocumentDB からに渡されます、 **編集** ビューです。

     **編集** ビューは、Http POST を行います、 **IndexController**します。 
    
    追加した 2 つ目のメソッドは、更新されたオブジェクトをデータベースに保存するために、DocumentDB への引き渡しを処理します。

これが必要なすべてをアプリケーションを実行し、不完全な **項目**, 、新規追加 **項目**, 、および編集 **項目**します。

## <a name="_Toc395637773"></a>手順 6: ローカルでアプリケーションを実行する

ローカル コンピューターでアプリケーションをテストするには、次の操作を行います。

1. Visual Studio で F5 キーを押して、デバッグ モードでアプリケーションをビルドします。 すると、アプリケーションがビルドされてブラウザーが起動し、先ほど見た空のグリッド ページが表示されます。

    ![このデータベース チュートリアルで作成された、ToDo リスト Web アプリケーションのスクリーン ショット](./media/documentdb-dotnet-application/image24.png)

    自分のコード サンプル プロジェクトを比較するにはエラーにこの時点で発生した場合 [GitHub][]

2. をクリックして、 **[新規作成** リンクし、値を追加する、 **名前** と **説明** フィールドです。 ままにして、 **完了** ] チェック ボックスを選択しないそれ以外の場合、新しい **項目** は完了済みの状態で追加され、最初のリストには表示されません。

    ![Create ビューのスクリーン ショット](./media/documentdb-dotnet-application/image25.png)

3. をクリックして **作成** にリダイレクトして、 **インデックス** ビューと **項目** 、一覧に表示されます。

    ![Index ビューのスクリーン ショット](./media/documentdb-dotnet-application/image26.png)

    ご自由にさらにいくつかの追加 **項目** todo リストにします。

3. をクリックして **編集** ] の横に、 **項目** 一覧を **編集** 、オブジェクトのプロパティを更新するビューを含む、 **完了** フラグ。 マークした場合、 **完了** フラグを設定し、クリックして **保存**, 、 **項目** 未完了タスクの一覧から削除されます。

    ![[Completed] ボックスがオンになっている Index ビューのスクリーン ショット](./media/documentdb-dotnet-application/image27.png)

4. アプリケーションのテストが完了したら、Ctrl キーを押しながら F5 キーを押してアプリケーションのデバッグを中止します。 これで、アプリケーションをデプロイする準備が整いました。

##<a name="_Toc395637774"></a>手順 7: Azure Websites にアプリケーションをデプロイする

以上で、DocumentDB と連携するアプリケーションが完成しました。今度は、この Web アプリケーションを Azure Websites にデプロイします。 選択した場合は **クラウドでホスト** プロジェクトを Visual Studio に空の ASP.NET MVC を作成するときに、ごく簡単し、は生成する作業の大部分をします。 

1. プロジェクトを右クリックを行うには必要なはこのアプリケーションを発行する **ソリューション エクスプ ローラー** ] をクリック **発行**します。

    ![ソリューション エクスプローラーの [発行] オプションのスクリーン ショット](./media/documentdb-dotnet-application/image28.png)

2. すべては、あらかじめ構成に従って、資格情報です。実際には、サイトが既に作成された Azure での **送信先 URL** ] をクリックを行うだけで済みますが、 **発行**します。

    ![Visual Studio の [Web の発行] ダイアログ ボックスのスクリーン ショット - ASP NET MVC チュートリアル ステップ バイ ステップ](./media/documentdb-dotnet-application/image29.png)

数秒すると、Web アプリケーションの発行が完了し、ブラウザーが起動されます。作成したアプリケーションが Azure で実行されているようすが確認できます。

##<a name="_Toc395637775"></a>次のステップ

ご利用ありがとうございます。 ここでは初めての方を対象に、Azure DocumentDB を使用した ASP.NET MVC Web アプリケーションを作成し、Azure Websites に発行する方法を説明しました。 このチュートリアルに含まれていない詳細や削除の機能など、完全なアプリケーションのソース コードをダウンロードまたはから複製された [GitHub][]します。 これらの機能を自分のアプリケーションに追加する場合は、該当するコードを入手してアプリケーションに追加してください。

アプリケーションに追加の機能を追加するに入手できる Api を確認、 [Documentdb .NET ライブラリ](https://msdn.microsoft.com/library/azure/dn948556.aspx) 気軽に DocumentDB .NET ライブラリへの投稿に [GitHub][]します。 


[\*]: https://microsoft.sharepoint.com/teams/DocDB/Shared%20Documents/Documentation/Docs.LatestVersions/PicExportError
[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Preventing Cross-Site Request Forgery]: http://go.microsoft.com/fwlink/?LinkID=517254
[Basic CRUD Operations in ASP.NET MVC]: http://go.microsoft.com/fwlink/?LinkId=317598
[GitHub]: https://github.com/Azure-Samples/documentdb-net-todo-app


