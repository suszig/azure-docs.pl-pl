<properties 
    pageTitle="Java Web プロジェクトでの Application Insights のトラブルシューティング" 
    description="トラブルシューティング ガイド - Application Insights でライブ Java アプリケーションを監視します。" 
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
    ms.date="10/21/2015" 
    ms.author="awills"/>
 
# Java 用 Application Insights のトラブルシューティングおよび Q&A

質問または問題 [Java で Visual Studio Application Insights][java]でしょうか。 ここでは、いくつかのヒントを紹介します。


## ビルド エラー

*Eclipse で、Maven または Gradle を使用して Application Insights SDK を追加すると、ビルドまたはチェックサムの検証エラーが発生します。*

* 場合、依存関係 <version> 要素がワイルドカード文字を含むパターンを使用して (例: (Maven) `<version>[1.0,)</version>` または (Gradle) `version:'1.0.+'`) のように、代わりに、特定のバージョンを指定してみてください `1.0.2`します。 参照してください、 [リリース ノート](app-insights-release-notes-java.md) の最新バージョンです。

## データが表示されない 

*Application Insights が正常に追加された後でアプリケーションを実行したところ、ポータルにデータが表示されません*

* 少し待ってから、[最新の情報に更新] をクリックします。 グラフは周期的に自動で更新されますが、手動で更新することもできます。 更新間隔は、グラフの時間範囲によって異なります。
* プロジェクトのリソース フォルダーにある ApplicationInsights.xml ファイル内で、インストルメンテーション キーが定義されていることをご確認ください。
* この xml ファイルに `<DisableTelemetry>true</DisableTelemetry>` ノードが存在しないことをご確認ください。
* ファイアウォールでは、dc.services.visualstudio.com と f5.services.visualstudio.com への発信トラフィック用に TCP ポート 80 と 443 を開く必要がある場合があります。
* Microsoft Azure のスタート ボードで、サービス状態マップをご確認ください。 アラート表示がある場合は、"OK" が表示されるまで待ってから、Application Insights アプリケーション ブレードをいったん閉じて開き直します。
* プロジェクトのリソース フォルダーにある ApplicationInsights.xml ファイル内で、ルート ノードの下に `<SDKLogger />` 要素を追加して IDE コンソール ウィンドウへのログを有効にし、[Error] から始まるエントリを調べます。
* 正しい ApplicationInsights.xml ファイルが Java SDK によって正常に読み込まれたことを確認します。そのためには、コンソールの出力メッセージに「構成ファイルが正常に検出されました」というメッセージがあるかどうかを確認します。
* 構成ファイルが見つからない場合、出力メッセージを確認して構成ファイルの検索範囲を確かめ、ApplicationInsights.xml がこれらの検索場所のいずれかに存在していることを確認します。 通例、構成ファイルは Application Insights SDK の JAR ファイルの近くに見つかります。 たとえば、Tomcat の場合は WEB-INF/lib フォルダーがこれに相当します。



#### データが表示されていたのに停止しました。

* チェック、 [状態ブログ](http://blogs.msdn.com/b/applicationinsights-status/)します。
* データ ポイントの月間クォータに達していませんか? Open Settings/Quota and Pricing to find out. 上限に達している場合は、プランをアップグレードするか、追加容量分を購入することができます。 参照してください、 [料金プラン](http://azure.microsoft.com/pricing/details/application-insights/)します。



## 使用状況データがない

*要求と応答時間についてのデータは表示されますが、ページ ビュー、ブラウザー、またはユーザーについてのデータが表示されません。*

サーバーからテレメトリを送信するためのアプリ設定は正常に行われています。 次に、次の手順は [web ブラウザーからテレメトリを送信する web ページを設定][usage]します。

または、使用するクライアントが、アプリの場合、 [電話やその他のデバイス][platforms], からテレメトリを送信することができます。 

クライアントおよびサーバー テレメトリの両方の設定に、同じインストルメンテーション キーを使用します。 データは同じ Application Insights リソース内に表示され、クライアントとサーバーのイベントを関連付けることができるようになります。



## テレメトリの無効化

*テレメトリの収集を無効にする方法を教えてください。*

コード内で以下のように指定します。

    TelemetryConfiguration config = TelemetryConfiguration.getActive();
    config.setTrackingIsDisabled(true);


**または** 

プロジェクトのリソース フォルダーにある ApplicationInsights.xml を更新します。 ルート ノードの下に次の内容を追加します。

    <DisableTelemetry>true</DisableTelemetry>

XML メソッドを使用するうえで、値を変更した場合はアプリケーションを再起動する必要があります。

## ターゲットの変更

*自分のプロジェクトがデータを送信する Azure のリソースを変更するにはどうすればいいですか?*

* [新しいリソースのインストルメンテーション キーを取得します。][java]
* Eclipse の Azure Toolkit を使用して、プロジェクトに Application Insights を追加した場合は、web プロジェクトを右クリックしてが **Azure**, 、**Configure Application Insights**, 、キーを変更するとします。
* それ以外の場合は、プロジェクトのリソース フォルダーにある ApplicationInsights.xml 内のキーを更新します。


## Azure のスタート画面

*表示しています [Azure ポータル](http://portal.azure.com)します。 このマップから、自分のアプリについて何か情報が得られるのでしょうか?*

いいえ、そのマップは世界中の Azure サーバーの正常性を表しています。

*Azure のスタート画面 (ホーム画面) から、アプリに関するデータを見つける方法を教えてください。*

仮定 [Application Insights のアプリをセットアップ][java], 参照] をクリックして、Application Insights を選択して、アプリ用に作成したアプリ リソースを選択します。 次からその場所にすばやくアクセスできるように、スタート画面にアプリをピン留めすることができます。

## イントラネット サーバー

*イントラネット上のサーバーを監視できますか?*

はい、お使いのサーバーがパブリック インターネットを介して Application Insights ポータルにテレメトリを送信できるなら、可能です。 

ファイアウォールでは、dc.services.visualstudio.com と f5.services.visualstudio.com への発信トラフィック用に TCP ポート 80 と 443 を開く必要がある場合があります。

## データの保持 

*ポータルでのデータ保持期間はどのくらいですか? セキュリティで保護されていますか?*

参照してください [データの保持とプライバシー][data]します。

## 次のステップ

*Java サーバー アプリ用に Application Insights を設定しました。 ほかには何ができるか教えてください。*

* [Web ページの可用性の監視][availability]
* [Web ページの使用状況の監視][usage]
* [使用状況の追跡およびデバイス アプリでの問題の診断][platforms]
* [アプリの使用状況を追跡するためのコードを記述する][track]
* [診断ログのキャプチャ][javalogs]


## 問い合わせ

* [スタック オーバーフロー](http://stackoverflow.com/questions/tagged/ms-application-insights)

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[data]: app-insights-data-retention-privacy.md
[java]: app-insights-java-get-started.md
[javalogs]: app-insights-java-trace-logs.md
[platforms]: app-insights-platforms.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-web-track-usage.md

 

