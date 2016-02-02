<properties 
    pageTitle="App Service 環境によるレイヤード セキュリティ アーキテクチャ" 
    description="App Service 環境によるレイヤード セキュリティ アーキテクチャの実装" 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/13/2015" 
    ms.author="stefsch"/>   


# App Service 環境によるレイヤード セキュリティ アーキテクチャの実装

## 概要

App Service Environment は、Virtual Network にデプロイされる分離されたランタイム環境です。開発者は、セキュリティ アーキテクチャを階層化し、物理的なアプリケーションの層ごとにネットワーク アクセスのレベルに違いを設けることができます。

一般に、API バックエンドは通常のインターネット アクセスから隠し、アップストリームの Web アプリにのみ API の呼び出しを許可することが望ましいと考えられています。 [ネットワーク セキュリティ グループ (Nsg) ][networksecuritygroups] API アプリケーションへのパブリック アクセスを制限する App Service 環境が含まれるサブネットで使用できます。

以下の図は、WebAPI ベースのアプリを App Service Environment にデプロイしたアーキテクチャの例です。 3 つの Web アプリのインスタンスが、独立した 3 つの App Service Environment に別々にデプロイされ、バックエンドにある同じ WebAPI アプリを呼び出します。

![Conceptual Architecture][conceptualarchitecture]

"apiase" を含んだサブネットに対するネットワーク セキュリティ グループは、アップストリームの Web アプリから入ってくる呼び出しと、自分自身からの呼び出しを許可します。これを示したのが緑色の正符号です。 一方、インターネットから入ってくる一般的なトラフィックについてはアクセスを明示的に拒否しています。

以降、"apiase" を含んだサブネットに対してネットワーク セキュリティ グループを構成する手順について説明します。

## ネットワークの動作の決定

必要なネットワーク セキュリティ ルールを把握するためには、API アプリを含んだ App Service Environment へのアクセスをどのネットワーク クライアントに許可し、どのクライアントをブロックするかを調べる必要があります。

[ネットワーク セキュリティ グループ (Nsg) ][networksecuritygroups] のサブネットに適用されるサブネットに App Service 環境が展開されていると、NSG に含まれている規則に適用 **すべて** App Service 環境で実行されるアプリケーション。 この記事のサンプル アーキテクチャを使用して、"apiase" を含んだサブネットに対してネットワーク セキュリティ グループを適用すると、"apiase" App Service Environment 上で動作するすべてのアプリが同じセキュリティ ルール一式で保護されます。

- **アップ ストリームの呼び出し元の送信の IP アドレスを決定:**  IP アドレスまたはアップ ストリームの呼び出し元のアドレスには何ですか? これらのアドレスは NSG で明示的にアクセスを許可する必要があります。 App Service Environment 間の呼び出しは "インターネット" を介した呼び出しと見なされます。つまり、3 つのアップストリーム App Service Environment にそれぞれ割り当てられた送信 IP アドレスを、"apiase" サブネットの NSG でアクセスを許可する必要があります。 App Service 環境で実行されるアプリを参照して、発信 IP アドレスを特定する方法についての詳細については、 [ネットワーク アーキテクチャ ][networkarchitecture] Overview (英語)。
- **バックエンド API アプリは自己呼び出しを行うか。**バックエンド アプリケーションが自己呼び出しを行うかどうかは、見逃しやすいポイントです。 App Service Environment のバックエンド API アプリケーションに自己呼び出しが伴う場合、これも "インターネット" を介した呼び出しと見なされます。 サンプル アーキテクチャのケースでは、"apiase" App Service Environment の送信 IP アドレスについてもアクセスを許可する必要があります。

## ネットワーク セキュリティ グループの設定

一連の送信 IP アドレスが確認できたら、今度はネットワーク セキュリティ グループを構築します。 App Service 環境は現在のみでサポートされている"v1"仮想ネットワーク、ため [NSG 構成 ][networksecuritygroupsclassic] は標準的な NSG は Powershell のサポートを使用して実現します。

このサンプル アーキテクチャの環境は、米国中南部に置かれているため、そのリージョンに空の NSG を作成します。

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

まず、明示的な許可上の記事で説明したように、Azure の管理インフラストラクチャの規則が追加されます [受信トラフィック ][inboundtraffic] App Service 環境のです。

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

次に、1 つ目のアップストリーム App Service Environment ("fe1ase") からの HTTP 呼び出しと HTTPS 呼び出しを許可する 2 つのルールを追加します。

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

2 つ目と 3 つ目のアップストリーム App Service Environment ("fe2ase" と "fe3ase") についても同じ作業を行います。

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

最後に、バックエンド API の App Service Environment が自己呼び出しを行えるよう、その送信 IP アドレスにアクセス権を付与します。

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

構成すべきネットワーク セキュリティ ルールは以上です。すべての NSG には既定のルール一式が備わっており、インターネットから入ってくるアクセスが既定でブロックされるため、他にルールを設定する必要はありません。

以下に示したのは、このネットワーク セキュリティ グループに含まれる全ルールの一覧です。 最後のルール (ハイライト部分) に注目してください。明示的にアクセスが許可されている呼び出し元以外はすべて、このルールによって受信アクセスがブロックされます。

![NSG Configuration][nsgconfiguration]

最後に、"apiase" App Service Environment を含んだサブネットにこの NSG を適用する必要があります。

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

このサブネットに NSG を適用すると、アップストリームにある 3 つの App Service Environment と、API バックエンドを含んだ 1 つの App Service Environment とにのみ、"apiase" 環境に対する呼び出しが許可されるようになります。


## その他のリンクおよび情報

構成 [ネットワーク セキュリティ グループ ][networksecuritygroupsclassic] 従来の仮想ネットワークにします。

理解 [発信 IP アドレス ][networkarchitecture] と App Service 環境です。

[ネットワーク ポート ][inboundtraffic] App Service 環境で使用します。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]




[networksecuritygroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/ 
[networkarchitecture]: https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/ 
[networksecuritygroupsclassic]: https://azure.microsoft.com/documentation/articles/virtual-networks-create-nsg-classic-ps/ 
[inboundtraffic]: https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 
[conceptualarchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png 
[nsgconfiguration]: ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png 

