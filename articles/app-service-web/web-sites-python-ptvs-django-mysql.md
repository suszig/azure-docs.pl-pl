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

1.  Visual Studio で、**[ファイル]**、**[新しいプロジェクト]** の順にクリックします。

1.  PTVS サンプル VSIX のプロジェクト テンプレートは、**[Python]** の **[サンプル]** にあります。 **[投票 Django Web プロジェクト]** を選択し、[OK] をクリックしてプロジェクトを作成します。

    ![[新しいプロジェクト] ダイアログ](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)

1.  外部のパッケージをインストールするよう求めるメッセージが表示されます。 **[仮想環境にインストールする]** を選択します。

    ![External Packages ダイアログ](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)

1.  基になるインタープリターとして **Python 2.7** を選択します。

    ![Add Virtual Environment ダイアログ](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)

1.  プロジェクト ノードを右クリックして、**[Python]**、**[Django Sync DB]** の順に選択します。

    ![Django Sync DB コマンド](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSyncDB.png)

1.  Django 管理コンソールが表示されます。 プロンプトに従ってユーザーを作成します。

    これにより、プロジェクト フォルダーに sqlite データベースが作成されます。

    ![Django Management Console ウィンドウ](./media/web-sites-python-ptvs-django-mysql/PollsDjangoConsole.png)

1.  キーを押して、アプリケーションが動作することを確認 <kbd>f5 キーを押して</kbd>します。

1.  上部にあるナビゲーション バーで、**[ログイン]** をクリックします。

    ![Web ブラウザー](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)

1.  データベースを同期したときに作成したユーザーの資格情報を入力します。

    ![Web ブラウザー](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)

1.  **[サンプル投票の作成]** をクリックします。

    ![Web ブラウザー](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)

1.  投票をクリックして投票します。

    ![Web ブラウザー](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## MySQL Database を作成する

データベースには、Azure 上でホストされる ClearDB MySQL データベースを作成します。

別の方法として、Azure 上で実行する独自の仮想マシンを作成し、自分で MySQL をインストールして管理できます。

次の手順に従って、無料プランでデータベースを作成できます。

1.  ログイン、 [Azure ポータル](https://portal.azure.com/)します。

1.  ナビゲーション ウィンドウの上部にある **[新規]** をクリックします。 **[データ + ストレージ]**、**[MySQL データベース]** の順にクリックします。

1.  検索ボックスに「**mysql**」と入力し、次に **[MySQL データベース]** をクリックし、**[作成]** をクリックします。 -->

1.  新しいリソース グループを作成することで、新しい MySQL データベースを構成し、適切な場所を選択します。


1.  MySQL データベースが作成されたら、データベース ブレードの **[プロパティ]** をクリックします。
2.  [コピー] ボタンを使用して、**CONNECTIONSTRING** の値をクリップボードにコピーします。

## プロジェクトを構成する

このセクションでは、先ほど作成した MySQL データベースを使用するよう Web アプリを構成します。 Django と MySQL データベースを使用するために必要な追加の Python パッケージもインストールします。 Web アプリをローカルで実行します。

1.  Visual Studio で、*ProjectName* フォルダーから **settings.py** を開きます。 エディターで、接続文字列を一時的に貼り付けます。 この接続文字列の形式を次に示します。

        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>

    既定のデータベース **ENGINE** を変更して MySQL を使用し、**CONNECTIONSTRING** の**NAME**、**USER**、**PASSWORD**、**HOST** の値を設定します。

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

1.  ソリューション エクスプローラーで、**[Python 環境]** の下にある仮想環境を右クリックし、**[Python パッケージのインストール]** を選択します。

1. パッケージをインストール `mysql python` を使用して **easy_install**します。

    ![Install Package ダイアログ](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)

1.  プロジェクト ノードを右クリックして、**[Python]**、**[Django Sync DB]** の順に選択します。

    前のセクションで作成した MySQL データベースのテーブルが作成されます。 プロンプトに従ってユーザーを作成してください。最初のセクションで作成した sqlite データベースのユーザーと一致させる必要はありません。

    ![Django Management Console ウィンドウ](./media/web-sites-python-ptvs-django-mysql/PollsDjangoConsole.png)

1.  `F5` キーでアプリケーションを実行します。 **[サンプル投票の作成]** で作成された投票と、投票によって送信されたデータは、MySQL データベースでシリアル化されます。

## Web アプリを Azure App Service に発行する

Azure .NET SDK を使用すると、Web アプリを Azure App Service に簡単にデプロイできます。

1.  **ソリューション エクスプローラー**で、プロジェクト ノードを右クリックし、**[発行]** をクリックします。

    ![Publish Web ダイアログ](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)

1.  **[Microsoft Azure Web Apps]** をクリックします。

1.  **[新規]** をクリックして、新しい Web アプリを作成します。

1.  次のフィールドに入力し、**[作成]** をクリックします。
-   **[Web アプリケーション名]**
-   **[App Service プラン]**
-   **[リソース グループ]**
-   **[リージョン]**
-   **[データベース サーバー]** は、**[データベースなし]** のままにしておきます。


1.  他のすべての既定値をそのまま使用し、**[発行]** をクリックします。

1.  Web ブラウザーが自動的に開いて、発行した Web アプリが表示されます。 想定どおりに Web アプリが Azure でホストされている **MySQL** データベースを使用して動作していることが確認できます。

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





[python developer center]: /develop/python/ 
[azure cloud services]: ../cloud-services-python-ptvs.md 
[azure portal]: https://portal.azure.com 
[python tools for visual studio]: http://aka.ms/ptvs 
[python tools 2.2 for visual studio]: http://go.microsoft.com/fwlink/?LinkID=624025 
[python tools 2.2 for visual studio samples vsix]: http://go.microsoft.com/fwlink/?LinkID=624025 
[azure sdk tools for vs 2013]: http://go.microsoft.com/fwlink/?LinkId=323510 
[azure sdk tools for vs 2015]: http://go.microsoft.com/fwlink/?LinkId=518003 
[python 2.7 32-bit]: http://go.microsoft.com/fwlink/?LinkId=517190 
[python tools for visual studio documentation]: http://aka.ms/ptvsdocs 
[remote debugging on microsoft azure]: http://go.microsoft.com/fwlink/?LinkId=624026 
[web projects]: http://go.microsoft.com/fwlink/?LinkId=624027 
[cloud service projects]: http://go.microsoft.com/fwlink/?LinkId=624028 
[django documentation]: https://www.djangoproject.com/ 
[mysql]: http://www.mysql.com/ 

