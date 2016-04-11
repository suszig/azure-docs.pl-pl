<properties 
    pageTitle="Monitor a SharePoint site with Application Insights" 
    description="新しいインストルメンテーション キーで新しいアプリケーションの監視を開始します。" 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/06/2015" 
    ms.author="awills"/>

# Application Insights で SharePoint を監視する


[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]

Visual Studio Application Insights を使うと、アプリの可用性、パフォーマンス、利用状況を監視できます。 ここでは、SharePoint サイトのために Application Insights を設定する方法について学習します。


## Application Insights リソースの作成


 [Azure ポータル](http://portal.azure.com), 、新しい Application Insights リソースを作成します。 アプリケーションの種類として ASP.NET を選択します。

![[プロパティ] をクリックし、キーを選択して、Ctrl キーを押しながら C キーを押す](./media/app-insights-sharepoint/01-new.png)


表示されるブレードには、アプリケーションに関するパフォーマンスと使用状況データが表示されます。 次に Azure にログインするときにこのブレードに戻るには、スタート画面でそのタイルを見つけてください。 あるいは、[参照] ボタンをクリックして探します。
    


## Web ページに、スクリプトを追加する

クイック スタートで、Web ページのスクリプトを取得します。

![](./media/app-insights-sharepoint/02-monitor-web-page.png)

追跡するすべてのページの &lt;/head&gt; タグの直前に、スクリプトを挿入します。 Web サイトにマスター ページがある場合は、そこにスクリプトを配置できます。 たとえば、ASP.NET MVC プロジェクトで、View\Shared\_Layout.cshtml にスクリプトを配置します。

このスクリプトには、Application Insights リソースに利用統計情報を転送するインストルメンテーション キーが含まれています。

### コードをサイト ページに追加する

#### マスター ページで

サイトのマスター ページを編集すれば、そのサイトのすべてのページを監視できます。

マスター ページを確認し、SharePoint Designer またはその他のエディターを使用して編集します。

![](./media/app-insights-sharepoint/03-master.png)


直前にコードを追加します </head> タグが挿入されないというバグです。 


![](./media/app-insights-sharepoint/04-code.png)

#### あるいは個別ページで

ページを限定して監視するには、ページ別にスクリプトを追加します。 

Web パーツを挿入し、コード スニペットをそれに埋め込みます。


![](./media/app-insights-sharepoint/05-page.png)


## アプリに関するデータを表示する

アプリケーションを再デプロイします。

アプリケーションのブレードに戻り、 [Azure ポータル](http://portal.azure.com)します。

検索に最初のイベントが表示されます。 

![](./media/app-insights-sharepoint/09-search.png)

大量のデータが予想される場合は、数秒後に [最新の情報に更新] をクリックします。

概要ブレードでクリックして **利用状況分析** ユーザー、セッション、ページ ビューのグラフに表示します。

![](./media/app-insights-sharepoint/06-usage.png)

詳細については、任意のグラフ (例: ページ ビュー) をクリックしてください。

![](./media/app-insights-sharepoint/07-pages.png)

または、ユーザーをクリックしてください。


![](./media/app-insights-sharepoint/08-users.png)



## 次のステップ

* [Web テスト](app-insights-monitor-web-app-availability.md) 、サイトの可用性を監視します。

* [Application Insights](app-insights-overview.md) 他の種類のアプリにします。



<!--Link references-->


 
