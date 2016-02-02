<properties 
    pageTitle="Azure での Bottle を使用した Python Web アプリ" 
    description="Azure App Service Web Apps での Python Web アプリの実行について説明したチュートリアルです。" 
    services="app-service\web" 
    documentationCenter="python" 
    tags="python"
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="11/17/2015"
    ms.author="huvalo"/>



# Azure での Bottle を使用した Web アプリの作成

このチュートリアルでは、Azure App Service Web Apps で Python を実行するための基本的な方法について説明します。 Web Apps では、制限付きの無料のホスティングや迅速なデプロイを実行できます。また、Python も使用できます。 アプリケーションの拡張に合わせて、有料のホスティングに切り替えることができます。また、他のすべての Azure サービスと統合することもできます。

Bottle web フレームワークを使用して web アプリを作成します (このチュートリアルの代替バージョンを参照してください [Django](web-sites-python-create-deploy-django-app.md) と [Flask](web-sites-python-create-deploy-flask-app.md))します。 Azure Marketplaceから Web アプリを作成し、Git デプロイを設定して、リポジトリをローカルで複製します。 Web アプリをローカルで実行、変更を加える、コミットおよびプッシュするには、 [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)します。 チュートリアルでは、Windows または Mac/Linux での手順を紹介します。

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 前提条件

- Windows、Mac、または Linux
- Python 2.7 または 3.4
- setuptools、pip、virtualenv (Python 2.7 のみ)
- Git
- [Visual Studio の Python ツール 2.2][] (PTVS) に注意してくださいこれは省略可能。

**注**: 現在、TFS 発行は Python プロジェクトではサポートされていません。

### Windows

[Azure SDK for Python 2.7] のインストールをお勧め Python 2.7 または 3.4 (32 ビット) をインストールされていない場合または [Azure SDK for Python 3.4] は、Web Platform Installer を使用します。 いずれかをインストールすると、Python の 32 ビット バージョン、setuptools、pip、virtualenv などがインストールされます (32 ビット Python は Azure ホスト コンピューターにインストールするものです)。 または、[python.org] から Python を入手できます。

[Git のウィンドウ] をお勧めの Git、または [Windows 用の GitHub] です。 Visual Studio を使用する場合は、Git の統合のサポートを使用できます。

また、[Visual Studio の Python ツールの 2.2] のインストールをお勧めします。 これは省略可能ですが [Visual Studio] の場合、Web 後でこの無料の Visual Studio Community 2013 または Visual Studio Express 2013 を含むが示されます優れた Python IDE です。

### Mac/Linux

Python と Git は既にインストールされていると思いますが、Python 2.7 または 3.4 であることをご確認ください。


## Azure ポータルでの Web アプリの作成

アプリを作成する最初の手順がで web アプリを作成するには、 [Azure ポータル](https://portal.azure.com)します。

1. Azure ポータルの画面左下にある **[新規]** ボタンをクリックします。
2. **[Web + モバイル]** をクリックします。
3. 検索ボックスに、「python」と入力します。
4. 検索結果で **[Bottle]** を選択し、**[作成]** をクリックします。
5. 新しい App Service プランやそのリソース グループ名の作成など、新しい Bottle アプリを構成します。 **[作成]** をクリックします。
6. 新しく作成した web アプリの Git 発行を構成する」の手順に従って、 [Azure App Service での GIT による継続的なデプロイ](web-sites-publish-source-control.md)します。

## アプリケーションの概要

### Git リポジトリのコンテンツ

ここでは、初期の Git リポジトリにあるファイルの概要を説明します。次のセクションではこれらのファイルを複製します。

    \routes.py
    \static\content\
    \static\fonts\
    \static\scripts\
    \views\about.tpl
    \views\contact.tpl
    \views\index.tpl
    \views\layout.tpl

アプリケーションのメイン ソース。 マスター レイアウトを使用した 3 つのページ (index、about、contact) で構成されます。 静的コンテンツとスクリプト (bootstrap、jquery、modernizr、respond など)。

    \app.py

ローカル開発サーバーのサポート。 これを使用すると、アプリケーションをローカルで実行できます。

    \BottleWebProject.pyproj
    \BottleWebProject.sln

[Python Tools for Visual Studio] で使用するプロジェクト ファイルです。

    \ptvs_virtualenv_proxy.py

仮想環境と PTVS リモート デバッグのサポートのための IIS プロキシ。

    \requirements.txt

このアプリケーションで必要な外部パッケージ。 デプロイ スクリプトにより、このファイルに記載してあるパッケージが pip インストールされます。

    \web.2.7.config
    \web.3.4.config

IIS 構成ファイル。 デプロイ スクリプトは適切な web.x.y.config を使用して web.config としてコピーします。

### オプションのファイル - デプロイのカスタマイズ

[AZURE.INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### オプションのファイル - Python ランタイム

[AZURE.INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### サーバー上の追加のファイル

サーバー上にはいくつかのファイルが存在しますが、Git リポジトリには追加されません。 これらのファイルはデプロイ スクリプトによって作成されるものです。

    \web.config

IIS 構成ファイル。 デプロイごとに web.x.y.config から作成されます。

    \env\

Python 仮想環境。 互換性のある仮想環境がまだ Web アプリ上にない場合に、デプロイ時に作成されます。 requirements.txt に示されているパッケージは pip インストールされますが、パッケージを既にインストールしている場合は pip でインストールがスキップされます。

次の 3 つのセクションでは、以下の 3 つの異なる環境における Web アプリ開発を使用した処理方法を説明します。

- Windows、Python Tools for Visual Studio を使用
- Windows、コマンド ラインを使用
- Mac/Linux、コマンド ラインを使用


## Web アプリの開発 - Windows - Python Tools for Visual Studio

### リポジトリの複製

最初に、Azure ポータルで提供される URL を使用して、リポジトリを複製します。 詳細については、次を参照してください。 [Azure App Service での GIT による継続的なデプロイ](web-sites-publish-source-control.md)します。

リポジトリのルートにあるソリューション ファイル (.sln) を開きます。

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-solution-bottle.png)

### 仮想環境の作成

次にローカル開発用の仮想環境を作成します。 **[Python 環境]** を右クリックし、**[仮想環境の追加]** を選択します。

- 環境の名前が確認 `env`します。

- ベース インタープリターを選択します。 Web アプリで選択したものと同じバージョンの Python を使用していることを確認します (runtime.txt または Azure ポータルにある Web アプリの **[アプリケーションの設定]** ブレード)。

- パッケージのダウンロードとインストールのオプションが選択されていることを確認します。

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-add-virtual-env-27.png)

**[作成]** をクリックします。 これにより、仮想環境が作成され、requirements.txt に指定されている依存関係がインストールされます。

### 開発サーバーを使用した実行

F5 キーを押してデバッグを開始します。ローカルに実行されているページが Web ブラウザーで自動的に開きます。

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

ソースにブレークポイントを設定したり、ウォッチ ウィンドウを使用したりできます。さまざまな機能で、[Python Tools for Visual Studio のドキュメント] の詳細を参照してください。

### 変更を加える

次に、アプリケーション ソースやテンプレートに変更を加えてテストを行うことができます。

変更内容をテストしたら、Git リポジトリにコミットします。

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-commit-bottle.png)

### その他のパッケージのインストール

アプリケーションが、Python と Bottle 以外の依存関係を持つ場合があります。

pip を使用して追加のパッケージをインストールできます。 パッケージをインストールするには、仮想環境を右クリックし、**[Install Python Package]** を選択します。

たとえば、Azure storage、service bus などその他の Azure サービスへのアクセスを提供、Python 用 Azure SDK をインストールする次のように入力します `azure`:。

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-install-package-dialog.png)

仮想環境を右クリックし、**[Generate requirements.txt]** を選択して requirements.txt を更新します。

その後、requirements.txt への変更内容を Git リポジトリにコミットします。

### Azure へのデプロイ

デプロイを開始するには、**[同期]** または **[プッシュ]** をクリックします。 同期ではプッシュとプルの両方が実行されます。

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-git-push.png)

仮想環境を作成したり、パッケージをインストールしたりするため、最初のデプロイには時間がかかります。

Visual Studio にはデプロイの進捗状況は表示されません。 出力を確認する場合は、セクションを参照して、 [トラブルシューティング - デプロイ](#troubleshooting-deployment)します。

Azure URL を参照して、変更内容を表示します。


## Web アプリの開発 - Windows - コマンド ライン

### リポジトリの複製

最初に、Azure ポータルで提供される URL を使用してリポジトリを複製し、Azure リポジトリにリモートとして追加します。 詳細については、次を参照してください。 [Azure App Service での GIT による継続的なデプロイ](web-sites-publish-source-control.md)します。

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### 仮想環境の作成

開発環境用の新しい仮想環境を作成します (リポジトリには追加しません)。 Python の仮想環境を再配置することはできません。そのため、アプリケーションで作業する開発者ごとにローカルに仮想環境を作成することになります。

Web アプリで選択したものと同じバージョンの Python を使用していることを確認します (runtime.txt または Azure ポータルにある Web アプリの [アプリケーションの設定] ブレード)。

Python 2.7 の場合:

    c:\python27\python.exe -m virtualenv env

Python 3.4 の場合:

    c:\python34\python.exe -m venv env

アプリケーションに必要な外部パッケージをインストールします。 リポジトリのルートにある requirements.txt ファイルを使用すると、仮想環境にパッケージをインストールできます。

    env\scripts\pip install -r requirements.txt

### 開発サーバーを使用した実行

次のコマンドを使用すると、開発サーバーでアプリケーションを起動できます。

    env\scripts\python app.py

コンソールには、URL とサーバーがリッスンするポートが表示されます。

![](./media/web-sites-python-create-deploy-bottle-app/windows-run-local-bottle.png)

次に、Web ブラウザーを開き、その URL に移動します。

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

### 変更を加える

次に、アプリケーション ソースやテンプレートに変更を加えてテストを行うことができます。

変更内容をテストしたら、Git リポジトリにコミットします。

    git add <modified-file>
    git commit -m "<commit-comment>"

### その他のパッケージのインストール

アプリケーションが、Python と Bottle 以外の依存関係を持つ場合があります。

pip を使用して追加のパッケージをインストールできます。 たとえば、Azure Storage、Service Bus などの他の Azure サービスにアクセスできるようになる Azure SDK for Python をインストールするには、次のように入力します。

    env\scripts\pip install azure

requirements.txt が更新されていることを確認します。

    env\scripts\pip freeze > requirements.txt

変更をコミットします。

    git add requirements.txt
    git commit -m "Added azure package"

### Azure へのデプロイ

デプロイを開始するには、変更を Azure にプッシュします。

    git push azure master

仮想環境の作成、パッケージのインストール、web.config の作成といった、デプロイ スクリプトの出力が表示されます。

Azure URL を参照して、変更内容を表示します。


## Web アプリの開発 - Mac と Linux - コマンド ライン

### リポジトリの複製

最初に、Azure ポータルで提供される URL を使用してリポジトリを複製し、Azure リポジトリにリモートとして追加します。 詳細については、次を参照してください。 [Azure App Service での GIT による継続的なデプロイ](web-sites-publish-source-control.md)します。

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### 仮想環境の作成

開発環境用の新しい仮想環境を作成します (リポジトリには追加しません)。 Python の仮想環境を再配置することはできません。そのため、アプリケーションで作業する開発者ごとにローカルに仮想環境を作成することになります。

Web アプリで選択したものと同じバージョンの Python を使用していることを確認します (runtime.txt または Azure ポータルにある Web アプリの [アプリケーションの設定] ブレード)。

Python 2.7 の場合:

    python -m virtualenv env

Python 3.4 の場合:

    python -m venv env

または
    pyvenv env

アプリケーションに必要な外部パッケージをインストールします。 リポジトリのルートにある requirements.txt ファイルを使用すると、仮想環境にパッケージをインストールできます。

    env/bin/pip install -r requirements.txt

### 開発サーバーを使用した実行

次のコマンドを使用すると、開発サーバーでアプリケーションを起動できます。

    env/bin/python app.py

コンソールには、URL とサーバーがリッスンするポートが表示されます。

![](./media/web-sites-python-create-deploy-bottle-app/mac-run-local-bottle.png)

次に、Web ブラウザーを開き、その URL に移動します。

![](./media/web-sites-python-create-deploy-bottle-app/mac-browser-bottle.png)

### 変更を加える

次に、アプリケーション ソースやテンプレートに変更を加えてテストを行うことができます。

変更内容をテストしたら、Git リポジトリにコミットします。

    git add <modified-file>
    git commit -m "<commit-comment>"

### その他のパッケージのインストール

アプリケーションが、Python と Bottle 以外の依存関係を持つ場合があります。

pip を使用して追加のパッケージをインストールできます。 たとえば、Azure Storage、Service Bus などの他の Azure サービスにアクセスできるようになる Azure SDK for Python をインストールするには、次のように入力します。

    env/bin/pip install azure

requirements.txt が更新されていることを確認します。

    env/bin/pip freeze > requirements.txt

変更をコミットします。

    git add requirements.txt
    git commit -m "Added azure package"

### Azure へのデプロイ

デプロイを開始するには、変更を Azure にプッシュします。

    git push azure master

仮想環境の作成、パッケージのインストール、web.config の作成といった、デプロイ スクリプトの出力が表示されます。

Azure URL を参照して、変更内容を表示します。


## トラブルシューティング - パッケージのインストール

[AZURE.INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]


## トラブルシューティング - 仮想環境

[AZURE.INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]


## 次のステップ

Bottle と Python Tools for Visual Studio の詳細については、次のリンクをご覧ください。

- [Bottle のドキュメント]
- [Python Tools for Visual Studio のドキュメント]

Azure Table Storage と MongoDB の使用方法については、次のリンクをご覧ください。

- [Bottle と Python Tools for Visual Studio を使用した Azure 上の MongoDB]
- [Bottle と Python Tools for Visual Studio を使用した Azure 上の Azure テーブル ストレージ]

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)





[bottle and mongodb on azure with python tools for visual studio]: web-sites-python-ptvs-bottle-table-storage.md 
[bottle and azure table storage on azure with python tools for visual studio]: web-sites-python-ptvs-bottle-table-storage.md 
[azure sdk for python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281 
[azure sdk for python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990 
[python.org]: http://www.python.org/ 
[git for windows]: http://msysgit.github.io/ 
[github for windows]: https://windows.github.com/ 
[python tools for visual studio]: http://aka.ms/ptvs 
[python tools 2.2 for visual studio]: http://go.microsoft.com/fwlink/?LinkID=624025 
[visual studio]: http://www.visualstudio.com/ 
[python tools for visual studio documentation]: http://aka.ms/ptvsdocs 
[bottle documentation]: http://bottlepy.org/docs/dev/index.html 

