<properties
   pageTitle="リソースの複数のインスタンスのデプロイ | Microsoft Azure"
   description="Azure リソース マネージャー テンプレートで copy 操作と配列を使用して、リソースをデプロイする際に複数回反復処理する方法について説明します。"
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
   ms.date="12/01/2015"
   ms.author="tomfitz"/>

# Azure リソース マネージャーでリソースの複数のインスタンスを作成する

このトピックでは、Azure リソース マネージャー テンプレートで反復処理して、リソースの複数のインスタンスを作成する方法について説明します。

## copy、copyIndex、length

複数回作成するリソース内で定義することができます、 **コピー** オブジェクトを反復処理する回数を指定します。 copy は次の形式で指定します。

    "copy": { 
        "name": "websitescopy", 
        "count": "[parameters('count')]" 
    } 

現在の反復値にアクセスすることができます、 **copyIndex()** 機能、concat 関数内に示すようにします。

    [concat('examplecopy-', copyIndex())]

値の配列から複数のリソースを作成時に行うこともできます、 **長さ** カウントを指定します。 length 関数のパラメーターとして、配列を指定します。

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

## name でのインデックス値の使用

copy 操作では、増分するインデックス値に基づいた一意の名前を付けたリソースの複数のインスタンスを作成できます。 それぞれの末尾に一意の番号を追加する例。 
展開されているリソースの名前です。 次の 3 つの Web サイトをデプロイするには:

- examplecopy-0
- examplecopy-1
- examplecopy-2.

次のテンプレートを使用します。

    "parameters": { 
      "count": { 
        "type": "int", 
        "defaultValue": 3 
      } 
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', copyIndex())]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[parameters('count')]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          }
      } 
    ]

## インデックス値のオフセット

前述の例では、インデックス値が 0 から 2 に増えることがわかります。 インデックス値をオフセットするには、値を渡すことができます、 **copyIndex()** などの関数 **copyindex (1)**します。 実行する反復処理の数は copy 要素で指定されたままですが、copyIndex の値が指定された値でオフセットされます。 このため、同じテンプレートを使用して、前の例としてを指定する **copyindex (1)** という 3 つの web サイトがデプロイされます。

- examplecopy-1
- examplecopy-2
- examplecopy-3

## 配列の使用
   
コピー操作は、配列内の各要素に対して反復処理するため、配列で作業するときに特に便利です。 次の 3 つの Web サイトをデプロイするには:

- examplecopy-Contoso
- examplecopy-Fabrikam
- examplecopy-Coho

次のテンプレートを使用します。

    "parameters": { 
      "org": { 
         "type": "array", 
             "defaultValue": [ 
             "Contoso", 
             "Fabrikam", 
             "Coho" 
          ] 
      }
    }, 
    "resources": [ 
      { 
          "name": "[concat('examplecopy-', parameters('org')[copyIndex()])]", 
          "type": "Microsoft.Web/sites", 
          "location": "East US", 
          "apiVersion": "2015-08-01",
          "copy": { 
             "name": "websitescopy", 
             "count": "[length(parameters('org'))]" 
          }, 
          "properties": {
              "serverFarmId": "hostingPlanName"
          } 
      } 
    ]

もちろん、コピー数を配列の長さ以外の値に設定します。 たとえば、さまざまな値を含む配列を作成し、デプロイする配列要素の数を指定するパラメーターの値を渡すことができます。 この場合は、最初の例に示すようにコピー数を設定します。 

## ループ内のリソースに基づく

使用して他のリソースの後は、リソースを展開するよう指定することができます、 **dependsOn** 要素。 使用することができます、ループ内のリソースのコレクションに依存するリソースを展開する必要がある場合を提供します 
ループであり、コピーの名前、 **dependsOn** 要素。 次の例では、仮想マシンをデプロイする前に 3 つのストレージ アカウントをデプロイする方法について示します。 完全な仮想マシン定義は示されていません。 注意して、 
要素のコピーが **名** に設定 **storagecopy** と **dependsOn** に仮想マシンの要素が設定 **storagecopy**します。

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "resources": [
            {
                "apiVersion": "2015-06-15",
                "type": "Microsoft.Storage/storageAccounts",
                "name": "[concat('storage', uniqueString(resourceGroup().id), copyIndex())]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "accountType": "Standard_LRS"
                 },
                "copy": { 
                    "name": "storagecopy", 
                    "count": 3 
                }
            },
           {
               "apiVersion": "2015-06-15", 
               "type": "Microsoft.Compute/virtualMachines", 
               "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
               "dependsOn": ["storagecopy"],
               ...
           }
        ],
        "outputs": {}
    }

## 入れ子になったリソースでのループ

入れ子になったリソースにコピー ループを使用することはできません。 通常他のリソース内で入れ子になったとして定義したリソースの複数のインスタンスを作成する場合は、代わりに最上位のリソースとしてリソースを作成してくださいから親のリソースとのリレーションシップを定義、 **型** と **名** プロパティです。

たとえば、通常は Data Factory 内の入れ子になったリソースとしてデータセットを定義する場合、

    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[variables('dataFactoryName')]",
        ...
        "resources": [
        {
            "type": "datasets",
            "name": "[variables('dataSetName')]",
            "dependsOn": [
                "[variables('dataFactoryName')]"
            ],
            ...
        }
    }]
    
データセットの複数のインスタンスを作成するには、次のように、テンプレートを変更する必要があります。 完全修飾のタイプと名前にデータ ファクトリ名が含まれていることに注目してください。

    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[variables('dataFactoryName')]",
        ...
    },
    {
        "type": "Microsoft.DataFactory/datafactories/datasets",
        "name": "[concat(variables('dataFactoryName'), '/', variables('dataSetName'), copyIndex())]",
        "dependsOn": [
            "[variables('dataFactoryName')]"
        ],
        "copy": { 
            "name": "datasetcopy", 
            "count": "[parameters('count')]" 
        } 
        ...
    }]

## 次のステップ
- テンプレートのセクションでは、について学習する場合は、「 [Azure リソース マネージャー テンプレートの作成](./resource-group-authoring-templates.md)します。
- テンプレートで使用できる関数のすべて、次を参照してください。 [Azure リソース マネージャーのテンプレート関数](./resource-group-template-functions.md)します。
- テンプレートをデプロイする方法については、「 [Azure リソース マネージャー テンプレートを使用してアプリケーションを配置](resource-group-template-deploy.md)します。

