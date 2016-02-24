<properties
    pageTitle="Batch Management .NET でのアカウント管理 | Microsoft Azure"
    description="Batch Management .NET ライブラリを使用してアプリケーション内の Azure Batch アカウントを作成、削除、変更します。"
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="11/10/2015"
    ms.author="v-marsma"/>

# Azure Batch アカウントとクォータを Batch Management .NET で管理する

> [AZURE.SELECTOR]
- [Azure ポータル](batch-account-create-portal.md)
- [.NET のバッチ管理](batch-management-dotnet.md)

使用して Azure Batch アプリケーションに管理オーバーヘッドの削減、 [バッチ管理 .NET][api_mgmt_net] バッチ アカウントの作成、削除、キーの管理、およびクォータの検出を自動化するライブラリです。

- **作成し、Batch アカウントを削除** の任意の範囲内です。 たとえば、独立系ソフトウェア ベンダー (ISV) が課金のためにクライアントごとに異なる Batch アカウントを割り当てる場合、顧客ポータルにアカウントの作成および削除機能を追加できます。
- **取得し、アカウント キーを再生成** 、Batch アカウントのいずれかのプログラムを使用しています。 これは、アカウント キーの定期的なロールオーバーまたは期限切れを実施するセキュリティ ポリシーへの準拠を維持する場合に特に便利です。 さまざまな Azure リージョンに複数の Batch アカウントがある場合、このロールオーバー プロセスを自動化するとソリューションの効率が向上します。
- **アカウントのクォータを確認する** 評価版-エラー推測に頼らず、Batch アカウントがどのような制限を持つを特定するとします。 ジョブ開始前にアカウントのクォータをチェックしたり、プールを作成したり、コンピューティング ノードを追加したりすることで、いつどこでコンピューティング リソースが作成されるのかを事前に調整できます。 アカウントに追加リソースを割り当てる前に、クォータの増量が必要なアカウントを特定できます。
- **他の Azure サービスの機能を組み合わせる** 多機能な管理エクスペリエンスをバッチ管理の .NET を活用することで [Azure Active Directory][aad_about], 、および [Azure リソース マネージャー][resman_overview] 、同じアプリケーションにまとめる。 これらの機能とその API を使用して、エンド ツー エンドの管理ソリューションに摩擦のない認証エクスペリエンス、リソース グループの作成と削除、上記の機能を提供できます。

> [AZURE.NOTE] この記事では、プログラムによるバッチ アカウント、キー、およびクォータの管理に重点を置いています、中に行うこれらのアクティビティの多くを使用して、 [Azure ポータル][azure_portal]します。 参照してください [を作成し、Azure ポータルで、Azure Batch アカウントを管理](batch-account-create-portal.md) と [クォータや、Azure Batch サービスの制限](batch-quota-limit.md) の詳細。

## Batch アカウントの作成と削除

前述のように、Batch Management API の主要な機能の 1 つは、Azure リージョン内の Batch アカウントの作成と削除です。 そのために使用する [BatchManagementClient.Accounts.CreateAsync][net_create] と [DeleteAsync][net_delete], 、または対応する同期します。

次のコード例では、アカウントを作成し、新しく作成されたアカウントを Batch サービスから取得し、削除します。 この記事の他のスニペットおよび `batchManagementClient` のインスタンスが完全に初期化された [BatchManagementClient][net_mgmt_client]します。

```
// Create a new Batch account
await batchManagementClient.Accounts.CreateAsync("MyResourceGroup",
    "mynewaccount",
    new BatchAccountCreateParameters() { Location = "West US" });

// Get the new account from the Batch service
BatchAccountGetResponse getResponse = await batchManagementClient.Accounts.GetAsync("MyResourceGroup", "mynewaccount");
AccountResource account = getResponse.Resource;

// Delete the account
await batchManagementClient.Accounts.DeleteAsync("MyResourceGroup", account.Name);
```

> [AZURE.NOTE] バッチ管理 .NET ライブラリおよびその BatchManagementClient を使用するアプリケーションを必要と **サービス管理者** または **共同管理者** を管理する Batch アカウントを所有するサブスクリプションにアクセスします。 参照してください、 [Azure Active Directory](#aad) 以下のセクションと [AccountManagement][acct_mgmt_sample] 詳細については、コード サンプルです。

## アカウント キーの取得と再生成

プライマリとセカンダリのアカウント キーを使用して、サブスクリプション内のすべてのバッチ アカウントから取得 [ListKeysAsync][net_list_keys], 、およびこれらのキーを再生成する [RegenerateKeyAsync][net_regenerate_keys]します。

```
// Get and print the primary and secondary keys
BatchAccountListKeyResponse accountKeys = await batchManagementClient.Accounts.ListKeysAsync("MyResourceGroup", "mybatchaccount");
Console.WriteLine("Primary key:   {0}", accountKeys.PrimaryKey);
Console.WriteLine("Secondary key: {0}", accountKeys.SecondaryKey);

// Regenerate the primary key
BatchAccountRegenerateKeyResponse newKeys = await batchManagementClient.Accounts.RegenerateKeyAsync(
    "MyResourceGroup",
    "mybatchaccount",
    new BatchAccountRegenerateKeyParameters() { KeyName = AccountKeyType.Primary });
```

> [AZURE.TIP] 管理アプリケーションの接続の効率化されたワークフローを作成できます。 取得した後で管理したい Batch アカウントのアカウント キー [ListKeysAsync][net_list_keys], 、Batch .NET ライブラリの初期化中に、このキーを使用して [BatchSharedKeyCredentials][net_sharedkeycred] 初期化時に使用、 [BatchClient][net_batch_client]します。

## Azure サブスクリプションと Batch アカウントのクォータの確認

Azure サブスクリプションおよび Batch などの個別の Azure サービスはすべて、それらの中にある特定のエンティティの数を制限する既定のクォータを持っています。 Azure サブスクリプションの既定のクォータを参照して [Azure サブスクリプションとサービスの制限、クォータ、および制約](./../azure-subscription-service-limits.md), では、Batch サービスの [クォータや、Azure Batch サービスの制限](batch-quota-limit.md)します。 Batch Management .NET ライブラリを使用するとアプリケーション内のこれらのクォータを確認でき、アカウントや、プールやコンピューティング ノードなどのコンピューティング リソースを追加する前に割り当てを決定できます。

### Azure サブスクリプションでの Batch アカウント クォータの確認

リージョンに Batch アカウントを作成する前に、Azure サブスクリプションを調べて、そのリージョンにアカウントを追加できるかどうかを確認できます。

次のコード スニペットでを使用して、 [BatchManagementClient.Accounts.ListAsync][net_mgmt_listaccounts] 、サブスクリプション内のすべてのバッチ アカウントのコレクションを取得します。 アカウントの数は、対象地域を使用してこのコレクションを取得してを決めます [BatchManagementClient.Subscriptions][net_mgmt_subscriptions] をアカウント クォータのバッチを取得し、そのリージョンに作成できるアカウント (ある場合) の数を決定します。

```
// Get a collection of all Batch accounts within the subscription
BatchAccountListResponse listResponse = await batchManagementClient.Accounts.ListAsync(new AccountListParameters());
IList<AccountResource> accounts = listResponse.Accounts;
Console.WriteLine("Total number of Batch accounts under subscription id {0}:  {1}", creds.SubscriptionId, accounts.Count);

// Get a count of all accounts within the target region
string region = "westus";
int accountsInRegion = accounts.Count(o => o.Location == region);

// Get the account quota for the specified region
SubscriptionQuotasGetResponse quotaResponse = await batchManagementClient.Subscriptions.GetSubscriptionQuotasAsync(region);
Console.WriteLine("Account quota for {0} region: {1}", region, quotaResponse.AccountQuota);

// Determine how many accounts can be created in the target region
Console.WriteLine("Accounts in {0}: {1}", region, accountsInRegion);
Console.WriteLine("You can create {0} accounts in the {1} region.", quotaResponse.AccountQuota - accountsInRegion, region);
```

上記のスニペットで `creds` のインスタンスは、 [TokenCloudCredentials][azure_tokencreds]します。 このオブジェクトを作成する例を参照してください、 [AccountManagement][acct_mgmt_sample] GitHub のコード サンプルです。

### Batch アカウントでのコンピューティング リソース クォータの確認

Batch ソリューション内のコンピューティング リソースを増やす前に、リソースを割り当てることによって現在設定されているアカウント クォータを超えないことを確認できます。 次のコード サンプルでは `mybatchaccount` という名前の Batch アカウントのクォータ情報を単に出力していますが、実際のアプリケーションでは、このような情報を使用して、作成する予定の追加リソースをアカウントが処理できるかどうかを判断できます。

```
// First obtain the Batch account
BatchAccountGetResponse getResponse = await batchManagementClient.Accounts.GetAsync("MyResourceGroup", "mybatchaccount");
AccountResource account = getResponse.Resource;

// Now print the compute resource quotas for the account
Console.WriteLine("Core quota: {0}", account.Properties.CoreQuota);
Console.WriteLine("Pool quota: {0}", account.Properties.PoolQuota);
Console.WriteLine("Active job and job schedule quota: {0}", account.Properties.ActiveJobAndJobScheduleQuota);
```

> [AZURE.IMPORTANT] Azure サブスクリプションとサービスの既定のクォータはありますが、これらの制限の多く発生する可能性がで要求を発行して、 [Azure ポータル][azure_portal]します。 たとえばを参照してください [クォータや、Azure Batch サービスの制限](batch-quota-limit.md) 手順については、Batch アカウントのクォータを増やす方法です。

## Batch Management .NET、AAD、リソース マネージャー

バッチ管理 .NET ライブラリを使用する場合、両方の機能が活用通常 [Azure Active Directory][aad_about] (AAD) と [Azure リソース マネージャー][resman_overview]します。 以下で説明するサンプル プロジェクトでは、Azure Active Directory とリソース マネージャーの両方を利用して、Batch Management .NET API の動作を示しています。

### <a name="aad"></a>Azure Active Directory

Azure 自体が、顧客、サービス管理者、および組織のユーザーの認証に Azure Active Directory (AAD) を使用しています。 Batch Management .NET のコンテキストでは、AAD を使用してサブスクリプション管理者または共同管理者を認証した後、それによって管理ライブラリに Batch サービスのクエリおよびこの記事で説明する操作の実行を許可します。

次に、Azure に説明したサンプル プロジェクトで [Active Directory 認証ライブラリ][aad_adal] (ADAL) は、Microsoft ID の資格情報をユーザー入力を求めるために使用します。 サービス管理者または共同管理者の資格情報が提供されると、それにより、アプリケーションは Azure でサブスクリプションのリストをクエリし、リソース グループと Batch アカウントの両方を作成および削除できます。

### リソース マネージャー

Batch アカウントをバッチ管理 .NET ライブラリを作成する場合は通常作成内で、これらを [リソース グループ][resman_overview]します。 使用してプログラムを使用してリソース グループを作成することができます、 [ResourceManagementClient][resman_client] 内で見つかった、 [リソース マネージャーの .NET][resman_api] を使用して以前作成した既存のリソース グループにアカウントを追加するライブラリ、または、 [Azure ポータル][azure_portal]します。

## <a name="sample"></a>GitHub のサンプル プロジェクト

チェック アウト、 [AccountManagment][acct_mgmt_sample] バッチ管理 .NET ライブラリの動作を表示する github サンプル プロジェクトです。 このコンソール アプリケーションの作成および使用を示しています、 [BatchManagementClient][net_mgmt_client] と [ResourceManagementClient][resman_client], 、Azure の使用方法や、 [Active Directory 認証ライブラリ][aad_adal] (ADAL) 両方のクライアントで必要となります。

> [AZURE.IMPORTANT] サンプル アプリケーションを正常に実行するには、必要があります最初に登録すると、Azure 管理ポータルを使用して Azure Active Directory。 チェック アウト **のアプリケーションの追加** で [と Azure Active Directory アプリケーションを統合する][aad_integrate], 、自分のアカウント内でサンプル アプリケーションを登録する記事の手順に従います。

サンプル アプリケーションでは、次の操作が示されています。

1. Azure Active Directory (AAD) を使用してセキュリティ トークンを取得 [ADAL][aad_adal]します。 ユーザーがまだログインしていない場合は、Azure 資格情報の指定を求められます。
2. AAD をから取得したセキュリティ トークンを使用して作成、 [SubscriptionClient][resman_subclient] クエリを実行する Azure アカウントに関連付けられているサブスクリプションの一覧については、ユーザーが複数の場合は、いずれかを選択できるようにする検出されました。
3. 選択されたサブスクリプションに関連付けられた新しい資格情報オブジェクトを作成します。
4. 作成、 [ResourceManagementClient][resman_client] 新しい資格情報を使用します。
5. 使用して、 [ResourceManagementClient][resman_client] 新しいリソース グループを作成するには
6. 使用して、 [BatchManagementClient][net_mgmt_client] さまざまなバッチ アカウントの操作を実行します。
  - 新しく作成されたリソース グループに新しい Batch アカウントを作成します
  - Batch サービスから新しく作成されたアカウントを取得します
  - 新しいアカウントのアカウント キーを出力します
  - アカウント用の新しいプライマリ キーを再生成します
  - アカウントのクォータ情報を出力します
  - サブスクリプションのクォータ情報を出力します
  - サブスクリプション内のすべてのアカウントを出力します
  - 新しく作成されたアカウントを削除します
7. リソース グループを削除します

新しく作成した Batch アカウントとリソース グループを削除する前の両方を調べることができます、 [Azure ポータル][azure_portal]:

![リソース グループと Batch アカウントを表示する azure のポータル][1]
<br />
*Azure ポータルの新しいリソース グループと Batch アカウントを表示します。*

[aad_about]: ../active-directory/active-directory-whatis.md "What is Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Authentication Scenarios for Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrating Applications with Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_mgmt_net]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[azure_portal]: http://portal.azure.com
[azure_storage]: https://azure.microsoft.com/services/storage/
[azure_tokencreds]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.tokencloudcredentials.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_list_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.listkeysasync.aspx
[net_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.createasync.aspx
[net_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.deleteasync.aspx
[net_regenerate_keys]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.accountoperationsextensions.regeneratekeyasync.aspx
[net_sharedkeycred]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.auth.batchsharedkeycredentials.aspx
[net_mgmt_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.aspx
[net_mgmt_subscriptions]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.batchmanagementclient.subscriptions.aspx
[net_mgmt_listaccounts]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.batch.iaccountoperations.listasync.aspx
[resman_api]: https://msdn.microsoft.com/library/azure/mt418626.aspx
[resman_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.resourcemanagementclient.aspx
[resman_subclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.subscriptions.subscriptionclient.aspx
[resman_overview]: ../resource-group-overview.md

[1]: ./media/batch-management-dotnet/portal-01.png

