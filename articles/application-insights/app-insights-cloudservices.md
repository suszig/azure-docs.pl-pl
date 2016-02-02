<properties
   pageTitle="Azure Cloud Services 向けの Application Insights"
   description="Application Insights で Web と worker ロールを効果的に監視する"
   services="application-insights"
   documentationCenter=""
   authors="soubhagyadash"
   manager="douge"
   editor="alancameronwills"/>

<tags
   ms.service="application-insights"
   ms.devlang="na"
   ms.tgt_pltfrm="ibiza"
   ms.topic="article"
   ms.workload="tbd"
   ms.date="11/15/2015"
   ms.author="sdash"/>


# Azure Cloud Services 向けの Application Insights

*Application Insights はプレビュー段階です。*

[Microsoft Azure Cloud サービス アプリ](http://azure.microsoft.com/services/cloud-services/) で監視できます。 [Visual Studio Application Insights ][start] 可用性、パフォーマンス、障害および使用します。 アプリのパフォーマンスと効果に関するフィードバックが得られたら、各開発ライフサイクルにおける設計の方向性について、情報に基づいて選択できます。

![例](./media/app-insights-cloudservices/sample.png)

サブスクリプションが必要 [Microsoft Azure](http://azure.com)します。 Windows、XBox Live、またはその他の Microsoft クラウド サービスに与えられる Microsoft アカウントでサインインします。


#### Application Insights を使用してインストルメント化されたサンプル アプリケーション

これを見て [サンプル アプリケーション](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) Application Insights が Azure でホストされる 2 つの worker ロールで、クラウド サービスに追加されします。

以降では、同じようにして独自のクラウド サービス プロジェクトを調整する方法について説明します。

## 役割ごとに Application Insights リソースを作成する

Application Insights リソースでは、利用統計情報データが分析され、表示されます。

1.  [Azure ポータルの ][portal], 、新しい Application Insights リソースを作成します。 アプリケーションの種類として ASP.NET アプリを選択します。

    ![[新規]、](./media/app-insights-cloudservices/01-new.png)

2.  インストルメンテーション キーをコピーします。 これはじきに SDK を構成するために必要になります。

    ![[プロパティ] をクリックし、キーを選択して、Ctrl キーを押しながら C キーを押す](./media/app-insights-cloudservices/02-props.png)


通常、それぞれの Web と worker ロールからのデータに対して個別のリソースを作成するのが最良の方法になります。

代わりに、すべてのロールから 1 つのリソースにデータを送信が設定する可能性があります、 [既定のプロパティ ][apidefaults] をフィルター処理または各ロールからの結果をグループ化できるようにします。

## <a name="sdk"></a>各プロジェクトに SDK をインストールします。

1. Visual Studio で、クラウド アプリ プロジェクトの NuGet パッケージを編集します。

    ![プロジェクトを右クリックし、](./media/app-insights-cloudservices/03-nuget.png)

2. [Application Insights for Web] を追加 (http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet パッケージ。 SDK のこのバージョンには、ロールの情報など、サーバー コンテキストを追加するモジュールが含まれています。 worker ロールの場合、Windows Services の Application Insights を使用します。

    ![Search for "Application Insights"](./media/app-insights-cloudservices/04-ai-nuget.png)

3. データを Application Insights リソースに送信するように SDK を構成します。

    インストルメンテーション キーをファイルに構成設定として設定 `ServiceConfiguration.Cloud.cscfg`します。 ([Sample code](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/AzureEmailService/ServiceConfiguration.Cloud.cscfg)).

    ```XML
     <Role name="WorkerRoleA"> 
      <Setting name="APPINSIGHTS_INSTRUMENTATIONKEY" value="YOUR IKEY" /> 
     </Role>
    ```

    適切なスタートアップ関数の中に、構成設定のインストるメンテーション キーを設定します。

    ```C#
     TelemetryConfiguration.Active.InstrumentationKey = RoleEnvironment.GetConfigurationSettingValue("APPINSIGHTS_INSTRUMENTATIONKEY");
    ```

    」では、同じ名前 `APPINSIGHTS_INSTRUMENTATIONKEY` Azure 診断のレポートで、構成の設定が使用されます。

    この手順をアプリケーションの各ロールで実行します。 次の例を参照してください。

 * [Web ロール](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Global.asax.cs#L27)
 * [ワーカー ロール](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L232)
 * [Web ページの](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Views/Shared/_Layout.cshtml#L13)

4. ApplicationInsights.config ファイルが常に出力ディレクトリにコピーされるように設定します。

    (.config ファイルの中に、インストルメンテーション キーの配置を求めるメッセージがあります。 ただし、クラウド アプリケーションでは、それは .cscfg ファイルから設定することをお勧めします。 これにより、ポータルでロールが正確に識別されます)。


#### アプリを実行して発行する

アプリを実行し、Azure にサインインします。 作成した Application Insights リソース、および個々 のデータ ポイントに表示されるがわかります開いて [検索](app-insights-diagnostic-search.md), 、内のデータを集計および [メトリック エクスプ ローラー](app-insights-metrics-explorer.md)します。

さらにテレメトリを追加し (後のセクションを参照)、アプリを発行して、ライブの診断と使用状況のフィードバックを取得します。


#### データが表示されない場合

* 開いている、 [検索 ][diagnostic] タイル、個々 のイベントを表示します。
* アプリケーションを使用して、テレメトリがいくつか生成されるようにさまざまなページを開きます。
* 数秒待機してから [最新の情報に更新] をクリックします。
* 参照してください [トラブルシューティング ][qna]します。



## テレメトリの追加

以下のセクションでは、アプリケーションの異なる部分から他のテレメトリを取得する方法を示します。


## ワーカー ロールからの要求を追跡する

Web ロールでは、HTTP 要求に関するデータが要求モジュールによって自動的に収集されます。 参照してください、 [サンプルの MVCWebRole](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/MvcWebRole) 既定コレクションの動作をオーバーライドする方法の例についてです。

HTTP 要求の場合と同じ方法で追跡することで、worker ロールの呼び出しのパフォーマンスを記録できます。 Application Insights では、「要求」型の利用統計情報は、サーバー側で名前を付け、時間を指定し、個別に成功と失敗を判定できる作業単位を測定できます。 SDK は HTTP 要求を自動的に記録します。一方で、独自のコードを挿入し、worker ロールへの要求を追跡できます。

レポートの要求にインストルメント化された 2 つのサンプル worker ロールを参照してください: [WorkerRoleA](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleA) と [WorkerRoleB](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleB)

## Azure 診断

[Azure 診断](vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md) ロール管理イベント、パフォーマンス カウンター、およびアプリケーション ログにデータが含まれます。 これらのデータを Application Insights に送信し、他のテレメトリと共に表示して、問題の診断を容易にできます。

ロールが予期せずに失敗した場合、または起動に失敗した場合は、Azure 診断が特に役立ちます。

1. ロール (プロジェクトではない!) を右クリックします。 プロパティを表示して選択 **診断を有効にする**, 、**診断を Application Insights に送信**します。

    ![Search for "Application Insights"](./media/app-insights-cloudservices/21-wad.png)

    **または、アプリが既に発行されて動いている場合は**、サーバー エクスプローラーまたは Cloud エクスプローラーを開き、アプリを右クリックして同じオプションを選択します。

3.  他のテレメトリと同じ Application Insights リソースを選択します。

    必要な場合は、異なるサービス構成 (クラウド、ローカル) で異なるリソースを設定し、開発データとライブ データを分離しておくことができます。

3. 必要に応じて、 [Azure 診断の一部を除外](app-insights-azure-diagnostics.md) Application Insights に転送します。 既定ではすべてです。

### Azure 診断イベントを表示する

診断データは次の場所で見つかります。

* パフォーマンス カウンターは、カスタム メトリックとして表示されます。
* Windows イベント ログは、トレースとカスタム イベントとして表示されます。
* アプリケーション ログ、ETW ログ、診断インフラストラクチャ ログは、トレースとして表示されます。

パフォーマンス カウンターとイベントの数を表示する [メトリックス エクスプ ローラー](app-insights-metrics-explorer.md) し、新しいグラフを追加します。


![](./media/app-insights-cloudservices/23-wad.png)

使用 [検索](app-insights-diagnostic-search.md) Azure Diagnostics によって送信された、さまざまなトレース ログを検索します。 たとえば、ロールでハンドルされない例外が発生して、ロールがクラッシュし、リサイクルされた場合、Windows イベント ログのアプリケーション チャネルにその情報が表示されます。 検索機能を使用して、Windows イベント ログのエラーを確認し、例外のスタック トレース全体を取得することで、問題の根本原因を発見できます。


![](./media/app-insights-cloudservices/25-wad.png)

## アプリの診断

Azure 診断には、アプリが System.Diagnostics.Trace を使用して生成するログ エントリが自動的に含まれます。

Log4N または NLog フレームワークを使用すると、既に場合することもできますが、 [[netlogs] ログ トレースをキャプチャ][netlogs]します。

[カスタム イベントおよびメトリック ][api] では、クライアントまたはサーバーまたはその両方をアプリケーションのパフォーマンスと使用状況の詳細を参照してください。

## 依存関係

Application Insights SDK では、REST API や SQL サーバーなどの外部依存関係に対してアプリが実行した呼び出しをレポートできます。 このレポートを使用して、特定の依存関係が応答速度の低下や障害の原因となっているかどうかを確認できます。

依存関係を追跡するには、使用して、web/ワーカー ロールを設定する必要が、 [Application Insights エージェント](app-insights-monitor-performance-live-website-now.md) とも呼ばれます"Status Monitor"です。

Web/worker ロールで Application Insights エージェントを使用するには:

* 追加、 [AppInsightsAgent](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/WorkerRoleA/AppInsightsAgent) フォルダーと web/ワーカー ロール プロジェクトにある 2 つのファイルです。 常に出力ディレクトリにコピーされるように、ビルド プロパティを設定してください。 これらのファイルによってエージェントがインストールされます。
* CSDEF ファイルを開始タスクを追加して、よう [ここ](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/AzureEmailService/ServiceDefinition.csdef#L18)します。
* 注: *ワーカー ロール* ように、3 つの環境変数を必要と [ここ](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService/AzureEmailService/ServiceDefinition.csdef#L44)します。 これは web ロールでは必要ありません。

Application Insights ポータルに表示される内容の例を次に示します。

* 要求と依存関係が自動的に関連付けられる多様な診断:

    ![](./media/app-insights-cloudservices/SMxacy4.png)

* Web ロールのパフォーマンスと依存関係情報:

    ![](./media/app-insights-cloudservices/6yOBtKu.png)

* worker ロールの要求と依存関係の情報が表示されたスクリーンショット:

    ![](./media/app-insights-cloudservices/a5R0PBk.png)

## 例外

参照してください [Application Insights での例外の監視](app-insights-asp-net-exceptions.md) 未処理の別の web アプリケーションの種類からの例外を収集する方法の詳細についてのです。

サンプルの web ロールには MVC5 コントローラーおよび Web API 2 コントローラーが含まれています。 この 2 つからの未処理の例外は、以下を使用してキャプチャされます。

* [AiHandleErrorAttribute](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiHandleErrorAttribute.cs) セットアップ [ここ](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/FilterConfig.cs#L12) MVC5 コント ローラー用
* [AiWebApiExceptionLogger](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/Telemetry/AiWebApiExceptionLogger.cs) セットアップ [ここ](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/MvcWebRole/App_Start/WebApiConfig.cs#L25) Web API 2 コント ローラー

woker ロールの場合、例外を追跡する方法は 2 つあります。

* TrackException(ex)
* Application Insights トレース リスナー NuGet パッケージを追加した場合は、System.Diagnostics.Trace を使用して例外を記録できます。 [コード例です。](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L107)

## パフォーマンス カウンター

既定では、次のカウンターが収集されます。

    * \Process(??APP_WIN32_PROC??)\% Processor Time
    * \Memory\Available Bytes
    * \.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec
    * \Process(??APP_WIN32_PROC??)\Private Bytes
    * \Process(??APP_WIN32_PROC??)\IO Data Bytes/sec
    * \Processor(_Total)\% Processor Time

また、web ロールの場合は次も収集されます。

    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec    
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time
    * \ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue

追加のカスタムまたは他の windows パフォーマンス カウンターを示すとして指定できます [ここ](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/ApplicationInsights.config#L14)

  ![](./media/app-insights-cloudservices/OLfMo2f.png)

## worker ロールのテレメトリの関連付け

豊富な診断のエクスペリエンスでは、要求が失敗した場合や待機時間が長い場合の原因を確認できます。 Web ロールを使用すると、SDK によって関係するテレメトリの間に関連付けが自動的に設定されます。 
worker ロールの場合は、カスタムのテレメトリ初期化子を使用して、すべてのテレメトリに共通の Operation.Id context 属性を設定することで、これを実現できます。 
これにより、待機時間/失敗の問題の原因が依存関係とコードのどちらにあるかを一目で把握することができます。

その方法は次のとおりです。

* ように、CallContext に関連付け Id を設定 [ここ](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L36)します。 このケースでは、要求 ID を関連付け ID として使用しています。
* カスタムの TelemetryInitializer 実装を追加します。これにより、上で設定した correlationId に Operation.Id が設定されます。 次に示す: [ItemCorrelationTelemetryInitializer](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/Telemetry/ItemCorrelationTelemetryInitializer.cs#L13)
* カスタムのテレメトリ初期化子を追加します。 ApplicationInsights.config ファイルまたはコードを示すとして実行できます [ここ](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/Samples/AzureEmailService/WorkerRoleA/WorkerRoleA.cs#L233)

これで完了です。 ポータル エクスペリエンスは既に、関連付けられたすべてのテレメトリを一目で確認できるように設定されています。

![](./media/app-insights-cloudservices/bHxuUhd.png)



## クライアント テレメトリ

[[Client] web ページに JavaScript SDK を追加する][client] ページ ビュー数、ページの読み込み時間、スクリプトの例外などのブラウザー ベースのテレメトリを取得し、ページ スクリプトにカスタムのテレメトリを記述できるようにします。

## 可用性テスト

[Web の設定は、][availability] を確認する、アプリケーションが動作していて応答します。



## 例

[例では、](https://github.com/Microsoft/ApplicationInsights-Home/tree/master/Samples/AzureEmailService) web ロールと 2 つのワーカー ロールを持つサービスを監視します。

## 関連トピック

* [Azure 診断を Application Insights に送信を構成します。](app-insights-azure-diagnostics.md)
* [PowerShell を使用した Application Insights への Azure 診断の送信])(app-insights-powershell-azure-diagnostics.md)




[api]: app-insights-api-custom-events-metrics.md 
[apidefaults]: app-insights-api-custom-events-metrics.md#default-properties 
[apidynamicikey]: app-insights-api-custom-events-metrics.md#dynamic-ikey 
[availability]: app-insights-monitor-web-app-availability.md 
[azure]: app-insights-azure.md 
[client]: app-insights-javascript.md 
[diagnostic]: app-insights-diagnostic-search.md 
[netlogs]: app-insights-asp-net-trace-logs.md 
[perf]: app-insights-web-monitor-performance.md 
[portal]: http://portal.azure.com/ 
[qna]: app-insights-troubleshoot-faq.md 
[redfield]: app-insights-monitor-performance-live-website-now.md 
[start]: app-insights-overview.md 

