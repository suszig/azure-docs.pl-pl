<properties 
    pageTitle="サービス メトリックの監視" 
    description="Azure でグラフの監視をカスタマイズする方法について説明します。" 
    authors="stepsic-microsoft-com" 
    manager="ronmart" 
    editor="" 
    services="azure-portal"
documentationCenter=""/>

<tags 
    ms.service="azure-portal" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2015" 
    ms.author="stepsic"/>


# サービス メトリックの監視

すべての Azure サービスでは、サービスの正常性、パフォーマンス、可用性、使用状況を監視するための重要なメトリックが追跡されます。 Azure ポータルでこれらのメトリックを表示して、使用することも、 [REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx) または [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) メトリックの完全なセットをプログラムでアクセスします。

サービスによっては、メトリックを表示するために診断を有効にする必要がある場合があります。 また、Virtual Machines などのサービスでは、基本的なメトリック セットを利用できますが、使用頻度の高いメトリックをすべて有効にする必要もあります。 参照してください [監視を有効にし、診断](insights-how-to-use-diagnostics.md) の詳細。

## 監視グラフの使用

選択した任意の期間における任意のメトリックのグラフを作成できます。

1. [Azure ポータル](https://portal.azure.com/), 、] をクリックして **参照**, 、しするリソースの監視に必要とします。

2. **[監視]** セクションには、各 Azure リソースにとって最も重要なメトリックが含まれています。 Web アプリにはたとえば、 **要求とエラー**, ここで、仮想マシンは、 **CPU 使用率の** と **ディスクの読み取りと書き込み**:
    ![監視レンズ](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)

3. いずれかのグラフをクリックすると、**[メトリック]** ブレードが表示されます。 ブレードには、グラフに加えて、メトリック (選択した時間範囲の平均、最小値、最大値など) の集計を示すテーブルが表示されます。 次に、リソースのアラート ルールです。
    ![[メトリック] ブレード](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)

4. 表示される行をカスタマイズするには、グラフ上の **[編集]**、または [メトリック] ブレード上の **[グラフの編集]** コマンドをクリックします。

5. [クエリの編集] ブレードでは、次の 3 つの操作を実行できます。
    - 時間範囲を変更する
    - 棒グラフと折れ線グラフを切り替える
    - さまざまなメトリックを選択します。
    ![クエリの編集](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)

6. 期間の変更は簡単で、別の範囲 (**[過去 1 時間]** など) を選択して、ブレードの下部にある **[保存]** をクリックするだけです。 **[カスタム]** も選択できます。これを使用すると、過去 2 週間の任意の期間を選択できます。 たとえば、2 週間全体を表示することも、昨日の 1 時間だけを表示することもできます。 別の期間を指定するには、テキスト ボックスに入力します。
    ![カスタム期間](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)

7. 期間の下で、グラフに表示するメトリックを任意の数だけ選択できます。

8. [保存] をクリックすると、その特定のリソースに対する変更が保存されます。 たとえば、2 つの仮想マシンがある場合、一方でグラフを変更しても、もう一方には影響しません。

## サイド バイ サイドのグラフの作成

ポータルの強力なカスタマイズ機能を使用して、必要な数のグラフを追加できます。

1. **...** ブレードのクリックの上部にあるメニュー **タイルを追加する**:  
    ![Add Menu](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. そしてからグラフを選択できます、 **ギャラリー** 、画面の右側にあります。
    ![ギャラリー](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. 必要なメトリックが表示されない場合は、いつでも、事前設定されたメトリックの 1 つを追加し、**[編集]** でグラフを編集して必要なメトリックを表示できます。

## 使用量クォータの監視

ほとんどのメトリックでは時間の経過に伴う傾向が示されますが、使用量クォータなどの特定のデータでは、しきい値と一緒に特定の時点の情報が表示されます。

また、クォータを含むリソースでは、ブレードに使用量クォータを表示することもできます。

![使用方法](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

同様に使用できるメトリック、 [REST API](https://msdn.microsoft.com/library/azure/dn931963.aspx) または [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) 使用量クォータの完全なセットをプログラムでアクセスします。

## 次のステップ

* [警告通知を受け取る](insights-receive-alert-notifications.md) メトリックがしきい値を超えたときにします。
* [監視を有効にし、診断](insights-how-to-use-diagnostics.md) 、サービスに関する詳細な頻度の高いメトリックを収集します。
* [インスタンス数を自動的にスケール変更](insights-how-to-scale.md) 、サービスの可用性と応答性を確認します。
* [アプリケーションのパフォーマンスを監視](insights-perf-analytics.md) 、クラウド内に、コードが実行する方法に正確に理解したい場合。
* 使用 [JavaScript アプリと web ページ向けの Application Insights](../app-insights-web-track-usage.md) web ページを参照しているブラウザーに関するクライアント分析を取得します。
* [可用性と任意の web ページの応答性監視](../app-insights-monitor-web-app-availability.md) のかどうか、ページは停止検索できるように、Application Insights を使用します。





