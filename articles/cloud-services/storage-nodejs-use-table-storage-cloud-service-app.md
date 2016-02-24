<properties 
    pageTitle="テーブル ストレージを使用した Web アプリケーション (Node.js) | Microsoft Azure" 
    description="Express を使用する Web アプリケーションのチュートリアルを基に、Azure Storage サービスと Azure モジュールを追加するチュートリアル。" 
    services="cloud-services, storage" 
    documentationCenter="nodejs" 
    authors="TomArcher" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="tarcher"/>






# ストレージを使用する Node.js Web アプリケーション

## 概要

このチュートリアルで作成したアプリケーションを拡張します、
[Node.js Web Application using Express] Microsoft を使用してチュートリアル
データ管理サービスを使用する Node.js 用 azure クライアント ライブラリ。 クラスターの
web ベースのタスク一覧アプリケーションを作成するアプリケーションを拡張します。
Azure にデプロイすることができます。 タスク一覧では、ユーザー
タスクの取得、新しいタスクを追加し、タスクを完了としてマークします。

タスク項目は Azure ストレージに格納されます。 Azure
ストレージは、フォールト トレランスに優れた非構造化データ ストレージが提供し、
高可用性です。 Azure ストレージには、いくつかのデータ構造が含まれています。
格納して、データへのアクセスで記憶域を利用して
Node.js 用 Azure SDK に含まれる Api からのサービスまたは
REST api です。 詳細については、次を参照してください。 [Storing and Accessing Data in Azure]します。

このチュートリアルを完了したことを想定しています、 [Node.js Web
アプリケーション] と [Node.js with Express][Node.js Web Application using Express] チュートリアルです。

学習内容:

-   Jade テンプレート エンジンを操作する方法
-   Azure データ管理サービスを操作する方法

完成したアプリケーションのスクリーンショットは次のようになります。

![Internet Explorer で表示された完成した Web ページ](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## Web.Config のストレージ資格情報の設定

Azure ストレージにアクセスするには、ストレージに合格する必要があります。
資格情報。 そのためには、web.config アプリケーション設定を使用します。
これらの設定は環境変数としてノードにします。
Azure SDK によって読み取られます。

> [AZURE.NOTE] アプリケーションは、ストレージ資格情報は使用のみ
Azure に展開するときにのみ使用されます。 エミュレーターで実行している場合、アプリケーションは
ストレージ エミュレーターを使用します。

ストレージ アカウントの資格情報を取得するには、次の手順を実行します。
web.config 設定に追加します。

1.  これがまだ開いていない場合は、Azure PowerShell を起動します、 **開始** ] メニューの順に展開する **すべてのプログラム]、[Azure**, を右クリックして **Azure PowerShell**, 、し、[ **管理者として実行**します。

2.  アプリケーションが含まれているフォルダーに移動します。 たとえば、C:\\node\\tasklist\\WebRole1 です。

3.  Azure Powershell ウィンドウで次のコマンドレットを入力して、ストレージ アカウント情報を取得します。

        PS C:\node\tasklist\WebRole1> Get-AzureStorageAccounts

    これにより、ホストされるサービスに関連付けられたストレージ アカウントとアカウント キーのリストが取得されます。

    > [AZURE.NOTE] Azure SDK は、サービスを展開するときに、ストレージ アカウントを作成するため、以前のガイドでアプリケーションを配置するのにストレージ アカウントが存在しない必要があります。

4.  開いている、 **ServiceDefinition.csdef** 、アプリケーションが Azure に展開されるときに使用される環境設定を含むファイル。

        PS C:\node\tasklist> notepad ServiceDefinition.csdef

5.  次のブロックの挿入 **環境** STORAGE の置換要素と STORAGE ACCESS KEY} アカウント名と展開に使用するストレージ アカウントのプライマリ キーを使用します。

        <Variable name="AZURE_STORAGE_ACCOUNT" value="{STORAGE ACCOUNT}" />
        <Variable name="AZURE_STORAGE_ACCESS_KEY" value="{STORAGE ACCESS KEY}" />

    ![web.cloud.config ファイルの内容](./media/storage-nodejs-use-table-storage-cloud-service-app/node37.png)

6.  ファイルを保存して、メモ帳を閉じます。

### 追加モジュールをインストールする

2. 次のコマンドを使用して、[azure] をインストールする [node-uuid]、[nconf] および [async] モジュールにもローカルにエントリを保存する、 **package.json** ファイル。

        PS C:\node\tasklist\WebRole1> npm install azure-storage node-uuid async nconf --save

    このコマンドの出力は次のように表示されます。

        node-uuid@1.4.1 node_modules\node-uuid

        nconf@0.6.9 node_modules\nconf
        ├── ini@1.1.0
        ├── async@0.2.9
        └── optimist@0.6.0 (wordwrap@0.0.2, minimist@0.0.8)

        azure-storage@0.1.0 node_modules\azure-storage
        ├── extend@1.2.1
        ├── xmlbuilder@0.4.3
        ├── mime@1.2.11
        ├── underscore@1.4.4
        ├── validator@3.1.0
        ├── node-uuid@1.4.1
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.27.0 (json-stringify-safe@5.0.0, tunnel-agent@0.3.0, aws-sign@0.3.0, forever-agent@0.5.2, qs@0.6.6, oauth-sign@0.3.0, cookie-jar@0.3.0, hawk@1.0.0, form-data@0.1.3, http-signature@0.10.0)

##ノード アプリケーションでのテーブル サービスの使用

このセクションでは、作成された基本的なアプリケーションを拡張します、 **express** コマンドを追加することで、 **task.js** ファイルをタスクのモデルを格納します。 既存の属性も変更されます **app.js** され、新しい作成 **tasklist.js** モデルを使用するファイルです。

### モデルの作成

1.  **WebRole1** ディレクトリという名前の新しいディレクトリを作成する **モデル**します。

2.  **モデル** ディレクトリという名前の新しいファイルを作成する **task.js**します。 このファイルには、アプリケーションで作成されるタスクのモデルが格納されます。

3. 先頭に、 **task.js** ファイルに必要なライブラリを参照する次のコードを追加します。

        var azure = require('azure-storage');
        var uuid = require('node-uuid');
        var entityGen = azure.TableUtilities.entityGenerator;

4. 次に、Task オブジェクトを定義およびエクスポートするコードを追加します。 このオブジェクトは、テーブルへの接続を処理します。

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

5. 次に、Task オブジェクトの追加のメソッドを定義する次のコードを追加します。このメソッドによって、テーブルに格納されたデータを操作できます。

        Task.prototype = {
          find: function(query, callback) {
            self = this;
            self.storageClient.queryEntities(query, function entitiesQueried(error, result) {
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

1.  **WebRole1/ルート** ディレクトリという名前の新しいファイルを作成する **tasklist.js** し、テキスト エディターで開きます。

2. 次のコードを追加 **tasklist.js**します。 使用される azure および async モジュールが読み込まれます **tasklist.js**します。 これも定義されている、 **TaskList** のインスタンスに渡される関数、 **タスク** 先ほど定義したオブジェクトします。

        var azure = require('azure-storage');
        var async = require('async');

        module.exports = TaskList;

        function TaskList(task) {
          this.task = task;
        }

2. 追加を続行、 **tasklist.js** ファイルに使用するメソッドを追加することで **showTasks**, 、**addTask**, 、および **completeTasks**:

        TaskList.prototype = {
          showTasks: function(req, res) {
            self = this;
            var query = azure.TableQuery()
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

3. 保存、 **tasklist.js** ファイルです。

### app.js の変更

1.  **WebRole1** ディレクトリを開き、 **app.js** ファイルをテキスト エディターでします。 

2. ファイルの先頭に次のコードを追加します。このコードは、azure モジュールを読み込み、テーブル名とパーティション キーを設定します。

        var azure = require('azure-storage');
        var tableName = 'tasks';
        var partitionKey = 'hometasks';

3. app.js ファイル内で、次の行が表示されるまで下へスクロールします。

        app.use('/', routes);
        app.use('/users', users);

    これらの行を下のコードに置き換えます。 これにより、ストレージ アカウントへの接続を使って、 <strong>タスク</strong> のインスタンスが初期化されます。 これは <strong>TaskList</strong>に渡され、TaskList ではこれを使用してテーブル サービスを操作します。

        var TaskList = require('./routes/tasklist');
        var Task = require('./models/task');
        var task = new Task(azure.createTableService(), tableName, partitionKey);
        var taskList = new TaskList(task);

        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
    
4. 保存、 **app.js** ファイルです。

### index ビューの変更

1. ディレクトリに移動、 **ビュー** ディレクトリおよび開いている、 **index.jade** ファイルをテキスト エディターでします。

2. 内容を置き換える、 **index.jade** 次のコード ファイル。 これにより、既存のタスクを表示するビューと、新しいタスクの追加とタスクの完了済みのマーク付けを実行するためのフォームを定義します。

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
              if tasks != []
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

 **Layout.jade** ファイルで、 **ビュー** ディレクトリがその他のグローバル テンプレートとして使用される **.jade** ファイルです。 この手順で使用するように変更が [Twitter Bootstrap](https://github.com/twbs/bootstrap), 、見栄えの良い web サイトをデザインしやすくツールキットです。

1. ファイルをダウンロードして展開 [Twitter Bootstrap](http://getbootstrap.com/)します。 コピー、 **bootstrap.min.css** ファイルから、 **bootstrap \\dist\\css** フォルダーを **public \\stylesheets** tasklist アプリケーションのディレクトリ。

2.  **ビュー** フォルダーを開き、 **layout.jade** 、テキスト エディターで内容を次に置き換えます。

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

3. 保存、 **layout.jade** ファイルです。

### エミュレーターでのアプリケーションの実行

エミュレーターでアプリケーションを開始するには、次のコマンドを実行します。

    PS C:\node\tasklist\WebRole1> start-azureemulator -launch

ブラウザーが開き、次のページが表示されます。

![My Task List というタイトルの Web ページと新しいタスクを追加するためのタスクとフィールドを含むテーブル](./media/storage-nodejs-use-table-storage-cloud-service-app/node44.png)

フォームを使用してアイテムを追加するか、完了済みのマークを付けることにより既存のアイテムを削除します。

## Azure にアプリケーションをデプロイする


Windows PowerShell ウィンドウで、次のコマンドレットを呼び出して、ホストされるサービスを Azure に再デプロイします。

    PS C:\node\tasklist\WebRole1> Publish-AzureServiceProject -name myuniquename -location datacentername -launch

置換 **myuniquename** にこのアプリケーションの一意の名前。 置換 **datacentername** 、Azure データ センターの名前など、 **米国西部**します。

デプロイが完了すると、次のような応答が表示されます。

    PS C:\node\tasklist> publish-azureserviceproject -servicename tasklist -location "West US"
    WARNING: Publishing tasklist to Microsoft Azure. This may take several minutes...
    WARNING: 2:18:42 PM - Preparing runtime deployment for service 'tasklist'
    WARNING: 2:18:42 PM - Verifying storage account 'tasklist'...
    WARNING: 2:18:43 PM - Preparing deployment for tasklist with Subscription ID: 65a1016d-0f67-45d2-b838-b8f373d6d52e...
    WARNING: 2:19:01 PM - Connecting...
    WARNING: 2:19:02 PM - Uploading Package to storage service larrystore...
    WARNING: 2:19:40 PM - Upgrading...
    WARNING: 2:22:48 PM - Created Deployment ID: b7134ab29b1249ff84ada2bd157f296a.
    WARNING: 2:22:48 PM - Initializing...
    WARNING: 2:22:49 PM - Instance WebRole1_IN_0 of role WebRole1 is ready.
    WARNING: 2:22:50 PM - Created Website URL: http://tasklist.cloudapp.net/.

前回と同様に、指定したため、 **-起動** オプション、ブラウザーが開き、アプリケーションの発行が完了すると、Azure で実行されているが表示されます。

![My Task List ページを表示しているブラウザー ウィンドウ。 URL から、ページが Azure でホストされていることがわかります](./media/storage-nodejs-use-table-storage-cloud-service-app/getting-started-1.png)

## アプリケーションの停止と削除

アプリケーションを配置するには、後にすることも無効にすることができます。
コストを回避またはビルドし、無料の試用期間内の他のアプリケーションの展開
までの時間。

Azure では、消費されたサーバー時間の 1 時間単位の料金が Web ロール インスタンスに課金されます。
アプリケーションを展開すると、サーバー時間が消費される場合でも、
インスタンスが実行されていないと停止の状態にします。

次の手順では、アプリケーションの停止と削除の方法を示します。

1.  Windows PowerShell ウィンドウで、サービスのデプロイを停止します
    次のコマンドレットは、前のセクションで作成されます。

        PS C:\node\tasklist\WebRole1> Stop-AzureService

    サービスの停止には、数分間かかる場合があります。 サービスが停止すると、停止したことを知らせるメッセージが表示されます。

3.  サービスを削除するには、次のコマンドレットを呼び出します。

        PS C:\node\tasklist\WebRole1> Remove-AzureService contosotasklist

    求められたら、入力 **Y** サービスを削除します。

    サービスの削除には、数分間かかる場合があります。 サービスが削除されると、削除されたことを知らせるメッセージが表示されます。

  [Node.js Web Application using Express]: http://azure.microsoft.com/develop/nodejs/tutorials/web-app-with-express/
  [Storing and Accessing Data in Azure]: http://msdn.microsoft.com/library/azure/gg433040.aspx
  [Node.js Web Application]: http://azure.microsoft.com/develop/nodejs/tutorials/getting-started/
 
 

