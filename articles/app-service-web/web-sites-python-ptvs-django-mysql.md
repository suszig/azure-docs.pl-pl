<properties 
    pageTitle="Python Tools 2.2 for Visual Studio を使用した Azure 上の Django と MySQL" 
    description="Python Tools for Visual Studio を使用して、MySQL データベース インスタンスにデータを保存する Django Web アプリを作成し、Azure App Service Web Apps にデプロイする方法について説明します。" 
    services="app-service\web" 
    documentationCenter="python" 
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="get-started-article" 
    ms.date="11/17/2015"
    ms.author="huvalo"/>

# Python Tools 2.2 for Visual Studio を使用した Azure 上の Django と MySQL 

> [AZURE.SELECTOR]
- [.Net](web-sites-dotnet-get-started.md)
- [Node.js](web-sites-nodejs-develop-deploy-mac.md)
- [Java](web-sites-java-get-started.md)
- [PHP - Git](web-sites-php-mysql-deploy-use-git.md)
- [PHP - FTP](web-sites-php-mysql-deploy-use-ftp.md)
- [Python](web-sites-python-ptvs-django-mysql.md)

このチュートリアルでは、PTVS サンプル テンプレートのいずれかを使用して簡単な投票 web アプリを作成する [Python Tools for Visual Studio] を使用してします。 このチュートリアルではでもご覧いただけます、 [ビデオ](https://www.youtube.com/watch?v=oKCApIrS0Lo)します。

Azure でホストされる MySQL サービスを使用する方法、MySQL を使用する web アプリを構成する方法、および web アプリを発行する方法を学習して [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)します。

Bottle、Flask、Django web フレームワークを MongoDB、Azure テーブル ストレージ、MySQL および SQL Database のサービスを使用した、PTVS では、[Python デベロッパー センター] を Azure App Service Web Apps の開発を取り上げたその他の記事を参照してください。 この記事は、App Service 上では、中に、手順は、[Azure クラウド サービス] を開発する際に似ています。

## 前提条件

 - Visual Studio 2013 または 2015
 - [Python Tools 2.2 for Visual Studio]
 - [Python は、Visual Studio サンプル VSIX の 2.2 を Tools]
 - [Azure SDK Tools for VS 2013] または [Azure SDK Tools for VS 2015]
 - [Python 2.7 32-bit]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## プロジェクトを作成する

このセクションでは、サンプル テンプレートを使用して Visual Studio プロジェクトを作成します。 仮想環境を作成し、必要なパッケージをインストールします。 sqlite を使用して、ローカル データベースを作成します。 その後、アプリケーションをローカルで実行します。

1.  Visual Studio で、次のように選択します。 **ファイル**, 、**新しいプロジェクト**します。

1.  PTVS サンプル vsix のプロジェクト テンプレートが [利用可能な **Python**, 、**サンプル**します。 選択 **投票 Django Web プロジェクト** プロジェクトを作成するには、[ok] をクリックします。

    ![[新しいプロジェクト] ダイアログ](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)

1.  外部のパッケージをインストールするよう求めるメッセージが表示されます。 選択 **仮想環境にインストール**します。

    ![External Packages ダイアログ](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)

1.  選択 **Python 2.7** ベース インタープリターとして。

    ![Add Virtual Environment ダイアログ](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)

1.  プロジェクト ノードを右クリックして **Python**, 、**Django Sync DB**します。

    ![Django Sync DB コマンド](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSyncDB.png)

1.  Django 管理コンソールが表示されます。 プロンプトに従ってユーザーを作成します。

    これにより、プロジェクト フォルダーに sqlite データベースが作成されます。

    ![Django Management Console ウィンドウ](./media/web-sites-python-ptvs-django-mysql/PollsDjangoConsole.png)

1.  キーを押して、アプリケーションが動作することを確認します。 <kbd>F5</kbd>.

1.  クリックして **ログイン** 上部にあるナビゲーション バーからです。

    ![Web ブラウザー](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)

1.  データベースを同期したときに作成したユーザーの資格情報を入力します。

    ![Web ブラウザー](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)

1.  クリックして **サンプル投票の作成**します。

    ![Web ブラウザー](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)

1.  投票をクリックして投票します。

    ![Web ブラウザー](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## MySQL Database を作成する

データベースには、Azure 上でホストされる ClearDB MySQL データベースを作成します。

別の方法として、Azure 上で実行する独自の仮想マシンを作成し、自分で MySQL をインストールして管理できます。

次の手順に従って、無料プランでデータベースを作成できます。

1.  ログイン、 [Azure ポータル](https://portal.azure.com/)します。

1.  ナビゲーション ウィンドウの上部にあるをクリックして **新規**します。 クリックして **データ + ストレージ** > **MySQL データベース**します。 

  

1.  型"**mysql**"検索] ボックスをクリックし、 **MySQL データベース**, 、クリックして **作成**します。  -->
    <!-- ![Choose Add-on Dialog](./media/web-sites-python-ptvs-django-mysql/PollsDjangoClearDBAddon1.png) -->

1.  新しいリソース グループを作成することで、新しい MySQL データベースを構成し、適切な場所を選択します。

    <!-- ![Personalize Add-on Dialog](./media/web-sites-python-ptvs-django-mysql/PollsDjangoClearDBAddon2.png) -->

1.  MySQL データベースが作成されたら、クリックして **プロパティ** データベース ブレードにします。
2.  値をコピー] ボタンを使用して **接続文字列** クリップボードにコピーします。

## プロジェクトを構成する

このセクションでは、先ほど作成した MySQL データベースを使用するよう Web アプリを構成します。 Django と MySQL データベースを使用するために必要な追加の Python パッケージもインストールします。 Web アプリをローカルで実行します。

1.  Visual Studio で開きます **settings.py**, から、 *ProjectName* フォルダーです。 エディターで、接続文字列を一時的に貼り付けます。 この接続文字列の形式を次に示します。

        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>

    既定のデータベース変更 **エンジン** MySQL を使用し、値を設定する **名前**, 、**ユーザー**, 、**パスワード** と **ホスト** から、 **CONNECTIONSTRING**します。

        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.mysql',
                'NAME': '<Database>',
                'USER': '<User Id>',
                'PASSWORD': '<Password>',
                'HOST': '<Data Source>',
                'PORT': '',
            }
        }


1.  ソリューション エクスプ ローラーで [ **Python 環境**, 仮想環境を右クリックして、選択 **Install Python Package**します。

1. パッケージをインストール `mysql-python` を使用して **easy_install**します。

    ![Install Package ダイアログ](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)

1.  プロジェクト ノードを右クリックして **Python**, 、**Django Sync DB**します。 

    前のセクションで作成した MySQL データベースのテーブルが作成されます。 プロンプトに従ってユーザーを作成してください。最初のセクションで作成した sqlite データベースのユーザーと一致させる必要はありません。

    ![Django Management Console ウィンドウ](./media/web-sites-python-ptvs-django-mysql/PollsDjangoConsole.png)

1.  `F5` キーでアプリケーションを実行します。 作成された投票 **[Create Sample Polls** と投票によって送信されたデータは、MySQL データベースでシリアル化されます。

## Web アプリを Azure App Service に発行する

Azure .NET SDK を使用すると、Web アプリを Azure App Service に簡単にデプロイできます。

1.   **ソリューション エクスプ ローラー**, プロジェクト ノードを右クリックし、選択、 **発行**します。

    ![Publish Web ダイアログ](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)

1.  をクリックして **Microsoft Azure Web Apps**します。

1.  をクリックして **新規** 新しい web アプリを作成します。

1.  次のフィールドに入力し、クリックして **作成**します。
    -   **[Web アプリケーション名]**
    -   **App Service プラン**
    -   **リソース グループ**
    -   **リージョン**
    -   ままにして **データベース サーバー** 設定 **データベースが存在しません**

    <!-- ![Create Site on Microsoft Azure Dialog](./media/web-sites-python-ptvs-django-mysql/PollsCommonCreateWebSite.png) -->

1.  その他のすべての既定値をそのまま使用し、クリックして **発行**します。

1.  Web ブラウザーが自動的に開いて、発行した Web アプリが表示されます。 を使用して想定どおりに動作して web アプリを表示する必要があります、 **MySQL** Azure でホストされているデータベースです。

    ご利用ありがとうございます。

    ![Web ブラウザー](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)

## 次のステップ

Python Tools for Visual Studio、Django および MySQL の詳細については、以下のリンクを参照してください。

- [Python Tools for Visual Studio のドキュメント]
  - [Web プロジェクト]
  - [クラウド サービス プロジェクト]
  - [リモート Microsoft Azure でデバッグ]
- [Django のドキュメント]
- [MySQL]

詳細については、次を参照してください。、 [Python デベロッパー センター](/develop/python/)します。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)


<!--Link references-->
[Python Developer Center]: /develop/python/
[Azure Cloud Services]: ../cloud-services-python-ptvs.md

<!--External Link references-->
[Azure Portal]: https://portal.azure.com
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 for Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bit]: http://go.microsoft.com/fwlink/?LinkId=517190 
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Remote Debugging on Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Web Projects]: http://go.microsoft.com/fwlink/?LinkId=624027
[Cloud Service Projects]: http://go.microsoft.com/fwlink/?LinkId=624028
[Django Documentation]: https://www.djangoproject.com/
[MySQL]: http://www.mysql.com/
 

