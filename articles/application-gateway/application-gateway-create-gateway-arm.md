<properties 
   pageTitle="Azure リソース マネージャーを使用した Application Gateway の作成、起動、または削除 | Microsoft Azure"
   description="このページでは、Azure リソース マネージャーを使用して、Azure Application Gateway を作成、構成、起動、および削除する方法について説明します。"
   documentationCenter="na"
   services="application-gateway"
   authors="joaoma"
   manager="jdial"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="11/24/2015"
   ms.author="joaoma"/>



# Azure リソース マネージャーを使用した、Application Gateway の作成、起動、または削除

Application Gateway はロード バランサーの第 7 層です。 クラウドでもオンプレミスでも、異なるサーバー間のフェールオーバーと HTTP 要求のパフォーマンス ルーティングを提供します。 Application Gateway は、HTTP 負荷分散、クッキー ベースのセッション アフィニティ、SSL オフロードなどのアプリケーション配信機能を備えています。


> [AZURE.SELECTOR]
- [Azure Classic PowerShell steps](application-gateway-create-gateway.md)
- [Azure Resource Manager PowerShell](application-gateway-create-gateway-arm.md)
- [Azure Resource Manager template ](application-gateway-create-gateway-arm-template.md)



<BR>


この記事では、アプリケーション ゲートウェイを作成、構成、起動、および削除する手順について説明します。

>[AZURE.IMPORTANT] Azure リソースを使用する前に、Azure は現在、リソース マネージャーのデプロイ モデルと従来のデプロイ モデルの 2 種類を備えていることを理解しておくことが重要です。 理解しておいてください [とツールの配置モデル](azure-classic-rm.md) あらゆる Azure リソースを使用する前にします。 この記事の上部にあるタブをクリックすると、さまざまなツールについてのドキュメントを参照できます。このドキュメントでは、Azure リソース マネージャーを使用した Application Gateway の作成について説明します。 従来のバージョンを使用するには [PowerShell を使用してアプリケーション ゲートウェイ クラシック展開の作成](application-gateway-create-gateway.md)します。



## 開始する前に

1. Web Platform Installer を使用して、Azure PowerShell コマンドレットの最新バージョンをインストールします。 ダウンロードしてからの最新バージョンをインストール、 **Windows PowerShell** のセクションで、 [ダウンロード ページ](http://azure.microsoft.com/downloads/)します。
2. Application Gateway の仮想ネットワークとサブネットを作成します。 仮想マシンまたはクラウド デプロイでサブネットをしていないことを確認します。 Application Gateway そのものが、仮想ネットワーク サブネットに含まれている必要があります。
3. Application Gateway を使用するように構成するサーバーが存在している必要があります。つまり、仮想ネットワーク内、または割り当てられたパブリック IP/VIP を使用してエンドポイントが作成されている必要があります。

## Application Gateway の作成に必要な構成

- **バックエンド サーバー プール:** バックエンド サーバーの IP アドレスの一覧。 一覧の IP アドレスは、仮想ネットワークのサブネットに属しているか、パブリック IP/VIP である必要があります。
- **バックエンド サーバー プールの設定:** すべてのプールには、ポート、プロトコル、cookie ベースのアフィニティなどの設定があります。 これらの設定はプールに関連付けられ、プール内のすべてのサーバーに適用されます。
- **フロントエンド ポート:** このポートは、アプリケーション ゲートウェイで開かれたパブリック ポートです。 このポートにトラフィックがヒットすると、バックエンド サーバーのいずれかにリダイレクトされます。
- **リスナー:** リスナーには、フロントエンド ポート、プロトコル (Http または Https、大文字小文字の区別あり)、および SSL 証明書名 (オフロードの SSL を構成する場合) があります。
- **ルール:** ルールはリスナーとバックエンド サーバー プールを結び付け、トラフィックが特定のリスナーにヒットした際に送られるバックエンド サーバー プールを定義します。 現在、*basic* ルールのみサポートされます。 *basic* ルールは、ラウンド ロビンの負荷分散です。



## 新しい Application Gateway の作成

Azure クラシックと Azure リソース マネージャーの使用方法の違いは、設定が必要な Application Gateway と項目を作成する順番にあります。

リソース マネージャーを使用すると、Application Gateway を作成するすべての項目は個別に構成され、その後結合されて Application Gateway のリソースを作成します。


ここでは、Application Gateway を作成するために必要な手順を次に示します。

1. リソース マネージャーのリソース グループの作成
2. Application Gateway の仮想ネットワーク、サブネット、およびパブリック IP の作成
3. Application Gateway 構成オブジェクトの作成
4. Application Gateway のリソースを作成します。


## リソース マネージャーのリソース グループの作成

ARM コマンドレットを使用するように PowerShell モードを切り替えてください。 詳細については、使用 [リソース マネージャーでの Windows Powershell](powershell-azure-resource-manager.md)します。

### 手順 1.

        PS C:\> Login-AzureRmAccount

### 手順 2.

アカウントのサブスクリプションを確認する

        PS C:\> get-AzureRmSubscription 

適宜認証資格情報を使用します。<BR>

### 手順 3.

使用する Azure サブスクリプションを選択します。 <BR>


        PS C:\> Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### 手順 4.

新しいリソース グループを作成します (既存のリソース グループを使用する場合は、この手順をスキップしてください)。

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure リソース マネージャーでは、すべてのリソース グループの場所を指定する必要があります。 指定した場所は、そのリソース グループ内のリソースの既定の場所として使用されます。 Application Gateway を作成するためのすべてのコマンドで、同じリソース グループが使用されていることを確認します。

上記の例では、"appgw-RG" という名前のリソース グループと "West US" という名前の場所を作成しました。

## Application Gateway の仮想ネットワークとサブネットの作成

次の例では、リソース マネージャーを使用して仮想ネットワークを作成する方法を示します。

### 手順 1.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

アドレス範囲 10.0.0.0/24 を仮想ネットワークの作成に使用するサブネットの変数に割り当てます。

### 手順 2.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

サブネット 10.0.0.0/24 とプレフィックス 10.0.0.0/16 を使用して、West US 地域のリソース グループ "appw-rg" に、"appgwvnet" という名前の仮想ネットワークを作成します。

### 手順 3.

    $subnet=$vnet.Subnets[0]

## フロントエンド構成のパブリック IP アドレスの作成

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

West US 地域のリソース グループ "appw-rg" に、パブリック IP リソース "publicIP01" を作成します。


## Application Gateway 構成オブジェクトの作成

### 手順 1.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

"gatewayIP01" という名前の Application Gateway の IP 構成を作成します。 Application Gateway が起動すると、構成されているサブネットから IP アドレスが取得されて、ネットワーク トラフィックがバックエンド IP プール内の IP アドレスにルーティングされます。 各インスタンスは、1 つの IP アドレスを取得することに注意してください。

### 手順 2.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

この手順は、IP アドレス "134.170.185.46、134.170.188.221、134.170.185.50" を使用して、"pool01" という名前のバックエンド IP アドレス プールを構成します。 これらは、フロントエンド IP エンドポイントから送信されるネットワーク トラフィックを受信する IP アドレスとなります。 独自のアプリケーションの IP アドレス エンドポイントを追加するために、上記の IP アドレスを置き換えます。

### 手順 3.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

バックエンド プール内の負荷を分散したネットワーク トラフィックに対して、Application Gateway の設定 "poolsetting01" を構成します。

### 手順 4.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

この例では、パブリック IP エンドポイントに対して、"frontendport01" という名前のフロントエンド IP ポートを構成します。

### 手順 5.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

"fipconfig01" という名前のフロントエンド IP 構成を作成し、このフロントエンド IP 構成にパブリック IP アドレスを関連付けます。

### 手順 6

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

"listener01" という名前のリスナーを作成し、フロントエンド IP 構成にフロントエンド ポートを関連付けます。

### 手順 7.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

"rule01" という名前のロード バランサーのルーティング規則を作成し、ロード バランサーの動作を構成します。

### 手順 8.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

Application Gateway のインスタンスのサイズを構成します。
>[AZURE.NOTE]  *InstanceCount* の既定値は 2、最大値は 10 です。 *GatewaySize* の既定値は Medium です。 Standard_Small、Standard_Medium、Standard_Large のいずれかを選択できます。

## New-AzureApplicationGateway を使用した Application Gateway の作成

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

上記の手順の構成項目をすべて使用して、Application Gateway を作成します。 この例では、Application Gateway は "appgwtest" という名前です。


## Application Gateway の起動

ゲートウェイが構成されるを使用して、 `開始 AzureRmApplicationGateway` コマンドレット ゲートウェイを起動します。 アプリケーション ゲートウェイの課金は、ゲートウェイが正常に起動された後に開始します。


**注:** 、 `開始 AzureRmApplicationGateway` コマンドレットは最大 15 ~ 20 分かかる場合があります。

次の例では、Application Gateway の名前は "appgwtest" で、リソース グループの名前は "app-rg" です。


### 手順 1.

Application Gateway オブジェクトを取得し、変数 "$getgw" に関連付けます。

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName app-rg

### 手順 2.

使用 `開始 AzureRmApplicationGateway` アプリケーション ゲートウェイを起動します。

     Start-AzureRmApplicationGateway -ApplicationGateway $getgw  

## Application Gateway の状態の確認

使用して、 `Get AzureRmApplicationGateway` コマンドレット ゲートウェイの状態を確認します。 前の手順で *Start-AzureApplicationGateway* が成功した場合、State は *Running* になり、Vip と DnsName に有効な値が入力されます。

このサンプルで確認し、実行は、アプリケーション ゲートウェイに宛のトラフィックを受け入れる準備 `http://<generated-dns-name>.cloudapp.net`します。

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg
    
    Sku                               : Microsoft.Azure.Commands.Network.Models.PSApplicationGatewaySku
    GatewayIPConfigurations           : {gatewayip01}
    SslCertificates                   : {}
    FrontendIPConfigurations          : {frontendip01}
    FrontendPorts                     : {frontendport01}
    BackendAddressPools               : {pool01}
    BackendHttpSettingsCollection     : {setting01}
    HttpListeners                     : {listener01}
    RequestRoutingRules               : {rule01}
    OperationalState                  : 
    ProvisioningState                 : Succeeded
    GatewayIpConfigurationsText       : [
                                      {
                                        "Subnet": {
                                          "Id": "/subscriptions/###############################/resourceGroups/appgw-rg
                                    /providers/Microsoft.Network/virtualNetworks/vnet01/subnets/subnet01"
                                        },
                                        "ProvisioningState": "Succeeded",
                                        "Name": "gatewayip01",
                                        "Etag": "W/\"ddb0408e-a54c-4501-a7f8-8487c3530bd7\"",
                                        "Id": "/subscriptions/###############################/resourceGroups/appgw-rg/p
                                    roviders/Microsoft.Network/applicationGateways/appgwtest/gatewayIPConfigurations/gatewayip
                                    01"
                                      }
                                    ]
    SslCertificatesText               : []
    FrontendIpConfigurationsText      : [
                                      {
                                        "PrivateIPAddress": null,
                                        "PrivateIPAllocationMethod": "Dynamic",
                                        "Subnet": null,
                                        "PublicIPAddress": {
                                          "Id": "/subscriptions/###############################/resourceGroups/appgw-rg
                                    /providers/Microsoft.Network/publicIPAddresses/publicip01"
                                        },
                                        "ProvisioningState": "Succeeded",
                                        "Name": "frontendip01",
                                        "Etag": "W/\"ddb0408e-a54c-4501-a7f8-8487c3530bd7\"",
                                        "Id": "/subscriptions/###############################/resourceGroups/appgw-rg/p
                                    roviders/Microsoft.Network/applicationGateways/appgwtest/frontendIPConfigurations/frontend
                                    ip01"
                                      }
                                    ]
    FrontendPortsText                 : [
                                      {
                                        "Port": 80,
                                        "ProvisioningState": "Succeeded",
                                        "Name": "frontendport01",
                                        "Etag": "W/\"ddb0408e-a54c-4501-a7f8-8487c3530bd7\"",
                                        "Id": "/subscriptions/###############################/resourceGroups/appgw-rg/p
                                    roviders/Microsoft.Network/applicationGateways/appgwtest/frontendPorts/frontendport01"
                                      }
                                    ]
    BackendAddressPoolsText           : [
                                      {
                                        "BackendAddresses": [
                                          {
                                            "Fqdn": null,
                                            "IpAddress": "134.170.185.46"
                                          },
                                          {
                                            "Fqdn": null,
                                            "IpAddress": "134.170.188.221"
                                          },
                                          {
                                            "Fqdn": null,
                                            "IpAddress": "134.170.185.50"
                                          }
                                        ],
                                        "BackendIpConfigurations": [],
                                        "ProvisioningState": "Succeeded",
                                        "Name": "pool01",
                                        "Etag": "W/\"ddb0408e-a54c-4501-a7f8-8487c3530bd7\"",
                                        "Id": "/subscriptions/###############################/resourceGroups/appgw-rg/p
                                    roviders/Microsoft.Network/applicationGateways/appgwtest/backendAddressPools/pool01"
                                      }
                                    ]
    BackendHttpSettingsCollectionText : [
                                      {
                                        "Port": 80,
                                        "Protocol": "Http",
                                        "CookieBasedAffinity": "Disabled",
                                        "ProvisioningState": "Succeeded",
                                        "Name": "setting01",
                                        "Etag": "W/\"ddb0408e-a54c-4501-a7f8-8487c3530bd7\"",
                                        "Id": "/subscriptions/###############################/resourceGroups/appgw-rg/p
                                    roviders/Microsoft.Network/applicationGateways/appgwtest/backendHttpSettingsCollection/set
                                    ting01"
                                      }
                                    ]
    HttpListenersText                 : [
                                      {
                                        "FrontendIpConfiguration": {
                                          "Id": "/subscriptions/###############################/resourceGroups/appgw-rg
                                    /providers/Microsoft.Network/applicationGateways/appgwtest/frontendIPConfigurations/fronte
                                    ndip01"
                                        },
                                        "FrontendPort": {
                                          "Id": "/subscriptions/###############################/resourceGroups/appgw-rg
                                    /providers/Microsoft.Network/applicationGateways/appgwtest/frontendPorts/frontendport01"
                                        },
                                        "Protocol": "Http",
                                        "SslCertificate": null,
                                        "ProvisioningState": "Succeeded",
                                        "Name": "listener01",
                                        "Etag": "W/\"ddb0408e-a54c-4501-a7f8-8487c3530bd7\"",
                                        "Id": "/subscriptions/###############################/resourceGroups/appgw-rg/p
                                    roviders/Microsoft.Network/applicationGateways/appgwtest/httpListeners/listener01"
                                      }
                                    ]
    RequestRoutingRulesText           : [
                                      {
                                        "RuleType": "Basic",
                                        "BackendAddressPool": {
                                          "Id": "/subscriptions/###############################/resourceGroups/appgw-rg
                                    /providers/Microsoft.Network/applicationGateways/appgwtest/backendAddressPools/pool01"
                                        },
                                        "BackendHttpSettings": {
                                          "Id": "/subscriptions/###############################/resourceGroups/appgw-rg
                                    /providers/Microsoft.Network/applicationGateways/appgwtest/backendHttpSettingsCollection/s
                                    etting01"
                                        },
                                        "HttpListener": {
                                          "Id": "/subscriptions/###############################/resourceGroups/appgw-rg
                                    /providers/Microsoft.Network/applicationGateways/appgwtest/httpListeners/listener01"
                                        },
                                        "ProvisioningState": "Succeeded",
                                        "Name": "rule01",
                                        "Etag": "W/\"ddb0408e-a54c-4501-a7f8-8487c3530bd7\"",
                                        "Id": "/subscriptions/###############################/resourceGroups/appgw-rg/p
                                    roviders/Microsoft.Network/applicationGateways/appgwtest/requestRoutingRules/rule01"
                                      }
                                    ]
    ResourceGroupName                 : appgw-rg
    Location                          : westus
        Tag                               : {}
    TagsTable                         : 
    Name                              : appgwtest
    Etag                              : W/"ddb0408e-a54c-4501-a7f8-8487c3530bd7"
    Id                                : /subscriptions/###############################/resourceGroups/appgw-rg/providers/Microsoft.Network/applicationGateways/appgwtest

## Application Gateway の削除

Application Gateway を削除するには、次の手順を順番に実行する必要があります。

1. ゲートウェイを停止するには、`Stop-AzureRmApplicationGateway` コマンドレットを使用します。
2. ゲートウェイを削除するには、`Remove-AzureRmApplicationGateway` コマンドレットを使用します。
3. 使用して、ゲートウェイが削除されたことを確認、 `Get AzureRmApplicationGateway` コマンドレットです。


### 手順 1.

Application Gateway オブジェクトを取得し、変数 "$getgw" に関連付けます。

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### 手順 2.

使用 `Stop AzureRmApplicationGateway` アプリケーション ゲートウェイを停止します。

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

アプリケーション ゲートウェイが Stopped 状態に入るを使用して、 `削除 AzureRmApplicationGateway` コマンドレットをサービスを削除します。


    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force

>[AZURE.NOTE] "-force" スイッチを使用すると、削除の確認メッセージを表示しないように設定できます。
>

サービスが削除されたことを確認するには、`Get-AzureRmApplicationGateway` コマンドレットを使用します。 この手順は必須ではありません。


    Get-AzureRmApplicationGateway -Name appgwtest-ResourceGroupName appgw-rg

## 次のステップ

SSL オフロードを構成する場合は、「 [SSL 用のアプリケーション ゲートウェイの構成の負荷を軽減](application-gateway-ssl.md)します。

ILB とともに使用するアプリケーション ゲートウェイを構成するを参照して [内部ロード バランサー (ILB) アプリケーション ゲートウェイの作成](application-gateway-ilb.md)します。

負荷分散のオプション全般の詳細については、次を参照してください。

- [Azure ロード バランサー](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)






