<properties 
    pageTitle="ASP.NET 5 向けの Application Insights" 
    description="Web アプリケーションの可用性、パフォーマンス、使用状況を監視します。" 
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
    ms.date="11/11/2015" 
    ms.author="awills"/>


# ASP.NET 5 向けの Application Insights

Visual Studio Application Insights を使うと、Web アプリケーションの可用性、パフォーマンス、利用状況を監視できます。 アプリのパフォーマンスと効果に関するフィードバックが得られたら、各開発ライフサイクルにおける設計の方向性について、情報に基づいて選択できます。

![例](./media/app-insights-asp-net-five/sample.png)

サブスクリプションが必要 [Microsoft Azure](http://azure.com)します。 Windows、XBox Live、またはその他の Microsoft クラウド サービスに与えられる Microsoft アカウントでサインインします。


## 使用の開始

Visual Studio 2015 でプロジェクトを作成した場合、Application Insights は既に用意されています。 それ以外の場合に従ってください、 [ファースト ステップ ガイド](https://github.com/Microsoft/ApplicationInsights-aspnet5/wiki/Getting-Started)します。

## Application Insights の使用

サインイン、 [Microsoft Azure ポータル](https://portal.azure.com) し、[参照] アプリを監視するために作成したリソースにします。

別のブラウザー ウィンドウで、しばらくの間、アプリを使用します。 Application Insights グラフにデータが表示されます  ([更新] のクリックが必要な場合があります)。 開発中は少量のデータのみが表示されますが、これらのグラフは、アプリを発行して多数のユーザーが使用したときに真に有用になります。

[概要] ページは役に立つ可能性が最も高いできたらパフォーマンス グラフを示します。 サーバーの応答時間、ページの読み込み時間、および失敗した要求の数。 グラフの詳細とデータを表示するには、グラフをクリックします。

ポータルのビューは、次の 2 つの主なカテゴリに分類されます。

* [メトリックス エクスプ ローラー](app-insights-metrics-explorer.md) グラフとテーブルのメトリックと応答時間、障害発生率やメトリックを自分で作成するなど、カウントを表示、 [API](app-insights-api-custom-events-metrics.md)します。 アプリとそのユーザーの理解を深めるには、プロパティ値によってデータをフィルター処理してセグメント化します。
* [エクスプ ローラーの検索](app-insights-diagnostic-search.md) の特定の要求、例外、ログ トレースを自分で作成したイベントなどの個々 のイベントを一覧表示、 [API](app-insights-api-custom-events-metrics.md)します。 イベントのフィルター処理と検索、問題を調査するための関連イベント間の移動を行います。

## Alerts

* セットアップ [可用性テスト](app-insights-monitor-web-app-availability.md) を継続的に、世界中の場所からの web サイトをテストし、いずれかのテストが失敗するとすぐに電子メールを受け取ります。
* セットアップ [メトリック アラート](app-insights-monitor-web-app-availability.md) 応答時間や例外率などのメトリックが外部の許容範囲を移動するかどうかを知る。


## さらに多くのテレメトリを取得する

* [依存関係を監視](app-insights-dependencies.md) するかどうかは残りの部分、SQL、またはその他の外部のリソースはの速度が低下するを参照してください。
* [API を使用して](app-insights-api-custom-events-metrics.md) 独自のイベントと、アプリのパフォーマンスと使用状況の詳細を表示のメトリックを送信します。
* [可用性テスト](app-insights-monitor-web-app-availability.md) 、世界中から常にアプリケーションを確認します。


## オープン ソース

[コードを読んで協力して](https://github.com/Microsoft/ApplicationInsights-aspnet5)





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
[roles]: app-insights-resources-roles-access-control.md 
[start]: app-insights-overview.md 
[usage]: app-insights-web-track-usage.md 

