<properties
   pageTitle="リソース グループのデプロイメントのトラブルシューティング | Microsoft Azure"
   description="リソース マネージャーのデプロイメント モデルを使用して作成したリソースのデプロイメント時の一般的な問題、およびこれらの問題を検出および修正する方法について説明します。"
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="10/14/2015"
   ms.author="tomfitz;rasquill"/>

# Azure でのリソース グループのデプロイメントのトラブルシューティング

デプロイメント中に問題を発生した場合、その原因を究明する必要があります。 リソース マネージャーでは、発生した問題とその原因を 2 通りの方法で発見できます。 展開コマンドを使用するには、に関する情報を取得するには
リソース グループの特定の展開。 または、リソース グループに対して実行されたすべての操作に関する情報を取得するには、監査ログを使用します。  この情報により、解決するには、
発行し、ソリューション内の操作を再開します。

このトピックでは、デプロイメントのトラブルシューティングを行うための、デプロイメント コマンドの使用に重点を置いて説明します。 監査ログを使用して、リソースに対してあらゆる操作を追跡する方法の詳細については、次を参照してください。 [リソース マネージャーでの操作を監査](../resource-group-audit.md)します。

このトピックでは、Azure PowerShell、Azure CLI および REST API を使用してのトラブルシューティング情報を取得する方法を示します。 プレビュー ポータルを使用した展開のトラブルシューティングを行う方法の詳細については、次を参照してください。 [Azure ポータルを使用して、Azure リソースを管理する](../azure-portal/resource-group-portal.md)です。

ユーザーに表示される一般的なエラーの対処方法もこのトピックで説明しています。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。 クラシック デプロイメント モデルを使用してリソース グループを作成することはできません。


## PowerShell でのトラブルシューティング

[AZURE.INCLUDE [powershell-preview-inline-include](../../includes/powershell-preview-inline-include.md)]

使用した展開の全体的な状態を取得できます、 **Get AzureRmResourceGroupDeployment** コマンドです。 以下の例では、デプロイメントは失敗しています。

    PS C:\> Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -DeploymentName ExampleDeployment

    DeploymentName    : ExampleDeployment
    ResourceGroupName : ExampleGroup
    ProvisioningState : Failed
    Timestamp         : 8/27/2015 8:03:34 PM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    siteName         String                     ExampleSite
                    hostingPlanName  String                     ExamplePlan
                    siteLocation     String                     West US
                    sku              String                     Free
                    workerSize       String                     0

    Outputs           :

通常、各デプロイメントは、それぞれの操作がデプロイメント処理の 1 手順を示す、複数の操作で構成されています。 デプロイメントの問題を検出するには、通常デプロイメント操作に関する詳細を確認する必要があります。 持つ操作の状態を確認できます **Get AzureRmResourceGroupDeploymentOperation**します。

    PS C:\> Get-AzureRmResourceGroupDeploymentOperation -DeploymentName ExampleDeployment -ResourceGroupName ExampleGroup
    Id                        OperationId          Properties         
    -----------               ----------           -------------
    /subscriptions/xxxxx...   347A111792B648D8     @{ProvisioningState=Failed; Timestam...
    /subscriptions/xxxxx...   699776735EFC3D15     @{ProvisioningState=Succeeded; Times...

ここではデプロイメントの 2 つの操作が示されています。 1 つのプロビジョニングの状態は Failed になっており他は Succeeded になっています。

状態のメッセージは、次のコマンドを使用して取得できます。

    PS C:\> (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName ExampleDeployment -ResourceGroupName ExampleGroup).Properties.StatusMessage

    Code       : Conflict
    Message    : Website with given name mysite already exists.
    Target     :
    Details    : {@{Message=Website with given name mysite already exists.}, @{Code=Conflict}, @{ErrorEntity=}}
    Innererror :

## Azure CLI を使用したトラブルシューティング

使用した展開の全体的な状態を取得できます、 **azure グループ展開表示** コマンドです。 以下の例では、デプロイメントは失敗しています。

    azure group deployment show ExampleGroup ExampleDeployment

    info:    Executing command group deployment show
    + Getting deployments
    data:    DeploymentName     : ExampleDeployment
    data:    ResourceGroupName  : ExampleGroup
    data:    ProvisioningState  : Failed
    data:    Timestamp          : 2015-08-27T20:03:34.9178576Z
    data:    Mode               : Incremental
    data:    Name             Type    Value
    data:    ---------------  ------  ------------
    data:    siteName         String  ExampleSite
    data:    hostingPlanName  String  ExamplePlan
    data:    siteLocation     String  West US
    data:    sku              String  Free
    data:    workerSize       String  0
    info:    group deployment show command OK


デプロイメントが失敗した理由の詳細は、監査ログで調べることができます。 監査ログを表示するには、実行、 **azure グループ ログ表示** コマンドです。 含めることができます、 **--last-deployment** の最新のデプロイのログのみを取得するにはオプションです。

    azure group log show ExampleGroup --last-deployment

 **Azure グループ ログ表示** コマンドは、多くの情報を返すことができます。 通常、トラブルシューティングを行う場合は、失敗した操作に重点的に取り組みます。 次のスクリプトを使用して、 **-json** オプションと **jq** の展開エラーのログを検索します。 などのツールについて学習する **jq**, を参照してください [Azure とやり取りする便利なツール](#useful-tools-to-interact-with-azure)

    azure group log show ExampleGroup --json | jq '.[] | select(.status.value == "Failed")'

    {
      "claims": {
        "aud": "https://management.core.windows.net/",
        "iss": "https://sts.windows.net/<guid>/",
        "iat": "1442510510",
        "nbf": "1442510510",
        "exp": "1442514410",
        "ver": "1.0",
        "http://schemas.microsoft.com/identity/claims/tenantid": "<guid>",
        "http://schemas.microsoft.com/identity/claims/objectidentifier": "<guid>",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress": "someone@example.com",
        "puid": "XXXXXXXXXXXXXXXX",
        "http://schemas.microsoft.com/identity/claims/identityprovider": "example.com",

        "altsecid": "1:example.com:XXXXXXXXXXX",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "<hash string>",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "Tom",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "FitzMacken",
        "name": "Tom FitzMacken",
        "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
        "groups": "<guid>",
        "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "example.com#someone@example.com",
        "wids": "<guid>",
        "appid": "<guid>",
        "appidacr": "0",
        "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
        "http://schemas.microsoft.com/claims/authnclassreference": "1",
        "ipaddr": "000.000.000.000"
      },
      "properties": {
        "statusCode": "Conflict",
        "statusMessage": "{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"Target\":null,\"Details\":[{\"Message\":\"Website with given name
          mysite already exists.\"},{\"Code\":\"Conflict\"},{\"ErrorEntity\":{\"Code\":\"Conflict\",\"Message\":\"Website with given name mysite already exists.\",\"ExtendedCode\":
          \"54001\",\"MessageTemplate\":\"Website with given name {0} already exists.\",\"Parameters\":[\"mysite\"],\"InnerErrors\":null}}],\"Innererror\":null}"
      },
    ...

ご覧 **プロパティ** 失敗した操作は json での情報が含まれています。

使用することができます、 **-詳細** と **-vv** オプションの詳細については、ログを参照してください。  使用して、 **-詳細** 、操作の手順を表示するオプション `stdout`します。 完全な要求の履歴を使用して、 **-vv** オプション。 多くの場合、メッセージはエラーの原因に関する重要な手掛かりを提供します。

## REST API を使用したトラブルシューティング

リソース マネージャーの REST API は、デプロイメントに関する情報、デプロイメントの操作、特定の操作の詳細に関する情報を取得するための URI を提供します。 これらのコマンドの完全な説明については、次を参照してください。

- [テンプレートのデプロイメントに関する情報の取得](https://msdn.microsoft.com/library/azure/dn790565.aspx)
- [すべてのテンプレート デプロイメント操作の一覧を取得する](https://msdn.microsoft.com/library/azure/dn790518.aspx)
- [テンプレート デプロイメント操作に関する情報の取得](https://msdn.microsoft.com/library/azure/dn790519.aspx)


## 期限切れの資格情報の更新

Azure の資格情報が期限切れの場合や Azure アカウントにサインインしていない場合、デプロイメントは失敗します。 セッションが長時間開かれている場合、資格情報の期限が切れる場合があります。 資格情報は、次のオプションを使用して更新できます。

- PowerShell を使用して、 **ログイン AzureRmAccount** コマンドレット (または **Add-azureaccount** 1.0 プレビューする前に PowerShell のバージョン)。 発行設定ファイルの資格情報は、AzureResourceManager モジュールのコマンドレットには十分ではありません。
- Azure CLI を使用して **azure ログイン**します。 のヘルプを、認証エラーがあることを確認します [Azure CLI を正しく構成されている](../xplat-cli-connect.md)します。

## テンプレートおよびパラメーターの形式のチェック

テンプレートまたはパラメーター ファイルの形式が不正な場合、デプロイメントは失敗します。 デプロイメントを実行する前に、テンプレートとパラメーターの有効性をテストできます。

### PowerShell

PowerShell を使用して **テスト AzureRmResourceGroupDeployment** (または **Test-azureresourcegrouptemplate** 1.0 プレビューする前に PowerShell のバージョン)。

    PS C:\> Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile c:\Azure\Templates\azuredeploy.json -TemplateParameterFile c:\Azure\Templates\azuredeploy.parameters.json
    VERBOSE: 12:55:32 PM - Template is valid.

### Azure CLI

Azure CLI を使用して **azure グループ テンプレートの検証 <resource group>**

次の例では、テンプレートと、必要なパラメーターを検証する方法を示します。 Azure CLI が、必要なパラメーター値を要求します。

        azure group template validate \
        > --template-uri "https://contoso.com/templates/azuredeploy.json" \
        > resource-group
        info:    Executing command group template validate
        info:    Supply values for the following parameters
        adminUserName: UserName
        adminPassword: PassWord
        + Initializing template configurations and parameters
        + Validating the template
        info:    group template validate command OK

### REST API

REST API を参照してください。 [テンプレート デプロイの検証](https://msdn.microsoft.com/library/azure/dn790547.aspx)します。

## リソースをサポートする場所の確認

リソースの場所を指定するには、リソースをサポートする場所のいずれかを使用する必要があります。 リソースの場所を入力する前に、次のいずれかのコマンドを使用して、その場所がリソースの種類をサポートしていることを確認します。

### PowerShell

1.0 プレビューする前に PowerShell のバージョンは、リソースおよび使用し、サイトの完全な一覧を表示、 **Get-azurelocation** コマンドです。

    PS C:\> Get-AzureLocation

    Name                                    Locations                               LocationsString
    ----                                    ---------                               ---------------
    ResourceGroup                           {East Asia, South East Asia, East US... East Asia, South East Asia, East US,...
    Microsoft.ApiManagement/service         {Central US, East US, East US 2, Nor... Central US, East US, East US 2, Nort...
    Microsoft.AppService/apiapps            {East US, West US, South Central US,... East US, West US, South Central US, ...
    ...

リソースの特定の種類を指定するには、次を使用します。

    PS C:\> Get-AzureLocation | Where-Object Name -eq "Microsoft.Compute/virtualMachines" | Format-Table Name, LocationsString -Wrap

    Name                                                        LocationsString
    ----                                                        ---------------
    Microsoft.Compute/virtualMachines                           East US, East US 2, West US, Central US, South Central US,
                                                                North Europe, West Europe, East Asia, Southeast Asia,
                                                                Japan East, Japan West

PowerShell 1.0 のプレビューを使用して **Get AzureRmResourceProvider** サポートされている場所を取得します。

    PS C:\> Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web

    ProviderNamespace RegistrationState ResourceTypes               Locations
    ----------------- ----------------- -------------               ---------
    Microsoft.Web     Registered        {sites/extensions}          {Brazil South, ...
    Microsoft.Web     Registered        {sites/slots/extensions}    {Brazil South, ...
    Microsoft.Web     Registered        {sites/instances}           {Brazil South, ...
    ...

リソースの特定の種類を指定するには、次を使用します。

    PS C:\> ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations

    Brazil South
    East Asia
    East US
    Japan East
    Japan West
    North Central US
    North Europe
    South Central US
    West Europe
    West US
    Southeast Asia

### Azure CLI

Azure CLI を使用できます **azure の場所のリスト**します。 場所の一覧は長くなることがあり、多数のプロバイダーがあるため、ツールを使用して、まだ使用可能でない場所を使用する前に、プロバイダーと場所を確認します。 次のスクリプトを使用して **jq** Azure virtual machines のリソース プロバイダーが使用可能な場所を検出します。

    azure location list --json | jq '.[] | select(.name == "Microsoft.Compute/virtualMachines")'
    {
      "name": "Microsoft.Compute/virtualMachines",
      "location": "East US,East US 2,West US,Central US,South Central US,North Europe,West Europe,East Asia,Southeast Asia,Japan East,Japan West"
    }

### REST API

REST API を参照してください。 [リソース プロバイダーに関する情報を取得](https://msdn.microsoft.com/library/azure/dn790534.aspx)します。

## 一意のリソース名の作成

一部のリソース (特にストレージ アカウント、データベース サーバー、Web サイト) には、Azure 全体で一意となるリソース名を指定する必要があります。 現時点では、名前が一意であることをテストする方法はありません。 他の組織が使用する可能性がない名前付け規則を使用することをお勧めします。

## 認証、サブスクリプション、ロール、クォータの問題

認証と承認および Azure Active Directory に関連する、デプロイメントの成功を妨げる 1 つ以上の問題があることがあります。 Azure リソース グループを管理する方法に関係なく、アカウントにサインインするために使用する ID は Azure Active Directory オブジェクトである必要があります。 この ID は作成した、あるいは割り当てられた会社または学校アカウントにすることができます。または、アプリケーション用のサービス プリンシパルを作成できます。

しかし、Azure Active Directory では、ユーザーまたはユーザーの管理者が、どの ID がどのリソースにアクセスできるかを高い精度で制御できるようにします。 デプロイメントが失敗する場合は、認証または承認の問題の兆候があるかどうかについて要求自体を調べ、リソース グループのデプロイメントのログを調べます。 自分が一部のリソースに対するアクセス許可を持つ一方、他のリソースに対するアクセス許可のないことがわかる場合があります。 Azure CLI を使用すると、`azure ad` コマンドを使用して、Azure Active Directory テナントおよびユーザーを確認することができます。 (Azure CLI コマンドの一覧については、次を参照してください [。Mac、Linux、および Windows Azure リソース マネージャーでの Azure CLI を使用して](azure-cli-arm-commands.md).)

また、デプロイメントが既定のクォータに達すると、問題が発生する場合があります。既定のクォータは、リソース グループごと、サブスクリプションごと、アカウントごと、その他のスコープごとである可能性があります。 正しくデプロイするために使用可能なリソースがあるかどうかを十分に確認します。 完全なクォータについては、次を参照してください。 [Azure サブスクリプションとサービスの制限、クォータ、および制約](../azure-subscription-service-limits.md)します。

自分のサブスクリプションのコア クォータを調べるを使用してください、 `azure vm list-usage` Azure CLI でコマンド、および **Get AzureVMUsage** PowerShell コマンドレット。 次に Azure CLI のコマンドを示します。また、無料試用版アカウントのコア クォータが 4 であることも示しています。

    azure vm list-usage
    info:    Executing command vm list-usage
    Location: westus
    data:    Name   Unit   CurrentValue  Limit
    data:    -----  -----  ------------  -----
    data:    Cores  Count  0             4
    info:    vm list-usage command OK

上記のサブスクリプションで、米国西部リージョンに 4 つ以上のコアを作成するテンプレートをデプロイメントしようとした場合、 (ポータルでまたはデプロイメント ログを調査して) 次のようなデプロイメント エラーになります。

    statusCode:Conflict
    serviceRequestId:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    statusMessage:{"error":{"code":"OperationNotAllowed","message":"Operation results in exceeding quota limits of Core. Maximum allowed: 4, Current in use: 4, Additional requested: 2."}}

このような場合はポータルに移動し、ファイルをデプロイするリージョンのクォータを増加させるように、サポートに問題を報告してください。

> [AZURE.NOTE] リソース グループのクォータがサブスクリプション全体ではなく、個々 の地域のあることに注意してください。 米国西部に 30 のコアをデプロイする必要がある場合は、米国西部に 30 のリソース マネージャーのコアを要求する必要があります。 アクセスできるリージョンのいずれかで 30 のコアをデプロイする必要がある場合は、すべてのリージョンで 30 のリソース　マネージャー コアを要求する必要があります。
<!-- -->
コアについて具体的には、たとえば、確認できますのリージョンをパイプを使用する次のコマンドを使用して、適切なクォータ量を要求する必要があります **jq** json を解析するためです。
<!-- -->
        azure provider show Microsoft.Compute --json | jq '.resourceTypes[] | select(.name == "virtualMachines") | { name,apiVersions, locations}'
        {
          "name": "virtualMachines",
          "apiVersions": [
            "2015-05-01-preview",
            "2014-12-01-preview"
          ],
          "locations": [
            "East US",
            "West US",
            "West Europe",
            "East Asia",
            "Southeast Asia"
          ]
        }


## リソース プロバイダーの登録の確認

リソースは、リソース プロバイダーによって管理され、特定のプロバイダーを使用できるようアカウントやサブスクリプションが有効になっている場合があります。 プロバイダーを使用できるようになったら、使用登録もする必要があります。 ほとんどのプロバイダーは、Azure ポータルまたはご使用のコマンド ライン インターフェイスによって自動的に登録されますが、登録されない場合もあります。

### PowerShell

リソース プロバイダーと、登録状態の一覧を取得するには使用 **Get AzureProvider** 1.0 プレビューする前に PowerShell のバージョン。

    PS C:\> Get-AzureProvider

    ProviderNamespace                       RegistrationState                       ResourceTypes
    -----------------                       -----------------                       -------------
    Microsoft.AppService                    Registered                              {apiapps, appIdentities, gateways, d...
    Microsoft.Batch                         Registered                              {batchAccounts}
    microsoft.cache                         Registered                              {Redis, checkNameAvailability, opera...
    ...

プロバイダーを登録するには使用 **レジスタ AzureProvider**します。

Powershell 1.0 のプレビューを使用して **Get AzureRmResourceProvider**します。

    PS C:\> Get-AzureRmResourceProvider -ListAvailable

    ProviderNamespace               RegistrationState ResourceTypes
    -----------------               ----------------- -------------
    Microsoft.ApiManagement         Unregistered      {service, validateServiceName, checkServiceNameAvailability}
    Microsoft.AppService            Registered        {apiapps, appIdentities, gateways, deploymenttemplates...}
    Microsoft.Batch                 Registered        {batchAccounts}

プロバイダーを登録するには使用 **レジスタ AzureRmResourceProvider**します。

### Azure CLI

Azure CLI を使用してプロバイダーが登録されているかどうかを確認するには、`azure provider list` コマンドを使用します (切り捨てられた出力の例を次に示します)。

        azure provider list
        info:    Executing command provider list
        + Getting ARM registered providers
        data:    Namespace                        Registered
        data:    -------------------------------  -------------
        data:    Microsoft.Compute                Registered
        data:    Microsoft.Network                Registered  
        data:    Microsoft.Storage                Registered
        data:    microsoft.visualstudio           Registered
        data:    Microsoft.Authorization          Registered
        data:    Microsoft.Automation             NotRegistered
        data:    Microsoft.Backup                 NotRegistered
        data:    Microsoft.BizTalkServices        NotRegistered
        data:    Microsoft.Features               Registered
        data:    Microsoft.Search                 NotRegistered
        data:    Microsoft.ServiceBus             NotRegistered
        data:    Microsoft.Sql                    Registered
        info:    provider list command OK

この場合も、リージョン別の提供状況などのプロバイダーの詳細については、`azure provider list --json` と入力します。 次のコードはリストの先頭にあるのみを選択して表示します。

        azure provider list --json | jq '.[0]'
        {
          "resourceTypes": [
            {
              "apiVersions": [
                "2014-02-14"
              ],
              "locations": [
                "North Central US",
                "East US",
                "West US",
                "North Europe",
                "West Europe",
                "East Asia"
              ],
              "properties": {},
              "name": "service"
            }
          ],
          "id": "/subscriptions/<guid>/providers/Microsoft.ApiManagement",
          "namespace": "Microsoft.ApiManagement",
          "registrationState": "Registered"
        }

プロバイダーは、登録を必要とする場合は使用して、 `azure provider register <namespace>` コマンド、場所、 *名前空間* 値は、上記の一覧から取得します。

### REST API

登録状態を確認するには、「 [リソース プロバイダーに関する情報を取得](https://msdn.microsoft.com/library/azure/dn790534.aspx)します。

プロバイダーを登録するを参照してください。 [リソース プロバイダーとサブスクリプションの登録](https://msdn.microsoft.com/library/azure/dn790548.aspx)します。


## カスタム テンプレートでいつデプロイメントが成功したかを確認する

自分で作成したテンプレートを使用している場合は、すべてのプロバイダーがデプロイメントから正常に返されたときに、Azure リソース マネージャー システムがデプロイメントの成功を報告することを理解しておくことが重要です。 これは、すべてのテンプレート項目が使用法に応じてデプロイされたことを意味します。

ただし、これは必ずしもリソース グループが「アクティブで、ユーザーが使用できる状態」であるとは限りません。 たとえば、ほとんどのデプロイメントでは、アップグレードのダウンロード、他の非テンプレート リソースの待機、または複雑なスクリプトや、Azure が認識していない他の実行可能なアクティビティのインストールをデプロイメントに要求します。これは、アクティビティが、プロバイダーが追跡しているアクティビティではないためです。 このような場合、リソースが実際に使用できる状態になるまで時間がかかります。 その結果、デプロイメントが使用できるまでのある時にデプロイメントが成功の状態になることを予期する必要があります。

ただしの展開の成功をレポートを使用して、カスタム テンプレートのカスタム スクリプトを作成することで、Azure を防ぐことができます、 [CustomScriptExtension](http://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/) たとえば--をシステム全体で準備ができて全体の展開を監視する方法を認識して、ユーザーがデプロイ全体と対話する場合にのみ正常に返されます。 拡張機能が実行を使用して最後であることを確認する場合、 **dependsOn** テンプレートのプロパティです。 例は、表示されるときに [テンプレートの展開を作成する](https://msdn.microsoft.com/library/azure/dn790564.aspx)です。

## Azure とやり取りする便利なツール
コマンドラインから Azure リソースを使用する場合に、作業に役立つツールがあります。 Azure リソース グループのテンプレートは JSON ドキュメントで、Azure リソース マネージャー API は JSON を受信して返します。このため、JSON 解析ツールは、リソースに関する情報を参照したり、テンプレートとテンプレートのパラメーター ファイルを設計し、操作する際に最初に使用するツールの 1 つになります。

### Mac、Linux、Windows の各ツール
Mac、Linux、および Windows 用 Azure コマンド ライン インターフェイスを使用する場合に慣れておそらく標準のダウンロード ツールなど **[curl](http://curl.haxx.se/)** と **[wget](https://www.gnu.org/software/wget/)**, 、または **[Resty](https://github.com/beders/Resty)**, 、やなど、JSON ユーティリティ **[jq](http://stedolan.github.io/jq/download/)**, 、**[jsawk](https://github.com/micha/jsawk)**, 、および JSON を適切に処理する言語ライブラリです。 (これらのツールの多くもポートを持つ、Windows のなど [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)実際、Linux とその他のオープン ソース ソフトウェア ツールを Windows 上でも実行させるいくつかの方法があります。)。

このトピックにはで使用できるいくつかの Azure CLI コマンドが含まれています **jq** をより効率的に情報を正確に取得します。 Azure リソースの使用状況を理解するには、使い慣れているツールセットを選択する必要があります。

### PowerShell

PowerShell には、同じ手順を実行するいくつかの基本的なコマンドがあります。

- 使用して、 **[Invoke-webrequest](https://technet.microsoft.com/library/hh849901%28v=wps.640%29)** コマンドレットをリソース グループ テンプレートやパラメーターの JSON ファイルなどのファイルをダウンロードします。
- 使用して、 **[Convertfrom-json](https://technet.microsoft.com/library/hh849898%28v=wps.640%29.aspx)** JSON 文字列をカスタム オブジェクトに変換するためのコマンドレット ([PSCustomObject](https://msdn.microsoft.com/library/windows/desktop/system.management.automation.pscustomobject%28v=vs.85%29.aspx)) を含む JSON 文字列内の各フィールドのプロパティです。


## 次のステップ

テンプレートの作成をマスターする、 [Azure リソース マネージャーの作成テンプレート](../resource-group-authoring-templates.md), を見てみましょう、 [Azure クイック スタート テンプレート リポジトリ](https://github.com/Azure/azure-quickstart-templates) 展開可能な例についてです。 使用例、 **dependsOn** プロパティは、 [VM を作成する複数の Nic と RDP アクセス](https://github.com/Azure/azure-quickstart-templates/tree/master/201-1-vm-loadbalancer-2-nics)します。

<!--Image references-->

<!--Reference style links - using these makes the source content way more readable than using inline links-->

