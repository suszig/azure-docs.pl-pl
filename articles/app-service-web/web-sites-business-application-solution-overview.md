<properties 
    pageTitle="Azure App Service での基幹業務 Web アプリの作成" 
    description="このガイドでは、Azure App Service Web Apps を使用してイントラネットの基幹業務アプリケーションを作成する方法 (技術概要) について説明します。 これには、認証方式、Service Bus リレー、監視も含まれます。" 
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



# Azure App Service での基幹業務 Web アプリの作成

[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps は、基幹業務アプリケーションに最適な選択です。 こうしたアプリケーションは、イントラネット アプリケーションで、社内業務用にセキュリティで保護する必要があります。 通常は社内ディレクトリやオンプレミス データとサービスの一部へのアクセス、またはその統合に対する認証が必要になります。 

基幹業務アプリケーションを App Service Web Apps に移行すると、次のような大きなメリットがあります。

-  年間業績評価を処理するアプリケーションなど、動的なワークロードをスケールアップ/スケールダウンできます。 考査期間になると (特に大規模な会社では) トラフィックが急増します。 Azure では負荷が増大する考査期間中にインスタンスを増やし、それ以外の期間は元に戻すことができるので、コスト節約につながります。 
-  インフラストラクチャの購入と管理よりもアプリケーションの開発に集中できます。
-  従業員やパートナーが任意の場所からアプリケーションを使用する際のサポートが強化されます。 アプリケーションを使用する際、ユーザーは社内ネットワークに接続する必要がありません。IT スタッフは複雑なリバース プロキシの設定から解放されます。 社内アプリケーションへのアクセスを保護する手段として、いくつかの認証方式があります。

以下は、App Service Web Apps で実行されている、基幹業務アプリケーションの例です。 技術的な投資を最小限に抑えて、Web Apps をその他のサービスを構成することにより実現可能なことを図解しています。 **トポグラフィ内の要素をクリックして、詳細をご覧ください。** 

<object type="image/svg+xml" data="https://sidneyhcontent.blob.core.windows.net/documentation/web-app-notitle.svg" width="100%" height="100%"></object>

> [AZURE.NOTE]
> このガイドで取り上げるのは基幹業務アプリケーションで必要となる最も一般的な分野やタスクですが、 Azure App Service Web Apps には特殊なニーズに対応できるその他の機能も備わっています。 これらの機能を確認するにも、その他のガイドを参照して [グローバル Web プレゼンス](web-sites-global-web-presence-solution-overview.md) と [デジタル マーケティング キャンペーン](web-sites-digital-marketing-application-solution-overview.md)します。

## 既存のアセットの表示

さまざまな言語やフレームワークから既存の web アセットを App Service Web Apps に表示させます。

既存のアセットは .NET、PHP、Java、Node.js、Pythonに関係なく App Service Web Apps で実行できます。 使い慣れたを使用して Web アプリに移動する [FTP] ツールや、ソース管理システムです。 Web Apps など一般的なソース管理オプションからの直接発行をサポート [Visual Studio], 、[Visual Studio Team Services], と [Git] -ローカル、GitHub、BitBucket、DropBox、Mercurial など..

## アセットのセキュリティ保護

暗号化によってアセットを保護し、企業ユーザー (オンサイトやリモート) を認証して、ユーザーのアセットの使用を承認します。 

内部アセットを盗聴者に保護 [HTTPS]します。  **\*.Azurewebsites.net** ドメイン名では、SSL 証明書に付属して、カスタム ドメインを使用する場合できる、SSL 証明書を App Service Web Apps にします。 各 SSL 証明書には月単位の料金 (時間割り計算) が関連付けられています。 詳細については、次を参照してください。 [App Service Pricing Details]します。

[Authenticate users] 企業ディレクトリに対して。 App Service Web Apps では、Active Directory フェデレーション サービス (AD FS) などのオンプレミスの ID プロバイダーや、企業の Active Directory のデプロイで同期済みの Azure Active Directory テナントでユーザーを認証できます。 ユーザーは、オンサイトでも外出先でもシングル サインオンで Web Apps の Web プロパティにアクセスできます。 Office 365、Microsoft Intune などの既存のサービスでは、既に Azure Active Directory が使われています。 を通じて [Easy Auth], 、web アプリが非常に簡単に、同じ Azure Active Directory テナントを使用して認証をオンします。 

[Authorize users] web サイトのプロパティの使用。 最小限の追加コードで、たとえば `[Authorize]` 装飾を使用して、同じオンプレミスの ASP.NET コーディング パターンを App Service Web Apps で使用できます。 オンプレミスで管理するアプリケーションのように、細かくアクセス制御できる柔軟性はそのままになります。

## オンプレミスのリソースへの接続 ##

パフォーマンス用のクラウドやコンプライアンス用のオンプレミスに関わらず、Web アプリのデータやリソースに接続します。 Azure のデータを保持する詳細については、次を参照してください。 [Azure Trust Center]します。 

Web アプリのニーズに合わせて Azure でのさまざまなデータベース バックエンドから選択できますなど [Azure SQL Database] と [MySQL]します。 Azure でデータを安全に保存することで、地理的にデータを Web アプリの近くに置き、パフォーマンスを最適化させます。

ただし、ビジネスによってはデータをオンプレミスで保管する必要がある場合があります。 簡単に設定する app Service Web Apps により、 [hybrid connection] データベース バックエンドなどの内部設置型リソースにします。 いずれかでは、多数の web アプリを統合する内部設置型接続の管理を統合する場合は、 [Azure Virtual Network] サイト対サイト VPN を持ちます。 その後、Web アプリがオンプレミスであるかのようにオンプレミス リソースにアクセスできるようになります。

## 最適化

自動スケールで自動的にスケールし、Azure Redis Cache でキャッシュして、Web ジョブでタスクをバックグラウンドで実行し、Azure Traffic Manager で高可用性を維持することで、基幹業務アプリケーションを最適にできます。

App Service Web Apps の能力 [scale up and out] ワークロードのサイズに関係なく、基幹業務アプリケーションのニーズを満たします。 スケール アウト、web アプリを使用して手動で、 [Azure Portal], 通じてプログラムで、 [Service Management API] または [PowerShell scripting], 、自動スケール機能によって自動でします。  **標準** 階層、自動スケールにより CPU 使用率に基づいて自動的に web アプリにスケール アウトします。 ベスト プラクティスについては、次を参照してください。 [Troy Hunt]の [10 things I learned about rapidly scaling web apps with Azure]します。

応答性の高い web アプリを作成、 [Azure Redis Cache]します。 使用してデータをキャッシュするバックエンド データベースおよびその他の点からなど、 [ASP.NET session state] と [output cache]します。

[Azure Traffic Manager] を使用して、Web アプリの高可用性を維持します。 使用して、 **フェールオーバー** メソッド、Traffic Manager は、プライマリ サイトで問題がある場合にトラフィックをセカンダリ サイトに自動的にルーティングします。

## 監視と分析

Azure やサードパーティー製のツールで Web アプリのパフォーマンスを最新の状態に維持します。 重要な Web アプリのイベントでアラートを受信します。 Application Insight や HDInsight からの Web ログ分析でユーザー情報を簡単に取得します。 

取得、 [quick glance] の web アプリの現在のパフォーマンス メトリックとリソースのクォータを web アプリのブレードで、 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715)します。 アプリケーション全体の可用性、パフォーマンスと使用状況の 360 度ビューでは、使用 [Azure Application Insights] に高速かつ強力なトラブルシューティング、診断と使用状況の分析を提供します。 またはなどのサード パーティ製ツールを使用して [New Relic] web アプリのデータの監視の詳細を提供します。

 **標準** 層、アプリケーションの応答性の監視電子メール通知を受け取る、アプリが応答しなくなったときにします。 詳細については、次を参照してください。 [How to: Receive Alert Notifications and Manage Alert Rules in Azure]します。

## その他のリソース

- [App Service Web アプリのドキュメント](/services/app-service/web/)
- [Azure App Service Web アプリの学習マップ](websites-learning-map.md)
- [Azure Web ブログ](/blog/topics/web/)

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]



[Azure App Service]: /services/app-service/web/

[FTP]:web-sites-deploy.md#ftp
[Visual Studio]:web-sites-dotnet-get-started.md
[Visual Studio Team Services]:../cloud-services-continuous-delivery-use-vso.md
[Git]:web-sites-publish-source-control.md

[HTTPS]:web-sites-configure-ssl-certificate.md
[App Service Pricing Details]: /pricing/details/app-service/#ssl-connections
[Authenticate users]:web-sites-authentication-authorization.md
[Easy Auth]:/blog/2014/11/13/azure-websites-authentication-authorization/
[Authorize users]:web-sites-authentication-authorization.md

[Azure Trust Center]:/support/trust-center/
[MySQL]:web-sites-php-mysql-deploy-use-git.md
[Azure SQL Database]:web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md
[hybrid connection]:web-sites-hybrid-connection-get-started.md
[Azure Virtual Network]:web-sites-integrate-with-vnet.md

[scale up and out]:web-sites-scale.md
[Azure Portal]:http://portal.azure.com/
[Service Management API]:http://msdn.microsoft.com/library/windowsazure/ee460799.aspx
[PowerShell scripting]:http://msdn.microsoft.com/library/windowsazure/jj152841.aspx
[Troy Hunt]:https://twitter.com/troyhunt
[10 things I learned about rapidly scaling web apps with Azure]:http://www.troyhunt.com/2014/09/10-things-i-learned-about-rapidly.html
[Azure Redis Cache]:/blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/
[ASP.NET session state]:https://msdn.microsoft.com/library/azure/dn690522.aspx
[output cache]:https://msdn.microsoft.com/library/azure/dn798898.aspx

[quick glance]:web-sites-monitor.md
[Azure Application Insights]:http://blogs.msdn.com/b/visualstudioalm/archive/2015/01/07/application-insights-and-azure-websites.aspx
[New Relic]:../store-new-relic-cloud-services-dotnet-application-performance-management.md
[How to: Receive Alert Notifications and Manage Alert Rules in Azure]:http://msdn.microsoft.com/library/windowsazure/dn306638.aspx

 


