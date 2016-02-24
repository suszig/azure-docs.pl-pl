<properties
    pageTitle="トラブルシューティング: Web アプリのパフォーマンス低下"
    description="この記事は、Azure App Service でホストされている Web アプリで発生したパフォーマンスに関する問題のトラブルシューティングを行うために役立ちます。"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor=""
    tags="top-support-issue"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/23/2015"
    ms.author="cephalin"/>

# トラブルシューティング: Web アプリのパフォーマンス低下

この記事でホストされている web アプリのパフォーマンスの問題をトラブルシューティングできます。 [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)します。

この記事のどの時点でもその他のヘルプを必要がある場合で Azure のエキスパートに連絡することができます [MSDN Azure とスタック オーバーフロー フォーラム](http://azure.microsoft.com/support/forums/)します。 または、Azure サポート インシデントを送信できます。 移動して、 [Azure サポート サイト](http://azure.microsoft.com/support/options/) ] をクリック **サポートにお問い合わせ**します。

## 症状

ブラウザーで Web アプリにアクセスしたときのページの読み込みが遅く、タイムアウトすることもある

## 原因

この症状は多くの場合、アプリケーション レベルの問題が原因で発生します。その例を次に示します。

-   要求に時間がかかっている
-   アプリケーションのメモリ/CPU 使用率が高い
-   例外が発生してアプリケーションがクラッシュする

## トラブルシューティングの手順

トラブルシューティングは、大きく次の 3 つのタスクに分けられます。この 3 つのタスクを上から順に行います。

1.  [アプリケーションの動作を観察、監視する](#observe)
2.  [データを収集する](#collect)
3.  [問題を緩和する](#mitigate)

[App Service Web Apps](/services/app-service/web/) 各ステップでさまざまなオプションが用意されています。

<a name="observe" />
### 1.アプリケーションの動作を観察、監視する

#### サービス正常性を追跡する

Microsoft Azure は、サービスの中断やパフォーマンスの低下があるたびに、毎回公表します。 サービスの正常性を追跡できる、 [Azure ポータル](https://portal.azure.com/)します。 詳細については、次を参照してください。 [サービスのヘルス状態を追跡](insights-service-health.md)します。

#### Web アプリを監視する

Web アプリに問題が発生しているかどうかは、アプリを監視することによって確認することができます。 Web アプリのブレードで、[、 **要求とエラー** を並べて表示します。  **メトリック** ブレードが追加するすべてのメトリックを表示します。

Web アプリに関しては、次のメトリックを監視するようお勧めします。

-   平均メモリ ワーキング セット
-   平均応答時間
-   CPU 時間
-   メモリ ワーキング セット
-   Requests

![](./media/app-service-web-troubleshoot-performance-degradation/1-monitor-metrics.png)

詳細については、次を参照してください。

-   [Azure App Service の Web Apps の監視](web-sites-monitor.md)
-   [アラート通知の受信](insights-receive-alert-notifications.md)

#### Web エンドポイントの状態を監視する

Web アプリを実行している場合、 **標準** 価格レベル、Web アプリにより 3 つの地理的な場所からの 2 つのエンドポイントを監視できます。

エンドポイント監視では、地理的に分散した場所から Web URL の応答時間とアップタイムをテストする Web テストを構成します。 このテストでは、Web URL に対して HTTP get 操作を実行し、各場所から応答時間とアップタイムを測定します。 構成された各場所では、テストを 5 分ごとに実行します。

アップタイムは HTTP 応答コードを使用して監視され、応答時間はミリ秒単位で測定されます。 HTTP 応答コードが 400 以上である場合、または、応答に 30 秒以上かかる場合、監視テストは失敗します。 すべての指定した場所から監視テストが成功した場合、エンドポイントは利用可能と見なされます。

設定するを参照してください。 [方法: web エンドポイントの状態を監視](web-sites-monitor.md#webendpointstatus)します。

参照してください [を Azure Web サイトの保持とエンドポイントの監視 - Stefan Schackow 共演](/documentation/videos/azure-web-sites-endpoint-monitoring-and-staying-up/) エンドポイントの監視に関するビデオについてです。

#### 拡張機能を使用したアプリケーション パフォーマンスの監視

活用することにより、アプリケーションのパフォーマンスを監視することも _拡張機能をサイト_します。

App Service Web アプリにはそれぞれ拡張可能な管理エンド ポイントが用意されており、サイト拡張機能としてデプロイされている強力なツール一式を利用することができます。 これらのツールの範囲などのソース コード エディターから [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs.aspx) 管理接続されているリソース、MySQL データベースなどのツールを web アプリに接続します。

[Azure の Application Insights](/services/application-insights/) と [New Relic](/marketplace/partners/newrelic/newrelic/) は 2 つのパフォーマンスを利用できるサイト拡張機能を監視します。 New Relic を使用するには、実行時にエージェントをインストールします。 Azure Application Insights を使用するには、SDK でコードを再ビルドします。また、追加データにアクセスできる拡張機能もインストールできます。 SDK では、アプリの使用状況とパフォーマンスをさらに詳細に監視するコードを記述できます。

Application Insights を使用するのを参照してください。 [web アプリケーションのパフォーマンスを監視](app-insights-web-monitor-performance.md)します。

New Relic を使用するのを参照してください。 [Azure での New relic によるアプリケーション パフォーマンス管理](store-new-relic-cloud-services-dotnet-application-performance-management.md)します。

<a name="collect" />
### 2.データを収集する

####    Web アプリに対して診断ログを有効にする

Web Apps 環境は、Web サーバーと Web アプリケーションの両方のログ情報を診断する機能を備えています。 これらは論理的に Web サーバー診断とアプリケーション診断に分けられます。

##### Web サーバー診断

次の種類のログを有効または無効にできます。

-   **詳細なエラー ログ** -障害 (ステータス コード 400 以上) を示す HTTP ステータス コードのエラー情報を詳しく説明します。 このログには、サーバーがエラー コードを返した理由を特定するために役立つ情報が記録されている場合があります。
-   **失敗した要求トレース** -各コンポーネントにかかった時間、要求を処理するために使用する IIS コンポーネントのトレースなど、失敗した要求に関する詳細情報。 これは、Web アプリのパフォーマンスを向上させたり、特定の HTTP エラーの原因を分離しようとする場合に役立ちます。
-   **Web サーバーのログ記録** -W3C 拡張ログ ファイル形式を使用して、HTTP トランザクションに関する情報。 これは、サイトで処理された要求の数や特定の IP アドレスからの要求の数などの Web アプリの全体的なメトリックを特定するときに役立ちます。

##### アプリケーション診断

アプリケーション診断では、Web アプリケーションによって生成された情報を取り込むことができます。 ASP.NET アプリケーションは、`System.Diagnostics.Trace` クラスを使用してアプリケーション診断ログに情報を記録できます。

アプリケーション ログ記録を構成する方法の詳細な手順については、次を参照してください。 [Azure App Service での web アプリの診断ログを有効にする](web-sites-enable-diagnostic-log.md)です。

#### リモート プロファイリングの使用

Azure App Service では、Web Apps、API Apps、WebJobs のプロファイリングをリモートから実行できます。 プロセスの実行に予想以上に時間がかかる場合や、HTTP 要求の待機時間が通常よりも長く、かつプロセスの CPU 使用率も高い場合、対象プロセスをリモートからプロファイリングし、CPU サンプリング呼び出し履歴を取得して、プロセスのアクティビティとコードのホット パスを分析することができます。

詳細については、次を参照してください。 [Azure App Service でのリモート プロファイリング サポート](/blog/remote-profiling-support-in-azure-app-service)します。


#### Azure App Service サポート ポータルを使用する

Web Apps には、HTTP ログ、イベント ログ、処理ダンプなどを参照することによって、Web アプリに関連した問題をトラブルシューティングする機能があります。 サポート ポータルを使用してこのすべての情報にアクセスする **http://&lt;your アプリ名 >.scm.azurewebsites.net/Support**

一般的なトラブルシューティングの状況に合わせて、Azure App Service サポート ポータルには、3 つのタブが用意されています。次の 3 つの手順が想定されています。

1.  現在の動作を観察する
2.  診断情報を集め、組み込みのアナライザーを実行して分析する
3.  緩和する

問題が現在発生している場合はクリックして **分析** > **診断** > **今すぐ診断** の診断セッションを作成するには、HTTP を収集するログ、イベント ビューアのログで、メモリ ダンプ、PHP のエラー ログおよび PHP でレポートを処理します。

データの収集後にも、データに対する分析が実行され、HTML レポートが出力されます。

そのデータをダウンロードすることもできます。D:\home\data\DaaS フォルダーが既定の保存先となります。

アプリケーション サービスのサポートの Azure ポータルの詳細については、次を参照してください。 [Azure の web サイトのサイト拡張機能をサポートする新しい更新プログラム](/blog/new-updates-to-support-site-extension-for-azure-websites)します。

#### Kudu デバッグ コンソールを使用する

Web Apps には、ファイルのデバッグ、調査、アップロード用のデバッグ コンソールのほか、ご利用の環境についての情報を入手するための JSON エンドポイントが用意されています。 これと呼ばれますが、 _Kudu コンソール_ または _SCM ダッシュ ボード_ web アプリのです。

このダッシュ ボードをアクセスするには、リンクに移動して **https://&lt です。アプリ名 >.scm.azurewebsites.net/**します。

Kudu には次のような機能があります。

-   アプリケーションの環境設定
-   ログ ストリーム
-   診断ダンプ
-   デバッグ コンソール (Powershell のコマンドレットや基本的な DOS コマンドを実行可能)


Kudu にはもう 1 つ便利な機能があり、アプリケーションからファーストチャンス例外がスローされた場合に、Kudu と SysInternals ツール Procdump を使用してメモリ ダンプを作成することができます。 このメモリ ダンプはプロセスのスナップショットです。Web アプリに関して、通常より複雑な問題をトラブルシューティングできる場合も少なくありません。

Kudu で使用できる機能の詳細については、次を参照してください。
[Azure の web サイト チーム サービス ツールについて知っておくべき](/blog/windows-azure-websites-online-tools-you-should-know-about/)します。

<a name="mitigate" />
### 3.問題を緩和する

####    Web アプリをスケーリングする

高いパフォーマンスとスループットの Azure App Service であるアプリケーションを実行しているスケールを調整できます。 Web アプリのスケール アップには、2 つの関連する措置が伴います。1 つは、App Service プランの価格レベルを引き上げること、もう 1 つは、価格レベルを引き上げた後に特定の設定を構成することです。

スケーリングの詳細については、次を参照してください。 [Azure App Service で web アプリのスケール](web-sites-scale.md)します。

加えて、アプリケーションを複数のインスタンスで実行することもできます。 処理能力がアップするだけでなく、ある程度の耐障害性を確保することができます。 この場合、1 つのインスタンスでプロセスがダウンしても、他のインスタンスが要求の処理を続行します。

スケーリングは、[手動] または [自動] に設定することができます。

####    AutoHeal を使用する

AutoHeal は、選択された設定 (構成の変更、要求、メモリに基づく制限、要求の実行に必要な時間など) に従って、アプリのワーカー プロセスをリサイクルします。 ほとんどの場合、問題を回復するための一番の近道は、プロセスをリサイクルすることです。 Web アプリはいつでも、Azure ポータル内から直接、再起動できますが、AutoHeal はユーザーの介入なしでそれを自動的に実行します。 必要な作業は、Web アプリのルート web.config にいくつかのトリガーを追加することだけです。 .Net アプリケーション以外でも、これらの設定は同じように作用します。

詳細については、次を参照してください。 [Azure の Web サイトの自動修復](/blog/auto-healing-windows-azure-web-sites/)します。

####    Web アプリを再起動する

1 回限りの問題であれば、通常これが最も簡単な復旧方法です。  [Azure ポータル](https://portal.azure.com), 、web アプリのブレードで、停止、または、アプリを再起動するオプションがあります。

 ![](./media/app-service-web-troubleshoot-performance-degradation/2-restart.png)

Web アプリの管理には、Azure PowerShell を使用することもできます。 詳細については、次をご覧ください。
[Azure リソース マネージャーで Azure PowerShell を使用して](powershell-azure-resource-manager.md)します。

