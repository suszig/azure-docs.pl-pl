<properties
    pageTitle =「構築し、Azure App Service での Node.js API アプリのデプロイ」
    description =「Node.js API アプリ パッケージを作成し、Azure App Service にデプロイする方法について説明します」。
    サービス ="app-service \api"
    documentationCenter ="node"
    authors ="bradygaster"
    manager ="mohisri"
    エディター ="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="node"
    ms.topic="get-started-article"
    ms.date="11/27/2015"
    ms.author="bradygaster"/>

# Azure App Service での Node.js API アプリの構築とデプロイ

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

## 前提条件
1. [Node.js](nodejs.org) (このサンプルは Node.js 4.2.2 のバージョンがインストールされていると想定しています)、開発用コンピューターで実行されています。
1. [GitHub](https://github.com/) アカウント
1. Microsoft Azure [無料試用版アカウント](https://azure.microsoft.com/pricing/free-trial/)
1. ローカルの開発用ワークステーションに Git がインストールされている

## セットアップ手順
Node.js コマンド ラインを使用して次のコマンドを実行します。 Swaggerize Yo generator を使用すると、Swagger JSON ファイルに定義されている HTTP 要求の処理に必要になるベースライン Node.js コードをスキャフォールディングすることができます。 
 
1. インストール **yo** と **ジェネレーター swaggerize** の NPM モジュール グローバルにします。

        npm install -g yo
        npm install -g generator-swaggerize
        
1. 複製、 [GitHub リポジトリのサンプル コードを含む](https://github.com/Azure-Samples/app-service-api-node-contact-list)します。

        git clone https://github.com/Azure-Samples/app-service-api-node-contact-list.git
                
1. 基づく API をスキャフォールディングするコマンドを実行、 **api.json** ソース コードに含まれるファイルです。  **Api.json** ファイルを使用してをスキャフォールディングは実際の API を表す Swagger ファイルでは、次の手順でコマンド"yo swaggerize"です。 

        yo swaggerize
        
    **注:** API.json は、別のものとして、 *apiapp.json* API Apps プレビューの期間からのファイルです。

1. Swaggerize はハンドラーおよびに含まれる Swagger メタデータ用の構成をスキャフォールディング **api.json**します。 スキャフォールディング プロセス中は、GitHub ユーザー名や電子メール アドレスなど、さまざまな質問に回答します。 この情報は生成に使用、 **package.json** 、アプリケーションのフォルダー内のファイルです。 スキャフォールディング プロセス中に要求されるすべての質問の最も重要なはことを選択する **express** よう求められたら、このサンプルでは、API アプリが Azure で (またはローカル) を実行しているときに、後で、Swagger のヘルプ ページを生成する高速のビュー エンジンを使用します。  

    ![Swaggerize コマンド ライン](media/app-service-api-nodejs-api-app/swaggerize-command-line.png)
    
1. スキャフォールディングされたコードを含むフォルダーに移動する (この場合、 *ContactList* サブフォルダー)。 その後、インストール、 **jsonpath** NPM モジュールです。 

        npm install --save jsonpath
        
    コマンドライン エクスペリエンスにインストール結果が表示されます。 

    ![Jsonpath のインストール](media/app-service-api-nodejs-api-app/jsonpath-install.png)

1. インストール、 **swaggerize ui** NPM モジュールです。 

        npm install --save swaggerize-ui
        
    コマンドライン エクスペリエンスにインストール結果が表示されます。 

    ![Swaggerize UI のインストール](media/app-service-api-nodejs-api-app/swaggerize-ui-install.png)

1. コピー、 **lib** フォルダーから、 **開始** フォルダーに、 **ContactList** 、scaffolder によって作成されたフォルダーです。 

1. コードで置き換え、 **handlers/contacts.js** 次のコード ファイル。 このコードに格納された JSON データを使用して、 **lib/contacts.json** ファイルで提供される **lib/contactRepository.js**します。 次の新しい contats.js コードは HTTP 要求に応答して、このコードを使用するすべての contact を取得します。 

        'use strict';
        
        var repository = require('../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.all())
            }
        };

1. コードで置き換え、 **handlers/contacts/{id}.js** を使用して、次のコード ファイル **lib/contactRepository.js** を HTTP 要求で要求された連絡先を取得し、JSON ペイロードとして返すことです。 

        'use strict';

        var repository = require('../../lib/contactRepository');
        
        module.exports = {
            get: function contacts_get(req, res) {
                res.json(repository.get(req.params['id']))
            }    
        };

1. コードに置き換えます **server.js** 次のコードです。 次のコードは、server.js ファイルの変更点がわかるように、コメントで注記を入れています。 

        'use strict';

        var port = process.env.PORT || 8000; // first change

        var http = require('http');
        var express = require('express');
        var bodyParser = require('body-parser');
        var swaggerize = require('swaggerize-express');
        var swaggerUi = require('swaggerize-ui'); // second change
        var path = require('path');

        var app = express();

        var server = http.createServer(app);

        app.use(bodyParser.json());

        app.use(swaggerize({
            api: path.resolve('./config/api.json'), // third change
            handlers: path.resolve('./handlers'),
            docspath: '/swagger' // fourth change
        }));

        // change four
        app.use('/docs', swaggerUi({
          docs: '/swagger'  
        }));

        server.listen(port, function () { // fifth change
            app.setHost(undefined); // sixth and final change
        });

1. Node.js コマンドラインの実行可能ファイルを使用して、サーバーを起動します。 

        node server.js

    このコマンドを実行すると、Node.js HTTP サーバーが起動し、API のサービスが開始されます。 

1. 参照すると **http://localhost:8000/連絡先** 連絡先一覧の JSON の出力が表示されます (または、ブラウザーによって、ダウンロードするように求められます)。 

    ![すべての Contacts API の呼び出し](media/app-service-api-nodejs-api-app/all-contacts-api-call.png)

1. 参照すると **http://localhost:8000/連絡先/2** その id 値によって表されるメンバーが表示されます。 

    ![特定の Contact API の呼び出し](media/app-service-api-nodejs-api-app/specific-contact-api-call.png)

1. Swagger JSON データの処理を使用して、 **/swagger** エンドポイント。

    ![Contacts Swagger Json](media/app-service-api-nodejs-api-app/contacts-swagger-json.png)

1. 使用して、Swagger UI が提供される、 **/docs** エンドポイント。 Swagger UI では、豊富な HTML クライアント機能を使用して API をテストできます。

    ![Swagger UI](media/app-service-api-nodejs-api-app/swagger-ui.png)

## Azure ポータルでの新しい API アプリの作成
このセクションでは、新しい空の API アプリを Azure に作成するプロセスの手順について説明します。 次に、アプリを Git リポジトリに接続し、コードの変更を継続的に配信できるようにします。 

ソース コードを複製した GitHub リポジトリは、デプロイメントのためにコードをプッシュするリポジトリと同じではありません。 サンプル GitHub リポジトリにはコードの "Start" 状態が含まれていましたが、現在では、コードの "end" 状態をスキャフォールディングするようになりました。API アプリに関連付けられた Git リポジトリにのみ、そのコードをプッシュする必要があります。 最初の手順は、Azure ポータルを使用して API アプリを作成することです。次に、 

1. 参照、 [Azure ポータル](http://portal.azure.com)します。 

1. 新しい API アプリを作成します。 

    ![New Api App Portal](media/app-service-api-nodejs-api-app/new-api-app-portal.png)

1. 新しい API アプリを既存のリソース グループまたは App Service プランに追加するか、スクリーンショットのように、新しいリソース グループと App Service プランを作成することができます。 

    ![Api App Creation Blade](media/app-service-api-nodejs-api-app/api-app-creation-blade.png)

1. ポータルに API アプリが作成されたら、次のように API アプリの設定を含むブレードを参照します。 

    ![ポータルの設定へのナビゲーション](media/app-service-api-nodejs-api-app/portal-nav-to-settings.png)

1. クリックして、 **デプロイ資格情報** 設定] メニューでナビゲーション項目。 ブレードが開いたら、Node.js を API アプリに発行するときに使用するユーザー名とパスワードを追加します。 クリックして、 **保存** ボタンを **デプロイ資格情報を設定** ブレードです。 

    ![デプロイメント資格情報](media/app-service-api-nodejs-api-app/deployment-credentials.png)

1. デプロイメント資格情報を設定したら、App Service に関連付けられている Git リポジトリを作成できます。 このリポジトリにコードをプッシュするたびに、Azure App Service は変更を選択し、API アプリ インスタンスに直接デプロイします。 サイトに関連付けるに Git リポジトリを作成する] をクリックして、 **継続的なデプロイ** メニュー項目] メニューのブレードでは、次のように設定します。 クリックして、 **ローカル Git リポジトリ** オプション、 **ソースの選択** ブレードです。 次に、[OK] をクリックして Git リポジトリを作成します。

    ![Git リポジトリの作成](media/app-service-api-nodejs-api-app/create-git-repo.png)

1. Git リポジトリが作成されると、ブレードが更新され、アクティブなデプロイメントが表示されます。 リポジトリは新しいので、一覧にはアクティブなデプロイメントがありません。 

    ![アクティブなデプロイメントがない](media/app-service-api-nodejs-api-app/no-active-deployments.png)

1. 最後に、ポータルから Git リポジトリの URL をコピーします。 これを行うには、新しい API アプリのブレードへの移動を見て、 **Essentials** ブレードのセクションです。 確認する必要があります、 **Git クローン URL** Essentials] セクションにします。 その横に、クリップボードに URL をコピーするアイコンがあります。 アイコンをクリックして URL をコピーするか (URL にマウス ポイントを移動するとボタンが表示されます)、URL 全体を選択してクリップボードにコピーします。

    ![ポータルから Git Url を取得する](media/app-service-api-nodejs-api-app/get-the-git-url-from-the-portal.png)

    **注**: 次の手順で URL になるように、その時点で保存場所にすることを確認して、Git 複製必要があります。

以上の手順で Git リポジトリに新しい API アプリがバックアップされました。リポジトリにコードをプッシュし、Azure の継続的なデプロイメント機能を利用して変更を自動的にデプロイできるようになります。 

## API アプリの複製を Azure にデプロイする
Azure App Service が提供する組み込みの継続的な配信機能を使用すると、App Service に関連付けられた Git リポジトリにコードをコミットするだけで、Azure でソース コードが選択され、API アプリにデプロイされます。 

1. コピー、 **src/終了/ContactList** ように作成するコードの新しいローカル Git リポジトリをデスクトップ、またはその他のいくつかのフォルダーに swaggerize scaffolder によって作成されたフォルダーが入門のコードを含む GitHub から複製されたメイン リポジトリ外 live する必要があります。 

1. Node.js コマンド ライン エクスペリエンスを使用して新しいフォルダーに移動します。 そのフォルダーで、次のコマンドを実行して新しいローカル Git リポジトリを作成します。 

        git init

    このコマンドでローカル Git リポジトリが作成され、新しいリポジトリが初期化されたという確認メッセージが表示されます。 

    ![新しいローカル Git リポジトリ](media/app-service-api-nodejs-api-app/new-local-git-repo.png)

1. Node.js コマンド ライン エクスペリエンスを使用して次のコマンドを実行します。Git リモートがローカル リポジトリに追加されます。 リモート リポジトリは、先ほど作成し、Azure で実行されている API アプリと関連付けられているものです。 

        git remote add azure YOUR_GIT_CLONE_URL_HERE

    **注**: 上記の"YOUR_GIT_CLONE_URL_HERE"文字列を先ほどコピーした、独自の Git クローン URL に置き換えるでしょう。 

1. 次に、Node.js コマンド ライン エクスペリエンスから以下の 2 つのコマンドを実行します。 

        git add .
        git commit -m "initial revision"

    2 つのコマンドを完了すると、コマンド ライン ウィンドウに次のスクリーンショットのような内容が表示されます。 

    ![Git Commit の出力](media/app-service-api-nodejs-api-app/git-commit-output.png)

1. コードを Azure にプッシュして、API アプリへのデプロイメントをトリガーするには、Node.js コマンドラインで次のコマンドを実行します。 パスワードの入力を求められたら、Azure ポータルでデプロイメント資格情報を作成するときに使用したパスワードを入力します。 

        git push azure master

1. 戻る場合、 **継続的なデプロイ** API アプリのブレードで、デプロイが行われている表示されます。 

    ![デプロイメントの実行中](media/app-service-api-nodejs-api-app/deployment-happening.png)

    同時に、Node.js コマンド ラインにも実行中のデプロイメントの状態が反映されます。 

    ![Node Js デプロイメントの実行中](media/app-service-api-nodejs-api-app/node-js-deployment-happening.png)

1. 展開の完了、 **継続的なデプロイ** ブレードは、API アプリにコードの変更が正常に展開を反映します。 コピー、 **URL** で、 **Essentials** API アプリのブレードのセクションです。 

    ![デプロイメントの完了](media/app-service-api-nodejs-api-app/deployment-completed.png)

1. API 呼び出し、対象となる相手の URL を指定 Postman や Fiddler (または、web ブラウザー) などのクライアントは REST API を使用して、 **連絡先/** API アプリのエンドポイント。 

    **注:** URL は http://myapiapp.azurewebsites.net/contacts のようになります

    このエンドポイントに GET 要求を発行すると、API アプリの JSON 出力が表示されます。

    ![Postman に接続する Api](media/app-service-api-nodejs-api-app/postman-hitting-api.png)

## 次のステップ

ここでは、Node.js を使用して、初めての API アプリを作成してデプロイしました。 API アプリの取得中に、次のチュートリアル シリーズの番組をどのように開始する [CORS を使用して、JavaScript クライアントから API アプリを使用する](app-service-api-cors-consume-javascript.md)です。

このサンプルを応用すれば、ハンドラーにコードを追加して、データベースや、API アプリ インスタンスのディスクにデータを格納できるようになります。 [継続的なデプロイ] を有効にしたので、API アプリの機能を変更し、拡張する操作は、コードを変更して Git リポジトリにプッシュする操作と同じくらい簡単になります。 
