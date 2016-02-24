<properties 
    pageTitle="Application Insights で Docker アプリケーションを監視する" 
    description="Docker のパフォーマンス カウンター、イベント、および例外を、コンテナー化されたアプリからのテレメトリと共に Application Insights に表示できます。" 
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
    ms.date="12/01/2015" 
    ms.author="awills"/>
 
# Application Insights で Docker アプリケーションを監視する

ライフ サイクル イベントとパフォーマンス カウンターから [Docker](https://www.docker.com/) コンテナーは、Application Insights をグラフ化できます。 インストール、 [Application Insights](app-insights-overview.md) 、ホストとそのコンテナー内にイメージは、その他のイメージだけでなく、ホストのパフォーマンス カウンターに表示されます。

Docker で、すべての依存関係を備えた軽量コンテナーにアプリを配布します。 これらは、Docker エンジンを実行しているすべてのホスト コンピューターで実行します。

実行すると、 [Application Insights イメージ](https://hub.docker.com/r/microsoft/applicationinsights/) 、Docker ホストでは、これらのメリットが表示されます。

* ホストで実行されているすべてのコンテナーに関するライフサイクル テレメトリ (開始や停止など)。
* すべてのコンテナーのパフォーマンス カウンター。 CPU、メモリ、ネットワークの使用状況など。
* 場合する [Application Insights SDK をインストールされている](app-insights-java-live.md) コンテナーで実行されているアプリの場合、それらのアプリのすべてのテレメトリのコンテナーとホスト マシンを識別する追加のプロパティがあります。 たとえば、1 つ以上のホストで実行中のアプリのインスタンスがある場合、アプリのテレメトリをホスト別に簡単にフィルター処理できます。

![例](./media/app-insights-docker/00.png)


## Application Insights リソースを設定する

1. サインイン [Microsoft Azure ポータル](https://azure.com) ; アプリの Application Insights リソースを開くとまたは [、新しく作成](app-insights-create-new-resource.md)します。 

    *どのリソースを使用する必要がありますか。*ホスト上で実行されているアプリは、だれかによって開発されたかどうかは、する必要があります [Application Insights リソースを新規作成](app-insights-create-new-resource.md)します。 テレメトリの表示と分析はこの場所で行います  (アプリの種類には [その他] を選択します)。

    お役にする場合、アプリの開発者が、 [Application Insights SDK を追加](app-insights-java-live.md) それぞれにします。 すべてのアプリが 1 つのビジネス アプリケーションのコンポーネントである場合は、テレメトリを 1 つのリソースに送信するように構成することで、同じリソースを使用して Docker のライフサイクルとパフォーマンスのデータを表示できます。 

    3 番目のシナリオは、アプリの大半は自分で開発しているが、それらのテレメトリの表示には別のリソースを使用している場合です。 その場合は、Docker データ用の別個のリソースを作成できます。 

2.  Docker タイルの追加: 選択 **タイルを追加**, ギャラリーから、[Docker タイルをドラッグし、[クリックして **実行**します。 

    ![例](./media/app-insights-docker/03.png)


3. クリックして、 **Essentials** ドロップダウンをクリックし、インストルメンテーション キーをコピーします。 これを使用して SDK にテレメトリの送信先を指示します。


    ![example](./media/app-insights-docker/02-props.png)

すぐにまた戻ってテレメトリを見るので、ブラウザー ウィンドウはそのままにします。


## ホスト上で Application Insights モニターを実行する
 
テレメトリを表示する場所ができたので、テレメトリを収集して送信するコンテナー化されたアプリを設定できます。

1.  Docker ホストに接続します。 
2.  インストルメンテーション キーを次のコマンドで編集して実行します。
 
    ```

    docker run -v /var/run/docker.sock:/docker.sock -d microsoft/applicationinsights ikey=000000-1111-2222-3333-444444444
    ```

Docker ホストごとに 1 つの Application Insights イメージが必要です。 アプリケーションが複数の Docker ホストにデプロイされている場合は、上のコマンドをすべてのホストで繰り返します。

## アプリケーションを更新する

アプリケーションがインストルメント化された場合、 [Application Insights SDK for Java](app-insights-java-get-started.md), 、下にあるプロジェクトで ApplicationInsights.xml ファイルに次の行を追加、 `<TelemetryInitializers>` 要素。

```xml

    <Add type="com.microsoft.applicationinsights.extensibility.initializer.docker.DockerContextInitializer"/> 
```

これで、アプリから送信されるすべてのテレメトリ項目にコンテナーやホスト ID などの Docker 情報が追加されます。

## テレメトリの表示

Azure ポータルで Application Insights リソースに戻ります。

Docker タイルをクリックします。

特に Docker エンジンで他のコンテナーを実行している場合は、Docker コンテナーから到着するデータがすぐに表示されます 。


取得できるビューのいくつかを次に示します。

### ホスト、アクティビティ、イメージ別のパフォーマンス カウンター


![例](./media/app-insights-docker/10.png)


![例](./media/app-insights-docker/11.png)



詳細を表示するホストまたはイメージ名をクリックします。



ビューをカスタマイズするには、グラフまたはグリッド見出しをクリックするか、[グラフの追加] を使用します。 

[メトリックス エクスプ ローラーの詳細について](app-insights-metrics-explorer.md)します。

### Docker コンテナーのイベント


![例](./media/app-insights-docker/13.png)

個々 のイベントを調べるには、クリックして [検索](app-insights-diagnostic-search.md)します。 検索とフィルター 
必要なイベントを検索します。 イベントをクリックすると詳細情報が表示されます。
 
### コンテナー名別の例外
 

![例](./media/app-insights-docker/14.png)

### アプリのテレメトリに追加された Docker コンテキスト

AI SDK でインストルメント化されたアプリケーションから送信されたテレメトリを Docker コンテキスト付きで要求します。

![例](./media/app-insights-docker/16.png)

プロセッサ時間と使用可能メモリのパフォーマンス カウンター。Docker コンテナー名によってグループ化されています。


![例](./media/app-insights-docker/15.png)





## Q & A

*Docker からは取得できず、Application Insights からは取得できるものは何ですか*

* コンテナーおよびイメージ別のパフォーマンス カウンターの詳細情報です。
* コンテナーとアプリ データが 1 つのダッシュボードに統合されます。
* [テレメトリのエクスポート](app-insights-export-telemetry.md) Power BI や他のダッシュ ボードでは、データベースにさらに分析します。

*アプリ自体からテレメトリを取得するにはどうすればよいですか*

* Application Insights SDK をアプリにインストールします。 
学習の方法: [Java web アプリ](app-insights-java-get-started.md), 、[web アプリの Windows](app-insights-asp-net.md)します。

