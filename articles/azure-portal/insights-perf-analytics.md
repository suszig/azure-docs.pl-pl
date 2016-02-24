<properties 
    pageTitle="Azure Web アプリのパフォーマンスの監視" 
    description="チャートの読み込みおよび応答時間、依存関係の情報やパフォーマンス警告を設定します。" 
    services="azure-portal"
    documentationCenter="na"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="azure-portal" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/23/2015" 
    ms.author="awills"/>

# Azure Web アプリのパフォーマンスの監視

 [Azure ポータル](http://portal.azure.com) アプリケーションの依存関係の統計と詳細を収集する監視を設定することができます、 [Azure web アプリ](../app-service-web/app-service-web-overview.md) または [仮想マシン](../virtual-machines/virtual-machines-about.md)します。

Azure は、アプリケーション パフォーマンス監視をサポートしています (または、 *APM*) を活用して *拡張*します。 これらの拡張機能はアプリケーションにインストールされ、データを収集し、監視サービスにレポートを返します。 

Application Insights と New Relic は、利用できるパフォーマンス監視拡張機能のうちの 2 つです。 それらを使用するには、実行時にエージェントをインストールします。 Application Insights には、SDK を使用してコードをビルドするためのオプションもあります。 SDK では、アプリの使用状況とパフォーマンスをさらに詳細に監視するコードを記述できます。

## 拡張機能を有効にする

1. をクリックして **参照** web アプリまたは仮想マシンを選択し、インストルメント化します。

2. Application Insights または New Relic の拡張機能を追加します。 

    Web アプリをインストルメント化する場合:

![設定、拡張機能、追加、Application Insights](./media/insights-perf-analytics/05-extend.png)

仮想マシンを使用している場合は、次のように操作します。

![分析タイルをクリックする](./media/insights-perf-analytics/10-vm1.png)

### Application Insights 用のオプション: SDK を使用してリビルドする

Application Insights では、アプリへの SDK のインストールによって、より詳細なテレメトリを提供できます。 

Application Insights SDK を Visual Studio のプロジェクトに追加します。

![Web プロジェクトを右クリックし、[Application Insights の追加] を選択する](./media/insights-perf-analytics/03-add.png)

ログインが要求されたら、Azure アカウントの資格情報を使用します。

テレメトリをテストするには、開発用コンピューターでアプリを実行するか、単純に再発行します。 

SDK が API を提供できるように [カスタム テレメトリを記述](../app-insights-api-custom-events-metrics.md) 使用状況を追跡します。

## データを検索する

しばらくの間、アプリケーションを使用していくつかのテレメトリを生成します。

1. 次に、Web アプリまたは仮想マシンのブレードから、インストールされている拡張機能を確認します。
2. アプリケーションを示す行をクリックし、そのプロバイダーに移動します。
![[最新の情報に更新] をクリックする](./media/insights-perf-analytics/06-overview.png)

使用することも **参照** Application Insights コンポーネントまたはを使用して New Relic アカウントに直接移動します。

Application Insights のブレードに取得した後、実行できます。
- パフォーマンスを開きます。

![Application Insights の概要ブレードで、[パフォーマンス] タイルをクリックする](./media/insights-perf-analytics/07-dependency.png)

- ドリル スルーし、個別の要求を表示します。

![グリッドの依存関係をクリックし、関連する要求を参照します。](./media/insights-perf-analytics/08-requests.png)

- この例は、SQL 呼び出しの数と、平均期間や標準偏差などの関連する統計情報を含む、SQL 依存関係に費やされた時間を示しています。 

![](./media/insights-perf-analytics/01-example.png) 



## 次のステップ

* [サービス正常性のメトリックスを監視](insights-how-to-customize-monitoring.md) 、サービスの可用性と応答性を確認します。
* [監視を有効にし、診断](insights-how-to-use-diagnostics.md) 、サービスに関する詳細な頻度の高いメトリックを収集します。
* [警告通知を受け取る](insights-receive-alert-notifications.md) するたびに運用上のイベントが発生したり、メトリックがしきい値を超えます。
* 使用 [JavaScript アプリと web ページ向けの Application Insights](../app-insights-web-track-usage.md) web ページを参照しているブラウザーに関するクライアント分析を取得します。
* [可用性と任意の web ページの応答性監視](../app-insights-monitor-web-app-availability.md) のかどうか、ページは停止検索できるように、Application Insights を使用します。
 
