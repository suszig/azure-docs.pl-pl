<properties
    pageTitle="Azure CLI を使用したロールベースのアクセス制御 (RBAC) の管理 |Microsoft Azure"
    description="Azure コマンド ライン インターフェイスを使用して、ロールとロールのアクションの表示、サブスクリプションとアプリケーションのスコープへのロールの割り当てなどを行って、ロールベースのアクセス制御 (RBAC) を管理する方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="IHenkel"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="12/04/2015"
    ms.author="inhenk"/>

# Azure コマンド ライン インターフェイス (CLI) を使用したロールベースのアクセス制御 (RBAC) の管理
<!-- Azure Selector -->
> [AZURE.SELECTOR]
- [PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure CLI](role-based-access-control-manage-access-azure-cli.md)

## RBAC のロールの表示
### 使用可能なすべてのロールの表示
使用可能なすべてのロールを表示するには、次のコマンドを使用します。

        azure role list

次の例の一覧を表示する *使用可能なすべてのロール*します。

![](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### ロールのアクションの表示
ロールのアクションを表示するには、次のコマンドを使用します。

    azure role show <role in quotes>

次の例のアクションを示している、 *の共同作業者* と *仮想マシンの共同作業者* ロールです。

![](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

##  アクセス権の表示
### リソース グループに対して有効なロールの割り当ての表示
リソース グループに対して有効なリスト ロールの割り当てに使用します。

    azure role assignment list --resource-group <resource group name>

次の例にはロールの割り当てが有効になる、 *製薬 sales-projecforcast* グループです。

![](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### ユーザーへのロールの割り当て (ユーザーのグループに割り当てられているロールを含む) の表示

次の例にはロールの割り当てが有効になる、 *製薬 sales-projecforcast* グループです。

![](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

##  アクセス権の付与
割り当てるロールを特定した後、アクセス権を付与するには、次のコマンドを使用します。

    azure role assignment create

### サブスクリプションのスコープでのグループへのロールの割り当て
サブスクリプションのスコープでグループにロールを割り当てるには、次のコマンドを使用します。

   azure ロールの割り当ての作成 - objId < グループのオブジェクト id >--ロール <name of role> -スコープ < サブスクリプション/サブスクリプション id >

次の例では、 *リーダー* ロール *Christine Koch チーム* で、 *サブスクリプション* スコープ。

![](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### サブスクリプションのスコープでのアプリケーションへのロールの割り当て
サブスクリプションのスコープでアプリケーションにロールを割り当てるには、次のコマンドを使用します。

    azure role assignment create --objId  <applications's object id> --role <name of role> --scope <subscription/subscription id>

次の例の付与、 *の共同作業者* するロール、 *Azure AD* アプリケーションを選択したサブスクリプションです。

 ![](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### リソース グループのスコープでのユーザーへのロールの割り当て
リソース グループのスコープでユーザーにロールを割り当てるには、次のコマンドを使用します。

    azure role assignment create --signInName  <user's email address> --roleName <name of role in quotes> --resourceGroup <resource group name>

次の例の付与、 *仮想マシンの共同作業者* ユーザーにロール *samert@aaddemo.com* で、 *製薬 Sales-ProjectForcast* リソース グループのスコープ。

![](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### リソースのスコープでのグループへのロールの割り当て
リソースのスコープでグループにロールを割り当てるには、次のコマンドを使用します。

    azure role assignment create --objId  <group id> --roleName <name of role in quotes> --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

次の例の付与、 *仮想マシンの共同作業者* するロール、 *Azure AD* グループに、 *サブネット*します。

![](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

##  アクセス権の削除
ロールの割り当てを削除するには、次のコマンドを使用します。

    azure role assignment delete --objId <object id to from which to remove role> --roleName <role name>

次の例の削除、 *仮想マシンの共同作業者* からロールの割り当て *sammert@aaddemo.com* 上、 *製薬 Sales-ProjectForcast* リソース グループ。
次に、このサブスクリプションのグループからロールの割り当てを削除しています。

![](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## カスタム ロールの作成
カスタム ロールを作成するには、`azure role create` コマンドを使用します。

次の例と呼ばれるカスタム ロールを作成する *仮想マシンの演算子* のすべての読み取り操作へのアクセスを許可する *Microsoft.Compute*, 、*Microsoft.Storage*, と *Microsoft.Network* を再起動し、仮想マシンを監視を開始するには、リソース プロバイダー、およびアクセス権を付与します。 カスタム ロールは 2 つのサブスクリプションで使用できます。 この例では、入力として json ファイルが採用されます。

![](./media/role-based-access-control-manage-access-azure-cli/2-azure-role create-1.png)

![](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## カスタム ロールの修正

カスタム ロールを修正するには、最初に azure role show コマンドを使用してロール定義を取得します。 必要に応じてロール定義を変更します。 最後に、azure role set コマンドを使用して変更したロール定義を保存します。

次の例では、Microsoft.Insights/diagnosticSettings/* 操作が Actions に追加され、Azure サブスクリプションが Virtual Machine Operator カスタム ロールの AssignableScopes に追加されます。

![](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## カスタム ロールの削除

カスタム ロールをまず削除するには使用、 `azure role show` を決定するコマンド、 **Id** ロールのです。 使用して、 `azure role delete` を指定して、ロールを削除するコマンド、 **Id**します。

次の例では、削除、 *仮想マシン演算子* カスタム ロールです。

![](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## カスタム ロールの一覧表示

特定のスコープで割り当て可能なロールを一覧表示するには、`azure role list` コマンドを使用します。

次の例では、選択したサブスクリプションで割り当て可能なすべてのロールが一覧表示されます。

![](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

次の例では、 *仮想マシン演算子* カスタム ロールでは使用できません、 *Production4* サブスクリプションにそのサブスクリプションがないため、 **AssignableScopes** ロールのです。

![](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)





## RBAC のトピック
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]


