<properties 
    pageTitle="Azure App Service の .NET Web アプリに対する New Relic によるパフォーマンス管理" 
    description="New Relic のパフォーマンス監視を、Azure App Service で実行する ASP.NET アプリケーションに対して使用する方法について説明します。" 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="07/30/2015" 
    ms.author="stepsic"/>



# Azure App Service の .NET Web アプリに対する New Relic によるパフォーマンス管理

このガイドでは New Relic による世界クラスのパフォーマンス
web アプリを監視 [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)します。 New Relic をアプリケーションに追加するための
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
New Relic Pro は、使用している Web サイト モード (および占有モードを使用している場合はインスタンス サイズ) に基づいて複数のパッケージで提供されます。

料金情報については、次を参照してください。、 [Azure Marketplace の New Relic のページ](/marketplace/partners/newrelic/newrelic)します。

> [AZURE.NOTE] 料金は、コンピューティング インスタンス 10 個までについてのみ示します。 10 を超える場合は、ボリューム ディスカウント料金について New Relic (sales@newrelic.com) までお問い合わせください。

Azure ユーザーは、New Relic エージェントのデプロイ時に、New Relic Pro の 2 週間の試用サブスクリプションをご利用になれます。

Azure Marketplace を使用して New Relic にサインアップします。
--

New Relic は、Azure の Web ロール、Worker ロール、および Azure App Service とシームレスに統合されます。

Azure Marketplace から New Relic に直接サインアップするには、次の簡単な 4 つの手順を実行します。

## 手順 1. New Relic アカウントを作成する

1. ログイン、 [Azure ポータル](https://portal.azure.com) ] をクリック **新規** 隅にあります。
3. クリックして **開発者向けサービス** > **New Relic APM**します。
4. 次を指定することで、New Relic アカウントを構成し、クリックして **作成**します。
    - **名前**
    - **価格レベル**
    - **リソース グループ**
    - **[サブスクリプション]**
    - **Location (場所)**
    - **法律条項**

11. クリックした後 **作成**, 、New Relic アカウントの作成プロセスが開始されます。 クリックして状態を監視することができます、 **通知** ] ボタンをクリックします。 作成されたら、New Relic アカウントのブレードが表示されます。

12. New Relic ライセンス キーを取得するために見て、 **Essentials** ブレードの上部にあるパネルです。 Web Apps インスタンスは、このライセンス キーを、Web アプリと New Relic アカウントを統合するときに、アプリ設定に自動的に登録します。

## 手順 2. Web アプリ用に New Relic の統合を構成する

1. Web アプリのブレードを開き、 [Azure ポータル](https://portal.azure.com)します。
2. クリックして、「...」 ブレードの上部にあるメニュー **タイルを追加する**です。
3.  **監視** ] タブを選択 **アプリケーションの概要** し、web アプリのブレードに表示するタイルを先にドラッグします。
4. [完了] をクリックして、タイルの追加を完了します。
5. クリックして、 **アプリケーション監視** を並べて表示して、選択 **New Relic**します。
6. 前の手順で作成したアカウントを選択し、[ **OK**します。 

    ![](./media/store-new-relic-web-sites-dotnet-application-performance-management/configure-new-relic-integration.png)

    保存操作が完了したら、クリックして **すべての設定** web アプリのブレードをクリックし、 **アプリケーション設定**します。 確認する必要があります、 **NEWRELIC\_LICENSEKEY** 設定に追加される、 **アプリ設定** New Relic をサポートするために、ブレードのセクション。

    >[AZURE.NOTE] 最大 30 秒、新しいアプリケーション設定を有効にする時間がかかる場合があります。 設定をすぐに強制適用するには、Web アプリを再起動します。

## 手順 3. ASP.NET Web アプリを発行する

Visual Studio を使用して Web アプリを発行します。 Web アプリが既に発行済みの場合は、もう一度発行します。これにより Web Apps インスタンスに、New Relic の監視を有効にするために必要な New Relic NuGet パッケージが追加されます。

## 手順 4. New Relic でアプリケーションのパフォーマンスをチェックする

New Relic のダッシュボードを表示するには、次の手順を実行します。

2. Web アプリのブレードを開き、 [Azure ポータル](https://portal.azure.com)します。
3. クリックして **アプリケーション監視** > **アプリケーション名** > **New Relic で表示**します。

    ![](./media/store-new-relic-web-sites-dotnet-application-performance-management/view-new-relic-data.png)

3. アカウントの初回の使用時には、アカウント情報を構成します。
3. New Relic のメニュー バーから選択 **アプリケーション > (アプリケーションの名前)**します。

     **監視 > 概要** ダッシュ ボードが自動的に表示されます。

    ![New Relic の監視ダッシュボード](./media/store-new-relic-web-sites-dotnet-application-performance-management/NewRelic_app.png)

    一覧からアプリを選択した後、 **アプリケーション** ] メニュー、 **概要** ダッシュ ボードは、現在のアプリ サーバーとブラウザーの情報を表示します。

### <a id="using-new-relic"></a>New Relic の使用

[アプリケーション] メニューの一覧からアプリケーションを選択すると、[概要] ダッシュボードに、現在のアプリ サーバーとブラウザーの情報が表示されます。 2 つのビューを切り替える、 **アプリ サーバー** または **ブラウザー** ] ボタンをクリックします。

標準の <a href="https://newrelic.com/docs/site/the-new-relic-ui#functions">New Relic UI</a> と <a href="https://newrelic.com/docs/site/the-new-relic-ui#drilldown">ダッシュ ボードのドリルダウン</a> 機能の他に、アプリケーションの [概要] ダッシュボードには追加の機能が備わっています。

<table border="1">
  <thead>
    <tr>
      <th><b>目的操作</b></th>
      <th><b>これを行うには、次の手順を実行します。</b></th>
    </tr>
  </thead>
  <tbody>
    <tr>
       <td>選択したアプリケーションのサーバーまたはブラウザーのダッシュボード情報を表示する</td>
       <td>[ <b>ボタン</b> または <b>[ブラウザー] ボタンを</b> をクリックしてください。</td>
    </tr>
     <tr>
       <td>アプリケーションのしきい値レベルを表示する <a href="https://newrelic.com/docs/site/apdex" target="_blank">Apdex</a> スコア</td>
       <td>[Apdex スコア] の <b>?<b> アイコンをポイントします。</b></b></td>
    </tr>
    <tr>
       <td>世界規模の Apdex の詳細を表示する</td>
       <td>[概要] の <b>[ブラウザー] ボタンを</b> 任意の場所に、グローバル Apdex マップのポイントに表示します。<br /><b>ヒント:</b> 選択したアプリケーションの <a href="https://newrelic.com/docs/site/geography" target="_blank">地理的な場所</a>ダッシュボードに直接移動するには、 <b>グローバル Apdex の</b> タイトルをクリックするか、グローバル Apdex マップの任意の場所をクリックします。</td>
    </tr>
    <tr>
       <td>[Web トランザクション] <a href="https://docs.newrelic.com/docs/applications-menu/transactions-dashboard" target="_blank">ダッシュボードを</a> dashboard</td>
       <td>アプリケーションの [概要] ダッシュボードの [Web トランザクション] テーブルをクリックします。 または、特定の Web トランザクションに関する詳細 ( <a href="https://newrelic.com/docs/site/key-transactions" target="_blank">主要トランザクションなど) を表示するには、</a>その名前をクリックします。</td>
    </tr>
    <tr>
       <td>[Web トランザクション] <a href="https://newrelic.com/docs/site/errors" target="_blank">エラー</a> dashboard</td>
       <td>[エラー率] グラフと #39; アプリケーションの概要ダッシュ ボードのタイトル。<br /><b>ヒント:</b> [アプリケーション]、(アプリケーション)、[イベント]、[エラー] <b>アプリケーション</b> & gt;(アプリ) & gt;イベント & gt;エラー。</td>
    </tr>
    <tr>
       <td>アプリケーションのサーバーの詳細を表示する</td>
       <td><p>次のいずれかを実行します。<p>
        <ul>
          <li>ホストのテーブル ビューと各ホストのメトリックの詳細内訳を切り替えます。</li>
          <li>個々のサーバーの名前をクリックします。</li>
          <li>個々のサーバーの Apdex スコアをポイントします。</li>
          <li>個々のサーバーの CPU 使用率またはメモリをクリックします。</li>
        </ul>
       </p></p></td>
    </tr>
  </tbody>
</table>

以下に、ブラウザー ビューを選択したときのアプリケーションの [概要] ダッシュボードの例を示します。

![パッケージ マネージャー コンソール](./media/store-new-relic-web-sites-dotnet-application-performance-management/NewRelic_app_browser.png)

## 次のステップ

詳細については、次に示すその他のリソースを参照してください。

 * [Azure の web サイトの .NET エージェントをインストールする](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-websites#manual): New Relic .NET エージェントのインストール手順 
 * [新しい Relic ユーザー インターフェイス](https://newrelic.com/docs/site/the-new-relic-ui): 
New Relic UI の概要、ユーザー権限とプロファイルの設定、標準機能の使用、およびダッシュボードのドリルダウンの詳細
 * [アプリケーションの概要](https://newrelic.com/docs/site/applications-overview): 機能と New Relic のアプリケーションの概要ダッシュ ボードを使用するときに、関数
 * [Apdex](https://newrelic.com/docs/site/apdex): Apdex によってエンドユーザーのアプリケーションに対する満足測定方法の概要
 * [Real User Monitoring](https://newrelic.com/docs/features/real-user-monitoring): ユーザーにかかる RUM 詳細、時間を確認する方法の概要 
および使用しているブラウザーに関する詳細を確認する方法の概要
 * [解決に役立つ](https://newrelic.com/docs/site/finding-help): New Relic のオンライン ヘルプ センターで利用できるリソース

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)


[vswebsite]: web-sites-dotnet-get-started.md

[wmnugetbutton]: ./media/store-new-relic-web-sites-dotnet-application-performce-management/nrwmnugetbutton.png
[wmnugetgallery]: ./media/store-new-relic-web-sites-dotnet-application-performce-management/nrwmnugetgallery.png

[newrelicconf]: ./media/store-new-relic-web-sites-dotnet-application-performce-management/nrwmlicensekey.png
[vslicensekey]: ./media/store-new-relic-web-sites-dotnet-application-performce-management/nrvslicensekey.png
[add-on]: ./media/store-new-relic-web-sites-dotnet-application-performce-management/nraddon.png
[custom]: ./media/store-new-relic-web-sites-dotnet-application-performce-management/nrcustom.png
 
