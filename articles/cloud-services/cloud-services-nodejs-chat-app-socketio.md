<properties 
    pageTitle="Socket.io を使用する Node.js アプリケーション | Microsoft Azure" 
    description="Azure でホストされる Node.js アプリケーションで Socket.IO を使用する方法を説明します。" 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="TomArcher" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="09/01/2015" 
    ms.author="tarcher"/>






# Azure Cloud Services で Socket.IO を使用する Node.js チャット アプリケーションを構築する

Socket.IO は、node.js サーバーとクライアントの間の
リアルタイム通信を提供します。 このチュートリアルでは、
Azure で socket.IO ベースのチャット アプリケーションをホストする手順を説明します。 BLOB の詳細
詳細については、次を参照してください。 <a href="http://socket.io/">http://socket.io/</a>します。

完成したアプリケーションのスクリーンショットは次のようになります。

![Azure でホストされるサービスを表示しているブラウザー ウィンドウ][completed-app]

## 前提条件

この記事の例を正常に完了するには、次の製品とバージョンがインストールされている必要があります。

* インストール [Visual Studio 2013](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx)
* インストール [Node.js](https://nodejs.org/download/)
* インストール [Python バージョン 2.7.10](https://www.python.org/)

## クラウド サービス プロジェクトの作成

次の手順では、Socket.IO アプリケーションをストリーミングするクラウド サービス プロジェクトを作成します。

1. **[スタート] メニュー**または**スタート画面**で、**Azure PowerShell** を検索します。 最後に、**[Azure PowerShell]** を右クリックし、**[管理者として実行]** を選択します。

    ![Azure PowerShell アイコン][powershell-menu]

2. というディレクトリを作成 **c:\\node**します。

        PS C:\> md node

3. ディレクトリに移動、 **c:\\node** ディレクトリ

        PS C:\> cd node

4. 次のコマンドを入力して、**chatapp** という名前の新しいソリューションと **WorkerRole1** という名前の worker ロールを作成します。

        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole

    次の応答が表示されます。

    ![new-azureservice および add-azurenodeworkerrole コマンドレットの出力](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## チャットのサンプルのダウンロード

このプロジェクトでは、チャットのサンプル [Socket.IO を使用してください。
GitHub のリポジトリ] です。 次の手順を実行してサンプルをダウンロードし、
先ほど作成したプロジェクトに追加します。

1.  **[複製]** ボタンを使用して、リポジトリのローカル コピーを作成します。 **[ZIP]** ボタンを使用してプロジェクトをダウンロードすることもできます。

    ![https://github.com/LearnBoost/socket.io/tree/master/examples/chat が表示され、ZIP のダウンロード アイコンが強調表示されているブラウザー ウィンドウ][chat-example-view]

3.  表示されるまで、ローカル リポジトリのディレクトリ構造を移動、 **examples \\chat**
    ディレクトリに移動します。 このディレクトリの内容を、先ほど作成した
    **C:\\node\\chatapp\\WorkerRole1** ディレクトリが以前に作成します。

    ![アーカイブから展開された examples\\chat ディレクトリの内容を表示しているエクスプローラー][chat-contents]

    上記のスクリーン ショットで強調表示されている項目からコピーされたファイル、 **examples \\chat** ディレクトリ

4.  **C:\\node\\chatapp\\WorkerRole1** ディレクトリ、削除、 **server.js** ファイルを開き、名前を変更、 **app.js** ファイルを **server.js**します。 これにより、前に **Add-AzureNodeWorkerRole** コマンドレットで作成された既定の **server.js** ファイルが削除され、チャットのサンプルのアプリケーション ファイルに置き換えられます。

### Server.js の変更とモジュールのインストール

Azure エミュレーターでアプリケーションをテストする前に、
いくつかの点を変更する必要があります。 次の手順で
server.js ファイルを変更します。

1.  Visual Studio または任意のテキスト エディターで **server.js** ファイルを開きます。

2.  server.js の先頭にある **Module dependencies** セクションを探して、次のように **sio = require('..//..//lib//socket.io')** を含む行を **sio = require('socket.io')** に変更します。

        var express = require('express')
        , stylus = require('stylus')
        , nib = require('nib')
        //, sio = require('..//..//lib//socket.io'); //Original
        , sio = require('socket.io');                //Updated

3.  アプリケーションが適切なポートでリッスンするように、
    メモ帳などのエディターで server.js を開き、
    次の行 **3000** と **process.env.port** 次のようにします。

        //app.listen(3000, function () {            //Original
        app.listen(process.env.port, function () {  //Updated
          var addr = app.address();
          console.log('   app listening on http://' + addr.address + ':' + addr.port);
        });


変更を保存した後に **server.js**, 、次の手順を使用して
必要なモジュールをインストールし、
Microsoft Azure エミュレーターで実行します。

1.  使用して **Azure PowerShell**, 、ディレクトリに移動、 **C:\\node\\chatapp\\WorkerRole1** ディレクトリ コマンドを使用して、次に、このアプリケーションで必要なモジュールをインストールします。

        PS C:\node\chatapp\WorkerRole1> npm install

    これによって、package.json ファイルにリストされているモジュールがインストールされます。 実装した後
    完了すると、次のような出力が表示
    されます。

    ![npm install コマンドの出力][the-output-of-the-npm-install-command]

4.  このサンプルはもともと Socket.IO GitHub
    リポジトリの一部であり、相対パスによって Socket.IO ライブラリを
    直接参照していたため、package.json ファイルでは Socket.IO は参照されていませんでした。
    そのため、次のコマンドを発行してインストールする必要があります。

        PS C:\node\chatapp\WorkerRole1> npm install socket.io --save


### テストとデプロイ

1.  次のコマンドを発行してエミュレーターを起動します。

        PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch

2.  ブラウザーを開きに移動 **http://127.0.0.1**します。

3.  ブラウザー ウィンドウが開いたら、ニックネームを入力して Enter キーを押します。
    これにより、特定のニックネームでメッセージが投稿されます。 マルチユーザー機能をテストするには、
    同じ URL を使用して新しいブラウザー ウィンドウを開き、
    別のニックネームを入力します。

    ![User1 と User2 からのチャット メッセージを表示している 2 つのブラウザー ウィンドウ](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)

3.  アプリケーションのテストが終了したら、
    次のコマンドを発行してエミュレーターを停止します。

        PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator

4.  4.Azure にアプリケーションをデプロイするには、
    **Publish-AzureServiceProject** コマンドレットです。 次に例を示します。

        PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch

    > [AZURE.IMPORTANT] 必ず一意の名前を使用してください。一意でない場合は発行処理が失敗します。 デプロイが完了すると、ブラウザーが開き、デプロイされたサービスに移動します。
    > 
    > 指定したサブスクリプション名がインポートされた発行プロファイルに存在しないというエラーが出力された場合は、Azure にデプロイする前に、サブスクリプションの発行プロファイルをダウンロードしてインストールする必要があります。 参照してください、 **を Azure にアプリケーションを配置する** の [の構築し、Azure クラウド サービスへの Node.js アプリケーションをデプロイ](https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/)

    ![Azure でホストされるサービスを表示しているブラウザー ウィンドウ][completed-app]
    > [AZURE.NOTE] 指定したサブスクリプション名がインポートされた発行プロファイルに存在しないというエラーが出力された場合は、Azure にデプロイする前に、サブスクリプションの発行プロファイルをダウンロードしてインストールする必要があります。 参照してください、 **を Azure にアプリケーションを配置する** の [の構築し、Azure クラウド サービスへの Node.js アプリケーションをデプロイ](https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/)

アプリケーションが Azure で実行されており、
Socket.IO を使用して異なるクライアント間でチャット メッセージをリレーできます。
> [AZURE.NOTE] わかりやすくするために、このサンプルは同じインスタンスに接続したユーザー間でのチャットに制限されています。 つまり、クラウド サービスによって 2 つの worker ロール インスタンスが作成された場合、ユーザーは同じ worker ロール インスタンスに接続された他のユーザーとのみチャットすることができます。 複数のロール インスタンスで機能するようにこのアプリケーションを拡張するには、Service Bus などのテクノロジを使用して、インスタンス間で Socket.IO ストアの状態を共有します。 例については、[Service Bus キューおよびトピックの使用方法の例を参照してください、 [Azure SDK for Node.js GitHub リポジトリ](https://github.com/WindowsAzure/azure-sdk-for-node)します。

## 次のステップ

このチュートリアルでは、Azure Cloud Services でホストされる基本的なチャット アプリケーションを作成する方法を説明しました。 Azure の web サイトでこのアプリケーションをホストする方法については、次を参照してください。 [Socket.IO 使用する Node.js チャット アプリケーションの Azure Web サイトを構築 ][chatwebsite]します。

詳細については、「関連項目、 [Node.js デベロッパー センター](/develop/nodejs/)します。


[chatwebsite]: /develop/nodejs/tutorials/website-using-socketio/ 
[azure sla]: http://www.windowsazure.com/support/sla/ 
[azure sdk for node.js github repository]: https://github.com/WindowsAzure/azure-sdk-for-node 
[completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png 
[azure sdk for node.js]: https://www.windowsazure.com/develop/nodejs/ 
[node.js web application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/ 
[socket.io github repository]: https://github.com/LearnBoost/socket.io/tree/0.9.14 
[azure considerations]: #windowsazureconsiderations 
[hosting the chat example in a worker role]: #hostingthechatexampleinawebrole 
[summary and next steps]: #summary 
[powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png 
[chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat 
[chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png 
[chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png 
[the-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png 
[the output of the publish-azureservice command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png 

