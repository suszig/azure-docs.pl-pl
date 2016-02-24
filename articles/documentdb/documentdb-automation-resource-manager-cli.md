<properties
    pageTitle="DocumentDB Automation - リソース マネージャー - CLI | Microsoft Azure"
    description="Azure リソース マネージャーのテンプレートまたは CLI を使用し、DocumentDB データベース アカウントをデプロイします。 DocumentDB は、JSON データ用のクラウドベースの NoSQL データベースです。"
    services="documentdb"
    authors="mimig1"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager"
    documentationCenter=""/>


<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/03/2015" 
    ms.author="mimig"/>

# Azure リソース マネージャーのテンプレートと Azure CLI を利用し、DocumentDB アカウントを自動作成する

> [AZURE.SELECTOR]
- [Azure ポータル](documentdb-create-account.md)
- [Azure CLI および ARM](documentdb-automation-resource-manager-cli.md)

この記事では、Azure リソース マネージャーのテンプレートと Azure CLI (コマンドライン インターフェイス) を利用し、DocumentDB データベース アカウントを自動作成する方法について説明します。 Azure ポータルを使用して DocumentDB アカウントを作成するを参照してください。 [Azure ポータルを使用して DocumentDB データベース アカウントの作成](documentdb-create-account.md)します。

- [CLI を利用して DocumentDB アカウントを作成する](#quick-create-documentdb-account)
- [ARM テンプレートを利用して DocumentDB アカウントを作成する](#deploy-documentdb-from-a-template)

現在のところ、DocumentDB データベース アカウントは、ARM テンプレートと Azure CLI で作成できる唯一の DocumentDB リソースです。

## 準備

Azure リソース グループで Azure CLI を使用するには、適切な Azure CLI のバージョンと Azure アカウントを用意する必要があります。 Azure CLI を持っていない場合 [インストール](../xplat-cli-install.md)します。

### Azure CLI のバージョンを更新する

コマンド プロンプトで、「`azure --version`」と入力し、バージョン 0.9.11 以降が既にインストールされているかどうかを確認します。

    azure --version
    0.9.11 (node: 0.12.7)

バージョンが 0.9.11 か、後で、いずれかにする必要があります [Azure CLI をインストール](../xplat-cli-install.md) またはのネイティブ インストーラーを使用して、更新、または **npm** 」と入力して `npm update -g azure-cli` を更新または `npm install -g azure-cli` をインストールします。

### Azure アカウントとサブスクリプションを設定する

Azure サブスクリプションをまだ持っていないが、Visual Studio のサブスクリプションが、アクティブにできます、 [Visual Studio のサブスクライバーの特典](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)します。 サインアップできるか、 [無料評価版](http://azure.microsoft.com/pricing/free-trial/)します。

Azure リソース管理テンプレートを使用するには、職場のアカウント、学校のアカウント、Microsoft アカウントのいずれかの ID を所有している必要があります。 いずれかのアカウントを所有している場合、次のコマンドを入力します。

    azure login

次の出力が生成されます。 

    info:    Executing command login
    |info:    To sign in, use a web browser to open the page https://aka.ms/devicelogin. 
    Enter the code E1A2B3C4D to authenticate. If you're signing in as an Azure
    AD application, use the --username and --password parameters.

> [AZURE.NOTE] Azure アカウントを取得していない場合、別の種類のアカウントが必要があることを示すエラー メッセージが表示されます。 現在の Azure アカウントから 1 つを作成するを参照してください。 [Azure Active Directory の職場または学校の id を作成する.](../resource-group-create-work-id-from-personal.md)します。

開いている [https://aka.ms/devicelogin](https://aka.ms/devicelogin) ブラウザーで、コマンドの出力で提供されるコードを入力します。

![Microsoft Azure CLI のデバイス ログイン画面のスクリーンショット](media/documentdb-automation-resource-manager-cli/azure-cli-login-code.png)

コードを入力したら、ブラウザーで使用する ID を選択し、必要に応じてユーザー名とパスワードを入力します。

![使用する Azure サブスクリプションに関連付けられている Microsoft ID アカウントを選択する場所のスクリーンショット](media/documentdb-automation-resource-manager-cli/identity-cli-login.png)

ログインすると、次の確認画面が表示されます。表示されたら、ブラウザーのウィンドウを閉じることができます。

![Microsoft Azure クロスプラットフォーム コマンド ライン インターフェイスへのログインを確認するスクリーンショット](media/documentdb-automation-resource-manager-cli/login-confirmation.png)

コマンド シェルでは、次の出力も提供します。

    -info:    Added subscription Visual Studio Ultimate with MSDN
    +
    info:    login command OK

ここで説明する対話式のログイン方法以外に、Azure CLi でログインする方法もあります。 詳細については、他のメソッドと複数のサブスクリプションの処理の詳細については、次を参照してください。 [Azure サブスクリプションへの接続から、Azure コマンド ライン インターフェイス (Azure CLI)](../xplat-cli-connect.md)します。

### Azure CLI リソース グループ モードに切り替える

既定では、Azure CLI はサービス管理モードで起動 (**asm** モード)。 以下を入力してリソース グループ モードに切り替えます。

    azure config mode arm

次の出力が生成されます。

    info:    New mode is arm

`azure config mode asm` と入力することで、既定のコマンド セットに戻すことができます。

## <a id="quick-create-documentdb-account"></a>タスク: Azure CLI を使用して DocumentDB アカウントを作成します。

このセクションの指示に従い、Azure CLI で DocumentDB アカウントを作成します。 

### 手順 1: リソース グループを作成または取得する

DocumentDB アカウントを作成するには、最初にリソース グループを用意する必要があります。 使用するには、リソース グループの名がわかっている場合にスキップ [手順 2.](#create-documentdb-account-cli)します。 

現在のすべてのリソース グループの一覧を確認するには、次のコマンドを実行し、使用するリソース グループの名前をメモします。 

    azure group list

新しいリソース グループを作成するには、作成する新しいリソース グループの名前とリソース グループを作成するリージョンを指定します。 

    azure group create <resourcegroupname> <resourcegrouplocation>

 - `<resourcegroupname>` に使用できる文字は、英数字、ピリオド、アンダースコア、「-」、かっこのみです。末尾にピリオドを指定することはできません。 
 - `<resourcegrouplocation>` には、DocumentDB が一般公開されているリージョンのいずれかを指定します。 現在の地域の一覧が提供される、 [Azure リージョン ページ](https://azure.microsoft.com/regions/#services)します。

入力例:

    azure group create new_res_group westus

次の出力が生成されます。

    info:    Executing command group create
    + Getting resource group new_res_group
    + Creating resource group new_res_group
    info:    Created resource group new_res_group
    data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/new_res_group
    data:    Name:                new_res_group
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

エラーが発生した場合は、次を参照してください。 [トラブルシューティング](#troubleshooting)します。 

### <a id="create-documentdb-account-cli"></a> 手順 2: CLI を使用して DocumentDB アカウントを作成します。

コマンド プロンプトで次のコマンドを入力し、新規または既存のリソース グループで DocumentDB アカウントを作成します。

> [AZURE.TIP] Azure PowerShell または Windows PowerShell でこのコマンドを実行する場合は、予期しないトークンに関するエラーを受け取ります。 代わりに、Windows PowerShell コマンド プロンプトでこのコマンドを実行します。 

    azure resource create -g <resourcegroupname> -n <databaseaccountname> -r "Microsoft.DocumentDB/databaseAccounts" -o "2015-04-08" -l <databaseaccountlocation> -p "{\"databaseAccountOfferType\":\"Standard\"}" 

 - `<resourcegroupname>` に使用できる文字は、英数字、ピリオド、アンダースコア、「-」、かっこのみです。末尾にピリオドを指定することはできません。 
 - `<databaseaccountname>` に使用できる文字は、英小文字、数字、「-」のみです。文字数は 3 ～ 50 文字にする必要があります。
 - `<databaseaccountlocation>` には、DocumentDB が一般公開されているリージョンのいずれかを指定します。 現在の地域の一覧が提供される、 [Azure リージョン ページ](https://azure.microsoft.com/regions/#services)します。

入力例: 

    azure resource create -g new_res_group -n samplecliacct -r "Microsoft.DocumentDB/databaseAccounts" -o 2015-04-08  -l westus -p "{\"databaseAccountOfferType\":\"Standard\"}"

新しいアカウントがプロビジョニングされるとき、次の出力が生成されます。

    info:    Executing command resource create
    + Getting resource samplecliacct
    + Creating resource samplecliacct
    info:    Resource samplecliacct is updated
    data:
    data:    Id:        /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/new_res_group/providers/Microsoft.DocumentDB/databaseAccounts/samplecliacct
    data:    Name:      samplecliacct
    data:    Type:      Microsoft.DocumentDB/databaseAccounts
    data:    Parent:
    data:    Location:  West US
    data:    Tags:
    data:
    info:    resource create command OK

エラーが発生した場合は、次を参照してください。 [トラブルシューティング](#troubleshooting)します。 

アカウントになりますコマンドが返された後に、 **作成** に変更する前に、数分間は、状態、 **オンライン** の状態では使用可能な状態です。 内のアカウントの状態を確認することができます、 [Azure ポータル](https://portal.azure.com), の **DocumentDB アカウント** ブレードです。

## <a id="deploy-documentdb-from-a-template"></a>タスク: ARM テンプレートを使用して DocumentDB アカウントを作成します。

このセクションの指示に従い、Azure リソース マネージャー (ARM) のテンプレートとオプションのパラメーター ファイル (いずれも JSON ファイル) で DocumentDB アカウントを作成します。 テンプレートを利用すると、誤りなく必要なものを正確に表現し、それを繰り返すことができます。 

### ARM テンプレートとリソース グループについて

大部分のアプリケーションは、異なる種類のリソースの組み合わせ (1 つ以上の DocumentDB アカウント、ストレージ アカウント、仮想ネットワーク、コンテンツ配信ネットワークなど) から構築されます。 既定の Azure サービス管理 API と Azure ポータルでは、サービス単位のアプローチを使用してこれらの項目を表していました。 この方法では、個々のサービスを 1 つの論理的なデプロイ単位としてではなく、個別にデプロイ、管理 (またはこのことを実行するその他のツールを検索) する必要があります。

*Azure リソース マネージャー テンプレート* 展開および宣言型の方法の 1 つの論理的なデプロイ単位としてこれらの異なるリソースを管理するは可能になります。 何をデプロイするのかを Azure に 1 コマンドずつ命令するのではなく、JSON ファイル内にデプロイメント全体、つまりすべてのリソースと、関連する構成およびデプロイメント パラメーターを記述し、Azure にそれらのリソースを 1 つのグループとしてデプロイするよう指示します。

Azure リソース グループとどのような働きをする詳細について学習できます、 [Azure リソース マネージャーの概要](../resource-group-overview.md)します。 テンプレートの作成に知りたい場合は、次を参照してください。 [Azure リソース マネージャーの作成テンプレート](../resource-group-authoring-templates.md)します。

### 手順 1: テンプレートとパラメーター ファイルを作成する

次の内容でローカル テンプレート ファイルを作成します。 azuredeploy.json ファイルに名前を付けます。

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "databaseAccountName": {
                "type": "string"
            }
        },
        "variables": { },
        "resources": [
            {
                "apiVersion": "2015-04-08",
                "type": "Microsoft.DocumentDb/databaseAccounts",
                "name": "[parameters('databaseAccountName')]",
                "location": "[resourceGroup().location]",
                "properties": {
                    "name": "[parameters('databaseAccountName')]",
                    "databaseAccountOfferType": "Standard"
                }
            }
        ]
    }

このテンプレートはパラメーターを 1 つだけ必要とします。作成するデータベース アカウントの名前です。 新しいデータベース アカウントの場所はリソース グループと同じ場所に設定されます。

テンプレートに必要なパラメーターは 1 つだけであるため、コマンド ラインで値を入力するか、パラメーター ファイルを作成して値を指定できます。 

パラメーター ファイルを作成するには、新しいファイルに次の内容をコピーし、azuredeploy.parameters.json ファイルに名前を付けます。 コマンド プロンプトでデータベース アカウント名を指定する場合、このファイルを作成せずに続行できます。

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "databaseAccountName": {
                "value": "samplearmacct"
            }
        }
    }

azuredeploy.parameters.json ファイルで、「samplearmacct」という値を使用するデータベース名に変更し、ファイルを保存します。 `<databaseAccountName>` に使用できる文字は、英小文字、数字、「-」のみです。文字数は 3 ～ 50 文字にする必要があります。

### 手順 2: リソース グループを作成または取得する

DocumentDB アカウントを作成するには、最初にリソース グループを用意する必要があります。 使用するには、リソース グループの名前を既に把握している場合、場所があることを確認、 [DocumentDB が一般に利用可能なリージョン](https://azure.microsoft.com/regions/#services), にスキップ [手順 3](#create-account-from-template)します。 テンプレートでは、アカウントの場所はリソース グループと同じリージョンに作成されるので、DocumentDB を使用できないリージョンにアカウントを作成しようとすると、デプロイメント エラーが発生します。

現在のすべてのリソース グループの一覧を確認するには、次のコマンドを実行し、使用するリソース グループの名前をメモします。 

    azure group list

新しいリソース グループを作成するには、作成する新しいリソース グループの名前とリソース グループを作成するリージョンを指定します。 

    azure group create <resourcegroupname> <databaseaccountlocation>

 - `<resourcegroupname>` に使用できる文字は、英数字、ピリオド、アンダースコア、「-」、かっこのみです。末尾にピリオドを指定することはできません。 
 - `<databaseaccountlocation>` には、DocumentDB が一般公開されているリージョンのいずれかを指定します。 現在の地域の一覧が提供される、 [Azure リージョン ページ](https://azure.microsoft.com/regions/#services)します。

入力例:

    azure group create new_res_group westus

次の出力が生成されます。

    info:    Executing command group create
    + Getting resource group new_res_group
    + Creating resource group new_res_group
    info:    Created resource group new_res_group
    data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/new_res_group
    data:    Name:                new_res_group
    data:    Location:            West US
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

エラーが発生した場合は、次を参照してください。 [トラブルシューティング](#troubleshooting)します。 

### <a id="create-account-from-template"></a>手順 3: ARM テンプレートを使用して DocumentDB アカウントを作成します。

リソース グループで DocumentDB アカウントを作成するには、次のコマンドを実行し、テンプレート ファイルのパス、パラメーター ファイルまたはパラメーター値のパス、デプロイメントするリソース グループの名前、デプロイメント名を指定します (-n は任意です)。 

パラメーター ファイルを使用するには:

    azure group deployment create -f <PathToTemplate> -e <PathToParameterFile> -g <resourcegroupname> -n <deploymentname>

 - `<PathToTemplate>` は、手順 1 で作成した azuredeploy.json ファイルのパスです。
 - `<PathToParameterFile>` は、手順 1 で作成した azuredeploy.parameters.json ファイルのパスです。
 - `<resourcegroupname>` は、DocumentDB データベース アカウントを追加する既存のリソース グループ名です。 
 - `<deploymentname>` は、デプロイメント名 (省略可能) です。

入力例: 

    azure group deployment create -f azuredeploy.json -e azuredeploy.parameters.json -g new_res_group -n azuredeploy

あるいは、パラメーター ファイルなしでデータベース アカウント名パラメーターを指定し、代わりに値の入力を求めさせるには、次のコマンドを実行します。

    azure group deployment create -f <PathToTemplate> -g <resourcegroupname> -n <deploymentname>

New\_db_acct という名前の例の入力プロンプトとデータベース アカウントのエントリを示しています。

    azure group deployment create -f azuredeploy.json -g new_res_group -n azuredeploy
    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    databaseAccountName: samplearmacct

アカウントがプロビジョニングされると、次の情報を受け取ります。 

    info:    Executing command group deployment create
    + Initializing template configurations and parameters
    + Creating a deployment
    info:    Created template deployment "azuredeploy"
    + Waiting for deployment to complete
    data:    DeploymentName     : azuredeploy
    data:    ResourceGroupName  : new_res_group
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          : 2015-11-30T18:50:23.6300288Z
    data:    Mode               : Incremental
    data:    Name                 Type    Value
    data:    -------------------  ------  ------------------
    data:    databaseAccountName  String  samplearmacct
    data:    location             String  West US
    info:    group deployment create command OK

エラーが発生した場合は、次を参照してください。 [トラブルシューティング](#troubleshooting)します。  

アカウントになりますコマンドが返された後に、 **作成** に変更する前に、数分間は、状態、 **オンライン** の状態では使用可能な状態です。 内のアカウントの状態を確認することができます、 [Azure ポータル](https://portal.azure.com), の **DocumentDB アカウント** ブレードです。

## トラブルシューティング

リソース グループまたはデータベース アカウントの作成中に `Deployment provisioning state was not successful` のようなエラーが発生する場合、いくつかのトラブルシューティング方法があります。 

> [AZURE.NOTE] データベース アカウント名に不正な文字を提供することや、DocumentDB いない使用可能な場所を提供するには、配置エラーが発生します。 データベース アカウント名に使用できる文字は、英小文字、数字、「-」のみです。文字数は 3 ～ 50 文字にする必要があります。 すべての有効なデータベース アカウントの場所が一覧表示、 [Azure リージョン ページ](https://azure.microsoft.com/regions/#services)します。

- 出力に次の `Error information has been recorded to C:\Users\wendy\.azure\azure.err` が含まれている場合は、azure.err ファイルでエラー情報を確認してください。

- このログ ファイルには、リソース グループの役立つ情報が記録されている場合があります。 ログ ファイルを表示するには、次のコマンドを実行します。

        azure group log show <resourcegroupname> --last-deployment

    入力例:

        azure group log show new_res_group --last-deployment

    すると、 [Azure でのリソース グループ デプロイのトラブルシューティング](../resource-group-deploy-debug.md) の詳細。

- 次のスクリーンショットのように、エラー情報は Azure ポータルでも確認できます。 エラー情報に移動するには: ジャンプバーで [リソース グループ] をクリックし、エラーが発生したリソース グループを選択します。次に、[リソース グループ] ブレードの [Essentials] 領域で [前回のデプロイ] の日付をクリックし、[デプロイ履歴] ブレードで失敗したデプロイを選択し、[デプロイ] ブレードで赤い感嘆符が付いた [操作の詳細] 詳細をクリックします。 失敗したデプロイメントの状態メッセージが [操作の詳細] ブレードに表示されます。 

    ![デプロイメント エラー メッセージに移動する方法を示す Azure ポータルのスクリーンショット](media/documentdb-automation-resource-manager-cli/portal-troubleshooting-deploy.png) 

## 次のステップ

DocumentDB アカウントを作成できたら、次の手順として DocumentDB データベースを作成します。 データベースを作成するには、次のいずれかを使用します。

- Azure ポータルに明記されている [Azure ポータルを使用して DocumentDB データベースを作成する](documentdb-create-database.md)です。
- C# .NET のサンプル、 [DatabaseManagement](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples/DatabaseManagement) のプロジェクト、 [azure documentdb の dotnet](https://github.com/Azure/azure-documentdb-net/tree/master/samples/code-samples) GitHub のリポジトリです。
-  [DocumentDB Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)します。 DocumentDB には、.NET、Java、Python、Node.js、および JavaScript API の SDK があります。 

データベースを作成した後にする必要があります。 [1 つまたは複数のコレクションに追加](documentdb-create-collection.md) し、データベースに [ドキュメントの追加](documentdb-view-json-document-explorer.md) 、コレクションにします。 

使用することができます、コレクションにドキュメントを用意した後 [DocumentDB SQL](documentdb-sql-query.md) に [クエリを実行する](documentdb-sql-query.md#executing-queries) を使用してドキュメントに対して、 [クエリ エクスプ ローラー](documentdb-query-collections-query-explorer.md) プレビュー ポータルで、 [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx), 、またはのいずれか、 [Sdk](https://msdn.microsoft.com/library/azure/dn781482.aspx)します。

DocumentDB の詳細については、以下の資料を参照してください。

-   [DocumentDB のラーニング パス](https://azure.microsoft.com/documentation/learning-paths/documentdb/)
-   [DocumentDB のリソース モデルと概念](documentdb-resources.md)

他のテンプレートを使用する、次を参照してください。 [Azure クイック スタート テンプレート](http://azure.microsoft.com/documentation/templates/)します。
