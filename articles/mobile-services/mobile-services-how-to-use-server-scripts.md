<properties 
    pageTitle="JavaScript バックエンド モバイル サービスを操作する" 
    description="Azure Mobile Services でサーバー スクリプトを定義、登録、使用する方法の例を提供します。" 
    services="mobile-services" 
    documentationCenter="" 
    authors="RickSaling" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="mobile-services" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="javascript" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="12/01/2015" 
    ms.author="ricksal"/>


# JavaScript バックエンド モバイル サービスを操作する

この記事では、Azure Mobile Services で JavaScript バックエンドを使用する方法についての詳細な情報と例を提供します。 

##<a name="intro"></a>はじめに

JavaScript バックエンド モバイル サービスを使用すると、サーバーに格納されて実行される JavaScript コードとして、カスタムのビジネス ロジックを定義できます。 このサーバー スクリプト コードは、次のサーバー機能のいずれかに割り当てられます。

+ [挿入、読み取り、更新、または削除操作、特定のテーブルに][Table operations]します。
+ [スケジュールされたジョブ][Job Scheduler]します。
+ [カスタム API で定義された HTTP メソッド][Custom API anchor]します。 

サーバー スクリプトのメイン関数のシグネチャは、スクリプトが使用されるコンテキストによって異なります。 スクリプト間で共有される共通スクリプト コードを nodes.js モジュールとして定義することもできます。 詳細については、次を参照してください。 [ソース管理と共有コード][Source control, shared code, and helper functions]します。

個々 のサーバー スクリプト オブジェクトおよび関数の説明については、次を参照してください。 [Mobile Services server script reference]します。 


##<a name="table-scripts"></a>テーブル操作

テーブル操作スクリプトは、テーブルに対する操作に登録するサーバー スクリプト挿入、読み取り、更新、または削除 (*del*)。 このセクションでは、JavaScript バックエンドでテーブルを操作する方法について説明します。次のセクションが含まれています。

+ [テーブル操作の概要][Basic table operations]
+ [方法: テーブル操作に登録する]
+ [方法: 既定の応答をオーバーライドする]
+ [方法: execute success をオーバーライドする]
+ [方法: 既定のエラー処理をオーバーライドする]
+ [方法: 一意の ID 値を生成する](#generate-guids)
+ [方法: カスタム パラメーターを追加する]
+ [方法: テーブルのユーザーを処理する][How to: Work with users]

###<a name="basic-table-ops"></a>テーブル操作の概要

スクリプトの名前は、登録されている操作の種類と一致する必要があります。 1 つのテーブル操作に対して登録できるスクリプトは 1 つだけです。 REST 要求によって特定の操作が呼び出されるたびに、スクリプトが実行されます。たとえば、テーブルに項目を挿入する POST 要求を受け取ると、そのたびに特定のスクリプトが実行されます。 Mobile Services では、スクリプトの実行間で状態は保持されません。 スクリプトが実行されるたびに新しいグローバル コンテキストが作成されるため、スクリプトで定義されている状態変数がすべて再初期化されます。 要求間で状態を保存する必要がある場合は、モバイル サービス上でテーブルを作成し、そのテーブルに対して状態を読み書きします。 詳細については、次を参照してください。 [How to: Access tables from scripts]します。

操作が実行されたときにカスタマイズされたビジネス ロジックを強制的に実行する必要がある場合は、テーブル操作スクリプトを作成します。 たとえば、次のスクリプトでは、`text` フィールドの文字列の長さが 10 文字を超える挿入操作を拒否します。 

    function insert(item, user, request) {
        if (item.text.length > 10) {
            request.respond(statusCodes.BAD_REQUEST, 
                'Text length must be less than 10 characters');
        } else {
            request.execute();
        }
    }

テーブル スクリプト関数は、常に 3 つの引数を受け取ります。

- 最初の引数は、テーブル操作によって異なります。 

    - 挿入と更新では、 **項目** 操作の影響を受ける行の JSON 表現であるオブジェクト。 これにより、たとえば、名前で列の値にアクセスする *項目。所有者*, ここで、 *所有者* JSON 表現で名前の 1 つです。
    - 削除では、削除するレコードの ID です。 
    - 読み取りでは、 [query object] 返される行セットを指定します。

- 2 番目の引数は常に、 [ユーザー オブジェクト][User object] 要求を送信したユーザーを表します。 

- 3 番目の引数は常に、 [request オブジェクト][request object], 、要求された操作と、クライアントに送信される応答の実行を制御できます。

テーブル操作の正規のメイン関数署名は次のとおりです。 

+ [挿入][insert function]: `function insert (item, user, request) { ... }`
+ [更新プログラム][update function]: `function update (item, user, request) { ... }`
+ [削除][delete function]: `function del (id, user, request) { ... }`
+ [読み取り][read function]: `function read (query, user, request) { ... }`

>[AZURE.NOTE]削除操作に登録されている関数に指定する必要があります _del_ delete は JavaScript の予約されたキーワードであるためです。 

サーバー スクリプトには必ずメインの関数が含まれ、必要に応じて省略可能なヘルパー関数が使用されます。 サーバー スクリプトが特定のテーブル用に作成されていても、同じデータベース内の他のテーブルも参照することができます。 スクリプト間で共有できる一般的な関数をモジュールとして定義することもできます。 詳細については、次を参照してください。 [ソース管理と共有コード][Source control, shared code, and helper functions]します。

###<a name="register-table-scripts"></a>方法: テーブル スクリプトを登録する

次のいずれかの方法で、テーブル操作に登録されるサーバー スクリプトを定義できます。

+  [Azure classic portal]します。 テーブル操作にアクセス用のスクリプト、 **スクリプト** 、特定のテーブル] タブをクリックします。 `TodoItem` テーブルの挿入スクリプトに登録されている既定のコードを次に示します。 このコードを独自のカスタム ビジネス ロジックでオーバーライドできます。

    ![1][1]
    
    これを行う方法については、次を参照してください。 [Validate and modify data in Mobile Services by using server scripts]します。  

+ ソース管理を使用して。 ソース管理を有効にしている場合は、単に git リポジトリの .\service\table サブフォルダーに <em>`<table>`</em>.<em>`<operation>`</em>.js という名前のファイルを作成します。ここで、<em>`<table>`</em> はテーブルの名前であり、<em>`<operation>`</em> は登録するテーブル操作です。 詳細については、次を参照してください。 [ソース管理と共有コード][Source control, shared code, and helper functions]します。

+ コマンド プロンプトから Azure コマンド ライン ツールを使用して。 詳細については、次を参照してください。 [Using the command line tool]します。


テーブル操作スクリプトは、少なくとも 1 つの関数を呼び出す必要があります、 [request object] 、クライアントが応答を受信するかどうかを確認します。 
 
+ **execute 関数**: 要求された操作が完了し、標準の応答が返されます。
 
+ **respond 関数**: カスタム応答が返されます。

> [AZURE.IMPORTANT] ときに、スクリプトが、コード パスをどれも **実行** も **応答** が呼び出されると、操作が応答しなくなります。

次のスクリプトを呼び出し、 **実行** 関数が、クライアントによって要求されたデータ操作を完了します。 

    function insert(item, user, request) { 
        request.execute(); 
    }

この例では、データベースに項目を挿入し、適切な状態コードをユーザーに返します。 

ときに、 **実行** 関数が呼び出されると、 `item`, 、[クエリ][query object], 、または `id` スクリプト関数に最初の引数として渡された値を使用して、操作を実行します。 Insert、update、またはクエリ操作の場合、項目を変更または照会を呼び出す前に **実行**: 

    function insert(item, user, request) { 
        item.scriptComment =
            'this was added by a script and will be saved to the database'; 
        request.execute(); 
    } 
 
    function update(item, user, request) { 
        item.scriptComment = 
            'this was added by a script and will be saved to the database'; 
        request.execute(); 
    } 

    function read(query, user, request) { 
        // Only return records for the current user         
        query.where({ userid: user.userId}); 
        request.execute(); 
    }
 
>[AZURE.NOTE]削除スクリプトで、削除される変更の指定した userId 変数の値ではどのレコードには影響しません。

例については、次を参照してください。 [Read and write data], 、[Modify the request] と [Validate data]します。


###<a name="override-response"></a>方法: 既定の応答をオーバーライドする

スクリプトを使用して、既定の応答動作をオーバーライドできる検証ロジックを実装することもできます。 検証が失敗した場合を呼び出すだけ、 **応答** 関数の代わりに、 **実行** 機能し、応答をクライアントに書き込みます。 

    function insert(item, user, request) {
        if (item.userId !== user.userId) {
            request.respond(statusCodes.FORBIDDEN, 
            'You may only insert records with your userId.');
        } else {
            request.execute();
        }
    }

挿入された項目が持っていない場合にこの例では、要求が拒否されます、 `userId` に一致するプロパティ、 `userId` の [user object] 、認証されたクライアントに関連して渡されました。 この場合、データベース操作で (*挿入*) は実行されず、403 HTTP ステータス コードとカスタム エラー メッセージを持つ応答がクライアントに返されるとします。 例については、次を参照してください。 [Modify the response]します。

###<a name="override-success"></a>方法: execute success をオーバーライドする

既定ではテーブルの操作で、 **実行** 関数は、応答を自動的に書き込みます。 ただし、execute 関数に、成功および失敗時の動作をオーバーライドする 2 つのオプション パラメーターを渡すことができます。

渡すことによって、 **成功** を呼び出すときにハンドラーの実行、応答に記述する前に、クエリの結果を変更することができます。 次の例は、`execute({ success: function(results) { ... })` を呼び出すことで、データをデータベースから読み取ってから応答を書き込むまでの間に、追加の作業を実行します。

    function read(query, user, request) {
        request.execute({
            success: function(results) {
                results.forEach(function(r) {
                    r.scriptComment = 
                    'this was added by a script after querying the database';
                });
                request.respond();
            }
        });
    }

指定すると、 **成功** ハンドラーを **実行** 関数も呼び出す必要があります、 **応答** の一部として動作、 **成功** ハンドラー、ランタイムは、スクリプトが完了したことと、応答を書き込むことが認識できるようにします。 呼び出すと **応答** 引数を渡さず、モバイル サービスには、既定の応答が生成されます。 

>[AZURE.NOTE]呼び出すことができます **応答** を最初に呼び出した後でのみ、既定の応答を呼び出す引数を使わず、 **実行** 関数です。
 
###<a name="override-error"></a>方法: 既定のエラー処理をオーバーライドする

 **実行** 関数は、データベース、オブジェクトが無効または正しくないクエリへの接続の損失がある場合に失敗することができます。 エラーが発生すると、サーバー スクリプトは既定でエラーをログに記録し、エラー結果を応答に書き込みます。 Mobile Services には既定のエラー処理が用意されているため、サービスで発生する可能性のあるそうしたエラーを処理する必要はありません。 

特定の修正操作を実行する場合や、グローバル console オブジェクトを使用して詳細な情報をログに書き込む場合は、明示的なエラー処理を実装して、既定のエラー処理をオーバーライドすることができます。 こうと指定することによって、 **エラー** ハンドラーを **実行** 関数。

    function update(item, user, request) { 
      request.execute({ 
        error: function(err) { 
          // Do some custom logging, then call respond. 
          request.respond(); 
        } 
      }); 
    }
 

モバイル サービスがクライアントに、エラーの結果を返すエラー ハンドラーを提供する場合と **応答** が呼び出されます。

両方を提供することも、 **成功** と **エラー** ハンドラーしたい場合。

###<a name="generate-guids"></a>方法: 一意の ID 値を生成する

モバイル サービス テーブルの一意のカスタム文字列値をサポートしている **id** 列です。 このため、アプリケーションは、ID にメール アドレスやユーザー名などのカスタム値を使用できます。 

文字列 ID には、次のような利点があります。

+ データベースと往復することなく ID が生成されます。
+ 他のテーブルやデータベースのレコードをより簡単にマージできます。
+ ID 値をより適切にアプリケーションのロジックに統合できます。

挿入されたレコードで文字列 ID 値が設定されない場合は、Mobile Services によって ID 用の一意の値が生成されます。 一意の ID 値はサーバー スクリプトで生成できます。 次のスクリプト例は、カスタム GUID を生成し、新しいレコードの ID に割り当てます。 これは、レコードの ID として値を渡さなかった場合に、Mobile Services によって生成される ID 値に似ています。

    // Example of generating an id. This is not required since Mobile Services
    // will generate an id if one is not passed in.
    item.id = item.id || newGuid();
    request.execute();

    function newGuid() {
        var pad4 = function(str) { return "0000".substring(str.length) + str; };
        var hex4 = function () { return pad4(Math.floor(Math.random() * 0x10000 /* 65536 */ ).toString(16)); };
        return (hex4() + hex4() + "-" + hex4() + "-" + hex4() + "-" + hex4() + "-" + hex4() + hex4() + hex4());
    }


アプリケーションが ID の値を指定すると、Mobile Services はそれをそのまま格納します。 これには、前後の空白文字も含まれます。 値から空白文字が除去されることはありません。

`id` の値は一意である必要があり、次のセット内の文字を含まないようにする必要があります。

+ 制御文字: [0x0000-0x001F] および [0x007F-0x009F]。 詳細については、次を参照してください。 [ASCII 制御コード C0 および C1](http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set)します。
+  印刷可能文字: **"**(0x0022)、 **\ +** (0x002B)、 **/** (0x002F)、 **でしょうか。** (0x003F)、 **\\** (0x005C)、 **'** (0x0060)
+  ID "." および ".."

また、テーブルに整数 ID を使用することもできます。 整数 ID を使用するには、`mobile table create` コマンドで `--integerId` オプションを使用してテーブルを作成する必要があります。 このコマンドは、Azure のコマンド ライン インターフェイス (CLI) で使用されます。 CLI の使用の詳細については、次を参照してください。 [モバイル サービス テーブルの管理用コマンド](../virtual-machines-command-line-tools.md#Mobile_Tables)します。


###<a name="access-headers"></a>方法: カスタム パラメーターへのアクセスに関するページ

モバイル サービスに要求を送信する際に、要求の URI に 1 つ以上のカスタム パラメーターを含めることで、特定の要求の処理方法をテーブル操作スクリプトに指定できます。 その後、処理パスを判断するためにパラメーターを調べるようにスクリプトを変更します。

たとえば、POST 要求の次の URI が新しいの挿入を許可しない場合、サービスを示します *TodoItem* 同じテキスト値を設定しています。

        https://todolist.azure-mobile.net/tables/TodoItem?duplicateText=false

これらのカスタム クエリ パラメーターから JSON 値としてアクセスされる、 **パラメーター** のプロパティ、 [request object]します。  **要求** オブジェクトは、テーブル操作に登録されている関数にモバイル サービスによって提供されます。 次の挿入操作のサーバー スクリプトでは、挿入操作を実行する前に、`duplicateText` パラメーターの値を確認します。

        function insert(item, user, request) {
            var todoItemTable = tables.getTable('TodoItem');
            // Check the supplied custom parameter to see if
            // we should allow duplicate text items to be inserted.        
            if (request.parameters.duplicateText === 'false') {
                // Find all existing items with the same text
                // and that are not marked 'complete'. 
                todoItemTable.where({
                    text: item.text,
                    complete: false
                }).read({
                    success: insertItemIfNotComplete
                });
            } else {
                request.execute();
            }

            function insertItemIfNotComplete(existingItems) {
                if (existingItems.length > 0) {
                    request.respond(statusCodes.CONFLICT, 
                        "Duplicate items are not allowed.");
                } else {
                    // Insert the item as normal. 
                    request.execute();
                }
            }
        }

注意してください **insertItemIfNotComplete** 、 **実行** の関数、 [request object] 重複するテキストがない場合、項目を挿入する。 それ以外の場合は、 **応答** 、重複についてクライアントに通知する関数が呼び出されます。 

呼び出しの構文に注意してください、 **成功** 上記のコードで機能します。

                }).read({
                    success: insertItemIfNotComplete
                });

JavaScript では、これは次のようなより長い構文の短縮版です。 

        success: function(results) 
        { 
            insertItemIfNotComplete(results); 
        }


###<a name="work-with-users"></a>方法: ユーザーを処理する

Azure Mobile Services では、ID プロバイダーを使用して、ユーザーを認証できます。 詳細については、次を参照してください。 [Get started with authentication]します。 モバイル サービスを使用して、認証されたユーザーがテーブル操作を呼び出したとき、 [user object] 登録されたスクリプト関数にユーザーに関する情報を提供します。  **UserId** 格納およびユーザーに固有の情報を取得するプロパティを使用できます。 次の例に基づいて、item の owner プロパティを設定する、 **userId** 認証されたユーザーの。

    function insert(item, user, request) {
        item.owner = user.userId;
        request.execute();
    }

次の例に基づいて、query にフィルターを追加する、 **userId** 認証されたユーザーのです。 このフィルターでは、結果が現在のユーザーに属する項目のみに制限されます。  

    function read(query, user, request) {
        query.where({
            owner: user.userId
        });
        request.execute();
    }

##<a name="custom-api"></a>カスタム API

このセクションでは、カスタム API エンドポイントを作成して操作する方法について説明します。次のセクションが含まれています。 
    
+ [カスタム API の概要](#custom-api-overview)
+ [方法: カスタム API を定義する]
+ [方法: HTTP メソッドを実装する]
+ [方法: データを XML として送受信する]
+ [方法: カスタム API でユーザーとヘッダーを操作する]
+ [方法: カスタム API で複数のルートを定義する]

###<a name="custom-api-overview"></a>カスタム API の概要

カスタム API は、GET、POST、PUT、PATCH、DELETE という 1 つ以上の標準 HTTP メソッドによってアクセスされる、モバイル サービスのエンドポイントです。 カスタム API によってサポートされている各 HTTP メソッドに個別の関数 export を定義し、すべてを 1 つのスクリプト ファイルに含めることができます。 特定のメソッドを使用するカスタム API への要求が受信されると、登録されたスクリプトが呼び出されます。 詳細については、次を参照してください。 [Custom API]します。

カスタム API の関数がモバイル サービス ランタイムによって呼び出されたときに両方の [要求][request object] と [応答][response object] オブジェクトを指定します。 これらのオブジェクトの機能を公開する、 [express.js library], 、スクリプトで利用できます。 という名前の次のカスタム API **こんにちは** を返す非常に単純な例は、 _こんにちは, world!_ POST 要求に応答します。

        exports.post = function(request, response) {
            response.send(200, "{ message: 'Hello, world!' }");
        } 

 **送信** で機能、 [response object] クライアントに、指定された応答を返します。 このコードは、次の URL への POST 要求が送信されたときに呼び出されます。

        https://todolist.azure-mobile.net/api/hello  

グローバル状態は実行間で維持されます。 

###<a name="define-custom-api"></a>方法: カスタム API を定義する

次のいずれかの方法で、カスタム API エンドポイントの HTTP メソッドに登録されるサーバー スクリプトを定義できます。

+  [Azure classic portal]します。 カスタム API スクリプトは作成され、変更、 **API** ] タブをクリックします。 サーバー スクリプト コードは、 **スクリプト** 、特定のカスタム API] タブをクリックします。 次の図は、`CompleteAll` というカスタム API エンドポイントへの POST 要求によって呼び出されるスクリプトを示しています。 

    ![2][2]
    
    カスタム API メソッドに対するアクセス許可は、[アクセス許可] タブで割り当てます。 このカスタム API の作成方法を参照してください [Call a custom API from the client]します。  

+ ソース管理を使用して。 ソース管理を有効にしている場合は、単に git リポジトリの .\service\api サブフォルダーに <em>`<custom_api>`</em>.js という名前のファイルを作成します。ここで、<em>`<custom_api>`</em> は登録するカスタム API の名前です。 このスクリプト ファイルを含む、 _エクスポート_ 関数、カスタム API によって公開されている各 HTTP メソッド。 アクセス許可は、付属 .json ファイルで定義されます。 詳細については、次を参照してください。 [ソース管理と共有コード][Source control, shared code, and helper functions]します。

+ コマンド プロンプトから Azure コマンド ライン ツールを使用して。 詳細については、次を参照してください。 [Using the command line tool]します。

###<a name="handle-methods"></a>方法: HTTP メソッドを実装する

カスタム API は、1 つ以上の HTTP メソッド (GET、POST、PUT、PATCH、および DELETE) を処理できます。 カスタム API によって処理される HTTP メソッドごとに、エクスポートされる関数が定義されます。 1 つのカスタム API コード ファイルは、次の関数の 1 つまたはすべてをエクスポートできます。

        exports.get = function(request, response) { ... };
        exports.post = function(request, response) { ... };
        exports.patch = function(request, response) { ... };
        exports.put = function(request, response) { ... };
        exports.delete = function(request, response) { ... };

カスタム API エンドポイントは、サーバー スクリプトで実装されていない HTTP メソッドを使用して呼び出すことはできず、405 (許可されていないメソッド) エラー応答が返されます。 各サポート HTTP メソッドに個別のアクセス許可レベルを割り当てることができます。

###<a name="api-return-xml"></a>方法: データを XML として送受信する

クライアントがデータを格納および取得するときに、Mobile Services は JavaScript Object Notation (JSON) を使用してメッセージ本体のデータを表現します。 ただし、そうするのではなく、XML ペイロードを使用したいシナリオもあります。 たとえば、Windows ストア アプリには、サービスに XML の発行を要求する、組み込みの定期的な通知機能があります。 詳細については、次を参照してください。 [Define a custom API that supports periodic notifications]します。

次 **OrderPizza** カスタム API 関数は、応答ペイロードとして単純な XML ドキュメントを返します。

        exports.get = function(request, response) {
          response.set('content-type', 'application/xml');
          var xml = '<?xml version="1.0"?><PizzaOrderForm><PizzaOrderForm/>';
          response.send(200, xml);
        };

このカスタム API 関数は、次のエンドポイントへの HTTP GET 要求によって呼び出されます。

        https://todolist.azure-mobile.net/api/orderpizza

###<a name="get-api-user"></a>方法: カスタム API でユーザーとヘッダーを操作する

Azure Mobile Services では、ID プロバイダーを使用して、ユーザーを認証できます。 詳細については、次を参照してください。 [Get started with authentication]します。 モバイル サービスを使用して、認証されたユーザーがカスタム API を要求すると、 [user object] カスタム API コードにユーザーに関する情報を提供します。  [user object] の user プロパティからアクセス、 [request object]します。  **UserId** 格納およびユーザーに固有の情報を取得するプロパティを使用できます。 

次 **OrderPizza** カスタム API 関数に基づいて、item の owner プロパティを設定する、 **userId** 認証されたユーザーの。

        exports.post = function(request, response) {
            var userTable = request.service.tables.getTable('user');
            userTable.lookup(request.user.userId, {
                success: function(userRecord) {
                    callPizzaAPI(userRecord, request.body, function(orderResult) {
                        response.send(201, orderResult);
                    });
                }
            });
        
        };

このカスタム API 関数は、次のエンドポイントへの HTTP POST 要求によって呼び出されます。

        https://<service>.azure-mobile.net/api/orderpizza

特定の HTTP ヘッダーにアクセスすることも、 [request object], 、次のコードに示すようにします。

        exports.get = function(request, response) {    
            var header = request.header('my-custom-header');
            response.send(200, "You sent: " + header);
        };

この単純な例は、`my-custom-header` という名前のカスタム ヘッダーを読み取り、応答内で値を返します。

###<a name="api-routes"></a>方法: カスタム API で複数のルートを定義する

Mobile Services では、カスタム API 内で複数のパス (ルート) を定義できます。 たとえばのでは、次の Url を HTTP GET 要求、 **電卓** カスタム API を呼び出す、 **追加** または **減算** 関数をそれぞれ。 

+ `https://<service>.azure-mobile.net/api/calculator/add`
+ `https://<service>.azure-mobile.net/api/calculator/sub`

エクスポートすることによって複数のルートが定義されている、 **登録** 、渡される関数に、 **api** オブジェクト (のような [express object in express.js]) ルートをカスタム API エンドポイントを登録するためです。 次の例では、実装、 **追加** と **sub** 内のメソッド、 **電卓** カスタム API: 

        exports.register = function (api) {
            api.get('add', add);
            api.get('sub', subtract);
        }
        
        function add(req, res) {
            var result = parseInt(req.query.a) + parseInt(req.query.b);
            res.send(200, { result: result });
        }
        
        function subtract(req, res) {
            var result = parseInt(req.query.a) - parseInt(req.query.b);
            res.send(200, { result: result });
        }

 **Api** オブジェクトに渡される、 **登録** 関数は、各 HTTP メソッドの関数を公開 (**取得**, 、**投稿**, 、**配置**, 、**修正プログラム**, 、**削除**)。 これらの関数は、特定の HTTP メソッド用に定義された関数にルートを登録します。 各関数は 2 つのパラメーターを受け取ります。最初のパラメーターはルート名で、2 つ目のパラメーターはルートに登録される関数です。 

前のカスタム API の例での 2 つのルートは、次のような HTTP GET 要求で呼び出すことができます (応答と共に示します)。

+ `https://<service>.azure-mobile.net/api/calculator/add?a=1&b=2`

        {"result":3}

+ `https://<service>.azure-mobile.net/api/calculator/sub?a=3&b=5`

        {"result":-2}

##<a name="scheduler-scripts"></a>ジョブ スケジューラ

Mobile Services を使用すると、決まったスケジュールでのジョブまたは Azure クラシック ポータルからのオンデマンドとして実行できるサーバー スクリプトを定義できます。 スケジュールされたジョブは、テーブル データのクリーンアップやバッチ処理のような定期的なタスクを実行する場合に便利です。 詳細については、次を参照してください。 [Schedule jobs]します。

スケジュールされたジョブに登録されているスクリプトには、スケジュールされたジョブと同じ名前のメイン関数があります。 スケジュールされたスクリプトは HTTP 要求によって呼び出されないため、サーバー ランタイムによって渡すことができるコンテキストがなく、関数はパラメーターを受け取りません。 他の種類のスクリプトと同様に、サブルーティン関数を使用したり、共有モジュールを要求したりすることができます。 詳細については、次を参照してください。 [Source control, shared code, and helper functions]します。

###<a name="scheduler-scripts"></a>方法: スケジュールされたジョブ スクリプトを定義する

サーバー スクリプトは、Mobile Services Scheduler で定義されたジョブに割り当てることができます。 これらのスクリプトは、ジョブに属し、ジョブ スケジュールに従って実行されます (使用することも、 [Azure classic portal] 要求時にジョブを実行します。)スケジュールされたジョブを定義するスクリプトでは、Mobile Services からデータが渡されないため、パラメーターはありません。このようなスクリプトは通常の JavaScript 関数として実行され、Mobile Services と直接データをやり取りすることはありません。 

スケジュールされたジョブは、次のいずれかの方法で定義できます。 

+  [Azure classic portal] で、 **スクリプト** 、スケジューラ内でタブをクリックします。

    ![3][3]

    これを行う方法の詳細については、次を参照してください。 [Schedule backend jobs in Mobile Services]します。 

+ コマンド プロンプトから Azure コマンド ライン ツールを使用して。 詳細については、次を参照してください。 [Using the command line tool]します。

>[AZURE.NOTE]ソース管理を有効にした場合は、git リポジトリで .\service\scheduler サブフォルダーに直接スケジュールされたジョブのスクリプト ファイルを編集できます。 詳細については、次を参照してください。 [How to: Share code by using source control]します。

##<a name="shared-code"></a>ソース管理、共有コード、およびヘルパー関数

このセクションでは、ソース管理を活用して、カスタムの node.js モジュール、共有コードや他のコードの再利用計画を追加するする方法について説明します。次のセクションが含まれています。

+ [共有コードの活用の概要](#leverage-source-control)
+ [方法: Node.js モジュールを読み込む]
+ [方法: ヘルパー関数を使用する]
+ [方法: ソース管理を使用してコードを共有する]
+ [方法: アプリケーションの設定を操作する] 

###<a name="leverage-source-control"></a>共有コードの活用の概要

Mobile Services はサーバーの Node.js を使用するため、スクリプトは既に組み込みの Node.js モジュールへのアクセス許可を持っています。 ソース管理を使用して独自のモジュールを定義したり、他の Node.js モジュールをサービスに追加したりすることもできます。

次は、グローバルを使用して、スクリプトで利用できるより便利なモジュールのほんの一部 **必要** 関数。

+ **azure**: Node.js に対して Azure SDK の機能を公開します。 詳細については、次を参照してください。、 [Azure SDK for Node.js]します。 
+ **crypto**: OpenSSL の暗号化機能を提供します。 詳細については、次を参照してください。、 [Node.js に関するドキュメント][crypto API]します。
+ **パス**: ファイル パスを操作するためのユーティリティが含まれています。 詳細については、次を参照してください。、 [Node.js に関するドキュメント][path API]します。
+ **querystring**: クエリ文字列を操作するためのユーティリティが含まれています。 詳細については、次を参照してください。、 [Node.js に関するドキュメント][querystring API]します。
+ **要求**: Twitter や Facebook などの外部 REST サービスに HTTP 要求を送信します。 詳細については、次を参照してください。 [Send HTTP request]します。
+ **sendgrid**: Azure で Sendgrid 電子メール サービスを使用して、電子メールを送信します。 詳細については、次を参照してください。 [Send email from Mobile Services with SendGrid]します。
+ **url**: 解析し、Url を解決するためのユーティリティが含まれています。 詳細については、次を参照してください。、 [Node.js に関するドキュメント][url API]します。
+ **util**: 文字列の書式設定やオブジェクトの種類の確認など、さまざまなユーティリティが含まれています。 詳細については、次を参照してください。、 [Node.js に関するドキュメント][util API]します。 
+ **zlib**: gzip などの圧縮機能を公開し、縮小します。 詳細については、次を参照してください。、 [Node.js に関するドキュメント][zlib API]します。 

###<a name="modules-helper-functions"></a>方法: モジュールを利用する

モバイル サービスに公開する一連のスクリプトは、グローバルを使用して読み込むことができるモジュール **必要** 関数です。 たとえば、スクリプトを要求できます **要求** HTTP 要求を行います。 

    function update(item, user, request) { 
        var httpRequest = require('request'); 
        httpRequest('http://www.google.com', function(err, response, body) { 
            ... 
        }); 
    } 


###<a name="shared-code-source-control"></a>方法: ソース管理を使用してコードを共有する

ソース管理で Node.js のパッケージ マネージャー (npm) を使用して、モバイル サービスで利用できるモジュールを制御することができます。 この作業を実行する 2 つの方法があります。

+ npm によって発行およびインストールされるモジュールでは、package.json ファイルを使用して、モバイル サービスからインストールしようとするパッケージを宣言します。 この方法で、サービスは常に、必要なパッケージの最新バージョンにアクセスできます。 package.json ファイルは、`.\service` ディレクトリ内に存在しています。 詳細については、次を参照してください。 [Support for package.json in Azure Mobile Services]します。

+ プライベート モジュールまたはカスタム モジュールの場合は、npm を使用して、ソース管理の `.\service\node_modules` ディレクトリにモジュールを手動でインストールすることができます。 モジュールを手動でアップロードする方法の例は、次を参照してください。 [Leverage shared code and Node.js modules in your server scripts]します。

    >[AZURE.NOTE]ときに `node_modules` 既に存在するディレクトリ階層では、NPM の作成、 `\node-uuid` サブディレクトリが新しいを作成するのではなく `node_modules` リポジトリにします。 この場合、既存の `node_modules` ディレクトリを削除してください。

コミットした後、package.json ファイルまたはカスタム モジュールをリポジトリに、モバイル サービスを使用して **必要** 名前によって、モジュールを参照します。   

>[AZURE.NOTE] Package.json 内で指定するか、モバイル サービスにアップロードするモジュールは、サーバー スクリプト コードでのみ使用します。 これらのモジュールは、Mobile Services ランタイムによって使用されることはありません。

###<a name="helper-functions"></a>方法: ヘルパー関数を使用する

モジュールを要求する方法に加えて、個々のサーバー スクリプトにヘルパー関数を含めることもできます。 これらはメイン関数から分離された関数であり、スクリプト内のコードを分割するために使用できます。 

ヘルパー関数を含むの挿入操作に次の例では、テーブル スクリプトが登録されている **handleUnapprovedItem**:


    function insert(item, user, request) {
        if (!item.approved) {
            handleUnapprovedItem(item, user, request);
        } else {
            request.execute();
        }
    }
    
    function handleUnapprovedItem(item, user, request) {
        // Do something with the supplied item, user, or request objects.
    }
 
スクリプトでは、メインの関数の後にヘルパー関数を宣言する必要があります。 スクリプトでは、すべての変数を定義する必要があります。 宣言されていない変数があると、エラーが発生します。

ヘルパー関数は、1 回だけ定義して、複数のサーバー スクリプト間で共有することもできます。 スクリプト間で関数を共有するには、関数をエクスポートし、スクリプト ファイルを `.\service\shared\` ディレクトリに保存する必要があります。 次に示すのは、ファイル `.\services\shared\helpers.js` 内の共有関数をエクスポートするためのテンプレートです。

        exports.handleUnapprovedItem = function (tables, user, callback) {
            
            // Do something with the supplied tables or user objects and 
            // return a value to the callback function.
        };
 
その後は、テーブル操作スクリプトで次のようにして関数を使用することができます。

        function insert(item, user, request) {
            var helper = require('../shared/helper');
            helper.handleUnapprovedItem(tables, user, function(result) {
                    
                    // Do something based on the result.
                    request.execute();
                }
            }
        }

この例では、両方を渡す必要があります、 [tables object] と [user object] 共有関数にします。 これは、共有スクリプトをグローバルにアクセスできないため [tables object], 、および [user object] 要求のコンテキストでのみ存在します。

スクリプト ファイルを使用するか、共有ディレクトリにアップロードされます [ソース管理][How to: Share code by using source control] またはを使用して、 [コマンド ライン ツール][Using the command line tool]します。

###<a name="app-settings"></a>方法: アプリケーションの設定を操作する

Mobile Services を使用すると、値をアプリケーション設定として安全に格納できます。アプリケーション設定には、実行時にサーバー スクリプトからアクセスできます。  モバイル サービスのアプリケーション設定にデータを追加すると、名前/値ペアが暗号化されて格納され、サーバー スクリプトでそれらにアクセスできます。スクリプト ファイル内でそれらをハード コーディングする必要はありません。 詳細については、次を参照してください。 [App settings]します。

次のカスタム API の例を使用して、指定された [service object] アプリケーション設定値を取得します。  

        exports.get = function(request, response) {
        
            // Get the MY_CUSTOM_SETTING value from app settings.
            var customSetting = 
                request.service.config.appSettings.my_custom_setting;
                
            // Do something and then send a response.

        }

次のコードは構成モジュールを使用して、アプリケーション設定に格納されている Twitter アクセス トークン値を取得します。この値が、スケジュールされたジョブのスクリプトで使用されます。

        // Get the service configuration module.
        var config = require('mobileservice-config');

        // Get the stored Twitter consumer key and secret. 
        var consumerKey = config.twitterConsumerKey,
            consumerSecret = config.twitterConsumerSecret
        // Get the Twitter access token from app settings.    
        var accessToken= config.appSettings.TWITTER_ACCESS_TOKEN,
            accessTokenSecret = config.appSettings.TWITTER_ACCESS_TOKEN_SECRET;

このコードに格納されている Twitter コンシューマー キー値も取得、 **Identity** ポータル] タブをクリックします。  **Config オブジェクト** はテーブル操作とスケジュールされたジョブ スクリプトでは使用できない、する必要がありますが必要な設定にアクセスするアプリには構成モジュールです。 完全な例を参照してください。 [Schedule backend jobs in Mobile Services]します。

<h2><a name="command-prompt"></a>コマンド ライン ツールの使用</h2>

Mobile Services では、Azure コマンド ライン ツールを使用してサーバー スクリプトを作成、変更、および削除できます。 スクリプトをアップロードする前に、次のディレクトリ構造を使用していることを確認してください。

![4][4]

このディレクトリ構造は、ソース管理を使用する場合の git リポジトリと同じです。 

コマンド ライン ツールでスクリプト ファイルをアップロードする場合は、まず、`.\services\` ディレクトリに移動する必要があります。 次のコマンドは `table` サブディレクトリから `todoitem.insert.js` という名前のスクリプトをアップロードします。

        ~$azure mobile script upload todolist table/todoitem.insert.js
        info:    Executing command mobile script upload
        info:    mobile script upload command OK

次のコマンドは、モバイル サービスで管理されているすべてのスクリプト ファイルについての情報を返します。

        ~$ azure mobile script list todolist
        info:    Executing command mobile script list
        + Retrieving script information
        info:    Table scripts
        data:    Name                       Size
        data:    -------------------------  ----
        data:    table/channels.insert      1980
        data:    table/TodoItem.insert      5504
        data:    table/TodoItem.read        64
        info:    Shared scripts
        data:    Name              Size
        data:    ----------------  ----
        data:    shared/helper.js  62
        data:    shared/uuid.js    7452
        info:    Scheduled job scripts
        data:    Job name    Script name           Status    Interval     Last run  Next run
        data:    ----------  --------------------  --------  -----------  --------  --------
        data:    getUpdates  scheduler/getUpdates  disabled  15 [minute]  N/A       N/A
        info:    Custom API scripts
        data:    Name                    Get          Put          Post         Patch        Delete
        data:    ----------------------  -----------  -----------  -----------  -----------  -----------
        data:    completeall             application  application  application  application  application
        data:    register_notifications  application  application  user         application  application
        info:    mobile script list command OK

詳細については、次を参照してください。 [Commands to manage Azure Mobile Services]します。 

##<a name="working-with-tables"></a>テーブルの操作

このセクションでは、SQL Database テーブルのデータを直接操作する方法について説明します。次のセクションが含まれています。

+ [テーブルの操作の概要](#overview-tables)
+ [方法: スクリプトからテーブルにアクセスする]
+ [方法: 一括挿入を実行する]
+ [方法: JSON 型をデータベース型にマッピングする]
+ [テーブルにアクセスするための Transact-SQL の使用]

###<a name="overview-tables"></a>テーブルの操作の概要

Mobile Services の多くのシナリオでは、サーバー スクリプトがデータベース内のテーブルにアクセスする必要があります。 たとえば次のようになります。 モバイル サービスがスクリプトの実行間で状態を保持しないために、スクリプトの実行間で永続化する必要があるすべてのデータはテーブルに格納する必要があります。 アクセス許可テーブルのエントリを調べたり、監査データを単にログに書き込むだけでなくテーブルに保存したりする場合もあります (ログではデータの保存期間に限定があり、プログラムでアクセスすることができません)。 

モバイル サービスが、テーブルにアクセスする 2 つの方法を使用するか、 [table object] プロキシまたは Transact SQL を構成することでクエリを使用して、 [mssql object]します。  [table object] でを簡単にテーブル データにアクセスするとサーバー スクリプト コードからも、 [mssql object] より複雑なデータ操作をサポートし、最大の柔軟性を提供します。 

###<a name="access-tables"></a>方法: スクリプトからテーブルにアクセスする

使用して、スクリプトからテーブルにアクセスする最も簡単な方法は、 [tables object]します。  **GetTable** 関数が返される、 [table object] 、要求されたテーブルにアクセスするためのプロキシであるインスタンスです。 その後、プロキシで関数を呼び出すことで、データにアクセスして変更できます。 

テーブル操作とスケジュールされたジョブに登録されたスクリプトがアクセスできる、 [tables object] 、グローバル オブジェクトとして。 次のコード用のプロキシを取得する、 *TodoItems* グローバルからテーブル [tables object]: 

        var todoItemsTable = tables.getTable('TodoItems');

カスタム API スクリプトにアクセスできる、 [tables object] から、 <strong>サービス</strong> 、指定されたプロパティ [request object]します。 次のコードを取得 [tables object] 要求から。

        var todoItemsTable = request.service.tables.getTable('TodoItem');

> [AZURE.NOTE] 共有関数にアクセスできません、 **テーブル** オブジェクトに直接します。 共有関数内で tables オブジェクトを関数に渡す必要があります。

作成したら、 [table object], 、1 つまたは複数のテーブル操作関数を呼び出すことができます。 挿入、更新、削除または読み取る。 次の例では、permissions テーブルからユーザーのアクセス許可を読み取ります。

    function insert(item, user, request) {
        var permissionsTable = tables.getTable('permissions');
    
        permissionsTable
            .where({ userId: user.userId, permission: 'submit order'})
            .read({ success: checkPermissions });
            
        function checkPermissions(results) {
            if(results.length > 0) {
                // Permission record was found. Continue normal execution.
                request.execute();
            } else {
                console.log('User %s attempted to submit an order without permissions.', user.userId);
                request.respond(statusCodes.FORBIDDEN, 'You do not have permission to submit orders.');
            }
        }
    }

次の例では、監査情報を **監査** テーブル。

    function update(item, user, request) {
        request.execute({ success: insertAuditEntry });
        
        function insertAuditEntry() {
            var auditTable = tables.getTable('audit');
            var audit = {
                record: 'checkins',
                recordId: item.id,
                timestamp: new Date(),
                values: JSON.stringify(item)
            };
            auditTable.insert(audit, {
                success: function() {
                    // Write to the response now that all data operations are complete
                    request.respond();
                }
            });
        }
    }

最後の例は、ここで、コード サンプルには: [方法: カスタム パラメーターにアクセス][How to: Add custom parameters]します。

###<a name="bulk-inserts"></a>方法: 一括挿入を実行する

使用する場合、 **の** または **ながら** ループのテーブルに項目 (1,000 個など) の数が多いを直接挿入する、SQL 接続の制限に達して一部の挿入に失敗が発生する可能性があります。 要求が完了しないか、HTTP 500 内部サーバー エラーが返されます。  この問題を回避するには、一度に 10 個程度の項目をバッチとして挿入します。 1 つのバッチが挿入されるまで待ってから、次のバッチを挿入するようにします。

次のスクリプトを使用すると、レコードのバッチのサイズを設定して、並列挿入することができます。 レコードの数は大きくしないことをお勧めします。 関数 **insertItems** 再帰的な非同期挿入バッチが完了した自体を呼び出します。 最後にループでは、時刻、および呼び出しに 1 つのレコードを挿入の **insertComplete** 成功した場合と **errorHandler** エラーが発生します。 **insertComplete**  コントロールかどうか **insertItems** 再帰的に、次のバッチで呼び出されるかどうかまたは、ジョブを完了して、スクリプトが終了する必要があります。

        var todoTable = tables.getTable('TodoItem');
        var recordsToInsert = 1000;
        var batchSize = 10; 
        var totalCount = 0;
        var errorCount = 0; 
        
        function insertItems() {        
            var batchCompletedCount = 0;  
        
            var insertComplete = function() { 
                batchCompletedCount++; 
                totalCount++; 
                if(batchCompletedCount === batchSize || totalCount === recordsToInsert) {                        
                    if(totalCount < recordsToInsert) {
                        // kick off the next batch 
                        insertItems(); 
                    } else { 
                        // or we are done, report the status of the job 
                        // to the log and don't do any more processing 
                        console.log("Insert complete. %d Records processed. There were %d errors.", totalCount, errorCount); 
                    } 
                } 
            }; 
        
            var errorHandler = function(err) { 
                errorCount++; 
                console.warn("Ignoring insert failure as part of batch.", err); 
                insertComplete(); 
            };
        
            for(var i = 0; i < batchSize; i++) { 
                var item = { text: "This is item number: " + totalCount + i }; 
                todoTable.insert(item, { 
                    success: insertComplete, 
                    error: errorHandler 
                }); 
            } 
        } 
        
        insertItems(); 


完全なコード例と詳細な説明についてはこれに記載する [ブログの記事](http://blogs.msdn.com/b/jpsanders/archive/2013/03/20/server-script-to-insert-table-items-in-windows-azure-mobile-services.aspx)します。 このコードを使用する際には、使用する状況に合わせて修正したり、徹底的にテストしたりできます。

###<a name="JSON-types"></a>方法: JSON 型をデータベース型にマッピングする

クライアントとMobile Services データベース テーブルでは、データ型のコレクションが異なります。 データ型を簡単にマッピングできる場合もあれば、難しい場合もあります。 Mobile Services では、マッピングによってさまざまな型変換を実行します。

- クライアントの言語固有の型は JSON にシリアル化されます。
- JSON の表現は JavaScript に変換された後でサーバー スクリプトに使用されます。
- JavaScript のデータ型は SQL データベースの型を使用して保存されるときに変換、 [tables object]します。

クライアント スキーマから JSON への変換方法は、プラットフォームによって異なります。  Windows ストア クライアントと Windows Phone クライアントでは、JSON.NET が使用されます。 Android クライアントでは gson ライブラリが使用されます。  iOS クライアントでは NSJSONSerialization クラスが使用されます。 このようなライブラリでは既定のシリアル化動作が使用されます。ただし、日付のオブジェクトを、ISO 8601 を使用してエンコードされた日付が含まれている JSON 文字列に変換する場合は例外です。

使用してサーバー スクリプトを作成する場合 [insert], 、[update], 、[read] または [delete] 関数、データの JavaScript 表現にアクセスすることができます。 モバイル サービスは Node.js の逆シリアル化関数を使用して ([JSON.parse](http://es5.github.io/#x15.12)) に、ネットワーク上で JSON を JavaScript オブジェクトに変換します。 ただし、モバイル サービスが抽出する変換 **日付** ISO 8601 文字列からオブジェクトです。

使用すると、 [tables object] または [mssql object], 、または単純にテーブル スクリプトを実行、逆シリアル化された JavaScript オブジェクトが、SQL データベースに挿入します。 このプロセスでは、オブジェクトのプロパティが T-SQL 型にマッピングされます。

JavaScript のプロパティ|T-SQL 型
---|---
Number|Float(53)
Boolean|Bit
Date|DateTimeOffset(3)|
String|Nvarchar(max)
Buffer|サポートされていません
オブジェクト|サポートされていません
Array|サポートされていません
Stream|サポートされていません

###<a name="TSQL"></a>テーブルにアクセスするための Transact-SQL の使用

最も簡単な方法を使用してサーバー スクリプトからのデータに作業テーブルに、 [table object] プロキシ。 ただし、あるより高度なシナリオではサポートされていない、 [table object], 、このような結合クエリとその他の複雑なクエリや、ストアド プロシージャの呼び出しです。 このような場合を使用して、リレーショナル テーブルに対して直接 Transact SQL ステートメントを実行する必要があります、 [mssql object]します。 このオブジェクトには、以下の関数が用意されています。

- **クエリ**: TSQL 文字列で指定されたクエリを実行する結果が返されます、 **成功** コールバックに、 **オプション** オブジェクトです。 場合に、クエリがパラメーターを含めることができます、 *params* パラメーターが存在します。
- **queryRaw**: のような *クエリ* 設定、クエリから返される結果が '未加工' 形式である点が異なります (以下の例を参照してください)。
- **開く**: をモバイル サービス データベースへの接続を取得し、トランザクションなどのデータベース操作を呼び出す接続オブジェクトを使用することができます。

以下の方法は、下のものほど、クエリ処理に対して低レベルの制御を行うことができます。

+ [方法: 静的クエリを実行する]
+ [方法: 動的クエリを実行する]
+ [方法: リレーショナル テーブルを結合する]
+ [方法: を返すクエリを実行 *生* 結果]
+ [方法: データベース接続へのアクセスを取得する] 

####<a name="static-query"></a>方法: 静的クエリを実行する

次のクエリにはパラメーターがなく、`statusupdate` テーブルから 3 つのレコードを返します。 行セットは標準の JSON 形式です。

        mssql.query('select top 3 * from statusupdates', {
            success: function(results) {
                console.log(results);
            },
            error: function(err) {
                console.log("error is: " + err);
            }
        });


####<a name="dynamic-query"></a>方法: 動的なパラメーター化されたクエリを実行する

次の例は、permissions テーブルから各ユーザーのアクセス許可を読み取ることによってカスタム承認を実装しています。 プレースホルダー (?) は、クエリが実行されるときに、指定されたパラメーターに置き換えられます。

            var sql = "SELECT _id FROM permissions WHERE userId = ? AND permission = 'submit order'";
            mssql.query(sql, [user.userId], {
                success: function(results) {
                    if (results.length > 0) {
                        // Permission record was found. Continue normal execution. 
                        request.execute();
                    } else {
                        console.log('User %s attempted to submit an order without permissions.', user.userId);
                        request.respond(statusCodes.FORBIDDEN, 'You do not have permission to submit orders.');
                    }
                },
                error: function(err) {
                    console.log("error is: " + err);
                }   
            });


####<a name="joins"></a>方法: リレーショナル テーブルを結合する

使用して 2 つのテーブルを結合することができます、 **クエリ** のメソッド、 [mssql object] 、結合を実装する TSQL コードを渡します。 いくつかの項目があると仮定、 **ToDoItem** テーブルとテーブル内の各項目は、 **優先順位** プロパティで、テーブル内の列に対応しています。 項目は次のようになります。

        { text: 'Take out the trash', complete: false, priority: 1}

ある追加のテーブルと呼ばれる **優先順位** を優先度を含む行 **数** とテキスト **説明**します。 たとえば、優先度番号 1 の説明は "Critical" で、オブジェクトは次のようになります。

        { number: 1, description: 'Critical'}

これで置き換えます、 **優先順位** 番号優先度の値のテキストの説明で、項目。 そうするには、2 つのテーブルのリレーショナル結合を使用します。

        mssql.query('SELECT t.text, t.complete, p.description FROM ToDoItem as t INNER JOIN Priority as p ON t.priority = p.number', {
            success: function(results) {
                console.log(results);
            },
            error: function(err) {
                console.log("error is: " + err);
        });
    
スクリプトは 2 つのテーブルを結合し、結果をログに書き込みます。 結果のオブジェクトは、次のようになります。

        { text: 'Take out the trash', complete: false, description: 'Critical'}


####<a name="raw"></a>方法: を返すクエリを実行 *生* 結果

この例は、前の例と同様にクエリを実行しますが、結果セットを "未加工" の形式で返します。そのため、行ごと、列ごとに解析する必要があります。 このようなシナリオとして考えられるのは、Mobile Services でサポートされていないデータ型にアクセスする必要がある場合です。 次のコードは、単に出力をコンソール ログに書き込むので、未加工形式を調べることができます。

        mssql.queryRaw('SELECT * FROM ToDoItem', {
            success: function(results) {
                console.log(results);
            },
            error: function(err) {
                console.log("error is: " + err);
            }
        });

このクエリを実行したときの出力を次に示します。 テーブルの各列についてのメタデータと、行および列の表現が含まれます。

        { meta: 
           [ { name: 'id',
               size: 19,
               nullable: false,
               type: 'number',
               sqlType: 'bigint identity' },
             { name: 'text',
               size: 0,
               nullable: true,
               type: 'text',
               sqlType: 'nvarchar' },
             { name: 'complete',
               size: 1,
               nullable: true,
               type: 'boolean',
               sqlType: 'bit' },
             { name: 'priority',
               size: 53,
               nullable: true,
               type: 'number',
               sqlType: 'float' } ],
          rows: 
           [ [ 1, 'good idea for the future', null, 3 ],
             [ 2, 'this is important but not so much', null, 2 ],
             [ 3, 'fix this bug now', null, 0 ],
             [ 4, 'we need to fix this one real soon now', null, 1 ],
           ] }

####<a name="connection"></a>方法: データベース接続へのアクセスを取得する

使用することができます、 **開く** データベース接続へのアクセスを取得します。 このようにする理由の 1 つとして考えられるのは、データベース トランザクションを使用する必要がある場合です。

実行が成功、 **開く** データベースに接続に渡すを **成功** をパラメーターとして機能します。 次の関数のいずれかを呼び出すことができます、 **接続** オブジェクト: *閉じる*, 、*queryRaw*, 、*クエリ*, 、*beginTransaction*, 、*コミット*, 、および *ロールバック*します。

            mssql.open({
                success: function(connection) {
                    connection.query(//query to execute);
                },
                error: function(err) {
                    console.log("error is: " + err);
                }
            });

##<a name="debugging"></a>デバッグおよびトラブルシューティング

サーバー スクリプトをデバッグおよびトラブルシューティングするための主な方法は、サービス ログへの書き込みです。 既定では、Mobile Services は、サービス スクリプトの実行中に発生したエラーをサービス ログに書き込みます。 また、スクリプトでログに書き込むこともできます。 ログへの書き込みは、スクリプトをデバッグし、適切に動作しているかどうかを検証するための優れた方法です。

###<a name="write-to-logs"></a>方法: 出力をモバイル サービス ログに書き込む

グローバルを使用して、ログに書き込む、 [console object]します。 使用して、 **ログ** または **情報** 情報レベルの警告を記録します。  **警告** と **エラー** 関数は、これらは、強調、ログに、それぞれのレベルをログします。 

> [AZURE.NOTE] ログオン、モバイル サービスのログを表示する、 [Azure クラシック ポータル](https://manage.windowsazure.com/), 、モバイル サービスを選択してを選択し、 **ログ** ] タブをクリックします。

ログ関数を使用することも、 [console object] パラメーターを使用してメッセージをフォーマットします。 次の例では、メッセージ文字列のパラメーターとして JSON オブジェクトを指定します。

    function insert(item, user, request) {
        console.log("Inserting item '%j' for user '%j'.", item, user);  
        request.execute();
    }

文字列 `%j` は JSON オブジェクトのプレースホルダーとして使用されており、パラメーターを順番に指定していることに注意してください。 

ログが過大になることを避けるには、運用環境で使用する必要がない console.log() への呼び出しを削除または無効にする必要があります。

<!-- Anchors. -->
[Introduction]: #intro
[Table operations]: #table-scripts
[Basic table operations]: #basic-table-ops
[How to: Register for table operations]: #register-table-scripts
[How to: Define table scripts]: #execute-operation
[How to: override the default response]: #override-response
[How to: Modify an operation]: #modify-operation
[How to: Override success and error]: #override-success-error
[How to: Override execute success]: #override-success
[How to: Override default error handling]: #override-error
[How to: Access tables from scripts]: #access-tables
[How to: Add custom parameters]: #access-headers
[How to: Work with users]: #work-with-users
[How to: Define scheduled job scripts]: #scheduler-scripts
[How to: Refine access to tables]: #authorize-tables
[Using Transact-SQL to access tables]: #TSQL
[How to: Run a static query]: #static-query
[How to: Run a dynamic query]: #dynamic-query
[How to: Run a query that returns *raw* results]: #raw
[How to: Get access to a database connection]: #connection
[How to: Join relational tables]: #joins
[How to: Perform Bulk Inserts]: #bulk-inserts
[How to: Map JSON types to database types]: #JSON-types
[How to: Load Node.js modules]: #modules-helper-functions
[How to: Write output to the mobile service logs]: #write-to-logs
[Source control, shared code, and helper functions]: #shared-code
[Using the command line tool]: #command-prompt
[Working with tables]: #working-with-tables
[Custom API anchor]: #custom-api
[How to: Define a custom API]: #define-custom-api
[How to: Share code by using source control]: #shared-code-source-control
[How to: Use helper functions]: #helper-functions
[Debugging and troubleshooting]: #debugging
[How to: Implement HTTP methods]: #handle-methods
[How to: Work with users and headers in a custom API]: #get-api-user
[How to: Access custom API request headers]: #get-api-headers
[Job Scheduler]: #scheduler-scripts
[How to: Define multiple routes in a custom API]: #api-routes
[How to: Send and receive data as XML]: #api-return-xml
[How to: Work with app settings]: #app-settings

[1]: ./media/mobile-services-how-to-use-server-scripts/1-mobile-insert-script-users.png
[2]: ./media/mobile-services-how-to-use-server-scripts/2-mobile-custom-api-script.png
[3]: ./media/mobile-services-how-to-use-server-scripts/3-mobile-schedule-job-script.png
[4]: ./media/mobile-services-how-to-use-server-scripts/4-mobile-source-local-cli.png

<!-- URLs. -->
[Mobile Services server script reference]: http://msdn.microsoft.com/library/windowsazure/jj554226.aspx
[Schedule backend jobs in Mobile Services]: /develop/mobile/tutorials/schedule-backend-tasks/
[request object]: http://msdn.microsoft.com/library/windowsazure/jj554218.aspx
[response object]: http://msdn.microsoft.com/library/windowsazure/dn303373.aspx
[User object]: http://msdn.microsoft.com/library/windowsazure/jj554220.aspx
[push object]: http://msdn.microsoft.com/library/windowsazure/jj554217.aspx
[insert function]: http://msdn.microsoft.com/library/windowsazure/jj554229.aspx
[insert]: http://msdn.microsoft.com/library/windowsazure/jj554229.aspx
[update function]: http://msdn.microsoft.com/library/windowsazure/jj554214.aspx
[delete function]: http://msdn.microsoft.com/library/windowsazure/jj554215.aspx
[read function]: http://msdn.microsoft.com/library/windowsazure/jj554224.aspx
[update]: http://msdn.microsoft.com/library/windowsazure/jj554214.aspx
[delete]: http://msdn.microsoft.com/library/windowsazure/jj554215.aspx
[read]: http://msdn.microsoft.com/library/windowsazure/jj554224.aspx
[query object]: http://msdn.microsoft.com/library/windowsazure/jj613353.aspx
[apns object]: http://msdn.microsoft.com/library/windowsazure/jj839711.aspx
[mpns object]: http://msdn.microsoft.com/library/windowsazure/jj871025.aspx
[wns object]: http://msdn.microsoft.com/library/windowsazure/jj860484.aspx
[table object]: http://msdn.microsoft.com/library/windowsazure/jj554210.aspx
[tables object]: http://msdn.microsoft.com/library/windowsazure/jj614364.aspx
[mssql object]: http://msdn.microsoft.com/library/windowsazure/jj554212.aspx
[console object]: http://msdn.microsoft.com/library/windowsazure/jj554209.aspx
[Read and write data]: http://msdn.microsoft.com/library/windowsazure/jj631640.aspx
[Validate data]: http://msdn.microsoft.com/library/windowsazure/jj631638.aspx
[Modify the request]: http://msdn.microsoft.com/library/windowsazure/jj631635.aspx
[Modify the response]: http://msdn.microsoft.com/library/windowsazure/jj631631.aspx
[Azure classic portal]: https://manage.windowsazure.com/
[Schedule jobs]: http://msdn.microsoft.com/library/windowsazure/jj860528.aspx
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-dotnet/
[Commands to manage Azure Mobile Services]: ../virtual-machines-command-line-tools.md#Mobile_Scripts
[Windows Store Push]: /develop/mobile/tutorials/get-started-with-push-dotnet/
[Windows Phone Push]: /develop/mobile/tutorials/get-started-with-push-wp8/
[iOS Push]: /develop/mobile/tutorials/get-started-with-push-ios/
[Android Push]: /develop/mobile/tutorials/get-started-with-push-android/
[Azure SDK for Node.js]: http://go.microsoft.com/fwlink/p/?LinkId=275539
[Send HTTP request]: http://msdn.microsoft.com/library/windowsazure/jj631641.aspx
[Send email from Mobile Services with SendGrid]: /develop/mobile/tutorials/send-email-with-sendgrid/
[Get started with authentication]: http://go.microsoft.com/fwlink/p/?LinkId=287177
[crypto API]: http://go.microsoft.com/fwlink/p/?LinkId=288802
[path API]: http://go.microsoft.com/fwlink/p/?LinkId=288803
[querystring API]: http://go.microsoft.com/fwlink/p/?LinkId=288804
[url API]: http://go.microsoft.com/fwlink/p/?LinkId=288805
[util API]: http://go.microsoft.com/fwlink/p/?LinkId=288806
[zlib API]: http://go.microsoft.com/fwlink/p/?LinkId=288807
[Custom API]: http://msdn.microsoft.com/library/windowsazure/dn280974.aspx
[Call a custom API from the client]: /develop/mobile/tutorials/call-custom-api-dotnet/#define-custom-api
[express.js library]: http://go.microsoft.com/fwlink/p/?LinkId=309046
[Define a custom API that supports periodic notifications]: /develop/mobile/tutorials/create-pull-notifications-dotnet/
[express object in express.js]: http://expressjs.com/api.html#express
[Store server scripts in source control]: /develop/mobile/tutorials/store-scripts-in-source-control/
[Leverage shared code and Node.js modules in your server scripts]: /develop/mobile/tutorials/store-scripts-in-source-control/#use-npm
[service object]: http://msdn.microsoft.com/library/windowsazure/dn303371.aspx
[App settings]: http://msdn.microsoft.com/library/dn529070.aspx
[config module]: http://msdn.microsoft.com/library/dn508125.aspx
[Support for package.json in Azure Mobile Services]: http://go.microsoft.com/fwlink/p/?LinkId=391036
 


