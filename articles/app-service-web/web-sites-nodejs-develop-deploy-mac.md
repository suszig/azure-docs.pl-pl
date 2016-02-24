<properties
    pageTitle="Azure App Service での Node.js Web アプリの作成 | Microsoft Azure"
    description="Node.js アプリケーションを Azure App Service の Web アプリにデプロイする方法を説明します。"
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.date="11/20/2015"
    ms.author="robmcm"/>

# Azure App Service での Node.js Web アプリの作成

> [AZURE.SELECTOR]
- [.Net](web-sites-dotnet-get-started.md)
- [Node.js](web-sites-nodejs-develop-deploy-mac.md)
- [Java](web-sites-java-get-started.md)
- [PHP - Git](web-sites-php-mysql-deploy-use-git.md)
- [PHP - FTP](web-sites-php-mysql-deploy-use-ftp.md)
- [Python](web-sites-python-ptvs-django-mysql.md)

このチュートリアルは、単純なを作成する方法を示します [Node.js](http://nodejs.org) アプリケーションを展開し、 [web アプリ](app-service-web-overview.md) で [Azure App Service](../app-service/app-service-value-prop-what-is.md) を使用して [Git](http://git-scm.com)します。 このチュートリアルの手順は、Node.js を実行できる任意のオペレーティング システムで使用できます。

学習内容:

* Azure ポータルを使用して Azure App Service で Web アプリを作成する方法
* Node.js アプリケーションを Web アプリの Git リポジトリにプッシュすることでその Web アプリにデプロイする方法

完成したアプリケーションにより、ブラウザーに "hello world" という短い文字列が出力されます。

!["Hello World" メッセージを表示しているブラウザー][helloworld-completed]

チュートリアルより複雑な Node.js アプリケーションとサンプル コード、または Azure で Node.js を使用する方法に関するその他のトピックを参照してください、 [Node.js デベロッパー センター](/develop/nodejs/)します。

> [AZURE.NOTE]
> このチュートリアルを完了するには、Microsoft Azure アカウントが必要です。 アカウントを持っていない場合は、[Visual Studio サブスクライバーの特典を有効にする](/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)か、[無料試用版にサインアップ](/en-us/pricing/free-trial/?WT.mc_id=A261C142F)してください。
>
> 場合は、Azure アカウントにサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751)します。 有効期間が短いスターター Web アプリを App Service ですぐに作成できます。このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## Web アプリの作成と Git 発行の有効化

Azure App Service で Web アプリを作成して、Git 発行を有効にするには、次の手順に従います。 

[Git](http://git-scm.com/) は Azure web サイトの配置に使用できる分散型バージョン コントロール システムです。 Web アプリ用に記述したコードはローカルの Git リポジトリに格納されます。このコードをリモート リポジトリにプッシュして Azure にデプロイします。 このデプロイ方法は、App Service Web アプリの特徴です。  

1. サインイン、 [Azure ポータル](https://portal.azure.com)します。

2. クリックして、 **+ 新規** 上にアイコンが、Azure ポータルの左します。

3. クリックして **Web + モバイル**, 、] をクリックし、 **Web アプリ**します。

    ![][portal-quick-create]

4. Web アプリの名前を入力、 **Web アプリ** ボックス。

    Web アプリの URL は {name}.azurewebsites.net のようになるため、この名前は azurewebsites.net ドメイン内で一意である必要があります。 入力した名前が一意でない場合は、テキスト ボックスに赤色の感嘆符が表示されます。

5. 選択、 **サブスクリプション**します。

6. 選択、 **リソース グループ** か新規に作成します。

    リソース グループの詳細については、次を参照してください。 [Azure ポータルを使用して、Azure リソースを管理する](../resource-group-portal.md)です。

7. 選択、 **App Service プラン/場所** か新規に作成します。

    App Service プランに関する詳細については、次を参照してください [Azure App Service プランの概要。](../azure-web-sites-web-hosting-plans-in-depth-overview.md)

8. クリックして **作成**します。
   
    ![][portal-quick-create2]

    短時間 (通常は 1 分未満) で、新しい Web アプリの作成が完了します。

9. クリックして **Web アプリ > {新しい web アプリ}**します。

    ![](./media/web-sites-nodejs-develop-deploy-mac/gotowebapp.png)

10.  **Web アプリ** ブレードで、をクリックして、 **展開** 部分です。

    ![][deployment-part]

11.  **継続的なデプロイ** ブレードで、をクリックして **ソースの選択**

12. をクリックして **ローカル Git リポジトリ**, 、] をクリックし、 **OK**します。

    ![][setup-git-publishing]

13. デプロイ資格情報をまだ設定していない場合は設定します。

    a. Web アプリのブレードでクリックして **設定 > デプロイ資格情報**します。

    ![][deployment-credentials]
 
    b. ユーザー名とパスワードを作成します。 
    
    ![](./media/web-sites-nodejs-develop-deploy-mac/setdeploycreds.png)

14. Web アプリのブレードでクリックして **設定**, 、クリックして **プロパティ**します。
 
    発行するには、リモート Git リポジトリにプッシュします。 リポジトリの URL が下に表示 **GIT URL**します。 この URL は、チュートリアルの後半で使用します。

    ![][git-url]

## アプリケーションの作成とローカル テスト

このセクションで作成した、 **server.js** を少し変更したバージョンの [nodejs.org] から"Hello World"例を含むファイル。 コードでは、Azure Web アプリでの実行時にリッスンするポートとして process.env.PORT が追加されます。

1. という名前のディレクトリを作成する *helloworld*します。

2. という名前の新しいファイルを作成するには、テキスト エディターを使用して **server.js** で、 *helloworld* ディレクトリ。

2. 次のコードをコピー、 **server.js** ファイルを開き、ファイルを保存します。

        var http = require('http')
        var port = process.env.PORT || 1337;
        http.createServer(function(req, res) {
          res.writeHead(200, { 'Content-Type': 'text/plain' });
          res.end('Hello World\n');
        }).listen(port);

3. コマンド ラインを開き、次のコマンドを使用してローカルで Web アプリを開始します。

        node server.js

4. Web ブラウザーを開き、http://localhost:1337/ に移動します。 

    次のスクリーンショットに示すように、"Hello World" と表示された Web ページが開きます。

    !["Hello World" メッセージを表示しているブラウザー][helloworld-localhost]

## アプリケーションの発行

1. Git をまだインストールしていない場合はインストールします。

    お使いのプラットフォームのインストール手順については、次を参照してください。、 [Git のダウンロード ページ](http://git-scm.com/download)します。

1. コマンド ラインからのディレクトリに移動、 **helloworld** ディレクトリとローカル Git リポジトリを初期化するために、次のコマンドを入力します。

        git init


2. 次のコマンドを使用して、リポジトリにファイルを追加します。

        git add .
        git commit -m "initial commit"

3. 次のコマンドを使用して、先ほど作成した Azure Web アプリに更新をプッシュするために Git リモートを追加します。

        git remote add azure [URL for remote repository]

4. 次のコマンドを使用して、変更内容を Azure にプッシュします。

        git push azure master

    以前作成したパスワードを入力するように求められます。 出力は次の例のようになります。

        Counting objects: 3, done.
        Delta compression using up to 8 threads.
        Compressing objects: 100% (2/2), done.
        Writing objects: 100% (3/3), 374 bytes, done.
        Total 3 (delta 0), reused 0 (delta 0)
        remote: New deployment received.
        remote: Updating branch 'master'.
        remote: Preparing deployment for commit id '5ebbe250c9'.
        remote: Preparing files for deployment.
        remote: Deploying Web.config to enable Node.js activation.
        remote: Deployment successful.
        To https://user@testsite.scm.azurewebsites.net/testsite.git
         * [new branch]      master -> master

5. アプリを表示する] をクリックして、 **参照** のボタンでは、 **Web アプリ** Azure ポータルの一部です。

    ![[参照] ボタン](./media/web-sites-nodejs-develop-deploy-mac/browsebutton.png)

    ![Azure、Hello World](./media/web-sites-nodejs-develop-deploy-mac/helloworldazure.png)

## アプリケーションへの変更の発行

1. 開いている、 **server.js** をテキスト エディターでファイルを 'こんにちは \n' を 'こんにちは azure \n' に変更します。 

2. ファイルを保存します。

2. コマンド ラインからのディレクトリに移動、 **helloworld** ディレクトリし、次のコマンドを実行します。

        git add .
        git commit -m "changing to hello azure"
        git push azure master

    もう一度パスワードを入力するように求められます。

3. Web アプリの URL に移動したブラウザー ウィンドウを更新します。

    !["Hello Azure" と表示している Web ページ][helloworld-completed]

## デプロイをロールバックします。

 **Web アプリ** ブレードをクリックして **設定 > 継続的なデプロイ** で展開の履歴を表示する、 **展開** ブレードです。 以前のデプロイにロールバックする必要がある場合は、選択してクリックして **を再展開** で、 **の展開の詳細** ブレードです。

## 次のステップ

ここでは、Node.js アプリケーションを Azure App Service の Web アプリにデプロイしました。 App Service web apps での Node.js アプリケーションの実行方法の詳細については、次を参照してください。 [Azure App Service Web Apps: Node.js](http://blogs.msdn.com/b/silverlining/archive/2012/06/14/windows-azure-websites-node-js.aspx) と [Azure アプリケーションでの Node.js のバージョンを指定する](../nodejs-specify-node-version-azure-apps.md)です。

Node.js は、アプリケーションで使用できるモジュールのリッチなエコシステムを実現します。 モジュールと Web アプリがどのように連携するかについては、次を参照してください。 [Azure アプリケーションでの Node.js を使用してモジュール](../nodejs-use-node-modules-azure-apps.md)します。

Azure に展開された後に、アプリケーションで問題が発生した場合は、次を参照してください。 [Azure App Service で Node.js アプリケーションをデバッグする方法について](web-sites-nodejs-debug.md) についてを参照してください。

この記事では、Web アプリの作成に Azure ポータルを使用しています。 使用することも、 [Azure コマンド ライン インターフェイス](../xplat-cli-install.md) または [Azure PowerShell](../install-configure-powershell.md) 同じ操作を実行します。

Azure で Node.js アプリケーションを開発する方法の詳細については、次を参照してください。、 [Node.js デベロッパー センター](/develop/nodejs/)します。

[helloworld-completed]: ./media/web-sites-nodejs-develop-deploy-mac/helloazure.png
[helloworld-localhost]: ./media/web-sites-nodejs-develop-deploy-mac/helloworldlocal.png
[portal-quick-create]: ./media/web-sites-nodejs-develop-deploy-mac/create-quick-website.png
[portal-quick-create2]: ./media/web-sites-nodejs-develop-deploy-mac/create-quick-website2.png
[setup-git-publishing]: ./media/web-sites-nodejs-develop-deploy-mac/setup_git_publishing.png
[go-to-dashboard]: ./media/web-sites-nodejs-develop-deploy-mac/go_to_dashboard.png
[deployment-part]: ./media/web-sites-nodejs-develop-deploy-mac/deployment-part.png
[deployment-credentials]: ./media/web-sites-nodejs-develop-deploy-mac/deployment-credentials.png
[git-url]: ./media/web-sites-nodejs-develop-deploy-mac/git-url.png

