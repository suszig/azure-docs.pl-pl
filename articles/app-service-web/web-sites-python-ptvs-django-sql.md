<properties 
    pageTitle="Python Tools 2.2 for Visual Studio を使用した Azure 上の Django と SQL Database" 
    description="Python Tools for Visual Studio を使用して、SQL database インスタンスにデータを保存する Django Web アプリを作成し、Azure App Service Web Apps にデプロイする方法について説明します。" 
    services="app-service\web" 
    tags="python"
    documentationCenter="python" 
    authors="huguesv" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="11/13/2015"
    ms.author="huguesv"/>





# Python Tools 2.2 for Visual Studio を使用した Azure 上の Django と SQL Database

このチュートリアルでは、PTVS サンプル テンプレートのいずれかを使用して簡単な投票 web アプリを作成する [Python Tools for Visual Studio] を使用してします。 このチュートリアルではでもご覧いただけます、 [ビデオ](https://www.youtube.com/watch?v=ZwcoGcIeHF4)します。

Azure でホストされる SQL データベースを使用する方法、SQL データベースを使用する web アプリを構成する方法、および web アプリを発行する方法を学習して [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)します。

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

このセクションでは、サンプル テンプレートを使用して Visual Studio プロジェクトを作成します。 仮想環境を作成し、必要なパッケージをインストールします。 sqlite を使用して、ローカル データベースを作成します。 Web アプリをローカルで実行します。

1.  Visual Studio で、**[ファイル]**、**[新しいプロジェクト]** の順にクリックします。

1.  PTVS サンプル VSIX のプロジェクト テンプレートは、**[Python]** の **[サンプル]** にあります。 **[投票 Django Web プロジェクト]** を選択し、[OK] をクリックしてプロジェクトを作成します。

    ![[新しいプロジェクト] ダイアログ](./media/web-sites-python-ptvs-django-sql/PollsDjangoNewProject.png)

1.  外部のパッケージをインストールするよう求めるメッセージが表示されます。 **[仮想環境にインストールする]** を選択します。

    ![External Packages ダイアログ](./media/web-sites-python-ptvs-django-sql/PollsDjangoExternalPackages.png)

1.  基になるインタープリターとして **Python 2.7** を選択します。

    ![Add Virtual Environment ダイアログ](./media/web-sites-python-ptvs-django-sql/PollsCommonAddVirtualEnv.png)

1.  プロジェクト ノードを右クリックして、**[Python]**、**[Django Sync DB]** の順に選択します。

    ![Django Sync DB コマンド](./media/web-sites-python-ptvs-django-sql/PollsDjangoSyncDB.png)

1.  Django 管理コンソールが表示されます。 プロンプトに従ってユーザーを作成します。

    これにより、プロジェクト フォルダーに sqlite データベースが作成されます。

    ![Django Management Console ウィンドウ](./media/web-sites-python-ptvs-django-sql/PollsDjangoConsole.png)

1.  キーを押して、アプリケーションが動作することを確認 <kbd>f5 キーを押して</kbd>します。

1.  上部にあるナビゲーション バーで、**[ログイン]** をクリックします。

    ![Web ブラウザー](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalMenu.png)

1.  データベースを同期したときに作成したユーザーの資格情報を入力します。

    ![Web ブラウザー](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalLogin.png)

1.  **[サンプル投票の作成]** をクリックします。

    ![Web ブラウザー](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserNoPolls.png)

1.  投票をクリックして投票します。

    ![Web ブラウザー](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqliteBrowser.png)

## SQL Database の作成

データベースには、Azure SQL データベースを作成します。

データベースを作成するには、次のステップを実行します

1.  [Azure ポータル] にログインします。

1.  ナビゲーション ウィンドウの下部にある **[+新規]** をクリックします。 、をクリックして **データ + ストレージ** > **SQL Database**します。


1.  新しいリソース グループを作成することで、新しい SQL Database を構成し、適切な場所を選択します。


1.  SQL Database が作成されたら、データベース ブレードの **[Visual Studio で開く]** をクリックします。
2.  **[Configure your firewall]** をクリックします。
3.  **[ファイアウォールの設定]** ブレードで、 **開始 IP** と **終了 IP**で ファイアウォール規則を追加し、開発用コンピューターのパブリック IP アドレスに設定します。 **[保存]** をクリックします。

    これで、開発用のコンピューターからデータベース サーバーに接続できるようになります。

4.  データベース ブレードに戻り、**[プロパティ]** をクリックし、**[Show database connection strings (データベース接続文字列を表示)]** をクリックします。

2.  [コピー] ボタンを使用して、**ADO.NET** の値をクリップボードにコピーします。

## プロジェクトを構成する

このセクションで SQL データベースを使用する web アプリを構成します。 
対して行います。 また、Django で SQL Database を使用するために必要な Python パッケージも 
別途インストールします。 Web アプリをローカルで実行します。

1.  Visual Studio で、*ProjectName* フォルダーから **settings.py** を開きます。 エディターで、接続文字列を一時的に貼り付けます。 この接続文字列の形式を次に示します。

        Server=<ServerName>,<ServerPort>;Database=<DatabaseName>;User ID=<UserName>;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;


定義を編集 `データベース` 上記の値を使用します。

        DATABASES = {
            'default': {
                'ENGINE': 'sql_server.pyodbc',
                'NAME': '<DatabaseName>',
                'USER': '<UserName>',
                'PASSWORD': '{your_password_here}',
                'HOST': '<ServerName>',
                'PORT': '<ServerPort>',
                'OPTIONS': {
                    'driver': 'SQL Server Native Client 11.0',
                    'MARS_Connection': 'True',
                }
            }
        }

1.  ソリューション エクスプローラーで、**[Python 環境]** の下にある仮想環境を右クリックし、**[Python パッケージのインストール]** を選択します。

1.  パッケージをインストール `pyodbc` を使用して **pip**します。

    ![Install Python Package ダイアログ](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackagePyodbc.png)

1.  パッケージをインストール `django pyodbc azure` を使用して **pip**します。

    ![Install Python Package ダイアログ](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackageDjangoPyodbcAzure.png)

1.  プロジェクト ノードを右クリックして、**[Python]**、**[Django Sync DB]** の順に選択します。

    前のセクションで作成した SQL Database のテーブルが作成されます。 プロンプトに従ってユーザーを作成してください。最初のセクションで作成した sqlite データベースのユーザーと一致させる必要はありません。

    ![Django Management Console ウィンドウ](./media/web-sites-python-ptvs-django-sql/PollsDjangoConsole.png)

1.  `F5` キーでアプリケーションを実行します。 **[サンプル投票の作成]** で作成された投票と、投票によって送信されたデータは、SQL database でシリアル化されます。


## Web アプリを Azure App Service に発行する

Azure .NET SDK を使用すると、Web アプリを  Azure App Service Web Apps に簡単にデプロイできます。

1.  **ソリューション エクスプローラー**で、プロジェクト ノードを右クリックし、**[発行]** をクリックします。


1.  **[Microsoft Azure Web Apps]** をクリックします。

1.  **[新規]** をクリックして、新しい Web アプリを作成します。

1.  次のフィールドに入力し、**[作成]** をクリックします。
-   **[Web アプリケーション名]**
-   **[App Service プラン]**
-   **[リソース グループ]**
-   **[リージョン]**
-   **[データベース サーバー]** は、**[データベースなし]** のままにしておきます。


1.  他のすべての既定値をそのまま使用し、**[発行]** をクリックします。

1.  Web ブラウザーが自動的に開いて、発行した Web アプリが表示されます。 想定どおりに Web アプリ が Azure でホストされている **SQL** データベースを使用して動作していることが確認できます。

    ご利用ありがとうございます。

    ![Web ブラウザー](./media/web-sites-python-ptvs-django-sql/PollsDjangoAzureBrowser.png)

## 次のステップ

Python Tools for Visual Studio、Django および SQL Database の詳細については、以下のリンクを参照してください。

- [Python Tools for Visual Studio のドキュメント]
  - [Web プロジェクト]
  - [クラウド サービス プロジェクト]
  - [リモート Microsoft Azure でデバッグ]
- [Django のドキュメント]
- [SQL データベース]

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
[sql database]: /documentation/services/sql-database/ 

