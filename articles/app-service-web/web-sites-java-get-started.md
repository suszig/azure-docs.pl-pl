<properties
    pageTitle="Azure App Service での Java Web アプリの作成 | Microsoft Azure"
    description="このチュートリアルでは、Java Web アプリを Azure App Service にデプロイする方法を示します。"
    services="app-service\web"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor="jimbe"/>
<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="10/20/2015"
    ms.author="robmcm"/>


# Azure App Service での Java Web アプリの作成

> [AZURE.SELECTOR]
- [.Net](web-sites-dotnet-get-started.md)
- [Node.js](web-sites-nodejs-develop-deploy-mac.md)
- [Java](web-sites-java-get-started.md)
- [PHP - Git](web-sites-php-mysql-deploy-use-git.md)
- [PHP - FTP](web-sites-php-mysql-deploy-use-ftp.md)
- [Python](web-sites-python-ptvs-django-mysql.md)


このチュートリアルは、Java を作成する方法を示しています。 [Azure App Service で web アプリ](http://go.microsoft.com/fwlink/?LinkId=529714) を使用して、 [Azure ポータル](https://portal.azure.com/)します。 Azure ポータルは、Azure リソースの管理に使用できる Web インターフェイスです。
> [AZURE.NOTE] このチュートリアルを完了するには、Microsoft Azure アカウントが必要です。 アカウントを持っていない場合は、 [、Visual Studio のサブスクライバーの特典をアクティブ化][] または [無料試用版にサインアップ][]します。
>
> 場合は、Azure アカウントにサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用:operator[]][]します。 有効期間が短いスターター Web アプリを App Service ですぐに作成できます。このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## Java アプリケーション オプション

複数の方法で Java アプリケーションを App Service Web アプリにセットアップできます。

1. Azure Marketplace からテンプレートを使用する。

    Azure Marketplace には、Java Web アプリを自動的に作成して Tomcat または Jetty Web コンテナーで管理するテンプレートが含まれています。 テンプレートによってセットアップされる Web コンテナーは、構成可能です。 詳細については、次を参照してください。、 [Java テンプレートを使用して Azure Marketplace から](#marketplace) このチュートリアルのセクションです。

1. アプリを作成して**アプリケーション設定**を構成する。

    App Service では、複数のバージョンの Tomcat と Jetty が既定の構成で提供されています。 ホストするアプリケーションが組み込みバージョンの Web コンテナーで動作するようであれば、この方法で Web コンテナーをセットアップするのが最も簡単です。ただし、他の方法で利用できる各種構成機能を利用できません。 この方法では、Azure ポータルでアプリを作成した後、アプリの **[アプリケーション設定]** ブレードに移動して、お使いの Java のバージョンと目的の Java Web コンテナーを選びます。 この方法を使用する場合、アプリをホストするために worker によって使用されるローカル ハード ドライブからアプリが実行されるため、テナントのディスク スペースは使用されません。 このモデルを使用する場合、ファイル システムのこの部分のファイルを編集するためのアクセス権はありません。つまり、*server.xml* ファイルを構成したり、*/lib* フォルダーにライブラリ ファイルを追加したりすることはできません。 詳細については、次を参照してください。、 [を作成する Java web アプリの構成と](#appsettings) このチュートリアルの後半のセクションです。

3. アプリを作成してから構成ファイルを手動でコピーして編集する。

    App Service によって提供されるどの Web コンテナーにもデプロイしないカスタム Java アプリケーションをホストすることがあります。 例として、次にその理由をいくつか示します。

    * お使いの Java アプリケーションに必要な Tomcat または Jetty のバージョンが App Service によって直接サポートされていないか、ギャラリーで提供されていない。
    * お使いの Java アプリケーションが HTTP 要求を受け取り、WAR として既存の Web コンテナーにデプロイされない。
    * 最初から自分で Web コンテナーを構成したい。
    * App Service でサポートされていないバージョンの Java を使用して自分でアップロードしたい。

    これらの場合は、Azure ポータルを使ってアプリを作成して、適切なランタイム ファイルを手動で指定できます。 このとき、ファイルは App Service プランのストレージ領域クォータに対してカウントされます。 詳細については、次を参照してください。 [カスタム Java web アプリを Azure にアップロード](https://acom-sandbox.azurewebsites.net/en-us/documentation/articles/web-sites-java-custom-upload/)します。

## <a name="marketplace"></a> Java テンプレートを使用して Azure Marketplace から

このセクションでは、Azure Marketplace を使用して Java Web アプリを作成する方法について説明します。 Java ベースのモバイル アプリや API アプリを作成する場合にも、同様の一般的なフローを利用できます。

1. サインイン、 [Azure ポータル](https://portal.azure.com/)します。

2. **[新規]、[Marketplace]** の順にクリックします。

    ![](./media/web-sites-java-get-started/newmarketplace.png)

3. **[Web + モバイル]** をクリックします。

    左へスクロールして **[Marketplace]** ブレードを表示すると、**[Web + モバイル]** を選択できます。

4. 検索テキスト ボックスに、**Apache Tomcat** や **Jetty** など、Java アプリケーション サーバーの名前を入力して、Enter キーを押します。

5. 検索結果で、Java アプリケーション サーバーをクリックします。

    ![](./media/web-sites-java-get-started/webmobilejetty.png)

6. 最初の **[Apache Tomcat]** ブレードまたは **[Jetty]** ブレードで、**[作成]** をクリックします。

    ![](./media/web-sites-java-get-started/jettyblade.png)

7. 次の **[Apache Tomcat]** ブレードまたは **[Jetty]** ブレードで、**[Web アプリ]** ボックスに Web アプリの名前を入力します。

    Web アプリの URL は {name}.azurewebsites.net のようになるため、この名前は azurewebsites.net ドメイン内で一意である必要があります。 入力した名前が一意でない場合は、テキスト ボックスに赤色の感嘆符が表示されます。

8. **リソース グループ**を選択するか、新しく作成します。

    リソース グループの詳細については、次を参照してください。 [Azure ポータルを使用して、Azure リソースを管理する](../resource-group-portal.md)します。

9. **App Service プラン/場所**を選択するか、新しく作成します。

    App Service プランに関する詳細については、を参照してください [Azure App Service プランの概要](../azure-web-sites-web-hosting-plans-in-depth-overview.md)。

10. **[作成]** をクリックします。

    ![](./media/web-sites-java-get-started/jettyportalcreate2.png)

    短時間 (通常は 1 分未満) で、新しい Web アプリの作成が完了します。

11. **[Web アプリ]、{作成した新しい Web アプリ}** の順にクリックします。

12. **[URL]** をクリックして新しいサイトを参照します。

    ![](./media/web-sites-java-get-started/jettyurl.png)

    Tomcat には既定のページ セットが用意されているため、Tomcat を選択した場合は次の例のようなページが表示されます。

    ![Web app using Apache Tomcat](./media/web-sites-java-get-started/tomcat.png)

    Jetty を選択した場合は、次のようなページが表示されます。 Jetty には既定のページ セットが用意されていないため、空の Java サイトに使用されるのと同じ JSP がここでも使用されます。

    ![Web app using Jetty](./media/web-sites-java-get-started/jetty.png)

アプリケーション コンテナーで web アプリを作成したを参照してください、 [次のステップ](#next-steps) web アプリにアプリケーションをアップロードする方法についてのセクションです。

## <a name="portal"></a> 作成し、Java web アプリの構成

このセクションでは、Web アプリを作成し、ポータルの **[アプリケーション設定]** ブレードを使用して Java 向けに構成する方法について説明します。

1. サインイン、 [Azure ポータル](https://portal.azure.com/)します。

2. **[新規]、[Web + モバイル]、[Web アプリ]** の順にクリックします。

    ![](./media/web-sites-java-get-started/newwebapp.png)

4. **[Web アプリ]** ボックスに Web アプリの名前を入力します。

    Web アプリの URL は {name}.azurewebsites.net のようになるため、この名前は azurewebsites.net ドメイン内で一意である必要があります。 入力した名前が一意でない場合は、テキスト ボックスに赤色の感嘆符が表示されます。

5. **リソース グループ**を選択するか、新しく作成します。

    リソース グループの詳細については、次を参照してください。 [Azure ポータルを使用して、Azure リソースを管理する](../resource-group-portal.md)します。

6. **App Service プラン/場所**を選択するか、新しく作成します。

    App Service プランに関する詳細については、を参照してください [Azure App Service プランの概要](../azure-web-sites-web-hosting-plans-in-depth-overview.md)。

7. **[作成]** をクリックします。

    ![](./media/web-sites-java-get-started/newwebapp2.png)

8. Web アプリが作成されたら、**[Web アプリ]、{作成した Web アプリ}** の順にクリックします。

    ![](./media/web-sites-java-get-started/selectwebapp.png)

9. **[Web アプリ]** ブレードで、**[設定]** をクリックします。

10. [**アプリケーションの設定**] をクリックします。

11. 目的の **[Java バージョン]** を選択します。

12. 目的の **[Java マイナー バージョン]** を選択します。 **[最新]** を選択すると、お使いのアプリで、その Java メジャー バージョン向けの App Service で使用できる最新のマイナー バージョン使用することになります。

12. 目的の **[Web コンテナー]** を選択します。 "**最新**" で始まるコンテナー名を選択すると、お使いのアプリは、App Service で使用できる Web コンテナー メジャー バージョンの最新バージョンに保たれます。

    ![](./media/web-sites-java-get-started/versions.png)

13. **[保存]** をクリックします。

    お使いの Web アプリはすぐに Java ベースになり、選んだ Web コンテナーを使用するように構成されます。

14. **[Web アプリ]、{作成した新しい Web アプリ}** の順にクリックします。

15. **[URL]** をクリックして新しいサイトを参照します。

    表示された Web ページで、Java ベースの Web アプリが作成されたことを確認します。

## 次のステップ

この時点で、Java アプリケーション サーバーは Azure App Service の Web アプリで動作しています。 Web アプリには、独自のコードを展開するを参照してください。 [Java web アプリにアプリケーションや web ページを追加](web-sites-java-add-app.md)します。

Azure で Java アプリケーションの開発の詳細については、次を参照してください。、 [Java デベロッパー センター](/develop/java/)します。



[activate your visual studio subscriber benefits]: http://go.microsoft.com/fwlink/?LinkId=623901 
[sign up for a free trial]: http://go.microsoft.com/fwlink/?LinkId=623901 
[try app service]: http://go.microsoft.com/fwlink/?LinkId=523751 

