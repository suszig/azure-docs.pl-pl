<properties 
    pageTitle="Azure App Service での基幹業務 Web アプリの作成" 
    description="このガイドでは、Azure App Service Web Apps を使用してイントラネットの基幹業務アプリケーションを作成する方法 (技術概要) について説明します。これには、認証方式、Service Bus リレー、監視も含まれます。" 
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

既存のアセットは .NET、PHP、Java、Node.js、Pythonに関係なく App Service Web Apps で実行できます。 [FTP] の使い慣れたツールや、ソース管理システムを使用した Web アプリケーションへ移動することができます。 Web アプリは、[Visual Studio] などの一般的なソース管理オプションからの直接発行をサポートしています [Visual Studio Team Services], し、[Git] - ローカル、GitHub、BitBucket、DropBox、Mercurial など..

## アセットのセキュリティ保護

暗号化によってアセットを保護し、企業ユーザー (オンサイトやリモート) を認証して、ユーザーのアセットの使用を承認します。

盗聴者を [HTTPS] を使用して、内部アセットを保護します。  **\*.Azurewebsites.net** ドメイン名では、SSL 証明書に付属して、カスタム ドメインを使用する場合できる、SSL 証明書を App Service Web Apps にします。 各 SSL 証明書には月単位の料金 (時間割り計算) が関連付けられています。 詳細については、[アプリ サービス料金の詳細] を参照してください。

[ユーザーの認証] 企業ディレクトリに対してです。 App Service Web Apps では、Active Directory フェデレーション サービス (AD FS) などのオンプレミスの ID プロバイダーや、企業の Active Directory のデプロイで同期済みの Azure Active Directory テナントでユーザーを認証できます。 ユーザーは、オンサイトでも外出先でもシングル サインオンで Web Apps の Web プロパティにアクセスできます。 Office 365、Microsoft Intune などの既存のサービスでは、既に Azure Active Directory が使われています。 [簡単認証] で web アプリの同じ Azure Active Directory テナントに認証させることは簡単です。

[ユーザーの承認] の web サイトのプロパティの使用。 最小限の追加コードでは、同じ内部設置型の ASP.NET コーディング パターンを App Service Web Apps の使用を取り込むことができます、 `[Authorize]` 例については、装飾します。 オンプレミスで管理するアプリケーションのように、細かくアクセス制御できる柔軟性はそのままになります。

## オンプレミスのリソースへの接続

パフォーマンス用のクラウドやコンプライアンス用のオンプレミスに関わらず、Web アプリのデータやリソースに接続します。 Azure のデータを保持する方法については、[Azure のトラスト センター] を参照してください。

[MySQL]、[Azure SQL Database] などの web アプリのニーズに合わせて、Azure でのさまざまなデータベース バックエンドから選択することができます。 Azure でデータを安全に保存することで、地理的にデータを Web アプリの近くに置き、パフォーマンスを最適化させます。

ただし、ビジネスによってはデータをオンプレミスで保管する必要がある場合があります。 App Service Web Apps では、データベース バックエンドなど、内部設置型リソースを簡単に、[ハイブリッド接続] を設定できます。 内部設置型接続の管理を統合する場合は、いずれかで [Azure の仮想ネットワーク] をサイト間 VPN を持つ多数の web アプリを統合します。 その後、Web アプリがオンプレミスであるかのようにオンプレミス リソースにアクセスできるようになります。

## 最適化

自動スケールで自動的にスケールし、Azure Redis Cache でキャッシュして、Web ジョブでタスクをバックグラウンドで実行し、Azure Traffic Manager で高可用性を維持することで、基幹業務アプリケーションを最適にできます。

[スケール アップやスケール アウト] する App Service Web Apps の機能では、ワークロードのサイズに関係なく、基幹業務アプリケーションのニーズを満たしています。 [サービス管理 API] からプログラムを使用してポータルから手動で、[Azure]、web アプリをスケール [PowerShell スクリプト]、または、自動スケール機能によって自動でします。 **標準**階層では、自動スケールにより、CPU 使用率に基づいて Web アプリを自動的にスケールアウトできます。 ベスト プラクティスは、[Troy Hunt] の [I learned Azure での web アプリの迅速なスケーリングに関する 10 のポイント] を参照してください。

[Azure Redis Cache] の応答性の高い web アプリを作成します。 バックエンド データベースからデータをし、[ASP.NET セッション状態] などの他のことをキャッシュし、[出力キャッシュ] には、それを使用します。

[Azure Traffic Manager] を使用して、Web アプリの高可用性を維持します。 Traffic Manager では、**フェールオーバー** を使用して、プライマリ サイトに問題がある場合に、トラフィックをセカンダリ サイトに自動的にルーティングします。

## 監視と分析

Azure やサードパーティー製のツールで Web アプリのパフォーマンスを最新の状態に維持します。 重要な Web アプリのイベントでアラートを受信します。 Application Insight や HDInsight からの Web ログ分析でユーザー情報を簡単に取得します。

Web アプリのブレードで web アプリの現在のパフォーマンスの [概要] メトリックとリソースのクォータを取得、 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715)します。アプリケーション全体の可用性、パフォーマンスと使用状況の 360 度ビューでは、高速かつ強力なトラブルシューティング、診断と使用状況の分析を提供する [Azure Application Insights] を使用します。または、詳細監視データ、web アプリを提供する [New Relic] などのサード パーティ製ツールを使用します。

**標準**階層では、アプリの応答性を監視し、アプリが応答しなくなったときに電子メール通知を受信します。 詳細については、次を参照してください。 [方法: アラートの通知を受信し、Azure でアラート ルールの管理]。

## その他のリソース

- [App Service Web Apps のドキュメント](/services/app-service/web/)
- [Azure App Service Web Apps の学習マップ](websites-learning-map.md)
- [Azure Web ブログ](/blog/topics/web/)

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]




[azure app service]: /services/app-service/web/ 
[ftp]: web-sites-deploy.md#ftp 
[visual studio]: web-sites-dotnet-get-started.md 
[visual studio team services]: ../cloud-services-continuous-delivery-use-vso.md 
[git]: web-sites-publish-source-control.md 
[https]: web-sites-configure-ssl-certificate.md 
[app service pricing details]: /pricing/details/app-service/#ssl-connections 
[authenticate users]: web-sites-authentication-authorization.md 
[easy auth]: /blog/2014/11/13/azure-websites-authentication-authorization/ 
[authorize users]: web-sites-authentication-authorization.md 
[azure trust center]: /support/trust-center/ 
[mysql]: web-sites-php-mysql-deploy-use-git.md 
[azure sql database]: web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md 
[hybrid connection]: web-sites-hybrid-connection-get-started.md 
[azure virtual network]: web-sites-integrate-with-vnet.md 
[scale up and out]: web-sites-scale.md 
[azure portal]: http://portal.azure.com/ 
[service management api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx 
[powershell scripting]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx 
[troy hunt]: https://twitter.com/troyhunt 
[10 things i learned about rapidly scaling web apps with azure]: http://www.troyhunt.com/2014/09/10-things-i-learned-about-rapidly.html 
[azure redis cache]: /blog/2014/06/05/mvc-movie-app-with-azure-redis-cache-in-15-minutes/ 
[asp.net session state]: https://msdn.microsoft.com/library/azure/dn690522.aspx 
[output cache]: https://msdn.microsoft.com/library/azure/dn798898.aspx 
[quick glance]: web-sites-monitor.md 
[azure application insights]: http://blogs.msdn.com/b/visualstudioalm/archive/2015/01/07/application-insights-and-azure-websites.aspx 
[new relic]: ../store-new-relic-cloud-services-dotnet-application-performance-management.md 
[how to: receive alert notifications and manage alert rules in azure]: http://msdn.microsoft.com/library/windowsazure/dn306638.aspx 

