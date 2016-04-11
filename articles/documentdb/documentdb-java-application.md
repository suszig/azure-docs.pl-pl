<properties 
    pageTitle="DocumentDB を使用した Java アプリケーション開発のチュートリアル | Microsoft Azure"
    description="この Java Web アプリケーション チュートリアルでは、Azure DocumentDB サービスを使用して、Azure Websites でホストされる Java アプリケーションからデータを格納する方法やデータにアクセスする方法について説明します。"
    keywords="Application development, database tutorial, java application, java web application tutorial, documentdb, azure, Microsoft azure"
    services="documentdb" 
    documentationCenter="java" 
    authors="aliuy" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.devlang="java" 
    ms.topic="hero-article" 
    ms.tgt_pltfrm="NA" 
    ms.workload="data-services" 
    ms.date="10/20/2015" 
    ms.author="andrl"/>

# DocumentDB を使用した Java Web アプリケーションの作成

> [AZURE.SELECTOR]
- [.NET](documentdb-dotnet-application.md)
- [Node.js](documentdb-nodejs-application.md)
- [Java](documentdb-java-application.md)
- [Python](documentdb-python-application.md)

この Java web アプリケーション チュートリアルで使用する方法、 [Microsoft Azure DocumentDB](https://portal.azure.com/#gallery/Microsoft.DocumentDB) サービスを格納し、Azure Websites でホストされる Java アプリケーションからデータにアクセスします。 このトピックでは、次の内容を説明します。

- Eclipse で、基本的な JSP アプリケーションを作成する方法。
- Azure DocumentDB サービスを使用して、使用する方法、 [DocumentDB Java SDK](https://github.com/Azure/azure-documentdb-java)します。

この Java Web アプリケーション チュートリアルでは、次の図に示すように、タスクを作成、取得、完了済みとしてマークできる、Web ベースのタスク管理アプリケーションを作成する方法について説明します。 ToDo リストの各タスクは、JSON ドキュメントとして Azure DocumentDB に格納されます。

![My ToDo List Java アプリケーション](./media/documentdb-java-application/image1.png)

> [AZURE.TIP] このアプリケーションの開発のチュートリアルでは、Java を使用した経験があることを前提としています。 Java に慣れていない場合、または [前提条件となるツール](#Prerequisites), 、完全なダウンロードをお勧め [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) GitHub およびビルドを使用するプロジェクトを [この記事の最後にある手順](#GetProject)します。 プロジェクトをビルドした後でこの記事を見直すと、プロジェクトのコンテキストのコードについての洞察を得ることができます。  

##<a id="Prerequisites"></a>この Java Web アプリケーション チュートリアルの前提条件
このアプリケーション開発チュートリアルを読み始める前に、次の項目を用意する必要があります。

- アクティブな Azure アカウント。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト](../../pricing/free-trial/)をご覧ください。
- [Java Development Kit (JDK) 7 以上](http://www.oracle.com/technetwork/java/javase/downloads/index.html)します。
- [Eclipse IDE for Java EE Developers。](http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunasr1)
- [Java ランタイム環境 (Tomcat、Jetty など) が有効な Azure Web サイト。](../web-sites-java-get-started.md)

最初にこれらのツールをインストールする場合は、coreservlets.com のクイック スタート セクションで、インストール プロセスの [チュートリアル: TomCat7 のインストールと Eclipse を使用する](http://www.coreservlets.com/Apache-Tomcat-Tutorial/tomcat-7-with-eclipse.html) 記事です。 

##<a id="CreateDB"></a>手順 1: DocumentDB データベース アカウントを作成する
DocumentDB データベース アカウントを Azure でプロビジョニングするには:

1. データベース アカウントがない場合は作成の説明に従って [データベース アカウントの作成](documentdb-create-account.md)します。 既にアカウントがある場合は、手順 2. に進みます。
2. 使用して、 **キー** ブレードが次の図に示すように、エンドポイントをコピーする **URI** と **主キー** クリップボードと保持する web アプリケーションでこれらの値を使用すると便利です。 これから作成します。

![DocumentDB アカウントの作成 - データベース チュートリアル Screen shot of the Azure Portal, showing a DocumentDB account, with the ACTIVE hub highlighted, the Keys button highlighted on the DocumentDB account blade, and the URI, PRIMARY KEY and SECONDARY KEY values highlighted on the Keys blade][1]


##<a id="CreateJSP"></a>手順 2: Java JSP アプリケーションを作成する

JSP アプリケーションを作成するには: 

1. 最初に、Java プロジェクトを作成します。 Eclipse を起動し、クリックして **ファイル**, をクリックして **新規**, 、順にクリック **[Dynamic Web Project**します。 表示されない場合 **[Dynamic Web Project** 、次の操作を使用可能なプロジェクトとして一覧表示:] をクリックして **ファイル**, 、] をクリックして **新規**, 、] をクリックして **プロジェクト**..., 展開 **Web**, 、] をクリックして **[Dynamic Web Project**, 、] をクリック **次**します。 

    ![JSP Java アプリケーション開発](./media/documentdb-java-application/image10.png)

2. プロジェクト名を入力、 **プロジェクト名** ボックスで、し、[、 **ターゲットのランタイム** ドロップ ダウン メニューは、必要に応じて値 (Apache Tomcat v7.0 など) を選択し、クリックして **完了**します。 ターゲット ランタイムを選択すると、Eclipse でプロジェクトをローカルに実行できます。
3. Eclipse の Project Explorer ビューで、プロジェクトを展開します。 右クリック **WebContent**, をクリックして **新規**, 、順にクリック **JSP ファイル**します。
4.  **New JSP File** ダイアログ ボックスで、[ファイル名 **index.jsp**します。 親フォルダーの保持 **WebContent**, をクリックして次の図で示すように、 **次**します。

    ![新しい JSP ファイルの作成 - Java Web アプリケーションのチュートリアル](./media/documentdb-java-application/image11.png)

5.  **Select JSP Template** ダイアログ ボックスで、このチュートリアルを選択するために **New JSP File (html)**, 、クリックして **完了**します。

6. Index.jsp ファイルが Eclipse で開いたら、表示するテキストを追加 **Hello World!** 既存 <body> 要素。 更新しました。 <body> コンテンツは、次のコードのようになります。
    
        <body>
            <% out.println("Hello World!"); %>
        </body>

8. index.jsp ファイルを保存します。 
9. 手順 2. でターゲットのランタイムを設定する場合は、クリックして **プロジェクト** し **実行** JSP アプリケーションをローカルで実行します。

    ![Hello World – Java アプリケーションのチュートリアル](./media/documentdb-java-application/image12.png)

##<a id="InstallSDK"></a>手順 3: DocumentDB Java SDK をインストールする ##

DocumentDB Java SDK とその依存関係を取得する最も簡単な方法は [Apache Maven](http://maven.apache.org/)します。

そのためには、次の手順に従ってプロジェクトを maven プロジェクトに変換する必要があります。 

1. プロジェクト エクスプ ローラーでプロジェクトを右クリックし、をクリックして **構成**, 、クリックして **Convert to Maven Project**します。
2.  **作成 new POM** ウィンドウで、既定の設定をそのまま使用し] をクリックして **完了**します。
3.  **プロジェクト エクスプ ローラー**, 、pom.xml ファイルを開きます。 
4.  **の依存関係** ] タブの [、 **の依存関係** ] ウィンドウで、をクリックして **追加**します。
4.  **Select Dependency** ウィンドウで、次の操作します。
 -  **GroupId** ボックスで、「com.microsoft.azure」と入力します。
 -  **Artifact Id** ボックス、「azure-documentdb」と入力します。
 -  **バージョン** ボックス 1.4.0」と入力します。

    ![DocumentDB Java アプリケーション SDK をインストールする](./media/documentdb-java-application/image13.png)

    または、テキスト エディターを使用して、GroupId および ArtifactId の依存関係 XML を 直接 pom.xml に追加します。

        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-documentdb</artifactId>
            <version>1.4.0</version>
        </dependency>

5. クリックして **Ok** と Maven によって DocumentDB Java SDK がインストールされます。
6. pom.xml ファイルを保存します。

##<a id="UseService"></a>手順 4: Java アプリケーションで DocumentDB サービスを使用する 

1. まず TodoItem オブジェクトを定義します。

        @Data
        @Builder
        public class TodoItem {
            private String category;
            private boolean complete;
            private String id;
            private String name;
        }

    このプロジェクトで使用している [Project Lombok](http://projectlombok.org/) コンス トラクター、getter、setter、および、ビルダーを生成します。 または、手動でこのコードを記述したり、IDE で自動的に生成もできます。

2. DocumentDB サービスを起動するには、インスタンス化して、新しい **DocumentClient**します。 一般に、再利用する最善の方法は、 **DocumentClient** ではなく後続の要求ごとに新しいクライアントを構築します。 クライアントをラップすることによって、クライアントに再利用可能な **DocumentClientFactory**します。 これでクリップボードに保存した URI とプライマリ キーの値を貼り付ける必要がありますもと、 [手順 1.](#CreateDB)します。 [\_Endpoint\_here] では、URI をプライマリ キーを使用して [値] を置き換えます。

        private static final String HOST = "[YOUR_ENDPOINT_HERE]";
        private static final String MASTER_KEY = "[YOUR_KEY_HERE]";
    
        private static DocumentClient documentClient;
    
        public static DocumentClient getDocumentClient() {
            if (documentClient == null) {
                documentClient = new DocumentClient(HOST, MASTER_KEY,
                        ConnectionPolicy.GetDefault(), ConsistencyLevel.Session);
            }
    
            return documentClient;
        }

3. ここで、持続的な ToDo アイテムを DocumentDB に抽出するためのデータ アクセス オブジェクト (DAO) を作成します。

    ToDo 項目をコレクションに保存するために、クライアントはどのデータベースとコレクションが永続化するかを知っている必要があります (自己リンクによって参照されます)。 一般に、データベースへの追加のラウンドト リップを回避するために、可能な場合は、データベースとコレクションをキャッシュすることをお勧めします。

    次のコードは、データベースとコレクションが存在する場合に取得する方法、または存在しない場合に新規作成する方法を示します。

        public class DocDbDao implements TodoDao {
            // The name of our database.
            private static final String DATABASE_ID = "TodoDB";
        
            // The name of our collection.
            private static final String COLLECTION_ID = "TodoCollection";
        
            // The DocumentDB Client
            private static DocumentClient documentClient = DocumentClientFactory
                    .getDocumentClient();
        
            // Cache for the database object, so we don't have to query for it to
            // retrieve self links.
            private static Database databaseCache;
        
            // Cache for the collection object, so we don't have to query for it to
            // retrieve self links.
            private static DocumentCollection collectionCache;
        
            private Database getTodoDatabase() {
                if (databaseCache == null) {
                    // Get the database if it exists
                    List<Database> databaseList = documentClient
                            .queryDatabases(
                                    "SELECT * FROM root r WHERE r.id='" + DATABASE_ID
                                            + "'", null).getQueryIterable().toList();
        
                    if (databaseList.size() > 0) {
                        // Cache the database object so we won't have to query for it
                        // later to retrieve the selfLink.
                        databaseCache = databaseList.get(0);
                    } else {
                        // Create the database if it doesn't exist.
                        try {
                            Database databaseDefinition = new Database();
                            databaseDefinition.setId(DATABASE_ID);
        
                            databaseCache = documentClient.createDatabase(
                                    databaseDefinition, null).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - the app wasn't
                            // able to query or create the collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
        
                return databaseCache;
            }
        
            private DocumentCollection getTodoCollection() {
                if (collectionCache == null) {
                    // Get the collection if it exists.
                    List<DocumentCollection> collectionList = documentClient
                            .queryCollections(
                                    getTodoDatabase().getSelfLink(),
                                    "SELECT * FROM root r WHERE r.id='" + COLLECTION_ID
                                            + "'", null).getQueryIterable().toList();
        
                    if (collectionList.size() > 0) {
                        // Cache the collection object so we won't have to query for it
                        // later to retrieve the selfLink.
                        collectionCache = collectionList.get(0);
                    } else {
                        // Create the collection if it doesn't exist.
                        try {
                            DocumentCollection collectionDefinition = new DocumentCollection();
                            collectionDefinition.setId(COLLECTION_ID);

                            // Configure the new collection performance tier to S1.
                            RequestOptions requestOptions = new RequestOptions();
                            requestOptions.setOfferType("S1");
        
                            collectionCache = documentClient.createCollection(
                                    getTodoDatabase().getSelfLink(),
                                    collectionDefinition, requestOptions).getResource();
                        } catch (DocumentClientException e) {
                            // TODO: Something has gone terribly wrong - the app wasn't
                            // able to query or create the collection.
                            // Verify your connection, endpoint, and key.
                            e.printStackTrace();
                        }
                    }
                }
        
                return collectionCache;
            }
        }

4. 次の手順では、いくつかのコードを記述して、コレクションに TodoItems を持続的にアクセスさせます。 この例では使用 [Gson](https://code.google.com/p/google-gson/) をシリアル化し、TodoItem Plain Old Java Objects (Pojo) を JSON ドキュメントにシリアル化解除します。 [Jackson](http://jackson.codehaus.org/) または独自のカスタム シリアライザーも Pojo をシリアル化するための優れた代替手段。

        // We'll use Gson for POJO <=> JSON serialization for this example.
        private static Gson gson = new Gson();

        @Override
        public TodoItem createTodoItem(TodoItem todoItem) {
            // Serialize the TodoItem as a JSON Document.
            Document todoItemDocument = new Document(gson.toJson(todoItem));
    
            // Annotate the document as a TodoItem for retrieval (so that we can
            // store multiple entity types in the collection).
            todoItemDocument.set("entityType", "todoItem");
    
            try {
                // Persist the document using the DocumentClient.
                todoItemDocument = documentClient.createDocument(
                        getTodoCollection().getSelfLink(), todoItemDocument, null,
                        false).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
    
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }



5. DocumentDB データベースおよびコレクションのように、ドキュメントも自己リンクにより参照されます。 次のヘルパー関数は、自己リンクではなく別の属性 ("id" など) によりドキュメントを取得します。

        private Document getDocumentById(String id) {
            // Retrieve the document using the DocumentClient.
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.id='" + id + "'", null)
                    .getQueryIterable().toList();
    
            if (documentList.size() > 0) {
                return documentList.get(0);
            } else {
                return null;
            }
        }

6. 手順 5. のヘルパー メソッドを使用して、id により TodoItem の JSON ドキュメントを取得し、POJO に逆シリアル化できます。 

        @Override
        public TodoItem readTodoItem(String id) {
            // Retrieve the document by id using our helper method.
            Document todoItemDocument = getDocumentById(id);
    
            if (todoItemDocument != null) {
                // De-serialize the document in to a TodoItem.
                return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
            } else {
                return null;
            }
        }

7. DocumentClient を使用して、DocumentDB SQL により、TodoItems のコレクションまたはリストを取得することも可能です。
    
        @Override
        public List<TodoItem> readTodoItems() {
            List<TodoItem> todoItems = new ArrayList<TodoItem>();
    
            // Retrieve the TodoItem documents
            List<Document> documentList = documentClient
                    .queryDocuments(getTodoCollection().getSelfLink(),
                            "SELECT * FROM root r WHERE r.entityType = 'todoItem'",
                            null).getQueryIterable().toList();
    
            // De-serialize the documents in to TodoItems.
            for (Document todoItemDocument : documentList) {
                todoItems.add(gson.fromJson(todoItemDocument.toString(),
                        TodoItem.class));
            }
    
            return todoItems;
        }

8. DocumentClient を使用してドキュメントを更新する方法は多数あります。 Todo リスト アプリケーションでは、TodoItem が完了しているかどうかを切り替える機能が求められることがあります。 これは、ドキュメント内の "complete" 属性を更新することで実現できます。
    
        @Override
        public TodoItem updateTodoItem(String id, boolean isComplete) {
            // Retrieve the document from the database
            Document todoItemDocument = getDocumentById(id);
    
            // You can update the document as a JSON document directly.
            // For more complex operations - you could de-serialize the document in
            // to a POJO, update the POJO, and then re-serialize the POJO back in to
            // a document.
            todoItemDocument.set("complete", isComplete);
    
            try {
                // Persist/replace the updated document.
                todoItemDocument = documentClient.replaceDocument(todoItemDocument,
                        null).getResource();
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return null;
            }
    
            return gson.fromJson(todoItemDocument.toString(), TodoItem.class);
        }

9. 最後に、リストから TodoItem を削除する機能が求められる場合があります。 そのためには、前に作成したヘルパー メソッドを使用して、自己リンクを取得し、次にクライアントに削除を指示できます。
    
        @Override
        public boolean deleteTodoItem(String id) {
            // DocumentDB refers to documents by self link rather than id.
    
            // Query for the document to retrieve the self link.
            Document todoItemDocument = getDocumentById(id);
    
            try {
                // Delete the document by self link.
                documentClient.deleteDocument(todoItemDocument.getSelfLink(), null);
            } catch (DocumentClientException e) {
                e.printStackTrace();
                return false;
            }
    
            return true;
        }


##<a id="Wire"></a>手順 5: Java アプリケーション開発プロジェクトの残りの部分を接続する

これで、楽しい作業が終わりました。残っているのは簡単なユーザー インターフェイスの作成と DAO への連結です。

1. まず、DAO を呼び出すためのコントローラーの作成から始めましょう。

        public class TodoItemController {
            public static TodoItemController getInstance() {
                if (todoItemController == null) {
                    todoItemController = new TodoItemController(TodoDaoFactory.getDao());
                }
                return todoItemController;
            }
        
            private static TodoItemController todoItemController;
        
            private final TodoDao todoDao;
        
            TodoItemController(TodoDao todoDao) {
                this.todoDao = todoDao;
            }
        
            public TodoItem createTodoItem(@NonNull String name,
                    @NonNull String category, boolean isComplete) {
                TodoItem todoItem = TodoItem.builder().name(name).category(category)
                        .complete(isComplete).build();
                return todoDao.createTodoItem(todoItem);
            }
        
            public boolean deleteTodoItem(@NonNull String id) {
                return todoDao.deleteTodoItem(id);
            }
        
            public TodoItem getTodoItemById(@NonNull String id) {
                return todoDao.readTodoItem(id);
            }
        
            public List<TodoItem> getTodoItems() {
                return todoDao.readTodoItems();
            }
        
            public TodoItem updateTodoItem(@NonNull String id, boolean isComplete) {
                return todoDao.updateTodoItem(id, isComplete);
            }
        }

    より複雑なアプリケーションでは、コントローラーは、DAO の上に複雑なビジネス ロジックを格納することがあります。

2. 次に、サーブレットを作成して HTTP 要求をコントローラーにルーティングします。

        public class TodoServlet extends HttpServlet {
            // API Keys
            public static final String API_METHOD = "method";
        
            // API Methods
            public static final String CREATE_TODO_ITEM = "createTodoItem";
            public static final String GET_TODO_ITEMS = "getTodoItems";
            public static final String UPDATE_TODO_ITEM = "updateTodoItem";
        
            // API Parameters
            public static final String TODO_ITEM_ID = "todoItemId";
            public static final String TODO_ITEM_NAME = "todoItemName";
            public static final String TODO_ITEM_CATEGORY = "todoItemCategory";
            public static final String TODO_ITEM_COMPLETE = "todoItemComplete";
        
            public static final String MESSAGE_ERROR_INVALID_METHOD = "{'error': 'Invalid method'}";
        
            private static final long serialVersionUID = 1L;
            private static final Gson gson = new Gson();
        
            @Override
            protected void doGet(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
        
                String apiResponse = MESSAGE_ERROR_INVALID_METHOD;
        
                TodoItemController todoItemController = TodoItemController
                        .getInstance();
        
                String id = request.getParameter(TODO_ITEM_ID);
                String name = request.getParameter(TODO_ITEM_NAME);
                String category = request.getParameter(TODO_ITEM_CATEGORY);
                boolean isComplete = StringUtils.equalsIgnoreCase("true",
                        request.getParameter(TODO_ITEM_COMPLETE)) ? true : false;
        
                switch (request.getParameter(API_METHOD)) {
                case CREATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.createTodoItem(name,
                            category, isComplete));
                    break;
                case GET_TODO_ITEMS:
                    apiResponse = gson.toJson(todoItemController.getTodoItems());
                    break;
                case UPDATE_TODO_ITEM:
                    apiResponse = gson.toJson(todoItemController.updateTodoItem(id,
                            isComplete));
                    break;
                default:
                    break;
                }
        
                response.getWriter().println(apiResponse);
            }
    
            @Override
            protected void doPost(HttpServletRequest request,
                    HttpServletResponse response) throws ServletException, IOException {
                doGet(request, response);
            }
        }

3. ユーザーに表示する Web ユーザー インターフェイスが必要になります。 前に作成した index.jsp を書き換えてみましょう。

        <html>
        <head>
          <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
          <meta http-equiv="X-UA-Compatible" content="IE=edge;" />
          <title>Azure DocumentDB Java Sample</title>
        
          <!-- Bootstrap -->
          <link href="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css" rel="stylesheet">
        
          <style>
            /* Add padding to body for fixed nav bar */
            body {
              padding-top: 50px;
            }
          </style>
        </head>
        <body>
          <!-- Nav Bar -->
          <div class="navbar navbar-inverse navbar-fixed-top" role="navigation">
            <div class="container">
              <div class="navbar-header">
                <a class="navbar-brand" href="#">My Tasks</a>
              </div>
            </div>
          </div>
        
          <!-- Body -->
          <div class="container">
            <h1>My ToDo List</h1>
        
            <hr/>
        
            <!-- The ToDo List -->
            <div class = "todoList">
              <table class="table table-bordered table-striped" id="todoItems">
                <thead>
                  <tr>
                    <th>Name</th>
                    <th>Category</th>
                    <th>Complete</th>
                  </tr>
                </thead>
                <tbody>
                </tbody>
              </table>
        
              <!-- Update Button -->
              <div class="todoUpdatePanel">
                <form class="form-horizontal" role="form">
                  <button type="button" class="btn btn-primary">Update Tasks</button>
                </form>
              </div>
        
            </div>
        
            <hr/>
        
            <!-- Item Input Form -->
            <div class="todoForm">
              <form class="form-horizontal" role="form">
                <div class="form-group">
                  <label for="inputItemName" class="col-sm-2">Task Name</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemName" placeholder="Enter name">
                  </div>
                </div>
        
                <div class="form-group">
                  <label for="inputItemCategory" class="col-sm-2">Task Category</label>
                  <div class="col-sm-10">
                    <input type="text" class="form-control" id="inputItemCategory" placeholder="Enter category">
                  </div>
                </div>
        
                <button type="button" class="btn btn-primary">Add Task</button>
              </form>
            </div>
        
          </div>
        
          <!-- Placed at the end of the document so the pages load faster -->
          <script src="//ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js"></script>
          <script src="//ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js"></script>
          <script src="assets/todo.js"></script>
        </body>
        </html>

4. 最後に、Web ユーザー インターフェイスとサーブレットを結びつけるためにクライアント側の Javascript を記述します。
    
        var todoApp = {
          /*
           * API methods to call Java backend.
           */
          apiEndpoint: "api",
        
          createTodoItem: function(name, category, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "createTodoItem",
                "todoItemName": name,
                "todoItemCategory": category,
                "todoItemComplete": isComplete
              },
              function(data) {
                var todoItem = data;
                todoApp.addTodoItemToTable(todoItem.id, todoItem.name, todoItem.category, todoItem.complete);
              },
              "json");
          },
        
          getTodoItems: function() {
            $.post(todoApp.apiEndpoint, {
                "method": "getTodoItems"
              },
              function(data) {
                var todoItemArr = data;
                $.each(todoItemArr, function(index, value) {
                  todoApp.addTodoItemToTable(value.id, value.name, value.category, value.complete);
                });
              },
              "json");
          },
        
          updateTodoItem: function(id, isComplete) {
            $.post(todoApp.apiEndpoint, {
                "method": "updateTodoItem",
                "todoItemId": id,
                "todoItemComplete": isComplete
              },
              function(data) {},
              "json");
          },
        
          /*
           * UI Methods
           */
          addTodoItemToTable: function(id, name, category, isComplete) {
            var rowColor = isComplete ? "active" : "warning";
        
            todoApp.ui_table().append($("<tr>")
              .append($("<td>").text(name))
              .append($("<td>").text(category))
              .append($("<td>")
                .append($("<input>")
                  .attr("type", "checkbox")
                  .attr("id", id)
                  .attr("checked", isComplete)
                  .attr("class", "isComplete")
                ))
              .addClass(rowColor)
            );
          },
        
          /*
           * UI Bindings
           */
          bindCreateButton: function() {
            todoApp.ui_createButton().click(function() {
              todoApp.createTodoItem(todoApp.ui_createNameInput().val(), todoApp.ui_createCategoryInput().val(), false);
              todoApp.ui_createNameInput().val("");
              todoApp.ui_createCategoryInput().val("");
            });
          },
        
          bindUpdateButton: function() {
            todoApp.ui_updateButton().click(function() {
              // Disable button temporarily.
              var myButton = $(this);
              var originalText = myButton.text();
              $(this).text("Updating...");
              $(this).prop("disabled", true);
        
              // Call api to update todo items.
              $.each(todoApp.ui_updateId(), function(index, value) {
                todoApp.updateTodoItem(value.name, value.value);
                $(value).remove();
              });
        
              // Re-enable button.
              setTimeout(function() {
                myButton.prop("disabled", false);
                myButton.text(originalText);
              }, 500);
            });
          },
        
          bindUpdateCheckboxes: function() {
            todoApp.ui_table().on("click", ".isComplete", function(event) {
              var checkboxElement = $(event.currentTarget);
              var rowElement = $(event.currentTarget).parents('tr');
              var id = checkboxElement.attr('id');
              var isComplete = checkboxElement.is(':checked');
        
              // Toggle table row color
              if (isComplete) {
                rowElement.addClass("active");
                rowElement.removeClass("warning");
              } else {
                rowElement.removeClass("active");
                rowElement.addClass("warning");
              }
        
              // Update hidden inputs for update panel.
              todoApp.ui_updateForm().children("input[name='" + id + "']").remove();
        
              todoApp.ui_updateForm().append($("<input>")
                .attr("type", "hidden")
                .attr("class", "updateComplete")
                .attr("name", id)
                .attr("value", isComplete));
        
            });
          },
        
          /*
           * UI Elements
           */
          ui_createNameInput: function() {
            return $(".todoForm #inputItemName");
          },
        
          ui_createCategoryInput: function() {
            return $(".todoForm #inputItemCategory");
          },
        
          ui_createButton: function() {
            return $(".todoForm button");
          },
        
          ui_table: function() {
            return $(".todoList table tbody");
          },
        
          ui_updateButton: function() {
            return $(".todoUpdatePanel button");
          },
        
          ui_updateForm: function() {
            return $(".todoUpdatePanel form");
          },
        
          ui_updateId: function() {
            return $(".todoUpdatePanel .updateComplete");
          },
        
          /*
           * Install the TodoApp
           */
          install: function() {
            todoApp.bindCreateButton();
            todoApp.bindUpdateButton();
            todoApp.bindUpdateCheckboxes();
        
            todoApp.getTodoItems();
          }
        };
        
        $(document).ready(function() {
          todoApp.install();
        });

5. これで完成です。 後はアプリケーションをテストするだけです。 アプリケーションをローカルで実行し、入力項目の名前とカテゴリをクリックしていくつかの Todo 項目を追加 **タスクの追加**します。

6. チェック ボックスを切り替え] をクリックして完了するかどうかを更新することができます、項目が表示されたら、 **Update Tasks**します。

##<a id="Deploy"></a>手順 6: Azure Websites に Java アプリケーションをデプロイする

Azure Websites での Java アプリケーションのデプロイは簡単です。アプリケーションを WAR ファイルとしてエクスポートし、ソース管理 (例: GIT) または FTP のいずれかを使用してアップロードするだけです。

1. プロジェクトを右クリックし、アプリケーションを WAR としてエクスポートする **プロジェクト エクスプ ローラー**, をクリックして **エクスポート**, 、順にクリック **WAR ファイル**します。 
2.  **WAR Export** ウィンドウで、次の操作します。
 - [Web project] ボックスに、「azure-documentdb-java-sample」と入力します。
 - [Destination] ボックスでエクスポート先を選択し、WAR ファイルを保存します。
 - クリックして **完了**します。

3. WAR ファイルを手にした、単純にアップロードできますを Azure web サイトの **webapps** ディレクトリ。 ファイルをアップロードする方法については、次を参照してください。 [Azure の Java web サイトにアプリケーションの追加](../web-sites-java-add-app.md)します。

    WAR ファイルを webapps ディレクトリにアップロードすると、ランタイム環境により WAR ファイルの追加が検出され、WAR ファイルが自動的に読み込まれます。
4. 完成したアプリケーションの動作を確認するには、http://YOUR\_SITE\_NAME.azurewebsites.net/azure-documentdb-java-sample/ にアクセスして、タスクを追加します。

##<a id="GetProject"></a>GitHub からのプロジェクトの入手

このチュートリアルのすべてのサンプルが含まれている、 [todo](https://github.com/Azure-Samples/documentdb-java-todo-app) GitHub のプロジェクトです。 Todo プロジェクトを Eclipse にインポートと維持するため、ソフトウェアがあるリソースに一覧表示、 [の前提条件](#Prerequisites) セクション、次の操作を実行します。

1. インストール [Lombok をプロジェクト](http://projectlombok.org/)します。 Lombok は、プロジェクトのコンストラクター、getter、setter の生成に使用します。 lombok.jar ファイルをダウンロードしたら、ファイルをダブルクリックしてコマンド ラインからインストールします。 
2. Eclipse が開いている場合は、いったん終了してから再起動して Lombok を読み込みます。
3. Eclipse で、上、 **ファイル** ] メニューのをクリックして **インポート**します。
4.  **インポート** ウィンドウで、をクリックして **Git**, 、] をクリックして **Projects from Git**, 、順にクリック **次**します。 
5.  **Select Repository Source** 画面で、[ **Clone URI**します。
6.  **Source Git Repository** 画面の **URI** ボックス、https://github.com/Azure-Samples/documentdb-java-todo-app.git を入力し、クリックして **次**します。
7.  **Branch Selection** 画面であることを確認 **マスター** クリックして選択すると、 **次**します。
8.  **ローカルの格納先** 画面で、[ **参照** リポジトリがコピーでき、をクリックし、フォルダーを選択する **次**します。
9.  **のプロジェクトのインポートに使用するウィザード] を選択** 画面であることを確認 **既存のプロジェクトをインポート** クリックして選択すると、 **次**します。
10.  **Import Projects** 画面で、選択を解除、 **DocumentDB** [プロジェクト] をクリックして **完了**します。 DocumentDB プロジェクトには、依存関係として追加される DocumentDB Java SDK が含まれています。
11.  **プロジェクト エクスプ ローラー**, azure-documentdb-java-sample\src\com.microsoft.azure.documentdb.sample.dao\DocumentClientFactory.java し HOST と MASTER_KEY の値を URI と、DocumentDB アカウントの主キーに置き換え、ファイルを保存します。 詳細については、「[手順 1:DocumentDB データベース アカウントを作成する](#CreateDB)」を参照してください。
12.  **プロジェクト エクスプ ローラー**, を右クリックして、 **azure documentdb java サンプル**, 、] をクリックして **Build Path**, 、クリックして **Configure Build Path**します。
13.  **Java Build Path** ] 画面で、右側のウィンドウで、選択、 **ライブラリ** タブをクリックし、をクリックし、 **Add External JARs**します。 Lombok.jar ファイルの場所に移動し、をクリックして **開く**, 、] をクリックし、 **OK**します。
14. 使用する手順を開くには 12、 **プロパティ** ウィンドウをもう一度、し、左側のウィンドウでをクリックして **Targeted Runtimes**します。
15.  **Targeted Runtimes** 画面で、[ **新規**, [ **Apache Tomcat v7.0**, 、] をクリックし、 **[ok]**します。
16. 使用する手順を開くには 12、 **プロパティ** ウィンドウをもう一度、し、左側のウィンドウでをクリックして **Project Facets**します。
17.  **Project Facets** 画面で、[ **Dynamic Web Module** と **Java**, 、順にクリック **OK**します。
18.  **サーバー** を右クリックし、画面の下部にあるタブで、 **Tomcat v7.0 Server at localhost** ] をクリックし、 **を追加および削除**します。
19.  **を追加および削除** ウィンドウで、移動 **azure documentdb java サンプル** に、 **構成済み** ボックスをクリックして **完了**します。 
20.  **サーバー** ] タブを右クリックして **Tomcat v7.0 Server at localhost**, 、] をクリックし、 **再起動**します。
21. ブラウザーで http://localhost:8080/azure-documentdb-java-sample/ にアクセスして、タスク リストに項目を追加します。 既定のポート値を変更している場合は、8080 に代えて、使用している値を指定してください。
22. プロジェクトを Azure Web サイトにデプロイする方法については、「[手順 6: Azure Websites にアプリケーションをデプロイする](#Deploy)」を参照してください。 

[1]: media/documentdb-java-application/keys.png
 


