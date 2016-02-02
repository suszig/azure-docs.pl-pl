<properties 
    pageTitle="Azure App Service Web Apps で io.js を使用する方法" 
    description="io.js を使用して Azure App Service で Web アプリを使用する方法について説明します。" 
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
    ms.author="robmcm" />


# Azure App Service Web Apps で io.js を使用する方法

人気のある Node フォーク [io.js] 機能 joyent の Node.js よりオープンなガバナンス モデルより迅速なリリース サイクル、新しく実験的な JavaScript 機能のより迅速な導入などのさまざまな違いがあります。

中に [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) のユーザー指定の Node.js バイナリもにより Web Apps は、多くの Node.js バージョンがプレインストールされている、します。 この記事では、App Service Web Apps で io.js を使用できるようにする 2 つの方法を紹介します。1 つは、拡張デプロイメント スクリプトを使用して、利用可能な最新の io.js バージョンを使用するよう Azure を自動的に構成する方法です。もう 1 つは、io.js バイナリを手動でアップロードする方法です。

<a id="deploymentscript"></a>
## デプロイメント スクリプトの使用

Node.js アプリケーションをデプロイすると、App Service Web Apps は小さなコマンドをたくさん実行して、環境が適切に構成されるようにします。 デプロイメント スクリプトを使うと、このプロセスをカスタマイズして、io.js のダウンロードと構成を含めることができます。

[Io.js デプロイメント スクリプト](https://github.com/felixrieseberg/iojs-azure) は GitHub で入手できます。 Web アプリで io.js を有効にするには、**.deployment**、**deploy.cmd**、および **IISNode.yml** をアプリケーション フォルダーのルートにコピーして、Web Apps にデプロイするだけです。

1 つ目のファイルの **.deployment** は、デプロイ時に **deploy.cmd** を実行するよう Web Apps に指示します。 このスクリプトは Node.js アプリケーションの通常の手順をすべて実行しますが、io.js の最新バージョンもダウンロードします。 最後に、**IISNode.yml** が、プレインストールされている Node.js バイナリの代わりに、ダウンロードしたばかりの io.js バイナリを使用するよう Web Apps を構成します。
> [AZURE.NOTE] 使用中の io.js バイナリを更新するには、アプリケーションを再デプロイするだけです。アプリケーションをデプロイするたびに、スクリプトによって io.js の新しいバージョンがダウンロードされます。

<a id="manualinstallation"></a>
## 手動インストールの使用

カスタム io.js バージョンを手動でインストールする手順は 2 つだけです。 最初に、ダウンロード、 **win-x64** [io.js の配布] から直接バイナリ。 必要なのは、**iojs.exe** と **iojs.lib** の 2 つのファイルです。 両方のファイルを Web アプリ内のフォルダー (例: **bin/iojs**) に保存します。

プレインストールされている Node バージョンの代わりに **iojs.exe** を使用するよう Web Apps を構成する場合は、アプリケーションのルートに **IISNode.yml** ファイルを作成して、以下の行を追加します。

    nodeProcessCommandLine: "D:\home\site\wwwroot\bin\iojs\iojs.exe"

<a id="nextsteps"></a>
## 次のステップ

この記事では、App Service Web Apps で io.js を使用する方法として、提供されているデプロイメント スクリプトと手動インストールの 2 つについて説明しました。
> [AZURE.NOTE] io.js は開発途上のもので、Node.js に比べて、より頻繁に更新されます。 多くの Node.js モジュールが io.js で動作しない - してくださいのトラブルシューティング [GitHub の io.js] を参照してください。

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。


[io.js]: https://iojs.org 
[io.js distribution]: https://iojs.org/dist/ 
[io.js on github]: https://github.com/iojs/io.js 
[io.js deployment script]: https://github.com/felixrieseberg/iojs-azure 

