<properties
   pageTitle="開発環境とテスト環境 | Microsoft Azure"
   description="Azure リソース マネージャーのテンプレートを使用して、迅速かつ一貫した方法で開発環境およびテスト環境を作成し、削除する方法について説明します。"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="jimdial"
   manager="carolz"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/08/2015"
   ms.author="jdial"/>

# Microsoft Azure の開発環境とテスト環境

カスタム アプリケーションは一般的に、運用環境へのデプロイの前に、複数の開発環境やテスト環境にデプロイされます。 オンプレミスで環境を構築する場合、環境ごとに各アプリケーションへのコンピューティング リソースの調達または割り当てが行われます。 多くの場合、環境は複数の物理マシンまたは仮想マシンを用いた固有の構成で構築され、手動または複雑な自動化スクリプトでデプロイされます。 多くの場合、展開は、数時間かかるし、環境全体で一貫性のない構成で発生します。

## シナリオ ##

Microsoft Azure で開発環境やテスト環境をプロビジョニングすると、料金は使用したリソースの分のみで済みます。  この記事では、下図に示すように、開発環境とテスト環境の作成、保守、および削除が、Azure リソース マネージャーのテンプレートおよびパラメーター ファイルを使用することで、一貫性を保ちながらどれほど迅速に実行できるようになるかについて説明します。

![シナリオ](./media/solution-dev-test-environments-preview-portal/scenario.png)

上の図には、3 つの開発環境およびテスト環境が示されています。  それぞれに、Web アプリケーションと SQL データベースがあります。  環境ごとにアプリケーションとデータベースの名前が異なっています。  この記事では、テンプレートを使用してすべての環境に同じリソースをデプロイし、一意のパラメーター ファイルを使用して環境ごとに異なるリソース構成を指定する方法について説明します。

Azure リソース マネージャーの概念に慣れていない場合は、確認することをお勧めしますが、 [Azure リソース マネージャーの概要](resource-group-overview.md) この記事を読む前に記事です。

短時間で Azure リソース マネージャー テンプレートの使用経験を積みたい場合は、関連記事に目を通さずに、まず、この記事の手順を記載されているとおりに実行してみてください。 手順を進めるうちに、さまざまな不明点が生じると思います。  手順を一通り実行した後で、それらの手順についていろいろ試したり、関連資料に目を通したりすることで、不明点に対する答えの大部分を見つけることができるでしょう。

## Azure リソースの使用を計画する
アプリケーション設計の概要を用意できたら、次の項目を定義できます。

- アプリケーションに含める Azure リソース。 Azure SQL Database を使用する Azure Web アプリとしてアプリケーションを作成し、デプロイできます。  また、PHP と MySQL、IIS と SQL Server などのコンポーネントを使用して、仮想マシンにアプリケーションをビルドできます。  [Azure App Service、Cloud Services、および仮想マシンの比較]( app-service-web/choose-web-site-cloud-service-vm.md) 記事では、Azure リソースを利用するアプリケーションの場合を判断できます。
- 可用性、セキュリティ、スケールなど、アプリケーションが満たすサービス レベル要件。

## Azure リソース マネージャーのテンプレートを作成する
Azure リソース マネージャー テンプレートでは、アプリケーションが使用する Azure リソースをすべて定義します。 複数のテンプレートがあらかじめ用意されており、Azure プレビュー ポータルに直接デプロイすることも、ダウンロードして変更を加え、アプリケーション コードと共にソース管理システムに保存することもできます。  アプリケーションで使用したいリソースが既存のテンプレートに含まれている可能性があります。 使用できるテンプレートの一覧を記載して、 [Azure クイック スタート テンプレート](https://github.com/Azure/azure-quickstart-templates/) GitHub リポジトリ。 

一覧で表示されます、"[201-web-アプリ-sql のデータベース](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database)"フォルダーです。 カスタム アプリケーションには多くの場合 Web アプリケーションと SQL データベースが含まれるため、本記事ではこれ以降、テンプレートの使い方を理解しやすいように、このテンプレートを例として使用します。 すべてこのテンプレートを作成し、構成、すべてを説明するには、この記事の対象外を使用して、組織内の実際の環境を作成する場合は、必要しますが、ありますを完全に理解しておくを読み取って、 [SQL データベースを web アプリをプロビジョニング](app-service-web/app-service-web-arm-with-sql-database-provision.md) 記事です。

> [AZURE.NOTE] テンプレートを Azure に展開するには"Azure への展開] ボタンをクリックすると、直接、 [SQL データベースを Web アプリをプロビジョニング](http://azure.microsoft.com/documentation/templates/201-web-app-sql-database/) 記事です。  この操作はテンプレートについて詳しく知るのに役立ちますが、テンプレートとパラメーター値の編集やバージョン管理、アプリケーション コードと併せた保存はできません。 本記事のこの手順では、テンプレートとパラメーター値をアプリケーション コードと共に保存してバージョン管理する方法について説明します。

  **手順 1:** の内容を表示、 [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-sql-database/azuredeploy.json) 201-web-アプリ-sql のデータベース フォルダー内のファイルです。 これが Azure リソース マネージャーのテンプレート ファイルです。 表示モードをクリックして、"[Raw](https://github.com/Azure/azure-quickstart-templates/raw/master/201-web-app-sql-database/azuredeploy.json)"ボタンをクリックします。 マウスで、このファイルの内容全体を選択し、"TestApp1 Template.json" という名前のファイルとしてコンピューターに保存します。 

テンプレート ファイル内にある "リソース" セクションで、このテンプレートで作成する Azure リソースを定義します。 このテンプレートを作成するその他のリソースの種類間で [Azure Web App](app-service-web/app-service-web-overview.md) と [Azure SQL Database](sql-database/sql-database-technical-overview.md) リソースです。

また、"パラメーター" セクションでは、各リソースで構成できるパラメーターを定義します。 テンプレートで指定するパラメーターの中には、"defaultValue" プロパティを持つものとそうでないものがあります。 テンプレートを使用して Azure リソースをデプロイする場合、defaultValue プロパティを持たないパラメーターにはすべて値を指定する必要があります。  defaultValue プロパティを持つパラメーターは、値が指定されないと、テンプレート内で defaultValue パラメーターに指定した値が使用されます。

テンプレートでは、作成する Azure リソースと、各リソースの構成に使用できるパラメーターを定義します。 一般的には、各開発環境やテスト環境に同じリソースを作成します。 テンプレートと、独自の読み取りをデザインする方法についての詳細、 [Azure リソース マネージャーのテンプレートのデザインのベスト プラクティス](best-practices-resource-manager-design-templates.md) 記事です。

## パラメーター ファイルを作成する

各環境に同じ Azure リソースを作成する必要がありますが、リソースの構成は環境ごとに変えることができます。  このときに役立つのがパラメーター ファイルです。   

  **手順 2:** の内容を表示、 [azuredeploy parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-sql-database/azuredeploy-parameters.json) 201-web-アプリ-sql のデータベース フォルダー内のファイルです。 これは、手順 1 で保存したテンプレート ファイルのパラメーター ファイルです。 表示モードをクリックして、"[Raw](https://github.com/Azure/azure-quickstart-templates/raw/master/201-web-app-sql-database/azuredeploy-parameters.json)"ボタンをクリックします。 マウスで、このファイルの内容全体を選択し、コンピューター上に 3 つのファイルとして保存して、それぞれ次のように名前を設定します。

- TestApp1-Parameters-Development.json
- TestApp1-Parameters-Test.json
- TestApp1-Parameters-Pre-Production.json

  **手順 3:** テキスト エディターまたは JSON エディターを使用してファイルの既存の値を次の値に置き換えて、ステップ 2 で作成した開発およびテスト環境パラメーターのファイルを編集します。

  --TestApp1-Parameters-Development.json--

| パラメーター | 説明 |
|---|---|
| **siteName** | TestApp1Dev |
| **hostingPlanName** | TestApp1PlanDev |
| **siteLocation** | 米国中央部 |
| **serverName** | testapp1dev |
| **serverLocation** | 米国中央部 |
| **administratorLogin** | testapp1Admin |
| **administratorLoginPassword** | #testapp1XYZ |
| **databaseName** | testapp1dev |

--TestApp1-Parameters-Test.json--

| パラメーター | 説明 |
|---|---|
| **siteName** | TestApp1Test |
| **hostingPlanName** | TestApp1PlanTest |
| **siteLocation** | 米国中央部 |
| **serverName** | testapp1test |
| **serverLocation** | 米国中央部 |
| **administratorLogin** | testapp1Admin |
| **administratorLoginPassword** | #testapp1XYZ |
| **databaseName** | testapp1test |

手順 6 で開発環境およびテスト環境の Azure Web アプリと Azure SQL Database のリソースに対する固有の構成を作成する際に、これらのパラメーター ファイルを使用します。

 **手順 4:** 手順 2. で作成した TestApp1 パラメーター Pre Production.json パラメーター ファイルを編集します。  ファイルの内容全体を次の内容に置き換えます。

    {
      "$schema" : "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion" : "1.0.0.0",
      "parameters" : {
        "administratorLogin" : {
          "value" : "testApp1Admin"
        },
        "administratorLoginPassword" : {
          "value" : "testApp1XP49"
        },
        "databaseName" : {
          "value" : "testapp1preprod"
        },
        "hostingPlanName" : {
          "value" : "TestApp1PlanPreProd"
        },
        "serverLocation" : {
          "value" : "Central US"
        },
        "serverName" : {
          "value" : "testapp1preprod"
        },
        "siteLocation" : {
          "value" : "Central US"
        },
        "siteName" : {
          "value" : "TestApp1PreProd"
        },
        "sku" : {
          "value" : "Standard"
        },
        "requestedServiceObjectiveName" : {
          "value" : "S1"
        }
      }
    }

上記の実稼働前パラメーター ファイルで、 **sku** と **requestedServiceObjectiveName** パラメーターが *追加*, 、開発およびテストのパラメーター ファイルに追加されなかった、します。 これは、これらのパラメーターに指定された既定値がテンプレートにあり、開発環境とテスト環境ではその既定値が使用されるのに対して、運用前環境ではこれらのパラメーターに既定以外の値が使用されるためです。

運用前環境でこれらのパラメーターに既定以外の値が使用されるのは、運用環境で望ましいと考えられるパラメーター値をテストするためです。  これらのパラメーターを Azure にどの [ホスティング プランの Web アプリ](http://azure.microsoft.com/pricing/details/app-service/), 、または **sku** と Azure [SQL Database](http://azure.microsoft.com/pricing/details/sql-database/), 、または **requestedServiceObjectiveName** アプリケーションによって使用されています。  SKU やサービス目標名が異なると、コストや機能も異なり、サポートするサービス レベル メトリックも異なります。

次の表に、テンプレートで指定されているこれらのパラメーターの既定値と、運用前用パラメーター ファイルで既定値の代わりに使用される値を一覧表示します。

| パラメーター | 既定値 | パラメーター ファイルの値 |
|---|---|---|
| **sku** | 無料 | Standard |
| **requestedServiceObjectiveName** | S0 | S1 |

## 環境を作成する
内ですべての Azure リソースを作成する必要があります、 [Azure リソース グループ](azure-portal/resource-group-portal.md)します。 リソース グループを使用すると、Azure リソースをグループ化し、まとめて管理できます。  [アクセス許可](resource-group-rbac.md) 、組織内の特定のユーザーできる作成、変更、削除、またはそれらとそこに含まれるリソースを表示するように、リソース グループに割り当てることができます。  アラートおよび課金リソース グループ内のリソースに関する情報を表示できます、 [Azure プレビュー ポータル](https://portal.azure.com)します。 リソース グループは、Azure で作成された [場所](http://azure.microsoft.com/regions/)します。  この記事では、すべてのリソースが米国中部リージョンに作成されます。 実環境の作成を開始する際には、自社の要件に最も合ったリージョンを選択してください。 

  **手順 5:** を使用して、次の方法のいずれかの開発およびテスト環境のリソース グループを作成します。  どちらの方法でも、まったく同じ結果を達成できます。

  **方法 1:** Azure コマンド ライン インターフェイス (CLI)

  CLI があることを確認 [インストール](xplat-cli-install.md) したら、Windows、OS X または Linux コンピューター上 [接続されている](xplat-cli-connect.md) 、Azure サブスクリプションに、組織の ID。 CLI のコマンドラインから次のコマンドを入力し、開発環境用のリソース グループを作成します。

    azure group create "TestApp1-Development" "Central US"

  コマンドが成功すると、次の内容が返されます。

    info:    Executing command group create
    + Getting resource group TestApp1-Development
    + Creating resource group TestApp1-Development
    info:    Created resource group TestApp1-Development
    data:    Id:                  /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development
    data:    Name:                TestApp1-Development
    data:    Location:            centralus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

  テスト環境用のリソース グループを作成するには、次のコマンドを入力します。

    azure group create "TestApp1-Test" "Central US"

  運用前環境用のリソース グループを作成するには、次のコマンドを入力します。

    azure group create "TestApp1-Pre-Production" "Central US"

  **方法 2:** PowerShell

  PowerShell の Windows コンピューターにインストールされているし、サブスクリプションで詳述するように接続されていることを確認、 [をインストールして、Azure PowerShell を構成する方法](powershell-install-configure.md) 記事です。 PowerShell のコマンド プロンプトから次のコマンドを入力し、開発環境用のリソース グループを作成します。 Azure PowerShell 1.0 のプレビューを使用している場合、コマンドは **新規 AzureRmResourceGroup** 次のようにします。 1.0 プレビューする前に Azure PowerShell のバージョンを使用している場合、コマンドは **New-azureresourcegroup**します。

    New-AzureRmResourceGroup -Name TestApp1-Development -Location "Central US"

  コマンドが成功すると、次の内容が返されます。

    WARNING: The output object of this cmdlet will be modified in a future release.


    ResourceGroupName : TestApp1-Development
    Location          : centralus
    ProvisioningState : Succeeded
    Tags              : 
    Permissions       : 
                        Actions  NotActions
                        =======  ==========
                        *                  
                        
    ResourceId        : /subscriptions/uuuuuuuu-vvvv-wwww-xxxx-yyyy-zzzzzzzzzzzz/resourceGroups/TestApp1-Development

  テスト環境用のリソース グループを作成するには、次のコマンドを入力します。

    New-AzureRmResourceGroup -Name TestApp1-Test -Location "Central US"

  運用前環境用のリソース グループを作成するには、次のコマンドを入力します。

    New-AzureRmResourceGroup -Name TestApp1-Pre-Production -Location "Central US"

 **手順 6:** デプロイ テンプレート ファイルを使用して、アプリケーションと、パラメーター ファイルを使用して、次の方法のいずれかの環境ごとに各環境のリソース グループを Azure リソースです。  どちらの方法でも、まったく同じ結果を達成できます。

  **方法 1:** Azure コマンド ライン インターフェイス (CLI)

  CLI のコマンドラインから次のコマンドを入力し、開発環境用に作成したリソース グループにリソースをデプロイします。コマンド内の [path] は、前の手順で保存したファイルへのパスに置き換えてください。

    azure group deployment create -g TestApp1-Development -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Development.json 

  コマンドが成功すると、次の内容が返されます。

    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "Deployment1"
    + Waiting for deployment to complete
    data:    DeploymentName     : Deployment1
    data:    ResourceGroupName  : TestApp1-Development
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : XXXX-XX-XXT20:20:23.5202316Z
    data:    Mode               : Incremental
    data:    Name                           Type          Value
    data:    -----------------------------  ------------  ----------------------------
    data:    siteName                       String        TestApp1Dev
    data:    hostingPlanName                String        TestApp1PlanDev
    data:    siteLocation                   String        Central US
    data:    sku                            String        Free
    data:    workerSize                     String        0
    data:    serverName                     String        testapp1dev
    data:    serverLocation                 String        Central US
    data:    administratorLogin             String        testapp1Admin
    data:    administratorLoginPassword     SecureString  undefined
    data:    databaseName                   String        testapp1dev
    data:    collation                      String        SQL_Latin1_General_CP1_CI_AS
    data:    edition                        String        Standard
    data:    maxSizeBytes                   String        1073741824
    data:    requestedServiceObjectiveName  String        S0
    info:    group deployment create command OKx

  成功しなかった場合は、すべてのエラー メッセージを解決して、再度実行してください。  よくある問題として、Azure リソースの名前付けに関する制約に準拠していないパラメーター値の使用があります。 その他のトラブルシューティングのヒントを参照して、 [Azure でのリソース グループ デプロイのトラブルシューティング](virtual-machines/resource-group-deploy-debug.md) 記事です。

  CLI のコマンドラインから次のコマンドを入力し、テスト環境用に作成したリソース グループにリソースをデプロイします。コマンド内の [path] は、前の手順で保存したファイルへのパスに置き換えてください。

    azure group deployment create -g TestApp1-Test -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Test.json

  CLI のコマンドラインから次のコマンドを入力し、運用前環境用に作成したリソース グループにリソースをデプロイします。コマンド内の [path] は、前の手順で保存したファイルへのパスに置き換えてください。

    azure group deployment create -g TestApp1-Pre-Production -n Deployment1 -f [path]TestApp1-Template.json -e [path]TestApp1-Parameters-Pre-Production.json
  
  **方法 2:** PowerShell

  PowerShell のコマンド プロンプトから次のコマンドを入力し、開発環境用に作成したリソース グループにリソースをデプロイします。コマンド内の [path] は、前の手順で保存したファイルへのパスに置き換えてください。 Azure PowerShell 1.0 のプレビューを使用している場合、コマンドは **新規 AzureRmResourceGroupDeployment** 次のようにします。 1.0 プレビューする前に Azure PowerShell のバージョンを使用している場合、コマンドは **New-azureresourcegroupdeployment**します。

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Development -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Development.json -Name Deployment1 

  コマンドが成功すると、次の内容が返されます。

    DeploymentName    : Deployment1
    ResourceGroupName : TestApp1-Development
    ProvisioningState : Succeeded
    Timestamp         : XX/XX/XXXX 2:44:48 PM
    Mode              : Incremental
    TemplateLink      : 
    Parameters        : 
                        Name             Type                       Value     
                        ===============  =========================  ==========
                        siteName         String                     TestApp1Dev
                        hostingPlanName  String                     TestApp1PlanDev
                        siteLocation     String                     Central US
                        sku              String                     Free      
                        workerSize       String                     0         
                        serverName       String                     testapp1dev
                        serverLocation   String                     Central US
                        administratorLogin  String                     testapp1Admin
                        administratorLoginPassword  SecureString                         
                        databaseName     String                     testapp1dev
                        collation        String                     SQL_Latin1_General_CP1_CI_AS
                        edition          String                     Standard  
                        maxSizeBytes     String                     1073741824
                        requestedServiceObjectiveName  String                     S0        
                        
    Outputs           :

  成功しなかった場合は、すべてのエラー メッセージを解決して、再度実行してください。  よくある問題として、Azure リソースの名前付けに関する制約に準拠していないパラメーター値の使用があります。 その他のトラブルシューティングのヒントを参照して、 [Azure でのリソース グループ デプロイのトラブルシューティング](virtual-machines/resource-group-deploy-debug.md) 記事です。

  PowerShell のコマンド プロンプトから次のコマンドを入力し、テスト環境用に作成したリソース グループにリソースをデプロイします。コマンド内の [path] は、前の手順で保存したファイルへのパスに置き換えてください。

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Test -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Test.json -Name Deployment1

  PowerShell のコマンド プロンプトから次のコマンドを入力し、運用前環境用に作成したリソース グループにリソースをデプロイします。コマンド内の [path] は、前の手順で保存したファイルへのパスに置き換えてください。

    New-AzureRmResourceGroupDeployment -ResourceGroupName TestApp1-Pre-Production -TemplateFile [path]TestApp1-Template.json -TemplateParameterFile [path]TestApp1-Parameters-Pre-Production.json -Name Deployment1

テンプレートおよびパラメーター ファイルは、ソース管理システムでバージョン管理され、アプリケーション コードと共に保持されます。  また、上記のコマンドをスクリプト ファイルに保存し、コードと共に保存することもできます。

## 環境を管理する
各環境の Azure リソースの構成は、開発を進める間に、意図的に、または誤って、一貫性のない変更が加えられる場合があります。  これにより、アプリケーションの開発サイクル中に、不要なトラブルシューティングや問題解決が生じることがあります。

  **手順 7:** 環境を変更します。 開いている、 [Azure プレビュー ポータル](https://portal.azure.com) してサイン インして、同じアカウントで前の手順を完了するために使用します。 次の図に示すように、[すべて参照]、[リソース グループ] を順にクリックします (リソース グループを表示するために [参照] ブレードで下へスクロールする必要がある場合があります)。 前の手順のいずれかの方法を使用して作成した 3 つのリソース グループがすべて表示されます。  次に示すように TestApp1-Development リソース グループをクリックします。

  ![ポータル](./media/solution-dev-test-environments-preview-portal/portal1.png)

  TestApp1-Development リソース グループをクリックするとブレードが表示され、前の手順で実施したリソース グループのデプロイメントにおいてテンプレートで作成されたリソースが一覧表示されます。  次に示すように、[TestApp1Dev]、[削除] を順にクリックして TestApp1Dev Web アプリ リソースを削除します。

  ![ポータル](./media/solution-dev-test-environments-preview-portal/portal2.png)

  リソースを削除してよいかどうかを確認するメッセージがポータルに表示されたら、[はい] をクリックします。  リソース グループの内容は、本来あるべき内容と異なるものになりました。 複数のリソース グループから複数のリソースを削除したり、リソースの一部の構成設定を変更したりなど、いろいろ試してみてください。

> [AZURE.NOTE] リソース グループからリソースを削除する、Azure プレビュー ポータルを使用する代わりに、PowerShell を使用する可能性があります [Remove-azureresource](https://msdn.microsoft.com/library/azure/dn757676.aspx) コマンドか、または同じタスクを実行する CLI から [azure リソースの削除] コマンドです。  

  **手順 8:** 手順 6 で使用される同じコマンドを使用してリソース グループに環境を再展開が、"Deployment2"と"Deployment1"を置き換えます 次の図の [概要] セクションに示されているように、テンプレート内のすべてのリソースが再び TestApp1-Development リソース グループに存在しています。 Azure リソース マネージャーのテンプレートを使用して環境をデプロイする利点の 1 つは、いつでも簡単に環境を既知の状態に再デプロイできる点です。

  ![ポータル](./media/solution-dev-test-environments-preview-portal/portal3.png)

  図中の "前回のデプロイメント" の下にあるテキストをクリックすると、ブレードにリソース グループのデプロイ履歴が表示されます。  最初のデプロイでは "Deployment1"、2 回目のデプロイでは "Deployment2" という名前を使用したため、2 つのエントリが表示されています。  いずれかのデプロイメントをクリックすると、ブレードに各デプロイの結果が表示されます。

## 環境を削除する
環境の作成後に Azure リソースを使用しなくなった場合は、使用料金が発生しないように環境を削除できます。  環境の削除は環境の作成よりも簡単です。  前の手順では、環境ごとに個別の Azure リソース グループを作成しました。 リソース グループを削除すると、グループに含まれているリソースもすべて削除されます。 この点を念頭に置いて、次のいずれかの方法を用いて、前にデプロイした環境 (リソース グループ) をグループ内のすべての Azure リソースと共に削除します。

  **手順 9:** を使用して、次の方法のいずれかの環境を削除します。  どちらの方法でも、まったく同じ結果を達成できます。

  **方法 1: Azure CLI**

  CLI プロンプトで、次のように入力します。

    azure group delete "TestApp1-Development"

  メッセージが表示されたときに "y" を入力すると、次の内容が返されます。

    info:    Executing command group delete
    Delete resource group TestApp1-Development? [y/n] y
    + Deleting resource group TestApp1-Development
    info:    group delete command OK

  CLI プロンプトで、次のように入力して、残りの環境を削除します。

    azure group delete "TestApp1-Test"
    azure group delete "TestApp1-Pre-Production"
  
  **方法 2:** PowerShell

  Azure PowerShell 1.0 のプレビューを使用している場合、リソース グループを削除するコマンドは **削除 AzureRmResourceGroup** 次のようにします。 1.0 プレビューする前に Azure PowerShell のバージョンを使用している場合、コマンドは **Remove-azureresourcegroup**します。 PowerShell プロンプトで、次のように入力します。    

    Remove-AzureRmResourceGroup -Name TestApp1-Development

  メッセージが表示されたときに "y" を入力すると、次の内容が返されます。

    Confirm
    Are you sure you want to remove resource group 'TestApp1-Development'
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y

  PowerShell プロンプトで、次のように入力して、残りの環境を削除します。

    Remove-AzureRmResourceGroup -Name TestApp1-Test
    Remove-AzureRmResourceGroup -Name TestApp1-Pre-Production

使用する方法にかかわらず、コマンドの実行が完了すると、リソース グループとそこに含まれるすべてのリソースが存在しなくなり、リソースの料金請求は発生しなくなります。  

使用するアプリケーションの開発中に発生した Azure リソースの使用率の支出を最小限に抑える [Azure Automation](automation/automation-intro.md) したジョブ スケジュールを設定します。

- 1 日の最後に仮想マシンを停止し、1 日の開始時に再起動する。
- 1 日の最後に環境全体を削除し、1 日の開始時に再作成する。
 
ここまでで、開発環境とテスト環境の作成、保守、および削除をどれほど簡単に実行できるかを体験してきました。これらの操作を詳しく知りたい場合は、上記の手順をさらに試してみたり、この記事に含まれている参照資料に目を通したりしてみてください。

## 次のステップ

- [管理制御を委任](role-based-access-control-configure.md) を Microsoft Azure AD グループまたはユーザーを Azure のリソースに対する操作のサブセットを実行することができる特定のロールに割り当てることによって各環境でそれぞれ異なるリソースです。
- [タグの割り当て](resource-group-using-tags.md) それぞれの環境や個々 のリソースのリソース グループにします。 "環境" タグをリソース グループに追加し、その値を環境名に対応するように設定できます。 タグは、課金または管理の目的でリソースを整理する必要がある場合に特に役立ちます。
- 監視アラートとのリソース グループ リソースに対して課金、 [Azure プレビュー ポータル](https://portal.azure.com)します。

## その他のリソース

- [作成し、Visual Studio での Azure リソース マネージャー テンプレートを展開する](http://msdn.microsoft.com/library/azure/Dn872471.aspx) Azure SDK 2.6 がインストールされているとします。
- 使用して、アプリケーションを作成 [Visual Studio Enterprise](https://www.visualstudio.com/products/visual-studio-enterprise-vs), 、[Visual Studio Code](http://www.visualstudio.com/products/code-vs), 、または [Web Matrix](http://www.microsoft.com/web/webmatrix/)します。
- [Web アプリのデプロイ](app-service-web/web-sites-deploy.md) 作成した環境にします。
- 使用 [Visual Studio のリリース管理](http://msdn.microsoft.com/Library/vs/alm/Release/overview) 迅速かつ簡単に、頻繁にリリースを管理、継続的な展開パイプラインを作成します。
- プレビューの招待をリクエスト [Azure 開発/テスト ラボ](http://azure.microsoft.com/campaigns/devtest-lab/)します。 このプレビューでは、テンプレートを使用して開発/テスト ラボ環境を管理し、組織内での使用に対するクォータとポリシーを構成できます。

