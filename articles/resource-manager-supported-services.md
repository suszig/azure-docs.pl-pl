<properties
   pageTitle="リソース マネージャーでサポートされるサービスとリージョン |Microsoft Azure"
   description="リソース マネージャーをサポートするリソース プロバイダーおよびリソースをホストできるリージョンについて説明します。"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/04/2015"
   ms.author="tomfitz"/>

# リソース マネージャーによるサービス、リージョン、API バージョンのサポート

Azure リソース マネージャーは、アプリケーションを構成するサービスをデプロイして管理するための新しい方法を提供します。 
すべてではありませんがほとんどのサービスはリソース マネージャーをサポートし、一部のサービスはリソース マネージャーを部分的にのみサポートします。 Microsoft が将来のソリューションの重要な情報がすべてのサービスに対してリソース マネージャーを有効にするまでは、 
各サービスの現在の状態を把握する必要がありますが一貫してサポートします。 このトピックでは、Azure リソース マネージャーに対してサポートされるリソース プロバイダーの一覧を示します。

リソースをデプロイするときは、リソースをサポートしているリージョンとリソースで利用できる API バージョンも知っておく必要があります。 セクションでは、 [リージョン](#supported-regions) 領域は、サブスクリプションとリソースの動作を確認する方法を示しています。 セクションでは、 [API のサポートされているバージョン](#supported-api-versions) 使用する API バージョンを確認する方法を示しています。

次の表では、各サービスがリソース マネージャーによるデプロイと管理をサポートするかどうかを示します。 という名前の列 **のリソースの移動** 両方にこの種類のリソースを移動するかどうかは、 
新しいリソース グループと、新しいサブスクリプションです。 という名前の列 **ポータル** を使用してサービスを作成するかどうかを示す、 [Azure ポータル](https://portal.azure.com)します。


## コンピューティング

| サービス | リソース マネージャーが有効 | ポータル | リソースの移動 | REST API | スキーマ |
| ------- | ------------------------ | -------------- | -------------- |-------- | ------ |
| Virtual Machines | あり | あり、多くのオプション | いいえ       | [VM の作成](https://msdn.microsoft.com/library/azure/mt163591.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Compute.json) |
| Batch
   | あり     | [あり (クラシックのみ)](https://portal.azure.com/#create/Microsoft.BatchAccount) |  あり  | [Batch REST](https://msdn.microsoft.com/library/azure/dn820158.aspx) |        |
| Dynamics Lifecycle Services | はい | いいえ |    |      |        |
| Virtual Machines (クラシック) | 制限あり | あり、多くのオプション | 部分的 (後述参照) | - | - |
| リモート アプリケーション | いいえ   | いいえ | -              | -        | -      |
| Service Fabric | いいえ | いいえ | -           | -        | -      |

「Virtual Machines (クラシック)」とは、リソース マネージャー デプロイ モデルではなくクラシック デプロイ モデルを使用してデプロイされたリソースのことです。 一般に、これらのリソースが残念ですが、リソース マネージャーの操作をサポートして 
有効になっているいくつかの操作です。 これらの配置モデルの詳細については、次を参照してください。 [理解リソース マネージャーの展開と従来のデプロイ](resource-manager-deployment-model.md)します。 

Virtual Machines (クラシック) リソースは、新しいリソース グループには移動できますが、新しいサブスクリプションには移動できません。

## ネットワーク

| サービス | リソース マネージャーが有効 | ポータル | リソースの移動 | REST API | スキーマ |
| ------- | ------- | -------- | -------------- | -------- | ------ |
| Application Gateway | はい |  |      |          |        |
| DNS     | あり     |  |               | [DNS ゾーンの作成](https://msdn.microsoft.com/library/azure/mt130622.aspx)         | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) |
| Load Balancer | あり |    |          | [Load Balancer の作成](https://msdn.microsoft.com/library/azure/mt163574.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) |
| Virtual Networks | はい | [はい](https://portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) | いいえ        | [Virtual Network の作成](https://msdn.microsoft.com/library/azure/mt163661.aspx) | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Network.json) |
| Traffic Manager | はい | いいえ |           | [Traffic Manager プロファイルの作成](https://msdn.microsoft.com/library/azure/mt163581.aspx) |        |
| ExpressRoute | はい | なし | いいえ             | [ExpressRoute REST](https://msdn.microsoft.com/library/azure/mt586720.aspx)  |       |

## データ + ストレージ

| サービス | リソース マネージャーが有効 | ポータル | リソースの移動 | REST API | スキーマ |
| ------- | ------- | ------- | -------------- | -------- | ------ |
| DocumentDB | はい  | [はい](https://portal.azure.com/#create/Microsoft.DocumentDB) | あり  | [DocumentDB REST](https://msdn.microsoft.com/library/azure/dn781481.aspx) |   |
| Storage | はい     | [はい](https://portal.azure.com/#create/Microsoft.StorageAccount-ARM) |  いいえ  | [ストレージの作成](https://msdn.microsoft.com/library/azure/mt163564.aspx) | [ストレージ アカウント](resource-manager-template-storage.md) |
| Redis Cache | はい | [はい](https://portal.azure.com/#create/Microsoft.Cache.1.0.4) | あり |   | [2014-04-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Cache.json) |
| SQL Database | はい | [はい](https://portal.azure.com/#create/Microsoft.SQLDatabase.1.0.3) | あり  | [データベースの作成](https://msdn.microsoft.com/library/azure/mt163685.aspx) | [2014-04-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01-preview/Microsoft.Sql.json) |
| 検索 | はい  | [はい](https://portal.azure.com/#create/Microsoft.Search) | あり   | [Search REST](https://msdn.microsoft.com/library/azure/dn798935.aspx) |  |
| SQL Data Warehouse | はい | [あり](https://portal.azure.com/#create/Microsoft.SQLDataWarehouse.0.1.13-preview) |   |   |      |
| StorSimple | いいえ   | いいえ | -  | -        | -       |
| マネージ キャッシュ | いいえ | いいえ | -             | -        | -       |

## Web とモバイル

| サービス | リソース マネージャーが有効 | ポータル | リソースの移動 | REST API | スキーマ |
| ------- | ------- | -------- | -------------- | -------- | ------ |
| API Management | はい | なし | あり  | [API の作成](https://msdn.microsoft.com/library/azure/dn781423.aspx#CreateAPI) |        |
| API Apps | はい | [あり](https://portal.azure.com/#create/Microsoft.ApiApp) |   |   | [2015-03-01-preview](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-03-01-preview/Microsoft.AppService.json) |
| Web Apps | はい | [はい](https://portal.azure.com/#create/Microsoft.WebSite)  | あり、制限あり (後述参照) |          | [2015-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-08-01/Microsoft.Web.json) |
| Notification Hubs | はい | [はい](https://portal.azure.com/#create/Microsoft.NotificationHub) | あり  | [Notification Hub の作成](https://msdn.microsoft.com/library/azure/dn223269.aspx) | [2015-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2015-04-01/Microsoft.NotificationHubs.json) |
| Logic Apps | はい  | [はい](https://portal.azure.com/#create/Microsoft.EmptyWorkflow.0.2.0-preview) | はい   |          |        |
| Mobile Engagement | はい | なし |  はい  |          |        |

Web アプリを使用している場合、App Service プランのみを移動することはできません。 Web アプリを移動するには、次のオプションがあります。

- 移動先のリソース グループにまだ Microsoft.Web リソースがない場合は、すべてのリソースをあるリソース グループから別のリソース グループに移動します。
- Web アプリは別のリソース グループに移動しますが、App Service プランは元のリソース グループで保持します。

## 分析

| サービス | リソース マネージャーが有効 | ポータル | リソースの移動 | REST API | スキーマ |
| ------- | ------- | --------- | -------------- | -------- | ------ |
| イベント ハブ | はい   | いいえ |         | [Event Hub の作成](https://msdn.microsoft.com/library/azure/dn790676.aspx) |        |
| Stream Analytics | はい | [はい](https://portal.azure.com/#create/Microsoft.StreamAnalyticsJob) |        |          |        |
| HDInsights | はい  | [はい](https://portal.azure.com/#create/Microsoft.HDInsightCluster) | はい     |          |        |
| Data Factory | はい | [はい](https://portal.azure.com/#create/Microsoft.DataFactory) | あり | [Data Factory の作成](https://msdn.microsoft.com/library/azure/dn906717.aspx) |    |
| Machine Learning | いいえ | いいえ | -          | -        | -      |
| データ カタログ | いいえ | いいえ |  -             | -        | -       |

## メディアと CDN

| サービス | リソース マネージャーが有効 | ポータル | リソースの移動 | REST API | スキーマ |
| ------- | ------- | -------- | -------------- | -------- | ------ |
| CDN | あり | [あり](https://portal.azure.com/#create/Microsoft.CDN) |  |  |  |
| メディア サービス | いいえ | いいえ |  |  |  |


## ハイブリッド統合

| サービス | リソース マネージャーが有効 | ポータル | リソースの移動 | REST API | スキーマ |
| ------- | ------- | -------------- | -------------- | -------- | ------ |
| BizTalk Services | はい | いいえ |        |          | [2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.BizTalkServices.json) |
| Service Bus | あり | いいえ |     | [Service Bus REST](https://msdn.microsoft.com/library/azure/hh780717.aspx) |        |
| バックアップ | いいえ | いいえ | -              | -        | -       |
| Site Recovery | いいえ | いいえ | -             | -        | -       |

## ID 管理とアクセス管理 

| サービス | リソース マネージャーが有効 | ポータル | リソースの移動 | REST API | Schema |
| ------- | ------- | -------------- | -------------- | -------- | ------ |
| Azure Active Directory | いいえ | いいえ | -  | - | - |
| Azure の Actice Directory B2C | いいえ | いいえ | - | - | - |
| Multi-Factor Authentication | いいえ | いいえ | - | - | - |

## 開発者サービス 

| サービス | リソース マネージャーが有効 | ポータル | リソースの移動 | REST API | スキーマ |
| ------- | ------- | ---------- | -------------- | -------- | ------ |
| Application Insights | はい | [はい](https://portal.azure.com/#create/Microsoft.AppInsights) | いいえ   |          | [2014-04-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-04-01/Microsoft.Insights.json) |
| Bing Maps | はい   | [はい](https://portal.azure.com/#create/bingmaps.mapapis.1.0.4) |         |          |        |
| Visual Studio アカウント | あり |  |      |          | [2014-02-26](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-02-26/microsoft.visualstudio.json) |

## 管理 

| サービス | リソース マネージャーが有効 | ポータル | リソースの移動 | REST API | スキーマ |
| ------- | ------- | --------- | -------------- | -------- | ------ |
| Automation | はい  | [はい](https://portal.azure.com/#create/Microsoft.AutomationAccount.1.0.5) | はい     |          |        |
| Key Vault | はい    | なし | あり            | [Key Vault](https://msdn.microsoft.com/library/azure/dn903609.aspx) |        |
| Scheduler | はい   | いいえ |        |          | [2014-08-01](https://github.com/Azure/azure-resource-manager-schemas/blob/master/schemas/2014-08-01/Microsoft.Scheduler.json) |
| Operational Insights | はい | なし | はい   |          |        |
| IoTHubs | あり     | [あり](https://portal.azure.com/#create/Microsoft.IotHub) |               |          |        |

## リソース マネージャー

| 機能 | リソース マネージャーが有効 | ポータル | リソースの移動 | REST API | スキーマ |
| ------- | ------- | -------- | -------------- | -------- | ------ |
| 承認 | はい | 該当なし | 該当なし | [管理ロック](https://msdn.microsoft.com/library/azure/mt204563.aspx)<br >[ロールベースのアクセス制御](https://msdn.microsoft.com/library/azure/dn906885.aspx)  | [リソース ロック](resource-manager-template-lock.md)<br />[ロールの割り当て](resource-manager-template-role.md)  |
| リソース | はい | 該当なし | 該当なし | [リンク済みリソース](https://msdn.microsoft.com/library/azure/mt238499.aspx) | [リソース リンク](resource-manager-template-links.md) |


## サポートされているリージョン

通常、リソースをデプロイするときはリソースのリージョンを指定する必要があります。 リソース マネージャーはすべてのリージョンでサポートされていますが、デプロイするリソースはすべてのリージョンではサポートされていない場合があります。 また、 
リソースをサポートする一部の地域の使用を禁止する、サブスクリプションに制限があります。 これらの制限事項は、税のホームお住まいの国の問題に関連する可能性や、ポリシーの結果が配置されます。 
特定の領域のみを使用するサブスクリプション管理者です。 

すべての Azure サービスのすべてのサポートされているリージョンの一覧については、次を参照してください。 [地域によってサービス](https://azure.microsoft.com/regions/#services)。 ただし、この一覧は、サブスクリプションがサポートされていない領域を含めることがあります。 次のコマンドを実行すると、ご利用のサブスクリプションでサポートされているリージョンをリソース タイプ別に確認できます。

### REST API

サブスクリプション内で特定のリソースの種類の利用可能な領域を確認するを使用して、 [すべてのリソース プロバイダーを一覧表示](https://msdn.microsoft.com/library/azure/dn790524.aspx) 操作します。 

### PowerShell

次の例では、Azure PowerShell 1.0 を使用して Web サイトのサポートされるリージョンを取得する方法を示します。 1.0 のリリースの詳細については、次を参照してください [Azure PowerShell 1.0。](https://azure.microsoft.com/blog/azps-1-0/)

    PS C:\> ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
    
次のように出力されます。

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia
    Central US
    East US 2

Azure PowerShell 0.9.8 では、次のコマンドを使用します。

    PS C:\> ((Get-AzureProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

### Azure CLI

次の例では、リソースの種類ごとにサポートされるすべての場所が返されます。

    azure location list

またなどのツールでの場所の結果をフィルター処理することもできます。 **jq**します。 Jq などのツールの詳細については、次を参照してください。 [Azure とやり取りする便利なツール](/virtual-machines/resource-group-deploy-debug/#useful-tools-to-interact-with-azure)します。

    azure location list --json | jq '.[] | select(.name == "Microsoft.Web/sites")'

次のような結果が返されます。

    {
      "name": "Microsoft.Web/sites",
      "location": "Brazil South,East Asia,East US,Japan East,Japan West,North Central US,
            North Europe,South Central US,West Europe,West US,Southeast Asia,Central US,East US 2"
    }

## サポートされている API バージョン

テンプレートをデプロイするとき、各リソースの作成に使用する API バージョンを指定する必要があります。 API バージョンは、リリース プロバイダーがリリースする REST API のバージョンに一致します。 
リソース プロバイダーは新しい機能を有効にするとき、REST API の新しいバージョンをリリースします。 したがって、テンプレートで指定した API のバージョンに影響するプロパティで指定できます、 
テンプレートです。 一般的に、新しいテンプレートを作成するときは、最新の API バージョンを選択します。 既存のテンプレートの場合、以前の API バージョンの使用を続けるか、テンプレートを最新版に更新して新しい機能を最大限に活用するか決定できます。

### REST API

リソースの種類の利用可能な API バージョンを確認するを使用して、 [すべてのリソース プロバイダーを一覧表示](https://msdn.microsoft.com/library/azure/dn790524.aspx) 操作します。 

### PowerShell

次の例では、Azure PowerShell 1.0 を利用し、特定のリソースの種類で利用できる API バージョンを取得する方法を示します。

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
    
次のように出力されます。
    
    2015-08-01
    2015-07-01
    2015-06-01
    2015-05-01
    2015-04-01
    2015-02-01
    2014-11-01
    2014-06-01
    2014-04-01-preview
    2014-04-01

Azure PowerShell 0.9.8 では、次を使用します。

    PS C:\> ((Get-AzureProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions

### Azure CLI

次のコマンドで、リソース プロバイダーの情報 (利用可能な API バージョンを含む) をファイルに保存できます。

    azure provider show Microsoft.Web -vv --json > c:\temp.json

ファイルを検索、 **apiVersions** 要素

## 次のステップ

- リソース マネージャー テンプレートの作成方法については、次を参照してください。 [Azure リソース マネージャーの作成テンプレート](resource-group-authoring-templates.md)します。
- リソースをデプロイする詳細については、次を参照してください。 [Azure リソース マネージャー テンプレートを使用してアプリケーションを配置](resource-group-template-deploy.md)します。

