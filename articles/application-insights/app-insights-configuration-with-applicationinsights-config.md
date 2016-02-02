<properties 
    pageTitle="ApplicationInsights.config または .xml を使った Application Insights SDK の構成" 
    description="データ コレクション モジュールを有効または無効にし、パフォーマンス カウンターとその他のパラメーターを追加します" 
    services="application-insights"
    documentationCenter="" 
    authors="OlegAnaniev-MSFT"
    editor="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/05/2015" 
    ms.author="awills"/>


# ApplicationInsights.config または .xml を使った Application Insights SDK の構成

Application Insights .NET SDK は、いくつかの NuGet パッケージで構成されます。 この 
[コア パッケージ](http://www.nuget.org/packages/Microsoft.ApplicationInsights) にテレメトリを送信するため、API を提供 
Application Insights します。 [その他のパッケージ](http://www.nuget.org/packages?q=Microsoft.ApplicationInsights) 提供 
製品利用統計情報 _モジュール_ と _初期化子_ のアプリケーションとそのコンテキストからテレメトリを自動的に追跡します。 別 
構成ファイルを調整することができますを有効にして無効を切り替えテレメトリ モジュールと初期化子、パラメーターを設定 
これらの一部です。

構成ファイルの名前は `ApplicationInsights.config` または `ApplicationInsights.xml`, の種類に応じて、
想定しています。 プロジェクトに自動的に追加時にする [SDK ][start]します。 Web アプリにも追加されます。 
によって [状態モニターを IIS サーバー ][redfield], 、Appplication Insights を選択した場合、または 
[Azure の web サイトまたは VM ][azure]します。

コントロールを同等のファイルがない、 [[client] web ページの SDK][client]します。

このドキュメントでは、構成ファイルの各セクション、SDK のコンポーネントの制御方法、それらのコンポーネントを読み込む NuGet パッケージについて説明します。

## テレメトリ モジュール (ASP.NET)

各テレメトリ モジュールは特定の種類のデータを回収し、コア API を利用してデータを送信します。 モジュールはさまざまな NuGet パッケージによりインストールされます。NuGet パッケージはまた、必要な行を .config ファイルに追加します。

各モジュールの構成ファイルにノードが存在します。 モジュールを無効にするには、ノードを削除するか、コメント アウトします。



### 依存関係の追跡

[依存関係の追跡](app-insights-dependencies.md) データベースとの外部サービスとデータベースに対してアプリが呼び出しに関する製品利用統計情報を収集します。 このモジュールを IIS サーバーで機能を許可するのには、する必要があります [インストール状態の監視 ][redfield]します。 Azure web アプリまたは Vm で使用する [Application Insights 拡張機能 ][azure]します。

追跡コードを使用して、独自の依存関係を記述することも、 [TrackDependency API](app-insights-api-custom-events-metrics.md#track-dependency)します。


* `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.DependencyCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) NuGet パッケージ。

### パフォーマンス コレクター

[システム パフォーマンス カウンターを収集する](app-insights-web-monitor-performance.md#system-performance-counters) メモリおよびネットワークで IIS のインストール環境から負荷、CPU などです。 自分で設定したパフォーマンス カウンターなど、回収するカウンターを指定できます。

* `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.PerformanceCollectorModule`
* [Microsoft.ApplicationInsights.PerfCounterCollector](http://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector) NuGet パッケージ。


### Application Insights 診断テレメトリ

`DiagnosticsTelemetryModule` Application Insights インストルメンテーション コード自体でエラーを報告します。 たとえば、次のように入力します。 
コードのパフォーマンス カウンターにアクセスできない場合、または場合、 `ITelemetryInitializer` 例外をスローします。 トレース テレメトリ 
これによって追跡モジュールは、 [診断検索の ][diagnostic]します。

* `Microsoft.ApplicationInsights.Extensibility.Implementation.Tracing.DiagnosticsTelemetryModule`
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet パッケージ。 このパッケージのみをインストールする場合、ApplicationInsights.config ファイルは自動作成されません。

### 開発者モード

`DeveloperModeWithDebuggerAttachedTelemetryModule` 強制的に Application Insights `TelemetryChannel` データをすぐに、アプリケーションのプロセスにデバッガーがアタッチされている場合、一度に 1 つのテレメトリ項目を送信します。 これにより、アプリケーションによるテレメトリの追跡時や、アプリケーションが Application Insights ポータルに表示されるときの時間間隔が削減されます。 ここでは、CPU とネットワーク帯域幅のオーバーヘッドが著しく費やされます。

* `Microsoft.ApplicationInsights.WindowsServer.DeveloperModeWithDebuggerAttachedTelemetryModule`
* [Application Insights の Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet パッケージ

### Web 要求の追跡

レポート、 [応答時間と結果コード](app-insights-asp-net.md) HTTP 要求のです。

* `Microsoft.ApplicationInsights.Web.RequestTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet パッケージ

### 例外の追跡

`ExceptionTrackingTelemetryModule` web アプリで未処理の例外を追跡します。 参照してください [障害と例外 ][exceptions]します。

* `Microsoft.ApplicationInsights.Web.ExceptionTrackingTelemetryModule`
* [Microsoft.ApplicationInsights.Web](http://www.nuget.org/packages/Microsoft.ApplicationInsights.Web) NuGet パッケージ

* `Microsoft.ApplicationInsights.WindowsServer.UnobservedExceptionTelemetryModule` -トラック [タスク例外を無視された](http://blogs.msdn.com/b/pfxteam/archive/2011/09/28/task-exception-handling-in-net-4-5.aspx)します。
* `Microsoft.ApplicationInsights.WindowsServer.UnhandledExceptionTelemetryModule` -ワーカー ロール、windows サービス、およびコンソール アプリケーションの未処理の例外を追跡します。
* [Application Insights の Windows Server](http://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/) NuGet パッケージ。

### コア API

コア パッケージの提供、 [API のコア](https://msdn.microsoft.com/library/mt420197.aspx) sdk。 その他のテレメトリ モジュールを使用して、これとすることもできます [それを使用して、独自のテレメトリを定義](app-insights-api-custom-events-metrics.md)します。

* ApplicationInsights.config にエントリがありません。
* [Microsoft.ApplicationInsights](http://www.nuget.org/packages/Microsoft.ApplicationInsights) NuGet パッケージ。 この NuGet だけをインストールする場合、.config ファイルは生成されません。

## テレメトリ チャネル

テレメトリ チャネルにより、テレメトリのバッファリングと Application Insights サービスへの送信が管理されます。

* `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel` はサービスの既定のチャネル。 メモリにデータをバッファーします。
* `Microsoft.ApplicationInsights.PersistenceChannel` はコンソール アプリケーションの別の方法です。 アプリが停止したときにフラッシュされていないデータを永続ストレージに保存できます。また、アプリが再開したときにそのデータを送信します。


## テレメトリの初期化子 (ASP.NET)

テレメトリの初期化子は、テレメトリのあらゆるアイテムと共に送信されるコンテキスト プロパティを設定します。

実行できます [独自の初期化子を記述](app-insights-api-filtering-sampling.md#add-properties) コンテキスト プロパティを設定します。

標準の初期化子は Web または WindowsServer NuGet パッケージによりすべて設定されます。


* `AccountIdTelemetryInitializer` AccountId プロパティを設定します。
* `AuthenticatedUserIdTelemetryInitializer` JavaScript SDK によって設定された AuthenticatedUserId プロパティを設定します。
* `AzureRoleEnvironmentTelemetryInitializer` 更新プログラム、 `RoleName` と `RoleInstance` のプロパティ、 `デバイス` Azure ランタイム環境から抽出された情報を含むすべてのテレメトリ項目のコンテキスト。
* `BuildInfoConfigComponentVersionTelemetryInitializer` 更新プログラム、 `バージョン` のプロパティ、 `コンポーネント` から抽出された値を含むすべてのテレメトリ項目のコンテキスト、 `BuildInfo.config` MS ビルドによって生成されるファイル。
* `ClientIpHeaderTelemetryInitializer` 更新 `Ip` のプロパティ、 `場所` すべてのテレメトリ項目のコンテキストに基づいて、 `X-転送-に対して` 要求の HTTP ヘッダー。
* `DeviceTelemetryInitializer` の次のプロパティを更新、 `デバイス` すべてのテレメトリ項目のコンテキスト。
 - `型` "PC"に設定されています。
 - `Id` web アプリケーションが実行されているコンピューターのドメイン名に設定されています。
 - `OemName` から抽出された値に設定されている、 `Win32_ComputerSystem.Manufacturer` WMI を使用してフィールドです。
 - `モデル` から抽出された値に設定されている、 `Win32_ComputerSystem.Model` WMI を使用してフィールドです。
 - `NetworkType` から抽出された値に設定されている、 `NetworkInterface`します。
 - `言語` の名前に設定されている、 `CurrentCulture`します。
* `DomainNameRoleInstanceTelemetryInitializer` 更新プログラム、 `RoleInstance` のプロパティ、 `デバイス` すべてのコンテキスト
web アプリケーションが実行されているコンピューターのドメイン名で製品利用統計情報項目。
* `OperationNameTelemetryInitializer` 更新プログラム、 `名` のプロパティ、 `RequestTelemetry` と `名` のプロパティ、 `操作` HTTP メソッドと ASP.NET MVC コント ローラーと要求の処理に呼び出されるアクションの名前に基づいて、すべてのテレメトリ項目のコンテキスト。
* `OperationIdTelemetryInitializer` 更新プログラム、 `Operation.Id` すべてのテレメトリ項目のコンテキスト プロパティが自動的に自動的に生成された要求の処理中に追跡 `Requesttelemetry.id'`します。
* `SessionTelemetryInitializer` 更新プログラム、 `Id` のプロパティ、 `セッション` から抽出された値を含むすべてのテレメトリ項目のコンテキスト、 `ai_session` cookie はユーザーのブラウザーで実行する application Insights JavaScript インストルメンテーション コードが生成されます。
* `SyntheticTelemetryInitializer` 更新プログラム、 `ユーザー`, 、`セッション` と `操作` 可用性テストまたは検索エンジン bot など、合成ソースからの要求を処理するときに、すべてのテレメトリ項目のコンテキスト プロパティが追跡されます。 既定では、 [メトリックス エクスプ ローラー](app-insights-metrics-explorer.md) 合成テレメトリは表示されません。
* `UserAgentTelemetryInitializer` 更新プログラム、 `UserAgent` のプロパティ、 `ユーザー` すべてのテレメトリ項目のコンテキストに基づいて、 `User-agent` 要求の HTTP ヘッダー。
* `UserTelemetryInitializer` 更新プログラム、 `Id` と `AcquisitionDate` のプロパティ `ユーザー` から抽出された値を持つすべてのテレメトリ項目のコンテキスト、 `ai_user` cookie はユーザーのブラウザーで実行する Application Insights JavaScript インストルメンテーション コードが生成されます。


## テレメトリ プロセッサ (ASP.NET)

テレメトリ プロセッサは、SDK からポータルに送信される直前の各テレメトリ アイテムをフィルター処理し、変更できます。

実行できます [独自のテレメトリのプロセッサを作成する](app-insights-api-filtering-sampling.md#filtering)します。


#### アダプティブ サンプリング テレメトリ プロセッサー (2.0.0-beta3 以降)

この機能は、既定では有効になっています。 アプリが多数のテレメトリを送信する場合、このプロセッサはその一部を削除します。

```xml

    <TelemetryProcessors>
      <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.AdaptiveSamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">
        <MaxTelemetryItemsPerSecond>5</MaxTelemetryItemsPerSecond>
      </Add>
    </TelemetryProcessors>
```

パラメーターは、アルゴリズムが実現しようとするターゲットを指定します。 SDK の各インスタンスは独立して機能するため、サーバーが複数のコンピューターのクラスターである場合、テレメトリの実際の量もそれに応じて増加します。

[の詳細については、サンプリング](app-insights-sampling.md)します。



#### 固定レート サンプリング テレメトリ プロセッサー (2.0.0-beta1 以降)

標準もあります [テレメトリ プロセッサをサンプリング](app-insights-api-filtering-sampling.md#sampling) (から 2.0.1)。

```XML

    <TelemetryProcessors>
     <Add Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.SamplingTelemetryProcessor, Microsoft.AI.ServerTelemetryChannel">

     
     
     <SamplingPercentage>10</SamplingPercentage>
     </Add>
   </TelemetryProcessors>
```



## チャネル パラメーター (Java)

これらのパラメーターは、Java SDK が収集するテレメトリ データの格納とフラッシュする方法に影響します。

#### MaxTelemetryBufferCapacity

SDK のメモリー内ストレージに格納できるテレメトリ項目の数。 この数に達すると、テレメトリのバッファーがフラッシュされます。つまり、テレメトリの項目が、Application Insights サーバーに送信されます。

-   最小: 1
-   最大: 1000
-   既定値: 500

```

  <ApplicationInsights>
      ...
      <Channel>
       <MaxTelemetryBufferCapacity>100</MaxTelemetryBufferCapacity>
      </Channel>
      ...
  </ApplicationInsights>
```

#### FlushIntervalInSeconds

メモリー内ストレージに格納されているデータがフラッシュされる (Application Insights に送信される) 頻度を決定します。

-   最小: 1
-   最大: 300
-   既定値: 5

```

    <ApplicationInsights>
      ...
      <Channel>
        <FlushIntervalInSeconds>100</FlushIntervalInSeconds>
      </Channel>
      ...
    </ApplicationInsights>
```

#### MaxTransmissionStorageCapacityInMB

ローカル ディスクの永続的なストレージに割り当てられる MB 単位の最大サイズを決定します。 このストレージは、Application Insights のエンドポイントへの転送に失敗したテレメトリの項目を保存するために使用されます。 ストレージのサイズが満たされている場合は、テレメトリの新しい項目は破棄されます。

-   最小: 1
-   最大: 100
-   既定値: 10

```

   <ApplicationInsights>
      ...
      <Channel>
        <MaxTransmissionStorageCapacityInMB>50</MaxTransmissionStorageCapacityInMB>
      </Channel>
      ...
   </ApplicationInsights>
```



## InstrumentationKey

これは、データが表示される Application Insights のリソースを決定します。 通常、アプリケーションごとに、個別のキーを持つリソースを作成します。

キーを動的に設定する場合 (たとえば、アプリケーションからの結果を別のリソースに送信する場合)、構成ファイルからキーを省略し、代わりにコードで設定することができます。

TelemetryClient のすべてのインスタンスのキーを設定するには (標準のテレメトリ モジュールを含む)、TelemetryConfiguration.Active でキーを設定します。 ASP.NET サービスの global.aspx.cs など、初期化メソッドでキーの設定を行います。

```C#

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey = 
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      //...
```

特定のイベント セットを異なるリソースに送信する場合、特定の TelemetryClient のキーを設定できます。

```C#

    var tc = new TelemetryClient();
    tc.Context.InstrumentationKey = "----- my key ----";
    tc.TrackEvent("myEvent");
    // ...
```

[の詳細については、API の ][api]します。

新しいキーを取得する [Application Insights ポータルでの新しいリソースを作成する ][new]します。




[api]: app-insights-api-custom-events-metrics.md 
[azure]: ../insights-perf-analytics.md 
[client]: app-insights-javascript.md 
[diagnostic]: app-insights-diagnostic-search.md 
[exceptions]: app-insights-asp-net-exceptions.md 
[netlogs]: app-insights-asp-net-trace-logs.md 
[new]: app-insights-create-new-resource.md 
[redfield]: app-insights-monitor-performance-live-website-now.md 
[start]: app-insights-overview.md 

