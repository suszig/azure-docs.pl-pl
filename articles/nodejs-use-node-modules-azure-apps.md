<properties pageTitle="Node.js モジュールの操作" description="Azure Websites や Cloud Services を使用する際の Node.js モジュールの使用方法について説明します。" services="" documentationCenter="nodejs" authors="rmcmurray" manager="wpickett" editor=""/>

<tags ms.service="multiple" ms.workload="na" ms.tgt_pltfrm="na" ms.devlang="nodejs" ms.topic="article" ms.date="11/20/2015" ms.author="robmcm"/>






# Azure アプリケーションでの Node.js モジュールの使用

このドキュメントは、Azure でホストされているアプリケーションでの Node.js モジュールの使用に関するガイダンスを示します。 また、Azure でアプリケーションによって特定のバージョンのモジュールとネイティブ モジュールが確実に使用されるようにするためのガイダンスも紹介します。

Node.js モジュールである **package.json** および **npm-shrinkwrap.json** ファイルの使い方について既によくわかっている方は、この記事で概要を簡単にご確認ください。

* Azure の Web サイトでは **package.json** および **npm-shrinkwrap.json** ファイルが認識され、これらのファイルのエントリに基づいてモジュールをインストールできます。
* Azure のクラウド サービスには、開発環境にインストールされているすべてのモジュールが予想されると、 **node \_modules** ディレクトリ、展開パッケージの一部として含められる。

> [AZURE.NOTE] VM でのデプロイは Virtual Machines でホストされているオペレーティング システムに依存するので、この記事では Azure Virtual Machines については説明しません。

> [AZURE.NOTE] **package.json** または **npm-shrinkwrap.json** ファイルを使用したモジュール インストールのサポートを有効にできますが、このサポートを有効にするには、クラウド サービスのプロジェクトで使用される既定のスクリプトをカスタマイズする必要があります。 これを行う方法の例は、を参照してください [ノード モジュールのデプロイを回避する npm インストールを実行する Azure スタートアップ タスク](http://nodeblog.azurewebsites.net/startup-task-to-run-npm-in-azure)。

## Node.js モジュール

モジュールは、アプリケーションに対して特定の機能を提供する読み込み可能な JavaScript パッケージです。 通常は **npm** コマンド ライン ツールを使用してインストールされますが、http モジュールのようにコア Node.js パッケージの一部として提供されるものもあります。

格納されますモジュールをインストールすると、 **node \_modules** ディレクトリ、アプリケーション ディレクトリ構造のルートにします。 各モジュール、 **node \_modules** かつ独自のディレクトリが保持 **node \_modules** に依存し、依存関係チェーンの一番下のすべてのモジュールに対して繰り返さするすべてのモジュールを格納するディレクトリ。 これにより、インストールされている各モジュールが、自身が依存するモジュールに対して独自のバージョン要件を持つことができます。ただし、この結果、ディレクトリ構造が非常に大きくなる可能性があります。

展開するときに、 **node \_modules** ディレクトリ、アプリケーションの一部としてと比較を使用すると、展開のサイズを増やすには、 **package.json** または **npm shrinkwrap** ファイル。 ただし、それは保証実稼働環境で使用されているモジュールのバージョンは開発で使用されるものと一致しています。

### ネイティブ モジュール

ほとんどのモジュールがシンプルな JavaScript テキスト ファイルですが、中にはプラットフォーム固有のバイナリ イメージのモジュールもあります。 こうしたモジュールは、通常、インストール時に Python および node-gyp を使用してコンパイルされます。 Azure クラウド サービスが依存するので、 **node \_modules** がインストールされ、Windows 開発システムでコンパイルされる限り、インストールされているモジュールの一部として含まれるアプリケーション、ネイティブのモジュールの一部としてデプロイされているフォルダーが、クラウド サービスで動作する必要があります。

Azure Websites では、すべてのネイティブ モジュールはサポートされません。また極めて特別な前提条件があるコンパイルに失敗する可能性があります。 MongoDB のような人気のあるモジュールのいくつかは、ネイティブの依存関係があります(任意)。なくても問題なく作動しますが、ほぼすべてのネイティブ モジュールで 2 つの回避策が現在利用でき、成功が実証されています。

* ネイティブ モジュールの前提条件がすべてインストールされている Windows コンピューターで **npm install** を実行します。 次に、作成された展開 **node \_modules** フォルダーを Azure Websites にアプリケーションの一部として。
* Azure Websites はデプロイメント時に、カスタムの bash やシェル スクリプトを実行するように構成でき、カスタム コマンドを実行し、**npm install** が実行される方法を正確に構成できる機会を与えます。 これを行う方法を示すビデオについては、[Kudu でカスタム web サイトの展開スクリプト] を参照してください。

### package.json ファイルの使用

**Package.json** ファイルは、ホスティング プラットフォームを含める必要はありませんのではなく、依存関係をインストールできるようにアプリケーションに必要な最上位レベルの依存関係を指定する方法、 **node \_packages** フォルダーの展開の一部として。 アプリケーションのデプロイ後、**npm install** コマンドによって **package.json** ファイルが解析され、表示されているすべての依存関係がインストールされます。

開発中、モジュールをインストールするときに **--save**、**--save-dev**、または **--save-optional** パラメーターを使用すると、モジュールのエントリを **package.json** ファイルに自動的に追加することができます。 詳細については、次を参照してください。 [npm インストール](https://npmjs.org/doc/install.html)します。

**package.json** ファイルの潜在的な問題の 1 つが、このファイルでは最上位レベルの依存関係のバージョンしか指定されないという点です。 インストールされているモジュールはそれぞれ、自身が依存しているモジュールのバージョンを指定することがあれば、指定しないこともあります。したがって、依存関係チェーンが、開発時に使用されたもの以外になる可能性があります。
> [AZURE.NOTE]
> 場合に、Azure の Web に配置すると、 <b>package.json</b> ファイルは、Git を使用してアプリケーションを発行するときに、次のようなエラーが表示されますがネイティブ モジュールを参照します。

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'
>    
>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1
>


### npm-shrinkwrap.json ファイルの使用

**npm-shrinkwrap.json** ファイルは、**package.json** ファイルのモジュール バージョン管理制限への対処を試みるものです。 **package.json** ファイルに含まれるのは最上位レベルのモジュールのバージョンのみですが、**npm-shrinkwrap.json** ファイルには、完全なモジュール依存関係チェーンのバージョン要件が含まれます。

アプリケーションの運用の準備ができたら、バージョン要件を整理して確定し、**npm shrinkwrap** コマンドを使用して **npm-shrinkwrap.json** ファイルを作成できます。 現在インストールされているバージョンを使用して、 **node \_modules** フォルダー、およびこれらを記録、 **npm shrinkwrap** ファイルです。 ホスティング環境へのアプリケーションのデプロイ後、**npm install** コマンドによって **npm-shrinkwrap.json** ファイルが解析され、表示されているすべての依存関係がインストールされます。 詳細については、次を参照してください。 [npm インストール](https://npmjs.org/doc/install.html)します。
> [AZURE.NOTE]
>場合に、Azure の Web に配置すると、 <b>npm shrinkwrap</b> ファイルは、Git を使用してアプリケーションを発行するときに、次のようなエラーが表示されますがネイティブ モジュールを参照します。

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'
>    
>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1
>


## 次のステップ

Azure で Node.js モジュールを使用する方法を理解する方法について学習 [Node.js のバージョンを指定する] に [ビルドし、配置を Node.js Web サイト] および [Mac および Linux 用 Azure コマンド ライン インターフェイスを使用する方法]。

詳細については、次を参照してください。、 [Node.js デベロッパー センター](/develop/nodejs/)します。


[specify the node.js version]: nodejs-specify-node-version-azure-apps.md 
[how to use the azure command-line interface for mac and linux]: xplat-cli-install.md 
[build and deploy a node.js web site]: web-sites-nodejs-develop-deploy-mac.md 
[node.js web application with storage on mongodb (mongolab)]: store-mongolab-web-sites-nodejs-store-data-mongodb.md 
[publishing with git]: web-sites-publish-source-control.md 
[build and deploy a node.js application to an azure cloud service]: cloud-services-nodejs-develop-deploy-app.md 
[custom website deployment scripts with kudu]: /documentation/videos/custom-web-site-deployment-scripts-with-kudu/ 

