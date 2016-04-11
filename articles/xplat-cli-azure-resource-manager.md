<properties
    pageTitle="リソース マネージャーでの Azure CLI | Microsoft Azure"
    description="Mac、Linux、Windows 用の Azure CLI を利用し、リソース グループとして複数のリソースをデプロイします。"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services="azure-resource-manager"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2015"
    ms.author="danlep"/>

# Azure リソース マネージャーでの、Mac、Linux、および Windows 用 Azure CLI の使用

> [AZURE.SELECTOR]
- [Azure CLI](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)



この記事では、Azure リソース マネージャー モードで、Mac、Linux、Windows 用 Azure コマンドライン インターフェイス (CLI) を使用して、Azure リソースを作成し、管理する方法について説明します。

>[AZURE.NOTE] 作成し、コマンドラインでの Azure リソースの管理には、Azure アカウントが必要 ([無料評価版は、ここ](http://azure.microsoft.com/pricing/free-trial/))。 必要がありますも [Azure CLI をインストール](xplat-cli-install.md), 、および [自分のアカウントに関連付けられている Azure のリソースを使用するログオン](xplat-cli-connect.md)します。 これらの操作を完了した場合は、使用する準備が整いました。

## Azure リソース

Azure リソース マネージャーを使用して、作成のグループを管理 _リソース_ (ユーザーによって管理されるバーチャル マシン、データベース サーバー、データベース、または web サイトなどのエンティティ) 1 つの論理単位としてまたは _リソース グループ_します。

利点の 1 つの Azure リソース マネージャーは、Azure のリソースを作成すること、 _宣言_ 方法: 構造体と JSON 内のリソースのデプロイ可能なグループの関係を記述する *テンプレート*します。 テンプレートはパラメーターを特定します。そのパラメーターに値をコマンドの実行時にインラインで入力するか、別の JSON azuredeploy-parameters.json ファイルに保存できます。 これにより、同じテンプレートに異なるパラメーターを設定して、新しいリソースを簡単に作成できます。 たとえば、Web サイトを作成するテンプレートには、サイト名や Web サイトを配置するリージョンなどの共通的な設定を用意できます。

変更するか、グループを作成するテンプレートを使用すると、 _展開_ 作成されると、グループに適用されます。 Azure リソース マネージャーの詳細については、次を参照してください。、 [Azure リソース マネージャーの概要](../resource-group-overview.md)します。

デプロイを作成すると、従来の (サービス管理) デプロイ モデルと同様に、個々のリソースをコマンド ラインで強制的に管理できます。 たとえば、Azure リソース マネージャーの CLI コマンドを使用して開始、停止、またはなどのリソースを削除する [Azure リソース マネージャーの仮想マシン](../virtual-machines/virtual-machines-deploy-rmtemplates-azure-cli.md)します。

## 認証

Azure CLI を使用して Azure リソース マネージャーを操作するには、仕事か学校のアカウント (組織アカウント) か Microsoft アカウント (CLI バージョン 0.9.10 より) を使用して Microsoft Azure で認証する必要があります。 .publishsettings ファイルによってインストールされた証明書の場合、このモードでは認証できません。

Microsoft Azure への認証の詳細については、次を参照してください。 [Azure CLI から Azure サブスクリプションへの接続](xplat-cli-connect.md)します。

>[AZURE.NOTE] 作業を使用するか、学校のアカウント - Azure Active Directory によって管理--される場合は、アクセスと Azure リソースの使用量を管理する azure のロールベースのアクセス制御 (RBAC) を使用することもできます。 詳細については、「 [の管理とリソースへアクセスの監査](resource-group-rbac.md)します。

## Azure リソース マネージャー モードの設定

Azure リソース マネージャー モードは既定で無効になっているため、次のコマンドを使って Azure CLI リソース マネージャー コマンドを有効にしてください。

    azure config mode arm

>[AZURE.NOTE] Azure リソース マネージャー モードと Azure サービス管理モードは互いに排他的です。 つまり、どちらか一方のモードで作成されたリソースは、他方のモードは管理できません。

## 場所を見つける

ほとんどの Azure リソース マネージャー コマンドには、リソースを作成し、検索するための有効な場所が必要です。 次のコマンドを使用して、異なる Azure リソースに利用可能なすべての場所を検索できます。

    azure location list

これには Azure リソースと、「米国西部」や「米国東部」など、その Azure リソースが利用できる Azure リージョンが一覧表示されます。

## リソース グループの作成

リソース グループは、ネットワーク、ストレージ、その他のリソースの論理グループです。 Azure リソース マネージャー モードのほとんどすべてのコマンドにはリソース グループが必要です。 という名前のリソース グループを作成する _testRG_, 、たとえば、次のコマンドを使用します。

    azure group create -n "testRG" -l "West US"

その後、このグループへのリソースの追加を開始し、これを使用して新しい仮想マシンなどのリソースを構成できます。


## リソース グループ テンプレートの利用

### リソース グループ テンプレートの検索と構成

テンプレートを操作するときにするか [独自に作成](resource-group-authoring-templates.md), 、テンプレートは、のいずれかを使用、 [テンプレート ギャラリー](https://azure.microsoft.com/documentation/templates/), 、これでも使用可能な [GitHub](https://github.com/Azure/azure-quickstart-templates)します。

新しいテンプレートの作成はこの記事の範囲を超えて、ので最初に使用してみましょう、 _101 シンプル-vm-from image_ から使用できるテンプレート [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-linux-vm)します。 既定では、これにより新しい仮想ネットワークで Ubuntu 4.04.2-LTS 仮想マシンが 1 つ作成されます。リージョンは米国西部でサブネットが 1 つです。 次のパラメーターをいくつか指定すれば、このテンプレートを使用できます。

* 一意のストレージ アカウント名
* VM の管理者ユーザー名
* パスワード
* VM のドメイン名

>[AZURE.TIP] 次の手順では、Azure CLI を使用した VM テンプレートを使用する方法の 1 つを説明します。 その他の例については、次を参照してください。 [展開 Azure リソース マネージャー テンプレートと Azure CLI を使用して仮想マシンの管理と](../virtual-machines/virtual-machines-deploy-rmtemplates-azure-cli.md)です。

1. ダウンロード ファイル azuredeploy.json およびから azuredeploy.parameters.json [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/101-simple-linux-vm) 、ローカル コンピューター上の作業フォルダーにします。

2. テキスト エディターで、azuredeploy.parameters.json ファイルを開き、環境に適したパラメーター値を入力 (まま、 **ubuntuOSVersion** 値が変更されていない)。

        {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "newStorageAccountName": {
              "value": "MyStorageAccount"
            },
            "adminUsername": {
              "value": "MyUserName"
            },
            "adminPassword": {
              "value": "MyPassword"
            },
            "dnsNameForPublicIP": {
              "value": "MyDomainName"
            },
            "ubuntuOSVersion": {
              "value": "14.04.2-LTS"
            }
          }
        }
    ```
3. After saving the azuredeploy.parameters.json file, use the following command to create a new resource group based on the template. The `-e` option specifies the azuredeploy.parameters.json file that you modified in the previous step. Replace the *testRG* with the group name you wish to use, and *testDeploy* with a deployment name of your choice. The location should be same as the one specified in your template parameter file.

        azure group create "testRG" "West US" -f azuredeploy.json -d "testDeploy" -e azuredeploy.parameters.json

    This command will return OK after the deployment is uploaded, but before the deployment is applied to resources in the group.

4. To check the status of the deployment, use the following command.

        azure group deployment show "testRG" "testDeploy"

    The **ProvisioningState** shows the status of the deployment.

    If your deployment is successful, you will see output similar to the following.

        azure-cli@0.8.0:/# azure group deployment show testRG testDeploy
        info:    Executing command group deployment show
        + Getting deployments
        + Getting deployments
        data:    DeploymentName     : testDeploy
        data:    ResourceGroupName  : testRG
        data:    ProvisioningState  : Running
        data:    Timestamp          : 2015-10-26T16:15:29.5562024Z
        data:    Mode               : Incremental
        data:    Name                   Type          Value
        data:    ---------------------  ------------  ---------------------
        data:    newStorageAccountName  String        MyStorageAccount
        data:    adminUsername          String        MyUserName
        data:    adminPassword          SecureString  undefined
        data:    dnsNameForPublicIP     String        MyDomainName
        data:    ubuntuOSVersion        String        14.04.2-LTS
        info:    group deployment show command OK

    >[AZURE.NOTE] If you realize that your configuration isn't correct, and need to stop a long-running deployment, use the following command.
    >
    > `azure group deployment stop "testRG" "testDeploy"`
    >
    > If you don't provide a deployment name, one is created automatically based on the name of the template file. It is returned as part of the output of the `azure group create` command.

    Now you can SSH to the VM, using the domain name you specified. When connnecting to the VM, you need to use a fully qualified domain name of the form `<domainName>.<region>.cloudapp.azure.com`, such as `MyDomainName.westus.cloudapp.azure.com`.

5. To view the group, use the following command.

        azure group show "testRG"

    This command returns information about the resources in the group. If you have multiple groups, use the `azure group list` command to retrieve a list of group names, and then use `azure group show` to view details of a specific group.

You can also use a template directly from [GitHub](https://github.com/Azure/azure-quickstart-templates), instead of downloading one to your computer. To do this, pass the URL to the azuredeploy.json file for the template in your command by using the **--template-url** option. To get the URL, open azuredeploy.json on GitHub in _raw_ mode, and copy the URL that appears in the browser's address bar. You can then use this URL directly to create a deployment by using a command similar to the following.

    azure group deployment create "testDeploy" -g "testResourceGroup" --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json
You are prompted to enter the necessary template parameters.

> [AZURE.NOTE] It is important to open the JSON template in _raw_ mode. The URL that appears in the browser's address bar is different from the one that appears in regular mode. To open the file in _raw_ mode when viewing the file on GitHub, in the upper-right corner click **Raw**.

## Working with resources

While templates allow you to declare group-wide changes in configuration, sometimes you need to work with just a specific resource. You can do this using the `azure resource` commands.

> [AZURE.NOTE] When using the `azure resource` commands other than the `list` command, you must specify the API version of the resource you are working with using the `-o` parameter. If you are unsure about the API version to use, consult the template file and find the **apiVersion** field for the resource.

1. To list all resources in a group, use the following command.

        azure resource list "testRG"

2. To view an individual resource within the group, use a command like the following.

        azure resource show "testRG" "MyUbuntuVM" Microsoft.Compute/virtualMachines -o "2015-06-15"

    Notice the **Microsoft.Compute/virtualMachines** parameter. This indicates the type of the resource you are requesting information on. If you look at the template file downloaded earlier, you will notice that this same value is used to define the type of the virtual machine resource described in the template.

    This command returns information related to the virtual machine.

3. When viewing details on a resource, it is often useful to use the `--json` parameter. This makes the output more readable as some values are nested structures, or collections. The following example demonstrates returning the results of the **show** command as a JSON document.

        azure resource show "testRG" "MyUbuntuVM" Microsoft.Compute/virtualMachines -o "2015-06-15" --json

    >[AZURE.NOTE] You can save the JSON data to file by using the &gt; character to pipe the output to file. For example:
    >
    > `azure resource show "testRG" "MyUbuntuVM" Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`

4. To delete an existing resource, use a command like the following.

        azure resource delete "testRG" "MyUbuntuVM" Microsoft.Compute/virtualMachines -o "2015-06-15"

## Logging

To view logged information on operations performed on a group, use the `azure group log show` command. By default, this will list the last operation performed on the group. To view all operations, use the optional `--all` parameter. For the last deployment, use `--last-deployment`. For a specific deployment, use `--deployment` and specify the deployment name. The following example returns a log of all operations performed on the group *MyGroup*.

    azure group log show MyGroup --all

## Next steps

* For information on working with Azure Resource Manager using Azure PowerShell, see [Using Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).
* For information on working with Azure Resource Manager from the Azure portal, see [Using resource groups to manage your Azure resources][psrm].

[signuporg]: http://www.windowsazure.com/documentation/articles/sign-up-organization/
[adtenant]: http://technet.microsoft.com/library/jj573650#createAzureTenant
[psrm]: http://go.microsoft.com/fwlink/?LinkId=394760


