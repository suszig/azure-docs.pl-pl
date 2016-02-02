<properties
    pageTitle="Application Insights for Java のリリース ノート"
    description="Java SDK 用の最新の更新プログラム。"
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
    ms.date="09/21/2015"
    ms.author="awills"/>


# Java 向けの Application Insights SDK のリリース ノート

[Application Insights SDK for Java](app-insights-java-get-started.md) に、実行中のアプリに関するテレメトリを送信 [Application Insights](http://azure.microsoft.com/services/application-insights/), し、その使用状況とパフォーマンスを分析できます。

#### アプリケーションに SDK をインストールするには

参照してください [SDK for Java を使ってみる](app-insights-java-get-started.md)します。

#### 最新の SDK にアップグレードするには

アップグレード後に、ApplicationInsights.xml に対して行ったカスタマイズをもう一度マージする必要があります。 このファイルをコピーし、新しいファイルと比較します。

*Maven か Gradle を使用している場合*

1. pom.xml か build.gradle で特定のバージョン番号を指定した場合は、それを更新します。
2. プロジェクトの依存関係を更新します。

*それ以外の場合*

* 最新バージョンのダウンロード [Azure Libraries for Java](http://dl.msopentech.com/lib/PackageForWindowsAzureLibrariesForJava.html) 古いものと置き換えます。

新旧の ApplicationInsights.xml を比較します。 表示の変更の多くは、追加やモジュールを削除するためです。 以前のカスタマイズをもう一度設定します。

## バージョン 1.0.2

- コード内にインストルメンテーション キーを明示的に指定する場合に、そのインストルメンテーション キーが構成ファイルに指定されたインストルメンテーション キーを使用してオーバーライドされるのを防止します。
- すべての正常な HTTP 状態コードを処理し、関連する HTTP 要求を「成功した」としてレポートします。
- ConfigurationFileLocator によってスローされたすべての例外を処理します。


## バージョン 1.0.1

- [Java エージェント](app-insights-java-agent.md) は、次の依存関係情報を収集します。
    - HttpClient、OkHttp、RestTemplate (Spring) 経由で行われた HTTP 呼び出し。
    - Calls to Redis made via the Jedis client. 構成可能なしきい値が渡されると、SDK も呼び出しの引数を取得します。
    - Oracle DB クライアントと Apache Derby DB クライアントを使用して行われた JDBC 呼び出し。
    - 準備されたステートメントに対する ‘executeBatch’ クエリ タイプのサポート。SDK はステートメントをバッチ番号付きで表示します。
    - JDBC クライアントをサポートするクエリ プランの提供 (MySql、PostgreSql)。クエリ プランは構成できるしきい値が交差した場合のみフェッチされます。

## バージョン 1.0.0

- CollectD 用の Application Insights ライター プラグインのサポートを追加しました。
- Application Insights Java エージェントのサポートを追加します。
- HttpClient バージョン 4.2 以降のサポートでの互換性の問題を修正しました。

## バージョン 0.9.6

- Java SDK が servlet v2.5 および HttpClient pre-v4.3 に対応するようになりました。
- Adding support for Java EE interceptors.
- Logback アペンダーから重複する依存関係を削除しました。

## バージョン 0.9.5

- Cookie の解析エラーによりカスタム イベントがユーザーやセッションに関連付けられなくなる問題を修正しました。
- Improved logic for resolving the location of the ApplicationInsights.xml configuration file.
- Anonymous User and Session cookies will not be generated on the server side. JavaScript SDK によるインストルメンテーションが今すぐにはユーザーおよびセッションの web アプリの追跡を実装するために必要な – JavaScript SDK の cookie は引き続き適用されます。 この変更により、ユーザー数とセッション数の大幅な再計算が発生する場合があることに注意してください。これは、ユーザーが開始したセッションのみがカウントされるようになったためです。

## バージョン 0.9.4

- 32 ビット Windows コンピューターからパフォーマンス カウンターを収集できます。
- 依存関係、new を使用して手動で追跡をサポートして `trackDependency` メソッド API です。
- 利用統計情報項目に合成を追加することでタグを付けるための機能、 `SyntheticSource` に報告された項目のプロパティです。





