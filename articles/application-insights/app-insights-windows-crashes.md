<properties 
    pageTitle="Detect and diagnose crashes in Windows Store and Phone apps with Application Insights" 
    description="Application Insights を使用して Windows デバイス アプリのパフォーマンスの問題を分析します。" 
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
    ms.date="11/21/2015" 
    ms.author="awills"/>


# Application Insights による Windows ストア アプリと Windows Phone アプリでのクラッシュの検出と診断

*Application Insights はプレビュー段階です。*

自分のアプリでユーザーがクラッシュに遭遇した場合は、それをすばやく把握し、何が起きたのかを詳しく知る必要があります。 Application Insights を使ってクラッシュ発生頻度、get とアラートを生成し、各インシデントのレポートの調査を監視できます。

"クラッシュ" とは、キャッチされない例外に起因してアプリケーションが終了することを意味します。 それをレポートできるアプリが例外をキャッチする場合、 [TrackException API ][apiexceptions] 動作は継続します。 その場合、クラッシュとしてはログに記録されません。


## クラッシュ発生頻度の監視

これは既に完了していない、追加 [アプリに Application Insights をプロジェクトの ][windows], を再発行します。

アプリケーションの概要ブレードで番組がクラッシュした [Application Insights ポータルの ][portal]します。

![](./media/app-insights-windows-crashes/appinsights-d018-oview.png)

表示されている時間範囲はグラフごとに編集できます。


## クラッシュを検出するアラートの設定

![クラッシュのグラフから、](./media/app-insights-windows-crashes/appinsights-d023-alert.png)

## クラッシュの診断

アプリの一部のバージョンが他より頻繁にクラッシュしているかどうかを確認するには、次のようにクラッシュのグラフをクリックして進み、アプリケーションのバージョン別に分割します。

![](./media/app-insights-windows-crashes/appinsights-d26crashSegment.png)


クラッシュを引き起こしている例外を検索するには、[診断検索] を開きます。 例外に的を絞るには、次のように他のテレメトリの種類を削除することをお勧めします。

![](./media/app-insights-windows-crashes/appinsights-d26crashExceptions.png)

[診断検索の ][diagnostic]します。


関連するプロパティやスタック トレースなどの詳細を表示するには、いずれかの例外をクリックします。

![](./media/app-insights-windows-crashes/appinsights-d26crash.png)

次のように、発生日時がその例外に近い他の例外およびイベントを表示します。


![](./media/app-insights-windows-crashes/appinsights-d26crashRelated.png)

## トレース ログとイベントの挿入

問題の診断を支援するには [トレース呼び出しを挿入して、Application Insights の ][diagnostic]します。

## <a name="debug"></a>デバッグ モードとリリース モード

#### デバッグ

デバッグ モードでビルドする場合、イベントは生成されるとすぐに送信されます。 インターネット接続が切断され、接続が復旧する前にアプリを終了した場合、オフラインのテレメトリは破棄されます。

#### リリース

リリース構成でビルドする場合、イベントはデバイスに格納され、アプリケーション再開時に送信されます。 データは、アプリケーションの初回使用時にも送信されます。 起動時にインターネット接続されていない場合は、前のテレメトリだけでなく、現在のライフサイクルのテレメトリも格納され、次の再開時に送信されます。

## <a name="next"></a>次のステップ

[検出、トリアージ、および Application Insights による問題を診断する ][detect]

[Application Insights API ][api]

[[トレース] の診断ログをキャプチャします。][trace]

[トラブルシューティング](app-insights-windows-troubleshoot.md)







[api]: app-insights-api-custom-events-metrics.md 
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception 
[detect]: app-insights-detect-triage-diagnose.md 
[diagnostic]: app-insights-diagnostic-search.md 
[platforms]: app-insights-platforms.md 
[portal]: http://portal.azure.com/ 
[trace]: app-insights-search-diagnostic-logs.md 
[windows]: app-insights-windows-get-started.md 

