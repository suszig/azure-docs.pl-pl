<properties
   pageTitle="Azure リソース マネージャーでのリンクされたテンプレートの使用"
   description="Azure リソース マネージャー テンプレートでリンクされたテンプレートを使用して、モジュール構造のテンプレート ソリューションを作成する方法について説明します。 パラメーターの値を渡す方法、パラメーター ファイルを指定する方法、および URL を動的に作成する方法を示します。"
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
   ms.date="12/07/2015"
   ms.author="tomfitz"/>

# Azure リソース マネージャーでのリンクされたテンプレートの使用

1 つの Azure リソース マネージャー テンプレート内から別のテンプレートにリンクして、対象となる、目的に特化した一連のテンプレートにデプロイを分解することができます。 同様に、アプリケーションの分解 
コード クラスの数には、分解は、テスト、再利用、読みやすさの面でメリットを提供します。  

メイン テンプレートからリンクされたテンプレートにパラメーターを渡すことができます。それらのパラメーターは、呼び出し元のテンプレートによって公開されているパラメーターまたは変数に直接マップできます。 戻る output 変数を渡すことも、リンクされたテンプレート 
ソース テンプレートには、テンプレート間で双方向のデータ交換を有効にします。

## テンプレートへのリンク

2 つのテンプレート間のリンクを作成するには、リンク先のテンプレートを指すデプロイ リソースをメイン テンプレート内に追加します。 設定する、 **templateLink** プロパティをリンクされたテンプレートの URI。 Visual Studio Online アカウントは 
リンクされたテンプレートのパラメーター値を指定するか、テンプレートまたは直接で値を指定します。 
パラメーター ファイルにリンクしています。 次の例では、 **パラメーター** 、パラメーター値を直接指定するプロパティです。

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "nestedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

リソース マネージャー サービスはリンクされたテンプレートにアクセスできる必要があります。このため、リンクされたテンプレートには、ローカル ファイルまたはローカル ネットワーク上でのみ使用できるファイルは指定できません。 いずれかを含む URI 値を指定することのみできます **http** または **https**します。 オプションの 1 つとして、ストレージ アカウントにリンク先のテンプレートを配置し、次に示すように、その項目の URI を使用します。

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }


## パラメーター ファイルへのリンク

次の例では、 **parametersLink** パラメーター ファイルにリンクするプロパティです。

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "nestedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

リンクされているパラメーター ファイルの URI 値が、ローカル ファイルにすることはできませんし、いずれかを含める必要があります **http** または **https**します。

## 変数を使用したテンプレートのリンク

前の例では、URL の値をハード コーディングしてテンプレートをリンクする方法について説明しました。 この方法は簡単なテンプレートには適していますが、モジュール構造の大規模な一連のテンプレートを使用する場合にはあまり適していません。 ベース URL を格納する静的変数を作成する代わりに、 
メイン テンプレートにし、そのベース URL からリンクされたテンプレートの Url を動的に作成します。 この方法の利点がすることができます。 
簡単に移動したり、メイン テンプレートで静的変数を変更するだけで済むため、テンプレートを分岐します。 正しい URI が、メイン テンプレートから、分解されたテンプレート全体に渡されます。

次の例は、ベース URL を使用して、2 つの Url リンクされたテンプレートを作成する方法を示しています (**sharedTemplateUrl** と **vmTemplate**)。 

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

使用することも [deployment()](../resource-group-template-functions/#deployment) を現在のテンプレートのベース URL を取得しを使用して、同じ場所にその他のテンプレートの URL を取得します。 これは、テンプレートの場所が変更された場合や (バージョン管理などのため)、テンプレート ファイルのハード コーディング URL を回避する必要がある場合に便利です。 

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## リンクされたテンプレートから値を渡す

メイン テンプレートにリンクされたテンプレートから値を渡す必要がある場合は、値を作成することができます **出力** リンクされたテンプレートのセクションです。 例については、次を参照してください。 
[Azure リソース マネージャー テンプレートの状態共有](best-practices-resource-manager-state.md)します。

## 次のステップ
- [テンプレートの作成](./resource-group-authoring-templates.md)
- [テンプレートのデプロイ](resource-group-template-deploy.md)

