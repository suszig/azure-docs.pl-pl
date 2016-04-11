<properties 
    pageTitle="Express を使用した Web アプリケーション (Node.js) | Microsoft Azure" 
    description="クラウド サービスのチュートリアルを基に、Express モジュールの使用方法を示すチュートリアル。" 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/20/2015" 
    ms.author="robmcm"/>






# Azure Cloud Services での Express を使用した Node.js Web アプリケーションの構築

node.js には、コア ランタイムの最小限の機能セットが含まれます。
多くの場合、開発者は Node.js アプリケーションを開発するときに、サード パーティ モジュールを使用して追加機能を指定します
。 このチュートリアルの内容
使用して新しいアプリケーションを作成するが、 [Express][] モジュールで、Node.js web アプリケーションを作成するための MVC フレームワークを提供します。

完成したアプリケーションのスクリーンショットは次のようになります。

!["Welcome to Express in Azure" と表示している Web ブラウザー](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

##クラウド サービス プロジェクトの作成

"expressapp" という名前の新しいクラウド サービス プロジェクトを作成するには、次の手順を実行します。

1.  **[スタート] メニュー** または **のスタート画面で**, 、検索 **Azure PowerShell**します。 最後を右クリックし **Azure PowerShell** 選択 **管理者として実行**します。

    ![Azure PowerShell アイコン](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)

    [AZURE.INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

2. ディレクトリに移動、 **c:\\node** ディレクトリという名前の新しいソリューションを作成するには、次のコマンドを入力および **expressapp** という名前の web ロールと **WebRole1**:

        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21

    > [AZURE.NOTE] 既定では、 **Add-azurenodewebrole** 古いバージョンの Node.js を使用します。  **Set-azureserviceprojectrole** 上記のステートメントが Node の v0.10.21 を使用する Azure に指示します。  パラメーターには大文字と小文字の区別があることに注意してください。  チェックによって、正しいバージョンの Node.js が選択されたことを確認できる、 **エンジン** プロパティ **WebRole1\package.json**します。

##Express のインストール

1. 次のコマンドを発行して Express ジェネレーターをインストールします。

        PS C:\node\expressapp> npm install express-generator -g

    この npm コマンドにより次のような結果が出力されます。 

    ![Windows PowerShell での npm install express コマンドの出力の表示](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)

2. ディレクトリに移動、 **WebRole1** ディレクトリと、express コマンドを使用して新しいアプリケーションを生成します。

        PS C:\node\expressapp\WebRole1> express

    前のアプリケーションを上書きするかどうかを確認するメッセージが表示されます。 入力 **y** または **はい** を続行します。 Express によって、app.js ファイルと、アプリケーションを構築するためのフォルダー構造が生成されます。

    ![express コマンドの出力](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)


5.  package.json ファイルに定義された追加の依存関係をインストールするには、
    次のコマンドを入力します。

        PS C:\node\expressapp\WebRole1> npm install

    ![npm install コマンドの出力](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)

6.  次のコマンドを使用して、コピー、 **、bin/www** ファイルを **server.js**します。 これは、クラウド サービスがこのアプリケーションのエントリ ポイントを見つけられるようにするためです。

        PS C:\node\expressapp\WebRole1> copy bin/www server.js

    このコマンドが完了したら、 **server.js** 、WebRole1 ディレクトリ内のファイルです。

7.  変更、 **server.js** のいずれかを削除する、'.' 次の行からの文字。

        var app = require('../app');

    この変更を行うと、行は次のようになります。

        var app = require('./app');

    ファイルを移行したため、この変更が (以前 **、bin/www**,、) 必要になるアプリケーション ファイルと同じディレクトリにします。 この変更を行った後は、保存、 **server.js** ファイルです。

8.  次のコマンドを使用して、アプリケーションを Microsoft Azure エミュレーターで実行します。

        PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch

    ![Welcome to Express メッセージを含む Web ページ](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## ビューの変更

次に、"Welcome to Express in Azure" というメッセージが表示されるようにビューを変更します
。

1.  次のコマンドを入力して、index.jade ファイルを開きます。

        PS C:\node\expressapp\WebRole1> notepad views/index.jade

    ![index.jade ファイルの内容](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)

    Jade は Express アプリケーションで使用される既定のビュー エンジンです。 サブスクリプションの
    Jade ビュー エンジンに関する情報を参照してください [http://jade-lang.com][]します。

2.  テキストの最後の行を追加して変更 **azure**します。

    ![Index.jade ファイルの最後の行を読み取ります \#{title へようこそ p} で、Azure。](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)

3.  ファイルを保存して、メモ帳を終了します。

4.  ブラウザーの表示を最新情報に更新すると、変更を確認できます。

    ![ブラウザー ウィンドウで "Welcome to Express in Azure" と表示されたページ](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

使用して、アプリケーションをテストした後、 **Stop-azureemulator** コマンドレットをエミュレーターを停止します。

##Azure にアプリケーションをデプロイする

Azure PowerShell ウィンドウで使用して、 **Publish-AzureServiceProject** コマンドレットは、クラウド サービスにアプリケーションを配置するには

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

デプロイ操作が完了すると、ブラウザーが開き、Web ページが表示されます。

![Express ページを表示している Web ブラウザー。 URL は、ページが Azure でホストされていることを示している。](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## 次のステップ

詳細については、次を参照してください。、 [Node.js デベロッパー センター](/develop/nodejs/)します。

  [Node.js Web Application]: http://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
  [Express]: http://expressjs.com/
  [http://jade-lang.com]: http://jade-lang.com

 

