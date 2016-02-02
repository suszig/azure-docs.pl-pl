<properties 
    pageTitle="Azure App Service Web Apps でデジタル マーケティング キャンペーンを作成する" 
    description="このガイドでは、Azure App Service Web Apps を使用してデジタル マーケティング キャンペーンを作成する方法 (技術概要) について説明します。デプロイメント、ソーシャル メディアの統合、スケールの戦略、監視も取り上げます。" 
    editor="jimbe" 
    manager="wpickett" 
    authors="cephalin" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/10/2015" 
    ms.author="cephalin"/>


# Azure App Service Web Apps でデジタル マーケティング キャンペーンを作成する

[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps は、デジタル マーケティング キャンペーンに最適な選択です。 デジタル マーケティング キャンペーンは通常、有効期間が短く、短期間のマーケティング目標を促進するためのものです。 この場合、主に 2 つのシナリオが考えられます。 1 つは、サードパーティのマーケティング会社が顧客のためにキャンペーンを作成し、販促期間中はそのマーケティング会社がキャンペーンを管理するシナリオです。 もう 1 つは、デジタル マーケティング キャンペーンをマーケティング会社が作成した後、そのリソースの所有権を顧客へ譲渡するシナリオです。 その後は、顧客が単独でデジタル マーケティング キャンペーンを運営管理します。 両方のシナリオにとって適切な一致です。
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

以下は、App Service Web Apps を使用したマルチ チャンネルのグローバル デジタル マーケティング キャンペーンの例です。 技術的な投資を最小限に抑えて、App Service Web Apps とその他のサービスを構成することにより実現可能なことを図解しています。 **トポグラフィ内の要素をクリックして、詳細をご覧ください。**

<object type="image/svg+xml" data="https://sidneyhcontent.blob.core.windows.net/documentation/digital-marketing-notitle.svg" width="100%" height="100%"></object>
> [AZURE.NOTE]
> このガイドでは、Azure App Service Web Apps でデジタル マーケティング キャンペーンを実行するために調整されている最も一般的な領域とタスクの一部を紹介します。 ただし、App Service Web Apps で実装できる一般的な解決方法はほかにもあります。 これらのソリューションを確認するには、その他のガイドを参照して [グローバル Web プレゼンス](web-sites-global-web-presence-solution-overview.md) と [ビジネス アプリケーション](web-sites-business-application-solution-overview.md)します。

## 最初から作成するか、既存のアセットを使用する

ギャラリーで人気の CMS から新しい Web アプリをすばやく作成するか、さまざまな言語やフレームワークから既存の Web アセットを App Service Web Apps に取り込みます。

Azure Marketplace には、Orchard、Umbraco、Drupal、および [WordPress] など、一般的な web サイトのコンテンツ管理システム (CMS) からのテンプレートが用意されています。 お気に入りの CMS フレーバーを使用して Web アプリを作成できます。 [MySQL] や [Azure SQL Database] などのニーズに合わせてさまざまなデータベース バックエンドから選択することができます。

既存の Web アセットは .NET、PHP、Java、Node.js,、Python に関係なく Web Apps で実行できます。 使い慣れた [FTP] ツールを使用して Web アプリに移動することができます。 デジタル マーケティング キャンペーンを頻繁に作成している場合、ソース管理システムに既存の Web アセットが存在している可能性があります。 [Visual Studio] などの一般的なソース管理オプションから直接 Web アプリにデプロイすることができます [Visual Studio Team Services], [git] - ローカル、GitHub、BitBucket、DropBox、Mercurial など..

## 俊敏性を保つ

既存のソース管理から公開し続け、App Service Web Apps で A/B テストを実行して俊敏性を保ちます。

計画段階、プロトタイピング、開発、web アプリの初期段階、顧客とわかりますキャンペーン アプリの実際の動作バージョン [ステージング スロットに展開する] でライブ移行する前に、web アプリのです。 ソースを統合することでコントロールを [継続的に発行する] ステージング スロットに App Service Web Apps をし、準備ができたらダウンタイムなしで実稼働環境にスワップします。

また、ライブ web アプリへの変更を計画するとき、簡単に実行できます [実行 B のテスト] の機能を運用環境でテストを使用して提案される更新で、アプリケーションの設計に十分な情報に基づいて意思決定を行うために役立つの実際のユーザーの行動を分析します。


## ソーシャルに

App Service Web Apps のデジタル マーケティング キャンペーンは Facebook や Twitter などの人気プロバイダーで認証することでソーシャル メディアと統合できます。 ASP.NET アプリケーションでは、この方法の例は、次を参照してください。 [認証および SQL DB と ASP.NET MVC アプリの作成と Azure App Service にデプロイ] です。

さらに、各ソーシャル メディア サイトでは通常、.NET やその他のフレームワークから統合できる別の方法に関する情報が提供されます。

## リッチ メディアを使用してすべてのデバイスに到達する

次のように、その他の Azure サービスでデジタル マーケティング キャンペーンを強化できます。

-  [Azure メディア サービス] でグローバルにビデオをストリームのアップロードと
-  [Azure Marketplace の SendGrid サービス] を持つユーザーに電子メールを送信します。
-  Windows、iOS、および [モバイル サービス] で Android デバイスでプレゼンスを確立します。
-  [通知ハブ] で多くのデバイスにプッシュ通知を送信します。

## グローバル化

Azure Traffic Manager で諸地域のサイトにサービス提供し、Azure CDN でコンテンツを高速配信してグローバル化を目指します。

グローバル地域の顧客に対応するには、機能するには、最高のパフォーマンスを提供する地域のサイトへサイト訪問者をルーティングする [Azure Traffic Manager] を使用します。 または、複数の地域でホストされている Web アプリの複数のコピー間で、サイトの負荷を均等に分散できます。

[Azure CDN と web アプリの統合] でグローバルにユーザーに高速、静的コンテンツ稲妻を提供します。 Azure CDN は、[CDN ノード] の静的コンテンツをキャッシュ待機時間と、web アプリへの接続を最小限にすると、ユーザーに最も近いします。

## 最適化

自動スケールで自動的にスケールし、Azure Redis Cache でキャッシュして、Web ジョブでタスクをバックグラウンドで実行し、Azure Traffic Manager で高可用性を維持することで、Web アプリを最適化します。

[スケール アップやスケール アウト] する App Service Web Apps の機能は、デジタル マーケティング キャンペーンのケースをある予期しないワークロードに最適です。 スケール アウト、web アプリを使用して手動で、 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715), 、[サービス管理 API] にプログラムを [PowerShell スクリプト]、または、自動スケール機能によって自動でします。 **標準**階層では、自動スケールにより、CPU 使用率に基づいて Web アプリを自動的にスケールアウトできます。 この機能により、ユーザーのアクティビティを基に必要に応じて Web アプリをスケールアウトできるため、俊敏性を最大限にすると同時にコストを最小限に抑えることができます。 ベスト プラクティスは、[Troy Hunt] の [I learned Azure での web アプリの迅速なスケーリングに関する 10 のポイント] を参照してください。

[Azure Redis Cache] の応答性の高い web アプリを作成します。 バックエンド データベースからデータをし、[ASP.NET セッション状態] などの他のことをキャッシュし、[出力キャッシュ] には、それを使用します。

[Azure Traffic Manager] を使用して、Web アプリの高可用性を維持します。 Traffic Manager では、**フェールオーバー** を使用して、プライマリ サイトに問題がある場合に、トラフィックをセカンダリ サイトに自動的にルーティングします。

## 監視と分析

Azure やサードパーティー製のツールで Web アプリのパフォーマンスを最新の状態に維持します。 重要な Web アプリのイベントでアラートを受信します。 Application Insight や HDInsight からの Web ログ分析でユーザー情報を簡単に取得します。

Web アプリのブレードで web アプリの現在のパフォーマンスの [概要] メトリックとリソースのクォータを取得、 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715)します。アプリケーション全体の可用性、パフォーマンスと使用状況の 360 度ビューでは、高速かつ強力なトラブルシューティング、診断と使用状況の分析を提供する [Azure Application Insights] を使用します。または、詳細監視データ、web アプリを提供する [New Relic] などのサード パーティ製ツールを使用します。

**Standard** レベルでは、アプリの応答性を監視して、Web アプリの応答が停止する際にメール通知を受信します。 詳細については、次を参照してください。 [方法: アラートの通知を受信し、Azure でアラート ルールの管理]。

## その他のリソース

- [App Service Web Apps のドキュメント](/services/app-service/web/)
- [Azure App Service Web Apps の学習マップ](websites-learning-map.md)
- [Azure Web ブログ](/blog/topics/web/)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]


[azure app service]: /services/app-service/web/ 
[wordpress]: web-sites-php-web-site-gallery.md 
[mysql]: web-sites-php-mysql-deploy-use-git.md 
[azure sql database]: web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md 
[ftp]: web-sites-deploy.md#ftp 
[visual studio]: web-sites-dotnet-get-started.md 
[visual studio team services]: ../cloud-services-continuous-delivery-use-vso.md 
[git]: web-sites-publish-source-control.md 
[deploying to a staging slot]: web-sites-staged-publishing.md 
[continuously publish]: http://rickrainey.com/2014/01/21/continuous-deployment-github-with-azure-web-sites-and-staged-publishing/ 
[run a/b tests]: http://blogs.msdn.com/b/tomholl/archive/2014/11/10/a-b-testing-with-azure-websites.aspx 
[create an asp.net mvc app with auth and sql db and deploy to azure app service]: web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md 
[azure media services]: http://blogs.technet.com/b/cbernier/archive/2013/09/03/windows-azure-media-services-and-web-sites.aspx 
[sendgrid service in azure marketplace]: sendgrid-dotnet-how-to-send-email.md 
[mobile services]: ../mobile-services-dotnet-backend-windows-store-dotnet-push-notifications-app-users.md 
[notification hub]: ../mobile-services-dotnet-backend-windows-store-dotnet-push-notifications-app-users.md 
[azure traffic manager]: http://www.hanselman.com/blog/CloudPowerHowToScaleAzureWebsitesGloballyWithTrafficManager.aspx 
[integrating your web app with azure cdn]: cdn-websites-with-cdn.md 
[cdn node]: https://msdn.microsoft.com/library/azure/gg680302.aspx 
[scale up and out]: /manage/services/web-sites/how-to-scale-websites/ 
[azure management portal]: http://manage.windowsazure.com/ 
[service management api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx 
[powershell scripting]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx 
[troy hunt]: https://twitter.com/troyhunt 
[10 things i learned about rapidly scaling web apps with azure]: http://www.troyhunt.com/2014/09/10-things-i-learned-about-rapidly.html 
[azure redis cache]: /blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/ 
[asp.net session state]: https://msdn.microsoft.com/library/azure/dn690522.aspx 
[output cache]: https://msdn.microsoft.com/library/azure/dn798898.aspx 
[quick glance]: /manage/services/web-sites/how-to-monitor-websites/ 
[azure application insights]: http://blogs.msdn.com/b/visualstudioalm/archive/2015/01/07/application-insights-and-azure-websites.aspx 
[new relic]: /develop/net/how-to-guides/new-relic/ 
[how to: receive alert notifications and manage alert rules in azure]: http://msdn.microsoft.com/library/windowsazure/dn306638.aspx 
[gitstaging]: http://www.bradygaster.com/post/multiple-environments-with-windows-azure-web-sites 

