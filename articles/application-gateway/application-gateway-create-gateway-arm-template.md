
<properties 
   pageTitle="Azure リソース マネージャー テンプレートを使用した Application Gateway の作成 | Microsoft Azure"
   description="このページでは、Azure リソース マネージャー テンプレートを使用して、Azure Application Gateway を作成する方法について説明します。"
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
   ms.date="11/10/2015"
   ms.author="joaoma"/>


# ARM テンプレートを使用して Application Gateway を作成する

Application Gateway はロード バランサーの第 7 層です。 クラウドでもオンプレミスでも、異なるサーバー間のフェールオーバーと HTTP 要求のパフォーマンス ルーティングを提供します。 Application Gateway は、HTTP 負荷分散、クッキー ベースのセッション アフィニティ、SSL オフロードなどのアプリケーション配信機能を備えています。 

> [AZURE.SELECTOR]
- [従来の azure PowerShell](application-gateway-create-gateway.md)
- [Azure リソース マネージャーの PowerShell](application-gateway-create-gateway-arm.md)
- [Azure リソース マネージャーのテンプレート](application-gateway-create-gateway-arm-template.md)

<BR>

GitHub から既存の ARM テンプレートをダウンロードして変更し、そのテンプレートを GitHub、PowerShell、および Azure CLI からデプロイする方法を説明します。

GitHub から直接 ARM テンプレートをデプロイするだけで、変更を加えない場合は、Github からのテンプレートのデプロイに進んでください。


>[AZURE.IMPORTANT] Azure リソースを使用する前に、Azure が現在 2 つの配置モデルを持つことを理解しておく: リソース マネージャーとクラシックします。 理解しておいてください [とツールの配置モデル](azure-classic-rm.md) あらゆる Azure リソースを使用する前にします。 この記事の上部にあるタブをクリックすると、さまざまなツールについてのドキュメントを参照できます。このドキュメントでは、Azure リソース マネージャーを使用した Application Gateway の作成について説明します。 従来のバージョンを使用するには [PowerShell を使用してアプリケーション ゲートウェイ クラシック展開の作成](application-gateway-create-gateway.md)します。




## シナリオ

このシナリオでは次のものを作成します。

- Application Gateway (2 インスタンス)
- VirtualNetwork1 という名前の VNet、予約済み CIDR ブロック 10.0.0.0/16
- Appgatewaysubnet という名前のサブネット、CIDR ブロック 10.0.0.0/28
- トラフィックを負荷分散する Web サーバー用に設定した 2 個の事前構成済みバックエンド IP。 このテンプレート例で使用するバックエンド IP は 10.0.1.10 および 10.0.1.11 です

>[AZURE.NOTE] これらは、このテンプレートのパラメーターです。 テンプレートをカスタマイズするために azuredeploy.json を開くルール、リスナー、SSL を変更できます。



![arm-scenario](./media/application-gateway-create-gateway-arm-template/scenario-arm.png)



## ARM テンプレートのダウンロードおよび理解

Github から既存の ARM テンプレートをダウンロードして VNet と 2 つのサブネットを作成し、そのテンプレートに変更を加えて再利用することができます。 再利用するには、次の手順に従ってください。

1. Https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-create-application-gateway/に移動します。
2. クリックして **azuredeploy.json**, 、] をクリックし、 **RAW**します。
3. お使いのコンピューター上のローカル フォルダーにファイルを保存します。
4. ARM テンプレートを使用したことがある場合は、手順 7. に進みます。
5. 保存したファイルを開き、フォルダー内のコンテンツを見て **パラメーター** 5 行目にします。 ARM テンプレートのパラメーターでは、デプロイ中に入力できる、値のプレース ホルダーが用意されています。

    | パラメーター | 説明 |
    |---|---|
    | **location** | Application Gateway を作成する Azure リージョン |
    | **VirtualNetwork1** | 新しい VNet の名前 |
    | **addressPrefix** | VNet のアドレス空間 (CIDR 形式) |
    | **ApplicationGatewaysubnet** | Application Gateway サブネットの名前 |
    | **subnetPrefix** | Application Gateway サブネットの CIDR ブロック |
    | **skuname** | SKU インスタンスのサイズ |
    | **容量** | インスタンスの数 |
    | **backendaddress1** | 1 番目の Web サーバーの IP アドレス |
    | **backendaddress2** | 2 番目の Web サーバーの IP アドレス |
    

>[AZURE.IMPORTANT] ARM テンプレートを github に保持されますが、時間の経過と共に変更できます。 使用する前に、必ずテンプレートを確認してください。
    
6. 下にあるコンテンツを確認して **リソース** し、次のことを確認します。

    - **型**します。 テンプレートによって作成されるリソースのタイプ。 この場合、 **Microsoft.Network/applicationGateways**, 、アプリケーション ゲートウェイを表します。
    - **名前**します。 リソースの名前です。 使用して **[parameters('applicationGatewayName')]**, 、展開時にユーザーまたはパラメーター ファイルが入力として指定されますので、名前。
    - **プロパティ**します。 リソースのプロパティの一覧です。 このテンプレートは、Application Gateway の作成の過程で、仮想ネットワークとパブリック IP アドレスを使用します。

7. Https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-create-application-gateway/azuredeploy.json に移動します。
8. クリックして **azuredeploy paremeters.json**, 、] をクリックし、 **RAW**します。
9. お使いのコンピューター上のローカル フォルダーにファイルを保存します。
10. 保存したファイルを開き、パラメーターの値を編集します。 次の値を使用して、このシナリオで説明した Application Gateway をデプロイします。

        {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        {
        "location" : {
        "value" : "West US"
        },
        "addressPrefix": {
        "value": "10.0.0.0/16"
        },
        "subnetPrefix": {
        "value": "10.0.0.0/24"
        },
        "skuName": {
        "value": "Standard_Small"
        },
        "capacity": {
        "value": 2
        },
        "backendIpAddress1": {
        "value": "10.0.1.10"
        },
        "backendIpAddress2": {
        "value": "10.0.1.11"
        }
        }

11. ファイルを保存します。Json テンプレートとパラメーターのテンプレートのようなオンラインの json 検証ツールの使用をテストできる [JSlint.com](http://www.jslint.com/)
 
## PowerShell を使用して ARM テンプレートをデプロイする

1. Azure PowerShell を初めて使用する場合は、次を参照してください。 [インストールおよび Azure PowerShell の構成方法](powershell-install-configure.md) Azure にサインインし、サブスクリプションを選択最後までの指示に従います。
2. Azure PowerShell プロンプトから実行、  **Switch-azuremode** コマンドレットを次に示すように、リソース マネージャー モードに切り替えます。

        Switch-AzureMode AzureResourceManager
    
予想される出力:

        WARNING: The Switch-AzureMode cmdlet is deprecated and will be removed in a future release.

>[AZURE.WARNING] Switch-azuremode コマンドレットは間もなく廃止されます。 これらが廃止された場合は、すべてのリソース マネージャー コマンドレットの名前が変更されます。
    
3. 必要であれば、`New-AzureResourceGroup` コマンドレットを使用して新しいリソース グループを作成します。以下の例では、米国東部に AppgatewayRG という名前のリソース グループを新しく作成します。

        PS C:\> New-AzureResourceGroup -Name AppgatewayRG -Location "East US"
        VERBOSE: 5:38:49 PM - Created resource group 'AppgatewayRG' in location 'eastus'


        ResourceGroupName : AppgatewayRG
        Location          : eastus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                     Actions  NotActions
                     =======  ==========
                      *

        ResourceId        : /subscriptions/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/resourceGroups/AppgatewayRG

4. New-AzureResourceGroupDeployment コマンドレットを実行し、上記でダウンロードおよび変更したテンプレート ファイルとパラメーター ファイルを使用して、新しい VNet をデプロイします。

        New-AzureResourceGroupDeployment -Name TestAppgatewayDeployment -ResourceGroupName AppgatewayRG `
           -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json

コマンド ラインで生成される出力は、次のようになります。

        DeploymentName    : testappgatewaydeployment
        ResourceGroupName : appgatewayRG
        ProvisioningState : Succeeded
        Timestamp         : 9/19/2015 1:49:41 AM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                   Name             Type                       Value
                   ===============  =========================  ==========
                   location         String                     East US
                   addressPrefix    String                     10.0.0.0/16
                   subnetPrefix     String                     10.0.0.0/24
                   skuName          String                     Standard_Small
                   capacity         Int                        2
                   backendIpAddress1  String                     10.0.1.10
                   backendIpAddress2  String                     10.0.1.11
                    
        Outputs           :


## Azure CLI を使用して ARM テンプレートをデプロイする

Azure CLI を使用してダウンロードした ARM テンプレートをデプロイするには、次の手順に従います。

1. Azure CLI を初めて使用する場合は、次を参照してください。 [のインストールと Azure CLI の構成](xplat-cli-install.md) Azure アカウントとサブスクリプションを選択する時点までの指示に従います。
2. 実行、 **azure config モード** コマンドを次に示すように、リソース マネージャー モードに切り替えます。

        azure config mode arm

上記のコマンドで想定される出力を次に示します。

        info:   New mode is arm

3. 必要に応じて、実行、 **azure グループを作成** を次に示すように、新しいリソース グループを作成します。 コマンドの出力が表示されます。 出力の後に表示されるリストは、使用されたパラメーターについての説明です。 リソース グループの詳細については、次を参照してください。 [Azure リソース マネージャーの概要](resource-group-overview.md)します。

        azure group create -n appgatewayRG -l eastus

**n (または - 名前)**します。 新しいリソース グループの名前です。 このシナリオの *appgatewayRG*します。

**-l (または --location)**します。 新しいリソース グループが作成される Azure リージョンです。 このシナリオの *Eastus*します。

4. 実行、 **azure グループの展開を作成** テンプレートとパラメーターを使用して、新しい VNet を展開するコマンドレットではファイルをダウンロードし、上の変更します。 出力の後に表示されるリストは、使用されたパラメーターについての説明です。

        azure group deployment create -g appgatewayRG -n TestAppgatewayDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

上記のコマンドで想定される出力を次に示します。

        azure group deployment create -g appgatewayRG -n TestAppgatewayDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json
        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestAppgatewayDeployment"
        + Waiting for deployment to complete
        data:    DeploymentName     : TestAppgatewayDeployment
        data:    ResourceGroupName  : appgatewayRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-09-21T20:50:27.5129912Z
        data:    Mode               : Incremental
        data:    Name               Type    Value
        data:    -----------------  ------  --------------
        data:    location           String  East US
        data:    addressPrefix      String  10.0.0.0/16
        data:    subnetPrefix       String  10.0.0.0/24 
        data:    skuName            String  Standard_Small
        data:    capacity           Int     2
        data:    backendIpAddress1  String  10.0.1.10
        data:    backendIpAddress2  String  10.0.1.11
        info:    group deployment create command OK

**-g (または - リソース グループ)**します。 新しい VNet の作成場所となるリソース グループの名前です。

**-f (または - テンプレート ファイル)**します。 ARM テンプレート ファイルへのパスです。

**-e (または - パラメーター ファイル)**します。 ARM パラメーター ファイルへのパスです。 

## [デプロイ] をクリックして ARM テンプレートをデプロイする

クリックしてデプロイは、ARM テンプレートを使用するもう 1 つの方法です。 これは、Azure ポータルでテンプレートを使用する簡単な方法です。 


### 手順 1. 
リンクを使用して [アプリケーション ゲートウェイを展開する] をクリックします。](https://azure.microsoft.com/documentation/templates/101-application-gateway-public-ip/) するページにリダイレクトされます、ポータルのテンプレートのアプリケーション ゲートウェイです。


### 手順 2. 

[Azure へのデプロイ] をクリックします。

![arm-scenario](./media/application-gateway-create-gateway-arm-template/deploytoazure.png)

### 手順 3.

ポータルでのデプロイ テンプレートのパラメーターを入力し、[OK] をクリックします。

![arm-scenario](./media/application-gateway-create-gateway-arm-template/ibiza1.png)

### 手順 4.

[使用条件] を選択し、[購入] をクリックします。

### 手順 5.

[カスタム デプロイ] ブレードで、[作成] をクリックします。


 
## 次のステップ

SSL オフロードを構成する場合は、「 [SSL 用のアプリケーション ゲートウェイ構成負荷を軽減](application-gateway-ssl.md)します。

ILB とともに使用して、参照してくださいアプリケーション ゲートウェイを構成する場合は、 [内部ロード バランサー (ILB) アプリケーション ゲートウェイの作成](application-gateway-ilb.md)します。

負荷分散のオプション全般の詳細については、次を参照してください。

- [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure の Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)

