<properties
   pageTitle="テンプレートを使用した VM のカスタム スクリプト | Microsoft Azure"
   description="カスタム スクリプト拡張機能とリソース マネージャー テンプレートを使用して Windows および Linux Azure VM 構成タスクを自動化する"
   services="virtual-machines"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure-services"
   ms.date="11/01/2015"
   ms.author="kundanap"/>

# Azure リソース マネージャー テンプレートでのカスタム スクリプト拡張機能の使用

この記事では、Linux VM または Windows VM のブートストラップ ワークロード用に、カスタム スクリプト拡張機能を使用して Azure リソース マネージャー テンプレートを作成する方法の概要を説明します。

カスタム スクリプトの概要については、拡張機能を参照してください、記事 [ここ](virtual-machines-extensions-customscript.md)します。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] [従来のデプロイ モデル](virtual-machines-extensions-customscript.md)します。

カスタム スクリプト拡張機能は、導入以来、Windows VM と Linux VM でワークロードを構成するために広く使用されてきました。 Azure リソース マネージャー テンプレートの導入により、VM をプロビジョニングするだけでなく、VM でワークロードも構成する単一のテンプレートを作成できるようになりました。

## Azure リソース マネージャーのテンプレートについて

Azure リソース マネージャー テンプレートでは、リソース間の依存関係を定義することで、JSON 言語で Azure IaaS インフラストラクチャを宣言によって指定できます。 Azure リソース マネージャー テンプレートの概要については、次の記事をご覧ください。

- [リソース グループの概要](../resource-group-overview)
- [Azure CLI を使用したテンプレートのデプロイ](virtual-machines-deploy-rmtemplates-azure-cli)
- [Azure Powershell を使用したテンプレートのデプロイ](virtual-machines-deploy-rmtemplates-powershell)

### 前提条件

1. [ここ](http://azure.microsoft.com/downloads)から最新の Azure PowerShell コマンドレットまたは Azure CLI をインストールします。
2. スクリプトは、既存の仮想マシンで実行は、VM で、VM エージェントが有効になっていることを確認そうでない場合に従う [この](virtual-machines-extensions-install) いずれかをインストールします。
3. VM で実行するスクリプトを Azure Storage にアップロードします。 スクリプトは、1 つか複数のストレージ コンテナーから取得できます。
4. スクリプトを Github アカウントにアップロードすることもできます。
5. スクリプトは、拡張機能によって起動されるエントリ スクリプトが、他のスクリプトを順に起動するように記述されている必要があります。

## カスタム スクリプト拡張機能の使用

テンプレートを展開するため、同じバージョンの Azure サービス管理 Api についてのメモは、カスタム スクリプト拡張機能を使用します。 拡張機能では、同じパラメーターとシナリオ (Azure ストレージ アカウントまたは Github へのファイルのアップロードなど) をサポートします。 テンプレートで使用する際の重要な違いは、拡張機能のバージョンを majorversion.* 形式で指定するのではなく、正確なバージョンを指定する必要があることです。

 ## Linux VM のテンプレートの例

テンプレートの Resource セクションで、次の拡張機能リソースを定義します。

      {
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "MyCustomScriptExtension",
    "apiVersion": "2015-05-01-preview",
    "location": "[parameters('location')]",
    "dependsOn": ["[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"],
    "properties":
    {
      "publisher": "Microsoft.OSTCExtensions",
      "type": "CustomScriptForLinux",
      "typeHandlerVersion": "1.2",
      "settings": {
      "fileUris": [ "https: //raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-ubuntu/mongo-install-ubuntu.sh                        ],
      "commandToExecute": "shmongo-install-ubuntu.sh"
      }
    }
    }

## Windows VM のテンプレートの例

テンプレートの Resource セクションで、次のリソースを定義します。

       {
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "MyCustomScriptExtension",
       "apiVersion": "2015-05-01-preview",
       "location": "[parameters('location')]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.Compute",
           "type": "CustomScriptExtension",
           "typeHandlerVersion": "1.4",
           "settings": {
               "fileUris": [
               "http://Yourstorageaccount.blob.core.windows.net/customscriptfiles/start.ps1"
           ],
           "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
         }
       }
     }

上記の例で、ファイルの URL とファイル名を独自の設定に置き換えます。

テンプレートを作成したら、Azure CLI または Azure Powershell を使用してテンプレートをデプロイできます。

カスタム スクリプト拡張機能を使用して、VM でアプリケーションを構成する完全なサンプルについては、次の例をご覧ください。

<a href="https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/" target="_blank">Linux VM 上のカスタム スクリプト拡張機能</a>.
</br>
<a href="https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/" target="_blank">Windows 仮想マシン上のカスタム スクリプト拡張機能</a>.

