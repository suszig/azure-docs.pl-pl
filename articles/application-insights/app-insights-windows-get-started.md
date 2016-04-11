<properties
    pageTitle="Windows Phone アプリとストア アプリの分析 | Microsoft Azure"
    description="Windows デバイス アプリの使用状況とパフォーマンスを分析します。"
    services="application-insights"
    documentationCenter="windows"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/21/2015"
    ms.author="awills"/>

# Windows Phone アプリとストア アプリの分析

マイクロソフトは、デバイスの devOps 用の 2 つのソリューションを提供しています: [HockeyApp](http://hockeyapp.net/) devOps ワークフローとクラッシュ分析用および [Application Insights](app-insights-overview.md) 使用状況とクラッシュ分析用です。

[HockeyApp](http://hockeyapp.net/) ios では、モバイル DevOps ソリューションは、ベース Xamarin や Cordova、Unity のクロス プラットフォーム アプリだけでなく、OS X、Android、Windows のデバイス アプリです。 これを使用すると、ベータ テスト担当者にビルドを配布し、クラッシュ データを収集して、ユーザーからのフィードバックを得ることができます。 このソリューションは Visual Studio Team Services と統合されており、ビルドのデプロイメントや作業項目の統合を簡単に行うことができます。 詳細については、 [HockeyApp サポート技術情報](http://support.hockeyapp.net/kb) で最新の状態と、 [HockeyApp ブログ](http://hockeyapp.net/blog/)します。

アプリのサーバー側の場合は使用して [Application Insights](app-insights-overview.md) をアプリの web サーバー側のモニターに [ASP.NET](app-insights-asp-net.md) または [J2EE](app-insights-java-get-started.md)します。 同じ Application Insights リソースにテレメトリを送信して、クライアント側とサーバー側のイベントを関連付けることができます。

 [C++ ユニバーサル アプリ用の Application Insights SDK](https://github.com/Microsoft/ApplicationInsights-CPP) Application Insights ポータルにテレメトリを送信します。

Visual Studio Application Insights を使用すると、発行されたアプリケーションの次の内容を監視できます。

* [**使用状況**][windowsUsage] - ユーザーの数やユーザーが行っているアプリの使用。
* [**クラッシュ**][windowsCrash] - クラッシュの診断レポートを取得し、ユーザーへの影響を理解します。

![](./media/app-insights-windows-get-started/appinsights-d018-oview.png)

多くのアプリケーションの種類、 [Visual Studio によってアプリに Application Insights を追加](#ide) ほとんどの場合に通知されることがなく。 ただし、この記事を読んで状況を把握できるように、ここでは手動による手順について説明します。

必要なものは次のとおりです。

* サブスクリプションを [Microsoft Azure][azure]します。
* Visual Studio 2013 以降

## 1.Application Insights リソースの作成 

 [Azure ポータル][portal], 、新しい Application Insights リソースを作成します。

![[新規]、[開発者向けサービス]、[Application Insights] の順に選択する](./media/app-insights-windows-get-started/01-new.png)

A [リソース][roles] Azure では、サービスのインスタンス。 このリソースでは、アプリのテレメトリが分析されて画面に表示されます。

#### インストルメンテーション キーのコピー

このキーでリソースが識別されます。 リソースへデータを送信ように SDK を構成するには、このキーがすぐに必要になります。

![[要点] ボックスのドロワを開き、インストルメンテーション キーを選択する](./media/app-insights-windows-get-started/02-props.png)


## 2.アプリへの Application Insights SDK の追加

Visual Studio で、適切な SDK をプロジェクトに追加します。

If it's a Windows Universal app, repeat the steps for both the Windows Phone project and the Windows project.

1. Right-click the project in Solution Explorer and choose **Manage NuGet Packages**.

    ![](./media/app-insights-windows-get-started/03-nuget.png)

2. 「Application Insights」を検索します。

    ![](./media/app-insights-windows-get-started/04-ai-nuget.png)

3. 選択 **Windows アプリケーション向けの Application Insights**

4. プロジェクトのルートに ApplicationInsights.config ファイルを追加し、ポータルでコピーしたインストルメンテーション キーを挿入します。 この構成ファイルのサンプル xml を次に示します。 

    ```xml
        <?xml version="1.0" encoding="utf-8" ?>
        <ApplicationInsights>
            <InstrumentationKey>YOUR COPIED INSTRUMENTATION KEY</InstrumentationKey>
        </ApplicationInsights>
    ```

    ApplicationInsights.config ファイルのプロパティを設定します。 **ビルド アクション** = = **コンテンツ** と **出力ディレクトリにコピー** = = **常にコピー**します。
    
    ![](./media/app-insights-windows-get-started/AIConfigFileSettings.png)

5. 次の初期化コードを追加します。 このコードを `App()` コンストラクターに追加することをお勧めします。 他の場所にコピーすると、最初の PageViews のコレクションが失われることがあります。  

```C#
    public App()
    {
       // Add this initilization line. 
       WindowsAppInitializer.InitializeAsync();
    
       this.InitializeComponent();
       this.Suspending += OnSuspending;
    }  
```

**Windows Universal apps**: Repeat the steps for both the Phone and the Store project. [Windows 8.1 ユニバーサル アプリの例は](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/Windows%208.1%20Universal)です。

## <a name="network"></a>3.アプリのネットワーク アクセスの有効化

アプリがいない場合 [発信ネットワーク アクセスを要求する](https://msdn.microsoft.com/library/windows/apps/hh452752.aspx), としてそのマニフェストに追加する必要があります、 [機能に必要な](https://msdn.microsoft.com/library/windows/apps/br211477.aspx)です。

## <a name="run"></a>4.プロジェクトの実行

[F5 キーを押してアプリケーションを実行](http://msdn.microsoft.com/library/windows/apps/bg161304.aspx) し、これを使用して、テレメトリを生成します。 

Visual Studio で、受け取ったイベント数を確認できます。

![](./media/app-insights-windows-get-started/appinsights-09eventcount.png)

デバッグ モードでは、テレメトリは生成されるとすぐに送信されます。 リリース モードでは、テレメトリはデバイスに格納され、アプリケーションの再開時にのみ送信されます。


## <a name="monitor"></a>5.監視データの表示

プロジェクトから Application Insights を開きます。

![プロジェクトを右クリックして Azure ポータルを開く](./media/app-insights-windows-get-started/appinsights-04-openPortal.png)


最初、1 つまたは 2 つのポイントだけが表示されます。 次に例を示します。

![クリックしてより多くのデータを表示する](./media/app-insights-windows-get-started/appinsights-26-devices-01.png)

大量のデータが予想される場合は、数秒後に [最新の情報に更新] をクリックします。

いずれかのグラフをクリックして、詳細を表示します。 


## <a name="deploy"></a>5.ストアへのアプリケーションの発行

[アプリケーションの発行](http://dev.windows.com/publish) とデータが累積されるは、ユーザーがダウンロードし、使用を確認します。

## テレメトリのカスタマイズ

#### コレクターの選択

Application Insights SDK には、さまざまな種類のデータをアプリから自動的に収集する、複数のコレクターが用意されています。 既定では、これらはすべてアクティブになります。 ただし、アプリ コンストラクターでどのコレクターを初期化するかを選択することができます。

    WindowsAppInitializer.InitializeAsync( "00000000-0000-0000-0000-000000000000",
       WindowsCollectors.Metadata
       | WindowsCollectors.PageView
       | WindowsCollectors.Session 
       | WindowsCollectors.UnhandledException);

#### 独自のテレメトリ データを送信する

使用して、 [API][api] イベント、メトリック、および診断データを Application Insights に送信します。 概要:

```C#

 var tc = new TelemetryClient(); // Call once per thread

 // Send a user action or goal:
 tc.TrackEvent("Win Game");

 // Send a metric:
 tc.TrackMetric("Queue Length", q.Length);

 // Provide properties by which you can filter events:
 var properties = new Dictionary{"game", game.Name};

 // Provide metrics associated with an event:
 var measurements = new Dictionary{"score", game.score};

 tc.TrackEvent("Win Game", properties, measurements);

```

詳細については、次を参照してください。 [カスタム イベントおよびメトリック][api]します。

## 次の手順

* [アプリのクラッシュの検出と診断][windowsCrash]
* [メトリックの詳細][metrics]
* [診断検索の詳細][diagnostic]


## <a name="ide"></a>自動セットアップ

セットアップ手順を Visual Studio で自動実行する場合は、Windows Phone、Windows ストアなどのさまざまな種類のアプリで自動実行できます。

###<a name="new"></a>新しい Windows アプリ プロジェクトを作成する場合

[新しいプロジェクト] ダイアログ ボックスで [Application Insights] を選択します。 

サインインを求めるメッセージが表示されたら、Azure アカウント (Visual Studio Team Services アカウントとは異なります) の資格情報を使用します。

![](./media/app-insights-windows-get-started/appinsights-d21-new.png)


###<a name="existing"></a> 既存のプロジェクトの場合

ソリューション エクスプローラーから Application Insights を追加します。


![](./media/app-insights-windows-get-started/appinsights-d22-add.png)

## SDK の新しいリリースにアップグレードするには

ときに、 [新しいバージョンの SDK がリリースされた](app-insights-release-notes-windows.md):
* プロジェクトを右クリックし、[NuGet パッケージの管理] を選択します。 
* インストール済みの Application Insights パッケージを選択し、[アップグレード] アクションを選択します。


## <a name="usage"></a>次のステップ


[アプリのクラッシュの検出と診断][windowsCrash]

[診断ログのキャプチャと検索][diagnostic]


[アプリの使用状況の追跡][windowsUsage]

[API を使用したカスタム テレメトリの送信][api]

[トラブルシューティング][qna]



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[azure]: ../insights-perf-analytics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md
[windowsCrash]: app-insights-windows-crashes.md
[windowsUsage]: app-insights-windows-usage.md


