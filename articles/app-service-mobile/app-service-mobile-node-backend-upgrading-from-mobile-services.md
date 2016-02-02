<properties
    pageTitle="Mobile Services から Azure App Service (Node.js) へのアップグレード"
    description="簡単に Mobile Services アプリケーションを App Service モバイル アプリにアップグレードする方法について説明します。"
    services="app-service\mobile"
    documentationCenter=""
    authors="christopheranderson"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="node"
    ms.topic="article"
    ms.date="12/02/2015"
    ms.author="chrande"/>


# App Service への既存の Node.js Azure Mobile Service のアップグレード

App Service Mobile は、Microsoft Azure を使用してモバイル アプリケーションを構築する新しい方法です。 詳細については、[Mobile Apps は何ですか?] を参照してください。

このトピックでは、既存の Node.js バックエンド アプリケーションを Azure Mobile Services から新しい App Service Mobile Apps にアップグレードする方法について説明します。 このアップグレードの実行中も、既存の Mobile Services アプリケーションの動作を続行できます。

すべての App Service の機能にアクセスし、[App Service の料金] に応じて課金されます、モバイル バックエンドを Azure App Service にアップグレードするとき、モバイル サービスではなく価格設定します。

## 移行とアップグレード

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]
>[AZURE.TIP] お勧めする [ěúťsc](app-service-mobile-migrating-from-mobile-services.md) アップグレードを実行する前にします。 そうすることで、同じ App Service プランに両方のバージョンのアプリケーションを指定できるため、追加コストが発生しません。

### Mobile Apps Node.js サーバー SDK の機能強化

新しいへのアップグレード [Mobile Apps SDK](https://www.npmjs.com/package/azure-mobile-apps) など、機能強化の多くを提供します。

- 基づく [フレームワーク Express](http://expressjs.com/en/index.html), 、新しいノードの SDK は軽量と最新情報を新しいバージョンのノードたかとよう設計されています。 Express ミドルウェアでアプリケーションの動作をカスタマイズすることができます。

- Mobile Services SDK に比べ、パフォーマンスが大幅に向上します。

- モバイル バックエンドと共に Web サイトをホストできるようになりました。同様に、既存の expressv4 アプリケーションに Azure Mobile SDK を簡単に追加することができます。

- クロスプラットフォームとローカル開発用に構築されており、Mobile Apps SDK を開発して Windows、Linux、および OSX プラットフォーム上でローカルに実行できます。 実行中のような一般的なノード開発手法を簡単に使用では今すぐ [Mocha](https://mochajs.org/) 展開する前にテストします。

- ネイティブ モジュールで Redis を使用できる [hiredis](https://www.npmjs.com/package/hiredis); とため、App Service は、npm パッケージで自動的にインストール、展開するときに、展開パッケージにバイナリを含める必要はありません。

## <a name="overview"></a>基本的なアップグレードの概要

.NET Mobile Apps SDK とは異なり、Node バックエンドを Mobile Services から Mobile Apps にアップグレードすることは、パッケージ交換のように単純なものではありません。 Azure で制御するのではなく、ユーザーがアプリケーション スタック全体を所有するようになったため、基本的な Express アプリを作成してモバイル バックエンドをホストする必要があります。 テーブルと API コントローラーについては、概念は似ていますが、テーブル オブジェクトのエクスポートが必要になり、関数 API が多少変わりました。 この記事では、アップグレードの基本的な戦略を通じて説明が、移行する前に、読みたい、 [ノード バックエンドに関する「方法」](app-service-mobile-node-backend-how-to-use-server-sdk.md) 作業の開始前にします。
>[AZURE.TIP] アップグレードを開始する前に、このトピックの残りの部分を読み、よく理解しておくことをお勧めします。 下に列記した機能のうち、使用する機能をすべてメモしてください。

Mobile Services クライアント SDK と新しい Mobile Apps サーバー SDK の間に互換性は**ありません**。 アプリ用にサービスを接続する場合は、発行されたクライアントを現在使用しているサイトに変更を発行しないでください。 代わりに、複製として機能する新しいモバイル アプリを作成する必要があります。 このアプリケーションを同じ App Service プランに指定することで、追加の財務費用が発生しないようにすることができます。

アプリケーションには 2 つのバージョンがあります。1 つは変わらず、発行されたアプリとしてそのままの状態で機能し、もう 1 つはユーザーがアップグレードし、新しいクライアント リリースのターゲットにすることができます。 自分のペースでコードを移動し、テストできますが、バグの修正が両方に適用されることを確認する必要があります。 必要な数の (そのままの状態の) クライアント アプリを最新バージョンに更新したら、必要に応じて、元の移行されたアプリを削除することができます。 モバイル アプリと同じ App Service プランでホストされている場合は、追加の金銭的コストが発生することはありません。

このアップグレード プロセスの全概要は、次のとおりです。

1. 新しい Mobile App を作成する
2. 新しい Server SDK を使用するようにプロジェクトを更新する
3. クライアント アプリケーションの新しいバージョンをリリースする
4. (省略可能) 元の移行されたモバイル サービス アプリを削除する

## <a name="mobile-app-version"></a> アップグレードを開始

アップグレードの最初のステップは、新しいバージョンのアプリケーションをホストするモバイル アプリ リソースを作成することです。 既存のモバイル サービスを既に移行している場合は、同じホスティング プランでこのバージョンを作成します。 [Azure ポータル] を開き、移行済みのアプリケーションに移動します。 実行されている App Service プランをメモしてをおきます。

### 2 番目のアプリケーション インスタンスの作成

次に、2 番目のアプリケーション インスタンスを作成します。 App Service プランまたは "ホスティング プラン" を選択するよう求められたら、移行済みアプリケーションのプランを選択します。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

Mobile Services で使用したのと同じデータベースと通知ハブを使用することもできます。 [Azure ポータル] を開き、元のアプリケーションに移動してこれらの値をコピーし、をクリックできます **設定** > **アプリケーション設定**します。  **接続文字列**, 、コピー `MS_NotificationHubConnectionString` と `MS_TableConnectionString`します。 新しいアップグレード サイトに移動し、接続文字列を貼り付けて既存の値を上書きします。 アプリに必要な他のアプリケーション設定について、このプロセスを繰り返します。 移行したサービスを使用していない場合、接続文字列とのアプリ設定を読み取ることができます、 **構成** [Azure ポータル] の [モバイル サービス] セクションのタブをクリックします。

### Node を持つ基本的なモバイル アプリ バックエンドを作成する

Azure App Service Mobile Apps の Node.js バックエンドはすべて ExpressJS アプリケーションとして開始されます。 基本的なを作成する [Express](http://expressjs.com/en/index.html) 次のようにアプリケーション。

1. コマンドまたは PowerShell ウィンドウで、プロジェクト用の新しいディレクトリを作成します。

        mkdir basicapp

2. npm init を実行して、パッケージの構造を初期化します。

        cd basicapp
        npm init

    npm init コマンドでは、プロジェクトを初期化するための一連の質問が示されます。 以下の出力例を参照してください。

    ![npm init の出力][0]

3. npm リポジトリから express および azure-mobile-apps ライブラリをインストールします。

        npm install --save express azure-mobile-apps

4. app.js ファイルを作成して、基本的なモバイル サーバーを実装します。

     var express = require('express'),
         azureMobileApps = require('azure-mobile-apps');
    
     var app = express(),
         mobile = azureMobileApps();
    
     // Important all tables in the 'tables' directory
     mobile.tables.import('./tables');
     mobile.api.import('./api');
    
     // Provide initialization of any tables that are statically defined
     mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);
    
        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
        console.log('Now listening on ' + (process.env.PORT || 3000)));
     });



## サーバー プロジェクトの更新

Mobile Apps は、新しい [モバイル アプリ サーバー SDK] Mobile Services ランタイムと同じ機能の多くを提供するが、完全な実行時は自分が所有するようになりましたモバイル アプリでは、ノードのバージョンまたは任意のコードの更新を強制しないです。 上記の手順に従った場合、基本バージョンの Node モバイル ランタイムを使用できます。 これで、テーブルと API ロジックの Mobile Service からモバイル アプリへの移動、サーバー構成のカスタマイズ、プッシュの有効化、認証構成などを開始できます。

### 基本構成

サーバーには多数の構成設定がありますが、さまざまな既定値を使用することで作業を簡単に開始できます。 設定の多くはでセットアップする、[Azure ポータル、] を使用して、 **データ**, 、**認証/承認**, 、および **プッシュ** 設定メニュー。 ローカル開発では、データ、認証、およびプッシュを使用する場合に、ローカル開発環境の構成が必要になることがあります。

モバイル アプリ バックエンドの [アプリケーション設定] を使用して設定できる環境変数でサーバーを構成できます。

渡すことによって、Mobile Apps SDK をさらにカスタマイズすることができます、 [構成オブジェクト](http://azure.github.io/azure-mobile-apps-node/global.html#configuration) 、初期化子または [azureMobile.js という名前のファイルを作成する](app-service-mobile-node-backend-how-to-use-server-sdk/#howto-config-localdev) プロジェクトのルートにします。

### データとテーブルの操作

SDK には、作業を素早く簡単に開始できるようにするインメモリ データ プロバイダーが付属しています。 インメモリ プロバイダーでは再始動時にすべてのデータが失われ、複数のインスタンス間の整合性が保たれないため、早い段階で SQL DB の使用に切り替える必要があります。

モバイル アプリにモバイル サービスからビジネス ロジックの移動を開始するありますを初めて使用するテーブルの名前 ('.js' が付いた) のファイルを作成、 `./[テーブル` ディレクトリ。 モバイル アプリのテーブルの完全な例を確認できます [GitHub](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/todo/tables/TodoItem.js)します。 最も単純なバージョンは次のとおりです。

    var azureMobileApps = require('azure-mobile-apps');
    
    // Create a new table definition
    var table = azureMobileApps.table();
    
    module.exports = table;

各移植をロジックの一部を開始する、 `< tablename >. < 操作 > .js`, 、関数をテーブルにする必要があります。たとえば、read 関数を追加するとします。

Mobile Service で、TodoItem テーブルと読み取り操作を使用して、ユーザー ID に基づいて項目をフィルター処理する場合は、以下のようになります。

  関数 (クエリ、ユーザーの要求) {
    query.where ({userId: user.userId})。
    request.execute();
  {

Azure Mobile Apps テーブル コードに追加する関数は以下のようになります。

    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

コードを調べれば、ほとんどの関数パラメーターを確認できます。

### CORS

使用して、CORS を有効にすることができます、 [CORS の構成設定](http://azure.github.io/azure-mobile-apps-node/global.html#corsConfiguration) 、SDK に含まれています。

CORS を使用する場合は、重大な問題の主な領域が、 `eTag` と `場所` クライアント Sdk の適切に機能するためのヘッダーを許可する必要があります。

### プッシュ通知

Mobile Services 以降、Azure Notification Hubs SDK では大幅に更新されているものがいくつかあるため、一部の Notification Hubs 関数シグネチャが異なる場合があります。 それ以外の場合、機能はモバイル サービスに似ていますAzure Mobile SDK は通知ハブ用のアプリ設定が存在する場合は、通知ハブ インスタンスをプロビジョニングしで公開 `context.push`します。 サンプルを参照して [GitHub](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/push-on-insert/tables/TodoItem.js), 、次に示す該当するセクションを次に示すとします。

    table.insert(function (context) {
        // For details of the Notification Hubs JavaScript SDK,
        // see https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-nodejs-how-to-use-notification-hubs/
        logger.silly('Running TodoItem.insert');
    
        // This push uses a template mechanism, so we need a template/
        var payload = '<toast><visual><binding template="Toast01"><text id="1">INSERT</text></binding></visual></toast>';
    
        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    context.push.wns.send(null, payload, 'wns/toast', function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.silly('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
    });

### スケジュールされたジョブ

モバイル サービス バックエンド内にある既存のジョブが個別にアップグレードする必要がありますので、スケジュールされたジョブはモバイル アプリに組み込まれません。 1 つのオプションでは、モバイル アプリ コード サイトで [Web ジョブ] スケジュールを作成します。 ジョブ コードが含まれる API を設定し、[必要なスケジュールでそのエンドポイントをヒットする [Azure スケジューラ] を構成することもできますが。

## <a name="authentication"></a>認証に関する注意点

Mobile Services の認証コンポーネントは、App Service の認証/承認機能に移動されました。 読み取ることによって、サイトに対して有効にこれについて、 [モバイル アプリへの認証の追加](app-service-mobile-ios-get-started-users.md) トピックです。

AAD、Facebook、Google などの一部のプロバイダーでは、コピー アプリケーションから既存の登録を利用できます。 ID プロバイダーのポータルに移動して、新しいリダイレクト URL を登録に追加するだけです。 その後、クライアント ID とシークレットを使用して、App Service の認証/承認を構成します。

### コントローラー アクションの認証とユーザー ID

テーブルへのアクセスを制限することができますに設定する、テーブル レベル `table.access = '認証'`します。 完全な例を確認できます [GitHub](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/authentication/tables/TodoItem.js)します。

使用してユーザーの id 情報にアクセスできるように、 `user.getIdentity` 説明した方法 [ここ](http://azure.github.io/azure-mobile-apps-node/module-azure-mobile-apps_auth_user.html#~getIdentity)します。

## <a name="updating-clients"></a>クライアントの更新

モバイル アプリ バックエンドを運用している場合は、それを利用する新しいバージョンのクライアント アプリケーションを使用できます。 Mobile Apps には新しいバージョンのクライアント SDK も含まれ、上記のサーバー アップグレードと同様に、Mobile Apps バージョンをインストールする前に Mobile Services SDK へのすべての参照を削除する必要があります。

バージョン間での主な変更の 1 つは、コンストラクターでアプリケーション キーが不要になったことです。 現在は、モバイル アプリの URL を渡すだけです。 たとえば、.NET クライアントで、 `MobileServiceClient` コンス トラクターは、今すぐ。

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

新しい SDK のインストールおよび新しい構造の使用については、以下のリンクを介して参照できます。

- [iOS バージョン 3.0.0 以降](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) バージョン 2.0.0 またはそれ以降](app-service-mobile-dotnet-how-to-use-client-library.md)

アプリケーションでプッシュ通知を使用する場合は、変更されている内容もあるため、プラットフォームごとに固有の登録手順をメモしてください。

新しいクライアント バージョンの準備ができたら、アップグレードしたサーバー プロジェクトで試してみます。 機能することを確認したら、新しいバージョンのアプリケーションを顧客にリリースできます。 最後に、顧客がこれらの更新プログラムを受け取ったら、Mobile Services バージョンのアプリを削除できます。 これで、最新の Mobile Apps サーバー SDK を使用する App Service モバイル アプリに完全にアップグレードできました。





[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png 
[azure portal]: https://portal.azure.com/ 
[azure classic portal]: https://manage.windowsazure.com/ 
[what are mobile apps?]: app-service-mobile-value-prop.md 
[i already use web sites and mobile services – how does app service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services 
[mobile app server sdk]: https://www.npmjs.com/package/azure-mobile-apps 
[create a mobile app]: app-service-mobile-xamarin-ios-get-started.md 
[add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md 
[add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md 
[azure scheduler]: /en-us/documentation/services/scheduler/ 
[web job]: ../app-service-web/websites-webjobs-resources.md 
[how to use the .net server sdk]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md 
[migrate from mobile services to an app service mobile app]: app-service-mobile-migrating-from-mobile-services.md 
[migrate your existing mobile service to app service]: app-service-mobile-migrating-from-mobile-services.md 
[app service pricing]: https://azure.microsoft.com/en-us/pricing/details/app-service/ 
[.net server sdk overview]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md 
[azure portal]: https://portal.azure.com/ 
[odata]: http://www.odata.org 
[promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise 
[basicapp sample on github]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app 
[todo sample on github]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo 
[samples directory on github]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples 
[static-schema sample on github]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema 
[queryjs]: https://github.com/Azure/queryjs 
[node.js tools 1.1 for visual studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1 
[mssql node.js package]: https://www.npmjs.com/package/mssql 
[microsoft sql server 2014 express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx 
[expressjs middleware]: http://expressjs.com/guide/using-middleware.html 
[winston]: https://github.com/winstonjs/winston 

