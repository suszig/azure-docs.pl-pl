<properties
    pageTitle="Azure App Service での WordPress Web アプリの作成 | Microsoft Azure"
    description="Azure ポータルを使用して WordPress ブログ用の新しい Azure Web アプリを作成する方法について説明します。"
    services="app-service\web"
    documentationCenter="php"
    authors="tfitzmac"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="hero-article"
    ms.date="11/13/2015"
    ms.author="tomfitz"/>

# Azure App Service での WordPress Web アプリの作成

このチュートリアルでは、Azure Marketplace でアプリケーションを検索して App Service Web アプリにインストールする方法を示します。 このチュートリアルでは例として WordPress ブログ サイトを使用しますが、他の多くの Marketplace アプリケーションについても手順は同様です。

チュートリアルを完了すると、独自の WordPress ブログ サイトをクラウドで運用できるようになります。

![WordPress サイト](./media/web-sites-php-web-site-gallery/wpdashboard.png)

学習内容:

* Azure Marketplace でアプリケーション テンプレートを検索する方法。
* Azure App Service でテンプレートに基づく Web アプリを作成する方法。
* 新しい Web アプリと MySQL データベースの Azure App Service 設定を構成する方法。

Azure Marketplace には、Microsoft、サード パーティ企業、およびオープン ソース ソフトウェア活動によって開発された多種多様な人気の Web アプリが用意されています。 Web アプリがなどのさまざまな一般的なフレームワークで構築された [PHP](/develop/nodejs/) WordPress などの [.NET](/develop/net/), 、[Node.js](/develop/nodejs/), 、[Java](/develop/java/), 、および [Python](/develop/python/), 、入力する文字数。 使用しているブラウザーは、必要な唯一のソフトウェアを Azure Marketplace から web アプリを作成する、 [Azure ポータル](https://portal.azure.com/)します。 

> [AZURE.NOTE]
> このチュートリアルを完了するには、Microsoft Azure アカウントが必要です。 アカウントを持っていない場合は、[Visual Studio サブスクライバーの特典を有効にする](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)か、[無料試用版にサインアップ](/en-us/pricing/free-trial/?WT.mc_id=A261C142F)してください。
>
> 場合は、Azure アカウントにサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751)します。 有効期間が短いスターター Web アプリを App Service ですぐに作成できます。このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## WordPress を選択して Azure App Service 用に構成する

1. ログイン、 [Azure ポータル](https://portal.azure.com/)します。

2. クリックして **新しい**します。
    
    ![新規作成][5]
    
3. 検索 **WordPress**, 、クリックして **WordPress**します。

    ![WordPress リスト][7]
    
5. WordPress アプリの説明を読んだらクリックして **作成**します。

    ![作成](./media/web-sites-php-web-site-gallery/create.png)

4. Web アプリの名前を入力、 **Web アプリ** ボックス。

    Web アプリの URL は {name}.azurewebsites.net のようになるため、この名前は azurewebsites.net ドメイン内で一意である必要があります。 入力した名前が一意でない場合は、テキスト ボックスに赤色の感嘆符が表示されます。

8. サブスクリプションが複数ある場合には、使用するものを 1 つ選択します。 

5. 選択、 **リソース グループ** か新規に作成します。

    リソース グループの詳細については、次を参照してください。 [Azure ポータルを使用して、Azure リソースを管理する](../resource-group-portal.md)です。

5. 選択、 **App Service プラン/場所** か新規に作成します。

    App Service プランに関する詳細については、次を参照してください [Azure App Service プランの概要。](../azure-web-sites-web-hosting-plans-in-depth-overview.md) 

7. クリックして **データベース**, 、し、次に、 **新しい MySQL データベース** ブレードは、MySQL データベースを構成するために必要な値を指定します。

    a. 新しい名前を入力するか、既定の名前をそのまま使用します。

    b. ままにして、 **データベースの種類** 設定 **Shared**します。

    c. Web アプリ用に選択したのと同じ場所を選択します。

    d. 価格レベルを選択します。 このチュートリアルでは、Mercury (無料で、最小限の許可された接続とディスク領域を使用可能) で問題ありません。

8.  **新しい MySQL データベース** ブレードで、をクリックして **OK**します。 

8.  **WordPress** ブレードで、法律条項に同意し、 **作成**します。 

    ![Web アプリの構成](./media/web-sites-php-web-site-gallery/configure.png)

    Azure App Service によって、通常は 1 分以内に Web アプリが作成されます。 進捗状況を監視するには、ポータル ページの上部にあるベル アイコンをクリックします。

    ![進捗状況インジケーター](./media/web-sites-php-web-site-gallery/progress.png)

## WordPress Web アプリの起動と管理
    
7. Web アプリの作成が完了したら、Azure ポータルで、アプリケーションを作成したリソース グループに移動し、Web アプリとデータベースを確認できます。

    電球のアイコンで余分なリソースが [Application Insights](/services/application-insights/), 、web アプリの監視サービスを提供します。

1.  **リソース グループ** ブレードで、web アプリケーションの行をクリックします。

    ![Web アプリの構成](./media/web-sites-php-web-site-gallery/resourcegroup.png)

2. Web アプリのブレードでクリックして **参照**します。

    ![サイトの URL][browse]

3. WordPress の **ようこそ** ] ページで、WordPress に必要な構成情報を入力してをクリックし、 **WordPress のインストール**します。

    ![WordPress の構成](./media/web-sites-php-web-site-gallery/wpconfigure.png)

4. 作成した資格情報を使用してログイン、 **ようこそ** ページです。  

5. サイトのダッシュボード ページが開きます。    

    ![WordPress サイト](./media/web-sites-php-web-site-gallery/wpdashboard.png)

## 次のステップ

これでギャラリーから PHP Web アプリを作成してデプロイする方法はわかりました。 Azure で PHP を使用する方法の詳細については、次を参照してください。、 [PHP デベロッパー センター](/develop/php/)します。

App Service Web Apps の使用方法の詳細については、ページの左側 (ワイド ブラウザー ウィンドウの場合) またはページの上部 (幅の狭いブラウザー ウィンドウの場合) に表示されるリンクを参照してください。 

## 変更内容
* Web サイトから App Service への変更ガイド 』 を参照してください。 [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)します。

[5]: ./media/web-sites-php-web-site-gallery/startmarketplace.png
[7]: ./media/web-sites-php-web-site-gallery/search-web-app.png
[browse]: ./media/web-sites-php-web-site-gallery/browse-web.png

