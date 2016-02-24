<properties 
    pageTitle="以前の Visual Studio Team Services バージョンの Application Insights からのアップグレード" 
    description="既存のプロジェクトのアップグレード"
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
    ms.date="06/19/2015" 
    ms.author="awills"/>
 
# 以前の Visual Studio Team Services バージョンの Application Insights からのアップグレード

このドキュメントは、Visual Studio Team Services に含まれていた以前のバージョンの Application Insights を今でも使用しているプロジェクトがある場合にのみ役立ちます。 このバージョンはいずれ使用できなくなるため、Microsoft Azure 内のサービスである新しいバージョンにアップグレードすることをお勧めします。

## 自分が所有しているバージョン

Visual Studio 2013 Update 3 以降を使用してプロジェクトに Application Insights を追加していれば、ほとんどの場合は新しい Azure バージョンを使用しています。

ApplicationInsights.config を開きます。 `ActiveProfile` および `Profiles` ノードがある場合は以前のバージョンであり、アップグレードをする必要があります。

または、Visual Studio ソリューション エクスプ ローラーでプロジェクトを確認し、[参照設定] で Microsoft.ApplicationInsights を選択します。 [プロパティ] ウィンドウで、バージョンを特定します。 これが 0.12 未満の場合、アップグレードが必要です。

## Visual Studio プロジェクトがある場合

1. Visual Studio 2013 Update 3 以降でプロジェクトを開きます。
2. ApplicationInsights.config を削除します。 
3. プロジェクトから Application Insights NuGet パッケージを削除します。 
削除するには、ソリューション エクスプローラーでプロジェクトを右クリックし、[NuGet パッケージの管理] を選択します。

    ![](./media/app-insights-upgrade-vso-azure/nuget.png)
4. AppInsightsAgent フォルダーと格納されているファイルを削除します。

    ![](./media/app-insights-upgrade-vso-azure/folder.png)

5. ServiceDefinition.csdef と ServiceConfiguration.csfg からすべての Microsoft.AppInsights の設定名と値を削除します。

    ![](./media/app-insights-upgrade-vso-azure/csdef.png)
4. SDK: プロジェクトを右クリックし、 [Application Insights の追加] を選択][greenbrown]します。 これによって SDK がプロジェクトに追加され、さらに Azure で新しい Application Insights リソースが作成されます。
5. ログ: コードに LogEvent() など、以前の API の呼び出しが含まれている場合、それらはソリューションをビルドしようとしたときに見つかります。 ように更新 [新しい API を使用して][track]します。
6. Web ページ: プロジェクトに Web ページが含まれている場合、<head> セクション内のスクリプトを置き換えます。 一般に Views\Shared\_Layout.cshtml などのマスター ページが 1 つのコピーがあります。 [Azure で Application Insights リソースの [クイック スタート] ブレードから新しいスクリプトを取得][usage]します。 
Web ページが logEvent や logPage などの本文にテレメトリの呼び出しを含める場合 [新しい API を使用するように更新][api]します。
7. サーバー モニター: アプリが IIS で実行されているサービスの場合は、サーバーから Microsoft Monitoring Agent をアンインストールし、 [Application Insights Status Monitor をインストール][redfield]します。
8. Web テスト: web 可用性テストを使用していた場合 [新しいポータルで作成し直す][availability], 、そのアラートがあります。
9. アラート: を設定 [メトリックに対するアラート][alerts] Azure ポータルで。


## Java Web サービスがある場合

1. サーバー コンピューターで、Web サービスのスタートアップ ファイルから APM エージェントへの参照を削除して以前のエージェントを無効にします。 On a TomCat server, edit Catalina.bat. On a JBoss server, edit Run.bat. 
2. Web サービスを再起動します。
3. Microsoft Azure ポータルで [新しい Application Insights リソースを追加][java]します。 開発用コンピューターで追加 [Java SDK][java] 、web プロジェクトにします。
4. Web ページの <head> セクションのスクリプトを置き換えます  (サーバー側には 1 つのコピーだけが含まれている可能性があります)。[Azure の新しい Application Insights リソースの [クイック スタート] ブレードから新しいスクリプトを取得][usage]します。 
Web ページが logEvent や logPage などの本文にテレメトリの呼び出しを含める場合 [新しい API を使用するように更新][track]します。
8. Web テスト: web 可用性テストを使用していた場合 [新しいポータルで作成し直す][availability], 、そのアラートがあります。
9. アラート: を設定 [メトリックに対するアラート][alerts] Azure ポータルで。



<!--Link references-->

[alerts]: app-insights-alerts.md
[api]: app-insights-api-custom-events-metrics.md
[availability]: app-insights-monitor-web-app-availability.md
[greenbrown]: app-insights-start-monitoring-app-health-usage.md
[java]: app-insights-java-get-started.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 
