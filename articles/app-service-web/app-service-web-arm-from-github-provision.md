<properties 
    pageTitle="GitHub リポジトリにリンクされる Web アプリのデプロイ" 
    description="GitHub リポジトリのプロジェクトを含む Web アプリをデプロイするには、Azure Resource Manager テンプレートを使用します。" 
    services="app-service" 
    documentationCenter="" 
    authors="tfitzmac" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/16/2015" 
    ms.author="tomfitz"/>

# GitHub リポジトリにリンクされる Web アプリのデプロイ

このトピックでは、GitHub リポジトリ内のプロジェクトにリンクされる Web アプリをデプロイする Azure Resource Manager テンプレートを作成する方法について説明します。 デプロイ対象のリソースを定義する方法を学習し、 
デプロイの実行時に指定するパラメーターを定義する方法。 このテンプレートは、独自のデプロイに使用することも、要件に合わせてカスタマイズすることもできます。

テンプレートの作成方法の詳細については、次を参照してください。 [Azure リソース マネージャー テンプレートの作成](../resource-group-authoring-templates.md)します。

完全なテンプレートを参照してください。 [Web アプリにリンクされている GitHub テンプレート](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json)します。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## デプロイ対象

このテンプレートを使用して、GitHub 内のプロジェクトのコードを含む Web アプリケーションをデプロイします。

デプロイメントを自動的に実行するには、次のボタンをクリックします。

[![DAzure に eploy](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## パラメーター

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### repoURL

デプロイするプロジェクトを含む GitHub リポジトリの URL。 このパラメーターは既定値を含みますが、この値は、リポジトリに URL を指定する方法を示すことのみを目的としています。 この値はテンプレートのテスト時に使用できますが、テンプレートを使用する場合には、独自のリポジトリに URL を指定します。

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### 分岐

アプリケーションのデプロイ時に使用するリポジトリの分岐。 既定値は master ですが、デプロイするリポジトリ内のいずれかの分岐の名前を指定することもできます。

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }
    
## デプロイ対象のリソース

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### Web Apps

GitHub 内のプロジェクトにリンクされる Web アプリを作成します。 

使用して web アプリの名前を指定する、 **siteName** パラメーター、および使用して web アプリの場所、 **siteLocation** パラメーター。  **DependsOn** 要素、テンプレートは、web アプリを定義します。 
サービス ホスティング プランには依存とします。 ホスティング プランに依存するため、ホスティング プランの作成が完了するまで、Web アプリは作成されません。  **DependsOn** 要素が配置を指定するためのみ 
順序です。 Azure Resource Mananger は、同時に両方のリソースを作成しようとします。 ホスティング プランに依存するものとして web アプリをオフにした場合と、web アプリがホストする前に作成された場合にエラーが発生する可能性があります。 
計画してください。

Web アプリにも子リソースで定義されている **リソース** 以下のセクションです。 この子リソースは、Web アプリと共にデプロイされるプロジェクトのソース管理を定義します。 このテンプレートでは、ソース管理 
特定の GitHub リポジトリにリンクされます。 GitHub リポジトリがコードで定義されている **"RepoUrl":"https://github.com/davidebbo-test/Mvc52Application.git"** する可能性がありますハード コード リポジトリの URL を繰り返しパラメーターの最小数を必要とするときに 1 つのプロジェクトを展開するテンプレートを作成するときにします。
リポジトリの URL をハードコーディングする代わりに、リポジトリの URL のパラメーターを追加およびをこの値を使用して、 **RepoUrl** プロパティです。 リポジトリ URL パラメーターの例を見ることができます、 
[Web ジョブ テンプレートを使用して web アプリ](../app-service-web-deploy-web-app-with-webjobs.md)します。

    {
      "apiVersion":"2015-04-01",
      "name":"[parameters('siteName')]",
      "type":"Microsoft.Web/sites",
      "location":"[parameters('siteLocation')]",
      "dependsOn":[
         "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties":{
        "serverFarmId":"[parameters('hostingPlanName')]"
      },
       "resources":[
         {
           "apiVersion":"2015-04-01",
           "name":"web",
           "type":"sourcecontrols",
           "dependsOn":[
             "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
           ],
           "properties":{
             "RepoUrl":"https://github.com/davidebbo-test/Mvc52Application.git",
             "branch":"master"
           }
         }
       ]
     }

## デプロイを実行するコマンド

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### PowerShell

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -siteLocation "West US" -ResourceGroupName ExampleDeployGroup

### Azure CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json


 

