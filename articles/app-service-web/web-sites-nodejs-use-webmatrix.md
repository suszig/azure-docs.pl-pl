<properties 
    pageTitle="WebMatrix を使用した Node.js Web アプリの構築と Azure へのデプロイ" 
    description="WebMatrix を使用して Node.js アプリケーションを作成し、Azure App Service Web Apps にデプロイする方法を示すチュートリアル。" 
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
    ms.topic="article" 
    ms.date="11/20/2015"
    ms.author="robmcm"/>


# WebMatrix を使用した Node.js Web アプリの構築と Azure へのデプロイ

このチュートリアルは、WebMatrix を使用して Node.js アプリケーションの開発し、デプロイする方法を示します [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web アプリです。 WebMatrix は、Microsoft から提供されている無料の Web 開発ツールで、Web サイトまたは Web アプリの開発に必要なものがすべて用意されています。 WebMatrix には、コードの入力補完機能、作成済みのテンプレート、Jade、LESS、CoffeeScript のエディターのサポートなど、Node.js を簡単に使用できるようにする機能が用意されています。 詳細について [WebMatrix](https://www.microsoft.com/web/webmatrix/)します。

このチュートリアルを完了すると、Azure App Service で動作する Node.js Web アプリが完成します。
 
完成したアプリケーションのスクリーンショットは次のようになります。

![Azure node Web サイト][webmatrix-node-completed]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## Azure へのサインイン

Azure App Service で Web アプリを作成するには、次のステップに従います。

1. WebMatrix の起動
2. 初めて WebMatrix を使用する場合は、Azure へのサインインを求めるメッセージが表示されます。  それ以外の場合、クリックすると、 **サイン イン** ボタンをクリックし、選択 **アカウントの追加**します。  選択 **サインイン** Microsoft アカウントを使用します。

    ![アカウントの追加][addaccount]

3. Azure アカウントにサインアップしている場合は、Microsoft アカウントを使用してログインできます。

    ![Azure へのサインイン][signin]  


## Azure の組み込みテンプレートを使用したサイトの作成

1. スタート画面で、をクリックして、 **新規** ボタンをクリックし、選択 **テンプレート ギャラリー** テンプレート ギャラリーから新しいサイトを作成します。

    ![テンプレート ギャラリーからの新しいサイト][sitefromtemplate]

2.  **Site from Template** ダイアログで、 **ノード** し、[ **Express サイト**します。 最後に、クリックして **次**します。 前提条件がない場合は、 **Express サイト** テンプレートにインストールするように求められます。

    ![Express テンプレートの選択][webmatrix-templates]

3. Azure にサインインする場合は、ここでローカル サイト用の App Service Web アプリを作成することができます。  一意の名前を選択し、App Service Web アプリの作成先のデータ センターを選択します。 

    ![Azure でのサイトの作成][nodesitefromtemplateazure]
    
4. WebMatrix による ローカル サイトおよび App Service Web アプリの構築が終了すると、WebMatrix IDE が表示されます。

    ![Web Matrix IDE][webmatrix-ide]

##Azure へのアプリケーションの発行

1. WebMatrix で、次のようにクリックします。 **発行** から、 **ホーム** 表示するリボン、 **発行のプレビュー** 、サイトのダイアログ ボックス。

    ![発行のプレビュー][webmatrix-node-publishpreview]

2. クリックして **続行**します。 発行が完了すると、App Service Web アプリの URL が WebMatrix IDE の下部に表示されます。

    ![発行の完了][webmatrix-publish-complete]

3. リンクをクリックして、ブラウザーで App Service Web アプリを開きます。

    ![Express Web アプリ][webmatrix-node-express-site]

##アプリケーションの変更と再発行

アプリケーションは簡単に変更して再発行することができます。 ここでは、内の見出しに単純な変更を加えるは、 **index.jade** ファイルを開き、アプリケーションを再発行します。

1. WebMatrix で、次のように選択します。 **ファイル**, 、の順に展開し、 **ビュー** フォルダーです。 開いている、 **index.jade** ファイルをダブルクリックしています。

    ![WebMatrix での index.jade の表示][webmatrix-modify-index]

2. 段落の行を次のように変更します。

        p Welcome to #{title} with WebMatrix on Azure!

3. 変更内容を保存し、発行アイコンをクリックします。 最後に、クリックして **続行** で、 **発行のプレビュー** ダイアログし、更新が発行されるまで待ちます。

    ![発行のプレビュー][webmatrix-republish]

4. 発行が完了したら、発行プロセスの完了時に返されたリンクを使用して、更新されたApp Service Web アプリを表示します。

    ![Azure node Web アプリ][webmatrix-node-completed]

##次のステップ

アプリケーションで使用するバージョンを指定する方法と Azure に用意されている Node.js のバージョンに関する詳細については、「 [Azure アプリケーションで Node.js バージョンを指定する](../nodejs-specify-node-version-azure-apps.md)です。

Azure に展開された後に、アプリケーションで問題が発生した場合は、次を参照してください。 [Azure App Service での Node.js web アプリをデバッグする方法について](web-sites-nodejs-debug.md) についてを参照してください。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

[WebMatrix WebSite]: http://www.microsoft.com/click/services/Redirect2.ashx?CR_CC=200106398
[WebMatrix for Azure]: http://go.microsoft.com/fwlink/?LinkID=253622&clcid=0x409

[webmatrix-node-completed]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-complete.png
[webmatrix-templates]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-templates.png

[webmatrix-node-publishpreview]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publishpreview.png

[webmatrix-ide]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-ide.png
[webmatrix-publish-complete]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-publish-complete.png
[webmatrix-node-express-site]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-express-webiste.png
[webmatrix-modify-index]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-edit.png
[webmatrix-republish]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-republish.png
[addaccount]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-add-account.png
[signin]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-sign-in.png
[sitefromtemplate]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-site-from-template.png
[nodesitefromtemplateazure]: ./media/web-sites-nodejs-use-webmatrix/webmatrix-node-site-azure.png
 
