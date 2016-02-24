<properties 
    pageTitle="Application Insights に関するトラブルシューティングと Q & A" 
    description="Visual Studio Application Insights について不明な点や問題点はありませんか? ここで解決してください。" 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/25/2015" 
    ms.author="awills"/>
 
# トラブルシューティングと Q & A - Application Insights for ASP.NET

## Application Insights と共に使用できるもの

[プラットフォームを確認][platforms]

## これは無料ですか。

* 無料の選択した場合は yes。 [価格レベル](app-insights-pricing.md)します。 ほとんどの機能と大量のデータが手に入ります。 
* Microsoft Azure に登録するには、クレジット カード データを登録する必要がありますが、他の有料の Azure のサービスを使用するか、明示的に有料レベルにアップグレードしない限り、料金は発生しません。
* アプリが Free レベルの月間クォータより多くのデータを送信すると、ログの記録を停止します。 このような場合は、料金の支払いを開始するか、月末にクォータがリセットされるまでお待ちください。
* 基本的な使用とセッション データは、クォータの対象になりません。
* 30 日間の無料トライアルが用意されています。この期間中、Premium の機能を無料でご利用いただけます。
* 各アプリケーション リソースには、個別のクォータがあり、それぞれに個別に価格レベルを設定します。

#### 料金を払うと何ができますか。

* 大きな [データの月間クォータ](http://azure.microsoft.com/pricing/details/application-insights/)します。
* 月間クォータを超えてデータの収集を続行するための、「超過」支払いオプション。 データがクォータを超えた場合は、1 MB ごとに課金されます。
* [連続エクスポート](app-insights-export-telemetry.md)します。

## SDK の追加

#### <a name="q01"></a>Visual Studio で Application Insights をプロジェクトに追加するオプションがありません

+ した確認 [Visual Studio 2013 Update 3 以降](http://go.microsoft.com/fwlink/?LinkId=397827)します。 Application Insights Tools がプレインストールされています。
+ このツールは、あらゆる種類のアプリケーションをサポートしているわけではありませんが、Application Insights SDK をプロジェクトに手動で追加することはできます。 使用 [ここ][windows]します。 


#### <a name="q02"></a>新しい Web プロジェクトが作成されましたが Application Insights が追加されませんでした

これは、次の場合に発生することがあります。

* Application Insights ポータルとの通信に失敗した場合。つまり、
* お客様のアカウントになんらかの問題があります。
* のみがある [サブスクリプションまたは新しいリソースを作成しようとしていたグループへの読み取りアクセス](app-insights-resources-roles-access-control.md)します。

対応策:

+ 適切な Azure アカウントのログイン資格情報を指定していることを確認してください。 新しいバージョンのツールでは、[新しいプロジェクト] ダイアログに表示される Microsoft Azure の資格情報と、Visual Studio の右上に表示される資格情報が異なる場合があります。
+ アクセスがあることを確認して、ブラウザーで、 [Azure ポータル](https://portal.azure.com)します。 設定を開き、制限がないかどうか確認してください。
+ [既存のプロジェクトに Application Insights を追加する][start]: ソリューション エクスプ ローラーで、プロジェクトを右クリックし、"Application Insights の追加] を選択
+ 機能しない場合は、次の [手動プロシージャ](app-insights-start-monitoring-app-health-usage.md) 、ポータルにリソースを追加し、SDK をプロジェクトに追加します。 

#### <a name="emptykey"></a>エラー「インストルメンテーション キーは空にできません」が発生しました

Application Insights をインストールしているとき、またはログ アダプターをインストールしているときに、何かの問題が発生した可能性があります。

ソリューション エクスプ ローラーで右クリック `ApplicationInsights.config` 選択 **Application Insights の更新**します。 Azure へのサインインを促すダイアログが表示されます。または、Application Insights のリソースを作成するか、既存のリソースを再利用します。


#### <a name="q14"></a>Application Insights によってどのような変更がプロジェクトに加えられますか?

詳細は、プロジェクトの種類によって異なります。 Web アプリケーションの場合: 


+ 次のファイルがプロジェクトに追加されます。

 + ApplicationInsights.config 
 + ai.js


+ Installs these NuGet packages:

 -  *Application Insights API* -コア API

 -  *Application Insights API for Web Applications* - サーバーからテレメトリを送信に使用されます

 -  *Application Insights API for JavaScript Applications* - クライアントからテレメトリを送信に使用されます

    The packages include these assemblies:

 - Microsoft.ApplicationInsights

 - Microsoft.ApplicationInsights.Platform

+ 次の項目を挿入します。

 - web.config

 - packages.config

+ (新しいプロジェクトのみを [Application Insights を既存のプロジェクトに追加する][start], 、手動で設定する必要がある)。これらを Application Insights リソース ID で初期化するためのスニペットを、クライアントとサーバーのコードに挿入します。 たとえば、MVC アプリでは、Views/Shared/_Layout.cshtml マスター ページにコードが挿入されます。

####<a name="NuGetBuild"></a> 表示される「NuGet パッケージが存在しない」、ビルド サーバーでは、開発用コンピューターで [ok] にビルドすべて

参照してください [NuGet パッケージの復元](http://docs.nuget.org/Consume/Package-Restore)
 [自動パッケージの復元](http://docs.nuget.org/Consume/package-restore/migrating-to-automatic-package-restore)します。

####<a name="FailUpdate"></a> 表示される「プロジェクト参照は、コンピューターにない NuGet パッケージ」と NuGet パッケージを 0.17 以降に更新した後にビルドしようとしています。

NuGet パッケージを 0.17 以降に更新した後に上記のエラーが発生した場合は、proj ファイルを編集し、残っている BCL ターゲットを削除する必要があります。

これを行うには、次の手順を実行します。

1. ソリューション エクスプローラーでプロジェクトを右クリックし、[プロジェクトのアンロード] を選択します。
2. Right-click on project again and choose Edit *yourProject.csproj* 
3. プロジェクト ファイルの一番下に移動しのような BCL ターゲットを削除します。 
    ```
    <Import Project="..\packages\Microsoft.Bcl.Build.1.0.14\tools\Microsoft.Bcl.Build.targets" Condition="Exists('..\packages\Microsoft.Bcl.Build.1.0.14\tools\Microsoft.Bcl.Build.targets')" />
      
      <Target Name="EnsureBclBuildImported" BeforeTargets="BeforeBuild" Condition="'$(BclBuildImported)' == ''">
      
        <Error Condition="!Exists('..\packages\Microsoft.Bcl.Build.1.0.14\tools\Microsoft.Bcl.Build.targets')" Text="This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see http://go.microsoft.com/fwlink/?LinkID=317567." HelpKeyword="BCLBUILD2001" />
        
        <Error Condition="Exists('..\packages\Microsoft.Bcl.Build.1.0.14\tools\Microsoft.Bcl.Build.targets')" Text="The build restored NuGet packages. Build the project again to include these packages in the build. For more information, see http://go.microsoft.com/fwlink/?LinkID=317568." HelpKeyword="BCLBUILD2002" />
        
      </Target>
      ```
4. ファイルを保存します。
5. Right-click on the project and choose Reload *yourProject.csproj*

## 以前のバージョンの SDK からアップグレードする方法。

参照してください、 [リリース ノート](app-insights-release-notes.md) アプリケーションの種類に適切な SDK のです。 


## データが表示されない

#### <a name="q03"></a>Application Insights が正常に追加された後でアプリケーションを実行したところ、ポータルにデータが表示されません

+ 概要ページで、検索タイルをクリックして診断検索を開いてください。 データはまずここに表示されます。
+ [更新] ボタンをクリックします。 ブレードは周期的に自動で更新されますが、手動でも更新できます。 時間範囲が広いと、更新間隔は長くなります。
+ Microsoft Azure のスタート画面で、サービス状態マップを確認してください。 アラート表示がある場合は、"OK" が表示されるまで待ってから、Application Insights アプリケーション ブレードをいったん閉じて開き直します。
+ さらに、確認 [状態ブログ](http://blogs.msdn.com/b/applicationinsights-status/)します。
+ ApplicationInsights.config を編集した場合は、TelemetryInitializers と TelemetryProcessors の構成を慎重に確認します。 不適切な名前が付けられた型またはパラメーターがあると、SDK によってデータが送信されない場合があります。

#### サーバーにアプリケーションを発行したのでデータがない

+ すべての Microsoft. ApplicationInsights DLL が Microsoft.Diagnostics.Instrumentation.Extensions.Intercept.dll と一緒にサーバーにコピーされたことを確認します。
+ ファイアウォールでは、dc.services.visualstudio.com と f5.services.visualstudio.com への発信トラフィック用に TCP ポート 80 と 443 を開く必要がある場合があります。
+ プロキシを使用して、企業ネットワークの外に送信、設定した場合 [defaultProxy](https://msdn.microsoft.com/library/aa903360.aspx) web.config
+ Windows Server 2008: 次の更新プログラムをインストールしておく: [KB2468871](https://support.microsoft.com/kb/2468871), 、[KB2533523](https://support.microsoft.com/kb/2533523), 、[KB2600217](https://support.microsoft.com/kb/2600217)します。


#### <a name="q04"></a>自分の web サイトの [利用状況分析] にデータが表示されません。

+ データは、Web ページのスクリプトによって取得されます。 既存の web プロジェクトに Application Insights を追加する場合 [を手動でスクリプトを追加する必要がある][start]です。
+ Internet Explorer がサイトを互換モードで表示していないかご確認ください。
+ ブラウザーのデバッグ機能 (一部のブラウザーでは F12 を押した後に [ネットワーク] を選択) を使用して、dc.services.visualstudio.com にデータが送信されていることをご確認ください。

#### データが表示されていたのに停止しました。

* チェック、 [状態ブログ](http://blogs.msdn.com/b/applicationinsights-status/)します。
* データ ポイントの月間クォータに達していませんか? [設定]、[クォータと価格] の順に開いてご確認ください。 上限に達している場合は、プランをアップグレードするか、追加容量分を購入することができます。 参照してください、 [料金プラン](http://azure.microsoft.com/pricing/details/application-insights/)します。


#### 予期しているデータがすべて表示されません

* **サンプリングできます。**アプリケーションが送信するデータ量が多く、Application Insights SDK for ASP.NET バージョン 2.0.0-beta3 以降を使用している場合は、アダプティブ サンプリング機能が動作して、テレメトリの一定の割合のみが送信される可能性があります。 これを無効にすることができます。 [サンプリングについて説明します。](app-insights-sampling.md)


#### <a name="q08"></a>Application Insights を使ってイントラネット Web サーバーを監視できますか?

はい。サーバーがデータをパブリック インターネットに送信できる場合は健全性と利用状況を監視できます。 ファイアウォールで、dc.services.visualstudio.com と f5.services.visualstudio.com にトラフィックを送るために TCP ポート 80 と 443 を開く必要があります。

ただし、サービスに対して Web テストを実行する場合は、パブリック インターネットからポート 80 でサービスにアクセスできる必要があります。

#### パブリック インターネットへのアクセスがないイントラネット Web サーバーを監視できますか?

dc.services.visualstudio.com への https POST 呼び出しを中継できるプロキシを用意する必要があります。 

## Status Monitor が機能しない

参照してください [状態の監視のトラブルシューティング](app-insights-monitor-performance-live-website-now.md#troubleshooting)します。 最も頻繁に問題になるのは、ファイアウォールのポートです。

## ポータル

#### <a name="q05"></a>Microsoft Azure プレビューのスタート ボードを表示しています。 Application Insights でデータを見つけるにはどうすればいいですか?

次のどちらかの操作を行います。

* [参照]、[Application Insights]、目的のプロジェクトの順に選択します。 いない場合、プロジェクトが、する必要があります [を Visual Studio で web プロジェクトに Application Insights を追加する][start]です。

* Visual Studio のソリューション エクスプローラーで、Web プロジェクトを右クリックし、[Application Insights ポータルを開く] を選択します。


#### <a name="update"></a>自分のプロジェクトがデータを送信する Azure のリソースを変更するにはどうすればいいですか?

ソリューション エクスプ ローラーで右クリック `ApplicationInsights.config` 選択 **Application Insights の更新**します。 Azure の既存または新規のリソースにデータを送信できます。 更新ウィザードでは、サーバー SDK のデータの送信先を決定する、ApplicationInsights.config のインストルメンテーション キーを変更します。 [すべて更新] を選択解除している場合を除き、Web ページ内のキーが表示される場所でもキーが変更されます。


#### <a name="q06"></a>Microsoft Azure プレビューのホーム画面のマップに表示されているのはアプリケーションの状態ですか?

いいえ。 このマップには Azure サービスの状態が示されています。 Web テストの結果を確認するには、[参照]、[Application Insights]、対象のアプリケーションの順に選択し、Web テストの結果を表示します。 



#### <a name="data"></a>ポータルでのデータ保持期間はどのくらいですか? セキュリティで保護されていますか?

見て [データの保持とプライバシー][data]します。

## ログの記録

#### <a name="post"></a>診断検索で POST データを表示する方法

POST データは自動ではログに記録されませんが、TrackTrace 呼び出しを使用してメッセージ パラメーターにデータを格納できます。 文字列プロパティの制限よりもサイズ制限は大きいですが、フィルター処理には使用できません。 

## セキュリティ

#### ポータルのデータはセキュリティで保護されていますか? 保持期間はどれくらいですか?

参照してください [データの保持とプライバシー][data]します。


## <a name="q17"></a> Application Insights の機能をすべて有効にしているでしょうか?

<table border="1">
<tr><th>表示内容</th><th>表示方法</th><th>用途</th></tr>
<tr><td>Availability charts</td><td><a href="../app-insights-monitor-web-app-availability/">Web tests</a></td><td>Web アプリが稼働しているか確認する</td></tr>
<tr><td>サーバー アプリ パフォーマンス: 応答時間、...
</td><td><a href="../app-insights-asp-net/">アプリケーションの正常性と利用状況の監視</a><br/>または <br/><a href="../app-insights-monitor-performance-live-website-now/">Install AI Status Monitor on server</a> (コードを記述または <a href="../app-insights-api-custom-events-metrics/#track-dependency">track dependencies</a>)</td><td>パフォーマンスの問題を検出する</td></tr>
<tr><td>Dependency telemetry</td><td><a href="../app-insights-monitor-performance-live-website-now/">Install AI Status Monitor on server</a></td><td>Diagnose issues with databases or other external components</td></tr>
<tr><td>Get stack traces from exceptions</td><td><a href="../app-insights-search-diagnostic-logs/#exceptions">Insert TrackException calls in your code</a> (ただし、いくつかが自動的に報告されます)</td><td>例外を検出して診断する</td></tr>
<tr><td>Search log traces</td><td><a href="../app-insights-search-diagnostic-logs/">Add a logging adapter</a></td><td>Diagnose exceptions, perf issues</td></tr>
<tr><td>クライアントの利用状況の基本情報: ページ ビュー、セッション、...</td><td><a href="../app-insights-javascript/">JavaScript initializer in web pages</a></td><td>Usage analytics</td></tr>
<tr><td>クライアントのカスタム メトリック</td><td><a href="../app-insights-api-custom-events-metrics/">Tracking calls in web pages</a></td><td>Enhance user experience</td></tr>
<tr><td>サーバーのカスタム メトリック</td><td><a href="../app-insights-api-custom-events-metrics/">Tracking calls in server code</a></td><td>ビジネス インテリジェンス</td></tr>
</table>

Web サービスが Azure VM で実行されている場合 [診断を取得][azurediagnostic] があります。

## Automation

実行できます [PowerShell スクリプトを記述](app-insights-powershell-script-create-resource.md) Application Insights リソースを作成します。

## その他の回答

* [Application Insights フォーラム](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)


<!--Link references-->

[azurediagnostic]: ../insights-how-to-use-diagnostics.md
[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md

 
