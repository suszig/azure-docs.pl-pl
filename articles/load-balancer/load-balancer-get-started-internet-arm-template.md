<properties 
   pageTitle="テンプレートを使用したリソース マネージャーでのインターネット接続ロード バランサーの作成 | Microsoft Azure"
   description="ARM テンプレートを使用して、リソース マネージャーでインターネット接続ロード バランサーを作成する方法について説明します。"
   services="load-balancer"
   documentationCenter="na"
   authors="joaoma"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="11/20/2015"
   ms.author="joaoma" />

# ARM テンプレートを使用したインターネットに接続するロード バランサーの作成の開始

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)] この記事では、リソース マネージャーの配置モデルについて説明します。 こともできます [インターネットへの従来のデプロイ モデルを使用してロード バランサーを作成する方法について](load-balancer-get-started-internet-classic-portal.md)


[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## [デプロイ] をクリックして ARM テンプレートをデプロイする

パブリック リポジトリで使用できるサンプル テンプレートは、上記のシナリオの生成に使用した既定値を含むパラメーター ファイルを使用します。 クリックを使用して展開するこのテンプレートを展開するには、次の [このリンク](http://go.microsoft.com/fwlink/?LinkId=544801), をクリックして **Deploy to Azure**, 、必要に応じて、既定のパラメーター値を置き換えるし、ポータルの指示に従います。

## PowerShell を使用して ARM テンプレートをデプロイする

PowerShell を使用してダウンロードした ARM テンプレートをデプロイするには、次の手順に従います。

1. Azure PowerShell を初めて使用する場合は、次を参照してください。 [インストールおよび Azure PowerShell の構成方法](powershell-install-configure.md) Azure にサインインし、サブスクリプションを選択最後までの指示に従います。
2. 実行、 **Switch-azuremode** コマンドレットを次に示すように、リソース マネージャー モードに切り替えます。

        Switch-AzureMode AzureResourceManager

    上記のコマンドで想定される出力を次に示します。

        WARNING: The Switch-AzureMode cmdlet is deprecated and will be removed in a future release.

    >[AZURE.WARNING] Switch-azuremode コマンドレットは間もなく廃止されます。 これらが廃止された場合は、すべてのリソース マネージャー コマンドレットの名前が変更されます。

3. 実行、 **New-azureresourcegroup** コマンドレットをテンプレートを使用してリソース グループを作成します。

        New-AzureResourceGroup -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-natrules/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-natrules/azuredeploy.parameters.json'  

## Azure CLI を使用して ARM テンプレートをデプロイする

Azure CLI を使用して ARM テンプレートをデプロイするには、次の手順に従います。

1. Azure CLI を初めて使用する場合は、次を参照してください。 [のインストールと Azure CLI の構成](xplat-cli.md) Azure アカウントとサブスクリプションを選択する時点までの指示に従います。
2. 実行、 **azure config モード** コマンドを次に示すように、リソース マネージャー モードに切り替えます。

        azure config mode arm

    上記のコマンドで想定される出力を次に示します。

        info:    New mode is arm

3. 移動し、ブラウザーから **https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-natrules/azuredeploy.parameters.json**, 、json ファイルの内容をコピーし、コンピューターに新しいファイルに貼り付けます。 このシナリオではあるをコピーしてより小さい値という名前のファイル **c:\lb\azuredeploy.parameters.json**します。
4. 実行、 **azure グループの展開を作成** テンプレートとパラメーターを使用して新しいロード バランサーを展開するコマンドレットではファイルをダウンロードし、上の変更します。 出力の後に表示されるリストは、使用されたパラメーターについての説明です。

        azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-natrules/azuredeploy.json' -e 'c:\lb\azuredeploy.parameters.json'

## 次のステップ

[内部ロード バランサーの構成の開始](load-balancer-internal-getstarted.md)

[ロード バランサー分散モードの構成](load-balancer-distribution-mode.md)

[ロード バランサーのアイドル TCP タイムアウト設定の構成](load-balancer-tcp-idle-timeout.md)

