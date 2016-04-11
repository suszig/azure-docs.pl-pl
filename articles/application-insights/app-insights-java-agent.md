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

あれば [Application Insights で Java web アプリをインストルメント化された][java], 、コード変更を行わず、詳細な分析を取得する Java エージェントを使用することができます。


* **依存関係:** など、他のコンポーネントをアプリケーションがで行われた呼び出しに関するデータ。
 * **REST 呼び出し** HttpClient、OkHttp および RestTemplate (スプリング) を介して行われます。
 * **Redis** Jedis クライアントを介して行われる呼び出しです。 呼び出しにかかる時間が 10 秒を超えた場合、エージェントは呼び出し引数も取得します。
 * **[JDBC 呼び出し](http://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** -MySQL、SQL Server、PostgreSQL、SQLite、Oracle DB、または Apache Derby DB です。 "executeBatch"の呼び出しはサポートされています。 MySQL と PostgreSQL で呼び出しにかかる時間が 10 秒を超えた場合、エージェントはクエリ プランをレポートします。 
* **例外の検出:** 、コードで処理される例外に関するデータ。
* **メソッドの実行時間:** 特定のメソッドの実行にかかる時間に関するデータ。

Java エージェントを使用するには、これをサーバーにインストールします。 Web アプリをインストルメント化する必要があります、 [Application Insights Java SDK][java]します。

## Jave 用の Application Insights エージェントのインストール

1. コンピューターで、Java サーバーを実行している [エージェントをダウンロードする](https://azuredownloads.blob.core.windows.net/applicationinsights/sdk.html)です。
2. アプリケーション サーバーのスタートアップ スクリプトを編集し、次の JVM を追加します。

    `javaagent:`*エージェントの JAR ファイルへの完全パス*

    たとえば、Linux マシンの Tomcat で以下を実行します。

    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`


3. アプリケーション サーバーを再起動します。

## エージェントの構成

`AI-Agent.xml` という名前のファイルを作成し、エージェントの JAR ファイルの場所と同じフォルダーに配置します。

xml ファイルの内容を設定します。 次の例を編集して、必要に応じて、機能を含めるか省略します。 

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>
        
        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments will be sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>
           
           <!-- Set SQL query duration above which query plan will be reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions 
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne" 
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String:I)V" />
        </Class>
        
      </Instrumentation>
    </ApplicationInsightsAgent>

```

レポートの例外や、個々のメソッドのメソッド タイミングを有効にする必要があります。

既定では、`reportExecutionTime` は true、`reportCaughtExceptions` は false です。

## データの表示

Application Insights リソースで集計されたリモートの依存関係およびメソッドの実行時間が表示されます [[パフォーマンス] タイル][metrics]します。 

依存関係、例外、メソッドのレポートの個々 のインスタンスを検索するには、開く [検索][diagnostic]します。 

[詳細については依存関係の問題の診断 -](app-insights-dependencies.md#diagnosis)です。



## 疑問がある場合 問題が発生した場合

[Java のトラブルシューティング](app-insights-java-troubleshoot.md)



<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiexceptions]: app-insights-api-custom-events-metrics.md#track-exception
[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[metrics]: app-insights-metrics-explorer.md
[usage]: app-insights-web-track-usage.md

 
