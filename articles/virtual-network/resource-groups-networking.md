<properties 
   pageTitle="ネットワーク リソース プロバイダーの概要 | Microsoft Azure"
   description="Azure リソース マネージャーのネットワーク リソース プロバイダーについて説明します。"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="12/11/2015"
   ms.author="telmos" />

# ネットワーク リソース プロバイダー
今日のビジネスの成功を支えるニーズとは、大規模なネットワーク対応アプリケーションを俊敏、柔軟、安全、かつ反復可能な方法でビルドおよび管理する能力です。 Azure リソース マネージャー (ARM) では、リソース グループ内の 1 つのリソース コレクションとして、このようなアプリケーションを作成することができます。 このようなリソースは、ARM の下でさまざまなリソース プロバイダーを介して管理されています。 

Azure リソース マネージャーは、さまざまなリソース プロバイダーに依存して、リソースへのアクセスを提供しています。 3 つの主なリソース プロバイダーは、Network、Storage および Compute です。 このドキュメントは、次などのネットワーク リソース プロバイダーの特徴と利点について説明します。

- **メタデータ** – タグを使用してリソースに情報を追加することができます。 これらのタグは、リソース グループとサブスクリプションのリソース使用率を追跡するために使用されます。
- **ネットワークの制御の拡大** - ネットワーク リソースは疎結合されより細かく制御することができます。 つまり、ネットワーク リソースを管理する柔軟性が高まります。
- **高速構成** -ネットワーク リソースは疎結合されているために作成および並列のネットワーク リソースを調整します。 これにより、構成にかかる時間が大幅に減少しました。
- **ロールベースのアクセス制御** -RBAC がセキュリティで保護された管理用のカスタム ロールの作成を許可するだけでなく、特定のセキュリティ スコープを持つ、既定のロールを提供します。 
- **管理と展開を簡略化** -展開およびリソース グループ内のリソースの 1 つのコレクションとしてアプリケーション全体のスタックを作成できるため、アプリケーションを管理する方が簡単です。 テンプレートの JSON ペイロードを用意するだけでデプロイできるため、デプロイにかかる時間が短縮されます。
- **迅速なカスタマイズ** の宣言型形式のテンプレートを使用すると、デプロイメントを反復可能かつ迅速なカスタマイズを有効にすることができます。 
- **反復可能なカスタマイズ** の宣言型形式のテンプレートを使用すると、デプロイメントを反復可能かつ迅速なカスタマイズを有効にすることができます。
- **管理インターフェイス** -次のインターフェイスのいずれかを使用して、リソースを管理することができます。
    - REST ベースの API
    - PowerShell
    - .NET SDK
    - Node.JS SDK
    - Java SDK
    - Azure CLI
    - プレビュー ポータル
    - ARM テンプレートの言語

## ネットワーク リソース 
ネットワーク リソースを、1 つのコンピューティング リソース (仮想マシン) を介して管理するのではなく、独立して管理できるようになりました。 これにより、リソース グループに複雑かつ大規模なインフラストラクチャを構成する際により高度な柔軟性と俊敏性が保証されます。

複数の層のアプリケーションに関連するサンプルのデプロイの概念図を次に示します。 NIC、パブリック IP アドレス、VM などの各リソースは個別に管理できます。 

![ネットワーク リソース モデル](./media/resource-groups-networking/Figure2.png)

すべてのリソースには、共通のプロパティのセットとその個々のプロパティがあります。 共通のプロパティは次のとおりです。

|プロパティ|説明|値の例|
|---|---|---|
|**name**|リソースの一意の名前。 リソースの種類ごとに、独自の名前付け制限がある。|PIP01、VM01、NIC01|
|**location**|リソースが存在する Azure リージョン|westus、eastus|
|**id**|URI に基づいた一意の ID|/subscriptions/<subGUID>/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP|

リソースの個々のプロパティは以下のセクションで確認できます。

[AZURE.INCLUDE [virtual-networks-nrp-pip-include](../../includes/virtual-networks-nrp-pip-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nic-include](../../includes/virtual-networks-nrp-nic-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-nsg-include](../../includes/virtual-networks-nrp-nsg-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-udr-include](../../includes/virtual-networks-nrp-udr-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vnet-include](../../includes/virtual-networks-nrp-vnet-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-dns-include](../../includes/virtual-networks-nrp-dns-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-lb-include](../../includes/virtual-networks-nrp-lb-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-appgw-include](../../includes/virtual-networks-nrp-appgw-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-vpn-include](../../includes/virtual-networks-nrp-vpn-include.md)]

[AZURE.INCLUDE [virtual-networks-nrp-tm-include](../../includes/virtual-networks-nrp-tm-include.md)]

## 管理インターフェイス
Azure のネットワーク リソースは、さまざまなインターフェイスを使用して管理できます。 このドキュメントでは、これらのインターフェイスのうち REST API およびテンプレートに焦点を当てます。

### REST API 
前述のように、ネットワーク リソースは、さまざまなインターフェイス (REST API、NET SDK、Node.JS SDK、Java SDK、PowerShell、CLI、Azure ポータルおよびテンプレートなど) を経由して管理できます。 

REST API は、HTTP 1.1 プロトコル仕様に準拠しています。 API の一般的な URI の構造は次のとおりです。 

    https://management.azure.com/subscriptions/{subscription-id}/providers/{resource-provider-namespace}/locations/{region-location}/register?api-version={api-version}

中かっこ付きのパラメーターは、次の要素を表します。

- **サブスクリプション id** -Azure サブスクリプション id。
- **リソース プロバイダーの名前空間** -使用しているプロバイダーの名前空間。 ネットワーク リソース プロバイダーの値は *Microsoft.Network*します。
- **リージョン名** -Azure リージョン名

REST API を呼び出す際に、次の HTTP メソッドがサポートされます。

- **PUT** - 特定の種類のリソースを作成、リソース プロパティの変更、またはリソース間の関連付けを変更に使用されます。 
- **取得** – プロビジョニングされているリソースの情報の取得に使用します。
- **削除** - 既存のリソースの削除に使用されます。

要求と応答はいずれも JSON ペイロードの形式に準拠しています。 詳細については、次を参照してください。 [Azure リソース管理 Api](https://msdn.microsoft.com/library/azure/dn948464.aspx)します。

### ARM テンプレートの言語
(API または SDK を使って) 命令型でリソースを管理するだけでなく、ARM テンプレート言語を使用することで、宣言型のプログラミング形式によってネットワーク リソースをビルドおよび管理することもできます。 

テンプレートのサンプル表現を次に示します – 

    {
      "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
      "contentVersion": "<version-number-of-template>",
      "parameters": { <parameter-definitions-of-template> },
      "variables": { <variable-definitions-of-template> },
      "resources": [ { <definition-of-resource-to-deploy> } ],
      "outputs": { <output-of-template> }    
    }

テンプレートとは基本的に言ってリソースおよびインスタンス値をパラメーターで挿入した JSON による記述です。 次の例を使用すると、2 つのサブネットを含む仮想ネットワークを作成できます。

    {
        "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/VNET.json",
        "contentVersion": "1.0.0.0",
        "parameters" : {
          "location": {
            "type": "String",
            "allowedValues": ["East US", "West US", "West Europe", "East Asia", "South East Asia"],
            "metadata" : {
              "Description" : "Deployment location"
            }
          },
          "virtualNetworkName":{
            "type" : "string",
            "defaultValue":"myVNET",
            "metadata" : {
              "Description" : "VNET name"
            }
          },
          "addressPrefix":{
            "type" : "string",
            "defaultValue" : "10.0.0.0/16",
            "metadata" : {
              "Description" : "Address prefix"
            }
    
          },
          "subnet1Name": {
            "type" : "string",
            "defaultValue" : "Subnet-1",
            "metadata" : {
              "Description" : "Subnet 1 Name"
            }
          },
          "subnet2Name": {
            "type" : "string",
            "defaultValue" : "Subnet-2",
            "metadata" : {
              "Description" : "Subnet 2 name"
            }
          },
          "subnet1Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.0.0/24",
            "metadata" : {
              "Description" : "Subnet 1 Prefix"
            }
          },
          "subnet2Prefix" : {
            "type" : "string",
            "defaultValue" : "10.0.1.0/24",
            "metadata" : {
              "Description" : "Subnet 2 Prefix"
            }
          }
        },
        "resources": [
        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "[parameters('virtualNetworkName')]",
          "location": "[parameters('location')]",
          "properties": {
            "addressSpace": {
              "addressPrefixes": [
                "[parameters('addressPrefix')]"
              ]
            },
            "subnets": [
              {
                "name": "[parameters('subnet1Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet1Prefix')]"
                }
              },
              {
                "name": "[parameters('subnet2Name')]",
                "properties" : {
                  "addressPrefix": "[parameters('subnet2Prefix')]"
                }
              }
            ]
          }
        }
        ]
    }

テンプレートを使用する場合は、パラメーター値を手動で提供するオプションか、パラメーター ファイルを使用するオプションがあります。 次の例は、上記のテンプレートで使用可能なパラメーター値のセットを示しています。

    {
      "location": {
          "value": "East US"
      },
      "virtualNetworkName": {
          "value": "VNET1"
      },
      "subnet1Name": {
          "value": "Subnet1"
      },
      "subnet2Name": {
          "value": "Subnet2"
      },
      "addressPrefix": {
          "value": "192.168.0.0/16"
      },
      "subnet1Prefix": {
          "value": "192.168.1.0/24"
      },
      "subnet2Prefix": {
          "value": "192.168.2.0/24"
      }
    }


テンプレートを使用する主な利点は、次のとおりです。

- 宣言型形式でリソース グループに複雑なインフラストラクチャをビルドできます。 依存関係の管理など、リソースを作成するオーケストレーションは、ARM によって処理されます。 
- インフラストラクチャは、パラメーターを変更するだけで、さまざまなリージョンで、あるいは同一リージョン内で、繰り返し作成することができます。 
- 宣言型形式では、テンプレートの作成とインフラストラクチャの展開におけるリード タイムを短縮できます。 

サンプル テンプレートを参照してください。 [Azure クイック スタート テンプレート](https://github.com/Azure/azure-quickstart-templates)します。

ARM テンプレート言語の詳細については、次を参照してください。 [Azure リソース マネージャー テンプレートの言語](../resource-group-authoring-templates.md)です。

上記のサンプル テンプレートでは、仮想ネットワークとサブネットのリソースを使用します。 その他の使用可能なネットワーク リソースは、次の一覧のとおりです。

### テンプレートの使用

テンプレートから Azure にサービスをデプロイするには、PowerShell、AzureCLI を使用するか、または GitHub からクリック操作によってデプロイします。 GitHub でテンプレートからサービスをデプロイするには、次の手順を実行します。

1. GitHub で template3 ファイルを開きます。 たとえば、開く [2 つのサブネットを含む仮想ネットワーク](https://github.com/Azure/azure-quickstart-templates/tree/master/101-virtual-network)します。
2. をクリックして **Deploy to Azure**, 、資格情報使用の Azure ポータル サインインします。
3. クリックして、テンプレートを確認 **保存**します。
4. をクリックして **パラメーターの編集** など、場所の選択と *米国西部*, 、vnet およびサブネット。
5. 必要に応じて、変更、 **ADDRESSPREFIX** と **SUBNETPREFIX** パラメーター、およびクリック **[ok]**します。
6. クリックして **リソース グループの選択** vnet とサブネットを追加するリソース グループの順にクリックします。 クリックして、新しいリソース グループを作成する代わりに、 **新規作成**します。
3. クリックして **作成**します。 タイトルが表示 **プロビジョニング テンプレートのデプロイ**します。 デプロイが完了すると、画面の表示は以下のようになります。

![サンプル テンプレートのデプロイ](./media/resource-groups-networking/Figure6.png)


## 関連項目

[Azure ネットワーク API のリファレンス](https://msdn.microsoft.com/library/azure/dn948464.aspx)

[ネットワーク用 Azure PowerShell のリファレンス](https://msdn.microsoft.com/library/azure/mt163510.aspx)

[Azure リソース マネージャー テンプレートの言語](../resource-group-authoring-templates.md)

[Azure のネットワーク – 一般的に使用されているテンプレート](https://github.com/Azure/azure-quickstart-templates)

[コンピューティング リソース プロバイダー](../virtual-machines-azurerm-versus-azuresm)

[Azure リソース マネージャーの概要](../resource-group-overview)

[Azure リソース マネージャーでのロール ベースのアクセス制御](https://msdn.microsoft.com/library/azure/dn906885.aspx) 

[Azure リソース マネージャーでのタグの使用](https://msdn.microsoft.com/library/azure/dn848368.aspx)

[テンプレートのデプロイ](https://msdn.microsoft.com/library/azure/dn790549.aspx) 

現在

