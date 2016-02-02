<properties 
    pageTitle="Azure App Service Web Apps にグローバル Web プレゼンスを作成する" 
    description="このガイドでは、Azure App Service Web Apps に組織の (.COM) サイトをホストする方法 (技術概要) について説明します。これには、デプロイ、カスタム ドメイン、SSL、監視が含まれます。" 
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



# Azure App Service Web Apps にグローバル Web プレゼンスを作成する

[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps は、.COM サイトのグローバル web プレゼンスを確立するために必要なすべての機能です。 組織の規模に関係なく、ビジネス、ブランド認識、顧客とのコミュニケーションを促進するには、堅牢なセキュリティで保護された、スケーラブルなプラットフォームが必要です。 App Service Web Apps は、Microsoft がサポートするビジネス継続性により、企業のブランドとアイデンティティを維持します。
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

以下は、App Service Web Apps で実行されている .COM Web サイトの例です。 技術的な投資を最小限に抑えて、Web Apps をその他のサービスを構成することにより実現可能なことを図解しています。 **トポグラフィ内の要素をクリックして、詳細をご覧ください。**

<object type="image/svg+xml" data="https://sidneyhcontent.blob.core.windows.net/documentation/corp-website-visio.svg" width="100%" height="100%"></object>
> [AZURE.NOTE]
> このガイドでは、Azure App Service Web Apps で公開されている .COM サイトを実行するために調整されている最も一般的な領域とタスクの一部を紹介します。 ただし、Azure App Service Web Apps で実装できるその他の一般的なソリューションもあります。 これらのソリューションを確認するには、その他のガイドを参照して [デジタル マーケティング キャンペーン](web-sites-digital-marketing-application-solution-overview.md) と [ビジネス アプリケーション](web-sites-business-application-solution-overview.md)します。

## 最初から作成するか、既存のアセットを使用する

ギャラリーの人気の CMS から新しいサイトをすばやく作成するか、またはさまざまな言語とフレームワークから既存の Web アセットを App Service Web Apps に取り込みます。

Azure Marketplace には、Orchard、Umbraco、Drupal、および [WordPress] など、一般的な web サイトのコンテンツ管理システム (CMS) からのテンプレートが用意されています。 お気に入りの CMS フレーバーを使用して Web アプリを作成できます。 [MySQL] や [Azure SQL Database] などのニーズに合わせてさまざまなデータベース バックエンドから選択することができます。

既存のアセットは .NET、PHP、Java、Node.js、Pythonに関係なく App Service Web Apps で実行できます。 [FTP] の使い慣れたツールや、ソース管理システムを使用した Web アプリケーションへ移動することができます。 Web アプリは、[Visual Studio] などの一般的なソース管理オプションからの直接発行をサポートしています [Visual Studio Team Services], し、[Git] - ローカル、GitHub、BitBucket、DropBox、Mercurial など..

## 確実に発行する

既存のソース管理システムから継続的に直接発行し、コンテンツのライブテストを行うことによって、Web サイトを確実に発行します。

計画段階、プロトタイピング、サイトの開発の初期段階を表示できる web サイトの実際の動作バージョン [ステージング スロットに展開する] でライブ移行する前にサイトを App Service Web Apps のです。 ソースを統合することでは [継続的に発行する] ステージング スロットに Web アプリを使用して制御し、これを行う準備ができたらダウンタイムなしで実稼働環境にスワップします。 実稼働サイトで問題が発生した場合は、すぐにサイトの以前のバージョンにスワップできます。

また、実際の web サイトへの変更を計画するときを簡単に実行できます [実行 B のテスト] の機能を運用環境でテストを使用して提案される更新で、サイト設計に十分な情報に基づいて意思決定を行うために役立つの実際のユーザーの行動を分析します。

## ブランドとセキュリティ保護

無料の App Service Web Apps ドメインを使用するか、登録したドメイン名にマップし、証明機関の署名入り SSL 証明書でブランドを安全に保護します。

**\*.Azurewebsites.net** Web アプリで、web サイトを実行すると、ドメインは無料です。 [カスタム ドメインが] に、web サイトを割り当てることや、contoso.com などの GoDaddy などの DNS レジストリから取得したものです。

場合は、電子商取引の実行、またはその他の機密データを管理するユーザー情報の収集、ブランドの評判や顧客を [HTTPS] を保護できます。  **\*.Azurewebsites.net** は、既にドメイン名が、SSL 証明書に付属しており、カスタム ドメインを使用する場合、ことができます用に取り込む、SSL 証明書を Web Apps にします。 各 SSL 証明書には月単位の料金 (時間割り計算) が関連付けられています。 詳細については、[アプリ サービス料金の詳細] を参照してください。

## グローバル化

Azure Traffic Manager で諸地域のサイトにサービス提供し、Azure CDN でコンテンツを高速配信してグローバル化を目指します。

グローバル地域の顧客に対応するには、機能するには、最高のパフォーマンスを提供する地域のサイトへサイト訪問者をルーティングする [Azure Traffic Manager] を使用します。 または、複数の地域でホストされている Web アプリの複数のコピー間で、サイトの負荷を均等に分散できます。

[Azure CDN と web アプリの統合] でグローバルにユーザーに高速、静的コンテンツ稲妻を提供します。 Azure CDN は、[CDN ノード] の静的コンテンツをキャッシュ待機時間と、web サイトへの接続を最小限にすると、ユーザーに最も近いします。

## 最適化

自動スケールで自動的にスケールし、Azure Redis Cache でキャッシュして、Web ジョブでタスクをバックグラウンドで実行し、Azure Traffic Manager で高可用性を維持することで、.COM サイトを最適化します。

[スケール アップやスケール アウト] する App Service Web Apps の機能では、ワークロードのサイズに関係なく、.COM サイトのニーズを満たしています。 スケール アウト、web サイトを使用して手動で、 [Azure ポータル](https://portal.azure.com), 、[サービス管理 API] にプログラムを [PowerShell スクリプト]、または、自動スケール機能によって自動でします。 **標準**ホスティング プランでは、自動スケールにより CPU 使用率に基づいて Web サイトを自動的にスケールアウトします。 ベスト プラクティスは、[Troy Hunt] の [I learned Azure での web アプリの迅速なスケーリングに関する 10 のポイント] を参照してください。

[Azure Redis Cache] の応答性の高い web サイトを作成します。 バックエンド データベースからデータをし、[ASP.NET セッション状態] などの他のことをキャッシュし、[出力キャッシュ] には、それを使用します。

[Azure Traffic Manager] を使用して web サイトの高可用性を維持します。 Traffic Manager では、**フェールオーバー** を使用して、プライマリ サイトに問題がある場合に、トラフィックをセカンダリ サイトに自動的にルーティングします。

## 監視と分析

Azure またはサードパーティ製のツールで Web サイトのパフォーマンスを最新の状態に維持します。 Web サイトの重大なイベントでアラートを受信します。 Application Insight や HDInsight からの Web ログ分析でユーザー情報を簡単に取得します。

Web アプリのブレードで、[概要] web サイトの現在のパフォーマンスのメトリックとリソースのクォータを取得、 [Azure ポータル](https://portal.azure.com)します。アプリケーション全体の可用性、パフォーマンスと使用状況の 360 度ビューでは、高速かつ強力なトラブルシューティング、診断と使用状況の分析を提供する [Azure Application Insights] を使用します。または、詳細監視データ、web サイトを提供する [New Relic] などのサードパーティ ツールを使用します。

**標準**ホスティング プランでは、サイトが応答を停止するたびに、応答性監視サイトは電子メール通知を受信します。 詳細については、次を参照してください。 [方法: アラートの通知を受信し、Azure でアラート ルールの管理]。

## リッチ メディアを使用してすべてのデバイスに到達する

以下のようなリッチ メディアで .COM サイトを魅力的にします。

-  [Azure メディア サービス] でグローバルにビデオをストリームのアップロードと
-  [Azure Marketplace の SendGrid サービス] を持つユーザーに電子メールを送信します。

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
[custom domain]: web-sites-custom-domain-name.md 
[https]: web-sites-configure-ssl-certificate.md 
[app service pricing details]: /pricing/details/app-service/#ssl-connections 
[azure traffic manager]: http://www.hanselman.com/blog/CloudPowerHowToScaleAzureWebsitesGloballyWithTrafficManager.aspx 
[integrating your web app with azure cdn]: cdn-websites-with-cdn.md 
[cdn node]: https://msdn.microsoft.com/library/azure/gg680302.aspx 
[scale up and out]: web-sites-scale.md 
[azure management portal]: http://manage.windowsazure.com/ 
[service management api]: https://msdn.microsoft.com/library/azure/ee460799.aspx 
[powershell scripting]: https://msdn.microsoft.com/library/azure/jj152841.aspx 
[troy hunt]: https://twitter.com/troyhunt 
[10 things i learned about rapidly scaling web apps with azure]: http://www.troyhunt.com/2014/09/10-things-i-learned-about-rapidly.html 
[azure redis cache]: /blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/ 
[asp.net session state]: https://msdn.microsoft.com/library/azure/dn690522.aspx 
[output cache]: https://msdn.microsoft.com/library/azure/dn798898.aspx 
[quick glance]: web-sites-monitor.md 
[azure application insights]: http://blogs.msdn.com/b/visualstudioalm/archive/2015/01/07/application-insights-and-azure-websites.aspx 
[new relic]: ../store-new-relic-cloud-services-dotnet-application-performance-management.md 
[how to: receive alert notifications and manage alert rules in azure]: http://msdn.microsoft.com/library/windowsazure/dn306638.aspx 
[azure media services]: http://blogs.technet.com/b/cbernier/archive/2013/09/03/windows-azure-media-services-and-web-sites.aspx 
[sendgrid service in azure marketplace]: sendgrid-dotnet-how-to-send-email.md 

