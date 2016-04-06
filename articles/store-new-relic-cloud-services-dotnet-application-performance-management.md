<properties 
    pageTitle="Azure での New Relic の使用 | Microsoft Azure" 
    description="New Relic サービスを使用して Azure アプリケーションを管理および監視する方法について説明します。" 
    services="" 
    documentationCenter=".net" 
    authors="nickfloyd" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="03/16/2015" 
    ms.author="nickfloyd@newrelic.com"/>



# Azure の New Relic によるアプリケーション パフォーマンス管理

このガイドでは New Relic による世界クラスのパフォーマンス
監視を Azure でホストされるアプリケーション。 New Relic をアプリケーションに追加するための
すばやく簡単なプロセスについて説明し、New Relic のいくつかの機能を
紹介します。 New Relic の使い方の詳細については、次を参照してください。 [New Relic の使用](#using-new-relic)します。

## New Relic とは

New Relic は、運用アプリケーションを監視し、そのパフォーマンスと
信頼性についての深い洞察が得られる開発者向けのツールです。 また、
パフォーマンスの問題を特定、診断する際に、時間を節約するよう設計されていて、
これらの問題の解決に必要な情報をすばやく提示します。

New Relic は、Web トランザクションの読み込み時間とスループットを、
サーバーおよびユーザーのブラウザーの両方から追跡します。 データベース処理にかかった時間を表示し、
速度の遅いクエリと Web 要求を分析します。また、アップタイムの監視とアラート機能の提供、アプリケーション例外の追跡など、
数多くの機能を備えています。

## Azure ストアの New Relic 特別料金


New Relic Standard は、Azure ユーザーであれば無料でご利用いただけます。
New Relic Pro は、Azure クラウド サービスのインスタンス サイズに基づいて提供されます

料金情報については、次を参照してください。、 [Azure ストアの New Relic のページ](http://azure.microsoft.com/marketplace/partners/newrelic/newrelic)します。

> [AZURE.NOTE] 料金は、コンピューティング インスタンス 10 個までについてのみ示します。 10 を超える場合は、ボリューム ディスカウント料金について New Relic (sales@newrelic.com) までお問い合わせください。

Azure ユーザーは、New Relic エージェントのデプロイ時に、New Relic Pro の 2 週間の試用サブスクリプションをご利用になれます。

## Azure ストアを使用して New Relic にサインアップする

New Relic は、Azure の Web ロールおよびワーカー ロールとシームレスに統合されます。

Azure ストアから New Relic に直接サインアップするには、次の簡単な 3 つの手順を実行します。

### 手順 1. Azure ストアからサインアップする

1. ログイン、 [Azure 管理ポータル](https://manage.windowsazure.com)します。
2. 管理ポータルの下のウィンドウで、クリックして **新規**します。
3. クリックして **ストア**します。
4.  **アドオン選択** ダイアログで、 **New Relic** ] をクリック **次**します。
5.  **個人用サービス** ダイアログ ボックスで、目的の New Relic プランを選択します。
6. 該当する場合は、プロモーション コードを入力します。
7. Azure の設定における New Relic サービスの表示名を入力するか、
   設定、または既定値を使用して **NewRelic**します。 この名前は、サブスクライブした
   Azure ストア項目の一覧内で一意である必要があります。
8. リージョンの値を選択します。たとえば、 **米国西部**します。
9. クリックして **次**します。
10.  **購入の確認** ] ダイアログ ボックスで、プランと料金情報
    法律条項を確認します。 条項に同意する場合はクリックして **購入**します。
11. クリックした後 **購入**, 、New Relic アカウントの作成プロセスが開始されます。 その状態は、Azure 管理ポータルで監視できます。
12. New Relic ライセンス キーを取得するにはクリックして **出力値**です。 
13. 表示されたライセンス キーをコピーします。 このキーは、New Relic NuGet パッケージのインストール時に入力する必要があります。

### 手順 2. NuGet パッケージをインストールします。

1. Visual Studio ソリューションを開くか、
   **ファイル > 新規 > プロジェクト**します。

    ![Visual Studio](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget01.png)

2. Azure クラウド サービス プロジェクトがないかどうかは、
   ソリューションでは、ソリューション エクスプ ローラーで、アプリを右クリックしていずれかを追加し、
   選択すると **Azure クラウド サービス プロジェクトの追加**します。

    ![クラウド サービスを作成する](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget02.png)

3. 選択して、パッケージ マネージャー コンソールを開く **ツール > ライブラリ パッケージ マネージャー] >
   パッケージ マネージャー コンソール**します。 パッケージ マネージャー コンソール ウィンドウの上部で、
   プロジェクトが既定のプロジェクトになるよう設定します

    ![パッケージ マネージャー コンソール](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget04.png)

4. パッケージ マネージャー コマンド プロンプトで「 `Install-Package
   NewRelicWindowsAzure` とキーを押します **Enter**します。

    ![パッケージ マネージャーでのインストール](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget06.png)

5. ライセンス キーのダイアログで、Azure ストアから受け取ったライセンス キーを入力します。

    ![ライセンス キーの入力](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget07.png)

6. 省略可能: アプリケーション名のダイアログで名前を入力、アプリのようになります
   New Relic のダッシュボードに表示されているとおりに入力します。 または、既定のソリューション名を使用します。

    ![アプリケーション名の入力](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget08.png)

7. ソリューション エクスプ ローラーから、Azure クラウド サービス プロジェクトを右クリックし、選択 **発行**します。

    ![クラウド プロジェクトの発行](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget09.png)


**注:** 初めてこのアプリケーションを Azure に展開する場合は、入力を適宜、 
Azure の資格情報。 詳細については、次を参照してください [Azure の web サイトに ASP.NET Web アプリケーションを展開する。](app-service-web\web-sites-dotnet-get-started.md)

![発行の設定](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelicAzureNuget10.png)

### 手順 3. New Relic でアプリケーションのパフォーマンスをチェックする

New Relic のダッシュボードを表示するには、次の手順を実行します。

1. Azure ポータルからクリックして、 **管理** ] ボタンをクリックします。
2. New Relic アカウントの電子メールとパスワードを使用してサインインします。
3. New Relic のメニュー バーから選択 **アプリケーション > (アプリケーションの名前)**します。

     **監視 > 概要** ダッシュ ボードが自動的に表示されます。

    ![New Relic の監視ダッシュボード](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelic_app.png)

    一覧からアプリを選択した後、 **アプリケーション** ] メニュー、 **概要** ダッシュ ボードは、現在のアプリ サーバーとブラウザーの情報を表示します。

### <a id="using-new-relic"></a>New Relic の使用

[アプリケーション] メニューの一覧からアプリケーションを選択すると、[概要] ダッシュボードに、現在のアプリ サーバーとブラウザーの情報が表示されます。 2 つのビューを切り替える、 **アプリ サーバー** または **ブラウザー** ] ボタンをクリックします。

[標準の New Relic UI](https://newrelic.com/docs/site/the-new-relic-ui#functions") と[ダッシュボードのドリルダウン](https://newrelic.com/docs/site/the-new-relic-ui#drilldown)機能のほかにも、アプリケーションの [概要] ダッシュボードにはさまざまな機能があります。

| 目的操作 | これを行うには、次の手順を実行します。 |
| ----------------- | ---------- |
| 選択したアプリのサーバーまたはブラウザーのダッシュボード情報を表示する。 | クリックして、 **App Server** または **ブラウザー** ] ボタンをクリックします。 |
| アプリケーションのしきい値レベルを表示 [Apdex](https://newrelic.com/docs/site/apdex) スコアです。 | Apdex スコアをポイント **でしょうか。** アイコンをポイントします。 |
| 世界規模の Apdex の詳細を表示する。 | [概要] の **ブラウザー** ビューで、グローバル Apdex マップの任意の場所] をポイントします。 **ヒント:** 選択したアプリケーションに直接移動する [Geography](https://docs.newrelic.com/docs/new-relic-browser/geography-dashboard") ダッシュ ボードで、] をクリックして、 **グローバル Apdex** タイトル、または、グローバル Apdex マップの任意の場所をクリックします。 |
| ビュー、 [Web トランザクション](https://newrelic.com/docs/applications-dashboards/web-transactions) ダッシュ ボード。 | アプリケーションの [概要] ダッシュボードの [Web トランザクション] テーブルをクリックします。 または、特定の web トランザクションに関する詳細を表示 (を含む [主要トランザクション](https://newrelic.com/docs/site/key-transactions"), 、その名前をクリックします。 |
| ビュー、 [エラー](https://newrelic.com/docs/site/errors) ダッシュ ボード。 | アプリケーションの [概要] ダッシュボードの [エラー率] グラフのタイトルをクリックします。 **ヒント:** からエラー] ダッシュ ボードを表示することもできます。 **アプリケーション** > (アプリ) > イベント > エラーです。 |


さらに、アプリのサーバーの詳細を表示する場合は、次のいずれかを実行します。

- ホストのテーブル ビューと各ホストのメトリックの詳細内訳を切り替えます。
- 個々のサーバーの名前をクリックします。
- 個々のサーバーの Apdex スコアをポイントします。
- 個々のサーバーの CPU 使用率またはメモリをクリックします。

以下に、ブラウザー ビューを選択したときのアプリケーションの [概要] ダッシュボードの例を示します。

![パッケージ マネージャー コンソール](./media/store-new-relic-cloud-services-dotnet-application-performce-management/NewRelic_app_browser.png)

## 次のステップ

詳細については、次に示すその他のリソースを参照してください。

 * [Azure での .NET エージェントをインストールする](https://newrelic.com/docs/dotnet/installing-the-net-agent-on-azure): New Relic .NET エージェントのインストール手順 
 * [新しい Relic ユーザー インターフェイス](https://newrelic.com/docs/site/the-new-relic-ui): 
New Relic UI の概要、ユーザー権限とプロファイルの設定、標準機能の使用、およびダッシュボードのドリルダウンの詳細
 * [アプリケーションの概要](https://newrelic.com/docs/site/applications-overview): 機能と New Relic のアプリケーションの概要ダッシュ ボードを使用するときに、関数
 * [Apdex](https://newrelic.com/docs/site/apdex): Apdex によってエンドユーザーのアプリケーションに対する満足測定方法の概要
 * [Real User Monitoring](https://newrelic.com/docs/features/real-user-monitoring): ユーザーにかかる RUM 詳細、時間を確認する方法の概要 
および使用しているブラウザーに関する詳細を確認する方法の概要
 * [解決に役立つ](https://newrelic.com/docs/site/finding-help): New Relic のオンライン ヘルプ センターで利用できるリソース


