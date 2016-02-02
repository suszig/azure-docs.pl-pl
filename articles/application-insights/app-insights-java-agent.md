<properties 
    pageTitle="Java Web アプリでの依存関係、例外、および実行時間の監視" 
    description="Extended monitoring of your Java website with Application Insights (Application Insights を使用した Java Web サイトの監視の拡張)" 
    services="application-insights" 
    documentationCenter="java"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/03/2015" 
    ms.author="awills"/>


# Java Web アプリでの依存関係、例外、および実行時間の監視

*Application Insights はプレビュー段階です。*

あれば [Application Insights の ][java], 、コード変更を行わず、詳細な分析を取得する Java エージェントを使用することができます。


* **依存関係:** アプリケーションが他のコンポーネントに対して行った呼び出しについてのデータであり、次のものを含みます。
 * **REST 呼び出し**: HttpClient、OkHttp、および RestTemplate (Spring) 経由で行われた呼び出しです。
 * **Redis 呼び出し**: Jedis クライアント経由で行われた呼び出しです。 呼び出しにかかる時間が 10 秒を超えた場合、エージェントは呼び出し引数も取得します。
 * * *[JDBC 呼び出し](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)* *-MySQL、SQL Server、PostgreSQL、SQLite、Oracle DB、または Apache Derby DB です。 "executeBatch"の呼び出しはサポートされています。 MySQL と PostgreSQL で呼び出しにかかる時間が 10 秒を超えた場合、エージェントはクエリ プランをレポートします。
* **例外の検出:** コードで処理される例外に関するデータ。
* **メソッドの実行時間:** 特定のメソッドの実行にかかる時間に関するデータ。

Java エージェントを使用するには、これをサーバーにインストールします。 Web アプリをインストルメント化する必要があります、 [Application Insights Java SDK ][java]します。

## Jave 用の Application Insights エージェントのインストール

1. コンピューターで、Java サーバーを実行している [エージェントをダウンロードする](https://azuredownloads.blob.core.windows.net/applicationinsights/sdk.html)します。
2. アプリケーション サーバーのスタートアップ スクリプトを編集し、次の JVM を追加します。

    `javaagent:`*エージェントの JAR ファイルへの完全パス*

    たとえば、Linux マシンの Tomcat で以下を実行します。

    `エクスポート JAVA_OPTS ="$JAVA_OPTS javaagent: < エージェントの JAR ファイルへの完全パス >"`

3. アプリケーション サーバーを再起動します。

## エージェントの構成

という名前のファイルを作成する `AI Agent.xml` とエージェントの JAR ファイルと同じフォルダーに配置します。

xml ファイルの内容を設定します。 次の例を編集して、必要に応じて、機能を含めるか省略します。

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>

        
        <BuiltIn enabled="true">
           
           <Jedis enabled="true" thresholdInMS="1000"/>

           
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String:I)V" />
        </Class>

      </Instrumentation>
    </ApplicationInsightsAgent>
```

レポートの例外や、個々のメソッドのメソッド タイミングを有効にする必要があります。

既定では、 `reportExecutionTime` が true の場合 `reportCaughtExceptions` は false。

## データの表示

Application Insights リソースで集計されたリモートの依存関係およびメソッドの実行時間が表示されます [の ][metrics]します。

依存関係、例外、メソッドのレポートの個々 のインスタンスを検索するには、開く [検索 ][diagnostic]します。

[依存関係の問題の診断の詳細については](app-insights-dependencies.md#diagnosis)します。



## 疑問がある場合問題が発生した場合

[Java のトラブルシューティング](app-insights-java-troubleshoot.md)






[api]: app-insights-api-custom-events-metrics.md 
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception 
[availability]: app-insights-monitor-web-app-availability.md 
[diagnostic]: app-insights-diagnostic-search.md 
[eclipse]: app-insights-java-eclipse.md 
[java]: app-insights-java-get-started.md 
[javalogs]: app-insights-java-trace-logs.md 
[metrics]: app-insights-metrics-explorer.md 
[usage]: app-insights-web-track-usage.md 

