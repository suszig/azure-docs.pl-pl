<properties
    pageTitle="Visual Studio Application Insights を使ってみる | Microsoft Azure"
    description="オンプレミスまたは Microsoft Azure Web アプリケーションの使用状況、可用性、パフォーマンスを Visual Studio Application Insights で分析します。"
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="11/17/2015"
    ms.author="awills"/>

# Visual Studio Application Insights を使ってみる

*Application Insights はプレビュー段階です。*

問題を検出し、問題を解決し、アプリケーションの継続的に改善します。 実際のアプリケーションの問題を今すぐ診断する。 ユーザーがそれで何を行っているかを理解します。

構成は非常に簡単であり、数分以内で結果を確認できます。

現時点では、iOS、Android、Windows アプリ、J2EE と ASP.NET Web アプリケーション、WCF サービスをサポートしています。 Web アプリは、Azure 上か、お使いのオンプレミス サーバーで実行できます。 JavaScript SDK は任意の Web ページで実行できます。

## 作業開始

次の図に示すエントリ ポイントを、任意の順序で自由に組み合わせて開始します。 適切なパスを選択します。

Application Insights の動作をするには、利用統計情報を送信するアプリに SDK を追加することで、 [Azure ポータル](http://portal.azure.com)します。 サポートされているプラットフォーム、言語、IDE の多様な組み合わせに対応してさまざまな SDK が用意されています。

アカウントが必要 [Microsoft Azure](http://azure.com)します。 既にお持ちの組織のグループ アカウントにアクセスすることも、従量課金アカウントを取得することもできます。 Application Insights には Free レベルが用意されているため、アプリの人気が出るまでお支払いいただく必要はありません。 レビュー、 [料金のページ](https://azure.microsoft.com/pricing/details/application-insights/)します。

使用目的 | 操作 | 取得内容
---|---|---
 <a href="app-insights-asp-net.md">![ASP.NET](./media/app-insights-get-started/appinsights-gs-i-01-perf.png)</a> | <a href="app-insights-asp-net.md">Application Insights SDK を web プロジェクトに追加します。</a> <br/> ![取得します。](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-asp-net.md">![パフォーマンスと利用状況の監視](./media/app-insights-get-started/appinsights-gs-r-01-perf.png)</a>
<a href="app-insights-monitor-performance-live-website-now.md">![既にライブの ASP.NET サイト](./media/app-insights-get-started/appinsights-gs-i-04-red2.png)</a><br/><a href="app-insights-monitor-performance-live-website-now.md">![依存関係とパフォーマンスの監視](./media/app-insights-get-started/appinsights-gs-i-03-red.png)</a>|<a href="app-insights-monitor-performance-live-website-now.md">IIS サーバーに Status Monitor をインストールします。</a> <br/> ![取得します。](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-monitor-performance-live-website-now.md">![ASP.NET の依存関係の監視](./media/app-insights-get-started/appinsights-gs-r-03-red.png)</a>
<a href="insights-perf-analytics.md">![Azure の web アプリまたは VM](./media/app-insights-get-started/appinsights-gs-i-10-azure.png)</a>|<a href="insights-perf-analytics.md">Azure web アプリまたは VM で Insights を有効にします。</a> <br/> ![取得します。](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="insights-perf-analytics.md">![依存関係とパフォーマンスの監視](./media/app-insights-get-started/appinsights-gs-r-03-red.png)</a>
<a href="app-insights-java-get-started.md">![Java](./media/app-insights-get-started/appinsights-gs-i-11-java.png)</a>|<a href="app-insights-java-get-started.md">SDK を Java プロジェクトに追加します。</a><br/>![取得します。](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-java-get-started.md">![パフォーマンスと利用状況の監視](./media/app-insights-get-started/appinsights-gs-r-10-java.png)</a>
<a href="app-insights-web-track-usage.md">![JavaScript](./media/app-insights-get-started/appinsights-gs-i-02-usage.png)</a>|<a href="app-insights-web-track-usage.md">Web ページに Application Insights のスクリプトを挿入します。</a><br/>![取得します。](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-web-track-usage.md">![ページ ビューとブラウザーのパフォーマンス](./media/app-insights-get-started/appinsights-gs-r-02-usage.png)</a>
<a href="app-insights-monitor-web-app-availability.md">![可用性](./media/app-insights-get-started/appinsights-gs-i-05-avail.png)</a>|<a href="app-insights-monitor-web-app-availability.md">Web テストを作成します。</a><br/>![取得します。](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-monitor-web-app-availability.md">![可用性](./media/app-insights-get-started/appinsights-gs-r-05-avail.png)</a>
<a href="app-insights-platforms.md">![iOS、Android および Windows デバイス](./media/app-insights-get-started/appinsights-gs-i-07-device.png)</a>|<a href="http://hockeyapp.net">HockeyApp を使用します。</a><br/>![取得します。](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="http://hockeyapp.net">![クラッシュと使用状況データ](./media/app-insights-get-started/appinsights-gs-r-06-device.png)</a>

## サポートとフィードバック


* 質問および問題点:
 * [トラブルシューティング][qna]
 * [MSDN フォーラム](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)
 * [StackOverflow](http://stackoverflow.com/questions/tagged/ms-application-insights)
* バグ:
 * [接続](https://connect.microsoft.com/VisualStudio/Feedback/LoadSubmitFeedbackForm?FormID=6076)
* ご提案:
 * [ユーザーの声](http://visualstudio.uservoice.com/forums/121579-visual-studio/category/77108-application-insights)
* コード サンプル
 * [コード サンプル](app-insights-code-samples.md)



## <a name="video"></a>ビデオ


> [AZURE.VIDEO 218]

> [AZURE.VIDEO usage-monitoring-application-insights]

> [AZURE.VIDEO performance-monitoring-application-insights]



<!--Link references-->

[qna]: app-insights-troubleshoot-faq.md


