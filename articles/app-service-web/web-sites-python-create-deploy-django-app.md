<properties
    pageTitle="Azure での Django を使用した Web アプリの作成"
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
    ms.topic="hero-article" 
    ms.date="11/16/2015"
    ms.author="huvalo"/>


# Azure での Django を使用した Web アプリの作成

このチュートリアルでの Python の実行を開始する方法を説明します [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)します。 Web Apps では、制限付きの無料のホスティングや迅速なデプロイを実行できます。また、Python も使用できます。 アプリケーションの拡張に合わせて、有料のホスティングに切り替えることができます。また、他のすべての Azure サービスと統合することもできます。

Django web フレームワークを使用してアプリケーションを作成します. (このチュートリアルの代替バージョンを参照してください [Flask](web-sites-python-create-deploy-flask-app.md) と [Bottle](web-sites-python-create-deploy-bottle-app.md))。 Azure Marketplaceから Web アプリを作成し、Git デプロイを設定して、リポジトリをローカルで複製します。 その後、ローカルでアプリケーションを実行し、変更を行い、それらをコミットして Azure に送信します。 チュートリアルでは、Windows または Mac/Linux での手順を紹介します。

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。


## 前提条件

- Windows、Mac、または Linux
- Python 2.7 または 3.4
- setuptools、pip、virtualenv (Python 2.7 のみ)
- Git
- [Python Tools for Visual Studio][] (PTVS) に注意してくださいこれは省略可能。

**注**: Python プロジェクト現在は TFS 発行はできません。

### Windows

Python 2.7 または 3.4 (32 ビット) をインストールされていない場合、インストールが勧め [Azure SDK for Python 2.7] または [Azure SDK for Python 3.4] Web Platform Installer を使用します。 いずれかをインストールすると、Python の 32 ビット バージョン、setuptools、pip、virtualenv などがインストールされます (32 ビット Python は Azure ホスト コンピューターにインストールするものです)。 またはから Python を入手できます [python.org]します。

Git、ことをお勧め [Git for Windows] または [GitHub for Windows]します。 Visual Studio を使用する場合は、Git の統合のサポートを使用できます。

インストールをもお勧め [Python Tools 2.2 for Visual Studio]します。 これは省略可能がある場合、 [Visual Studio], 、無料の Visual Studio Community 2013 または Visual Studio Express 2013 for Web を含むし、そうことで、優れた Python IDE です。

### Mac/Linux

Python と Git は既にインストールされていると思いますが、Python 2.7 または 3.4 であることをご確認ください。


## ポータルでの Web アプリの作成

アプリを作成する最初の手順がで web アプリを作成するには、 [Azure ポータル](https://portal.azure.com)します。

1. Azure ポータルにログインし、をクリックして、 **新規** 左下隅のボタンをクリックします。
2. クリックして **Web + モバイル**します。
3. 検索ボックスに、「python」と入力します。
4. 検索結果に次のように選択します。 **Django**, 、クリック **作成**します。
5. 新しい App Service プランやそのリソース グループ名の作成など、新しい Django アプリを構成します。 クリックして **作成**します。
6. 新しく作成した web アプリの Git 発行を構成する」の手順に従って、 [Azure App Service での GIT による継続的なデプロイ](web-sites-publish-source-control.md)します。

## アプリケーションの概要

### Git リポジトリのコンテンツ

ここでは、初期の Git リポジトリにあるファイルの概要を説明します。次のセクションではこれらのファイルを複製します。

    \app\__init__.py
    \app\forms.py
    \app\models.py
    \app\tests.py
    \app\views.py
    \app\static\content\
    \app\static\fonts\
    \app\static\scripts\
    \app\templates\about.html
    \app\templates\contact.html
    \app\templates\index.html
    \app\templates\layout.html
    \app\templates\login.html
    \app\templates\loginpartial.html
    \DjangoWebProject\__init__.py
    \DjangoWebProject\settings.py
    \DjangoWebProject\urls.py
    \DjangoWebProject\wsgi.py

アプリケーションのメイン ソース。 マスター レイアウトを使用した 3 つのページ (index、about、contact) で構成されます。 静的コンテンツとスクリプト (bootstrap、jquery、modernizr、respond など)。

    \manage.py

ローカル管理、開発サーバーのサポート。 これを使用して、アプリケーションをローカルで実行したり、データベースを同期したりします。

    \db.sqlite3

既定のデータベース。 アプリケーションの実行に必要なテーブルが含まれますが、ユーザーは含まれません (ユーザーを作成するにはデータベースを同期します)。

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

プロジェクトで使用するためのファイル [Python Tools for Visual Studio]します。

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

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### 仮想環境の作成

次にローカル開発用の仮想環境を作成します。 右クリックして **Python 環境** 選択 **仮想環境の追加...**します。

- 環境名が、`env` となっていることを確認します。

- ベース インタープリターを選択します。 Web アプリの同じバージョンの選択されている Python を使用してください (runtime.txt または **アプリケーション設定** Azure ポータルで web アプリのブレード)。

- パッケージのダウンロードとインストールのオプションが選択されていることを確認します。

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

クリックして **作成**します。 これにより、仮想環境が作成され、requirements.txt に指定されている依存関係がインストールされます。

### スーパーユーザーの作成

アプリケーションに含まれるデータベースには、スーパーユーザーは定義されていません。 アプリケーションのサインイン機能または Django 管理インターフェイス (有効にしてある場合) を使用するには、スーパーユーザーを作成する必要があります。

プロジェクト フォルダーのコマンド ラインから実行します。

    env\scripts\python manage.py createsuperuser

画面表示に従って、ユーザー名、パスワードなどを設定します。

### 開発サーバーを使用した実行

F5 キーを押してデバッグを開始します。ローカルに実行されているページが Web ブラウザーで自動的に開きます。

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

ソースにブレークポイントを設定したり、ウォッチ ウィンドウを使用したりできます。参照してください、 [Python Tools for Visual Studio Documentation] 詳細については、さまざまな機能です。

### 変更を加える

次に、アプリケーション ソースやテンプレートに変更を加えてテストを行うことができます。

変更内容をテストしたら、Git リポジトリにコミットします。

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### その他のパッケージのインストール

アプリケーションが、Python と Django 以外の依存関係を持つ場合があります。

pip を使用して追加のパッケージをインストールできます。 パッケージをインストールするには、仮想環境を右クリックして **Install Python Package**します。

たとえば、Azure Storage、Service Bus などの他の Azure サービスにアクセスできるようになる Azure SDK for Python をインストールするには、`azure` を入力します。

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

仮想環境を右クリックして **requirements.txt を生成** requirements.txt を更新します。

その後、requirements.txt への変更内容を Git リポジトリにコミットします。

### Azure へのデプロイ

デプロイを開始するには、をクリックして **同期** または **プッシュ**します。 同期ではプッシュとプルの両方が実行されます。

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

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

### スーパーユーザーの作成

アプリケーションに含まれるデータベースには、スーパーユーザーは定義されていません。 アプリケーションのサインイン機能または Django 管理インターフェイス (有効にしてある場合) を使用するには、スーパーユーザーを作成する必要があります。

プロジェクト フォルダーのコマンド ラインから実行します。

    env\scripts\python manage.py createsuperuser

画面表示に従って、ユーザー名、パスワードなどを設定します。

### 開発サーバーを使用した実行

次のコマンドを使用すると、開発サーバーでアプリケーションを起動できます。

    env\scripts\python manage.py runserver

コンソールには、URL とサーバーがリッスンするポートが表示されます。

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

次に、Web ブラウザーを開き、その URL に移動します。

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### 変更を加える

次に、アプリケーション ソースやテンプレートに変更を加えてテストを行うことができます。

変更内容をテストしたら、Git リポジトリにコミットします。

    git add <modified-file>
    git commit -m "<commit-comment>"

### その他のパッケージのインストール

アプリケーションが、Python と Django 以外の依存関係を持つ場合があります。

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

### スーパーユーザーの作成

アプリケーションに含まれるデータベースには、スーパーユーザーは定義されていません。 アプリケーションのサインイン機能または Django 管理インターフェイス (有効にしてある場合) を使用するには、スーパーユーザーを作成する必要があります。

プロジェクト フォルダーのコマンド ラインから実行します。

    env/bin/python manage.py createsuperuser

画面表示に従って、ユーザー名、パスワードなどを設定します。

### 開発サーバーを使用した実行

次のコマンドを使用すると、開発サーバーでアプリケーションを起動できます。

    env/bin/python manage.py runserver

コンソールには、URL とサーバーがリッスンするポートが表示されます。

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

次に、Web ブラウザーを開き、その URL に移動します。

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### 変更を加える

次に、アプリケーション ソースやテンプレートに変更を加えてテストを行うことができます。

変更内容をテストしたら、Git リポジトリにコミットします。

    git add <modified-file>
    git commit -m "<commit-comment>"

### その他のパッケージのインストール

アプリケーションが、Python と Django 以外の依存関係を持つ場合があります。

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


## トラブルシューティング - 静的ファイル

Django には静的ファイルの収集に関する概念が設定されています。 それにより、すべての静的ファイルを元の場所から取得し、単一のフォルダーにそのファイルをコピーします。 このアプリケーションの場合、`/static` にコピーされます。

これが実行されるのは、静的ファイルがさまざまな Django「アプリ」から取得されるためです。 たとえば、Django 管理インターフェイスの静的ファイルは、仮想環境の Django ライブラリ サブフォルダーにあります。 このアプリケーションで定義する静的ファイルは `/app/static` にあります。 複数の Django「アプリ」を使用する場合は、静的ファイルを複数の場所に配置することになります。

アプリケーションをデバッグ モードで実行すると、アプリケーションは元の場所にある静的ファイルを使用します。

リリース モードでアプリケーションを実行すると、アプリケーションは **いない** 静的ファイルを使用します。 ファイルを使用するのは、Web サーバーの役割です。 このアプリケーションの場合は、IIS が `/static` の静的ファイルを使用します。

静的ファイルの収集は、デプロイ スクリプトの一部として自動的に行われます。前に収集されたファイルは消去されます。 つまり、収集はデプロイごとに実行されます。デプロイが少し遅くなりますが、古くなったファイルを使用できないようにすることで、セキュリティの問題を回避できます。

Django アプリケーションで静的なファイルの収集をスキップする場合は、次のようにします。

    \.skipDjango

その後、ローカル コンピューターで手動で収集する必要があります。

    env\scripts\python manage.py collectstatic

`\static` フォルダーを `.gitignore` から削除し、Git リポジトリに追加します。


## トラブルシューティング - 設定

`DjangoWebProject/settings.py` ではアプリケーションのさまざまな設定を変更できます。

開発者が便利なように、デバッグ モードが有効になっています。 ローカルで実行すると、静的ファイルを収集しなくても、イメージやその他の静的コンテンツを表示できるというメリットもあります。

デバッグ モードを無効にするには、次の操作を実行します。

    DEBUG = False

デバッグを無効にすると、Azure のホスト名を含めるために、`ALLOWED_HOSTS` の値を更新する必要があります。 次に例を示します。

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

有効にする場合:

    ALLOWED_HOSTS = (
        '*',
    )

実際には、より複雑な設定により、デバッグ モードとリリース モードを切り替え、ホスト名を取得できます。

Azure ポータルで環境変数を設定することができます **構成** ] ページで、 **アプリ設定** セクションです。  ソースに表示しない値 (接続文字列やパスワードなど) や、Azure とローカル コンピューターで設定を変える値を設定する場合に便利です。 `settings.py` では、`os.getenv` を使用して環境変数をクエリできます。


## データベースの使用

アプリケーションに含まれるデータベースは、sqlite データベースです。 設定がほとんど必要ないため、開発の際に使用すると便利な既定のデータベースです。 データベースはプロジェクト フォルダーの db.sqlite3 ファイルに格納されます。

Azure には、Django アプリケーションから簡単に使用できるデータベース サービスが用意されています。 使用に関するチュートリアル [SQL Database] と [MySQL] 、Django からアプリケーションを表示する手順でデータベースの設定を変更するデータベース サービスの作成に必要な `DjangoWebProject/settings.py`, 、およびインストールに必要なライブラリです。

もちろん、独自のデータベース サーバーを管理する場合は、Azure 上で実行する Windows または Linux 仮想マシンを使用して設定できます。


## Django 管理インターフェイス

モデルの構築を開始したら、一部のデータをデータベースに入力できます。 Django 管理インターフェイスを使用すると、コンテンツをインタラクティブに追加、編集できます。

管理インターフェイスのコードはアプリケーションのソースではコメント アウトされていますが、わかりやすい記号が付けられていますので、簡単に有効にできます (「admin」を検索)。

有効にしたらデータベースを同期し、アプリケーションを実行して `/admin` に移動します。


## 次のステップ

Django と Python Tools for Visual Studio の詳細については、次のリンクをご覧ください。

- [Django のドキュメント]
- [Python Tools for Visual Studio のドキュメント]

SQL Database と MySQL の詳細については、次のリンクをご覧ください。

- [Python Tools for Visual Studio を使用した Azure 上の Django と MySQL]
- [Python Tools for Visual Studio を使用した Azure 上の Django と SQL Database]

詳細については、次を参照してください。、 [Python デベロッパー センター](/develop/python/)します。


## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)


<!--Link references-->
[Django and MySQL on Azure with Python Tools for Visual Studio]: web-sites-python-ptvs-django-mysql.md
[Django and SQL Database on Azure with Python Tools for Visual Studio]: web-sites-python-ptvs-django-sql.md
[SQL Database]: web-sites-python-ptvs-django-sql.md
[MySQL]: web-sites-python-ptvs-django-mysql.md

<!--External Link references-->
[Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281
[Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990
[python.org]: http://www.python.org/
[Git for Windows]: http://msysgit.github.io/
[GitHub for Windows]: https://windows.github.com/
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Django Documentation]: https://www.djangoproject.com/


