<properties 
    pageTitle="Application Insights for .NET のリリース ノート" 
    description=".NET SDK 用の最新の更新プログラム。" 
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
    ms.date="11/11/2015" 
    ms.author="sergkanz"/>


# .NET 向けの Application Insights SDK のリリース ノート

[Application Insights SDK for .NET](app-insights-asp-net.md) に、実行中のアプリに関するテレメトリを送信 [Application Insights](http://azure.microsoft.com/services/application-insights/), し、その使用状況とパフォーマンスを分析できます。


#### アプリケーションに SDK をインストールするには

参照してください [for .NET Application Insights を使ってみる](app-insights-asp-net.md)します。

#### 最新の SDK にアップグレードするには

* アップグレード後に、ApplicationInsights.config に対して行ったカスタマイズをもう一度マージする必要があります。 カスタマイズしたかどうかが不明な場合は、新しいプロジェクトを作成して Application Insights を追加し、元の .config ファイルと新しいプロジェクトの .config ファイルを比較します。 相違点が見つかったら、メモしておきます。
* ソリューション エクスプローラーでプロジェクトを右クリックし、**[NuGet パッケージの管理]** を選択します。
* インストールされているパッケージを表示するフィルターを設定します。
* **[Microsoft.ApplicationInsights.Web]**、**[アップグレード]** の順に選択します  (これで従属するすべてのパッケージもアップグレードされます)。
* ApplicationInsights.config を前のコピーと比較します。 表示される変更の大部分は、モジュールを削除したり、パラメーター化できるようにしたことが原因です。 前のファイルに対して行ったカスタマイズをもう一度設定します。
* ソリューションをリビルドします。


## バージョン 2.0.0-beta3

- [アダプティブ サンプリング](app-insights-sampling.md)

## バージョン 2.0.0-beta2

- ITelemetryProcessor のサポート、コードまたは構成ファイルを使用して構成する機能が追加されました。 [により、SDK のカスタム フィルター処理](app-insights-api-telemetry-processors/#telemetry-processors)
- コンテキストの初期化子が削除されました。 使用 [テレメトリの初期化子](https://azure.microsoft.com/documentation/articles/app-insights-api-telemetry-processors/#telemetry-initializers) 代わりにします。
- Application Insights for .Net framework 4.6 が更新されました。
- カスタム イベント名に最大 512 文字まで使用できるようになりました。
- プロパティ `OperationContext.Name` の名前に変更 `RootName`します。
- プロパティ `Requesttelemetry.id'` を削除します。
- プロパティ `Id` と `Context.Operation.Id` RequestTelemetry は初期化できません新しい RequestTelemetry を作成するときにします。
- `RequestTelemetry.Name` できなくなった場合は初期化されていません。 `RequestTelemetry.Context.Operation.Name` が使用されます。
- 要求のモニタリングで、応答コード 401 は通常の認証ハンドシェイクの一部であり、これにより要求は成功します。
- UI スレッドから InMemoryChannel (既定のチャネル) を初期化する際に UI スレッドがロック状態になる問題を解消します。 これにより、WPF アプリケーションでの UI フリーズの問題が解消します。

## Version 2.0.0-beta1

- 必須フィールドの一部が指定されていない場合は、TrackDependency によって有効な JSON が生成されます。
- 冗長プロパティ `Requesttelemetry.id'` のプロキシのみが、 `RequestTelemetry.Operation.Id`します。
- 新しいインターフェイス `ISupportSampling` データ項目の種類のほとんどの明示的な実装です。
- `カウント` DependencyTelemetry のプロパティは廃止としてマークします。 使用 `SamplingPercentage` 代わりにします。
- 新しい `CloudContext` 導入およびプロパティ `RoleName` と `RoleInstance` から移動 `DeviceContext`します。
- 新しいプロパティ `AuthenticatedUserId` に `UserContext` 認証ユーザー id を指定します。
- 追加 `Microsoft.ApplicationInsights.Web.AccountIdTelemetryInitializer`, 、`Microsoft.ApplicationInsights.Web.AuthenticatedUserIdTelemetryInitializer` Javascript SDK によって設定された認証されたユーザー コンテキストを初期化します。
- 追加 `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ITelemetryProcessor` およびレートの数を固定として実装するサポートをサンプリングします。
- 追加 `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.TelemetryChannelBuilder` の作成ができる、 `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel` 一連の `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ITelemetryProcessor`します。

## バージョン 1.2

- ASP.NET ライブラリの依存関係を持たないテレメトリ初期化子から移動された `Microsoft.ApplicationInsights.Web` に新しい依存関係 nuget `Microsoft.ApplicationInsights.WindowsServer`
- `Microsoft.ApplicationInsights.Web.dll` に名前が変更された `Microsoft.AI.Web.dll`
- `Microsoft.ApplicationInsights.Web.TelemetryChannel` で nuget の名称変更 `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel`します。 `Microsoft.ApplicationInsights.Extensibility.Web.TelemetryChannel` でアセンブリの名前が変更された `Microsoft.AI.ServerTelemetryChannel.dll`します。 `Microsoft.ApplicationInsights.Extensibility.Web.TelemetryChannel` でクラスの名前が変更された `Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel`します。
- 除外する Web SDK の一部であるすべての名前空間が変更された `拡張` 部分です。 ApplicationInsights.config ですべてのテレメトリの初期化子を含む、 `ApplicationInsightsWebTracking` web.config 内のモジュール。
- ランタイム インストルメンテーション エージェント (Status Monitor または Azure WebSite 拡張機能を通して有効にします) を使用して収集される依存関係は、スレッドに HttpContext.Current が存在しない場合は非同期としてマークされません。
- プロパティ `SamplingRatio` の `DependencyTrackingTelemetryModule` は何もしませんし、不使用とマークします。
- `Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector` でアセンブリの名前が変更された `Microsoft.AI.PerfCounterCollector`
- Web SDK とデバイス SDK でいくつかの小さなバグが修正されています。


## バージョン 1.1

- 新しいテレメトリ タイプ `DependencyTelemetry` アプリケーションからの依存関係の呼び出しに関する情報の送信に使用できるようになりました (SQL、HTTP 呼び出しなど)。
- 新しいオーバー ロード メソッド `TelemetryClient.TrackDependency` 依存関係の呼び出しに関する情報を送信することができるようになりました。
- TelemetryConfiguration.CreateDefault の使用時に診断モジュールによってスローされる NullReferenceException が修正されました。

## バージョン 1.0

- テレメトリの初期化子とテレメトリ モジュールを別々 のサブ名前空間からルートに移動 `Microsoft.ApplicationInsights.Extensibility.Web` 名前空間。
- 既に含まれているために、"Web"プレフィックスをテレメトリの初期化子とテレメトリ モジュールの名前から削除しました、 `Microsoft.ApplicationInsights.Extensibility.Web` 名前空間の名前。
- 移動 `DeviceContextInitializer` から、 `Microsoft.ApplicationInsights` アセンブリを `Microsoft.ApplicationInsights.Extensibility.Web` アセンブリに変換し、 `ITelemetryInitializer`します。
- 名前空間とアセンブリの名前を変更する `Microsoft.ApplicationInsights.Extensibility.RuntimeTelemetry` に `Microsoft.ApplicationInsights.Extensibility.DependencyCollector` NuGet パッケージの名前との一貫性を維持します。
- 名前を変更 `RemoteDependencyModule` に `DependencyTrackingTelemetryModule`します。
- 名前を変更 `CustomPerformanceCounterCollectionRequest` に `PerformanceCounterCollectionRequest`します。

## バージョン 0.17

- Framework 4.5 アプリケーションの EventSource NuGet に対する依存関係が削除されました。
- 匿名ユーザーとセッションの Cookie は、サーバー側では生成されません。 Web アプリのユーザーおよびセッションの追跡を実装するために JS SDK によるインストルメンテーションが必要になりました。JavaScript SDK の Cookie は引き続き適用されます。 テレメトリ モジュール `WebSessionTrackingTelemetryModule` と `WebUserTrackingTelemetryModule` はサポートされなくなり、ApplicationInsights.config ファイルから削除されました。 この変更により、ユーザー数とセッション数の大幅な再計算が発生する場合があることに注意してください。これは、ユーザーが開始したセッションのみがカウントされるようになったためです。
- OSVersion の値が SDK により既定で設定されなくなりました。 空の場合、OS と OSVersion が Application Insights のパイプラインによってユーザー エージェントに基づいて計算されます。
- Web SDK では、高負荷のシナリオ用に最適化された永続化チャネルが使用されます。 "Spiral of death (悪循環)" の問題が修正されました。 "Spiral of death (悪循環)" とは、テレメトリの項目数が急激に増加し、エンドポイントのスロットルの限界を大幅に超えた場合に、一定時間の経過後に処理が再試行されるものの、その再試行でも調整が発生するような状態を指します。
- 開発者モードは実稼働用に最適化されています。 誤ってそのままにしておいても、追加情報の出力を試行する前ほどの大きな負荷は発生しません。
- 開発者モードが既定で有効になるのは、アプリケーションがデバッグ中の場合のみです。 使用してメソッドをオーバーライドすることができます `DeveloperMode` の  `ITelemetryChannel` インターフェイスです。

## バージョン 0.16

2015-04-28 プレビュー

- SDK は DNX ターゲット プラットフォームの監視を有効にするようになりましたサポート [.NET Core framework](http://www.dotnetfoundation.org/NETCore5) アプリケーションです。
- インスタンスを `TelemetryClient` もはやインストルメンテーション キーをキャッシュしません。 インストルメンテーション キー設定されていなかった場合 `TelemetryClient` 明示的に `InstrumentationKey` は null を返します。 設定すると、問題が解決 `TelemetryConfiguration.Active.InstrumentationKey` テレメトリ モジュールなどの依存関係コレクター、web 要求データ コレクションとパフォーマンス カウンター用のコレクターが新しいインストルメンテーション キーを使用してテレメトリがいくつかが既に収集された後です。

## バージョン 0.15

- 新しいプロパティ `Operation.SyntheticSource` で入手可能 `TelemetryContext`します。 テレメトリ項目を「本物のユーザー トラフィックではない」として設定し、そのトラフィックが生成された背景を指定できるようになりました。 このプロパティを設定する例としては、テスト自動化のトラフィックを負荷テストのトラフィックと区別できます。
- チャネル ロジックは「Microsoft.ApplicationInsights.PersistenceChannel」と呼ばれる別個の NuGet に移動されました。 既定のチャネルは「InMemoryChannel」という名前になりました。
- 新しいメソッド `TelemetryClient.Flush` ではにバッファーからのテレメトリ項目を同期的にフラッシュできます

## バージョン 0.13

使用可能な以前のバージョンのリリース ノートはありません。







