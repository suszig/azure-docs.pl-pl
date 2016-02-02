<properties 
    pageTitle="既にライブの Java Web アプリ向けの Application Insights" 
    description="サーバーで既に実行中の Web アプリケーションの監視を開始する" 
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
    ms.date="11/24/2015" 
    ms.author="awills"/>


# 既にライブの Java Web アプリ向けの Application Insights

*Application Insights はプレビュー段階です。*

監視を開始するには、J2EE サーバーで既に実行されている web アプリケーションがあれば、 [Appliction Insight](app-insights-overview.md) コードを変更したり、プロジェクトを再コンパイルする必要はありません。 この方法では、サーバーに送信される HTTP 要求、未処理の例外、パフォーマンス カウンターに関する情報が取得されます。

サブスクリプションが必要 [Microsoft Azure](https://azure.com)します。
> [AZURE.NOTE] このページの手順では、実行時の Web アプリに SDK を追加します。 これは、ソース コードの更新やリビルドを実行したくない場合に便利です。 ただし、可能な場合をお勧めする [SDK ソース コードを追加する](app-insights-java-get-started.md) 代わりにします。 追加すると、ユーザーの利用状況を追跡するためのコードの記述などで選択の幅が広がります。

## 1.Application Insights のインストルメンテーション キーを取得する

1. ログに、 [Microsoft Azure ポータル](https://portal.azure.com)
2. 新しい Application Insights リソースを作成します。

    ![[+] をクリックし、](./media/app-insights-java-live/01-create.png)
3. アプリケーションの種類を [Java Web アプリケーション] に設定します。

    ![名前を入力し、](./media/app-insights-java-live/02-create.png)
4. 新しいリソースのインストルメンテーション キーを見つけます。 このキーは、後でコード プロジェクトに貼り付けます。

    ![新しいリソース概要で、](./media/app-insights-java-live/03-key.png)

## 2.SDK のダウンロード

1. ダウンロード、 [Application Insights SDK for Java](https://azuredownloads.blob.core.windows.net/applicationinsights/sdk.html)します。
2. サーバーで、プロジェクトのバイナリの読み込み元のディレクトリに SDK の内容を展開します。Tomcat を使用している場合は、通常ます `webapps\ < your_app_name > \WEB-INF\lib`


## 3.Application Insights の xml ファイルを追加する

SDK を追加したフォルダーに ApplicationInsights.xml を作成します。 そのファイルに次の XML を入力します。

インストルメンテーション キーについては、Azure ポータルで入手したキーを使用してください。

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings" schemaVersion="2014-05-30">
    
    
      
    
      <InstrumentationKey>** Your instrumentation key **</InstrumentationKey>
    
    
      
    
      <TelemetryModules>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
      </TelemetryModules>
    
      
      
    
      <TelemetryInitializers>
        <Add   type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
        <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>
    
      </TelemetryInitializers>
    </ApplicationInsights>

* インストルメンテーション キーは、テレメトリのすべての項目と共に送信されます。インストルメンテーション キーを受け取った Application Insights は、リソース内にこのキーを表示します。
* HTTP 要求コンポーネントはオプションです。 このコンポーネントは、要求と応答時間に関するテレメトリをポータルに自動的に送信します。
* イベントの関連付けは、HTTP 要求コンポーネントに対する追加の操作です。 この操作では、サーバーで受信した各要求に識別子を割り当てた後、この識別子をテレメトリのすべての項目に "Operation.Id" プロパティとして追加します。 フィルターを設定して、各要求に関連付けられているテレメトリに関連付けることができます [診断検索](app-insights-diagnostic-search.md)します。


## 4.HTTP フィルターを追加する

プロジェクトの web.xml ファイルを見つけて開きます。アプリケーション フィルターが構成されている web-app ノードの下に次のコード スニペットをマージします。

最も正確な結果を得るためには、他のすべてのフィルターの前にこのフィルターをマップする必要があります。

    <filter>
      <filter-name>ApplicationInsightsWebFilter</filter-name>
      <filter-class>
        com.microsoft.applicationinsights.web.internal.WebRequestTrackingFilter
      </filter-class>
    </filter>
    <filter-mapping>
       <filter-name>ApplicationInsightsWebFilter</filter-name>
       <url-pattern>/*</url-pattern>
    </filter-mapping>

## 5.Web アプリを再起動する

## 6.Application Insights でのテレメトリを表示する

Application Insights リソースに戻ります [Microsoft Azure ポータル](https://portal.azure.com)します。

HTTP 要求データが概要ブレードに表示されます  (表示されない場合は、数秒待ってから [最新の情報に更新] をクリックします)。

![サンプル データ](./media/app-insights-java-live/5-results.png)


任意のグラフをクリックして、より詳細なメトリックを表示します。

![](./media/app-insights-java-live/6-barchart.png)



要求のプロパティを表示すると、その要求に関連付けられているテレメトリ イベント (要求や例外など) が表示されます。

![](./media/app-insights-java-live/7-instance.png)


[メトリックの詳細についてを説明します。](app-insights-metrics-explorer.md)



## 次のステップ

* [Web ページにテレメトリを追加](app-insights-web-track-usage.md) モニター ページ ビューやユーザー メトリックをします。
* [Web テストを設定](app-insights-monitor-web-app-availability.md) を確認する、アプリケーションが動作していて応答します。
* [ログ トレースをキャプチャします。](app-insights-java-trace-logs.md)
* [イベントおよびログを検索](app-insights-diagnostic-search.md) 問題の診断に役立ちます。







