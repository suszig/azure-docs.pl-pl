<properties
    pageTitle="Add Application Insights SDK to monitor your ASP.NET app | Microsoft Azure"
    description="オンプレミスまたは Microsoft Azure Web アプリケーションの使用状況、可用性、パフォーマンスを Application Insights で分析します。"
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/04/2015"
    ms.author="awills"/>


# Application Insights SDK を追加して ASP.NET アプリを監視する

*Application Insights はプレビュー段階です。*

[AZURE.INCLUDE [app-insights-selector-get-started](../../includes/app-insights-selector-get-started.md)]


Visual Studio Application Insights が監視するため、ライブ アプリケーション [を検出し、例外とパフォーマンスの問題を診断][detect], 、および [アプリの使用方法の把握][knowUsers]します。 Application Insights は、さまざまな種類のアプリケーションで使用できます。 Azure Web Apps に加えて、独自のオンプレミス IIS サーバーや Azure の仮想マシンでホストされているアプリに対しても機能します。



![パフォーマンス監視グラフのサンプル](./media/app-insights-start-monitoring-app-health-usage/10-perf.png)

*関連項目:*

* [ASP.NET 5](app-insights-asp-net-five.md)
* [デバイス アプリと Java サーバー][platforms]

#### 開始する前に

多くのアプリケーションの種類、 [Visual Studio によってアプリに Application Insights を追加](#ide) ほとんどの場合に通知されることがなく。 ただし、この記事を読んで状況を把握できるように、ここでは手動による手順について説明します。


必要なもの:

* サブスクリプションを [Microsoft Azure](http://azure.com)します。 チームまたは組織の Azure サブスクリプションの場合、所有者あなたを追加できますを使用して、 [Microsoft アカウント](http://live.com)します。
* Visual Studio 2013 以降

## <a name="add"></a> 1.Application Insights リソースの作成

サインイン、 [Azure ポータル][portal], 、新しい Application Insights リソースを作成します。 アプリケーションの種類として ASP.NET を選択します。

![[新規]、[Application Insights] の順にクリックする](./media/app-insights-start-monitoring-app-health-usage/01-new-asp.png)

A [リソース][roles] Azure では、サービスのインスタンス。 このリソースでは、アプリのテレメトリが分析されて画面に表示されます。

リソース ブレードと、プロパティの既定のコンテンツを設定に表示されるアプリケーションの種類の選択 [メトリックス エクスプ ローラー][metrics]します。

#### インストルメンテーション キーのコピー

これはリソースを識別するキーです。データをリソースに送信するために SDK の後の手順でインストールします。

![[プロパティ] をクリックし、キーを選択して、Ctrl キーを押しながら C キーを押す](./media/app-insights-start-monitoring-app-health-usage/02-props-asp.png)

新しいリソースを作成するために実行した手順は、任意のアプリケーションの監視を開始するための優れた方法です。 これで、データをリソースに送信できます。

## <a name="sdk"></a> 2.アプリケーションに SDK をインストールする

Application Insights SDK のインストールと構成は、作業中のプラットフォームによって異なります。 ASP.NET アプリの場合は簡単です。

1. Visual Studio で、Web アプリ プロジェクトの NuGet パッケージを編集します。

    ![プロジェクトを右クリックし、[Nuget パッケージの管理] を選択する](./media/app-insights-start-monitoring-app-health-usage/03-nuget.png)

2. Web Apps 向け Application Insights SDK をインストールします。

    !["Application Insights" の検索](./media/app-insights-start-monitoring-app-health-usage/04-ai-nuget.png)

3. (NuGet のインストールによって追加された) ApplicationInsights.config を編集します。 次のコードを終了タグの直前に挿入します。

    `<InstrumentationKey>` *コピーしたインストルメンテーション キー* `</InstrumentationKey>`

    (または、 [コードを記述してキーを設定][apikey] アプリです)。

#### 新しいバージョンの SDK にアップグレードするには

SDK の新しいバージョンは不定期でリリースされます。

アップグレードする、 [SDK の新しいリリース](app-insights-release-notes-dotnet.md), 、NuGet パッケージ マネージャーをもう一度開いて、インストールされているパッケージでフィルター処理します。 Select **Microsoft.ApplicationInsights.Web** and choose **Upgrade**.

ApplicationInsights.config をカスタマイズしている場合は、アップグレードする前にコピーを保存しておき、後から新しいバージョンに変更をマージします。


## <a name="run"></a> 3.プロジェクトの実行

使用して、 **f5 キーを押して** をアプリケーションを実行し、試してみる: をいくつかのテレメトリを生成するさまざまなページを開きます。

Visual Studio で、送信されたイベント数が表示されます。

![](./media/app-insights-start-monitoring-app-health-usage/appinsights-09eventcount.png)

## <a name="monitor"></a> 4.テレメトリの表示

戻り、 [Azure ポータル][portal] Application Insights リソースを参照します。


概要グラフでデータを探します。 最初、1 つまたは 2 つのポイントだけが表示されます。 次に例を示します。

![クリックしてより多くのデータを表示する](./media/app-insights-start-monitoring-app-health-usage/12-first-perf.png)

任意のグラフをクリックして、より詳細なメトリックを表示します。 [メトリックの詳細についてはこちらをご覧ください。][perf]

#### データが表示されない場合

* アプリケーションを使用して、テレメトリがいくつか生成されるようにさまざまなページを開きます。
* 開いている、 [検索][diagnostic] タイル、個々 のイベントを表示します。 メトリック パイプラインを経由すると、イベントの取得に少し時間がかかる場合があります。
* Wait a few seconds and click **Refresh**. グラフは周期的に自動で更新されますが、データの表示を待機している場合、手動で更新することもできます。
* 参照してください [トラブルシューティング][qna]します。

## アプリケーションの発行

ここで、アプリケーションを IIS または Azure にデプロイし、データ累積を確認します。

![Visual Studio を使用してアプリを発行する](./media/app-insights-start-monitoring-app-health-usage/15-publish.png)

デバッグ モードで実行している場合、テレメトリはパイプラインにより時間が短縮されるので、数秒でデータが表示されます。 リリース構成でアプリケーションをデプロイすると、データ累積速度は遅くなります。

#### サーバーに発行した後でデータはありませんか。

サーバーのファイアウォールで発信トラフィック用のこれらのポートを開きます。

+ `dc.services.visualstudio.com:443`
+ `f5.services.visualstudio.com:443`


#### ビルド サーバーで問題が発生した場合

参照してください [このトラブルシューティング項目](app-insights-troubleshoot-faq.md#NuGetBuild)します。

> [AZURE.NOTE] アプリはさまざまな利用統計情報を生成する場合 (ASP.NET の SDK バージョン 2.0.0-beta3 を使用している、またはそれ以降)、アダプティブ サンプリング モジュールはイベントの代表的な分数のみを送信することによって、ポータルに送信されるボリュームを自動的に削減します。 ただし、同じ要求に関連するイベントはグループ単位で選択または選択解除されるので、関連するイベントごとに操作できます。 
> [サンプリングについて](app-insights-sampling.md)します。


## 5.依存関係の追跡 (および IIS パフォーマンス カウンター) の追加

SDK によるデータへのアクセスでは、若干のサポートが必要です。 具体的には、アプリからデータベース、REST API、またその他の外部コンポーネントへの呼び出しを自動的に測定するには、次の追加のステップが必要です。 依存関係のメトリックは、パフォーマンスに関する問題の診断に非常に役立つ場合があります。

IIS サーバーで実行した場合は、この手順によって、システムで表示するパフォーマンス カウンター [メトリックス エクスプ ローラー](app-insights-metrics-explorer.md)します。

#### アプリが IIS サーバーで実行される場合

管理者権限を使用してサーバーにサインインし、インストール [Application Insights Status Monitor](http://go.microsoft.com/fwlink/?LinkId=506648)します。

必要に応じて [ファイアウォールでは、追加の送信ポートを開く](app-insights-monitor-performance-live-website-now.md#troubleshooting)します。

この手順ではまた、 [パフォーマンス カウンタのレポートを](app-insights-web-monitor-performance.md#system-performance-counters) など、CPU、メモリ、ネットワークの占有率。

#### アプリが Azure の Web アプリの場合

Azure の Web アプリのコントロール パネルで、Application Insights 拡張機能を追加します。

![Web アプリで、[設定]、[拡張機能]、[追加]、[Application Insights] の順に選択する](./media/app-insights-start-monitoring-app-health-usage/05-extend.png)


#### Azure Cloud Services プロジェクトの場合

[スクリプトを web ロールとワーカー ロールに追加](app-insights-cloudservices.md)します。



## 6.クライアント側の監視を追加します。

アプリケーションのサーバー側 (バックエンド) からテレメトリ データを送信する SDK を既にインストールしています。 このため、クライアント側の監視を追加することができます。 これにより、ユーザー、セッション、ページ ビュー、およびブラウザーで発生する例外やクラッシュに関するデータを入手できます。 また、独自のコードを記述して、ユーザーのアプリの操作をクリックやキーボード操作までの細部にわたって追跡できます。


すべてのページに JavaScript のスニペットを追加します。 コードは次に示す Application Insights のリソースから取得できます。

![Web アプリでクイック スタートを開き、[Web ページを監視するコードを取得する] をクリックする](./media/app-insights-start-monitoring-app-health-usage/02-monitor-web-page.png)

コードにはアプリケーション リソースを識別するインストルメンテーション キーが含まれています。

[Web ページの追跡についてはこちらをご覧ください。](app-insights-web-track-usage.md)


## アプリケーションのバージョンを追跡する

MSBuild プロセスで `buildinfo.config` が生成されていることを確認します。 .csproj ファイルに、次のコードを追加します。  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup> 
```

ビルド情報がある場合、Application Insights web モジュールは自動的に追加 **アプリケーション バージョン** テレメトリのすべてのアイテムにプロパティとして。 バージョンで実行するときにフィルターを適用することができる [診断検索の結果][diagnostic] または [メトリックを確認する][metrics]です。 

ただし、Visual Studio で開発者向けのビルドではなく、MS ビルドでのみビルド バージョン番号が生成されることに注意してください。

## 7.インストールを完了する

アプリケーションを 360 度から表示するために実行できるいくつかの作業を次に示します。

* [Web テストを設定][availability] を確認する、アプリケーションが動作していて応答します。
* [ログ トレースをキャプチャ][netlogs] お気に入りのログ記録フレームワークから
* [カスタム イベントおよびメトリックスの追跡][api] では、クライアントまたはサーバーまたはその両方をアプリケーションの使用方法の詳細を参照してください。

## <a name="ide"></a> 自動化された方法

この記事の冒頭では、Application Insights のリソースの作成と SDK のインストールを手動で実行する方法を示しました。 その手順を構成する 2 つの部分を理解しておくことは良いことであると信じています。 ただし、ASP.NET アプリ (とその他の多くのアプリ) には、もっとすばやく実行できる自動化された方法があります。

必要があります [Visual Studio](http://go.microsoft.com/fwlink/?linkid=397827&clcid=0x409) (2013 update 3 またはそれ以降) とアカウント [Microsoft Azure](http://azure.com)します。

#### 新しいプロジェクトの場合

Visual Studio で新しいプロジェクトを作成するときに必ず **Application Insights の追加** が選択されています。


![ASP.NET プロジェクトを作成する](./media/app-insights-start-monitoring-app-health-usage/appinsights-01-vsnewp1.png)

Visual Studio によって、Application Insights にリソースが作成され、プロジェクトに SDK が追加されて、キーが `.config` ファイルに配置されます。

かどうかは、プロジェクトが web ページに、さらに、 [JavaScript SDK][client] がマスター web ページです。

#### 既存のプロジェクトの場合

ソリューション エクスプ ローラーでプロジェクトを右クリックし [ **Application Insights の追加**します。

![[Application Insights の追加] を選択する](./media/app-insights-start-monitoring-app-health-usage/appinsights-03-addExisting.png)

Visual Studio によって、Application Insights にリソースが作成され、プロジェクトに SDK が追加されて、キーが `.config` ファイルに配置されます。

この場合、追加されません、 [JavaScript SDK][client] web ページにお勧めする次の手順として。

#### セットアップ オプション

初めての場合には、Microsoft Azure プレビュー版にログインまたはサインインするように求められます 

このアプリがより大きなアプリケーションの一部である場合は、使用する場合があります **設定を構成** 、他のコンポーネントと同じリソース グループに配置します。

*Application Insights オプションはありせんか。 Visual Studio 2013 Update 3 以降を使用しているし、拡張機能と更新プログラムでその Application Insights Tools が有効になっていることを確認します。*

#### プロジェクトから Application Insights を開く

![プロジェクトを右クリックして Azure ポータルを開く](./media/app-insights-start-monitoring-app-health-usage/appinsights-04-openPortal.png)




## <a name="video"></a>ビデオ

> [AZURE.VIDEO getting-started-with-application-insights]


<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apikey]: app-insights-api-custom-events-metrics.md#ikey
[availability]: app-insights-monitor-web-app-availability.md
[azure]: ../insights-perf-analytics.md
[client]: app-insights-javascript.md
[detect]: app-insights-detect-triage-diagnose.md
[diagnostic]: app-insights-diagnostic-search.md
[knowUsers]: app-insights-overview-usage.md
[metrics]: app-insights-metrics-explorer.md
[netlogs]: app-insights-asp-net-trace-logs.md
[perf]: app-insights-web-monitor-performance.md
[platforms]: app-insights-platforms.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md


