<properties 
    pageTitle="Azure App Service Web Apps にグローバル Web プレゼンスを作成する" 
    description="このガイドでは、Azure App Service Web Apps に組織の (.COM) サイトをホストする方法 (技術概要) について説明します。 これには、デプロイ、カスタム ドメイン、SSL、監視が含まれます。" 
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

Azure Marketplace は、一般的な web サイトからテンプレート Orchard、Umbraco、Drupal などのコンテンツ管理システム (CMS) と [WordPress]します。 お気に入りの CMS フレーバーを使用して Web アプリを作成できます。 など、ニーズに合わせてさまざまなデータベース バックエンドから選択できます [Azure SQL Database] と [MySQL]します。

既存のアセットは .NET、PHP、Java、Node.js、Pythonに関係なく App Service Web Apps で実行できます。 使い慣れたを使用して Web アプリに移動する [FTP] ツールや、ソース管理システムです。 Web Apps など一般的なソース管理オプションからの直接発行をサポート [Visual Studio], 、[Visual Studio Team Services], と [Git] -ローカル、GitHub、BitBucket、DropBox、Mercurial など..

## 確実に発行する

既存のソース管理システムから継続的に直接発行し、コンテンツのライブテストを行うことによって、Web サイトを確実に発行します。 

計画段階、プロトタイピング、サイトの開発の初期段階を表示できる web サイトの実際の動作バージョンして運用する前に [deploying to a staging slot] App Service Web Apps 上のサイトのです。 Web アプリを使用して、ソース管理を統合すると、することができます [continuously publish] 、ステージング スロットに、これを行う準備ができたらダウンタイムなしで実稼働環境にスワップします。 実稼働サイトで問題が発生した場合は、すぐにサイトの以前のバージョンにスワップできます。 

また、実際の web サイトへの変更を計画するときを簡単に実行できます [run A/B tests] で機能を運用環境でテストを使用して提案される更新で、サイト設計に十分な情報に基づいて意思決定を行うために役立つ実際のユーザーの行動を分析します。

## ブランドとセキュリティ保護

無料の App Service Web Apps ドメインを使用するか、登録したドメイン名にマップし、証明機関の署名入り SSL 証明書でブランドを安全に保護します。

 **\*.Azurewebsites.net** Web アプリで、web サイトを実行すると、ドメインは無料です。 または、自分の web サイトを割り当てることができます、 [custom domain] (contoso.com など) - GoDaddy などの DNS レジストリから取得したものです。

ブランドの評判や顧客を保護する場合は、電子商取引の実行、またはその他の機密データを管理するユーザー情報の収集、 [HTTPS]します。  **\*.Azurewebsites.net** は、既にドメイン名が、SSL 証明書に付属しており、カスタム ドメインを使用する場合、ことができます用に取り込む、SSL 証明書を Web Apps にします。 各 SSL 証明書には月単位の料金 (時間割り計算) が関連付けられています。 詳細については、次を参照してください。 [App Service Pricing Details]します。

## グローバル化

Azure Traffic Manager で諸地域のサイトにサービス提供し、Azure CDN でコンテンツを高速配信してグローバル化を目指します。

グローバル地域の顧客に対応するには、機能を使用して [Azure Traffic Manager] 最高のパフォーマンスを提供する地域のサイトへサイト訪問者をルーティングします。 または、複数の地域でホストされている Web アプリの複数のコピー間で、サイトの負荷を均等に分散できます。

静的コンテンツ世界中配信ユーザーに高速 [integrating your web app with Azure CDN]します。 Azure CDN キャッシュの静的コンテンツ、 [CDN node] 待機時間と、web サイトへの接続を最小限にすると、ユーザーに最も近いします。

## 最適化

自動スケールで自動的にスケールし、Azure Redis Cache でキャッシュして、Web ジョブでタスクをバックグラウンドで実行し、Azure Traffic Manager で高可用性を維持することで、.COM サイトを最適化します。

App Service Web Apps の能力 [scale up and out] ワークロードのサイズに関係なく、.COM サイトのニーズを満たします。 スケール アウト、web サイトを使用して手動で、 [Azure ポータル](https://portal.azure.com), 通じてプログラムで、 [Service Management API] または [PowerShell scripting], 、自動スケール機能によって自動でします。  **標準** ホスティング プラン、自動スケールにより CPU 使用率に基づいて自動的に web サイトを拡張します。 ベスト プラクティスについては、次を参照してください。 [Troy Hunt]の [10 things I learned about rapidly scaling web apps with Azure]します。

Web サイトを作成すると応答性の高い、 [Azure Redis Cache]します。 使用してデータをキャッシュするバックエンド データベースおよびその他の点からなど、 [ASP.NET session state] と [output cache]します。

使用して、web サイトの高可用性 [Azure Traffic Manager]します。 使用して、 **フェールオーバー** メソッド、Traffic Manager は、プライマリ サイトで問題がある場合にトラフィックをセカンダリ サイトに自動的にルーティングします。

## 監視と分析

Azure またはサードパーティ製のツールで Web サイトのパフォーマンスを最新の状態に維持します。 Web サイトの重大なイベントでアラートを受信します。 Application Insight や HDInsight からの Web ログ分析でユーザー情報を簡単に取得します。 

取得、 [quick glance] の web サイトの現在のパフォーマンス メトリックとリソースのクォータを web アプリのブレードで、 [Azure ポータル](https://portal.azure.com)します。 アプリケーション全体の可用性、パフォーマンスと使用状況の 360 度ビューでは、使用 [Azure Application Insights] に高速かつ強力なトラブルシューティング、診断と使用状況の分析を提供します。 またはなどのサード パーティ製ツールを使用して [New Relic] 詳細監視の web サイトのデータを提供します。

 **標準** ホスティング プラン、応答性監視サイト電子メール通知を受け取る、サイトが応答しなくなったときにします。 詳細については、次を参照してください。 [How to: Receive Alert Notifications and Manage Alert Rules in Azure]します。

## リッチ メディアを使用してすべてのデバイスに到達する

以下のようなリッチ メディアで .COM サイトを魅力的にします。

-  グローバルにビデオをストリームのアップロードと [Azure Media Services]
-  持つユーザーに電子メールを送信します。 [SendGrid service in Azure Marketplace]

## その他のリソース

- [App Service Web アプリのドキュメント](/services/app-service/web/)
- [Azure App Service Web アプリの学習マップ](websites-learning-map.md)
- [Azure Web ブログ](/blog/topics/web/)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]


[Azure App Service]: /services/app-service/web/

[WordPress]:web-sites-php-web-site-gallery.md
[MySQL]:web-sites-php-mysql-deploy-use-git.md
[Azure SQL Database]:web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md
[FTP]:web-sites-deploy.md#ftp
[Visual Studio]:web-sites-dotnet-get-started.md
[Visual Studio Team Services]:../cloud-services-continuous-delivery-use-vso.md
[Git]:web-sites-publish-source-control.md

[deploying to a staging slot]:web-sites-staged-publishing.md 
[continuously publish]:http://rickrainey.com/2014/01/21/continuous-deployment-github-with-azure-web-sites-and-staged-publishing/
[run A/B tests]:http://blogs.msdn.com/b/tomholl/archive/2014/11/10/a-b-testing-with-azure-websites.aspx

[custom domain]:web-sites-custom-domain-name.md
[HTTPS]:web-sites-configure-ssl-certificate.md
[App Service Pricing Details]: /pricing/details/app-service/#ssl-connections

[Azure Traffic Manager]:http://www.hanselman.com/blog/CloudPowerHowToScaleAzureWebsitesGloballyWithTrafficManager.aspx
[integrating your web app with Azure CDN]:cdn-websites-with-cdn.md 
[CDN node]:https://msdn.microsoft.com/library/azure/gg680302.aspx

[scale up and out]:web-sites-scale.md
[Azure Management Portal]:http://manage.windowsazure.com/
[Service Management API]:https://msdn.microsoft.com/library/azure/ee460799.aspx
[PowerShell scripting]:https://msdn.microsoft.com/library/azure/jj152841.aspx
[Troy Hunt]:https://twitter.com/troyhunt
[10 things I learned about rapidly scaling web apps with Azure]:http://www.troyhunt.com/2014/09/10-things-i-learned-about-rapidly.html
[Azure Redis Cache]:/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/
[ASP.NET session state]:https://msdn.microsoft.com/library/azure/dn690522.aspx
[output cache]:https://msdn.microsoft.com/library/azure/dn798898.aspx

[quick glance]:web-sites-monitor.md
[Azure Application Insights]:http://blogs.msdn.com/b/visualstudioalm/archive/2015/01/07/application-insights-and-azure-websites.aspx
[New Relic]:../store-new-relic-cloud-services-dotnet-application-performance-management.md
[How to: Receive Alert Notifications and Manage Alert Rules in Azure]:http://msdn.microsoft.com/library/windowsazure/dn306638.aspx

[Azure Media Services]:http://blogs.technet.com/b/cbernier/archive/2013/09/03/windows-azure-media-services-and-web-sites.aspx
[SendGrid service in Azure Marketplace]:sendgrid-dotnet-how-to-send-email.md

 


