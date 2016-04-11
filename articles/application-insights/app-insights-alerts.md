<properties 
    pageTitle="Application Insights のアラートの設定" 
    description="クラッシュ、例外、メトリックの変化に関して電子メールを受け取ります。" 
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
    ms.date="11/23/2015" 
    ms.author="awills"/>
 
# Application Insights のアラートの設定

[Visual Studio Application Insights][start] アプリのパフォーマンスまたは使用状況のメトリックの変化を警告することができます。 

Application Insights で、実行中のアプリを監視して、 [さまざまなプラットフォーム][platforms] パフォーマンス問題の診断し、使用パターンの把握に役立ちます。

次の 2 つの種類のアラートがあります。
 
* **Web テスト** サイトが、インターネットで使用できなくなったか応答して緩やかに変化を確認します。 [詳細については][availability]です。
* **メトリック アラート** いずれかのメトリックが期間には、エラー数、メモリ、ページ ビューなどの値のしきい値を超えたときに通知します。 

 [Web テストに関する別のページ][availability], ので、ここではメトリック アラートのみが表示されます。

## メトリック アラート

場合は、アプリの Application Insights を設定していない [まず][start]します。

メトリックがしきい値を超えたときに電子メールを受け取るには、メトリックス エクスプローラーまたは [概要] ブレードのアラート ルール タイルのいずれかから設定を始めます。

![[アラート ルール] ブレードで、[アラートの追加] をクリックします。 測定するリソースとしてアプリケーションを設定し、アラートの名前を指定し、メトリックを選択します。](./media/app-insights-alerts/01-set-metric.png)

その他のプロパティの前に、リソースを設定します。 **「(コンポーネント)」リソースを選択** 場合は、パフォーマンスまたは使用状況メトリック アラートを設定します。

しきい値を入力するように求められたら、単位に注意してください。

アラートに付ける名前は、リソース グループ内で (アプリケーション内だけでなく) 一意である必要があります。

*[アラートの追加] ボタンが表示されません。* -組織のアカウントを使用いますか。 所有者または投稿者が、アプリケーションのリソースにアクセスする場合に、アラートを設定できます。 [設定] -> [ユーザー] を表示します。 [アクセス制御の詳細情報][roles]します。

## アラートの表示

アラートの状態が非アクティブとアクティブとの間で変化すると、メールが送信されます。 

[アラート ルール] ブレードに、各アラートの現在の状態が表示されます。

アラート ボックスの一覧には、最近のアクティビティの概要が表示されます。

![](./media/app-insights-alerts/010-alert-drop.png)

状態の変更履歴は、操作イベント ログにも表示されます。

![下部付近の [概要] ブレードで、[過去 1 週間のイベント] をクリックします。](./media/app-insights-alerts/09-alerts.png)

*これらの「イベント」は、利用統計情報イベントまたはカスタム イベントに関係しますか。*

* いいえ、できません。 これらの操作イベントは、アプリケーションのリソースに起きたことのログだけです。 


## アラートのしくみ

* アラートには、"警告" と "正常" の 2 つの状態があります。 

* アラートの状態が変わると、電子メールが送信されます。

* アラートはメトリックが到着するたびに評価されますが、それ以外は評価されません。

* 評価では、前の期間にメトリックが集計され、これをしきい値と比較して新しい状態が決定されます。

* 選択した期間によって、メトリックが集計される間隔が指定されます。 これはアラートが評価される頻度には影響しません。アラートの評価の頻度は、メトリックの到着の頻度に左右されます。

* しばらくの間、特定のメトリックのデータが到着しない場合、このギャップはアラートの評価とメトリック エクスプローラーのグラフにさまざまな影響を及ぼします。 メトリック エクスプローラーでは、グラフのサンプリング間隔よりも長い期間データがない場合、グラフには値 0 が表示されます。 ただし、同じメトリックに基づくアラートは再評価されないので、アラートの状態が変わることはありません。 

    データが最終的に到着すると、グラフは 0 以外の値に戻ります。 アラートでは、指定した期間に使用できるデータに基づいて評価が行われます。 その期間に使用できる新しいデータ ポイントが 1 つしかない場合、集計はそのデータ ポイントだけに基づきます。

* 長い期間を設定した場合でも、アラートの状態 (警告状態と正常状態) 遷移が頻繁に発生する場合があります。 これは、メトリック値がしきい値付近で変動している場合に発生する可能性があります。 しきい値にヒステリシスありません。警告状態への切り替えは正常状態への切り替えと同じ値で発生します。



## 可用性のアラート

世界中の場所から任意の Web サイトをテストする Web テストを設定できます。 [詳細については][availability]です。

## どのようなアラートを設定するのが適切でしょうか。

これは、アプリケーションごとに異なります。 第一に、多数のメトリックを設定しないことをお勧めします。 通常の動作を把握するために、アプリケーションの実行中に、しばらくの間、メトリックのグラフを観察してください。 これは、パフォーマンスを向上させる方法を見つけるために役立ちます。 次に、メトリックが標準ゾーンを超えた場合に通知するアラートを設定します。 

次のような一般的なアラートがあります。

* [Web テスト][availability] はアプリケーションが web サイトまたは web サービス、パブリック インターネットに表示されている場合に重要です。 このテストは、アプリケーションの問題ではなく、通信事業者の問題である場合でも、サイトがダウンするか、応答速度が低下した場合に通知します。 ただし、これは代理テストであるため、ユーザーの実際のエクスペリエンスは測定されません。
* [ブラウザー メトリック][client], 、特にブラウザー **ページ読み込み時間**, 、web アプリケーションに適しています。 ページに多数のスクリプトがある場合探しますたい **ブラウザーの例外は**です。 これらのメトリックとアラートを取得するために設定する必要があります。 [web ページの監視][client]します。
* **サーバーの応答時間** と **失敗した要求を** web アプリケーションのサーバー側のです。 アラートの設定に加えて、これらのメトリックを監視して、高い要求レートでメトリックが過度に変動するかどうかを確認します。これは、アプリケーションでのリソースの不足を示している可能性があります。
* **サーバーの例外** - それらを表示するいくつかを行う必要がある [追加のセットアップ](app-insights-asp-net-exceptions.md)します。

## PowerShell を使用したアラートの設定

ほとんどの場合、手動によるアラート設定で十分に目的を果たせます。 ただし、メトリックのアラートを自動的に作成する必要がある場合は、PowerShell を使用することができます。

#### 1 回限りのセットアップ

以前に Azure サブスクリプションで PowerShell を使用したことがない場合

スクリプトを実行するコンピューターに Azure PowerShell モジュールをインストールします。 

 * インストール [Microsoft Web Platform Installer (v5 またはそれ以上)](http://www.microsoft.com/web/downloads/platform.aspx)します。
 * このインストーラーを使用して Microsoft Azure PowerShell をインストールする。


#### Connect to Azure

Azure PowerShell を起動し、 [サブスクリプションへの接続](powershell-install-configure.md):

```PowerShell

    Add-AzureAccount
    Switch-AzureMode AzureResourceManager
```


#### アラートの取得

    Get-AlertRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

#### アラートの追加


    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US"
     -RuleType Metric



#### 例 1

「HTTP 要求に対するサーバーの応答時間」の 5 分間の平均が 1 秒を超えた場合、電子メールで通知してください。 私の Application Insights リソースは IceCreamWebApp と呼ばれており、リソース グループ Fabrikam 内にあります。 私は Azure サブスクリプションの所有者です。

GUID は、サブスクリプション ID です (アプリケーションのインストルメンテーション キーではありません)。

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

#### 例 2

私が使用して、アプリケーションがある [TrackMetric()](app-insights-api-custom-events-metrics.md#track-metric) "salesPerHour"という名前のメトリックを報告するには 24 時間にわたる "salesPerHour" の平均が 100 を下回る場合は、私の同僚に電子メールを送信してください。

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

同じ規則には、メトリックを使用して、報告に使用できます、 [測定パラメーター](app-insights-api-custom-events-metrics.md#properties) TrackEvent または trackPageView などの別の追跡呼び出しのです。

#### メトリックの名前

メトリックの名前 | 画面の名前 | 説明
---|---|---
`basicExceptionBrowser.count`|ブラウザーの例外|ブラウザーでスローされた、キャッチされない例外の数。
`basicExceptionServer.count`|サーバーの例外|アプリによってスローされた未処理の例外の数
`clientPerformance.clientProcess.value`|クライアントの処理時間|ドキュメントの最終バイトを受信してから、DOM が読み込まれるまでの時間。 非同期要求がまだ処理されている可能性があります。
`clientPerformance.networkConnection.value`|ページ読み込みのネットワーク接続時間| ブラウザーがネットワークに接続するために要する時間です。 キャッシュされている場合は、0 にすることができます。
`clientPerformance.receiveRequest.value`|受信応答時間| ブラウザーが要求を送信してから応答を受信し始めるまでの時間。
`clientPerformance.sendRequest.value`|要求送信時間| ブラウザーが要求を送信するのに要する時間。
`clientPerformance.total.value`|ブラウザーのページ読み込み時間|ユーザーが要求を出してから DOM、スタイル シート、スクリプト、およびイメージが読み込まれるまでの時間。
`performanceCounter.available_bytes.value`|使用可能なメモリ|プロセスまたはシステムの用途で、すぐに利用できる物理メモリ。
`performanceCounter.io_data_bytes_per_sec.value`|IO 処理速度|ファイル、ネットワーク、およびデバイスに対する読み書きで 1 秒あたりに処理される合計バイト数。
`performanceCounter.number_of_exceps_thrown_per_sec`|例外レート|1 秒あたりにスローされる例外。
`performanceCounter.percentage_processor_time.value`|プロセス CPU|アプリケーション プロセスの実行命令に対してプロセッサが使用するすべてのプロセス スレッドの経過時間の割合。
`performanceCounter.percentage_processor_total.value`|プロセッサ時間|プロセッサが非アイドル スレッドで費やす時間の割合。
`performanceCounter.process_private_bytes.value`|プロセスのプライベート バイト|監視対象のアプリケーション プロセスに対して専用に割り当てられるメモリ。
`performanceCounter.request_execution_time.value`|ASP.NET 要求の実行時間|最新の要求の実行時間。
`performanceCounter.requests_in_application_queue.value`|実行キュー内の ASP.NET 要求|アプリケーション要求キューの長さ。
`performanceCounter.requests_per_sec`|ASP.NET 要求レート|ASP.NET からの 1 秒あたりのアプリケーションへのすべての要求のレート。
`remoteDependencyFailed.durationMetric.count`|依存関係の障害|サーバー アプリケーションによる外部リソースの呼び出しが失敗した回数。
`request.duration`|サーバーの応答時間|HTTP 要求を受信してから、応答の送信を終了するまでの時間。
`request.rate`|要求レート|1 秒あたりのアプリケーションに出されるすべての要求のレート。
`requestFailed.count`|失敗した要求|400 またはこれより大きな応答コードを生じさせた HTTP 要求の数 
`view.count`|ページ ビュー|Web ページに対するクライアント ユーザーの要求数。 代理トラフィックはフィルターで除外されます。
{カスタム メトリック名}|{メトリック名}|メトリックの値がによって報告された [TrackMetric](app-insights-api-custom-events-metrics.md#track-metric) または、 [追跡呼び出しの測定値パラメーター](app-insights-api-custom-events-metrics.md#properties)します。

メトリックは、さまざまなテレメトリ モジュールによって送信されます。

メトリック グループ | コレクター モジュール
---|---
basicExceptionBrowser、<br/>clientPerformance、<br/>view | [ブラウザーの JavaScript](app-insights-javascript.md)
performanceCounter | [パフォーマンス](app-insights-configuration-with-applicationinsights-config.md#nuget-package-3)
remoteDependencyFailed| [依存関係](app-insights-configuration-with-applicationinsights-config.md#nuget-package-1)
要求<br/>requestFailed|[サーバー要求](app-insights-configuration-with-applicationinsights-config.md#nuget-package-2)


## 関連項目


* [テンプレートから Application Insights と Web テスト リソースを作成する](app-insights-powershell.md)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[client]: app-insights-javascript.md
[platforms]: app-insights-platforms.md
[roles]: app-insights-resources-roles-access-control.md
[start]: app-insights-overview.md

 

