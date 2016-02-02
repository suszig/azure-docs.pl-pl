<properties 
    pageTitle="Redis Cache のプロビジョニング" 
    description="Azure リソース マネージャー テンプレートを使用し、Redis Cache をデプロイします。" 
    services="app-service" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="web" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/16/2015" 
    ms.author="tomfitz"/>


# テンプレートを使用して Redis Cache を作成する

このトピックでは、Azure Redis Cache をデプロイする Azure リソース マネージャーのテンプレートを作成する方法について説明します。 キャッシュを既存のストレージ アカウントで使用することで、診断データを保持することができます。 デプロイ対象のリソースを定義する方法を学習し、 
デプロイの実行時に指定するパラメーターを定義する方法。 このテンプレートは、独自のデプロイに使用することも、要件に合わせてカスタマイズすることもできます。

現時点では、診断設定は、サブスクリプションの同じリージョン内のすべてのキャッシュで共有されます。 領域内の 1 つのキャッシュを更新すると、領域内の他のすべてのキャッシュに反映されます。

テンプレートの作成方法の詳細については、次を参照してください。 [Azure リソース マネージャー テンプレートの作成](../resource-group-authoring-templates.md)します。

完全なテンプレートを参照してください。 [Redis Cache のテンプレート](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json)します。
>[AZURE.NOTE] ARM テンプレートの新しい [Premium 階層](cache-premium-tier-intro.md) を利用します。 
>
>-    [クラスタ リングと Premium の Redis Cache を作成する](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
>-    [データの永続性での Premium Redis Cache の作成](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
>-    [VNet および省略可能なクラスタ リングでの Premium Redis Cache の作成](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
>
>最新のテンプレートを確認するを参照してください。 [Azure クイック スタート テンプレート](https://azure.microsoft.com/documentation/templates/) 探します `Redis Cache`します。

## デプロイ対象

このテンプレートでは、診断データ用に既存のストレージ アカウントを使用する Azure Redis Cache をデプロイします。

デプロイを自動的に実行するには、次のボタンをクリックします。

[![を Azure にデプロイ](./media/cache-redis-cache-arm-provision/deploybutton.png)] (https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## パラメーター

Azure リソース マネージャーを使用して、テンプレートのデプロイ時に値を指定するパラメーターを定義します。 テンプレートには、すべてのパラメーター値を含む Parameters という名前のセクションがあります。
展開しているかに基づいてプロジェクトに基づいて変化するこれらの値のパラメーターを定義する必要があります、 
環境に展開します。 常に同じ値に対してはパラメーターを定義しないでください。 テンプレートにおいて、それぞれのパラメーターの値は、デプロイ対象のリソースを定義するために使用されます。

テンプレートに含まれるそれぞれのパラメーターについて説明します。

[AZURE.INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### redisCacheLocation

Redis Cache の場所。 最適なパフォーマンスのために、アプリケーションとキャッシュで同じ場所を使用することをお勧めします。

    "redisCacheLocation": {
      "type": "string"
    }

### diagnosticsStorageAccountName

診断用に使用する既存のストレージ アカウントの名前。

    "diagnosticsStorageAccountName": {
      "type": "string"
    }

### enableNonSslPort

非 SSL ポート経由のアクセスを許可するかどうかを示すブール値です。

    "enableNonSslPort": {
      "type": "bool"
    }

### diagnosticsStatus

診断が有効かどうかを示す値です。 ON または OFF を使用します。

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## デプロイ対象のリソース

### Redis Cache

Azure Redis Cache を作成します。

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "redisVersion": "[parameters('redisCacheVersion')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
        "resources": [
          {
            "apiVersion": "2014-04-01",
            "type": "diagnosticSettings",
            "name": "service", 
            "location": "[parameters('redisCacheLocation')]",
            "dependsOn": [
              "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
            ],
            "properties": {
              "status": "[parameters('diagnosticsStatus')]",
              "storageAccountName": "[parameters('diagnosticsStorageAccountName')]",
              "retention": "30"
            }
          }
        ]
    }

## デプロイを実行するコマンド

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache -redisCacheLocation "West US"

### Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup



