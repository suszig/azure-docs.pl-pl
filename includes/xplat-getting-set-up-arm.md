<properties services="virtual-machines" title="Using Azure CLI with Azure Resource Manager" authors="squillace" solutions="" manager="timlt" editor="tysonn" />

<tags
   ms.service="virtual-machine"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="linux"
   ms.workload="infrastructure"
   ms.date="04/13/2015"
   ms.author="rasquill" />

## Azure リソース マネージャー (ARM) での Azure CLI の使用

リソース マネージャーのコマンドとテンプレートで Azure CLI を使用して、リソース グループを使用する Azure リソースとワークロードをデプロイできるようにするには、(当然のことですが) Azure のアカウントが必要です。 アカウントがあるない場合は取得できます、 [無料 Azure 評価版は、ここ](http://azure.microsoft.com/pricing/free-trial/)します。

> [AZURE.NOTE] Azure アカウントがない場合、MSDN サブスクリプションのサブスクリプションがアクティブ化して無料の Azure クレジットを取得できます、 [MSDN サブスクリプション会員の特典は、ここ](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -無料のアカウントを使用することもできます。 どちらを使用しても Azure にアクセスできます。

### 手順 1. Azure CLI のバージョンを確認する

命令型コマンドと ARM テンプレートに Azure CLI を使用するには、バージョン 0.8.17 以降が必要です。 バージョンを確認するには、「`azure --version`」と入力します。 次のような結果が表示されます。

    $ azure --version
    0.8.17 (node: 0.10.25)

Azure CLI のバージョンを更新する必要がある場合は、次を参照してください。 [Azure CLI](https://github.com/Azure/azure-xplat-cli)します。

### 手順 2. Azure で職場または学校の ID を使用していることを確認する

使用している場合にのみ、ARM コマンド モードを使用できる、 [Azure Active Directory テナント](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) または [サービス プリンシパル名](https://msdn.microsoft.com/library/azure/dn132633.aspx)します。 (これらとも呼ばれます *組織 id*.)

この ID があることを確認するには、「`azure login`」と入力し、画面の指示に従って、職場または学校のユーザー名とパスワードを使用してサインインします。 この ID がある場合は、次のような結果が表示されます。

    $ azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@contoso.onmicrosoft.com
    Password: *********
    |info:    Added subscription Visual Studio Ultimate with MSDN
    info:    Setting subscription Visual Studio Ultimate with MSDN as default
    info:    Added subscription Azure Free Trial
    +
    info:    login command OK

結果が表示されない場合は、Microsoft アカウント ID を使用して、新しいテナント (またはサービス プリンシパル) を作成する必要があります (個人の MSDN サブスクリプションや無料試用版サブスクリプションの場合は、結果が表示されないことがよくあります)。Microsoft id で作成した Azure アカウントから職場または学校の id を作成するを参照してください。 [Azure AD ディレクトリを新しい Azure サブスクリプションに関連付ける](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant)します。 既に組織 ID があると思われる場合は、アカウントを作成した担当者に直接確認してください。

### 手順 3. Azure サブスクリプションを選択する

Azure アカウントでのサブスクリプションが 1 つだけの場合、Azure CLI 自体が既定でそのサブスクリプションに関連付けられます。 」と入力して使用するサブスクリプションを選択する必要がある場合は、複数のサブスクリプションがある場合は、 `azure account set <subscription id or name> true` 、 _サブスクリプション id または名前_ はサブスクリプション id またはサブスクリプション名の現在のセッションで使用したいです。

次のように表示されます。

    $ azure account set "Azure Free Trial" true
    info:    Executing command account set
    info:    Setting subscription to "Azure Free Trial" with id "2lskd82-434-4730-9df9-akd83lsk92sa".
    info:    Changes saved
    info:    account set command OK

### 手順 4. ARM モードで Azure CLI を使用する

Azure リソース管理 (ARM) モードで Azure CLI を使用するには、「`azure config mode arm`」と入力します。 次のように表示されます。

    $ azure config mode arm
    info:    New mode is arm

> [AZURE.NOTE] 」と入力して Azure サービス管理コマンドの使用に切り替えることができます `azure config mode asm`します。

