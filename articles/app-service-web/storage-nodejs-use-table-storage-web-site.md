<properties
    pageTitle="Azure Table サービスを使用する Node.js Web アプリ"
    description="Azure Table サービスを使用して、Azure App Service Web Apps でホストされる Node.js アプリケーションのデータを格納する方法を説明するチュートリアル。"
    tags="azure-portal"
    services="app-service\web, storage"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="11/20/2015"
    ms.author="robmcm"/>



# Azure Table サービスを使用する Node.js Web アプリ

## 概要

このチュートリアルには、Azure データ管理で提供される Table サービスを使用して、保存してからデータにアクセスする方法がでは、 [node] アプリケーションでホスト [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web アプリです。 このチュートリアルでは、ノードを使用したいくつかの経験があると想定し、 [Git]します。

学習内容:

* npm (ノード パッケージ マネージャー) を使用してノード モジュールをインストールする方法

* Azure Table サービスを使用する方法

* Azure CLI を使用して Web アプリを作成する方法

このチュートリアルでは、タスクを作成、取得、完了する機能を備えた、単純な Web ベースの "To DO リスト" アプリケーションを作成します。 タスクは Table サービスに格納されます。

完成したアプリケーションを次に示します。

![空のタスク一覧が表示されている Web ページ][node-table-finished]

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。


## 前提条件

この記事の手順を実行する前に、次のソフトウェアがインストールされていることを確認してください。

* [node] version 0.10.24 以上

* [Git]


[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## ストレージ アカウントの作成

Azure ストレージ アカウントを作成します。 アプリでは、このアカウントを使用して To Do リストの項目を保存します。

1.  ログイン、 [Azure ポータル](https://portal.azure.com)します。

2. クリックして、 **新規** ポータルの下部にあるアイコンを左クリックし、 **データ + ストレージ** > **ストレージ**します。 ストレージ アカウントに一意の名前を付けるされ、新しい作成 [リソース グループ](../resource-group-overview.md) にします。

    ![New ボタン](./media/storage-nodejs-use-table-storage-web-site/configure-storage.png)

    ストレージ アカウントが作成されると、 **通知** ボタンが緑色の点滅 **成功** し、ストレージ アカウントのブレードが開いて、作成した新しいリソース グループに属していることを表示します。

5. ストレージ アカウントのブレードで、[ **設定** > **キー**します。 プライマリ アクセス キーをクリップボードにコピーします。

    ![アクセス キー][portal-storage-access-keys]


##モジュールのインストールとスキャフォールディングの生成

ここでは、新しい Node アプリケーションを作成し、npm を使用してモジュール パッケージを追加します。 このアプリケーションで使用する、 [Express] と [Azure] モジュールです。 Express モジュールは node の Model View Controller フレームワークを提供し、Azure モジュールは Table サービスへの接続を提供します。

### express のインストールとスキャフォールディングの生成

1. コマンド ラインからという名前の新しいディレクトリを作成 **tasklist** し、そのディレクトリに移動します。  

2. 次のコマンドを入力して、Express モジュールをインストールします。

        npm install express-generator@4.2.0 -g

    オペレーティング システムによっては、コマンドの前に 「sudo」を入力することが必要な場合があります。

        sudo npm install express-generator@4.2.0 -g

    出力は次の例のようになります。

        express-generator@4.2.0 /usr/local/lib/node_modules/express-generator
        ├── mkdirp@0.3.5
        └── commander@1.3.2 (keypress@0.1.0)

    > [AZURE.NOTE] '-G' パラメーターは、モジュールをグローバルにインストールされます。 使用してこのように、 **express** の追加パス情報を入力しなくても web アプリのスキャフォールディングを生成します。

4. アプリケーションのスキャフォールディングを作成するには、入力、 **express** コマンド。

        express

    このコマンドの出力は次の例のようになります。

           create : .
           create : ./package.json
           create : ./app.js
           create : ./public
           create : ./public/images
           create : ./routes
           create : ./routes/index.js
           create : ./routes/users.js
           create : ./public/stylesheets
           create : ./public/stylesheets/style.css
           create : ./views
           create : ./views/index.jade
           create : ./views/layout.jade
           create : ./views/error.jade
           create : ./public/javascripts
           create : ./bin
           create : ./bin/www

           install dependencies:
             $ cd . && npm install

           run the app:
             $ DEBUG=my-application ./bin/www

    いくつかの新しいディレクトリやファイルのようになりましたがある、 **tasklist** ディレクトリ。

### 追加モジュールをインストールする

ファイルのいずれかを **express** 作成は **package.json**します。 このファイルには、モジュールの依存関係のリストが含まれます。 後でアプリケーションを App Service Web Apps にデプロイするときに、このファイルによって Azure にインストールする必要があるモジュールが特定されます。

コマンドラインでに記述されたモジュールをインストールするには、次のコマンドを入力して、 **package.json** ファイルです。 場合によっては、"sudo" を使用する必要があります。

    npm install

このコマンドの出力は次の例のようになります。

    debug@0.7.4 node_modules\debug

    cookie-parser@1.0.1 node_modules\cookie-parser
    ├── cookie-signature@1.0.3
    └── cookie@0.1.0

    [...]


次に、インストールするには、次のコマンドを入力、 [azure], 、[node-uuid], 、[nconf] と [async] モジュール。

    npm install azure-storage node-uuid async nconf --save

 **--保存** フラグは、これらのモジュールのエントリを追加、 **package.json** ファイルです。

このコマンドの出力は次の例のようになります。

    async@0.9.0 node_modules\async

    node-uuid@1.4.1 node_modules\node-uuid

    nconf@0.6.9 node_modules\nconf
    ├── ini@1.2.1
    ├── async@0.2.9
    └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.10)

    [...]


## アプリケーションを作成する

これで、アプリケーションを作成する準備ができました。

### モデルの作成

A *モデル* アプリケーション内のデータを表すオブジェクトします。 このアプリケーションの場合、To Do リストの項目を表す Task オブジェクトが唯一のモデルです。 Task には次のフィールドがあります。

- PartitionKey
- RowKey
- name (文字列)
- category (文字列)
- completed (ブール値)

**PartitionKey** と **RowKey** テーブル サービスでテーブル キーとして使用されます。 詳細については、次を参照してください。 [テーブル サービス データ モデルについて](https://msdn.microsoft.com/library/azure/dd179338.aspx)します。


1.  **Tasklist** ディレクトリという名前の新しいディレクトリを作成する **モデル**します。

2.  **モデル** ディレクトリという名前の新しいファイルを作成する **task.js**します。 このファイルには、アプリケーションで作成されるタスクのモデルが格納されます。

3. 先頭に、 **task.js** ファイルに必要なライブラリを参照する次のコードを追加します。

        var azure = require('azure-storage');
        var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;

4. Task オブジェクトを定義し、エクスポートする次のコードを追加します。 このオブジェクトは、テーブルへの接続を処理します。

        module.exports = Task;

        function Task(storageClient, tableName, partitionKey) {
          this.storageClient = storageClient;
          this.tableName = tableName;
          this.partitionKey = partitionKey;
          this.storageClient.createTableIfNotExists(tableName, function tableCreated(error) {
            if(error) {
              throw error;
            }
          });
        };

5. Task オブジェクトの追加のメソッドを定義する次のコードを追加します。このメソッドによって、テーブルに格納されたデータを操作できます。

        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(this.tableName, query, null, function entitiesQueried(error, result) {
              if(error) {
                callback(error);
              } else {
                callback(null, result.entries);
              }
            });
          },

          addItem: function(item, callback) {
            self = this;
            // use entityGenerator to set types
            // NOTE: RowKey must be a string type, even though
            // it contains a GUID in this example.
            var itemDescriptor = {
              PartitionKey: entityGen.String(self.partitionKey),
              RowKey: entityGen.String(uuid()),
              name: entityGen.String(item.name),
              category: entityGen.String(item.category),
              completed: entityGen.Boolean(false)
            };
            self.storageClient.insertEntity(self.tableName, itemDescriptor, function entityInserted(error) {
              if(error){  
                callback(error);
              }
              callback(null);
            });
          },

          updateItem: function(rKey, callback) {
            self = this;
            self.storageClient.retrieveEntity(self.tableName, self.partitionKey, rKey, function entityQueried(error, entity) {
              if(error) {
                callback(error);
              }
              entity.completed._ = true;
              self.storageClient.updateEntity(self.tableName, entity, function entityUpdated(error) {
                if(error) {
                  callback(error);
                }
                callback(null);
              });
            });
          }
        }

6. 保存して閉じる、 **task.js** ファイルです。

### コントローラーの作成

A *コント ローラー* HTTP 要求を処理し、HTML 応答を表示します。

1.  **Tasklist/ルート** ディレクトリという名前の新しいファイルを作成する **tasklist.js** し、テキスト エディターで開きます。

2. 次のコードを追加 **tasklist.js**します。 使用される azure および async モジュールが読み込まれます **tasklist.js**します。 これも定義されている、 **TaskList** のインスタンスに渡される関数、 **タスク** 先ほど定義したオブジェクトします。

        var azure = require('azure-storage');
        var async = require('async');

        module.exports = TaskList;

3. 定義、 **TaskList** オブジェクトです。

        function TaskList(task) {
          this.task = task;
        }


4. 次のメソッドを追加 **TaskList**:

        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = new azure.TableQuery()
              .where('completed eq ?', false);
            self.task.find(query, function itemsFound(error, items) {
              res.render('index',{title: 'My ToDo List ', tasks: items});
            });
          },

          addTask: function(req,res) {
            var self = this
            var item = req.body.item;
            self.task.addItem(item, function itemAdded(error) {
              if(error) {
                throw error;
              }
              res.redirect('/');
            });
          },

          completeTask: function(req,res) {
            var self = this;
            var completedTasks = Object.keys(req.body);
            async.forEach(completedTasks, function taskIterator(completedTask, callback) {
              self.task.updateItem(completedTask, function itemsUpdated(error) {
                if(error){
                  callback(error);
                } else {
                  callback(null);
                }
              });
            }, function goHome(error){
              if(error) {
                throw error;
              } else {
               res.redirect('/');
              }
            });
          }
        }


### app.js の変更

1.  **Tasklist** ディレクトリを開き、 **app.js** ファイルです。 このファイルを実行して、先ほど作成した、 **express** コマンドです。

2. ファイルの先頭に次のコードを追加します。このコードは、azure モジュールを読み込み、テーブル名とパーティション キーを設定し、この例で使用するストレージ資格情報を設定します。

        var azure = require('azure-storage');
        var nconf = require('nconf');
        nconf.env()
             .file({ file: 'config.json', search: true });
        var tableName = nconf.get("TABLE_NAME");
        var partitionKey = nconf.get("PARTITION_KEY");
        var accountName = nconf.get("STORAGE_NAME");
        var accountKey = nconf.get("STORAGE_KEY");

    > [AZURE.NOTE] nconf は、環境変数から構成値を読み込む、または **config.json** ファイルで、後で作成します。

3. app.js ファイル内で、次の行が表示されるまで下へスクロールします。

        app.use('/', routes);
        app.use('/users', users);

    これらの行を下のコードに置き換えます。 これにより、ストレージ アカウントへの接続を使って、 <strong>タスク</strong> のインスタンスが初期化されます。 これは <strong>TaskList</strong>に渡され、TaskList ではこれを使用してテーブル サービスを操作します。

        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(accountName, accountKey), tableName, partitionKey);
        var taskList = new TaskList(task);

        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));

4. 保存、 **app.js** ファイルです。

### index ビューの変更

1. 開いている、 **tasklist/views/index.jade** ファイルをテキスト エディターでします。

2. ファイルの内容全体を次のコードに置き換えます。 これにより、既存のタスクを表示するビューが定義されます。このビューには、新しいタスクを追加し、既存のタスクを完了済みとしてマークするためのフォームが含まれます。

        extends layout

        block content
          h1= title
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
                    td #{task.name._}
                    td #{task.category._}
                    - var day   = task.Timestamp._.getDate();
                    - var month = task.Timestamp._.getMonth() + 1;
                    - var year  = task.Timestamp._.getFullYear();
                    td #{month + "/" + day + "/" + year}
                    td
                      input(type="checkbox", name="#{task.RowKey._}", value="#{!task.completed._}", checked=task.completed._)
            button.btn(type="submit") Update tasks
          hr
          form.well(action="/addtask", method="post")
            label Item Name:
            input(name="item[name]", type="textbox")
            label Item Category:
            input(name="item[category]", type="textbox")
            br
            button.btn(type="submit") Add item

3. 保存して閉じます **index.jade** ファイルです。

### グローバル レイアウトの変更

 **Layout.jade** ファイルで、 **ビュー** ディレクトリは、その他のグローバル テンプレート **.jade** ファイルです。 この手順で使用するように変更が [Twitter Bootstrap](https://github.com/twbs/bootstrap), 、見栄えのよい、web アプリをデザインしやすくツールキットです。

ファイルをダウンロードして展開 [Twitter Bootstrap](http://getbootstrap.com/)します。 コピー、 **bootstrap.min.css** ブートス トラップからファイル **css** フォルダーに、 **パブリック/スタイル シート** 、アプリケーションのディレクトリ。

 **ビュー** フォルダーを開き、 **layout.jade** 全体の内容を次に置き換えます。

    doctype html
    html
      head
        title= title
        link(rel='stylesheet', href='/stylesheets/bootstrap.min.css')
        link(rel='stylesheet', href='/stylesheets/style.css')
      body.app
        nav.navbar.navbar-default
          div.navbar-header
          a.navbar-brand(href='/') My Tasks
        block content

### config ファイルの作成

アプリケーションをローカルで実行するために、Azure Storage の資格情報を config ファイルに追加します。 という名前のファイルを作成する **config.json* * を次の JSON:

    {
        "STORAGE_NAME": "<storage account name>",
        "STORAGE_KEY": "<storage access key>",
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

置換 **ストレージ アカウント名** 記憶領域の名前を前に作成したアカウントし、置換 **ストレージ アクセス キー** 、ストレージ アカウントのプライマリ アクセス キー。 次に例を示します。

    {
        "STORAGE_NAME": "nodejsappstorage",
        "STORAGE_KEY": "KG0oDd..."
        "PARTITION_KEY": "mytasks",
        "TABLE_NAME": "tasks"
    }

このファイルを保存 *上位の 1 つのディレクトリ レベル* よりも、 **tasklist** ディレクトリで、次のようにします。

    parent/
      |-- config.json
      |-- tasklist/

これは、公開される可能性のあるソース コントロールに config ファイルがチェックインされないようにするためです。 アプリを Azure にデプロイするときは、config ファイルではなく環境変数を使用します。


## ローカルでアプリケーションを実行する

ローカル コンピューターでアプリケーションをテストするには、次の手順を実行します。

1. コマンドラインでのディレクトリに移動、 **tasklist** ディレクトリ。

2. 次のコマンドを使用して、ローカルでアプリケーションを起動します。

        npm start

3. Web ブラウザーを開いて、http://127.0.0.1:3000 にアクセスします。

    次の例のような Web ページが表示されます。

    ![空のタスク一覧が表示されている Web ページ][node-table-finished]

4. 新しいタスク項目を作成する名前とカテゴリを入力し、クリックして **項目の追加**します。 

6. タスクを完了としてマークする **完了** ] をクリック **更新タスク**します。

    ![タスク一覧の新しいアイテムの画像][node-table-list-items]

アプリケーションがローカルで実行されていても、データは Azure Table サービスに保存されています。

## Azure へのアプリケーションのデプロイ

このセクションの手順では、Azure コマンド ライン ツールを使用して App Service で新しい Web アプリを作成し、Git を使用してアプリケーションをデプロイします。 これらの手順を実行するには、Azure サブスクリプションが必要です。

> [AZURE.NOTE] 次の手順を使用して実行することもできます。、 [Azure ポータル](https://portal.azure.com)します。 See [Build and deploy a Node.js web app in Azure App Service].
>
> これが初めて作成した Web アプリの場合、Azure ポータルを使用してこのアプリケーションをデプロイする必要があります。

最初に、インストール、 [Azure CLI] コマンド ラインから次のコマンドを入力します。

    npm install azure-cli -g

### 発行の設定をインポートする

この手順では、サブスクリプションに関する情報が含まれたファイルをダウンロードします。

1. 次のコマンドを入力します。

        azure account download

    このコマンドにより、ブラウザーが起動し、ダウンロード ページに移動します。 ログインを求められた場合は、Azure サブスクリプションに関連付けられたアカウントを使用してログインします。

    <!-- ![The download page][download-publishing-settings] -->

    ファイルのダウンロードが自動的に開始されます。ダウンロードが開始されない場合は、ページの先頭にあるリンクをクリックして、手動でファイルをダウンロードできます。 ファイルを保存し、ファイル パスを書き留めます。

2. 次のコマンドを入力して設定をインポートします。

        azure account import <path-to-file>

    そのためには、前の手順でダウンロードした発行設定ファイルのパスとファイル名を指定します。

3. 設定をインポートしたら、不要になった発行設定ファイルを削除してください。 このファイルには、Azure サブスクリプションに関する機密情報が含まれているためです。

### App Service Web アプリの作成

1. コマンドラインでのディレクトリに移動、 **tasklist** ディレクトリ。

2. 次のコマンドを使用して、新しい Web アプリを作成します。

        azure site create --git

    Web アプリの名前と場所の入力を求められます。 一意の名前を指定し、Azure ストレージ アカウントと同じ地理的な場所を選択します。

    `--git` パラメーターにより、Azure にこの Web アプリの Git リポジトリが作成されます。 [なし] が存在し、追加する場合も、現在のディレクトリで Git リポジトリが初期化に、 [Git remote] 'azure' という名前を使用して Azure にアプリケーションを発行します。 最後に、作成、 **web.config** ファイルで、Azure ノード アプリケーションをホストするために使用する設定が含まれます。 `--git` パラメーターを省略した場合でも、ディレクトリに Git リポジトリが含まれていれば、"azure" リモートが作成されます。

    このコマンドが完了すると、次のような出力が表示されます。 始まる行 **で web サイトが作成された** web アプリの URL が含まれています。

        info:   Executing command site create
        help:   Need a site name
        Name: TableTasklist
        info:   Using location southcentraluswebspace
        info:   Executing `git init`
        info:   Creating default .gitignore file
        info:   Creating a new web site
        info:   Created web site at  tabletasklist.azurewebsites.net
        info:   Initializing repository
        info:   Repository initialized
        info:   Executing `git remote add azure https://username@tabletasklist.azurewebsites.net/TableTasklist.git`
        info:   site create command OK

    > [AZURE.NOTE] これは、サブスクリプションの最初の App Service web アプリである場合は、Azure ポータルを使用して web アプリを作成するように指示されます。 詳細については、次を参照してください。 [Build and deploy a Node.js web app in Azure App Service]します。

### 環境変数の設定

この手順では、Azure の Web アプリの構成に環境変数を追加します。
コマンド ラインで次のように入力します。

    azure site appsetting add
        STORAGE_NAME=<storage account name>;STORAGE_KEY=<storage access key>;PARTITION_KEY=mytasks;TABLE_NAME=tasks


置換 **<storage account name>** 記憶領域の名前を前に作成したアカウントし、置換 **<storage access key>** 、ストレージ アカウントのプライマリ アクセス キー。 (以前に作成した config.json ファイルと同じ値を使用します)。

またはで環境変数を設定、 [Azure ポータル](https://portal.azure.com):

1.  クリックして、web アプリのブレードを開きます **参照** > **Web Apps** > web アプリの名前。

1.  Web アプリのブレードで、[ **設定をすべて** > **アプリケーション設定**します。

    <!-- ![Top Menu](./media/storage-nodejs-use-table-storage-web-site/PollsCommonWebSiteTopMenu.png) -->

1.  下へスクロールして、 **アプリ設定** セクションし、キー/値ペアを追加します。

    ![アプリ設定](./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png)

1. クリックして **保存**します。


### アプリケーションの発行

アプリを発行するには、コード ファイルを Git にコミットし、azure/master にプッシュします。

1. デプロイメント資格情報を設定します。

        azure site deployment user set <name> <password>

2. アプリケーション ファイルを追加し、コミットします。

        git add .
        git commit -m "adding files"

3. コミットを App Service の Web アプリにプッシュします。

        git push azure master

    使用 **マスター** ターゲット分岐として。 デプロイの最後に、次の例のようなステートメントが表示されます。

        To https://username@tabletasklist.azurewebsites.net/TableTasklist.git
         * [new branch]      master -> master

4. プッシュ操作が完了したら、先ほど `azure create site` コマンドから返された Web アプリの URL を参照してアプリケーションを表示します。


## 次のステップ

この記事の手順では、Table サービスを使用して情報を格納する方法を説明しましたが、MongoDB を使用することもできます。 参照してください [Node.js web app with MongoDB] の詳細。

## その他のリソース

[Azure CLI]

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

[Build and deploy a Node.js web app in Azure App Service]: web-sites-nodejs-develop-deploy-mac.md
[Continuous deployment using GIT in Azure App Service]: web-sites-publish-source-control.md
[Azure Developer Center]: /develop/nodejs/


[node]: http://nodejs.org
[Git]: http://git-scm.com
[Express]: http://expressjs.com
[for free]: http://windowsazure.com
[Git remote]: http://git-scm.com/docs/git-remote

[Node.js web app with MongoDB]: web-sites-nodejs-store-data-mongodb.md
[Azure CLI]: ../xplat-cli-install.md

[Continuous deployment using GIT in Azure App Service]: web-sites-publish-source-control.md
[azure]: https://github.com/Azure/azure-sdk-for-node
[node-uuid]: https://www.npmjs.com/package/node-uuid
[nconf]: https://www.npmjs.com/package/nconf
[async]: https://www.npmjs.com/package/async

[Azure Portal]: https://portal.azure.com


[node-table-finished]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_empty.png
[node-table-list-items]: ./media/storage-nodejs-use-table-storage-web-site/table_todo_list.png
[download-publishing-settings]: ./media/storage-nodejs-use-table-storage-web-site/azure-account-download-cli.png
[portal-new]: ./media/storage-nodejs-use-table-storage-web-site/plus-new.png
[portal-storage-account]: ./media/storage-nodejs-use-table-storage-web-site/new-storage.png
[portal-quick-create-storage]: ./media/storage-nodejs-use-table-storage-web-site/quick-storage.png
[portal-storage-access-keys]: ./media/storage-nodejs-use-table-storage-web-site/manage-access-keys.png

[go-to-dashboard]: ./media/storage-nodejs-use-table-storage-web-site/go_to_dashboard.png
[web-configure]: ./media/storage-nodejs-use-table-storage-web-site/sql-task-configure.png
[app-settings-save]: ./media/storage-nodejs-use-table-storage-web-site/savebutton.png
[app-settings]: ./media/storage-nodejs-use-table-storage-web-site/storage-tasks-appsettings.png

[Create and deploy a Node.js application to an Azure Web Site]: web-sites-nodejs-develop-deploy-mac.md
 

