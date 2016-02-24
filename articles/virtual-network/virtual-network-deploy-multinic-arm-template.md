<properties 
   pageTitle="リソース マネージャーでのテンプレートを使用した複数の NIC VM のデプロイ | Microsoft Azure"
   description="リソース マネージャーでテンプレートを使用して、複数の NIC を持つ VM をデプロイする方法を学習します。"
   services="virtual-network"
   documentationCenter="na"
   authors="telmosampaio"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="11/20/2015"
   ms.author="telmos" />

# テンプレートを使用した複数の NIC VM のデプロイ

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)] [従来のデプロイ モデル](virtual-network-deploy-multinic-classic-ps.md)します。

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

この時点では、単一の NIC を含む VM と複数の NIC を含む VM を同じリソース グループ内で保持できないため、バック エンド サーバーをリソース グループに実装し、他のすべてのコンポーネントを別のセキュリティ グループに実装します。 次の手順がという名前のリソース グループを使用して *IaaSStory* メイン リソース グループのおよび *IaaSStory バックエンド* バック エンド サーバーのです。

## 前提条件

バックエンド サーバーをデプロイするには、このシナリオで必要なすべてのリソースを含むメイン リソース グループをデプロイする必要があります。 これらのリソースをデプロイするには、以下の手順に従います。

1. 移動 [テンプレート ページ](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC)します。
2. テンプレート] ページで、以下の桁数に **親のリソース グループ**, をクリックして **Deploy to Azure**します。
3. 必要に応じて、パラメーター値を変更し、Azure プレビュー ポータル内の手順に従ってリソース グループをデプロイします。

> [AZURE.IMPORTANT] ストレージのアカウント名が一意であることを確認してください。 Azure では重複するストレージ アカウント名を使用できません。 

## デプロイ テンプレートについて

このドキュメントに付属するテンプレートをデプロイする前に、テンプレートを理解する必要があります。 次の手順では、対象のテンプレートの概要について説明しています。

1. 移動 [テンプレート ページ](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC)します。
2. クリックして **azuredeploy.json** テンプレート ファイルを開きます。
3. 通知、 *osType* 以下に示すパラメーターです。 このパラメーターは、複数のオペレーティング システム関連の設定と共に、データベース サーバー用の VM イメージを選択するために使用します。

        "osType": {
          "type": "string",
          "defaultValue": "Windows",
          "allowedValues": [
            "Windows",
            "Ubuntu"
          ],
          "metadata": {
            "description": "Type of OS to use for VMs: Windows or Ubuntu."
          }
        },

4. 変数の一覧まで下へスクロールしの定義を確認、 *dbVMSetting* 以下に示す変数。 含まれる配列要素のいずれかの受信、 *dbVMSettings* 変数です。 ソフトウェア開発の用語に置き換える場合は、表示、 *dbVMSettings* ハッシュ テーブル、または、dictionay として変数です。

        "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"

5. バック エンドで SQL が動作している Windows VM をデプロイするとします。 値、 *osType* は *Windows*, 、および *dbVMSetting* 変数には、次に示す最初の値を表す要素が含まれます、 *dbVMSettings* 変数です。

          "Windows": {
            "vmSize": "Standard_DS3",
            "publisher": "MicrosoftSQLServer",
            "offer": "SQL2014SP1-WS2012R2",
            "sku": "Standard",
            "version": "latest",
            "vmName": "DB",
            "osdisk": "osdiskdb",
            "datadisk": "datadiskdb",
            "nicName": "NICDB",
            "ipAddress": "192.168.2.",
            "extensionDeployment": "",
            "avsetName": "ASDB",
            "remotePort": 3389,
            "dbPort": 1433
          },

6. 通知、 *vmSize* 値を含む *Standard_DS3*します。 特定の VM のサイズに対してのみ、複数の NIC の使用が許可されます。 どの VM サイズは、複数 NIC にアクセスして有効になっていることを確認できる、 [複数 NIC の概要](virtual-networks-multiple-nics.md)します。
7. 下へスクロールして **リソース** に最初の要素に注目してください。 ここでは、ストレージ アカウントについて記述しています。 このストレージ アカウントは、各データベース VM で使用されるデータ ディスクの管理に使用されます。 このシナリオでは、各データベース VM で、OS ディスクが標準ストレージに格納され、2 つのデータ ディスクが SSD (Premium) ストレージに格納されます。

        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('prmStorageName')]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "Storage Account - Premium"
          },
          "properties": {
            "accountType": "[parameters('prmStorageType')]"
          }
        },

8. 下にスクロールして、次に示す次のリソースを表示します。 このリソースは、各データベース VM でデータベース アクセスに使用される NIC を表しています。 使用して、 **コピー** このリソース内の関数です。 テンプレートでは、多数の Vm、必要なに基づいてとして展開することができます、 *dbCount* パラメーター。 したがって、データベース アクセス用に同じ数 (VM ごとに 1 つ) の NIC を作成する必要があります。

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB DA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

9. 下にスクロールして、次に示す次のリソースを表示します。 このリソースは、各データベース VM で管理に使用される NIC を表しています。 ここでも、データベース VM ごとにこれらの NIC が 1 つ必要です。 通知、 *networkSecurityGroup* 要素、RDP、SSH のみこの NIC にへのアクセスを許可する NSG をリンクします。

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB RA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
                  },
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

10. 下にスクロールして、次に示す次のリソースを表示します。 このリソースは、すべてのデータベース VM によって共有される可用性セットを表しています。 このように、保守中に動作するセット内に常に 1 台の VM が存在するようにします。

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/availabilitySets",
          "name": "[variables('dbVMSetting').avsetName]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "AvailabilitySet - DB"
          }
        },

11. 下にスクロールして、次のリソースを表示します。 次の最初の数行が示すように、このリソースはデータベース VM を表しています。 使用して、 **コピー** ように複数の Vm は基に作成されますをもう一度、関数、 *dbCount* パラメーター。 また、 *dependsOn* コレクションです。 可用性セット、ストレージ アカウントと共に、VM をデプロイするときに、事前に作成する必要のある 2 つの NIC が示されます。

          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
          "location": "[variables('location')]",
          "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
            "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
            "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
          ],
          "tags": {
            "displayName": "VMs - DB"
          },
          "copy": {
            "name": "dbvmcount",
            "count": "[parameters('dbCount')]"
          },

12. VM のリソースでスクロール ダウンして、 **networkProfile** 要素を次に示すようです。 各 VM で参照される 2 つの NIC があることに注目してください。 設定する必要があります VM 用複数 Nic を作成するときに、 *プライマリ* に Nic のいずれかのプロパティ *true*, 、残りを *false*します。

        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
              "properties": { "primary": true }
            },
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
              "properties": { "primary": false }
            }
          ]
        }
      }

## [デプロイ] をクリックして ARM テンプレートをデプロイする

> [AZURE.IMPORTANT] 実行するかどうかを確認、 [の前提条件](#Pre-requisites) 以下の手順に従う前に手順を実行します。

パブリック リポジトリで使用できるサンプル テンプレートは、上記のシナリオの生成に使用した既定値を含むパラメーター ファイルを使用します。 クリックを使用して展開するこのテンプレートを展開するには、次の [このリンク](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC), の右側に **バックエンド リソース (ドキュメントを参照してください) をグループ化** ] をクリックして **Deploy to Azure**, 、必要に応じて、既定のパラメーター値を置き換えるし、ポータルの指示に従います。

次の図は、デプロイ後の新しいリソース グループの内容を示しています。

![Back end resource group](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## PowerShell を使用してテンプレートをデプロイする

PowerShell を使用してダウンロードしたテンプレートをデプロイするには、次の手順に従います。

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

3. 実行、 **新規 AzureRmResourceGroup** コマンドレットをテンプレートを使用してリソース グループを作成します。

        New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'   

    予想される出力:

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              : 
        Permissions       : 
                            Actions  NotActions
                            =======  ==========
                            *                  
                            
        Resources         : 
                            Name                 Type                                 Location
                            ===================  ===================================  ========
                            ASDB                 Microsoft.Compute/availabilitySets   westus  
                            DB1                  Microsoft.Compute/virtualMachines    westus  
                            DB2                  Microsoft.Compute/virtualMachines    westus  
                            NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                            wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  
                            
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## Azure CLI を使用してテンプレートをデプロイする

Azure CLI を使用してテンプレートをデプロイするには、次の手順に従います。

1. Azure CLI を初めて使用する場合は、次を参照してください。 [のインストールと Azure CLI の構成](xplat-cli.md) Azure アカウントとサブスクリプションを選択する時点までの指示に従います。
2. 実行、 **azure config モード** コマンドを次に示すように、リソース マネージャー モードに切り替えます。

        azure config mode arm

    上記のコマンドで想定される出力を次に示します。

        info:    New mode is arm

3. 開いている、 [パラメーター ファイル](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), 、その内容を選択して、コンピューターのファイルに保存します。 この例では、パラメーター ファイルを保存しました *parameters.json*します。

4. 実行、 **azure グループの展開を作成** テンプレートとパラメーターを使用して、新しい VNet を展開するコマンドレットではファイルをダウンロードし、上の変更します。 出力の後に表示されるリストは、使用されたパラメーターについての説明です。

        azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json

    予想される出力:

        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
