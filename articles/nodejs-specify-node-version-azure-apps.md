<properties
    pageTitle="Node.js のバージョンの指定"
    description="Azure Websites と Cloud Services で使用される Node.js のバージョンを指定する方法を説明します。"
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="11/20/2015"
    ms.author="robmcm"/>

# Azure アプリケーションでの Node.js のバージョンの指定

Node.js アプリケーションをホストするときに、アプリケーションで特定のバージョンの Node.js を使用することが必要になる場合があります。 Azure でホストされるアプリケーションについてこれを行うには、いくつかの方法があります。

##既定のバージョン

Azure が提供する Node.js のバージョンは常に更新されます。 別に指定しない場合、最新のバージョンを使用します。

> [AZURE.NOTE] Azure クラウド サービス (web またはワーカー ロール) でアプリケーションをホストするいるし、アプリケーションを展開した初めてのときは、Azure は Azure で使用できる既定のバージョンのいずれかと一致する場合、開発環境にインストールしている Node.js の同じバージョンを使用しようとします。

##package.json でバージョンを指定する

次のコードを追加することで使用する Node.js のバージョンを指定する、 **package.json** ファイル。

    "engines":{"node":version}

ここで *バージョン* を使用する特定のバージョン番号を指定します。 バージョンについてより複雑な条件をよう指定でくことができます。

    "engines":{"node": "0.6.22 || 0.8.x"}

0.6.22 はホスティング環境で利用可能なバージョンには含まれていないため、0.8 シリーズで利用可能な最も高いバージョンである 0.8.4 が代わりに使用されます。

##アプリケーションの設定で Websites のバージョンを指定する
Web サイトでアプリケーションをホストしている場合は、環境変数を設定することができます **WEBSITE_NODE_DEFAULT_VERSION** を目的のバージョン。 

##PowerShell で Cloud Services のバージョンを指定する

使用して既定の Node.js バージョンをオーバーライドするには、クラウド サービスでアプリケーションをホストして Azure PowerShell を使用してアプリケーションをデプロイする場合、 **Set-azureserviceprojectrole** PowerShell コマンドレット。 次に例を示します。

    Set-AzureServiceProjectRole WebRole1 Node 0.8.4

上記のステートメント内のパラメーターは大文字と小文字が区別されます。  チェックによって、正しいバージョンの Node.js が選択されたことを確認できる、 **エンジン** ロールのプロパティ **package.json**します。

使用することも、 **Get-azureserviceprojectroleruntime** Node.js のバージョンが使用できるクラウド サービスとしてホストされるアプリケーションの一覧を取得します。  プロジェクトが依存している Node.js のバージョンは、この一覧でいつでも確認できます。

##Azure Websites でカスタム バージョンを使用する

Azure には Node.js の既定のバージョンが複数用意されていますが、既定以外のバージョンを使用することもできます。 アプリケーションが Azure Website としてホストされている場合、これを使用して、 **iisnode.yml** ファイルです。 以下の手順では、Azure Website で Node.Js のカスタム バージョンを使用するプロセスを説明します。

1. 新しいディレクトリを作成し、作成、 **server.js** ディレクトリ内のファイルです。  **Server.js** ファイルは、次を含める必要があります。

        var http = require('http');
        http.createServer(function(req,res) {
          res.writeHead(200, {'Content-Type': 'text/html'});
          res.end('Hello from Azure running node version: ' + process.version + '</br>');
        }).listen(process.env.PORT || 3000);

    これにより、Web サイトを閲覧するときに、使用されている Node.js のバージョンが表示されます。

2. 新しい Web サイトを作成し、サイトの名前をメモしておきます。 たとえば、次の用途、 [Azure Command-line tools] という名前の新しい Azure Website を作成する **mywebsite**, 、web サイトに対して Git リポジトリを有効にします。

        azure site create mywebsite --git

3. という名前の新しいディレクトリを作成する **bin** を含んでいるディレクトリの子として、 **server.js** ファイルです。

4. 特定のバージョンをダウンロード **node.exe** (Windows 版) をアプリケーションで使用する場合します。 たとえば、次の用途 **curl** バージョン 0.8.1 をダウンロードします。

        curl -O http://nodejs.org/dist/v0.8.1/node.exe

    保存、 **node.exe** ファイルを **bin** フォルダーの以前に作成します。

5. 作成、 **iisnode.yml** と同じディレクトリ内のファイル、 **server.js** ファイルを開き、次の内容を追加、 **iisnode.yml** ファイル。

        nodeProcessCommandLine: "D:\home\site\wwwroot\bin\node.exe"

    このパスは、where、 **node.exe** Azure web サイトにアプリケーションを発行した、プロジェクト内のファイルが配置されます。

6. アプリケーションを発行します。 たとえば、先ほど --git パラメーターを指定して新しい Web サイトを作成したので、次のコマンドによって、自分のローカル Git リポジトリにアプリケーション ファイルが追加された後、Web サイトのリポジトリにプッシュされます。

        git add .
        git commit -m "testing node v0.8.1"
        git push azure master

    アプリケーションが発行された後、ブラウザーで Web サイトを開きます。 "Hello from Azure running node version: v0.8.1" というメッセージが表示されます。

##次のステップ

説明をアプリケーションによって使用されている Node.js のバージョンを指定する方法を理解する方法 [work with modules], 、[build and deploy a Node.js Web Site], 、および [How to use the Azure Command-Line Tools for Mac and Linux]します。

詳細については、次を参照してください。、 [Node.js デベロッパー センター](/develop/nodejs/)します。

[How to use the Azure Command-Line Tools for Mac and Linux]: xplat-cli-install.md
[Azure Command-line tools]: xplat-cli-install.md
[work with modules]: nodejs-use-node-modules-azure-apps.md
[build and deploy a Node.js Web Site]: web-sites-nodejs-develop-deploy-mac.md

