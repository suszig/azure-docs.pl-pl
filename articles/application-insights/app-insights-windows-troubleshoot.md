<properties 
    pageTitle="Windows デバイスの Application Insights のトラブルシューティング" 
    description="Windows デバイスの Application Insights に関するトラブルシューティング ガイドと質疑応答です。" 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/24/2015" 
    ms.author="awills"/>


# Windows デバイス用 Application Insights のトラブルシューティングおよび Q&A

質問または問題 [Windows ][windows]でしょうか。 ここでは、いくつかのヒントを紹介します。



## データが表示されない

*Application Insights が正常に追加された後でアプリケーションを実行したところ、ポータルにデータが表示されません。*

* 少し待ってから、[最新の情報に更新] をクリックします。
* ApplicationInsights.config ファイル内で、インストルメンテーション キーが定義されていることをご確認ください。また、定義されているインストルメンテーション キーが Application Insights ポータル内のキーと同じであることをご確認ください。 キーを表示するには、概要ブレードで [Essentials] をクリックします。
* アプリを確認してください [発信ネットワーク アクセスを要求](https://msdn.microsoft.com/library/windows/apps/hh452752.aspx)します。
* エミュレーターまたはテスト デバイスと Application Insights ポータルの間にファイアウォールはありますか。 dc.services.visualstudio.com と f5.services.visualstudio.com への送信トラフィック用に TCP ポート 80 および 443 を開く必要がある可能性があります。
* Microsoft Azure のスタート ボードで、サービス状態マップをご確認ください。 アラート表示がある場合は、"OK" が表示されるまで待ってから、Application Insights アプリケーション ブレードをいったん閉じて開き直します。


#### データが表示されていたのに停止しました。

* チェック、 [状態ブログ](http://blogs.msdn.com/b/applicationinsights-status/)します。
* データ ポイントの月間クォータに達していませんか? Open Settings/Quota and Pricing to find out. 上限に達している場合は、プランをアップグレードするか、追加容量分を購入することができます。 参照してください、 [料金プラン](http://azure.microsoft.com/pricing/details/application-insights/)します。


## ユニバーサル アプリに Application Insights を追加する方法は?

Visual Studio 2015 で新しいソリューションを作成する場合は、[新しいプロジェクト] ダイアログで [Application Insights の追加] オプションを選択します。 これは、すべてのターゲットとするアプリの種類から、同じ Application Insights リソースにテレメトリを送信します。

ユニバーサル アプリ ソリューションを既に作成している場合は、各プライマリ プロジェクトを右クリックし、**[Application Insights の追加]** を選択します。



## テレメトリの無効化

*テレメトリの収集を無効にする方法を教えてください。*

コード内で以下のように指定します。

    TelemetryConfiguration config = TelemetryConfiguration.Active;
    config.TrackingIsDisabled = true;

## ターゲット リソースの変更

*自分のプロジェクトがデータを送信する Application Insights のリソースを変更するにはどうすればいいですか?*

新しい Application Insights の概要ブレードで、[Essentials] を開き、インストルメンテーション キーをコピーします。

キーを ApplicationInsights.config ファイルに貼り付けます、 `< InstrumentationKey >` ノードです。

あるいは、実行時にターゲットを変更する場合、次を使用します。

     var telemetry = new TelemetryClient();
     telemetry.Context.InstrumentationKey = newKey;

## クライアント サーバー アプリケーションの監視方法は?

これを行うには 2 つの方法があります。

* クライアント用とサーバー用に (互いに異なるインストルメンテーション キーで) 2 つの Application Insights リソースを作成します。 ただし、これらのリソースは同じ Azure リソース グループ内に作成してください。 これにより、リソースの切り替えが簡単にできます。
* 1 つの Application Insights リソースを使用し、クライアントとサーバー両方のプロジェクトにそのインストルメンテーション キーを配置します。 その後、2 つのソースのメトリックスとイベントを関連付けることができます。

クライアントとサーバーのイベントを関連付けるために、要求ごとに操作 ID を生成します。 それをクライアントとサーバーの間で送信し、両方の端で利用統計情報に追加します。

    telemetry.Context.OperationId = opid;

## Azure のスタート画面

* 表示しています [Azure ポータル](http://portal.azure.com)します。 Does the map tell me something about my app?*

いいえ、そのマップは世界中の Azure サーバーの正常性を表しています。

*Azure のスタート画面 (ホーム画面) から、アプリに関するデータを見つける方法を教えてください。*

仮定 [Application insights の ][windows], 参照] をクリックして、Application Insights を選択して、アプリ用に作成したリソースを選択します。 次からその場所にすばやくアクセスできるように、スタート画面にリソースをピン留めすることができます。

## データの保持

* ポータルでのデータの保持する期間 Is it secure?*

参照してください [データの保持とプライバシー ][data]します。

## 次のステップ

*I set up Application Insights for my Java server app. その他は何ができますか *。

* [[可用性] web ページの可用性を監視します][availability]
* [[使用状況] ページの使用状況を監視します。][usage]
* [使用状況を追跡し、デバイス アプリの ][platforms]
* [アプリの ][track]
* [診断ログ ][javalogs]


## 問い合わせ

* [スタック オーバーフロー](http://stackoverflow.com/questions/tagged/ms-application-insights)




[availability]: app-insights-monitor-web-app-availability.md 
[data]: app-insights-data-retention-privacy.md 
[javalogs]: app-insights-java-trace-logs.md 
[platforms]: app-insights-platforms.md 
[track]: app-insights-api-custom-events-metrics.md 
[universal]: app-insights-windows-get-started.md#universal 
[usage]: app-insights-web-track-usage.md 
[windows]: app-insights-windows-get-started.md 

