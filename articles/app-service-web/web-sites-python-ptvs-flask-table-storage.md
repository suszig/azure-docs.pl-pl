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

このチュートリアルでは、PTVS サンプル テンプレートのいずれかを使用して簡単な投票 web アプリを作成する [Python Tools for Visual Studio] を使用してします。 このチュートリアルではでもご覧いただけます、 [ビデオ](https://www.youtube.com/watch?v=qUtZWtPwbTk)します。

この投票 Web アプリでは、リポジトリの抽象化を定義します。そのため、異なる種類のリポジトリ (メモリ内、Azure テーブル ストレージ、MongoDB) を簡単に切り替えることができます。

Azure ストレージ アカウントを作成する方法、Azure テーブル ストレージを使用する web アプリを構成する方法、および web アプリを発行する方法を学習して [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)します。

Bottle、Flask、Django web フレームワークを MongoDB、Azure テーブル ストレージ、MySQL および SQL Database のサービスを使用した、PTVS では、[Python デベロッパー センター] を Azure App Service Web Apps の開発を取り上げたその他の記事を参照してください。 この記事は、App Service 上では、中に、手順は、[Azure クラウド サービス] を開発する際に似ています。

## 前提条件

 - Visual Studio 2013 または 2015
 - [Python Tools 2.2 for Visual Studio]
 - [Python は、Visual Studio サンプル VSIX の 2.2 を Tools]
 - [Azure SDK Tools for VS 2013] または [Azure SDK Tools for VS 2015]
 - [Python 2.7 32-bit] または [Python 3.4 32-bit]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## プロジェクトを作成する

このセクションでは、サンプル テンプレートを使用して Visual Studio プロジェクトを作成します。 仮想環境を作成し、必要なパッケージをインストールします。 次に、既定のメモリ内リポジトリを使用してアプリケーションをローカルで実行します。

1.  Visual Studio で、**[ファイル]**、**[新しいプロジェクト]** の順にクリックします。

1.  PTVS サンプル VSIX のプロジェクト テンプレートは、**[Python]** の **[サンプル]** にあります。 **[投票 Flask Web プロジェクト]** を選択し、[OK] をクリックしてプロジェクトを作成します。

    ![[新しいプロジェクト] ダイアログ](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskNewProject.png)

1.  外部のパッケージをインストールするよう求めるメッセージが表示されます。 **[仮想環境にインストールする]** を選択します。

    ![External Packages ダイアログ](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskExternalPackages.png)

1.  ベース インタープリターとして **[Python 2.7]** または **[Python 3.4]** を選択します。

    ![Add Virtual Environment ダイアログ](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAddVirtualEnv.png)

1.  `F5` キーを押してアプリケーションの動作を確認します 既定では、アプリケーションは、構成を必要としないメモリ内リポジトリを使用します。 Web サーバーが停止すると、すべてのデータが失われます。

1.  **[Create Sample Polls]** をクリックし、投票内容をクリックして投票します。

    ![Web ブラウザー](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskInMemoryBrowser.png)

## Azure のストレージ アカウントの作成

ストレージ操作を行うには、Azure のストレージ アカウントが必要です。 ストレージ アカウントを作成するには、次のステップを実行します

1.  ログイン、 [Azure ポータル](https://portal.azure.com/)します。

2. ポータルの左上にある **[新規]** アイコンをクリックし、**[データ + ストレージ]** > **[ストレージ アカウント]** をクリックします。 をクリックして **作成**, 、ストレージ アカウントに一意の名前を付けるされ、新しい作成 [リソース グループ](../resource-group-overview.md) にします。


 ストレージ アカウントが作成されると、**[通知]** ボタンに緑色の "**成功**" という文字が点滅し、ストレージ アカウントのブレードが開いて、作成した新しいリソース グループに属していることが示されます。

5. ストレージ アカウントのブレードの **[設定]** 部をクリックします。 アカウント名とプライマリ キーをメモします。

    この情報は、次のセクションでプロジェクトを構成するために必要です。

## プロジェクトを構成する

このセクションでは、先ほど作成したストレージ アカウントを使用するための構成をアプリケーションに対して行います。 Azure ポータルから接続の設定を取得する方法を見ていきます。 その後、アプリケーションをローカルで実行します。

1.  Visual Studio のソリューション エクスプローラーで、使用するプロジェクト ノードを右クリックし、**[プロパティ]** を選択します。 **[デバッグ]** タブをクリックします。

    ![Project Debug 設定](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageProjectDebugSettings.png)

1.  **[サーバー コマンドのデバッグ]** の **[環境]** で、アプリケーションに必要な環境変数の値を設定します。

        REPOSITORY_NAME=azuretablestorage
        STORAGE_NAME=<storage account name>
        STORAGE_KEY=<primary access key>

    これにより、**デバッグを開始**したときに環境変数が設定されます。 **デバッグなしで開始**したときに変数を設定する場合は、**[サーバー コマンドの実行]** でも同じ値を設定します。

    Windows コントロール パネルを使用して環境変数を定義してもかまいません。 ソース コードまたはプロジェクト ファイルに資格情報を格納するのを避ける場合には、これがより優れた方法です。 新しい環境変数の値をアプリケーションから利用するためには、Visual Studio を再起動する必要があります。

1.  Azure テーブル ストレージ リポジトリを実装するコードは、**models/azuretablestorage.py** にあります。 Python からテーブル サービスを使用する方法の詳細については、[ドキュメント] を参照してください。

1.  `F5` キーでアプリケーションを実行します。 **[サンプル投票の作成]** で作成された投票と、投票によって送信されたデータは、Azure テーブル ストレージでシリアル化されます。
    > [AZURE.NOTE] Python 2.7 仮想環境では、Visual Studio で例外による中断が発生する場合があります。 キーを押して `f5 キーを押して` web プロジェクトの読み込みを続行します。

1.  アプリケーションが、**Azure テーブル ストレージ** リポジトリを使用していることを確認するには、**[情報]** ページを確認します。

    ![Web ブラウザー](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageAbout.png)

## Azure テーブル ストレージを調査する

ストレージ テーブルは、Visual Studio のサーバー エクスプローラーを使用して簡単に表示したり編集したりできます。 このセクションでは、投票アプリケーションに使用されているテーブルの内容をサーバー エクスプローラーを使用して表示します。
> [AZURE.NOTE] Microsoft Azure ツールをインストールする、使用できる必要があります [Azure SDK for .NET] の一部として。

1.  **[サーバー エクスプローラー]**を開きます。 **[Azure]**、**[ストレージ]**、使用するストレージ アカウント、**[テーブル]** の順に展開します。


1.  **投票**テーブルか**選択肢**テーブルをダブルクリックすると、ドキュメント ウィンドウでテーブルの内容を表示し、エンティティを追加、削除、編集できます。


## Web アプリを Azure App Service に発行する

Azure .NET SDK を使用すると、Web アプリを Azure App Service に簡単にデプロイできます。

1.  **[Microsoft Azure Web Apps]** をクリックします。

1.  **[新規]** をクリックして、新しい Web アプリを作成します。

1.  次のフィールドに入力し、**[作成]** をクリックします。
-   **[Web アプリケーション名]**
-   **[App Service プラン]**
-   **[リソース グループ]**
-   **[リージョン]**
-   **[データベース サーバー]** は、**[データベースなし]** のままにしておきます。


1.  他のすべての既定値をそのまま使用し、**[発行]** をクリックします。

1.  Web ブラウザーが自動的に開いて、発行した Web アプリが表示されます。 [情報] ページを参照すると、**Azure テーブル ストレージ**リポジトリではなく、**メモリ内**リポジトリを使用していることが確認できます。

    これは、Azure App Service の Web Apps インスタンスで環境変数が設定されていないので、**settings.py** で指定された既定値が使用されているためです。

## Web Apps インスタンスを構成する

このセクションでは、Web Apps インスタンスの環境変数を構成します。

1.  [Azure ポータル](https://portal.azure.com), をクリックして、web アプリのブレードを開きます **参照** > **Web Apps** > web アプリの名前。

1.  Web アプリのブレードで、**[すべての設定]**、**[アプリケーションの設定]** の順にクリックします。


1.  下へスクロールして、 **アプリ設定** セクションし、値を設定します **移動**, 、**前** と **STORAGE\_KEY** 前のセクションで説明したようです。


1. **[保存]**、**[再起動]** の順にクリックし、最後に **[参照]** をクリックします。


1.  想定どおりに Web アプリが**Azure テーブル ストレージ** リポジトリを使用して動作していることが確認できます。

    ご利用ありがとうございます。

    ![Web ブラウザー](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureBrowser.png)

## 次のステップ

Python Tools for Visual Studio、Flask および Azure テーブル ストレージの詳細については、以下のリンクを参照してください。

- [Python Tools for Visual Studio のドキュメント]
  - [Web プロジェクト]
  - [クラウド サービス プロジェクト]
  - [リモート Microsoft Azure でデバッグ]
- [Flask のドキュメント]
- [Azure ストレージ]
- [Azure SDK for Python]
- [Python からテーブル ストレージ サービスを使用する方法]

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)





[python developer center]: /develop/python/ 
[azure cloud services]: ../cloud-services-python-ptvs.md 
[documentation]: ../storage-python-how-to-use-table-storage.md 
[how to use the table storage service from python]: ../storage-python-how-to-use-table-storage.md 
[azure portal]: https://portal.azure.com 
[azure sdk for .net]: http://azure.microsoft.com/downloads/ 
[python tools for visual studio]: http://aka.ms/ptvs 
[python tools 2.2 for visual studio]: http://go.microsoft.com/fwlink/?LinkID=624025 
[python tools 2.2 for visual studio samples vsix]: http://go.microsoft.com/fwlink/?LinkID=624025 
[azure sdk tools for vs 2013]: http://go.microsoft.com/fwlink/?LinkId=323510 
[azure sdk tools for vs 2015]: http://go.microsoft.com/fwlink/?linkid=518003 
[python 2.7 32-bit]: http://go.microsoft.com/fwlink/?LinkId=517190 
[python 3.4 32-bit]: http://go.microsoft.com/fwlink/?LinkId=517191 
[python tools for visual studio documentation]: http://aka.ms/ptvsdocs 
[flask documentation]: http://flask.pocoo.org/ 
[remote debugging on microsoft azure]: http://go.microsoft.com/fwlink/?LinkId=624026 
[web projects]: http://go.microsoft.com/fwlink/?LinkId=624027 
[cloud service projects]: http://go.microsoft.com/fwlink/?LinkId=624028 
[azure storage]: http://azure.microsoft.com/documentation/services/storage/ 
[azure sdk for python]: https://github.com/Azure/azure-sdk-for-python 

