<properties 
    pageTitle="Python Tools 2.2 for Visual Studio を使用した Azure 上の Flask と Azure テーブル ストレージ" 
    description="Python Tools for Visual Studio を使用して、Azure テーブル ストレージにデータを保存する Flask Web アプリを作成し、Azure App Service Web Apps にデプロイする方法について説明します。" 
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
    ms.date="11/18/2015"
    ms.author="huvalo"/>




# Python Tools 2.2 for Visual Studio を使用した Azure 上の Flask と Azure テーブル ストレージ 

このチュートリアルで使用して [Python Tools for Visual Studio] 投票 web アプリのサンプル テンプレートのいずれかを使用して簡単な作成をします。 このチュートリアルではでもご覧いただけます、 [ビデオ](https://www.youtube.com/watch?v=qUtZWtPwbTk)します。

この投票 Web アプリでは、リポジトリの抽象化を定義します。そのため、異なる種類のリポジトリ (メモリ内、Azure テーブル ストレージ、MongoDB) を簡単に切り替えることができます。

Azure ストレージ アカウントを作成する方法、Azure テーブル ストレージを使用する web アプリを構成する方法、および web アプリを発行する方法を学習して [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)します。

参照してください、 [Python Developer Center] Bottle を使用して、PTVS では、Azure App Service Web Apps の開発を取り上げたその他の記事では、Flask および Django web フレームワークと、MongoDB、Azure テーブル ストレージ、MySQL および SQL データベース サービスです。 開発するときにこの記事は、App Service 上では、中に、手順と似ています [Azure Cloud Services]します。

## 前提条件

 - Visual Studio 2013 または 2015
 - [Python Tools 2.2 for Visual Studio]
 - [Python Tools 2.2 for Visual Studio サンプル VSIX]
 - [Azure SDK Tools for VS 2013] または [Azure SDK Tools for VS 2015]
 - [Python 2.7 32-bit] または [Python 3.4 32-bit]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## プロジェクトを作成する

このセクションでは、サンプル テンプレートを使用して Visual Studio プロジェクトを作成します。 仮想環境を作成し、必要なパッケージをインストールします。 次に、既定のメモリ内リポジトリを使用してアプリケーションをローカルで実行します。

1.  Visual Studio で、次のように選択します。 **ファイル**, 、**新しいプロジェクト**します。

1.  PTVS サンプル vsix のプロジェクト テンプレートが [利用可能な **Python**, 、**サンプル**します。 選択 **投票 Flask Web プロジェクト** プロジェクトを作成するには、[ok] をクリックします。

    ![[新しいプロジェクト] ダイアログ](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskNewProject.png)

1.  外部のパッケージをインストールするよう求めるメッセージが表示されます。 選択 **仮想環境にインストール**します。

    ![External Packages ダイアログ](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskExternalPackages.png)

1.  選択 **Python 2.7** または **Python 3.4** ベース インタープリターとして。

    ![Add Virtual Environment ダイアログ](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAddVirtualEnv.png)

1.  `F5` キーを押して、アプリケーションが動作することを確認します。 既定では、アプリケーションは、構成を必要としないメモリ内リポジトリを使用します。 Web サーバーが停止すると、すべてのデータが失われます。

1.  クリックして **[Create Sample Polls**, 投票] をクリックし、投票します。

    ![Web ブラウザー](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskInMemoryBrowser.png)

## Azure のストレージ アカウントの作成

ストレージ操作を行うには、Azure のストレージ アカウントが必要です。 ストレージ アカウントを作成するには、次のステップを実行します

1.  ログイン、 [Azure ポータル](https://portal.azure.com/)します。

2. クリックして、 **新規** ポータルの上部にあるアイコンを左クリックし、 **データ + ストレージ** > **ストレージ アカウント**します。 をクリックして **作成**, 、ストレージ アカウントに一意の名前を付けるされ、新しい作成 [リソース グループ](../resource-group-overview.md) にします。

    <!-- ![New Button](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAzurePlusNew.png) -->

    ストレージ アカウントが作成されると、 **通知** ボタンが緑色の点滅 **成功** し、ストレージ アカウントのブレードが開いて、作成した新しいリソース グループに属していることを表示します。

5. クリックして、 **設定** ブレードで、ストレージ アカウントの一部です。 アカウント名とプライマリ キーをメモします。

    この情報は、次のセクションでプロジェクトを構成するために必要です。

## プロジェクトを構成する

このセクションでは、先ほど作成したストレージ アカウントを使用するための構成をアプリケーションに対して行います。 Azure ポータルから接続の設定を取得する方法を見ていきます。 その後、アプリケーションをローカルで実行します。

1.  Visual Studio で、ソリューション エクスプ ローラーでプロジェクト ノードを右クリックし、選択 **プロパティ**します。 をクリックして、 **デバッグ** ] タブをクリックします。

    ![Project Debug 設定](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageProjectDebugSettings.png)

1.  アプリケーションに必要な環境変数の値を設定 **サーバー コマンドのデバッグ**, 、**環境**します。

        REPOSITORY_NAME=azuretablestorage
        STORAGE_NAME=<storage account name>
        STORAGE_KEY=<primary access key>

    これは、環境変数を設定時に、 **[デバッグ開始]**します。 ときに設定する変数の場合、 **デバッグなしで開始**, 、下にある同じ値を設定 **サーバー コマンドの実行** もします。

    Windows コントロール パネルを使用して環境変数を定義してもかまいません。 ソース コードまたはプロジェクト ファイルに資格情報を格納するのを避ける場合には、これがより優れた方法です。 新しい環境変数の値をアプリケーションから利用するためには、Visual Studio を再起動する必要があります。

1.  Azure テーブル ストレージ リポジトリを実装するコードは **models/azuretablestorage.py**します。 参照してください、 [documentation] Python からテーブル サービスを使用する方法についての詳細。

1.  `F5` キーでアプリケーションを実行します。 作成された投票 **[Create Sample Polls** と投票によって送信されたデータは、Azure テーブル ストレージにシリアル化されます。

    > [AZURE.NOTE] Python 2.7 仮想環境によっては、Visual Studio で、例外による中断があります。  `F5` を押して Web プロジェクトの読み込みを続行します。

1.  参照、 **に関する** アプリケーションを使用していることを確認する] ページ、 **Azure テーブル ストレージ** リポジトリです。

    ![Web ブラウザー](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageAbout.png)

## Azure テーブル ストレージを調査する

ストレージ テーブルは、Visual Studio のサーバー エクスプローラーを使用して簡単に表示したり編集したりできます。 このセクションでは、投票アプリケーションに使用されているテーブルの内容をサーバー エクスプローラーを使用して表示します。

> [AZURE.NOTE] Microsoft Azure ツールをインストールする、として使用可能である必要がありますの一部では、 [Azure SDK for .NET]します。

1.  開いている **サーバー エクスプ ローラー**します。 展開 **Azure**, 、**ストレージ**, 、ストレージ アカウントを **テーブル**します。

    <!-- ![Server Explorer](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonServerExplorer.png) -->

1.  ダブルクリックして、 **ポーリング** または **選択肢** エンティティを追加、削除、編集と同様に、ドキュメント ウィンドウで、テーブルの内容を表示するテーブル。

    <!-- ![Table Query Results](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonServerExplorerTable.png) -->

## Web アプリを Azure App Service に発行する

Azure .NET SDK を使用すると、Web アプリを Azure App Service に簡単にデプロイできます。

1.  をクリックして **Microsoft Azure Web Apps**します。

1.  をクリックして **新規** 新しい web アプリを作成します。

1.  次のフィールドに入力し、クリックして **作成**します。
    -   **[Web アプリケーション名]**
    -   **App Service プラン**
    -   **リソース グループ**
    -   **リージョン**
    -   ままにして **データベース サーバー** 設定 **データベースが存在しません**

    <!-- ![Create Site on Microsoft Azure Dialog](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonCreateWebSite.png) -->

1.  その他のすべての既定値をそのまま使用し、クリックして **発行**します。

1.  Web ブラウザーが自動的に開いて、発行した Web アプリが表示されます。 参照する場合は、情報] ページが表示されますが使用される、 **インメモリ** リポジトリ、いない、 **Azure テーブル ストレージ** リポジトリです。

    あるためで指定された既定値を使用して、環境変数が、Azure App Service で Web アプリのインスタンスに設定されていないため **settings.py**します。

## Web Apps インスタンスを構成する

このセクションでは、Web Apps インスタンスの環境変数を構成します。

1.   [Azure ポータル](https://portal.azure.com), をクリックして、web アプリのブレードを開きます **参照** > **Web Apps** > web アプリの名前。

1.  Web アプリのブレードで、[ **設定をすべて**, 、順にクリックして **アプリケーション設定**します。

    <!-- ![Top Menu](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonWebSiteTopMenu.png) -->

1.  下へスクロールして、 **アプリ設定** セクションし、値を設定します **移動**, 、**前** と **STORAGE\_KEY** 前のセクションで説明したようです。

    <!-- ![App Settings](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonWebSiteConfigureSettingsTableStorage.png) -->

1. をクリックして **保存**, 、し **再起動** し、最後に **参照**します。

    <!-- ![Bottom Menu](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonWebSiteConfigureBottomMenu.png) -->

1.  を使用して想定どおりに動作して web アプリを表示する必要があります、 **Azure テーブル ストレージ** リポジトリです。

    ご利用ありがとうございます。

    ![Web ブラウザー](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureBrowser.png)

## 次のステップ

Python Tools for Visual Studio、Flask および Azure テーブル ストレージの詳細については、以下のリンクを参照してください。

- [Python Tools for Visual Studio のドキュメント]
  - [Web プロジェクト]
  - [クラウド サービス プロジェクト]
  - [Microsoft Azure でのリモート デバッグ]
- [Flask のドキュメント]
- [Azure Storage (Azure Storage)]
- [Azure SDK for Python]
- [Python からテーブル ストレージ サービスを使用する方法]

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)


<!--Link references-->
[Python Developer Center]: /develop/python/
[Azure Cloud Services]: ../cloud-services-python-ptvs.md
[documentation]: ../storage-python-how-to-use-table-storage.md
[How to Use the Table Storage Service from Python]: ../storage-python-how-to-use-table-storage.md

<!--External Link references-->
[Azure Portal]: https://portal.azure.com
[Azure SDK for .NET]: http://azure.microsoft.com/downloads/
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 for Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2013]: http://go.microsoft.com/fwlink/?LinkId=323510
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?linkid=518003
[Python 2.7 32-bit]: http://go.microsoft.com/fwlink/?LinkId=517190 
[Python 3.4 32-bit]: http://go.microsoft.com/fwlink/?LinkId=517191
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Flask Documentation]: http://flask.pocoo.org/
[Remote Debugging on Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Web Projects]: http://go.microsoft.com/fwlink/?LinkId=624027
[Cloud Service Projects]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure Storage]: http://azure.microsoft.com/documentation/services/storage/
[Azure SDK for Python]: https://github.com/Azure/azure-sdk-for-python
 


