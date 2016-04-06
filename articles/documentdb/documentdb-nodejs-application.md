<properties 
    pageTitle="Node.js について学習する - DocumentDB Node.js チュートリアル | Microsoft Azure" 
    description="Node.js について学習する このチュートリアルでは、Microsoft Azure DocumentDB を使用して、Azure Websites にホストされた Node.js Express Web アプリケーションからデータを格納する方法やデータにアクセスする方法について説明します。" 
    keywords="Application development, database tutorial, learn node.js, node.js tutorial, documentdb, azure, Microsoft azure"
    services="documentdb" 
    documentationCenter="nodejs" 
    authors="ryancrawcour" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="hero-article" 
    ms.date="10/20/2015" 
    ms.author="ryancraw"/>

# <a name="_Toc395783175"></a>DocumentDB を使用した Node.js Web アプリケーションの作成

> [AZURE.SELECTOR]
- [.NET](documentdb-dotnet-application.md)
- [Node.js](documentdb-nodejs-application.md)
- [Java](documentdb-java-application.md)
- [Python](documentdb-python-application.md)

この Node.js チュートリアルでは、Azure DocumentDB サービスを使用して、Azure Websites にホストされた Node.js Express アプリケーションからデータを格納する方法やデータにアクセスする方法について説明します。

まずは、次のビデオを視聴することをお勧めします。このビデオでは、Azure DocumentDB データベース アカウントをプロビジョニングし、JSON ドキュメントを Node.js アプリケーションに格納する方法を学習できます。 

> [AZURE.VIDEO azure-demo-getting-started-with-azure-documentdb-on-nodejs-in-linux]

その後でこのs Node.js チュートリアルに戻ると、次の質問の答えを見つけることができます。

- Documentdb npm モジュールを使って DocumentDB を操作する方法
- Web アプリケーションを Azure Websites にデプロイする方法

このデータベースのチュートリアルでは、単純な web ベースを作成します
完了する機能を備えた、単純な Web ベースのタスク管理アプリケーションを
作成します。 対象となるタスクは、JSON ドキュメントとして Azure
DocumentDB に保存するものとします。

![この Node.js チュートリアルで作成した、My Todo List アプリケーションのスクリーン ショット](./media/documentdb-nodejs-application/image1.png)

チュートリアルを最後まで実施する時間はないが、完成済みのソリューションが必要な場合は、 問題ではなく、完全なサンプル ソリューションから取得できます [GitHub][]します。

## <a name="_Toc395783176"></a>前提条件

> [AZURE.TIP] この Node.js チュートリアルでは、Node.js と Azure Websites を使用したいくつかの経験があることを前提としています。

この記事の手順を実行する前に、次のソフトウェアがインストール
されていることを確認してください。

- アクティブな Azure アカウント アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、[Azure の無料試用版サイト](../../pricing/free-trial/)をご覧ください。
- [Node.js][] バージョン v0.10.29 以降。
- [Express ジェネレーター](http://www.expressjs.com/starter/generator.html) (によってこれをインストールできる `npm install express-generator -g`)
- [Git][]します。

## <a name="_Toc395637761"></a>手順 1: DocumentDB データベース アカウントを作成する

最初に、DocumentDB アカウントを作成します。 既にアカウントがある場合にスキップできます [手順 2: 新しい Node.js アプリケーションの作成](#_Toc395783178)します。

[AZURE.INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

[AZURE.INCLUDE [documentdb-keys](../../includes/documentdb-keys.md)]

## <a name="_Toc395783178"></a>手順 2: 新しい Node.js アプリケーションを作成する

これで、基本的な Hello World Node.js プロジェクトを使用して作成する方法を学習しましょう、 [Express](http://expressjs.com/) フレームワークです。

1. お使いのターミナルを開きます。

2. Express ジェネレーターを使用するという新しいアプリケーションを生成する **todo**します。

        express todo

3. 開く、新しい **todo** 依存関係のディレクトリをインストールします。

        cd todo
        npm install

4. 新しいアプリケーションを実行します。

        npm start

5. ブラウザーに移動して、新しいアプリケーションを表示 [http://localhost:3000](http://localhost:3000)します。

    ![Node.js について学習する - ブラウザー ウィンドウでの Hello World アプリケーションのスクリーン ショット](./media/documentdb-nodejs-application/image12.png)

## <a name="_Toc395783179"></a>手順 3: 追加モジュールのインストール

 **Package.json** ファイルは 1 つのルートに作成されるファイルの
プロジェクトのルートに作成されるファイルの 1 つです。 このファイルには、Express アプリケーションで必要な追加モジュールのリストが
含まれます。 このファイルは、後でこのアプリケーションを Azure の Web サイトにデプロイするときに、
アプリケーションのサポートのために Azure にインストールする必要があるモジュールを
判断するために使用されます。 このチュートリアルでは、追加で 2 つのパッケージをインストールする必要があります。

1. インストール、ターミナルに戻り、 **async** npm でモジュールです。

        npm install async --save

1. インストール、 **documentdb** npm でモジュールです。 このモジュールで、DocumentDB のすべての操作を行います。

        npm install documentdb --save

3. 簡単に確認、 **package.json** 追加モジュールをアプリケーションのファイルが表示されます。 このファイルは、アプリケーションの実行時にどのパッケージをダウンロードし、インストールする必要があるかを Azure に伝えます。 表示される内容は以下の例のようになります。

    ![package.json タブのスクリーン ショット](./media/documentdb-nodejs-application/image17.png)

       This tells Node (and Azure later) that your application depends on these additional modules.

## <a name="_Toc395783180"></a>手順 4: ノード アプリケーションでの DocumentDB サービスの使用

最初の設定と構成を行った後は、いよいよ Azure DocumentDB を使ってコードを作成する作業に入ります。

### モデルの作成

1. プロジェクト ディレクトリ内には、という名前の新しいディレクトリを作成 **モデル**します。
2.  **モデル** ディレクトリという名前の新しいファイルを作成する **taskDao.js**します。 このファイルには、アプリケーションで作成されるタスクのモデルが格納されます。
3. 同じ **モデル** ディレクトリという別の新しいファイルを作成する **docdbUtils.js**します。 このファイルには、アプリケーション全体で使用する、いくつかの便利で再利用可能なコードが含まれます。 
4. 次のコードをコピー **docdbUtils.js**

        var DocumentDBClient = require('documentdb').DocumentClient;
            
        var DocDBUtils = {
            getOrCreateDatabase: function (client, databaseId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id=@id',
                    parameters: [{
                        name: '@id',
                        value: databaseId
                    }]
                };
        
                client.queryDatabases(querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
        
                    } else {
                        if (results.length === 0) {
                            var databaseSpec = {
                                id: databaseId
                            };
        
                            client.createDatabase(databaseSpec, function (err, created) {
                                callback(null, created);
                            });
        
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            },
        
            getOrCreateCollection: function (client, databaseLink, collectionId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id=@id',
                    parameters: [{
                        name: '@id',
                        value: collectionId
                    }]
                };             
                
                client.queryCollections(databaseLink, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
        
                    } else {        
                        if (results.length === 0) {
                            var collectionSpec = {
                                id: collectionId
                            };
                            
                            var requestOptions = {
                                offerType: 'S1'
                            };
                            
                            client.createCollection(databaseLink, collectionSpec, requestOptions, function (err, created) {
                                callback(null, created);
                            });
        
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            }
        };
                
        module.exports = DocDBUtils;

> [AZURE.TIP] createCollection では、コレクションのプランの種類を指定するために使用する省略可能な requestOptions パラメーターを受け取ります。 requestOptions.offerType の値を指定しないと、コレクションは既定のプランの種類を使用して作成されます。
> DocumentDB のプランの種類の詳細についてを参照してください [DocumentDB のパフォーマンス レベル](documentdb-performance-levels.md) 
        
3. 保存して閉じる、 **docdbUtils.js** ファイルです。

4. 先頭に、 **taskDao.js** ファイルに参照する次のコードを追加、 **DocumentDBClient** と **docdbUtils.js** 上記で作成しました。

        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');

4. 次に、Task オブジェクトを定義およびエクスポートするコードを追加します。 これにより、Task オブジェクトの初期化と、使用するデータベースおよびドキュメント コレクションのセット アップ.が実行されます。

        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
        
          this.database = null;
          this.collection = null;
        }
        
        module.exports = TaskDao;

5. 次に、Task オブジェクトの追加のメソッドを定義する次のコードを追加します。このメソッドによって、DocumentDB に格納されたデータを操作できます。

        TaskDao.prototype = {
            init: function (callback) {
                var self = this;
        
                docdbUtils.getOrCreateDatabase(self.client, self.databaseId, function (err, db) {
                    if (err) {
                        callback(err);

                    } else {
                        self.database = db;
                        docdbUtils.getOrCreateCollection(self.client, self.database._self, self.collectionId, function (err, coll) {
                            if (err) {
                                callback(err);
        
                            } else {
                                self.collection = coll;
                            }
                        });
                    }
                });
            },
        
            find: function (querySpec, callback) {
                var self = this;
        
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
        
                    } else {
                        callback(null, results);
                    }
                });
            },
        
            addItem: function (item, callback) {
                var self = this;
        
                item.date = Date.now();
                item.completed = false;
        
                self.client.createDocument(self.collection._self, item, function (err, doc) {
                    if (err) {
                        callback(err);
        
                    } else {
                        callback(null, doc);
                    }
                });
            },
        
            updateItem: function (itemId, callback) {
                var self = this;
        
                self.getItem(itemId, function (err, doc) {
                    if (err) {
                        callback(err);
        
                    } else {
                        doc.completed = true;
        
                        self.client.replaceDocument(doc._self, doc, function (err, replaced) {
                            if (err) {
                                callback(err);
        
                            } else {
                                callback(null, replaced);
                            }
                        });
                    }
                });
            },
        
            getItem: function (itemId, callback) {
                var self = this;
        
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id=@id',
                    parameters: [{
                        name: '@id',
                        value: itemId
                    }]
                };
        
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
        
                    } else {
                        callback(null, results[0]);
                    }
                });
            }
        };

6. 保存して閉じる、 **taskDao.js** ファイルです。 

### コントローラーの作成

1.  **ルート** という名前の新しいファイルを作成する、プロジェクトのディレクトリ **tasklist.js**します。 
2. 次のコードを追加 **tasklist.js**します。 使用される DocumentDBClient および async モジュールが読み込まれます **tasklist.js**します。 これも定義されている、 **TaskList** のインスタンスに渡される関数、 **タスク** 先ほど定義したオブジェクトします。

        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
        
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
        
        module.exports = TaskList;

3. 追加を続行、 **tasklist.js** ファイルに使用するメソッドを追加することで **showTasks、addTask**, 、および **completeTasks**:
        
        TaskList.prototype = {
            showTasks: function (req, res) {
                var self = this;
        
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.completed=@completed',
                    parameters: [{
                        name: '@completed',
                        value: false
                    }]
                };
        
                self.taskDao.find(querySpec, function (err, items) {
                    if (err) {
                        throw (err);
                    }
        
                    res.render('index', {
                        title: 'My ToDo List ',
                        tasks: items
                    });
                });
            },
        
            addTask: function (req, res) {
                var self = this;
                var item = req.body;
        
                self.taskDao.addItem(item, function (err) {
                    if (err) {
                        throw (err);
                    }
        
                    res.redirect('/');
                });
            },
        
            completeTask: function (req, res) {
                var self = this;
                var completedTasks = Object.keys(req.body);
        
                async.forEach(completedTasks, function taskIterator(completedTask, callback) {
                    self.taskDao.updateItem(completedTask, function (err) {
                        if (err) {
                            callback(err);
                        } else {
                            callback(null);
                        }
                    });
                }, function goHome(err) {
                    if (err) {
                        throw err;
                    } else {
                        res.redirect('/');
                    }
                });
            }
        };


4. 保存して閉じる、 **tasklist.js** ファイルです。
 
### config.js の追加

1. という名前の新しいファイルを作成、プロジェクト ディレクトリに **config.js**します。
2. 次のコードを追加 **config.js**します。 これにより、アプリケーションに必要な値と構成設定が定義されます。

        var config = {}
        
        config.host = process.env.HOST || "[the URI value from the DocumentDB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[the PRIMARY KEY value from the DocumentDB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
        
        module.exports = config;

3.  **Config.js** ファイルでの HOST および AUTH_KEY の DocumentDB アカウントのキー] ブレードで、値を使用して値を更新、 [Microsoft Azure ポータル](http://portal.azure.com):

4. 保存して閉じる、 **config.js** ファイルです。
 
### app.js の変更

1. プロジェクト ディレクトリで開き、 **app.js** ファイルです。 これは、先ほどの Express Web アプリケーション作成時に作成されたファイルです。
2. 先頭に次のコードを追加 **app.js**
    
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');

3. このコードは、使用する構成ファイルを定義し、さらにこのファイルから値を読み取り、それをこれから使用するいくつかの変数に代入します。
4. 次の 2 行を置き換える **app.js** ファイル。

        app.use('/', routes);
        app.use('/users', users); 

      次のコードに置き換えます。

        var docDbClient = new DocumentDBClient(config.host, {
            masterKey: config.authKey
        });
        var taskDao = new TaskDao(docDbClient, config.databaseId, config.collectionId);
        var taskList = new TaskList(taskDao);
        taskDao.init();
        
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));


6. これらの行の新しいインスタンスを定義する、 **TaskDao** DocumentDB への新しい接続のオブジェクト (から読み取った値を使用して、 **config.js**)、タスク オブジェクトを初期化しメソッドへのフォーム アクションをバインド、 **TaskList** コント ローラーです。 

7. 最後に、保存して閉じる、 **app.js** ファイルはほぼ完了です。
 
## <a name="_Toc395783181"></a>手順 5: ユーザー インターフェイスの構築

次に、ユーザーがアプリケーションとやり取りするためのユーザー インターフェイスを作成します。 使用して作成した Express アプリケーション **Jade** ビュー エンジンとして。 Jade の詳細についてを参照してください [http://jade-lang.com/](http://jade-lang.com/)します。

1.  **Layout.jade** ファイルで、 **ビュー** ディレクトリがその他のグローバル テンプレートとして使用される **.jade** ファイルです。 この手順で使用するように変更が [Twitter Bootstrap](https://github.com/twbs/bootstrap), 、見栄えの良い web サイトをデザインしやすくツールキットです。 
2. 開いている、 **layout.jade** ファイルは、 **ビュー** フォルダー内容を次に置き換えます
    
        doctype html
        html
          head
            title= title
            link(rel='stylesheet', href='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css')
            link(rel='stylesheet', href='/stylesheets/style.css')
          body
            nav.navbar.navbar-inverse.navbar-fixed-top
              div.navbar-header
                a.navbar-brand(href='#') My Tasks
            block content
            script(src='//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js')
            script(src='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js')



    これが実質的には、 **Jade** エンジンに、このアプリケーションに HTML を描画し、作成、 **ブロック** と呼ばれる **コンテンツ** 、コンテンツ ページのレイアウトを指定しています。
    保存して閉じます **layout.jade** ファイルです。

4. 開き、 **index.jade** ファイル、ビューには、アプリケーションで使用され、次のファイルの内容に置き換えます。

        extends layout
        
        block content
          h1 #{title}
          br
        
          form(action="/completetask", method="post")
            table.table.table-striped.table-bordered
              tr
                td Name
                td Category
                td Date
                td Complete
              if (typeof tasks === "undefined")
                tr
                  td
              else
                each task in tasks
                  tr
                    td #{task.name}
                    td #{task.category}
                    - var date  = new Date(task.date);
                    - var day   = date.getDate();
                    - var month = date.getMonth() + 1;
                    - var year  = date.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name:
            input(name="name", type="textbox")
            label Item Category:
            input(name="category", type="textbox")
            br
            button.btn(type="submit") Add item

    これはレイアウトを拡張のコンテンツを提供、 **コンテンツ** でプレース ホルダー、 **layout.jade** ファイルの前の手順です。
    
    このレイアウトでは、2 つの HTML フォームを作成しています。 
    最初のフォームには、データをポストによって項目を更新できるボタン用のテーブルが含まれています。 **/completetask** コント ローラーのメソッドです。
    2 番目の形式には、2 つの入力フィールドとポストによって新しい項目を作成するためのボタンが含まれています。 **/addtask** コント ローラーのメソッドです。
    
    アプリケーションが動作するために必要なコードはこれですべてです。

5. 開いている、 **style.css** ファイル **public \stylesheets** ディレクトリと、次のコードをコードに置き換えます。

        body {
          padding: 50px;
          font: 14px "Lucida Grande", Helvetica, Arial, sans-serif;
        }
        a {
          color: #00B7FF;
        }
        .well label {
          display: block;
        }
        .well input {
          margin-bottom: 5px;
        }
        .btn {
          margin-top: 5px;
          border: outset 1px #C8C8C8;
        }

    保存して閉じます **style.css** ファイルです。

## <a name="_Toc395783181"></a>手順 6: ローカルでのアプリケーションの実行

1. ローカル コンピューターでアプリケーションをテストするには、ターミナルで `npm start` を実行します。アプリケーションが開始し、ブラウザーが起動して次の画像のようなページが開きます。

    ![ブラウザー ウィンドウでの MyTodo List アプリケーションのスクリーン ショット](./media/documentdb-nodejs-application/image18.png)


2. 表示された [Item]、[Item Name]、[Category] のフィールドを使用して情報を入力し、
クリックしてについては、 **項目の追加**します。

3. ページが更新され、ToDo リストに新しく作成された項目が
表示されます。

    ![ToDo リストに新しい項目があるアプリケーションのスクリーン ショット](./media/documentdb-nodejs-application/image19.png)

4. タスクを完了するには、[Complete] 列のチェック ボックスをオンにし、
クリックして **タスクを更新する**です。

## <a name="_Toc395783182"></a>手順 7: Azure Websites へのアプリケーション開発プロジェクトのデプロイ

1. まだデプロイを実施していない場合、Azure Web サイトの Git リポジトリを有効にします。 これを行う方法の手順を参照して [ここ](../web-sites-publish-source-control-git.md#step4)します。

2. Git リモートとして Azure Web サイトを追加します。

        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git

3. リモートにプッシュすることでデプロイを実施します。

        git push azure master

4. 数秒すると、Web アプリケーションの発行が完了し、ブラウザーが起動します。
作成したアプリケーションが Azure で実行されているようすが
確認できます。

## <a name="_Toc395637775"></a>次のステップ

ご利用ありがとうございます。 これで、Azure DocumentDB を使用した最初の Node.js Express Web アプリケーションの
作成と、Azure Websites への発行が完了しました。

完全な参照アプリケーションのソース コードはからダウンロードできます [GitHub][]します。

詳細については、次を参照してください。、 [Node.js デベロッパー センター](/develop/nodejs/)します。

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[Github]: https://github.com/Azure-Samples/documentdb-node-todo-app
 


