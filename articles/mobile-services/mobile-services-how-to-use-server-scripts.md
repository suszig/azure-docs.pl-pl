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

## <a name="intro"></a>概要

JavaScript バックエンド モバイル サービスを使用すると、サーバーに格納されて実行される JavaScript コードとして、カスタムのビジネス ロジックを定義できます。 このサーバー スクリプト コードは、次のサーバー機能のいずれかに割り当てられます。

+ [挿入、読み取り、更新、または ][table operations]します。
+ [スケジュールされたジョブが ][job scheduler]します。
+ [カスタムで定義された HTTP メソッド API ][custom api anchor]します。

サーバー スクリプトのメイン関数のシグネチャは、スクリプトが使用されるコンテキストによって異なります。 スクリプト間で共有される共通スクリプト コードを nodes.js モジュールとして定義することもできます。 詳細については、次を参照してください。 [ソース管理と共有コード ][source control, shared code, and helper functions]します。

個々のサーバー スクリプト オブジェクトおよび関数の詳細については、[モバイル サービスのサーバー スクリプト リファレンス]を参照してください。


## <a name="table-scripts"></a>テーブル操作

テーブル操作スクリプトは、テーブルに対する操作 (insert、read、update、または delete (*del*)) に登録されたサーバー スクリプトです。 このセクションでは、JavaScript バックエンドでテーブルを操作する方法について説明します。次のセクションが含まれています。

+ [テーブル操作 ][basic table operations]
+ [方法: テーブル操作の登録]
+ [方法: 既定の応答をオーバーライドする]
+ [方法: execute success をオーバーライド]
+ [方法: 既定のエラー処理をオーバーライドする]
+ [方法: 一意の ID 値を生成します。](#generate-guids)
+ [方法: カスタム パラメーターを追加]
+ [方法: テーブルのユーザーを処理 ][how to: work with users]

### <a name="basic-table-ops"></a>テーブル操作の概要

スクリプトの名前は、登録されている操作の種類と一致する必要があります。 1 つのテーブル操作に対して登録できるスクリプトは 1 つだけです。 REST 要求によって特定の操作が呼び出されるたびに、スクリプトが実行されます。たとえば、テーブルに項目を挿入する POST 要求を受け取ると、そのたびに特定のスクリプトが実行されます。 Mobile Services では、スクリプトの実行間で状態は保持されません。 スクリプトが実行されるたびに新しいグローバル コンテキストが作成されるため、スクリプトで定義されている状態変数がすべて再初期化されます。 要求間で状態を保存する必要がある場合は、モバイル サービス上でテーブルを作成し、そのテーブルに対して状態を読み書きします。 詳細については、次を参照してください。 [方法: スクリプトからテーブルにアクセス]。

操作が実行されたときにカスタマイズされたビジネス ロジックを強制的に実行する必要がある場合は、テーブル操作スクリプトを作成します。 たとえば、次のスクリプトには、挿入操作は拒否します。 ここでの文字列の長さ、 `テキスト` フィールドは 10 文字より長い場合。

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

    - 挿入と更新では、**item** オブジェクトです。これは、操作の影響を受ける行の JSON 表現です。 そのため、*item.Owner* のように、名前で列の値にアクセスできます。*Owner* は JSON 表現でのいずれかの名前です。
    - 削除では、削除するレコードの ID です。
    - 読み取りでは、[クエリのオブジェクト] 返される行セットを指定します。

- 2 番目の引数は常に、 [ユーザー オブジェクトの ][user object] 要求を送信したユーザーを表します。

- 3 番目の引数は常に、 [request オブジェクト ][request object], 、要求された操作と、クライアントに送信される応答の実行を制御できます。

テーブル操作の正規のメイン関数署名は次のとおりです。

+ [挿入 ][insert function]: `関数の挿入 (項目、ユーザーの要求) {…}`
+ [更新 ][update function]: `(項目、ユーザーの要求) の更新プログラムの機能 {...}`
+ [[機能の削除] 削除][delete function]: `関数 del (id、ユーザーの要求) {…}`
+ [読み取り ][read function]: `関数読み取り (クエリ、ユーザーの要求) {…}`

>[AZURE.NOTE]delete は JavaScript の予約語であるため、削除操作に登録する関数には _del_ という名前を付ける必要があります。 

サーバー スクリプトには必ずメインの関数が含まれ、必要に応じて省略可能なヘルパー関数が使用されます。 サーバー スクリプトが特定のテーブル用に作成されていても、同じデータベース内の他のテーブルも参照することができます。 スクリプト間で共有できる一般的な関数をモジュールとして定義することもできます。 詳細については、次を参照してください。 [ソース管理と共有コード ][source control, shared code, and helper functions]します。

### <a name="register-table-scripts"></a>方法: テーブル スクリプトを登録します。

次のいずれかの方法で、テーブル操作に登録されるサーバー スクリプトを定義できます。

+ [Azure クラシック ポータルで] です。 テーブル操作用のスクリプトには、特定のテーブルの **[スクリプト]** タブでアクセスします。 挿入スクリプトに登録されている既定のコードを次に示します、 `TodoItem` テーブルです。 このコードを独自のカスタム ビジネス ロジックでオーバーライドできます。

    ![1][1]

    これを行う方法については、次を参照してください。 [検証サーバー スクリプトを使用したモバイル サービスでのデータおよび変更] です。

+ ソース管理を使用して。ソース管理を有効にしたら、という名前のファイルを作成するだけで <em>`< テーブル >`</em>.<em>`< 操作 >`</em>、git リポジトリの .\service\table サブフォルダーに .js、 <em>`< テーブル >`</em> テーブルの名前を指定し、 <em>`< 操作 >`</em> 登録テーブル操作は、です。詳細については、次を参照してください。 [ソース管理と共有コード ][source control, shared code, and helper functions]します。

+ コマンド プロンプトから Azure コマンド ライン ツールを使用して。 詳細については、[コマンド ライン ツールを使用する] を参照してください。


テーブル操作スクリプトは、少なくとも 1 つのクライアントが応答を受け取っているかどうかを確認する [request オブジェクト] の次の関数を呼び出す必要があります。

+ **execute 関数**: 操作を要求されたとおりに完了し、標準の応答を返します。

+ **respond 関数**: カスタム応答を返します。

> [AZURE.IMPORTANT] スクリプトに、**execute** も **respond** も呼び出されないコード パスが含まれている場合、操作が応答しなくなることがあります。

次のスクリプトでは、**execute** 関数を呼び出して、クライアントによって要求されたデータ操作を完了します。

    function insert(item, user, request) { 
        request.execute(); 
    }

この例では、データベースに項目を挿入し、適切な状態コードをユーザーに返します。

ときに、 **実行** 関数が呼び出されると、 `項目`, 、[クエリ ][query object], 、または `id` スクリプト関数に最初の引数として渡された値を使用して、操作を実行します。 挿入、更新、またはクエリ操作では、**execute** を呼び出す前に、item や query を変更できます。

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

>[AZURE.NOTE]削除スクリプトでは、指定した userId 変数の値を変更しても、削除されるレコードには影響しません。

例については、[データの読み取りと書き込み] を参照してください。、、[要求の変更] および [データの検証] です。


### <a name="override-response"></a>方法: 既定の応答をオーバーライドします。

スクリプトを使用して、既定の応答動作をオーバーライドできる検証ロジックを実装することもできます。 検証が失敗したら、**execute** 関数の代わりに **respond** 関数を呼び出して、応答をクライアントに書き込みます。

    function insert(item, user, request) {
        if (item.userId !== user.userId) {
            request.respond(statusCodes.FORBIDDEN, 
            'You may only insert records with your userId.');
        } else {
            request.execute();
        }
    }

挿入された項目が持っていない場合にこの例では、要求が拒否されます、 `userId` に一致するプロパティ、 `userId` [ユーザー オブジェクト] の認証されたクライアントに提供されます。 その場合、データベース操作 (*insert*) は実行されず、403 HTTP ステータス コードとカスタム エラー メッセージを含む応答がクライアントに返されます。 例については、[応答の変更] を参照してください。

### <a name="override-success"></a>方法: execute success をオーバーライド

テーブル操作の場合、既定では、**execute** 関数は応答を自動的に書き込みます。 ただし、execute 関数に、成功および失敗時の動作をオーバーライドする 2 つのオプション パラメーターを渡すことができます。

execute を呼び出すときに **success** ハンドラーを渡すことで、クエリの結果を変更した後でそれを応答に書き込むことができます。 次の例では `実行 ({success: function(results) {...})` 応答を書き込む前に、データベースからデータが読み取られた後に、追加の作業を実行します。

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

**execute** 関数に **success** ハンドラーを渡す場合は、スクリプトが完了して応答を書き込むことができることをランタイムに通知するために、**success** ハンドラーの一部として **respond** 関数も呼び出す必要があります。 引数を渡さずに **respond** を呼び出すと、既定の応答がモバイル サービスによって生成されます。
>[AZURE.NOTE]最初に **execute** 関数を呼び出した後でのみ、引数を指定せずに **respond** 関数を呼び出すと、既定の応答を呼び出すことができます。

### <a name="override-error"></a>方法: 既定のエラー処理をオーバーライドします。

**execute** 関数は、データベースへの接続が失われた場合、オブジェクトが無効である場合、クエリが間違っている場合に失敗することがあります。 エラーが発生すると、サーバー スクリプトは既定でエラーをログに記録し、エラー結果を応答に書き込みます。 Mobile Services には既定のエラー処理が用意されているため、サービスで発生する可能性のあるそうしたエラーを処理する必要はありません。

特定の修正操作を実行する場合や、グローバル console オブジェクトを使用して詳細な情報をログに書き込む場合は、明示的なエラー処理を実装して、既定のエラー処理をオーバーライドすることができます。 それには、**execute** 関数に **error** ハンドラーを渡します。

    function update(item, user, request) { 
      request.execute({ 
        error: function(err) { 
          // Do some custom logging, then call respond. 
          request.respond(); 
        } 
      }); 
    }

error ハンドラーを渡した場合は、**respond** が呼び出されたときに、Mobile Services からクライアントにエラー結果が返されます。

必要であれば、**success** ハンドラーと **error** ハンドラーも渡すことができます。

### <a name="generate-guids"></a>方法: 一意の ID 値を生成します。

Mobile Services は、テーブルの **ID** 列で一意のカスタム文字列値をサポートしています。 このため、アプリケーションは、ID にメール アドレスやユーザー名などのカスタム値を使用できます。

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

値、 `id` で一意である必要があり、次のセット内の文字を含めることはできません。

+ 制御文字: [0x0000-0x001F] および [0x007F-0x009F]。 詳細については、次を参照してください。 [ASCII 制御コード C0 および C1](http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set)します。
+  印刷可能文字: **"**(0x0022)、 **\ +** (0x002B)、 **/** (0x002F)、 **?**(0x003F)、 **\\** (0x005C)、 **'** (0x0060)
+  ID "." および ".."

また、テーブルに整数 ID を使用することもできます。 整数 ID を使用するにはしてテーブルを作成する必要があります、 `モバイル テーブル作成` コマンドを使用して、 `--integerId` オプション。 このコマンドは、Azure のコマンド ライン インターフェイス (CLI) で使用されます。 CLI の使用の詳細については、次を参照してください。 [モバイル サービス テーブルの管理用コマンド](../virtual-machines-command-line-tools.md#Mobile_Tables)します。


### <a name="access-headers"></a>方法: カスタム パラメーターにアクセス

モバイル サービスに要求を送信する際に、要求の URI に 1 つ以上のカスタム パラメーターを含めることで、特定の要求の処理方法をテーブル操作スクリプトに指定できます。 その後、処理パスを判断するためにパラメーターを調べるようにスクリプトを変更します。

たとえば、POST 要求の次の URI では、同じテキスト値を持つ新しい *TodoItem* の挿入を許可しないようにサービスに指示します。

        https://todolist.azure-mobile.net/tables/TodoItem?duplicateText=false

これらのカスタム クエリ パラメーターから JSON 値としてアクセスされる、 **パラメーター** [request オブジェクト] のプロパティです。 **request** オブジェクトは、モバイル サービスからテーブル操作に登録されている関数に提供されます。 次の挿入操作のサーバー スクリプトの値を調べて、 `duplicateText` パラメーターの挿入操作を実行する前に。

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

した **insertItemIfNotComplete** 、 **実行** [request オブジェクト] の機能が重複するテキストがない場合、項目を挿入する。 それ以外の場合には、 **応答** 、重複についてクライアントに通知する関数が呼び出されます。

前のコードでの **success** 関数への呼び出しの構文に注意してください。

                }).read({
                    success: insertItemIfNotComplete
                });

JavaScript では、これは次のようなより長い構文の短縮版です。

        success: function(results) 
        { 
            insertItemIfNotComplete(results); 
        }

### <a name="work-with-users"></a>方法: ユーザーを処理

Azure Mobile Services では、ID プロバイダーを使用して、ユーザーを認証できます。 詳細については、次を参照してください。 [を使ってみる認証] です。 認証されたユーザーがテーブル操作を呼び出したとき、モバイル サービスは [ユーザー オブジェクト] を使用して、登録されたスクリプト関数にユーザーに関する情報を提供します。 **userId** プロパティを使用すると、ユーザー固有の情報を保存および取得できます。 次の例では、認証済みのユーザーの **userId** に基づいて、item の owner プロパティを設定します。

    function insert(item, user, request) {
        item.owner = user.userId;
        request.execute();
    }

次の例では、認証されたユーザーの **userId** に基づいて、query にフィルターを追加します。 このフィルターでは、結果が現在のユーザーに属する項目のみに制限されます。

    function read(query, user, request) {
        query.where({
            owner: user.userId
        });
        request.execute();
    }

## <a name="custom-api"></a>カスタム Api

このセクションでは、カスタム API エンドポイントを作成して操作する方法について説明します。次のセクションが含まれています。

+ [カスタム Api の概要](#custom-api-overview)
+ [方法: カスタム API を定義する]
+ [方法: HTTP メソッドを実装]
+ [方法: XML としてデータの送受信を行う]
+ [方法: カスタム API でユーザーとヘッダーを使用]
+ [方法: カスタム API で複数のルートを定義する]

### <a name="custom-api-overview"></a>カスタム Api の概要

カスタム API は、GET、POST、PUT、PATCH、DELETE という 1 つ以上の標準 HTTP メソッドによってアクセスされる、モバイル サービスのエンドポイントです。 カスタム API によってサポートされている各 HTTP メソッドに個別の関数 export を定義し、すべてを 1 つのスクリプト ファイルに含めることができます。 特定のメソッドを使用するカスタム API への要求が受信されると、登録されたスクリプトが呼び出されます。 詳細については、[カスタム API] を参照してください。

カスタム API の関数がモバイル サービス ランタイムによって呼び出されたときに両方の [要求 ][request object] と [応答 ][response object] オブジェクトを指定します。 これらのオブジェクトは、これをスクリプトで利用できる [express.js ライブラリ] の機能を公開します。 という名前の次のカスタム API **こんにちは** _Hello, を返す非常に単純な例は、world! _ を POST 要求に対して応答。

        exports.post = function(request, response) {
            response.send(200, "{ message: 'Hello, world!' }");
        } 

**送信** [応答オブジェクト] での関数は、クライアントに、指定された応答を返します。 このコードは、次の URL への POST 要求が送信されたときに呼び出されます。

        https://todolist.azure-mobile.net/api/hello  

グローバル状態は実行間で維持されます。

### <a name="define-custom-api"></a>方法: カスタム API を定義します。

次のいずれかの方法で、カスタム API エンドポイントの HTTP メソッドに登録されるサーバー スクリプトを定義できます。

+ [Azure クラシック ポータルで] です。 カスタム API スクリプトは、**[API]** タブで作成および変更できます。 サーバー スクリプト コードは、特定のカスタム API の **[スクリプト]** タブにあります。 POST 要求によって呼び出されるスクリプトを次に示します、 `CompleteAll` カスタム API エンドポイント。

    ![2][2]

    カスタム API メソッドに対するアクセス許可は、[アクセス許可] タブで割り当てます。 このカスタム API の作成方法を表示するには、[クライアントからのカスタム API を呼び出す] を参照してください。

+ ソース管理を使用して。ソース管理を有効にしたら、という名前のファイルを作成するだけで <em>`< custom_api >`</em>、git リポジトリの .\service\api サブフォルダーに .js、 <em>`< custom_api >`</em> 登録するカスタム API の名前を指定します。このスクリプト ファイルには、カスタム API によって公開されている各 HTTP メソッドの _exported_ 関数が含まれています。アクセス許可は、付属 .json ファイルで定義されます。詳細については、次を参照してください。 [ソース管理と共有コード ][source control, shared code, and helper functions]します。

+ コマンド プロンプトから Azure コマンド ライン ツールを使用して。 詳細については、[コマンド ライン ツールを使用する] を参照してください。

### <a name="handle-methods"></a>方法: HTTP メソッドを実装

カスタム API は、1 つ以上の HTTP メソッド (GET、POST、PUT、PATCH、および DELETE) を処理できます。 カスタム API によって処理される HTTP メソッドごとに、エクスポートされる関数が定義されます。 1 つのカスタム API コード ファイルは、次の関数の 1 つまたはすべてをエクスポートできます。

        exports.get = function(request, response) { ... };
        exports.post = function(request, response) { ... };
        exports.patch = function(request, response) { ... };
        exports.put = function(request, response) { ... };
        exports.delete = function(request, response) { ... };

カスタム API エンドポイントは、サーバー スクリプトで実装されていない HTTP メソッドを使用して呼び出すことはできず、405 (許可されていないメソッド) エラー応答が返されます。 各サポート HTTP メソッドに個別のアクセス許可レベルを割り当てることができます。

### <a name="api-return-xml"></a>方法: XML としてデータの送受信を行う

クライアントがデータを格納および取得するときに、Mobile Services は JavaScript Object Notation (JSON) を使用してメッセージ本体のデータを表現します。 ただし、そうするのではなく、XML ペイロードを使用したいシナリオもあります。 たとえば、Windows ストア アプリには、サービスに XML の発行を要求する、組み込みの定期的な通知機能があります。 詳細については、[定期的な通知をサポートするカスタム API を定義する] を参照してください。

次の **OrderPizza** カスタム API 関数は、応答ペイロードとして単純な XML ドキュメントを返します。

        exports.get = function(request, response) {
          response.set('content-type', 'application/xml');
          var xml = '<?xml version="1.0"?><PizzaOrderForm><PizzaOrderForm/>';
          response.send(200, xml);
        };

このカスタム API 関数は、次のエンドポイントへの HTTP GET 要求によって呼び出されます。

        https://todolist.azure-mobile.net/api/orderpizza

### <a name="get-api-user"></a>方法: カスタム API でユーザーとヘッダーを使用

Azure Mobile Services では、ID プロバイダーを使用して、ユーザーを認証できます。 詳細については、次を参照してください。 [を使ってみる認証] です。 ときに認証されたユーザー要求のカスタム API では、モバイル サービスは、[ユーザーのオブジェクト] を使用して、カスタム API コードにユーザーに関する情報を提供します。 [ユーザー オブジェクト] は、[request オブジェクト] の user プロパティからアクセスされます。 **userId** プロパティを使用すると、ユーザー固有の情報を保存および取得できます。

次の **OrderPizza** カスタム API 関数では、認証済みのユーザーの **userId** に基づいて、item の owner プロパティを設定します。

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

次のコードに示すようには、[request オブジェクト] から特定の HTTP ヘッダーにアクセスすることもできます。

        exports.get = function(request, response) {    
            var header = request.header('my-custom-header');
            response.send(200, "You sent: " + header);
        };

この簡単な例は、という名前のカスタム ヘッダーを読み取り `マイ カスタム ヘッダー`, 、応答で、値が返されます。

### <a name="api-routes"></a>方法: カスタム API で複数のルートを定義します。

Mobile Services では、カスタム API 内で複数のパス (ルート) を定義できます。 たとえば、**calculator** カスタム API での次の URL への HTTP GET 要求は、それぞれ **add** 関数または **subtract** 関数を呼び出します。

+ `https://&lt;service&gt;.azure-mobile.net/api/calculator/add`
+ `https://&lt;service&gt;.azure-mobile.net/api/calculator/sub`

エクスポートすることによって複数のルートが定義されている、 **登録** 、渡される関数に、 **api** ルートをカスタム API エンドポイントの登録に使用されるオブジェクトを (の [高速 express.js 内のオブジェクト] に似ています)。 次の例は、**calculator** カスタム API の **add** メソッドと **sub** メソッドを実装しています。

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

**register** 関数に渡された **api** オブジェクトは、各 HTTP メソッド (**get**、**post**、**put**、**patch**、**delete**) の関数を公開します。 これらの関数は、特定の HTTP メソッド用に定義された関数にルートを登録します。 各関数は 2 つのパラメーターを受け取ります。最初のパラメーターはルート名で、2 つ目のパラメーターはルートに登録される関数です。

前のカスタム API の例での 2 つのルートは、次のような HTTP GET 要求で呼び出すことができます (応答と共に示します)。

+ `https://&lt;service&gt;.azure-mobile.net/api/calculator/add?a=1&b=2`

        {"result":3}

+ `https://&lt;service&gt;.azure-mobile.net/api/calculator/sub?a=3&b=5`

        {"result":-2}


## <a name="scheduler-scripts"></a>ジョブ スケジューラ

Mobile Services を使用すると、決まったスケジュールでのジョブまたは Azure クラシック ポータルからのオンデマンドとして実行できるサーバー スクリプトを定義できます。 スケジュールされたジョブは、テーブル データのクリーンアップやバッチ処理のような定期的なタスクを実行する場合に便利です。 詳細については、[ジョブのスケジュール] を参照してください。

スケジュールされたジョブに登録されているスクリプトには、スケジュールされたジョブと同じ名前のメイン関数があります。 スケジュールされたスクリプトは HTTP 要求によって呼び出されないため、サーバー ランタイムによって渡すことができるコンテキストがなく、関数はパラメーターを受け取りません。 他の種類のスクリプトと同様に、サブルーティン関数を使用したり、共有モジュールを要求したりすることができます。 詳細については、[ソース管理、共有コード、およびヘルパー関数] を参照してください。

### <a name="scheduler-scripts"></a>方法: を定義するスケジュールされたジョブ スクリプト

サーバー スクリプトは、Mobile Services Scheduler で定義されたジョブに割り当てることができます。 これらのスクリプトは、ジョブに属し、ジョブ スケジュールに従って実行されます (も使えば [Azure クラシック ポータル] ジョブをオンデマンドで実行することができます。) スケジュールされたジョブを定義するスクリプトでは、Mobile Services からデータが渡されないため、パラメーターはありません。このようなスクリプトは通常の JavaScript 関数として実行され、Mobile Services と直接データをやり取りすることはありません。

スケジュールされたジョブは、次のいずれかの方法で定義できます。

+ [Azure クラシック ポータル] で、 **スクリプト** 、スケジューラ内でタブをクリックします。

    ![3][3]

    これを行う方法の詳細については、[モバイル サービスでのバックエンド ジョブのスケジュール] を参照してください。

+ コマンド プロンプトから Azure コマンド ライン ツールを使用して。 詳細については、[コマンド ライン ツールを使用する] を参照してください。

>[AZURE.NOTE]ソース管理を有効にしている場合は、git リポジトリの .\service\scheduler サブフォルダーにあるスケジュールされたジョブのスクリプト ファイルを直接編集できます。 詳細については、次を参照してください。 [方法: ソース管理を使用してコードを共有] です。

## <a name="shared-code"></a>ソース管理、共有コード、およびヘルパー関数

このセクションでは、ソース管理を活用して、カスタムの node.js モジュール、共有コードや他のコードの再利用計画を追加するする方法について説明します。次のセクションが含まれています。

+ [共有コードの活用の概要](#leverage-source-control)
+ [方法: Node.js モジュールを読み込む]
+ [方法: ヘルパー関数を使用]
+ [方法: ソース管理を使用してコードを共有]
+ [方法: アプリケーションの設定を使用]

### <a name="leverage-source-control"></a>共有コードの活用の概要

Mobile Services はサーバーの Node.js を使用するため、スクリプトは既に組み込みの Node.js モジュールへのアクセス許可を持っています。 ソース管理を使用して独自のモジュールを定義したり、他の Node.js モジュールをサービスに追加したりすることもできます。

以下に、グローバルな **require** 関数を使用することによってスクリプトで利用できるいくつかのより便利なモジュールを示します。

+ **azure**: Node.js に対して Azure SDK の機能を公開します。 詳細については、[Azure SDK for Node.js] を参照してください。
+ **crypto**: OpenSSL の暗号化機能を提供します。 詳細については、次を参照してください。、 [Node.js に関するドキュメント ][crypto api]します。
+ **path**: ファイル パスを操作するためのユーティリティが含まれています。 詳細については、次を参照してください。、 [Node.js に関するドキュメント ][path api]します。
+ **querystring**: クエリ文字列を操作するためのユーティリティが含まれています。 詳細については、次を参照してください。、 [Node.js に関するドキュメント ][querystring api]します。
+ **request**: Twitter や Facebook などの外部 REST サービスに HTTP 要求を送信します。 詳細については、[HTTP 要求の送信] を参照してください。
+ **sendgrid**: Azure の Sendgrid 電子メール サービスを使用して、電子メールを送信します。 詳細については、次を参照してください。 [SendGrid を使用したモバイル サービスから電子メールを送信する]。
+ **url**: URL を解析および解決するためのユーティリティが含まれています。 詳細については、次を参照してください。、 [Node.js に関するドキュメント ][url api]します。
+ **util**: 文字列の書式設定やオブジェクトの種類の確認など、さまざまなユーティリティが含まれています。 詳細については、次を参照してください。、 [Node.js に関するドキュメント ][util api]します。
+ **zlib**: gzip や deflate などの圧縮機能を公開します。 詳細については、次を参照してください。、 [Node.js に関するドキュメント ][zlib api]します。

### <a name="modules-helper-functions"></a>方法: モジュールを利用します。

Mobile Services では、スクリプトで **require** グローバル関数を使用して読み込むことができる一連のモジュールを公開しています。 たとえば、スクリプトから **request** で HTTP 要求を行うよう要求できます。

    function update(item, user, request) { 
        var httpRequest = require('request'); 
        httpRequest('http://www.google.com', function(err, response, body) { 
            ... 
        }); 
    } 

### <a name="shared-code-source-control"></a>方法: ソース管理を使用してコードを共有

ソース管理で Node.js のパッケージ マネージャー (npm) を使用して、モバイル サービスで利用できるモジュールを制御することができます。 この作業を実行する 2 つの方法があります。

+ npm によって発行およびインストールされるモジュールでは、package.json ファイルを使用して、モバイル サービスからインストールしようとするパッケージを宣言します。 この方法で、サービスは常に、必要なパッケージの最新バージョンにアクセスできます。 Package.json ファイルが在住、 `. \service` ディレクトリ。 詳細については、[Azure Mobile Services での package.json のサポート] を参照してください。

+ プライベート モジュールまたはカスタム モジュールを npm を使用にモジュールを手動でインストールする、 `.\service\node_modules` 、ソース管理のディレクトリ。 モジュールを手動でアップロードする方法の例は、[共有を活用するコードと Node.js モジュールのサーバー スクリプトで] を参照してください。
    >[AZURE.NOTE]ときに `node_modules` 既に存在するディレクトリ階層では、NPM の作成、 `\node-uuid` サブディレクトリが新しいを作成するのではなく `node_modules` リポジトリにします。 この場合、削除、既存 `node_modules` ディレクトリ。

モバイル サービスに対応する package.json ファイルまたはカスタム モジュールをリポジトリにコミットした後、**require** を使用してそのモジュールを名前によって参照します。
>[AZURE.NOTE] package.json 内で指定するモジュール、またはモバイル サービスにアップロードするモジュールは、サーバー スクリプト コード内でのみ使用されます。 これらのモジュールは、Mobile Services ランタイムによって使用されることはありません。

### <a name="helper-functions"></a>方法: ヘルパー関数を使用

モジュールを要求する方法に加えて、個々のサーバー スクリプトにヘルパー関数を含めることもできます。 これらはメイン関数から分離された関数であり、スクリプト内のコードを分割するために使用できます。

次の例では、テーブル スクリプトが挿入操作に登録されます。この挿入操作に、ヘルパー関数 **handleUnapprovedItem** が含まれています。


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

ヘルパー関数は、1 回だけ定義して、複数のサーバー スクリプト間で共有することもできます。 スクリプト間で関数を共有するには、関数をエクスポートする必要がありにスクリプト ファイルが存在する必要があります、 `.\service\shared\` ディレクトリ。 ファイル内の共有関数をエクスポートするためのテンプレートを次に示します `.\services\shared\helpers.js`:

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

この例では、[tables オブジェクト] と [ユーザー オブジェクト] の両方を共有関数に渡す必要があります。 これは、共有スクリプトをグローバルにアクセスできないために、[tables オブジェクト] と [ユーザー オブジェクト] は、要求のコンテキストでのみが存在します。

スクリプト ファイルを使用するか、共有ディレクトリにアップロードされます [ソース管理 ][how to: share code by using source control] またはを使用して、 [コマンド ライン ツールの ][using the command line tool]します。

### <a name="app-settings"></a>方法: アプリケーションの設定を使用

Mobile Services を使用すると、値をアプリケーション設定として安全に格納できます。アプリケーション設定には、実行時にサーバー スクリプトからアクセスできます。 モバイル サービスのアプリケーション設定にデータを追加すると、名前/値ペアが暗号化されて格納され、サーバー スクリプトでそれらにアクセスできます。スクリプト ファイル内でそれらをハード コーディングする必要はありません。 詳細については、[アプリ設定] を参照してください。

次のカスタム API の例を使用して、指定されたアプリケーション設定値を取得する [サービスのオブジェクト]。

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

このコードは、ポータルの **[ID]** タブに格納されている Twitter コンシューマー キー値も取得します。 **config オブジェクト**はテーブル操作とスケジュールされたジョブ スクリプトでは使用できないため、アプリケーション設定にアクセスするには構成モジュールを要求する必要があります。 完全な例は、[モバイル サービスでのバックエンド ジョブのスケジュール] を参照してください。

<h2>
            <a name="command-prompt"></a>
            コマンド ライン ツールの使用
          </h2>

Mobile Services では、Azure コマンド ライン ツールを使用してサーバー スクリプトを作成、変更、および削除できます。 スクリプトをアップロードする前に、次のディレクトリ構造を使用していることを確認してください。

![4][4]

このディレクトリ構造は、ソース管理を使用する場合の git リポジトリと同じです。

初めて移動する必要があるコマンド ライン ツールからのスクリプト ファイルをアップロードするとき、 `.\services\` ディレクトリ。 次のコマンドはという名前のスクリプトをアップロード `todoitem.insert.js` から、 `テーブル` サブディレクトリ。

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

詳細については、[Azure Mobile Services を管理するコマンド] を参照してください。

## <a name="working-with-tables"></a>テーブルの操作

このセクションでは、SQL Database テーブルのデータを直接操作する方法について説明します。次のセクションが含まれています。

+ [テーブルの操作の概要](#overview-tables)
+ [方法: スクリプトからテーブルにアクセス]
+ [方法: 一括挿入を実行する]
+ [方法: マップ JSON 型をデータベース型に]
+ [テーブルにアクセスするには、TRANSACT-SQL を使用]

### <a name="overview-tables"></a>テーブルの操作の概要

Mobile Services の多くのシナリオでは、サーバー スクリプトがデータベース内のテーブルにアクセスする必要があります。 たとえば次のようになります。 モバイル サービスがスクリプトの実行間で状態を保持しないために、スクリプトの実行間で永続化する必要があるすべてのデータはテーブルに格納する必要があります。 アクセス許可テーブルのエントリを調べたり、監査データを単にログに書き込むだけでなくテーブルに保存したりする場合もあります (ログではデータの保存期間に限定があり、プログラムでアクセスすることができません)。

モバイル サービスは、[テーブルのオブジェクト] のプロキシを使用して、または [mssql オブジェクト] を使用して TRANSACT-SQL クエリを作成することによって、テーブルへのアクセスの 2 つの方法です。 [テーブルの object] では、簡単にするとサーバー スクリプト コードからテーブル データにアクセスするが、[mssql オブジェクト] がより複雑なデータ操作をサポートし、最大限の柔軟性を提供します。

### <a name="access-tables"></a>方法: スクリプトからテーブルにアクセス

[テーブルのオブジェクト] を使用中のスクリプトからテーブルにアクセスする最も簡単な方法です。  **GetTable** 関数は、要求されたテーブルにアクセスするためのプロキシである [テーブルのオブジェクト] インスタンスを返します。 その後、プロキシで関数を呼び出すことで、データにアクセスして変更できます。

両方のテーブル操作にスクリプトが登録され、スケジュールされたジョブは、グローバル オブジェクトとして [tables オブジェクト] にアクセスできます。 次のコード用のプロキシを取得する、 *TodoItems* グローバルからテーブル [tables オブジェクト]。

        var todoItemsTable = tables.getTable('TodoItems');

カスタム API スクリプトから [tables オブジェクト] にアクセスできる、 <strong>サービス</strong> 、指定されたプロパティ [request オブジェクト]。 次のコードでは、要求から [tables オブジェクト] を取得します。

        var todoItemsTable = request.service.tables.getTable('TodoItem');

> [AZURE.NOTE] 共有関数は、**tables** オブジェクトに直接アクセスできません。 共有関数内で tables オブジェクトを関数に渡す必要があります。

[テーブルのオブジェクト] を作成したら、1 つまたは複数のテーブル操作関数を呼び出すことができます。 挿入、更新、削除または読み取る。 次の例では、permissions テーブルからユーザーのアクセス許可を読み取ります。

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

次の例では、**audit** テーブルに監査情報を書き込みます。

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

最後の例は、ここで、コード サンプルには: [方法: カスタム パラメーターにアクセス ][how to: add custom parameters]します。

### <a name="bulk-inserts"></a>方法: 一括挿入を実行

使用する場合、 **の** または **ながら** ループのテーブルに項目 (1,000 個など) の数が多いを直接挿入する、SQL 接続の制限に達して一部の挿入に失敗が発生する可能性があります。 要求が完了しないか、HTTP 500 内部サーバー エラーが返されます。 この問題を回避するには、一度に 10 個程度の項目をバッチとして挿入します。 1 つのバッチが挿入されるまで待ってから、次のバッチを挿入するようにします。

次のスクリプトを使用すると、レコードのバッチのサイズを設定して、並列挿入することができます。 レコードの数は大きくしないことをお勧めします。 非同期挿入バッチが完了すると、**insertItems** 関数が自身を再帰的に呼び出します。 最後にループでは、時刻、および呼び出しに 1 つのレコードを挿入の **insertComplete** 成功した場合と **errorHandler** エラーが発生します。 **insertComplete**  コントロールかどうか **insertItems** 再帰的に、次のバッチで呼び出されるかどうかまたは、ジョブを完了して、スクリプトが終了する必要があります。

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

### <a name="JSON-types"></a>方法: マップ JSON 型をデータベース型

クライアントとMobile Services データベース テーブルでは、データ型のコレクションが異なります。 データ型を簡単にマッピングできる場合もあれば、難しい場合もあります。 Mobile Services では、マッピングによってさまざまな型変換を実行します。

- クライアントの言語固有の型は JSON にシリアル化されます。
- JSON の表現は JavaScript に変換された後でサーバー スクリプトに使用されます。
- JavaScript のデータ型は、[tables オブジェクト] を使用して保存されたときに SQL データベースの型に変換されます。

クライアント スキーマから JSON への変換方法は、プラットフォームによって異なります。 Windows ストア クライアントと Windows Phone クライアントでは、JSON.NET が使用されます。 Android クライアントでは gson ライブラリが使用されます。 iOS クライアントでは NSJSONSerialization クラスが使用されます。 このようなライブラリでは既定のシリアル化動作が使用されます。ただし、日付のオブジェクトを、ISO 8601 を使用してエンコードされた日付が含まれている JSON 文字列に変換する場合は例外です。

[挿入] を使用するサーバー スクリプトを作成するときに [更新]、[読み取り] または [削除]、関数、データの JavaScript 表現にアクセスすることができます。 モバイル サービスは Node.js の逆シリアル化関数を使用して ([JSON.parse](http://es5.github.io/#x15.12)) 、ネットワーク上で JSON を JavaScript オブジェクトに変換します。 ただし、モバイル サービスが抽出する変換 **日付** ISO 8601 文字列からオブジェクトです。

[テーブルのオブジェクト] または [mssql オブジェクト] を使用するか、または自動的にテーブル スクリプトを実行時に、逆シリアル化された JavaScript オブジェクトは、SQL データベースに挿入されます。 このプロセスでは、オブジェクトのプロパティが T-SQL 型にマッピングされます。

 JavaScript のプロパティ| T-SQL 型
---|---
 Number| Float(53)
 Boolean| Bit
 Date| DateTimeOffset(3)|
 String| Nvarchar(max)
 Buffer| サポートされていません
 オブジェクト| サポートされていません
 Array| サポートされていません
 Stream| サポートされていません

### <a name="TSQL"></a>TRANSACT-SQL を使用してテーブルにアクセスする

[テーブルのオブジェクト] のプロキシを使用してサーバー スクリプトからのデータに作業テーブルに最も簡単な方法です。 ただし、あるより高度なシナリオでサポートされていない [テーブル オブジェクト] など、結合クエリ、およびその他の複雑なクエリとストアド プロシージャの呼び出しとしてをします。 このような場合に、[mssql オブジェクト] を使用して、リレーショナル テーブルに対して直接 Transact SQL ステートメントを実行する必要があります。 このオブジェクトには、以下の関数が用意されています。

- **query**: TSQL 文字列で指定されたクエリを実行します。結果は **options** オブジェクトに対する **success** コールバックに返されます。 *params* パラメーターがある場合、クエリにパラメーターを含めることができます。
- **queryRaw**: *query* と同様ですが、クエリから返される結果セットが `未加工` 形式である点が異なります (以下の例を参照)。
- **open**: Mobile Services データベースへの接続を取得する場合に使用します。この connection オブジェクトを使用して、トランザクションなどのデータベース操作を呼び出すことができます。

以下の方法は、下のものほど、クエリ処理に対して低レベルの制御を行うことができます。

+ [方法: 静的クエリを実行]
+ [方法: 動的クエリを実行]
+ [方法: リレーショナル テーブルを結合]
+ [方法: を返すクエリを実行 *生* 結果]
+ [方法: データベース接続へのアクセスを取得]

#### <a name="static-query"></a>方法: 静的クエリを実行

次のクエリ パラメーターを持たないし、から 3 つのレコードを返す、 `statusupdate` テーブルです。 行セットは標準の JSON 形式です。

        mssql.query('select top 3 * from statusupdates', {
            success: function(results) {
                console.log(results);
            },
            error: function(err) {
                console.log("error is: " + err);
            }
        });

#### <a name="dynamic-query"></a>方法: 動的パラメーター化されたクエリを実行

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

#### <a name="joins"></a>方法: リレーショナル テーブルを結合

使用して 2 つのテーブルを結合することができます、 **クエリ** 、結合を実装する TSQL コードを渡す [mssql オブジェクト] のメソッドです。 **ToDoItem** テーブルにいくつかの項目があり、各項目には **priority** プロパティがあって、テーブルの列に対応しているとします。 項目は次のようになります。

        { text: 'Take out the trash', complete: false, priority: 1}

また、**Priority** という名前の追加のテーブルがあり、優先度の**番号**とテキストの**説明**を含む行があるとします。 たとえば、優先度番号 1 の説明は "Critical" で、オブジェクトは次のようになります。

        { number: 1, description: 'Critical'}

ここで、項目の**優先度**番号を、そのテキスト説明に置き換えることができます。 そうするには、2 つのテーブルのリレーショナル結合を使用します。

        mssql.query('SELECT t.text, t.complete, p.description FROM ToDoItem as t INNER JOIN Priority as p ON t.priority = p.number', {
            success: function(results) {
                console.log(results);
            },
            error: function(err) {
                console.log("error is: " + err);
        });

スクリプトは 2 つのテーブルを結合し、結果をログに書き込みます。 結果のオブジェクトは、次のようになります。

        { text: 'Take out the trash', complete: false, description: 'Critical'}

#### <a name="raw"></a>方法: を返すクエリを実行 *生* 結果

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

#### <a name="connection"></a>方法: データベース接続へのアクセスの取得

**open** メソッドを使用して、データベース接続にアクセスできます。 このようにする理由の 1 つとして考えられるのは、データベース トランザクションを使用する必要がある場合です。

**open** の実行が成功すると、データベース接続がパラメーターとして **success** 関数に渡されます。 **connection** オブジェクトに対して呼び出すことができる関数は、*close*、*queryRaw*、*query*、*beginTransaction*、*commit*、および *rollback* です。

            mssql.open({
                success: function(connection) {
                    connection.query(//query to execute);
                },
                error: function(err) {
                    console.log("error is: " + err);
                }
            });

## <a name="debugging"></a>デバッグとトラブルシューティング

サーバー スクリプトをデバッグおよびトラブルシューティングするための主な方法は、サービス ログへの書き込みです。 既定では、Mobile Services は、サービス スクリプトの実行中に発生したエラーをサービス ログに書き込みます。 また、スクリプトでログに書き込むこともできます。 ログへの書き込みは、スクリプトをデバッグし、適切に動作しているかどうかを検証するための優れた方法です。

### <a name="write-to-logs"></a>方法: モバイル サービス ログに出力を書き込みます

グローバルを使用して、ログに書き込むには、[コンソール object] です。 情報レベルの警告をログに記録するには、**log** 関数または **info** 関数を使用します。 **warning** 関数および **error** 関数では、それぞれのレベルをログに記録します。これらは、ログ内で強調されます。
> [AZURE.NOTE] ログオン、モバイル サービスのログを表示する、 [Azure クラシック ポータル](https://manage.windowsazure.com/), 、モバイル サービスを選択してを選択し、 **ログ** ] タブをクリックします。

パラメーターを使用して、メッセージの書式設定を [console オブジェクト] のログ関数を使用することもできます。 次の例では、メッセージ文字列のパラメーターとして JSON オブジェクトを指定します。

    function insert(item, user, request) {
        console.log("Inserting item '%j' for user '%j'.", item, user);  
        request.execute();
    }

注意して文字列 `%j` ように、JSON オブジェクトとそのパラメーターのプレース ホルダーは順番に指定を使用します。

ログが過大になることを避けるには、運用環境で使用する必要がない console.log() への呼び出しを削除または無効にする必要があります。




[introduction]: #intro 
[table operations]: #table-scripts 
[basic table operations]: #basic-table-ops 
[how to: register for table operations]: #register-table-scripts 
[how to: define table scripts]: #execute-operation 
[how to: override the default response]: #override-response 
[how to: modify an operation]: #modify-operation 
[how to: override success and error]: #override-success-error 
[how to: override execute success]: #override-success 
[how to: override default error handling]: #override-error 
[how to: access tables from scripts]: #access-tables 
[how to: add custom parameters]: #access-headers 
[how to: work with users]: #work-with-users 
[how to: define scheduled job scripts]: #scheduler-scripts 
[how to: refine access to tables]: #authorize-tables 
[using transact-sql to access tables]: #TSQL 
[how to: run a static query]: #static-query 
[how to: run a dynamic query]: #dynamic-query 
[how to: run a query that returns *raw* results]: #raw 
[how to: get access to a database connection]: #connection 
[how to: join relational tables]: #joins 
[how to: perform bulk inserts]: #bulk-inserts 
[how to: map json types to database types]: #JSON-types 
[how to: load node.js modules]: #modules-helper-functions 
[how to: write output to the mobile service logs]: #write-to-logs 
[source control, shared code, and helper functions]: #shared-code 
[using the command line tool]: #command-prompt 
[working with tables]: #working-with-tables 
[custom api anchor]: #custom-api 
[how to: define a custom api]: #define-custom-api 
[how to: share code by using source control]: #shared-code-source-control 
[how to: use helper functions]: #helper-functions 
[debugging and troubleshooting]: #debugging 
[how to: implement http methods]: #handle-methods 
[how to: work with users and headers in a custom api]: #get-api-user 
[how to: access custom api request headers]: #get-api-headers 
[job scheduler]: #scheduler-scripts 
[how to: define multiple routes in a custom api]: #api-routes 
[how to: send and receive data as xml]: #api-return-xml 
[how to: work with app settings]: #app-settings 
[1]: ./media/mobile-services-how-to-use-server-scripts/1-mobile-insert-script-users.png 
[2]: ./media/mobile-services-how-to-use-server-scripts/2-mobile-custom-api-script.png 
[3]: ./media/mobile-services-how-to-use-server-scripts/3-mobile-schedule-job-script.png 
[4]: ./media/mobile-services-how-to-use-server-scripts/4-mobile-source-local-cli.png 
[mobile services server script reference]: http://msdn.microsoft.com/library/windowsazure/jj554226.aspx 
[schedule backend jobs in mobile services]: /develop/mobile/tutorials/schedule-backend-tasks/ 
[request object]: http://msdn.microsoft.com/library/windowsazure/jj554218.aspx 
[response object]: http://msdn.microsoft.com/library/windowsazure/dn303373.aspx 
[user object]: http://msdn.microsoft.com/library/windowsazure/jj554220.aspx 
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
[read and write data]: http://msdn.microsoft.com/library/windowsazure/jj631640.aspx 
[validate data]: http://msdn.microsoft.com/library/windowsazure/jj631638.aspx 
[modify the request]: http://msdn.microsoft.com/library/windowsazure/jj631635.aspx 
[modify the response]: http://msdn.microsoft.com/library/windowsazure/jj631631.aspx 
[azure classic portal]: https://manage.windowsazure.com/ 
[schedule jobs]: http://msdn.microsoft.com/library/windowsazure/jj860528.aspx 
[validate and modify data in mobile services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-dotnet/ 
[commands to manage azure mobile services]: ../virtual-machines-command-line-tools.md#Mobile_Scripts 
[windows store push]: /develop/mobile/tutorials/get-started-with-push-dotnet/ 
[windows phone push]: /develop/mobile/tutorials/get-started-with-push-wp8/ 
[ios push]: /develop/mobile/tutorials/get-started-with-push-ios/ 
[android push]: /develop/mobile/tutorials/get-started-with-push-android/ 
[azure sdk for node.js]: http://go.microsoft.com/fwlink/p/?LinkId=275539 
[send http request]: http://msdn.microsoft.com/library/windowsazure/jj631641.aspx 
[send email from mobile services with sendgrid]: /develop/mobile/tutorials/send-email-with-sendgrid/ 
[get started with authentication]: http://go.microsoft.com/fwlink/p/?LinkId=287177 
[crypto api]: http://go.microsoft.com/fwlink/p/?LinkId=288802 
[path api]: http://go.microsoft.com/fwlink/p/?LinkId=288803 
[querystring api]: http://go.microsoft.com/fwlink/p/?LinkId=288804 
[url api]: http://go.microsoft.com/fwlink/p/?LinkId=288805 
[util api]: http://go.microsoft.com/fwlink/p/?LinkId=288806 
[zlib api]: http://go.microsoft.com/fwlink/p/?LinkId=288807 
[custom api]: http://msdn.microsoft.com/library/windowsazure/dn280974.aspx 
[call a custom api from the client]: /develop/mobile/tutorials/call-custom-api-dotnet/#define-custom-api 
[express.js library]: http://go.microsoft.com/fwlink/p/?LinkId=309046 
[define a custom api that supports periodic notifications]: /develop/mobile/tutorials/create-pull-notifications-dotnet/ 
[express object in express.js]: http://expressjs.com/api.html#express 
[store server scripts in source control]: /develop/mobile/tutorials/store-scripts-in-source-control/ 
[leverage shared code and node.js modules in your server scripts]: /develop/mobile/tutorials/store-scripts-in-source-control/#use-npm 
[service object]: http://msdn.microsoft.com/library/windowsazure/dn303371.aspx 
[app settings]: http://msdn.microsoft.com/library/dn529070.aspx 
[config module]: http://msdn.microsoft.com/library/dn508125.aspx 
[support for package.json in azure mobile services]: http://go.microsoft.com/fwlink/p/?LinkId=391036 

