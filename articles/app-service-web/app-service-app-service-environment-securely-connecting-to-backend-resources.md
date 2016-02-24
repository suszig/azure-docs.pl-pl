<properties 
    pageTitle="App Service 環境からバックエンド リソースへの安全な接続" 
    description="App Service 環境からバックエンド リソースに安全に接続する方法について説明します。" 
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
    ms.date="12/08/2015" 
    ms.author="stefsch"/>   

# App Service 環境からバックエンド リソースへの安全な接続 #

## 概要 ##
App Service 環境は常に地域クラシック"v1"のサブネットの [仮想ネットワーク] で作成されるため [仮想ネットワーク]、App Service 環境から他のバックエンド リソースへの発信接続は、仮想ネットワーク経由でのみフローできます。  

**注:**  "v2"ARM で管理された仮想ネットワークの App Service 環境を作成することはできません。

たとえば、ポート 1433 がロックされている仮想マシンのクラスターで実行されている SQL Server がある場合があります。  このエンドポイントは、同じ仮想ネットワークの他のリソースからのアクセスを許可する目的のみで使用されることがあります。  

別の例として、機密性の高いエンドポイントが内部設置型を実行する場合があり、どちらか [サイト対サイト] [SiteToSite] を使用して Azure に接続されているか、[Azure ExpressRoute] [ExpressRoute] 接続します。  その結果、サイト対サイトまたは ExpressRoute トンネルに接続されている仮想ネットワーク内のリソースのみがオンプレミスのエンドポイントにアクセスできるようになります。

これらのすべてのシナリオで、App Service 環境で実行中のアプリが、さまざまなサーバーとリソースに安全に接続できます。  App Service 環境で実行されているアプリから同じ仮想ネットワーク内の (または同じ仮想ネットワークに接続されている) プライベート エンドポイントへの送信トラフィックは、仮想ネットワーク経由でのみ行われます。  プライベート エンドポイントへの送信トラフィックがパブリック インターネット経由で送信されることはありません。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## 発信接続と DNS の要件 ##
App Service 環境を適切に機能させるには、Azure Storage ワールドワイドへの発信アクセスだけでなく、同じ Azure リージョン内の SQL Database への接続も必要であることに注意してください。  仮想ネットワーク内で発信インターネット アクセスがブロックされている場合、App Service 環境はこれらの Azure エンドポイントにアクセスすることはできません。

顧客の仮想ネットワーク内にカスタム DNS サーバーが構成されていることもあります。  App Service 環境は、*.database.windows.net、*.file.core.windows.net、および *.blob.core.windows.net の Azure エンドポイントを解決できる必要があります。  

また、App Service 環境を作成する前に、仮想ネットワーク上のカスタム DNS サーバーをセットアップしておくことをお勧めします。  App Service 環境の作成中に仮想ネットワークの DNS 構成が変更された場合、App Service 環境の作成プロセスは失敗します。  VPN ゲートウェイの他端にカスタム DNS サーバーが存在していて、その DNS サーバーが到達不能または使用できない場合、App Service 環境の作成プロセスも失敗します。 

## SQL Server への接続
SQL Server の構成には一般的に、ポート 1433 でリッスンしているエンドポイントがあります。

![SQL Server エンドポイント][SqlServerEndpoint]

このエンドポイントへのトラフィックを制限する方法は 2 つあります。


- [ネットワーク アクセス制御リスト][NetworkAccessControlLists](ネットワーク Acl)

- [ネットワーク セキュリティ グループ][NetworkSecurityGroups]


## ネットワーク ACL でのアクセスの制限

ポート 1433 は、ネットワーク アクセス制御リストを使用して保護できます。  次の例は、仮想ネットワーク内から送信されたクライアント アドレスをホワイトリストに登録し、その他のすべてのクライアントへのアクセスをブロックします。

![ネットワーク アクセス制御リストの例][NetworkAccessControlListExample]

SQL Server が使用して、SQL Server インスタンスに接続できるように、同じ仮想ネットワークの App Service 環境で実行されているすべてのアプリケーション、 **VNet 内部** SQL Server 仮想マシンの IP アドレスです。  

次の例の接続文字列は、プライベート IP アドレスを使用して SQL Server を参照します。

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

仮想マシンにはパブリック エンドポイントもありますが、ネットワーク ACL によって、パブリック IP アドレスを使用した接続の試行は拒否されます。 

## ネットワーク セキュリティ グループを使用したアクセスの制限
アクセスを保護する方法にはほかに、ネットワーク セキュリティ グループを使用する方法があります。  ネットワーク セキュリティ グループは、個々の仮想マシン、または仮想マシンを含むサブネットに適用できます。

まず、ネットワーク セキュリティ グループを作成する必要があります。

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

VNet 間の内部トラフィックのみにアクセスを制限すると、ネットワーク セキュリティ グループをごく単純に使用できます。  ネットワーク セキュリティ グループの既定の規則では、同じ仮想ネットワーク内の他のネットワーク クライアントからのアクセスのみが許可されます。

その結果、SQL Server へのアクセスのロックは、SQL Server を実行している仮想マシンまたはその仮想マシンを含むサブネットに対する既定の規則を使用してネットワーク セキュリティ グループを適用するくらい単純になります。

次の例では、ネットワーク セキュリティ グループを、このグループを含むサブネットに適用します。

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
    
最終的に、VNet 内部のアクセスを許可し、外部アクセスをブロックするセキュリティの規則の組み合わせになります。

![既定のネットワーク セキュリティの規則][DefaultNetworkSecurityRules]


## 使用の開始

App Service 環境で開始するには、[App Service 環境の概要] を参照してください [IntroToAppServiceEnvironment]

App Service 環境への着信トラフィックの制御に関する詳細については、[App Service 環境へのトラフィックの受信を制御する] を参照してください [ControlInboundASE]。

Azure App Service プラットフォームの詳細については、[Azure App Service] [AzureAppService] を参照してください。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 

