<properties 
    pageTitle="Application Insights でのデータ保持と保存" 
    description="データ保持およびプライバシー ポリシー ステートメント" 
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

# Application Insights でのデータの収集、保持、保存 

*Application Insights はプレビュー段階です。*

## 概要

この記事で収集したデータに関する質問に回答  [Visual Studio Application Insights][start] 処理方法と格納されているとします。

Application Insights は、プレビュー段階の Azure サービスです。 プレビューで」に記載されているポリシー、データの保護を目指しておりますが中に、 [Azure のセキュリティ、プライバシー、およびコンプライアンスのホワイト ペーパー](http://go.microsoft.com/fwlink/?linkid=392408)します。


## コレクション

#### Application Insights はどのようにデータを収集しますか。

Application Insights SDK およびエージェントは、アプリケーションに結び付けられると、データを Application Insights サービスに送信します。  サービスは、このデータを処理して、レポート、アラート、その他の機能を提供します。  これには、プロパティ、カスタム イベントなど、API を使用してキャプチャ対象として選択したデータが含まれます。

#### どれくらいの量のデータをキャプチャできますか。 

**1 秒あたり**: インストルメンテーション キーごとの 1 秒あたり最大 500 データ ポイント (つまり、アプリケーションごと)。 無料の [価格レベル][pricing], 、上限は 100 dp/秒です。

次の 3 つのバケットは別々にカウントされます。

* [TrackTrace 呼び出し](app-insights-api-custom-events-metrics.md#track-trace) と [ログのキャプチャ](app-insights-asp-net-trace-logs.md)
* [例外](app-insights-api-custom-events-metrics.md#track-exception), 、50/秒の制限を低くして件名です。
* その他すべてのテレメトリ (ページ ビュー、要求、依存関係、メトリック、カスタム イベント、Web テストの結果)。

**毎月**: 間 5 1500万データに応じてポイントと、毎月、 [料金プラン](http://azure.microsoft.com/pricing/details/application-insights/)します。 を除き、無料 [価格レベル][pricing], 、上限に達した場合は、追加の容量を購入することができます。


A *データ ポイント* アプリについて Azure ポータルに送信されるテレメトリの項目。 次の手段で送信できます。

* [SDK モジュール](app-insights-configuration-with-applicationinsights-config.md) 例要求またはクラッシュを報告するか、パフォーマンスを測定するために、データを自動的に収集します。
* [API](app-insights-api-custom-events-metrics.md) `Track...` など、記述した呼び出し `TrackEvent` または `trackPageView`です。
* [可用性 web テスト](app-insights-monitor-web-app-availability.md) を設定することです。

テレメトリには次の項目が含まれます。

* 内の各行 [診断検索](app-insights-diagnostic-search.md)
* どのグラフから生データ [メトリックス エクスプ ローラー](app-insights-metrics-explorer.md) ごとに集計します。 パフォーマンス カウンターのようなメトリック データは、通常は、メトリック エクスプローラーの個々のポイントとして表示されません。
* 各 web テストの結果で、 [可用性](app-insights-monitor-web-app-availability.md) レポートします。

ユーザーとセッションの数は、価格設定の目的のクォータには含まれません。

*アプリが 1 秒あたりのレートを超えるとどうなりますか。*

* アプリから送信されるデータ量は分単位で評価されます。 1 分間で平均して 1 秒あたりのレートを超える場合、一部の要求がサーバーから拒否されます。 このような場合、一部のバージョンの SDK は再送信を試みるので、データ量の増加する時間が数分間長くなりますが、JavaScript SDK など他の SDK は、拒否されたデータを単に破棄します。

*アプリがどれだけのデータ ポイントを送信しているかを知る方法はありますか。*

* [設定]、[クォータと価格] の順に開き、[データ ボリューム] グラフを表示します。
* またはメトリックス エクスプ ローラーを選択し、新しいグラフを追加 **データ ポイントのボリューム** メトリックです。 グループ化をオンにすると、またはグループを **データ型**します。

*アプリが送信するデータの量を減らすことができますか。*

* 使用 [サンプリング](app-insights-sampling.md)します。 このテクノロジは、メトリックや、検索で関連するアイテム間を移動する機能を損なうことなく、データ レートを削減します。 ASP.NET SDK 2.0.0-beta3 以降、既定でアダプティブ サンプリングが有効です。
* [製品利用統計情報コレクターをオフに](app-insights-configuration-with-applicationinsights-config.md) する必要がないです。


#### データはどれだけの期間保持されますか。 

依存して、 [料金プラン](http://azure.microsoft.com/pricing/details/application-insights/)します。

生データ ポイント (つまり、診断検索で調べることができる項目) は、7 ～ 30 日です。

集計されたデータ (つまり、メトリックス エクスプローラーに表示されるカウント、平均、その他の統計データ) は、1 分の詳細度であれば 30 日、(種類に応じて) 1 時間または 1 日の詳細度であれば少なくとも 13 か月の期間にわたって保持されます。

#### 各種のデータに対してどのような制限がありますか。

1.  アプリケーションに対して最大 200 の一意のメトリックの名前と 200 の一意のプロパティの名前。 メトリックには、イベントなどの他のデータ型の測定値と同様に TrackMetric を通じて送信されるデータが含まれます。  [メトリックとプロパティの名前][api] はデータ型にスコープが制限されず、インストルメンテーション キーごとにグローバルです。
2.  [プロパティ][apiproperties] フィルター処理に使用することができ、グループ化のみがあるプロパティごとに一意の値を 100 未満にします。 一意の値が 100 を超えた後も、プロパティは検索とフィルタリングに使用できますが、フィルター処理には使用できなくなります。
3.  要求名やページの URL などの標準プロパティは、1 週間あたりの 1000 の一意な値に制限されます。 1000 の一意の値を超えると、追加の値は「その他の値」としてマークされます。 元の値は、全文テキスト検索とフィルタリングに引き続き使用できます。


## アクセス

#### データを見ることができるのはだれですか。

お客様と、組織アカウントを持っている場合はチーム メンバーが、データを見ることができます。 

お客様とチーム メンバーはデータをエクスポートできます。また、他の場所にデータをコピーしたり、第三者にデータを渡したりすることもできます。

#### アプリから Application Insights に送信された情報を Microsoft はどのように利用しますか。

Microsoft は、お客様にサービスを提供する目的でのみデータを使用します。


## 場所

#### データが保持されている場所はどこですか。 

* アメリカ合衆国内です。 

#### データを他の場所 (たとえば、ヨーロッパ) に保存することはできますか。 

* まだありません。 

## セキュリティ 

#### データのセキュリティは保たれますか。 

データは、Microsoft Azure サーバーに保管されます。 アカウント、Azure ポータルの場合、アカウントの制限事項が記載されている、 [Azure のセキュリティ、プライバシー、コンプライアンスの文書](http://go.microsoft.com/fwlink/?linkid=392408)します。 Visual Studio Team Services ポータルでアカウントに対して、 [Visual Studio Team Services のデータの保護](http://download.microsoft.com/download/8/E/E/8EE6A61C-44C2-4F81-B870-A267F1DF978C/MicrosoftVisualStudioOnlineDataProtection.pdf) ドキュメントが適用されます。 

Microsoft のスタッフによるデータへのアクセスは制限されます。 Microsoft は、Application Insights の使用をサポートするために必要であれば、ユーザーからアクセス許可を得た上でデータにアクセスします。 

このほか、Application Insights の機能の向上に役立てるために、すべてのお客様のアプリケーションのデータ (データ レート、トレースの平均サイズなど) を集計のうえ使用します。

#### だれかのテレメトリが Application Insights データに干渉する可能性はありますか。

Web ページのコード内にインストルメンテーション キーがある場合には、だれかがそれを使用してお客様のアカウントに追加のテレメトリを送信することができます。 追加データの量が多いと、アプリのパフォーマンスと使用状況がメトリックに適切に示されなくなる可能性があります。

コードを他のプロジェクトと共有する場合は、インストルメンテーション キーを必ず削除してください。

## 暗号化

#### Application Insights サーバーでデータは暗号化されますか。 

現時点で、サーバー内では暗号化されません。

データをデータ センター間で移動するときは、すべてのデータが暗号化されます。

#### アプリケーションから Application Insights サーバーに送信されるときにデータは暗号化されますか。

はい。ポータルからほぼすべての SDK (Web サーバー、デバイス、HTTPS Web ページを含みます) への送信に https が使用されます。 唯一の例外は、プレーンな HTTP Web ページから送信されるデータです。

## 個人を特定できる情報

#### 個人を特定できる情報 (PII) は Application Insights に送信されますか。 

はい。 

一般的なガイダンス:

* ほとんどの標準テレメトリ (つまり、コードを書かなくても送信されるテレメトリ) には、明示的な PII は含まれません。 ただし、イベントのコレクションから推論することによって、個人を特定できる場合があります。
* 例外とトレースのメッセージには PII が含まれている可能性があります。
* カスタム テレメトリ (つまり、API またはログ トレースを使用してコードに記述する TrackEvent などの呼び出し) には、自分で選んだすべてのデータを含めることができます。


収集されるデータに関する詳細な説明については、このドキュメントの末尾の表をご覧ください。



#### 私は PII に関する法令を遵守する責任を負いますか。

はい。 データの収集と使用に関して法令および Microsoft Online Services の条項に従っていることを確認するのはお客様の責任です。

お客様は、アプリケーションが収集するデータと、データの使用方法について、顧客に適切に通知する必要があります。

#### ユーザーは Application Insights を無効にできますか。

直接無効にすることはできません。 ユーザーが Application Insights を無効にするために操作できるスイッチはありません。

ただし、アプリケーションでそのような機能を実装することはできます。 すべての SDK には、テレメトリの収集を無効にする API 設定が含まれています。 

#### アプリケーションが意図せずに機密情報を収集しています。 このデータの収集を取り消して Application Insights にデータが保持されないようにすることはできますか。

Application Insights がデータをフィルター処理したり、削除したりすることはありません。 データを適切に管理して、そのようなデータが Application Insights に送信されないように注意してください。



## Application Insights によって送信されるデータ

SDK はプラットフォームごとに異なり、インストールできるコンポーネントも複数あります  (を参照してください [Application Insights - 開始][start].)各コンポーネントは、それぞれ異なるデータを送信します。

#### さまざまなシナリオで送信されるデータのクラス

操作  | 収集されるデータのクラス (次の表を参照)
---|---
[Application Insights SDK を .NET Web プロジェクトに追加する][greenbrown] | ServerContext<br/>Inferred<br/>Perf counters<br/>Requests<br/>**例外**<br/>Session<br/>users
[Status Monitor を IIS をインストールします。][redfield]<br/>[Azure VM または Web アプリに AI 拡張機能を追加します。][azure]|依存関係<br/>ServerContext<br/>Inferred<br/>Perf counters
[Application Insights SDK を Java Web アプリに追加する][java]|ServerContext<br/>Inferred<br/>Request (要求)<br/>Session<br/>users
[JavaScript SDK を Web ページに追加する][client]|ClientContext <br/>Inferred<br/>ページ<br/>ClientPerf
[SDK を Windows ストア アプリに追加する][windows]|DeviceContext<br/>ユーザー<br/>クラッシュ データ
[既定のプロパティを定義する][apiproperties]|**プロパティ** のすべての標準とカスタム イベント
[Call TrackMetric][api]|数値<br/>**プロパティ**
[Call Track*][api]|イベント名<br/>**プロパティ**
[Call TrackException][api]|**例外**<br/>スタック ダンプ<br/>**プロパティ**
SDK はデータを収集できません。 次に例を示します。 <br/> -パフォーマンス カウンターにアクセスできません<br/> -テレメトリ初期化子での例外 | SDK diagnostics
 

 [の他のプラットフォームの Sdk][platforms], 、自分のドキュメントを参照してください。



#### 収集されるデータのクラス

収集されるデータのクラス | 含まれるデータ (網羅的なリストではありません) 
---|---
**プロパティ**|**コードによって決まる任意のデータ**
DeviceContext |Id、IP、ロケール、デバイス モデル、ネットワーク、ネットワークの種類、OEM の名前、画面解像度、ロール インスタンス、ロール名、デバイスの種類
ClientContext |OS、ロケール、言語、ネットワーク、ウィンドウの解像度
Session | セッション ID
ServerContext |コンピューター名、ロケール、OS、デバイス、ユーザー セッション、ユーザー コンテキスト、操作 
Inferred |IP アドレス、タイムスタンプ、OS、ブラウザーからの geo ロケーション
メトリック | メトリックの名前と値
イベント | イベントの名前と値
PageViews | URL とページ名または画面名
Client perf | URL/ページ名、ブラウザーの読み込み時間
Requests |URL、期間、応答コード
依存関係|種類 (SQL、HTTP、...)、接続文字列または URI、同期または非同期、期間、成功、SQL ステートメント (Status Monitor による)
**例外** | 型、 **メッセージ**, 、呼び出し履歴、ソース ファイルと行番号、スレッド id
Crashes | プロセス id、親プロセスの id、クラッシュ スレッドの id です。アプリケーションの修正プログラム、id、ビルドします。 例外の種類、アドレス、理由です。難読化されたシンボルとレジスタ、バイナリの開始と終了アドレス、バイナリ名とパス、cpu の種類
Trace | **メッセージ** および重大度レベル
Perf counters | プロセッサ時間、使用可能なメモリ、要求レート、例外レート、プロセスのプライベート バイト、IO レート、要求の期間、要求のキューの長さ
可用性 | Web テストの応答コード、各テスト ステップの期間、テスト名、タイムスタンプ、成功、応答時間、テストの場所
SDK diagnostics | トレース メッセージまたは例外 

実行できます [ApplicationInsights.config を編集して、データの一部を無効に切り替える][config]


## クレジット

この製品から利用可能な MaxMind によって作成された GeoLite2 データが含まれています。 [http://www.maxmind.com](http://www.maxmind.com)します。

## <a name="video"></a>ビデオ

#### はじめに

> [AZURE.VIDEO application-insights-introduction]

#### 作業開始

> [AZURE.VIDEO getting-started-with-application-insights]




<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[apiproperties]: app-insights-api-custom-events-metrics.md#properties
[azure]: ../insights-perf-analytics.md
[client]: app-insights-javascript.md
[config]: app-insights-configuration-with-applicationinsights-config.md
[greenbrown]: app-insights-asp-net.md
[java]: app-insights-java-get-started.md
[platforms]: app-insights-platforms.md
[pricing]: http://azure.microsoft.com/pricing/details/application-insights/
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md

 

