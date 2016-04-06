<properties 
    pageTitle="Azure App Service Web Apps でデジタル マーケティング キャンペーンを作成する" 
    description="このガイドでは、Azure App Service Web Apps を使用してデジタル マーケティング キャンペーンを作成する方法 (技術概要) について説明します。 デプロイメント、ソーシャル メディアの統合、スケールの戦略、監視も取り上げます。" 
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

Azure Marketplace は、一般的な web サイトからテンプレート Orchard、Umbraco、Drupal などのコンテンツ管理システム (CMS) と [WordPress]します。 お気に入りの CMS フレーバーを使用して Web アプリを作成できます。 など、ニーズに合わせてさまざまなデータベース バックエンドから選択できます [Azure SQL Database] と [MySQL]します。

既存の Web アセットは .NET、PHP、Java、Node.js,、Python に関係なく Web Apps で実行できます。 使い慣れたを使用して Web アプリに移動する [FTP] ツールです。 デジタル マーケティング キャンペーンを頻繁に作成している場合、ソース管理システムに既存の Web アセットが存在している可能性があります。 できますを Web アプリから直接デプロイする一般的なソース制御オプションなど [Visual Studio], 、[Visual Studio Team Services], 、および [Git] -ローカル、GitHub、BitBucket、DropBox、Mercurial など..

## 俊敏性を保つ

既存のソース管理から公開し続け、App Service Web Apps で A/B テストを実行して俊敏性を保ちます。 

計画段階、プロトタイピング、開発、web アプリの初期段階、顧客とわかりますキャンペーン アプリの実際の動作バージョンして運用する前に [deploying to a staging slot] web アプリのです。 App Service Web Apps では、ソース管理を統合すると、することができます [continuously publish] 、ステージング スロット、および準備ができたらダウンタイムなしで実稼働環境にスワップします。 

また、ライブ web アプリへの変更を計画するとき、簡単に実行できます [run A/B tests] で機能を運用環境でテストを使用して提案される更新で、アプリケーションの設計に十分な情報に基づいて意思決定を行うために役立つ実際のユーザーの行動を分析します。


## ソーシャルに

App Service Web Apps のデジタル マーケティング キャンペーンは Facebook や Twitter などの人気プロバイダーで認証することでソーシャル メディアと統合できます。 ASP.NET アプリケーションでは、この方法の例は、次を参照してください。 [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service]します。 

さらに、各ソーシャル メディア サイトでは通常、.NET やその他のフレームワークから統合できる別の方法に関する情報が提供されます。

## リッチ メディアを使用してすべてのデバイスに到達する

次のように、その他の Azure サービスでデジタル マーケティング キャンペーンを強化できます。

-  グローバルにビデオをストリームのアップロードと [Azure Media Services]
-  持つユーザーに電子メールを送信します。 [SendGrid service in Azure Marketplace]
-  Windows、iOS、および使用して Android デバイスでプレゼンスを確立します。 [Mobile Services]
-  多くのデバイスにプッシュ通知を送信します。 [Notification Hub]

## グローバル化

Azure Traffic Manager で諸地域のサイトにサービス提供し、Azure CDN でコンテンツを高速配信してグローバル化を目指します。

グローバル地域の顧客に対応するには、機能を使用して [Azure Traffic Manager] 最高のパフォーマンスを提供する地域のサイトへサイト訪問者をルーティングします。 または、複数の地域でホストされている Web アプリの複数のコピー間で、サイトの負荷を均等に分散できます。

静的コンテンツ世界中配信ユーザーに高速 [integrating your web app with Azure CDN]します。 Azure CDN キャッシュの静的コンテンツ、 [CDN node] 待機時間と、web アプリへの接続を最小限にすると、ユーザーに最も近いします。

## 最適化

自動スケールで自動的にスケールし、Azure Redis Cache でキャッシュして、Web ジョブでタスクをバックグラウンドで実行し、Azure Traffic Manager で高可用性を維持することで、Web アプリを最適化します。

App Service Web Apps の能力 [scale up and out] は予測不可能なワークロードに最適のデジタル マーケティング キャンペーンのケースがあります。 スケール アウト、web アプリを使用して手動で、 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715), 通じてプログラムで、 [Service Management API] または [PowerShell scripting], 、自動スケール機能によって自動でします。  **標準** 階層、自動スケールにより CPU 使用率に基づいて自動的に web アプリにスケール アウトします。 この機能により、ユーザーのアクティビティを基に必要に応じて Web アプリをスケールアウトできるため、俊敏性を最大限にすると同時にコストを最小限に抑えることができます。 ベスト プラクティスについては、次を参照してください。 [Troy Hunt]の [10 things I learned about rapidly scaling web apps with Azure]します。

応答性の高い web アプリを作成、 [Azure Redis Cache]します。 使用してデータをキャッシュするバックエンド データベースおよびその他の点からなど、 [ASP.NET session state] と [output cache]します。

使用して web アプリの高可用性 [Azure Traffic Manager]します。 使用して、 **フェールオーバー** メソッド、Traffic Manager は、プライマリ サイトで問題がある場合にトラフィックをセカンダリ サイトに自動的にルーティングします。

## 監視と分析

Azure やサードパーティー製のツールで Web アプリのパフォーマンスを最新の状態に維持します。 重要な Web アプリのイベントでアラートを受信します。 Application Insight や HDInsight からの Web ログ分析でユーザー情報を簡単に取得します。 

取得、 [quick glance] の web アプリの現在のパフォーマンス メトリックとリソースのクォータを web アプリのブレードで、 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715)します。 アプリケーション全体の可用性、パフォーマンスと使用状況の 360 度ビューでは、使用 [Azure Application Insights] に高速かつ強力なトラブルシューティング、診断と使用状況の分析を提供します。 またはなどのサード パーティ製ツールを使用して [New Relic] web アプリのデータの監視の詳細を提供します。

 **標準** 層、アプリケーションの応答性の監視電子メール通知を受け取る web アプリが応答しなくなったときにします。 詳細については、次を参照してください。 [How to: Receive Alert Notifications and Manage Alert Rules in Azure]します。

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

[Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service]:web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md

[Azure Media Services]:http://blogs.technet.com/b/cbernier/archive/2013/09/03/windows-azure-media-services-and-web-sites.aspx
[SendGrid service in Azure Marketplace]:sendgrid-dotnet-how-to-send-email.md
[Mobile Services]:../mobile-services-dotnet-backend-windows-store-dotnet-push-notifications-app-users.md
[Notification Hub]:../mobile-services-dotnet-backend-windows-store-dotnet-push-notifications-app-users.md

[Azure Traffic Manager]:http://www.hanselman.com/blog/CloudPowerHowToScaleAzureWebsitesGloballyWithTrafficManager.aspx
[integrating your web app with Azure CDN]:cdn-websites-with-cdn.md 
[CDN node]:https://msdn.microsoft.com/library/azure/gg680302.aspx

[scale up and out]:/manage/services/web-sites/how-to-scale-websites/
[Azure Management Portal]:http://manage.windowsazure.com/
[Service Management API]:http://msdn.microsoft.com/library/windowsazure/ee460799.aspx
[PowerShell scripting]:http://msdn.microsoft.com/library/windowsazure/jj152841.aspx
[Troy Hunt]:https://twitter.com/troyhunt
[10 things I learned about rapidly scaling web apps with Azure]:http://www.troyhunt.com/2014/09/10-things-i-learned-about-rapidly.html
[Azure Redis Cache]:/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/
[ASP.NET session state]:https://msdn.microsoft.com/library/azure/dn690522.aspx
[output cache]:https://msdn.microsoft.com/library/azure/dn798898.aspx

[quick glance]:/manage/services/web-sites/how-to-monitor-websites/
[Azure Application Insights]:http://blogs.msdn.com/b/visualstudioalm/archive/2015/01/07/application-insights-and-azure-websites.aspx
[New Relic]:/develop/net/how-to-guides/new-relic/
[How to: Receive Alert Notifications and Manage Alert Rules in Azure]:http://msdn.microsoft.com/library/windowsazure/dn306638.aspx

  
  [gitstaging]:http://www.bradygaster.com/post/multiple-environments-with-windows-azure-web-sites  
 


