<properties
    pageTitle="Azure Key Vault の概要 | Microsoft Azure"
    description="このチュートリアルを使用すると、Azure Key Vault で、強化されたコンテナーを Azure に作成し、暗号化キーやシークレットを Azure に格納して管理できるようになります。"
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="11/10/2015"
    ms.author="cabailey"/>


# Azure Key Vault の概要

Azure Key Vault は、ほとんどのリージョンで使用できます。 詳細については、次を参照してください。、 [Key Vault の料金のページ](../../../../pricing/details/key-vault/)します。

## はじめに

このチュートリアルを使用すると、Azure Key Vault で、強化されたコンテナー (資格情報コンテナー) を Azure に作成し、暗号化キーやシークレットを Azure に格納して管理できるようになります。 ここでは、Azure PowerShell を使用して、Azure アプリケーションで使用できるキーまたはパスワードを含む資格情報コンテナーを作成するプロセスについて説明します。 アプリケーションがそのキーやパスワードを使用する方法についても説明します。

*推定所要時間:** 20 分
>[AZURE.NOTE]  このチュートリアルでは、Key Vault でキーやシークレットを使用するようにアプリケーションを承認する手順の中で、Azure アプリケーションを作成する方法については説明していません。
>
>現時点では、Azure ポータルで Azure Key Vault を構成できません。 代わりに、Azure PowerShell 命令を使用します。 クロスプラット フォーム コマンド ライン インターフェイスの手順については、次を参照してください。 または、 [対応するチュートリアル](key-vault-manage-with-cli.md)します。

Azure Key Vault の概要については、を参照してください [Azure Key Vault は何ですか?](key-vault-whatis.md)。

## 前提条件

このチュートリアルを完了するには、以下が必要です。

- Microsoft Azure サブスクリプション。 サインアップすることができますがない場合は 1 つ、 [無料評価版](../../../../pricing/free-trial)します。
- Azure PowerShell **1.0 以降のバージョン**。 Azure PowerShell のインストールを Azure サブスクリプションに関連付けることは、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md)します。 Azure PowerShell を既にインストールしている、Azure PowerShell コンソールから、バージョンが不明な場合は、を入力 `(Get-module-ListAvailable azure)。バージョン`します。 Azure PowerShell バージョン 0.9.1 ～ 0.9.8 がインストールされている場合は、少し変更を加えるだけで、引き続きこのチュートリアルを利用できます。 たとえば、使用する必要があります、 `Switch-azuremode AzureResourceManager` コマンドと Azure Key Vault のコマンドの一部が変更されました。 バージョン 0.9.1 0.9.8 を Key Vault のコマンドレットの一覧は、次を参照してください。 [Azure Key Vault コマンドレット](https://msdn.microsoft.com/library/azure/dn868052\(v=azure.98\).aspx)します。
- このチュートリアルで作成したキーやパスワードを使用して構成されるアプリケーション。 サンプル アプリケーションは、 [Microsoft ダウンロード センター](http://www.microsoft.com/en-us/download/details.aspx?id=45343)します。 手順については、付属の Readme ファイルをご覧ください。


このチュートリアルは、Azure PowerShell の初心者向けに設計されていますが、モジュール、コマンドレット、セッションなどの基本概念を理解していることを前提としています。 詳細については、次を参照してください。 [Windows PowerShell の概要](https://technet.microsoft.com/library/hh857337.aspx)します。

このチュートリアルに表示されているコマンドレットの詳しいヘルプを確認には、**Get-Help** コマンドレットを使用します。

    Get-Help <cmdlet-name> -Detailed

たとえば、**Add-AzureAccount** コマンドレットのヘルプを確認するには、次のように入力します。

    Get-Help Add-AzureAccount -Detailed

また、次のチュートリアルをお読みになり、Azure PowerShell での Azure リソース マネージャーについて詳しく理解してください。

- [インストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md)
- [リソース マネージャーで Azure PowerShell の使用](../powershell-azure-resource-manager.md)


## <a id="connect"></a>サブスクリプションへの接続します。

Azure PowerShell セッションを開始し、次のコマンドで Azure アカウントにサインインします。

    Login-AzureRmAccount 

ポップアップ ブラウザー ウィンドウで、Azure アカウントのユーザー名とパスワードを入力します。 Azure PowerShell は、このアカウントに関連付けられているすべてのサブスクリプションを取得し、既定で最初のサブスクリプションを使用します。

複数のサブスクリプションがあり、特定の 1 つのサブスクリプションを指定して Azure Key Vault を使用する場合は、次のように入力して自分のアカウントのサブスクリプションを表示します。

    Get-AzureRmSubscription

使用するサブスクリプションを指定するには、次のように入力します。

    Set-AzureRmContext -SubscriptionId <subscription ID>

Azure PowerShell を構成する方法の詳細については、次を参照してください。  [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md)します。


## <a id="resource"></a>新しいリソース グループを作成します。

Azure リソース マネージャーを使用すると、すべての関連するリソースが 1 つのリソース グループ内に作成されます。 このチュートリアルでは、**ContosoResourceGroup** という名前の新しいリソース グループを作成します。

    New-AzureRmResourceGroup –Name 'ContosoResourceGroup' –Location 'East Asia'

## <a id="vault"></a>キー コンテナーを作成します。

使用して、 [新規 AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt603736.aspx) コマンドレットを key vault を作成します。 このコマンドレットには、3 つの必須パラメーター (**リソース グループ名**、**Key Vault 名**、**地理的な場所**) が含まれています。

たとえば、Key Vault 名に **ContosoKeyVault**、リソース グループ名に **ContosoResourceGroup**、場所に**東アジア**を使用する場合は、次のように入力します。

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

このコマンドレットの出力は、作成されたばかりの Key Vault のプロパティを示します。 最も重要な 2 つのプロパティは、次のとおりです。

- **Vault Name**: この例では、これは **ContosoKeyVault** です。 この名前を他の Key Vault コマンドレットに使用できます。
- **資格情報コンテナーの URI**: 例では、これは、https://contosokeyvault.vault.azure.net/です。 その REST API から資格情報コンテナーを使用するアプリケーションは、この URI を使用する必要があります。

Azure アカウントは、この Key Vault ですべての操作の実行が許可されるようになりました。 まだ、どのユーザーも許可されていません。

## <a id="add"></a>キーやシークレットを key vault に追加します。

Azure Key Vault でソフトウェアで保護されたキーを作成する場合を使用して、 [Add-azurekeyvaultkey](https://msdn.microsoft.com/library/azure/dn868048.aspx) コマンドレットと入力します。

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -Destination 'Software'

ただし、既存のソフトウェアで保護されたキーが、C:\ ドライブの PFX ファイルに保存されていて、softkey.pfx という名前で Azure Key Vault にアップロードする場合は、次のように入力して、変数 **securepfxpwd** を .PFX ファイルのパスワード **123** に設定します。

    $securepfxpwd = ConvertTo-SecureString –String '123' –AsPlainText –Force

次のように入力して、.PFX ファイルからキーをインポートします。このキーは、Key Vault サービスのソフトウェアで保護されます。

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd

作成したキーや、Azure Key Vault にアップロードしたキーは、その URI を使用すると参照できます。 使用して **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey** 常に現在のバージョンを取得および使用する **https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87** この特定バージョンを取得します。

このキーの URI を表示するには、次のように入力します。

    $Key.key.kid

資格情報コンテナーにシークレットを追加する (SQLPassword という名前で値が Pa$$w0rd のパスワードを Azure Key Vault に設定する) には、次のように入力して、まず Pa$$w0rd 値をセキュリティで保護された文字列に変換します。

    $secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force

その後、次のように入力します。

    $secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue

Azure Key Vault に追加したパスワードは、その URI を使用すると参照できます。 使用して **https://ContosoVault.vault.azure.net/secrets/SQLPassword** 常に現在のバージョンを取得および使用する **https://ContosoVault.vault.azure.net/secrets/SQLPassword/90018dbb96a84117a0d2847ef8e7189d** この特定バージョンを取得します。

このシークレットの URI を表示するには、次のように入力します。

    $secret.Id

作成したキーやシークレットを表示してみましょう。

- 表示するには、キーを入力: `Get AzureKeyVaultKey – VaultName 'ContosoKeyVault'`
- 表示するには、シークレットを入力: `Get AzureKeyVaultSecret – VaultName 'ContosoKeyVault'`

Key Vault とキーやシークレットは、アプリケーションを使用できる状態になりました。 これらを使用するには、アプリケーションを承認する必要があります。

## <a id="register"></a>Azure Active Directory に登録されたアプリケーション

この手順は通常、開発者が別のコンピューター上で行います。 これは Azure Key Vault に固有のものではありませんが、完全を期すために説明します。

>[AZURE.IMPORTANT] チュートリアルを完了するには、この手順で登録するアカウント、資格情報コンテナー、アプリケーションがすべて同じ Azure ディレクトリに格納されている必要があります。

Key Vault を使用するアプリケーションは、Azure Active Directory から取得したトークンを使用して認証する必要があります。 これを行うには、アプリケーションの所有者は、まず Azure Active Directory でアプリケーションを登録する必要があります。 登録の最後に、アプリケーションの所有者は次の値を取得します。


- **アプリケーション ID** (クライアント ID とも呼ばれます) と**認証キー** (共有シークレットとも呼ばれます)。 アプリケーションは、トークンを取得するために、この 2 つの値を Azure Active Directory に示す必要があります。 これを行うようにアプリケーションを構成する方法は、アプリケーションによって異なります。 Key Vault のサンプル アプリケーション用に、アプリケーション所有者は app.config ファイルでこれらの値を設定します。

Azure Active Directory にアプリケーションを登録するには:

1. Azure ポータルにサインインします。
2. 左側で、**[Active Directory]** をクリックし、アプリケーションを登録するディレクトリを選択します。<br> <br> **注:** 、key vault を作成すると、Azure サブスクリプションを含む同じディレクトリを選択する必要があります。ディレクトリが不明な場合は、**[設定]** をクリックし、Key Vault を作成したサブスクリプションを見つけて、最後の列に表示されているディレクトリ名をご確認ください。

3. **[アプリケーション]** をクリックします。 アプリがディレクトリに追加されていない場合は、このページには **[アプリケーションの追加]** リンクのみが表示されます。 リンクをクリックするか、コマンド バーの **[追加]** をクリックします。
4.  **[アプリケーションの追加]** ウィザードの **[What do you want to do?]** ページで、**[組織で開発中のアプリケーションを追加]** をクリックします。
5.  **[アプリケーション情報の指定]** ページで、アプリケーションの名前を指定し、**[Web アプリケーションや Web API]** (既定値) を選択します。 **次へ**アイコンをクリックします。
6.  **[アプリのプロパティ]** ページで、**[サインオン URL]** と **[アプリケーション ID/URI]** を Web アプリケーションに指定します。 この手順のため作成できる、アプリケーションがこれらの値を持たない場合 (たとえば、両方のボックス http://test1.contoso.com を指定できます)。 これらのサイトが存在するかどうかは関係ありません。各アプリケーションのアプリケーション ID URI がディレクトリ内のすべてのアプリケーションで異なっていることが重要です。 ディレクトリは、この文字列を使用してアプリを識別します。
7.  **完了**アイコンをクリックし、ウィザードで変更内容を保存します。
8.  **[クイック スタート]** ページで、**[構成]** をクリックします。
9.  **[キー]** セクションまでスクロールし、期間を選択し、**[保存]** をクリックします。 ページが更新され、キーの値が表示されます。 このキーと **[クライアント ID]** の値を使用してアプリケーションを構成する必要があります (この構成の手順はアプリケーション固有です)。
10. このページからクライアント ID 値をコピーします。この値は、資格情報コンテナーに権限を設定するために次の手順で使用します

## <a id="authorize"></a>キーやシークレットを使用するアプリケーションを承認します。

キーやシークレットの資格情報コンテナーにアクセスするアプリケーションを承認するために使用して、
 [セット AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/azure/mt603625.aspx) コマンドレットです。

たとえば、資格情報コンテナー名が **ContosoKeyVault** で、承認するアプリケーションのクライアント ID が 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed の場合、アプリケーションの暗号化を解除し、資格情報コンテナー内のキーで署名することを承認するには、次のように実行します。


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign

その同じアプリケーションを認証し、資格情報コンテナーのシークレットの読み取りを許可する場合、次を実行します。


    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToSecrets Get

## <a id="HSM"></a>ハードウェア セキュリティ モジュール (HSM) を使用する場合

さらに安心感を高めたい場合には、ハードウェア セキュリティ モジュール (HSM) でキーのインポートや生成を行うことができ、キーは HSM の境界内から出ることはありません。 HSM は、FIPS 140-2 レベル 2 で検証済みです。 このセクションをスキップし、移動する場合はこの要件が自分に当てはまらない、 [key vault と関連付けられているキーやシークレットを削除](#delete)します。

これらの HSM で保護されたキーを作成する必要があります、 [HSM で保護されたキーをサポートするサブスクリプション資格情報コンテナーの](../../../pricing/free-trial)します。


資格情報コンテナーを作成するときに、**-SKU** パラメーターを追加します。


    New-AzureRmKeyVault -VaultName 'ContosoKeyVaultHSM' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -SKU 'Premium'

この資格情報コンテナーには、ソフトウェアで保護されたキー (前述のとおり) と HSM で保護されたキーを追加できます。 HSM で保護されたキーを作成するには、**-Destination** パラメーターを 'HSM' に設定します。

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -Destination 'HSM'

次のコマンドを使用して、自分のコンピューターの PFX ファイルからキーをインポートできます。 このコマンドでは、Key Vault サービスでキーを HSM にインポートします。

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\softkey.pfx' -KeyFilePassword $securepfxpwd -Destination 'HSM'

次のコマンドは、“bring your own key" (BYOK) パッケージをインポートします。 これを使用すると、ローカルの HSM でキーを生成し、これを Key Vault サービスで HSM に転送でき、キーは HSM の境界内から出ることはありません。

    $key = Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMKey' -KeyFilePath 'c:\ITByok.byok' -Destination 'HSM'

この BYOK パッケージを生成する方法について詳細を参照してください。 [生成し、Azure Key Vault の HSM 保護キーを転送する方法](key-vault-hsm-protected-keys.md)します。

## <a id="delete"></a>Key vault と関連付けられているキーやシークレットを削除します。

使用して key vault を削除するには key vault とキーやシークレットが含まれているを不要な場合、 [削除 AzureRmKeyVault](https://msdn.microsoft.com/library/azure/mt619485.aspx) コマンドレット。

    Remove-AzureRmKeyVault -VaultName 'ContosoKeyVault'

または、Key Vault やそのグループに含まれる他のすべてのリソースを含む、Azure リソース グループ全体を削除できます。

    Remove-AzureRmResourceGroup -ResourceGroupName 'ContosoResourceGroup'

## <a id="other"></a>他の Azure PowerShell コマンドレット

Azure Key Vault の管理に役立つその他のコマンドは次のとおりです。

- `$Keys = get AzureKeyVaultKey VaultName 'ContosoKeyVault'`: このコマンドは、すべてのキーと選択したプロパティの表を取得します。
- `$Keys [0]`: このコマンドは、指定したキーのプロパティの完全な一覧を表示します。
- `Get AzureKeyVaultSecret`: このコマンドは、すべてのシークレットの名前と選択したプロパティの表を一覧表示します。
- `削除 AzureKeyVaultKey VaultName 'ContosoKeyVault' の名前 'ContosoFirstKey'`: 例を特定のキーを削除する方法です。
- `削除 AzureKeyVaultSecret VaultName 'ContosoKeyVault' の名前 'SQLPassword'`: 例を特定のシークレットを削除する方法です。


## <a id="next"></a>次のステップ

Web アプリケーションで Azure Key Vault を使用するフォロー アップ チュートリアルを参照してください。 [Web アプリケーションから Azure Key Vault の使用](key-vault-use-from-web-application.md)します。

Azure Key Vault 用の Azure PowerShell 1.0 コマンドレットの一覧は、次を参照してください。 [Azure Key Vault コマンドレット](https://msdn.microsoft.com/library/azure/dn868052.aspx)します。


プログラミング リファレンスを参照してください [Azure Key Vault 開発者ガイド](key-vault-developers-guide.md)します。





