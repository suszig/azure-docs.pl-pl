<properties 
    pageTitle="App Service 環境の概要" 
    description="すべてのアプリを実行するためのセキュリティで保護され VNet に参加している専用のスケール ユニットを提供する、App Service 環境の機能について説明します。" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/06/2015"
    ms.author="stefsch"/>

# App Service 環境の概要

## 概要 ##
App Service 環境は、 [Premium][PremiumTier] サービス高スケールは、Azure App Service アプリを安全に実行するための完全に分離され、専用の環境を提供する Azure App Service のプランのオプションを含む [Web Apps][WebApps], 、[Mobile Apps][MobileApps], 、および [API Apps][APIApps]します。  

App Service 環境は、以下を必要とするアプリケーション ワークロードに最適です。

- 高スケール
- 分離およびセキュリティで保護されたネットワーク アクセス

顧客は、複数の App Service 環境を 1 つの Azure リージョン内に作成することも、複数の Azure リージョンにわたって作成することもできます。  そのため、App Service 環境は、高 RPS のワークロードをサポートするステートレス アプリケーション層の水平方向のスケーリングに最適です。

App Service 環境は、単一の顧客のアプリケーションだけを実行するために分離され、常に仮想ネットワークにデプロイされます。  顧客は、受信および送信アプリケーション ネットワーク トラフィックをきめ細かく制御できます。また、アプリケーションは、オンプレミスの企業リソースへの仮想ネットワーク経由のセキュリティで保護された高速接続を確立できます。

App Service 環境の高い拡張性を有効にしてセキュリティ保護の概要については、ネットワーク アクセスを参照してください、 [AzureCon Deep Dive][AzureConDeepDive] App Service 環境に!

複数の App Service 環境を使用して水平スケーリングの詳細をセットアップする方法の記事を参照して、 [地理的に分散アプリケーションのフット プリント][GeodistributedAppFootprint]します。

AzureCon 詳細に示すようにセキュリティ アーキテクチャの構成方法を表示するには、実装に関する記事を参照して、 [セキュリティ アーキテクチャを階層化](app-service-app-service-environment-layered-security.md) App Service 環境の使用。

App Service 環境で実行されるアプリへのアクセスは、Web アプリケーション ファイアウォール (WAF) などのアップ ストリーム デバイスによって制限できます。  記事「 [App Service 環境の、WAF の構成](app-service-app-service-environment-web-application-firewall.md) このシナリオについて説明します。 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## 専用のコンピューティング リソース ##
App Service 環境内のすべてのコンピューティング リソースは、1 つのサブスクリプション専用です。App Service 環境は、1 つのアプリケーションだけが使用する最大 50 個のコンピューティング リソースで構成できます。

App Service 環境は、1 つのフロントエンド コンピューティング リソース プールと、1 ～ 3 個の worker コンピューティング リソース プールで構成されます。 

フロントエンド プールには、SSL 終了と、App Service 環境内でのアプリ要求の自動負荷分散を担当するコンピューティング リソースが含まれます。 

各ワーカー プールに割り当てられたコンピューティング リソースが含まれています [App Service プラン][AppServicePlan], 、さらに 1 つまたは複数の Azure App Service アプリを含めることができます。  App Service 環境には、異なるワーカー プールを 3 つまで配置できるため、ワーカー プールごとに異なるコンピューティング リソースを柔軟に選択できます。  

たとえば、これにより、開発アプリまたはテスト アプリ向けの App Service プランでは、性能の低いコンピューティング リソースを含むワーカー プールを 1 つ作成できます。  2 番目 (または 3 番目) のワーカー プールでは、実稼働アプリを実行する App Service プラン向けにより性能の高いコンピューティング リソースを使用することができます。

フロント エンドとワーカー プールに使用できるコンピューティング リソースの量についての詳細については、次を参照してください。 [App Service 環境を構成する方法][HowToConfigureanAppServiceEnvironment]します。  

App Service 環境でサポートされている使用可能なコンピューティング リソース サイズの詳細を参照してください、 [App Service の料金][AppServicePricing] ページを Premium 価格レベルの App Service 環境の使用可能なオプションを確認します。

## Virtual Network のサポート ##
App Service 環境は、既存の地域のクラシック"v1"仮想ネットワーク、または新しい地域クラシック"v1"仮想ネットワークでは作成か、([仮想ネットワークの詳細][MoreInfoOnVirtualNetworks])。  App Service 環境は常に地域仮想ネットワーク (もっと正確に言えば、地域仮想ネットワークのサブネット内) に存在するため、仮想ネットワークのセキュリティ機能を使用して、受信と送信の両方のネットワーク通信を制御できます。  

使用する [ネットワーク セキュリティ グループ][NetworkSecurityGroups] を App Service 環境が存在するサブネットの受信ネットワーク通信を制限します。  これによって、Web アプリケーション ファイアウォールやネットワーク SaaS プロバイダーなど、アップストリーム デバイスおよびサービスの背後でアプリを実行できます。  

また、アプリは、内部データベースや Web サービスなどの企業リソースに頻繁にアクセスする必要があります。  一般的な方法では、これらのエンドポイントを、Azure 仮想ネットワーク内を通過する内部ネットワーク トラフィックのみが使用できるようにします。  App Service 環境を内部のサービスと同じ仮想ネットワークに参加すると、環境内で実行されるアプリは、アクセス可能で到達可能なエンドポイントを含む [サイト対サイト][SiteToSite] と [Azure ExpressRoute][ExpressRoute] 接続します。

仮想ネットワークと内部設置型ネットワークの App Service 環境のしくみの詳細については、上、次の記事を参照してください [ネットワーク アーキテクチャ][NetworkArchitectureOverview], 、[受信トラフィックの制御][ControllingInboundTraffic], 、および [バックエンドを安全に接続する][SecurelyConnectingToBackends]します。 

**注:**  "v2"仮想ネットワークの App Service 環境を作成することはできません。

## 使用の開始

App Service 環境で開始するを参照してください [An App Service 環境の作成方法][HowToCreateAnAppServiceEnvironment。]

Azure App Service プラットフォームの詳細については、次を参照してください。 [Azure App Service][AzureAppService]します。

App Service 環境のネットワーク アーキテクチャの概要については、次を参照してください。、 [ネットワーク アーキテクチャの概要][NetworkArchitectureOverview] 記事です。

Expressroute では、App Service 環境の使用の詳細については、次の資料を参照してください [Express Route と App Service 環境][NetworkConfigDetailsForExpressRoute]します。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment。]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->

 

<!--HONumber=Apr16_HO2-->
