<properties 
    pageTitle="ASP.NET 向けの Application Insights" 
    description="オンプレミスまたは Microsoft Azure Web アプリケーションのパフォーマンス、可用性、使用状況を Application Insights で分析します。" 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="awills"/>



# ASP.NET 向けの Application Insights の設定

[AZURE.INCLUDE [app-insights-selector-get-started-dotnet](../../includes/app-insights-selector-get-started-dotnet.md)]

Application Insights SDK は実行中の Web アプリケーションから Azure ポータルにテレメトリを送信します。Azure ポータルにサインインすれば、アプリのパフォーマンスや使用状況をグラフで確認できます。

![パフォーマンス監視グラフのサンプル](./media/app-insights-asp-net/10-perf.png)

特定の要求、例外、ログ イベントを調査し、相互に関連付けることもできます。 API を利用してテレメトリを追加し、パフォーマンスと使用状況を詳しく監視できます。

#### 開始する前に

必要なもの:

* サブスクリプションを [Microsoft Azure](http://azure.com)します。 チームまたは組織の Azure サブスクリプションの場合、所有者あなたを追加できますを使用して、 [Microsoft アカウント](http://live.com)します。
* Visual Studio 2013 Update 3 以降。

## <a name="ide"></a> Visual Studio でプロジェクトに Application Insights を追加します。

#### 新しいプロジェクトの場合

Visual Studio に新しいプロジェクトを作成するとき、Application Insights が選択されていることを確認してください。


![ASP.NET プロジェクトを作成する](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)


#### 既存のプロジェクトの場合

ソリューション エクスプローラーでプロジェクトを右クリックし、[Application Insights の追加] を選択します。

![[Application Insights の追加] を選択する](./media/app-insights-asp-net/appinsights-03-addExisting.png)





#### セットアップ オプション

初めての場合には、Microsoft Azure にログインまたはサインインするように求められます

このアプリがより大きなアプリケーションの一部である場合は、**[設定の構成]** を使用して、他のコンポーネントと同じリソース グループに配置することをお勧めします。


#### <a name="land"></a> Application Insights の追加 ' 何か。

コマンドは、次の手順をでした (する代わりが [を手動で行う](app-insights-start-monitoring-app-health-usage.md) 希望するかどうか)。

* Application Insights リソースが作成 [Azure ポータルの ][portal]します。 ここにデータが表示されます。 リソースを識別する*インストルメンテーション キー*を取得します。
* プロジェクトに Application Insights Web SDK NuGet パッケージが追加されます。 Visual Studio で表示するには、プロジェクトを右クリックし、[NuGet パッケージの管理] を選択します。
* インストルメンテーション キーを配置 `ApplicationInsights.config`します。


## <a name="run"></a> プロジェクトを実行します。

F5 キーを使用してアプリケーションを実行して、試します。さまざまなページが開き、いくつかのテレメトリが生成されます。

Visual Studio で、送信されたイベント数が表示されます。

![](./media/app-insights-asp-net/appinsights-09eventcount.png)

## <a name="monitor"></a> Application Insights を開く

Application Insights のリソースを開く、 [Azure ポータルの ][portal]します。

![プロジェクトを右クリックして Azure ポータルを開く](./media/app-insights-asp-net/appinsights-04-openPortal.png)

### メトリック: 集計データ

概要グラフでデータを探します。 最初、1 つまたは 2 つのポイントだけが表示されます。 次に例を示します。

![クリックしてより多くのデータを表示する](./media/app-insights-asp-net/12-first-perf.png)

任意のグラフをクリックして、より詳細なメトリックを表示します。 [メトリックの詳細についてを説明します。][perf]

* *ユーザーまたはページのデータはありませんか?* - [ユーザーとページのデータを追加する](../article/application-insights/app-insights-asp-net-client.md)

### 検索: 個々のイベント

個々の要求とそれに関連するイベントを調査するには [検索] を開きます。

![](./media/app-insights-asp-net/21-search.png)

[検索の詳細についてください。](app-insights-diagnostic-search.md)

* *関連するイベントがない場合 -*セットアップ [サーバー例外](../article/application-insights/app-insights-asp-net-exception-mvc.md) と [の依存関係](../article/application-insights/app-insights-asp-net-dependencies.md)します。

### データが表示されない場合

* 対象が正しいことを確認します。 サインイン、 [Azure ポータル](https://portal.azure.com), 、] をクリックして"を参照 >"、"Application Insights"し、アプリケーションを選択します。
* アプリケーションを使用して、テレメトリがいくつか生成されるようにさまざまなページを開きます。
* 開いている、 [検索 ][diagnostic] ブレードで、個々 のイベントを表示します。 メトリック パイプラインを経由すると、イベントの表示に少し時間がかかる場合があります。
* 数秒待機してから [最新の情報に更新] をクリックします。
* 参照してください [トラブルシューティング ][qna]します。


## アプリケーションの発行

ここで、アプリケーションをデプロイし、データ累積を確認します。

デバッグ モードで実行している場合、テレメトリはパイプラインにより時間が短縮されるので、数秒でデータが表示されます。 アプリケーションをデプロイすると、データ累積速度は遅くなります。

#### ビルド サーバーで問題が発生した場合

参照してください [このトラブルシューティング項目](app-insights-troubleshoot-faq.md#NuGetBuild)します。
> [AZURE.NOTE] (ASP.NET SDK バージョン 2.0.0-beta3 以降を使用している状態で) アプリから大量のテレメトリが生成されると、アダプティブ サンプリング モジュールからイベントの代表的な部分のみが送信され、ポータルに送信されるデータ量が自動的に削減されます。 ただし、同じ要求に関連するイベントはグループ単位で選択または選択解除されるので、関連するイベントごとに操作できます。 
> [サンプリングについて](app-insights-sampling.md)します。

## 次のステップ

- [ユーザーとページのデータ](../article/application-insights/app-insights-asp-net-client.md#selector1)
- [例外](../article/application-insights/app-insights-asp-net-exception-mvc.md#selector1)
- [依存関係](../article/application-insights/app-insights-asp-net-dependencies.md#selector1)
- [可用性](../article/application-insights/app-insights-monitor-web-app-availability.md#selector1)





## 新しいバージョンの SDK にアップグレードするには

アップグレードする、 [SDK の新しいリリース](app-insights-release-notes-dotnet.md), 、NuGet パッケージ マネージャーをもう一度開いて、インストールされているパッケージでフィルター処理します。 [Microsoft.ApplicationInsights.Web]、[アップグレード] の順に選択します。

ApplicationInsights.config をカスタマイズしている場合は、アップグレードする前にコピーを保存しておき、後から新しいバージョンに変更をマージします。



## <a name="video"></a>ビデオ

> [AZURE.VIDEO getting-started-with-application-insights]





[api]: app-insights-api-custom-events-metrics.md 
[apikey]: app-insights-api-custom-events-metrics.md#ikey 
[availability]: app-insights-monitor-web-app-availability.md 
[azure]: ../insights-perf-analytics.md 
[client]: app-insights-javascript.md 
[detect]: app-insights-detect-triage-diagnose.md 
[diagnostic]: app-insights-diagnostic-search.md 
[knowusers]: app-insights-overview-usage.md 
[metrics]: app-insights-metrics-explorer.md 
[netlogs]: app-insights-asp-net-trace-logs.md 
[perf]: app-insights-web-monitor-performance.md 
[portal]: http://portal.azure.com/ 
[qna]: app-insights-troubleshoot-faq.md 
[redfield]: app-insights-monitor-performance-live-website-now.md 
[roles]: app-insights-resources-roles-access-control.md 
[start]: app-insights-overview.md 

