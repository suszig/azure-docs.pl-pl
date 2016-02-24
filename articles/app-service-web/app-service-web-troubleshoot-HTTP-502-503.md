<properties
    pageTitle="トラブルシューティング: HTTP 502/503 が原因で Web アプリが利用できない"
    description="この記事では、Azure App Service でホストされている Web アプリで発生した HTTP 502/503 エラーのトラブルシューティングについて取り上げます。"
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

# トラブルシューティング: HTTP 502/503 が原因で Web アプリが利用できない

この記事でホストされている web アプリの HTTP 502/503 エラーを解決できます。 [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)します。

この記事のどの時点でもその他のヘルプを必要がある場合で Azure のエキスパートに連絡することができます [MSDN Azure とスタック オーバーフロー フォーラム](http://azure.microsoft.com/support/forums/)します。 または、Azure サポート インシデントを送信できます。 移動して、 [Azure サポート サイト](http://azure.microsoft.com/support/options/) ] をクリック **サポートにお問い合わせ**します。

## 症状

ブラウザーで Web アプリにアクセスすると、HTTP "502 Bad Gateway" または HTTP "503 Service Unavailable" が返されます。

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

####    サービス正常性を追跡する

Microsoft Azure は、サービスの中断やパフォーマンスの低下があるたびに、毎回公表します。 サービスの正常性を追跡できる、 [Azure ポータル](https://portal.azure.com/)します。 詳細については、次を参照してください。 [サービスのヘルス状態を追跡](insights-service-health.md)します。

####    Web アプリを監視する

Web アプリに問題が発生しているかどうかは、アプリを監視することによって確認することができます。 Web アプリのブレードで、[、 **要求とエラー** を並べて表示します。  **メトリック** ブレードが追加するすべてのメトリックを表示します。

Web アプリに関しては、次のメトリックを監視するようお勧めします。

-   平均メモリ ワーキング セット
-   平均応答時間
-   CPU 時間
-   メモリ ワーキング セット
-   Requests

![](./media/app-service-web-troubleshoot-HTTP-502-503/1-monitor-metrics.png)

詳細については、次を参照してください。

-   [Azure App Service の Web Apps の監視](web-sites-monitor.md)
-   [アラート通知の受信](insights-receive-alert-notifications.md)

<a name="collect" />
### 2.データを収集する

####    Azure App Service サポート ポータルを使用する

Web Apps には、HTTP ログ、イベント ログ、処理ダンプなどを参照することによって、Web アプリに関連した問題をトラブルシューティングする機能があります。 サポート ポータルを使用してこのすべての情報にアクセスする **http://&lt;your アプリ名 >.scm.azurewebsites.net/Support**

一般的なトラブルシューティングの状況に合わせて、Azure App Service サポート ポータルには、3 つのタブが用意されています。次の 3 つの手順が想定されています。

1.  現在の動作を観察する
2.  診断情報を集め、組み込みのアナライザーを実行して分析する
3.  緩和する

問題が現在発生している場合はクリックして **分析** > **診断** > **今すぐ診断** の診断セッションを作成するには、HTTP を収集するログ、イベント ビューアのログで、メモリ ダンプ、PHP のエラー ログおよび PHP でレポートを処理します。

データの収集後にも、データに対する分析が実行され、HTML レポートが出力されます。

そのデータをダウンロードすることもできます。D:\home\data\DaaS フォルダーが既定の保存先となります。

アプリケーション サービスのサポートの Azure ポータルの詳細については、次を参照してください。 [Azure の web サイトのサイト拡張機能をサポートする新しい更新プログラム](/blog/new-updates-to-support-site-extension-for-azure-websites)します。

####    Kudu デバッグ コンソールを使用する

Web Apps には、ファイルのデバッグ、調査、アップロード用のデバッグ コンソールのほか、ご利用の環境についての情報を入手するための JSON エンドポイントが用意されています。 これと呼ばれますが、 _Kudu コンソール_ または _SCM ダッシュ ボード_ web アプリのです。

このダッシュ ボードをアクセスするには、リンクに移動して **https://&lt です。アプリ名 >.scm.azurewebsites.net/**します。

Kudu には次のような機能があります。

-   アプリケーションの環境設定
-   ログ ストリーム
-   診断ダンプ
-   デバッグ コンソール (Powershell のコマンドレットや基本的な DOS コマンドを実行可能)


Kudu にはもう 1 つ便利な機能があり、アプリケーションからファーストチャンス例外がスローされた場合に、Kudu と SysInternals ツール Procdump を使用してメモリ ダンプを作成することができます。 このメモリ ダンプはプロセスのスナップショットです。Web アプリに関して、通常より複雑な問題をトラブルシューティングできる場合も少なくありません。

Kudu で使用できる機能の詳細については、次を参照してください。
[Azure Websites online tools ツールについて知っておくべき](/blog/windows-azure-websites-online-tools-you-should-know-about/)します。

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

 ![](./media/app-service-web-troubleshoot-HTTP-502-503/2-restart.png)

Web アプリの管理には、Azure PowerShell を使用することもできます。 詳細については、次をご覧ください。
[Azure リソース マネージャーで Azure PowerShell を使用して](powershell-azure-resource-manager.md)します。

