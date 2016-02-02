<properties 
    pageTitle="Windows 向けの Application Insights のリリース ノート" 
    description="Windows ストア SDK 用の最新の更新プログラムです。" 
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
    ms.date="09/18/2015" 
    ms.author="sergkanz"/>


# Windows Phone と Windows ストア向けの Application Insights SDK のリリース ノート

[Application Insights SDK](app-insights-windows-get-started.md) に、実行中のアプリに関するテレメトリを送信 [Application Insights](http://azure.microsoft.com/services/application-insights/), し、その使用状況とパフォーマンスを分析できます。


#### アプリケーションに SDK をインストールするには

参照してください [Application Insights for Windows Phone とストアのアプリケーションを使ってみる](app-insights-windows-get-started.md)します。

#### 最新の SDK にアップグレードするには

* 実行済みのカスタマイズを維持するには、ApplicationInsights.config をコピーします。
* ソリューション エクスプローラーでプロジェクトを右クリックし、**[NuGet パッケージの管理]** を選択します。
* インストールされているパッケージを表示するフィルターを設定します。
* インストール済みの Application Insights パッケージを選択し、[アップグレード] を選択します。
* 新旧のバージョンの ApplicationInsights.config を比較します。 旧バージョンに対して行ったカスタマイズをもう一度マージします。
* ソリューションをリビルドします。

## バージョン 1.1.1

### Windows SDK

- Windows Phone の Silverlight SDK を使用する場合は、クラッシュ時にハングを修正します。 この変更を行うと、WindowsAppInitialier.InitializeAsync(...) の呼び出しの後で、約 2 秒後以降に発生するクラッシュがディスクに保存され、次回のアプリケーションの起動時に送信されます。 呼び出し後約 2 秒より前に発生するクラッシュは、無視されます。
- NuGet の依存関係を特定のバージョンのコアと Microsoft.ApplicationInsights.PersistenceChannel (v1.2.3) に設定します。

### コア SDK

- コアは、github で管理されます。 コア SDK の将来のリリース ノートを参照できる [github で](http://github.com/Microsoft/ApplicationInsights-dotnet/releases)

## バージョン 1.1

### コア SDK

- SDK は、新しいテレメトリ タイプを今すぐ導入 `DependencyTelemetry` アプリケーションからの依存関係の呼び出しに関する情報が含まれます
- 新しいメソッド `TelemetryClient.TrackDependency` により、アプリケーションから依存関係の呼び出しに関する情報を送信するには

## バージョン 1.0.0

### Windows App SDK

- Windows アプリの新しい初期化。 新しい `WindowsAppInitializer` クラス `InitializeAsync()` SDK コレクションの初期化をブートス トラップ メソッドを使用します。 この変更により、以前の ApplicationInsights.config の手法より細かい制御とアプリ初期化パフォーマンスの飛躍的な向上が実現されます。
- DeveloperMode は自動設定されなくなりました。 DeveloperMode 動作を変更するには、コード内に指定する必要があります。
- NuGet パッケージでは、ApplicationInsights.config が挿入されません。 手動で NuGet パッケージを追加するときに、新しい WindowsAppInitializer を使用することをお勧めします。
- ApplicationInsights.config のみ読み取りを行う `< InstrumentationKey >`, 、WindowsAppInitializer 設定の [その他のすべての設定は無視されます。
- Store Market は SDK によって自動収集されます。
- 多くのバグが修正され、安定性とパフォーマンスが向上しています。

### コア SDK

- ApplicationInsights.config ファイルが不要になり、 NuGet パッケージによって追加もされません。 構成はコード内で完全に指定できます。
- NuGet パッケージは、ターゲット ファイルをソリューションに追加しません。 これにより、デバッグ ビルド時に DeveloperMode の自動設定が削除されます。 DeveloperMode をコード内に手動で設定する必要があります。

## バージョン 0.17

### Windows App SDK

- Windows App SDK で、Windows 10 テクニカル プレビューに対して、または VS 2015 RC により作成されたユニバーサル Windows アプリがサポートされるようになりました。

### コア SDK

- TelemetryClient は既定で InMemoryChannel を初期化します。
- 新しい API が追加されると、 `TelemetryClient.Flush()`します。 このフラッシュのメソッドは、そのクライアントに記録されたすべてのテレメトリのアップロードの即時ブロックをトリガーします。 これにより、プロセスのシャット ダウン前にアップロードを手動でトリガーできます。
- NuGet パッケージに .Net 4.5 ターゲットが追加されました。 このターゲットには外部依存関係がありません (BCL および EventSource 依存関係は削除されました)。

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





