<properties 
    pageTitle="Eclipse で Java 用に Application Insights を使う" 
    description="Eclipse プラグインを使用して、Java Web サイトのパフォーマンスと利用状況を Application Insights で監視できるようにします" 
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
    ms.date="11/25/2015" 
    ms.author="awills"/>


# Eclipse で Java 用に Application Insights を使う

Application Insights SDK は、利用状況とパフォーマンスを分析できるようにするために、Java Web アプリケーションからテレメトリを送信します。 Application Insights 用 Eclipse プラグインは、プロジェクトに SDK を自動的にインストールします。これにより、すぐに使用可能なテレメトリに加えて、カスタムのテレメトリの作成に使用できる API が提供されます。


## 前提条件

現在、プラグインは、Eclipse のMaven プロジェクトと動的 Web プロジェクトに対応しています 
([[Application Insights を他の種類の Java プロジェクトの ][java].)

必要なものは次のとおりです。

* Oracle JRE 1.6 以降。
* サブスクリプションを [Microsoft Azure](http://azure.microsoft.com/)します。 (、で作業を開始できます [無料評価版](http://azure.microsoft.com/pricing/free-trial/).)
* [Eclipse IDE for Java EE Developers](http://www.eclipse.org/downloads/), Indigo 以降。
* Windows 7 以降または Windows Server 2008 以降。

## Eclipse に SDK をインストールする (1 回限り)

この操作は、コンピューターごとに 1 回行う必要があります。 この手順では、SDK を各動的 Web プロジェクトに追加できるツールキットをインストールします。

1. Eclipse で、[Help]、[Install New Software] の順にクリックします。

    ![[Help]、](./media/app-insights-java-eclipse/0-plugin.png)

2. SDK は Azure Toolkit の下の http://dl.windowsazure.com/eclipse です。
3. **[Contact all update sites]** チェック ボックスをオフにします。

    ![Application Insights SDK の場合、](./media/app-insights-java-eclipse/1-plugin.png)

Java プロジェクトごとに、残りの手順に従います。

## Azure で Application Insights のリソースを作成する

1. サインイン、 [Azure ポータル](https://portal.azure.com)します。
2.

## アプリケーションの正常性と利用状況の監視

1. Web プロジェクトのコンテキスト メニューから Application Insights を追加します。

    ![新しいリソース概要で、](./media/app-insights-java-eclipse/02-context-menu.png)

2. Azure ポータルで入手したインストルメンテーション キーを貼り付けます。

    ![新しいリソース概要で、](./media/app-insights-java-eclipse/03-ikey.png)


このキーは、テレメトリのすべての項目と共に送信され、リソース内にこれを表示するように Application Insights に指示します。

## アプリケーションを実行してメトリックを表示する

アプリケーションを実行します。

Microsoft Azure の Application Insights リソースに戻ります。

HTTP 要求データが概要ブレードに表示されます  (表示されない場合は、数秒待ってから [最新の情報に更新] をクリックします)。

![サーバーの応答、要求の数、エラー](./media/app-insights-java-eclipse/5-results.png)


任意のグラフをクリックして、より詳細なメトリックを表示します。

![名前別の要求数](./media/app-insights-java-eclipse/6-barchart.png)


[メトリックの詳細についてを説明します。][metrics]



要求のプロパティを表示すると、その要求に関連付けられているテレメトリ イベント (要求や例外など) が表示されます。

![この要求のすべてのトレース](./media/app-insights-java-eclipse/7-instance.png)


## クライアント側のテレメトリ

クイック スタートブレードで、[マイ Web ページを監視するためのコードを取得する] をクリックします。

![アプリの概要ブレードを選択し、クイック スタート マイ web ページを監視するためのコードを取得します。 スクリプトをコピーします。](. media/app-insights-java-eclipse/02-monitor-web-page.png/)

HTML ファイルの先頭にコード スニペットを挿入します。

#### クライアント側のデータの表示

更新された Web ページを開いて使用します。 1 ～ 2 分待ってから、Application Insights に戻り、[使用状況] ブレードを開きます  ([概要] ブレードで下へスクロールし、[使用状況] をクリックします)。

[使用状況] ブレードに、ページ ビュー、ユーザー、セッションのメトリックが表示されます。

![セッション、ユーザー、ページ ビュー](./media/app-insights-java-eclipse/appinsights-47usage-2.png)

[クライアント側のテレメトリの設定について説明します。][usage]

## アプリケーションの発行

次に、サーバーにアプリを発行してユーザーがアプリを使用できるようにし、ポータルに表示されるテレメトリを監視します。

* アプリケーションがこれらのポートにテレメトリを送信できるようにファイアウォールが設定されていることを確認します。

 * dc.services.visualstudio.com:443
 * dc.services.visualstudio.com:80
 * f5.services.visualstudio.com:443
 * f5.services.visualstudio.com:80

* Windows サーバーに次のものをインストールします。

 * [Microsoft Visual C 再頒布可能パッケージ](http://www.microsoft.com/download/details.aspx?id=40784)

    (これにより、パフォーマンス カウンターが有効になります。)

## 例外と要求エラー

未処理の例外は、自動的に収集されます。

![](./media/app-insights-java-eclipse/21-exceptions.png)

その他の例外に関するデータを収集するには 2 つのオプションがあります。

* [TrackException への呼び出しをコードに挿入](app-insights-api-custom-events-metrics.md#track-exception)します。
* [Java エージェントをサーバーにインストール](app-insights-java-agent.md)します。 監視するメソッドを指定します。


## メソッドの呼び出しと外部依存関係の監視

[Java エージェントをインストール](app-insights-java-agent.md) 指定された内部メソッドと、JDBC を通じて呼び出しタイミング データと共にログに記録します。


## パフォーマンス カウンター

概要ブレードを下へスクロールし、をクリックして、  **サーバー** を並べて表示します。 一連のパフォーマンス カウンターが表示されます。


![下にスクロールして ](./media/app-insights-java-eclipse/11-perf-counters.png)

### パフォーマンス カウンター コレクションをカスタマイズする

パフォーマンス カウンターの標準セットのコレクションを無効にするには、ApplicationInsights.xml ファイルのルート ノードの下に次のコードを追加します。

    <PerformanceCounters>
       <UseBuiltIn>False</UseBuiltIn>
    </PerformanceCounters>

### 追加のパフォーマンス カウンターを収集する

収集する追加のパフォーマンス カウンターを指定できます。

#### JMX カウンター (Java 仮想マシンによって公開されます)

    <PerformanceCounters>
      <Jmx>
        <Add objectName="java.lang:type=ClassLoading" attribute="TotalLoadedClassCount" displayName="Loaded Class Count"/>
        <Add objectName="java.lang:type=Memory" attribute="HeapMemoryUsage.used" displayName="Heap Memory Usage-used" type="composite"/>
      </Jmx>
    </PerformanceCounters>

*   `displayName` – Application Insights ポータルに表示される名前です。
*   `objectName` – [JMX オブジェクトの名前。
*   `属性` -取得する JMX オブジェクト名の属性
*   `型` (省略可能) - JMX オブジェクトの属性の型。
 *  既定値: int、long などの単純型。
 *  `複合`: パフォーマンス カウンター データは、"attribute.data"形式では
 *  `テーブル`: パフォーマンス カウンター データはテーブル行形式です



#### Windows パフォーマンス カウンター

各 [Windows パフォーマンス カウンターを](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx) はカテゴリのメンバー (フィールドがクラスのメンバーである同様)。 カテゴリについては、グローバルに設定することも、数字または名前付きインスタンスを設定することもできます。

    <PerformanceCounters>
      <Windows>
        <Add displayName="Process User Time" categoryName="Process" counterName="%User Time" instanceName="__SELF__" />
        <Add displayName="Bytes Printed per Second" categoryName="Print Queue" counterName="Bytes Printed/sec" instanceName="Fax" />
      </Windows>
    </PerformanceCounters>

*   displayName - Application Insights ポータルに表示される名前。
*   categoryName - このパフォーマンス カウンターが関連付けられているパフォーマンス カウンターのカテゴリ (パフォーマンス オブジェクト)。
*   counterName - パフォーマンス カウンターの名前。
*   instanceName - パフォーマンス カウンター カテゴリ インスタンスの名前、または空の文字列 ("") (カテゴリにインスタンスが 1 つ含まれている場合)。 CategoryName が Process、および収集するパフォーマンス カウンターが現在の JVM プロセスからでは、アプリが実行されている場合、指定 `「_ _self _ _」`します。

パフォーマンス カウンターはでカスタム メトリックとして表示 [メトリックス エクスプ ローラー ][metrics]します。

![](./media/app-insights-java-eclipse/12-custom-perfs.png)


### Unix パフォーマンス カウンター

* [Application Insights プラグインを使用して collectd をインストール](app-insights-java-collectd.md) をさまざまなシステムとネットワークに関するデータを取得します。

## 可用性 Web テスト

Application Insights では、Web サイトを定期的にテストして、Web サイトが正常に動作および応答していることを確認できます。 [[可用性] を設定する][availability], 、下にスクロールして [可用性] をクリックします。

![下にスクロール、](./media/app-insights-java-eclipse/31-config-web-test.png)

応答時間のグラフが表示されます。また、サイトがダウンしている場合はメールによる通知を受け取ります。

![Web テストの例](./media/app-insights-java-eclipse/appinsights-10webtestresult.png)

[可用性 web テストについて説明します。][availability]

## 診断ログ

トレース用に Logback または Log4J (v1.2 または v2.0) を使用している場合は、トレース ログを自動的に Application Insights に送信して、Application Insights でトレース ログを探索および検索できます。

[詳細については、診断ログ ][javalogs]

## カスタムのテレメトリ

Java Web アプリケーションに数行のコードを挿入して、ユーザーの行動を調べたり、問題の診断に役立つ情報を取得したりすることができます。

コードは、Web ページの JavaScript とサーバー側 Java のどちらにも挿入できます。

[カスタムのテレメトリ ][track]



## 次のステップ

#### 問題の検出と診断

* [Web クライアント テレメトリ ][usage] web クライアントからパフォーマンス テレメトリを取得します。
* [Web の設定は、][availability] を確認する、アプリケーションが動作していて応答します。
* [イベントおよびログ ][diagnostic] 問題の診断に役立ちます。
* [[Javalogs] の Log4J または Logback トレースをキャプチャします。][javalogs]

#### 利用状況を追跡する

* [Web クライアント テレメトリ ][usage] モニター ページ ビューや基本的なユーザー メトリックをします。
* [カスタム イベントおよびメトリックス ][track] 、アプリケーションの使用状況、クライアントとサーバーの両方について説明します。





[availability]: app-insights-monitor-web-app-availability.md 
[diagnostic]: app-insights-diagnostic-search.md 
[java]: app-insights-java-get-started.md 
[javalogs]: app-insights-java-trace-logs.md 
[metrics]: app-insights-metrics-explorer.md 
[track]: app-insights-api-custom-events-metrics.md 
[usage]: app-insights-web-track-usage.md 

