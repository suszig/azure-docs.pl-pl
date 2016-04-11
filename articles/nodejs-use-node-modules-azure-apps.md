<properties pageTitle="Node.js モジュールの操作" description="Azure Websites や Cloud Services を使用する際の Node.js モジュールの使用方法について説明します。" services="" documentationCenter="nodejs" authors="rmcmurray" manager="wpickett" editor=""/>

<tags ms.service="multiple" ms.workload="na" ms.tgt_pltfrm="na" ms.devlang="nodejs" ms.topic="article" ms.date="11/20/2015" ms.author="robmcm"/>





# Azure アプリケーションでの Node.js モジュールの使用

このドキュメントは、Azure でホストされているアプリケーションでの Node.js モジュールの使用に関するガイダンスを示します。 また、Azure でアプリケーションによって特定のバージョンのモジュールとネイティブ モジュールが確実に使用されるようにするためのガイダンスも紹介します。

Node.js モジュールの使用について熟知している場合 **package.json** と **npm shrinkwrap** 概要の説明とはこの記事では、ファイルを次に示します。

* Azure の web サイトで **package.json** と **npm shrinkwrap** ファイルし、これらのファイルのエントリに基づいてモジュールをインストールできます。
* Azure のクラウド サービスには、開発環境にインストールされているすべてのモジュールが予想されると、 **node \_modules** ディレクトリ、展開パッケージの一部として含められる。

> [AZURE.NOTE] Azure の仮想マシンは、バーチャル マシンの配置操作が仮想マシンでホストされているオペレーティング システムに依存するので、この記事では説明しません。

> [AZURE.NOTE] 使用してモジュールのインストールのサポートを有効にすることは **package.json** または **npm shrinkwrap** ファイルを Azure のクラウド サービス プロジェクトで使用される既定のスクリプトをカスタマイズが必要です。 これを行う方法の例は、次を参照してください [ノード モジュールのデプロイを回避する npm インストールを実行する Azure スタートアップ タスク。](http://nodeblog.azurewebsites.net/startup-task-to-run-npm-in-azure)

##Node.js モジュール

モジュールは、アプリケーションに対して特定の機能を提供する読み込み可能な JavaScript パッケージです。 モジュールは、通常インストールを使用して、 **npm** よう、http モジュールのコマンド ライン ツールは、コア Node.js パッケージの一部として提供されます。

格納されますモジュールをインストールすると、 **node \_modules** ディレクトリ、アプリケーション ディレクトリ構造のルートにします。 各モジュール、 **node \_modules** かつ独自のディレクトリが保持 **node \_modules** に依存し、依存関係チェーンの一番下のすべてのモジュールに対して繰り返さするすべてのモジュールを格納するディレクトリ。 これにより、インストールされている各モジュールが、自身が依存するモジュールに対して独自のバージョン要件を持つことができます。ただし、この結果、ディレクトリ構造が非常に大きくなる可能性があります。

展開するときに、 **node \_modules** ディレクトリ、アプリケーションの一部としてと比較を使用すると、展開のサイズを増やすには、 **package.json** または **npm shrinkwrap** ファイル。 ただし、それは保証実稼働環境で使用されているモジュールのバージョンは開発で使用されるものと一致しています。

###ネイティブ モジュール

ほとんどのモジュールがシンプルな JavaScript テキスト ファイルですが、中にはプラットフォーム固有のバイナリ イメージのモジュールもあります。 こうしたモジュールは、通常、インストール時に Python および node-gyp を使用してコンパイルされます。 Azure クラウド サービスが依存するので、 **node \_modules** がインストールされ、Windows 開発システムでコンパイルされる限り、インストールされているモジュールの一部として含まれるアプリケーション、ネイティブのモジュールの一部としてデプロイされているフォルダーが、クラウド サービスで動作する必要があります。

Azure Websites では、すべてのネイティブ モジュールはサポートされません。また極めて特別な前提条件があるコンパイルに失敗する可能性があります。 MongoDB のような人気のあるモジュールのいくつかは、ネイティブの依存関係があります(任意)。なくても問題なく作動しますが、ほぼすべてのネイティブ モジュールで 2 つの回避策が現在利用でき、成功が実証されています。

* 実行 **npm インストール** を持つすべてのネイティブ モジュールの前提条件がインストールされている Windows コンピューター。 次に、作成された展開 **node \_modules** フォルダーを Azure Websites にアプリケーションの一部として。
* Azure の web サイトを構成できます展開時に、カスタムの bash やシェル スクリプトを実行するカスタム コマンドを実行し、方法を正確に構成する機会を与える **npm インストール** が実行されています。 これを行う方法を示すビデオについては、次を参照してください。 [Custom Website Deployment Scripts with Kudu]します。

###package.json ファイルの使用

 **Package.json** ファイルは、ホスティング プラットフォームを含める必要はありませんのではなく、依存関係をインストールできるようにアプリケーションに必要な最上位レベルの依存関係を指定する方法、 **node \_packages** フォルダーの展開の一部として。 アプリケーションを展開すると後、 **npm インストール** コマンドが解析に使用される、 **package.json** ファイルし、されているすべての依存関係をインストールします。

開発時に使用することができます、 **--保存**, 、**--save-dev**, 、または **--save-optional** パラメーターには、モジュールのエントリを追加するモジュールをインストールするときに、 **package.json** ファイルに自動的にします。 詳細については、次を参照してください。 [npm インストール](https://npmjs.org/doc/install.html)します。

潜在的な問題の 1 つ、 **package.json** ファイルは、最上位レベルの依存関係のバージョンのみを指定しています。 インストールされているモジュールはそれぞれ、自身が依存しているモジュールのバージョンを指定することがあれば、指定しないこともあります。したがって、依存関係チェーンが、開発時に使用されたもの以外になる可能性があります。

> [AZURE.NOTE]
> Azure Websites へのデプロイ中、<b>package.json</b> ファイルがネイティブ モジュールを参照すると、Git を使用してアプリケーションを発行するときに次のようなエラーが表示されます。

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


###npm-shrinkwrap.json ファイルの使用

 **Npm shrinkwrap** ファイルのモジュール バージョン管理制限に対処するは、 **package.json** ファイルです。 中に、 **package.json** ファイルには、上位レベルのモジュールのバージョンにはのみが含まれています、 **npm shrinkwrap** ファイルには、完全なモジュール依存関係チェーンのバージョン要件が含まれています。

アプリケーションの運用環境の準備ができたら場合は、ロックのバージョン要件を作成、 **npm shrinkwrap** ファイルを使用して、 **npm shrinkwrap** コマンドです。 現在インストールされているバージョンを使用して、 **node \_modules** フォルダー、およびこれらを記録、 **npm shrinkwrap** ファイルです。 ホスティング環境にアプリケーションが配置された後、 **npm インストール** コマンドが解析に使用される、 **npm shrinkwrap** ファイルし、されているすべての依存関係をインストールします。 詳細については、次を参照してください。 [npm インストール](https://npmjs.org/doc/install.html)します。

> [AZURE.NOTE]
>Azure Websites へのデプロイ中、<b>npm-shrinkwrap.json</b> ファイルがネイティブ モジュールを参照すると、Git を使用してアプリケーションを発行するときに次のようなエラーが表示されます。

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


##次のステップ

ここで Azure で Node.js モジュールを使用する方法を理解してについて学習する方法 [specify the Node.js version], 、[build and deploy a Node.js Web Site], 、および [How to use the Azure Command-Line Interface for Mac and Linux]します。

詳細については、次を参照してください。、 [Node.js デベロッパー センター](/develop/nodejs/)します。

[specify the Node.js version]: nodejs-specify-node-version-azure-apps.md
[How to use the Azure Command-Line Interface for Mac and Linux]: xplat-cli-install.md
[build and deploy a Node.js Web Site]: web-sites-nodejs-develop-deploy-mac.md
[Node.js Web Application with Storage on MongoDB (MongoLab)]: store-mongolab-web-sites-nodejs-store-data-mongodb.md
[Publishing with Git]: web-sites-publish-source-control.md
[Build and deploy a Node.js application to an Azure Cloud Service]: cloud-services-nodejs-develop-deploy-app.md
[Custom Website Deployment Scripts with Kudu]: /documentation/videos/custom-web-site-deployment-scripts-with-kudu/

