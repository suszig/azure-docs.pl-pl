<properties 
    pageTitle="Manage pricing and quota for Application Insights" 
    description="必要な価格プランを選択します" 
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
    ms.date="11/18/2015" 
    ms.author="awills"/>


# Application Insights の価格とクォータの管理

*Application Insights はプレビュー段階です。*

[[料金] 料金][pricing] の [Visual Studio Application Insights ][start] アプリケーションごとのデータ量に基づきます。 真の意味で Free レベルがあり、いくつかの制限はありますが、ほとんどの機能を利用できます。

各 Application Insights リソースは個々のサービスとして課金され、Azure のサブスクリプションの課金内容に加えられます。

[[料金] 料金プランを参照してください][pricing]します。

## Application Insights リソースのクォータと料金プランの確認

アプリケーション リソースの設定からクォータと価格のブレードを開くことができます。

![設定の選択、クォータと価格](./media/app-insights-pricing/01-pricing.png)

選択した料金プランは次に影響します。

* [月間クォータ](#monthly-quota) -1 か月あたりを分析する利用統計情報の量。
* [データ レート](#data-rate) -アプリからデータを処理できる最大転送率。
* [保有](#data-retention) - 表示するための Application Insights ポータルで長い形式のデータを保持します。
* [連続エクスポート](#continuous-export) - 他のツールやサービスにデータをエクスポートするかどうか。

これらの制限は、Application Insights のリソースごとに個別に設定されます。

### 無料の Premium 評価版

最初に新しい Application Insights リソースを作成するときは、Free レベルから開始します。

いつでも 30 日間無料の Premium 評価版に切り替えることができます。 これにより、Premium レベルを体験できます。 30 日が経過すると、明示的に別のレベルを選択していなければ、以前のレベルに自動的に戻ります。 評価期間中はいつでもご希望のレベルを選択できますが、30 日の期間が終了するまでは無料試用版を取得できます。


## 月間クォータ

* 毎月、アプリケーションは、指定された量のテレメトリを Application Insights に送ることができます。 参照してください、 [料金プランの ][pricing] の実際の数値。
* クォータは、選択した価格レベルによって異なります。
* クォータは、毎月 1 日の午前 0 時 UTC からカウントされます。
* データ ポイントのグラフは、今月クォータのどのくらいが使用されたかを示しています。
* クォータは、*データ ポイント*単位で測定されます。単一のデータ ポイントは、追跡メソッドの 1 つの呼び出しで、コードで明示的に呼び出されるか、標準テレメトリ モジュールのいずれかによって呼び出されます。 データ ポイントには次の項目が含まれます。
 * 『 各行 [診断検索](app-insights-diagnostic-search.md)します。
 * 生各測定値、 [メトリック](app-insights-metrics-explorer.md) パフォーマンス カウンターなどです。 (グラフに表示されるポイントは、通常は複数の生のデータ ポイントを集計したものです)
 * 上の各ポイント、 [web テスト (可用性)](app-insights-monitor-web-app-availability.md) グラフ。
* *セッション データ*は、クォータにカウントされません。 これには、ユーザー、セッション、環境、デバイスのデータの数が含まれます。


### 超過料金

アプリケーションの送信量が月間クォータを超える場合、次の操作を実行できます。

* 追加データ分を支払います。 参照してください、 [料金プランの ][pricing] 詳細です。 このオプションは、事前に選択できます。 このオプションは、無料の価格レベルでは選択できません。
* 価格レベルをアップグレードします。
* 何もしません。 セッション データは引き続き記録されますが、それ以外のデータは、診断の検索やメトリックス エクスプローラーに表示されません。


### 送信するデータ量はどのくらいですか。

価格ブレードの下部にあるグラフは、アプリケーションのデータ ポイント量を示しています。これは、データ ポイントの種類でグループ化されます  (このグラフはメトリック エクスプローラーで作成することもできます)。

![価格ブレードの下部](./media/app-insights-pricing/03-allocation.png)

グラフをクリックすると、詳細が表示されます。時間範囲の詳細を入手するには、グラフをドラッグして [+] をクリックします。


## データ速度

月間クォータの他にも、データ速度での調整の上限もあります。 無料の [価格レベルが ][pricing] 上限は 200 のデータ ポイント数/秒 5 分間の平均し、有料層の 500/秒の平均を求める 1 分を超える。

次の 3 つのバケットは別々にカウントされます。

* [TrackTrace calls](app-insights-api-custom-events-metrics.md#track-trace) and [captured logs](app-insights-asp-net-trace-logs.md)
* [例外](app-insights-api-custom-events-metrics.md#track-exception), 、50 ポイント/秒によって制限されます。
* その他すべてのテレメトリ (ページ ビュー、セッション、要求、依存関係、メトリック、カスタム イベント、Web テストの結果)

アプリが数分にわたり制限を超えてデータを送信すると、データの一部が破棄される場合があります。 この状況が発生したことを通知する警告が表示されます。

### データ速度を下げるためのヒント

調整の上限に対処する方法を次に示します。

* Use [Sampling](app-insights-sampling.md). このテクノロジは、メトリックや、検索で関連するアイテム間を移動する機能を損なうことなく、データ レートを削減します。
* オフにしてがなくてもコレクション モジュール [ApplicationInsights.config を編集](app-insights-configuration-with-applicationinsights-config.md)します。 たとえば、パフォーマンス カウンターや依存関係のデータが重要ではないと判断した場合などに検討します。
* 事前集計メトリック。 TrackMetric への呼び出しをアプリに配置した場合、平均計算と測定のバッチの標準偏差を受け入れるオーバーロードを使用して、トラフィックを減らすことができます。 使用することも、 [パッケージを事前に集計](https://www.myget.org/gallery/applicationinsights-sdk-labs)します。


### 名前の制限

1.  アプリケーションに対して最大 200 の一意のメトリックの名前と 200 の一意のプロパティの名前。 メトリックには、イベントなどの他のデータ型の測定値と同様に TrackMetric を通じて送信されるデータが含まれます。 [メトリックとプロパティの名前 ][api] はデータ型にスコープが制限されず、インストルメンテーション キーごとにグローバルです。
2.  [プロパティ ][apiproperties] フィルター処理に使用することができ、グループ化のみがあるプロパティごとに一意の値を 100 未満にします。 一意の値が 100 を超えた後も、プロパティは検索とフィルタリングに使用できますが、フィルター処理には使用できなくなります。
3.  要求名やページの URL などの標準プロパティは、1 週間あたりの 1000 の一意な値に制限されます。 1000 の一意の値を超えると、追加の値は "その他の値" としてマークされます。 元の値は、全文テキスト検索とフィルタリングに引き続き使用できます。

## データの保持

価格レベルによって、データがポータルに保持される期間が決まるため、設定可能な時間範囲も判断できます。


* 生データ ポイント (つまり、診断検索で確認できるインスタンス) は、7 ～ 30 日間です。
* 集計されたデータ (つまり、メトリックス エクスプローラーに表示されるカウント、平均、その他の統計データ) は、1 分の詳細度であれば 30 日、(種類に応じて) 1 時間または 1 日の詳細度であれば少なくとも 13 か月の期間にわたって保持されます。




## Azure へのサブスクリプションの課金内容の確認

Application Insights の課金は Azure の課金内容に加えられます。 課金] か [Azure ポータルの [課金内容の Azure の詳細を表示、 [Azure Billing Portal](https://account.windowsazure.com/Subscriptions)します。

![サイド メニューで、](./media/app-insights-pricing/02-billing.png)

## 制限の概要

[AZURE.INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]





[api]: app-insights-api-custom-events-metrics.md 
[apiproperties]: app-insights-api-custom-events-metrics.md#properties 
[start]: app-insights-overview.md 
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/ 

