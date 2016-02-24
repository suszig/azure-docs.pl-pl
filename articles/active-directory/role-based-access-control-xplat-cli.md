<properties
    pageTitle="Azure コマンドライン インターフェイスによるロールベースのアクセス制御の管理"
    description="Azure コマンドライン インターフェイスによるロールベースのアクセス制御の管理"
    services="active-directory"
    documentationCenter="na"
    authors="IHenkel"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/14/2015"
    ms.author="inhenk"/>

# Azure コマンドライン インターフェイス (Azure CLI) によるロールベースのアクセス制御の管理 #

> [AZURE.SELECTOR]
- [Windows PowerShell](role-based-access-control-powershell.md)
- [Azure CLI](/role-based-access-control-xplat-cli-install.md)

Azure ポータルと Azure リソース マネージャー API のロールベースのアクセス制御 (RBAC) を使用すると、サブスクリプションとリソースへのアクセスを詳細に管理できます。 この機能を使用すると、Active Directory ユーザー、グループ、サービス プリンシパルに特定のスコープで役割を割り当てて、アクセス権を付与できます。

このチュートリアルでは、Azure CLI を使用してロールベースのアクセス制御を管理する方法について学習します。 役割の割り当てを作成して確認するプロセスについて説明します。

**推定所要時間:** 15 分

## 前提条件 ##

Azure CLI を使用して RBAC を管理するには、事前に以下を用意しておく必要があります。

- Azure CLI バージョン 0.8.8 以降。 最新のバージョンをインストールして、Azure サブスクリプションに関連付けるを参照してください。 [のインストールと Azure CLI の構成](../xplat-cli-install.md)します。
- セットアップし、Azure CLI で Azure リソース マネージャーを使用に、またに慣れるための次のチュートリアルを読みしてください: [リソース マネージャーで Azure CLI の使用](../xplat-cli-azure-resource-manager.md)

## このチュートリアルの内容 ##

* [サブスクリプションへの接続](#connect)
* [既存の役割の割り当ての確認](#check)
* [役割の割り当ての作成](#create)
* [アクセス許可の確認](#verify)
* [次のステップ](#next)

## <a id="connect"></a>サブスクリプションへの接続 ##

RBAC は Azure リソース マネージャーのみと協働するので、最初に Azure リソース マネージャー モードに切り替えます。 次のコマンドを入力します。

    azure config mode arm

詳細についてを参照してください [リソース マネージャーで Azure CLI の使用](../xplat-cli-azure-resource-manager.md)

ご使用の Azure サブスクリプションに接続するには、以下のように入力します。

    azure login -u <username>

コマンド ライン プロンプトで、Azure アカウントのパスワードを入力します (組織アカウントのみ使用してください)。 Azure CLI は、このアカウントのご使用のサブスクリプションをすべて入手し、最初のサブスクリプションを既定として使うように自己構成します。 役割ベースのアクセス制御の使用時には、共同管理者であるか役割が割り当てられていることによってアクセス許可があるサブスクリプションのみを入手できることに注意してください。

サブスクリプションが複数ある場合に、別のサブスクリプションに切り替えるには、以下のように入力します。

    # This will show you the subscriptions under the account.
    azure account list
    # Use the subscription name to select the one you want to work on.
    azure account set <subscription name>

詳細についてを参照してください [のインストールし、Azure CLI の構成](../xplat-cli-install.md)します。

## <a id="check"></a>既存の役割の割り当ての確認 ##

ここで、既にサブスクリプションに存在している役割の割り当てを確認しましょう。 次のコマンドを入力します。

    azure role assignment list

サブスクリプション内の役割の割り当てがすべて返されます。 注意点が 2 つあります。

1. サブスクリプション レベルの読み取りアクセス権が必要です。
2. サブスクリプションに役割の割り当てが多数ある場合は、すべて取得するまでに時間がかかることがあります。

特定の役割の定義向けに、特定のスコープで、特定のユーザーに対して行われた既存の役割の割り当てを確認することもできます。 次のコマンドを入力します。

    azure role assignment list -g group1 --upn <user email> -o Owner

Azure AD ディレクトリ内の特定のユーザー (リソース グループ「group1」に関する「所有者」役割の割り当てを持つユーザー) に関する役割の割り当てがすべて返されます。 役割は、次の 2 つの場所から割り当てられます。

1. ユーザーに対する、リソース グループに関する「所有者」の役割の割り当て。
2. ユーザーに対する、このリソース グループの親 (この例ではサブスクリプション) に関する「所有者」役割の割り当て。特定の親リソースでアクセス許可がある場合、そのすべての子リソースに対する同じアクセス許可も付与されているからです。

このコマンドレットのパラメーターはすべて省略可能です。 パラメーターを組み合わせて、さまざまなフィルターで役割の割り当てを確認することができます。

## <a id="create"></a>役割の割り当ての作成 ##

役割の割り当てを作成するには、以下の点について考慮する必要があります。

- ロールを割り当てるユーザー: 次の Azure Active Directory コマンドレットを使用して、ディレクトリに入っているユーザー、グループ、およびサービス プリンシパルを確認できます。

    ```
    azure ad user list  
    azure ad user show  
    azure ad group list  
    azure ad group show  
    azure ad group member list  
    azure ad sp list  
    azure ad sp show  
    ```

- 割り当てる役割: 次のコマンドレットを使用して、サポートされているロールの定義を表示できます。

    `azure role list`

- 割り当てるスコープ: スコープには 3 つのレベルがあります。

    - 現在のサブスクリプション
    - リソース グループ。 リソース グループの一覧を入手するには、`azure group list` と入力します。
    - リソース。 リソースの一覧を入手するには、`azure resource list` と入力します。

次に、`azure role assignment create`  を使ってロールの割り当てを作成します。 次に例を示します。

 - ユーザーに関する現在のサブスクリプション レベルでの閲覧者の役割の割り当てが作成されます。

    `azure role assignment create --upn <user's email> -o Reader`

- 次を使用すると、リソース グループ レベルでの役割の割り当てが作成されます。

    `PS C:\> azure role assignment create --upn <user's email> -o Contributor -g group1`

- 次を使用すると、リソース レベルでの役割の割り当てが作成されます。

    `azure role assignment create --upn <user's email> -o Owner -g group1 -r Microsoft.Web/sites -u site1`

## <a id="verify"></a>アクセス許可の確認 ##

ご使用のアカウントに役割の割り当てがあることを確認してから、次を実行することで、実際に役割の割り当てが付与したアクセス許可を確認することができます。

    PS C:\> azure group list
    PS C:\> azure resource list

これらの 2 つのコマンドレットでは、読み取りのアクセス許可があるリソース グループまたはリソースのみが返されます。 また、持っているアクセス許可も表示します。

続いて、`azure group create` などの他のコマンドレットを実行しようとすると、アクセス許可がない場合にはアクセス拒否エラーを受け取ります。

## <a id="next"></a>次のステップ ##

Azure CLI を使用したロールベースのアクセス制御の管理の詳細と、関連トピックについては、以下を参照してください。

- [Azure のロール ベースのアクセス制御](../role-based-access-control-configure.md)
- [Azure CLI のインストールと構成](../xplat-cli-install.md)
- [リソース マネージャーでの Azure CLI の使用](../xplat-cli-azure-resource-manager.md)
- [Azure リソースを管理するリソース グループを使用した](../azure-preview-portal-using-resource-groups.md): を作成し、Azure 管理ポータルでリソース グループを管理する方法について説明します。
- [Azure ブログ](http://blogs.msdn.com/windowsazure): Azure での新機能について説明します。
- [Windows PowerShell を使用した役割ベースのアクセス制御の構成](role-based-access-control-powershell.md)
- [役割ベースのアクセス制御のトラブルシューティング](role-based-access-control-troubleshooting.md)

