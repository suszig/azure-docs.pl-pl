<properties 
    pageTitle="Application Insights の .NET トレース ログを調べる" 
    description="Trace、NLog、または Log4Net で生成されたログを検索します。" 
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
    ms.date="11/25/2015" 
    ms.author="awills"/>


# Application Insights の .NET トレース ログを調べる

送信されるログは NLog、log4Net、System.Diagnostics.Trace を診断追跡に、ASP.NET アプリケーションを使用する場合 [Visual Studio Application Insights ][start], することができますを表示、検索したりします。 ログはアプリケーションから送信される他の利用統計情報と結合されます。それにより、互いのユーザー要求にサービスを提供することに関連付けられているトレースを特定し、それらを他のイベントや例外レポートに相互に関連付けることができます。
> [AZURE.NOTE] ログ キャプチャ モジュールは必要ですか。 サード パーティのロガーの便利なアダプターでは考えて既に NLog を使用していない場合は、log4Net、System.Diagnostics.Trace を呼び出すだけ [アプリケーション インサイト TrackTrace()](app-insights-api-custom-events-metrics.md#track-trace) 直接します。

まだインストールしていない場合 [Application Insights をプロジェクトの設定 ][start], はすぐに実行します。 プロジェクト ファイルが生成されます、 `ApplicationInsights.config` と NuGet パッケージ `Microsoft.ApplicationInsights.Web`します。


## ログ記録フレームワークにアダプターをインストールする

ログ記録フレームワーク (log4Net、NLog、System.Diagnostics.Trace) を使用している場合、それらのログを他の利用統計情報と共に Application Insights に送信するアダプターをインストールできます。

1. log4Net または NLog を使用する場合は、プロジェクト内にインストールします。
2. ソリューション エクスプローラーでプロジェクトを右クリックし、[**NuGet パッケージの管理**] を選択します。
3. Search for "Application Insights"

    ![適切なパッケージのプレリリース バージョンを入手する](./media/app-insights-asp-net-trace-logs/appinsights-36nuget.png)

4. 次のいずれかの適切なパッケージを選択します。
  + Microsoft.ApplicationInsights.TraceListener (System.Diagnostics.Trace コールをキャプチャするため)
  + Microsoft.ApplicationInsights.NLogTarget
  + Microsoft.ApplicationInsights.Log4NetAppender

NuGet パッケージは、必要なアセンブリをインストールし、web.config または app.config も変更します。

#### 診断ログの呼び出しを挿入する

System.Diagnostics.Trace を使用する場合、通常の呼び出しは次のようになります。

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

log4net、または NLog を使用する場合

    logger.Warn("Slow response - database01");

## トレース API を直接利用する

Application Insights トレース API を直接呼び出すことができます。 ログ記録のアダプターはこの API を使用します。

次に例を示します。

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");

TrackTrace の利点は、比較的長いデータをメッセージの中に配置できることです。 たとえば、その中に POST データをエンコードできます。


## ログを調査する

[概要] ブレードで、アプリの [Application Insights ポータルの ][portal], 、選択 [検索 ][diagnostic]します。

![Application Insights で、](./media/app-insights-asp-net-trace-logs/020-diagnostic-search.png)

![診断検索](./media/app-insights-asp-net-trace-logs/10-diagnostics.png)

たとえば、次の操作ができます。

* ログ トレースまたは特定のプロパティを持つ項目をフィルター処理する
* 特定の項目の詳細に調べる
* 同じユーザー要求に関連する (つまり、OperationId が同じ) 他の利用統計情報を探す
* このページの構成をお気に入りとして保存する

> [AZURE.NOTE] **サンプリング。**アプリケーションが送信するデータ量が多く、Application Insights SDK for ASP.NET バージョン 2.0.0-beta3 以降を使用している場合は、アダプティブ サンプリング機能が動作して、テレメトリの一定の割合のみが送信される可能性があります。 [サンプリングについて説明します。](app-insights-sampling.md)

## 次のステップ

[ASP.NET][exceptions]

[の詳細については、診断検索の ][diagnostic]します。



## トラブルシューティング

### <a name="emptykey"></a>「インストルメンテーション キーを空にすることはできません」というエラーが表示されます。

Application Insights をインストールしないでログ アダプターの Nuget パッケージをインストールした可能性があります。

ソリューション エクスプ ローラーで右クリック `ApplicationInsights.config` 選択 **Application Insights の更新**します。 Azure へのサインインを促すダイアログが表示されます。または、Application Insights のリソースを作成するか、既存のリソースを再利用します。 これで問題は修正されます。

### 診断検索にトレースが表示されますが、他のイベントがありません

すべてのイベントと要求がパイプラインを通過するまで時間がかかることがあります。

### <a name="limits"></a>データの量が保持されますか。

各アプリケーションで、1 秒あたり 500 イベントまでです。 イベントは 7 日間保持されます。

### 予期されるログ エントリの一部が表示されません

アプリケーションが送信するデータ量が多く、Application Insights SDK for ASP.NET バージョン 2.0.0-beta3 以降を使用している場合は、アダプティブ サンプリング機能が動作して、テレメトリの一定の割合のみが送信される可能性があります。 [サンプリングについて説明します。](app-insights-sampling.md)

## <a name="add"></a>次のステップ

* [可用性と応答性のテスト ][availability]
* [トラブルシューティング ][qna]








[availability]: app-insights-monitor-web-app-availability.md 
[diagnostic]: app-insights-diagnostic-search.md 
[exceptions]: app-insights-web-failures-exceptions.md 
[portal]: http://portal.azure.com/ 
[qna]: app-insights-troubleshoot-faq.md 
[start]: app-insights-overview.md 

