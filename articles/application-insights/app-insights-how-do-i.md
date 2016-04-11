<properties 
    pageTitle="Application Insights での作業" 
    description="Application Insights での FAQ。" 
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
    ms.date="10/11/2015" 
    ms.author="awills"/>

# Application Insights での作業

## 電子メールの受信

### サイトがダウンした場合の電子メール

設定、 [可用性 web テスト](app-insights-monitor-web-app-availability.md)します。

### サイトが過負荷になっている場合の電子メール

設定、 [アラート](app-insights-alerts.md) に **サーバーの応答時間**します。 1 ～ 2 秒の間のしきい値で機能する必要があります。

![](./media/app-insights-how-do-i/030-server.png)

アプリがエラー コードを返すことによって負荷の兆候を示す場合もあります。 警告を設定 **失敗した要求を**します。

警告を設定する場合は、 **サーバー例外**, 、実行する必要があります [追加のセットアップ](app-insights-asp-net-exceptions.md) データを表示するためにします。

### 例外での電子メール

1. [例外の監視を設定します](app-insights-asp-net-exceptions.md)
2. [アラート設定](app-insights-alert.md) 例外に関するメトリックをカウントします


### アプリのイベントでの電子メール

特定のイベントが発生したときに電子メールを受け取りたいものとします。 Application Insights は、直接この機能を提供しませんが、 [メトリックがしきい値を超えたときにアラートを送信](app-insights-alerts.md)します。 

アラートを設定できます [カスタム メトリック](app-insights-api-custom-events-metrics.md#track-metric), がないカスタム イベント、します。 イベントが発生したときにメトリックを増やすコードを記述します。

    telemetry.TrackMetric("Alarm", 10);

または:

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

アラートには 2 つの状態があるため、アラートを終了するときは低い値を送信する必要があります。

    telemetry.TrackMetric("Alarm", 0.5);

グラフを作成する [メトリックス エクスプ ローラー](app-insights-metric-explorer.md) アラームの設定を表示します。

![](./media/app-insights-how-do-i/010-alarm.png)

メトリックが短時間中間値を超えたら発生するようにアラートを設定します。


![](./media/app-insights-how-do-i/020-threshold.png)

平均計算期間を最小に設定します。 

メトリックがしきい値を上回ったときと下回ったときの両方で、電子メールを受け取ります。

考慮すべき点:

* アラートには、"警告" と "正常" の 2 つの状態があります。 状態はメトリックを受信した場合にのみ評価されます。
* 電子メールは状態が変化したときにのみ送信されます。 これは、高い値と低い値の両方のメトリックを送信する必要がある理由です。 
* アラートを評価するため、前の期間に受け取った値の平均が計算されます。 これはメトリックを受信するたびに行われるので、設定した期間より頻繁に電子メールが送信される可能性があります。
* 電子メールは "警告" と "正常" の両方で送信されるので、1 回限りのイベントを 2 つの状態として考え直すことができます。 たとえば、"ジョブ完了" イベントの代わりに、"ジョブ進行中" という状態を考え、その場合はジョブの開始時と終了時に電子メールを受け取ります。

### アラートの自動設定

[Use PowerShell to create new alerts (PowerShell を使用した新しいアラートの作成)](app-insights-alerts/#set-alerts-by-using-powershell)

## PowerShell を使用した Application Insights の管理

* [新しいリソースの作成に関するページ](app-insights-powershell-script-create-resource.md)
* [新しいアラートの作成に関するページ](app-insights-alerts/#set-alerts-by-using-powershell)

## アプリケーションのバージョンとスタンプ

### 開発、テスト、および運用環境の結果を区分する

* さまざまな環境に、個別の iKey を設定します。
* さまざまなスタンプ (開発、テスト、運用) のタグに、個別のプロパティ値を持つテレメトリを設定します。

[詳細情報](app-insights-separate-resources.md)
 

### ビルド番号でのフィルター処理

新しいバージョンのアプリを発行するときは、異なるビルドのテレメトリを区別する必要があります。

アプリケーションのバージョンのプロパティを設定するには、フィルター処理できるように [検索](app-insights-diagnostic-search.md) と [メトリックス エクスプ ローラー](app-insights-metrics-explorer.md) 結果。 


![](./media/app-insights-how-do-i/050-filter.png)

アプリケーション バージョン プロパティを設定するには複数の方法があります。

* 直接設定します。

    `telemetryClient.Context.Component.Version = typeof(MyProject.MyClass).Assembly.GetName().Version;`

* 行を折り返す、 [テレメトリ初期化子](app-insights-api-custom-events-metrics.md#telemetry-initializers) TelemetryClient のすべてのインスタンスが常に設定されていることを確認します。

* [ASP.NET] `BuildInfo.config` でバージョンを設定します。 Web モジュールは BuildLabel ノードからバージョンを取得します。 このファイルをプロジェクトに追加し、ソリューション エクスプローラーで [常にコピーする] プロパティを設定します。

    ```XML

    <?xml version="1.0" encoding="utf-8"?>
    <DeploymentEvent xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/VisualStudio/DeploymentEvent/2013/06">
      <ProjectName>AppVersionExpt</ProjectName>
      <Build type="MSBuild">
        <MSBuild>
          <BuildLabel kind="label">1.0.0.2</BuildLabel>
        </MSBuild>
      </Build>
    </DeploymentEvent>

    ```
* [ASP.NET] MSBuild で自動的に BuildInfo.config を生成します。 そのためには、.csproj ファイルに数行を追加します。

    ```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup> 
    ```

    これにより、という名前のファイルが生成されます。 *yourProjectName*します。BuildInfo.config します。 これは発行プロセスで BuildInfo.config という名前に変更されます。

    Visual Studio でビルドすると、ビルド ラベルにはプレースホルダー (AutoGen_...) が含まれます。 一方、MSBuild でビルドすると、適切なバージョン番号が設定されます。

    MSBuild がバージョン番号を生成できるようにするには、AssemblyReference.cs で `1.0.*` のようなバージョンを設定します。

## バックエンド サーバーとデスクトップ アプリを監視する

[Windows Server SDK モジュールを使用して](app-insights-windows-desktop.md)します。


## データの視覚化

#### 複数のアプリケーションのメトリックを使用したダッシュボード

*  [メトリック エクスプ ローラー](app-insights-metrics-explorer.md), 、グラフをカスタマイズおよびをお気に入りとして保存します。 Azure ダッシュボードにピン留めします。
* 

#### 他のソースや Application Insights からのデータのあるダッシュボード

* [Power BI にテレメトリをエクスポート](app-insights-export-power-bi.md)します。 

または

* SharePoint をダッシュボードとして使用して、SharePoint Web パーツにデータを表示します。 [連続エクスポートし、Stream Analytics を使用して SQL にエクスポート](app-insights-code-sample-export-sql-stream-analytics.md)します。  PowerView を使用してデータベースを確認し、PowerView の SharePoint Web パーツを作成します。


### 複雑なフィルター処理、セグメント化、結合

* [連続エクスポートし、Stream Analytics を使用して SQL にエクスポート](app-insights-code-sample-export-sql-stream-analytics.md)します。  PowerView を使用してデータベースを確認します。

<a name="search-specific-users"></a>
### 匿名ユーザーまたは認証済みユーザーのフィルター処理

ユーザーがサインインすると場合、は、設定、 [ユーザー id を認証](app-insights-api-custom-events-metrics.md#authenticated-users)します。 (自動的には設定されません。) 

次に以下のことを行えます。

* 特定のユーザー ID で検索

![](./media/app-insights-how-do-i/110-search.png)

* 匿名ユーザーまたは認証済みユーザーに対するメトリックのフィルター処理

![](./media/app-insights-how-do-i/115-metrics.png)

## プロパティ名または値を変更する

作成、 [フィルター](app-insights-api-filtering-sampling.md#filtering)します。 これにより、アプリから Application Insights にテレメトリが送信される前に、テレメトリの変更またはフィルター処理ができるようになります。

## 特定のユーザーとその使用状況を一覧表示する

たい場合 [特定のユーザーを検索する](#search-specific-users), を設定することができます、 [ユーザー id を認証](app-insights-api-custom-events-metrics.md#authenticated-users)します。

ユーザーが表示するページやログインの頻度についてユーザーを一覧表示するには、2 つのオプションがあります。

* [認証されたユーザー id](app-insights-api-custom-events-metrics.md#authenticated-users), 、[データベースにエクスポートする](app-insights-code-sample-export-sql-stream-analytics.md) および適切なツールがあるユーザー データの分析を使用します。
* ユーザー数が少ない場合は、メトリック値またはイベント名として関心のあるデータを使用し、ユーザー ID をプロパティとして設定して、カスタム イベントまたはメトリックを送信します。 ページ ビューを分析するには、標準の JavaScript trackPageView 呼び出しを置き換えます。 サーバー側のテレメトリを分析するには、テレメトリ初期化子を使用して、ユーザー ID をすべてのサーバー テレメトリに追加します。 次に、ユーザー ID を基に、メトリックや検索結果をフィルター処理および細分化します。


## アプリから Application Insights へのトラフィックの削減

*  [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), 、すべてのモジュールがなくても、このようなパフォーマンス カウンター コレクターを無効にします。
* 使用 [サンプリングおよびフィルタ リング](app-insights-api-filtering-sampling.md) SDK にします。
* 使用している場合は、 [TrackMetric](app-insights-api-custom-events-metrics.md#track-metric), 、結果を送信する前にバッチのメトリック値の集計を計算します。 これに対して提供される TrackMetric() のオーバーロードを考慮します。


詳細について [価格とクォータ](app-insights-pricing.md)します。

## 遠隔測定を無効にする

 **を動的に停止および開始** 収集と、サーバーからのテレメトリの送信。

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



 **選択されている標準のコレクターを無効にする** - たとえば、パフォーマンス カウンター、HTTP 要求や依存関係 - 削除するかに関連する行をコメント アウト [ApplicationInsights.config](app-insights-api-custom-events-metrics.md)します。 たとえば、独自の TrackRequest データを送信する場合にこれを行います。



## システム パフォーマンス カウンターの表示

メトリックス エクスプローラーに表示できるメトリックには、一連のシステム パフォーマンス カウンターがあります。 「定義済みブレードがある **サーバー** いくつかの方法を表示します。

![Application Insights リソースを開いて、[サーバー] をクリック](./media/app-insights-how-do-i/121-servers.png)

### パフォーマンス カウンターのデータが表示されない場合

* **IIS サーバー** 独自マシンまたはバーチャル マシン上にします。 [Status Monitor をインストール](app-insights-monitor-performance-live-website-now.md)します。 
* **Azure の web サイト** -パフォーマンス カウンターはまだサポートされていません。 Azure Web サイトのコントロール パネルの標準パーツとして取得できるメトリックがいくつか用意されています。
* **Unix サーバー** - [collectd をインストール](app-insights-java-collectd.md)

### 表示するパフォーマンス カウンターの数を増やすには

* まず、 [新しいグラフを追加](app-insights-metrics-explorer.md) カウンターが、基本的なセットが用意されているかどうかとします。
* ない場合は、 [パフォーマンス カウンター モジュールによって収集されたセットにカウンターを追加して](app-insights-web-monitor-performance.md#system-performance-counters)します。



