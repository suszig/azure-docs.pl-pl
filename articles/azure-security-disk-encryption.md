<properties
   pageTitle="Windows および Linux IaaS VM の Azure ディスク暗号化 | Microsoft Azure"
   description="このドキュメントでは、Windows および Linux IaaS VM の Microsoft Azure ディスク暗号化の概要を示します。"
   services="virtual-machines, cloud-services, storage"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="azure-security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/10/2015"
   ms.author="devtiw"/>



# Windows および Linux IaaS VM プレビューの Azure ディスク暗号化

> [AZURE.NOTE] このドキュメントの情報は、プレビュー リリースの Azure ディスク暗号化に適用されます。


Microsoft Azure では、データのプライバシーおよびデータ主権の確保に積極的に取り組んでおり、暗号化キーの暗号化、制御、管理、また、データ アクセスの制御と監査を行うための幅広い先進テクノロジを介して Azure でホストされるデータを制御できます。そのため、Azure の顧客はビジネス ニーズに最適なソリューションを柔軟に選択することができます。このドキュメントでは、"Windows および Linux IaaS VM の Azure ディスク暗号化" という新しいテクノロジ ソリューションを紹介します。このソリューションは、組織のセキュリティおよびコンプライアンス コミットメントを満たすためのデータの保護に役立ちます。ここでは、サポートされているシナリオやユーザー エクスペリエンスを含む、Azure ディスク暗号化機能の使用方法に関する詳細なガイダンスを提供します。

**注**: ここに含まれる特定の推奨事項により、データ、ネットワーク、またはコンピューティング リソースの使用量が増え、その結果、ライセンスまたはサブスクリプション コストの追加が必要になる可能性があります。

## 概要

Azure ディスク暗号化は、Windows および Linux IaaS 仮想マシン ディスクを暗号化できる新機能です。 Azure ディスクの暗号化は、業界標準を活用して [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) Windows の機能と [DM Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux、OS とデータ ディスクのボリュームの暗号化を提供する機能です。 ソリューションが連携 [Azure キー](https://azure.microsoft.com/documentation/services/key-vault/) 資格情報コンテナーを制御およびディスクの暗号化キーおよび仮想マシンのディスクのすべてのデータが、Azure ストレージに保存されて暗号化されるようにしながら、資格情報コンテナーのサブスクリプションの機密情報を管理することにします。

### 暗号化のシナリオ

Azure ディスク暗号化ソリューションでは、次の 3 つのユーザー暗号化シナリオがサポートされています。

- 顧客が暗号化した VHD と暗号化キーから作成された新しい IaaS VM での暗号化を有効にする

- Azure ギャラリーから作成された新しい IaaS VM での暗号化を有効にする

- Azure で既に実行されている既存の IaaS VM での暗号化を有効にする

ソリューションでは、Microsoft Azure で有効になっている場合、パブリック プレビュー リリースの IaaS VM で以下がサポートされます。

- Azure Key Vault との統合

- 標準 [、D、および G シリーズ IaaS Vm](https://azure.microsoft.com/pricing/details/virtual-machines/)

- 使用して作成された IaaS Vm での暗号化を有効にする [Azure リソース マネージャー](resource-group-overview.md) モデル

- すべての Azure パブリック [領域](https://azure.microsoft.com/regions/)


ソリューションでは、パブリック プレビュー リソースの以下のシナリオ、機能およびテクノロジはサポートされません。

- Basic VM および Standard DS (Premium Storage) シリーズの IaaS VM

- 従来の VM の作成方法を使用して作成された IaaS VM

- Azure ディスク暗号化を使用して有効になっている、IaaS VM での暗号化を無効にする機能

- オンプレミス キー管理サービスとの統合

- Windows Server Technical Preview 3

- Red Hat Enterprise Linux


### 暗号化機能

Azure IaaS VM の Azure ディスク暗号化を有効にしてデプロイすると、指定された構成に応じて、以下の機能が有効になります。

- 顧客ストレージに保存中のブート ボリュームを保護するための OS ボリュームの暗号化

- 顧客ストレージに保存中のデータ ボリュームを保護するためのデータ ボリュームの暗号化

- 顧客の Azure Key Vault サブスクリプションの暗号化キーとシークレットの保護

- 暗号化された IaaS VM の暗号化状態のレポート

- IaaS 仮想マシンからのディスク暗号化構成設定の削除

Windows および Linux 用 IaaS VM の Azure ディスク暗号化ソリューションには、Windows のディスク暗号化拡張機能、Linux のディスク暗号化拡張機能、ディスク暗号化 PowerShell コマンドレット、ディスク暗号化 CLI コマンドレットおよびディスク暗号化 Azure リソース マネージャー テンプレートが含まれます。 Azure ディスク暗号化ソリューションは、Windows または Linux OS を実行している IaaS VM でサポートされます。 サポートされているオペレーティング システムの詳細については、後述の前提条件セクションを参照してください。

パブリック プレビュー期間中は、Azure ディスク暗号化による VM ディスクの暗号化に対して課金されることはありません。 ディスク暗号化が一般公開されてからも継続する予定です。 ただし、市場および競合他社状況に応じて価格は変更になる可能性があります。

### 価値の提案

Azure ディスク暗号化の管理ソリューションは、クラウドにおいて次のビジネス ニーズに対応できます。

-   業界標準の暗号化テクノロジを使用して保存中の IaaS VM をセキュリティで保護し、組織のセキュリティおよびコンプライアンス要件に対処する。

-   IaaS VM は顧客管理のキーとポリシーに従ってブートし、Key Vault での使用量を監査することができる。


### 暗号化のワークフロー

Windows および Linux VM に対してディスク暗号化を有効にするために必要な手順の概要を以下に示します。

1. 顧客は、上記 3 つの暗号化シナリオから暗号化シナリオを選択します。

2. 顧客は、Azure ディスク暗号化 ARM テンプレート、PS コマンドレットまたは CLI コマンドを使用してディスク暗号化を有効にすることを選択し、暗号化構成を指定します。

    - 顧客が暗号化した VHD シナリオの場合、顧客は暗号化した VHD をストレージ アカウントに、暗号化キー マテリアルを Key Vault にアップロードし、暗号化構成を指定して新しい IaaS VM での暗号化を有効にします。

    - Azure ギャラリーから作成された新しい VM と Azure で既に実行されている既存の VM の場合、顧客は、IaaS VM での暗号化を有効にするための暗号化構成を指定します。

3. 顧客は Key Vault からの暗号化キー マテリアル (Windows システムの場合は BitLocker 暗号化キー、Linux の場合はパスフレーズ) の読み取りアクセス権を Azure プラットフォームに付与し、IaaS VM での暗号化を有効にします。

4. 顧客は Key Vault に暗号化キー マテリアルを書き込むための Azure AD ID を指定し、上記シナリオ 2 と 3 の IaaS VM での暗号化を有効にします。

5.  Azure サービス管理では暗号化および Key Vault 構成で VM サービス モデルを更新し、顧客のために暗号化された VM をプロビジョニングします。

![Azure での Microsoft マルウェア対策](./media/azure-security-disk-encryption/disk-encryption-fig1.JPG)

## 前提条件

概要セクションに記載されているサポート対象シナリオの Azure IaaS VM で Azure ディスク暗号化を有効にするための前提条件を以下に示します。

- ユーザーは、サポートされているリージョンにおいて Azure でリソースを作成するための有効なアクティブ Azure サブスクリプションが必要です。

- Azure ディスク暗号化は、Windows サーバー SKU (Windows Server 2008 R2、Windows Server 2012、Windows Server 2012 R2) でサポートされています。 ソリューションは、Windows Server 2008 オペレーティング システムではサポートされません。 Windows Server Technical Preview はパブリック プレビュー リリースではサポートされません。

**注**: Windows Server 2008 R2 の場合、Azure で暗号化を有効にする前に、.Net Framework 4.5 をインストールする必要があります。 オプションの更新プログラムをインストールすることで Windows update からインストールできる"Windows Server 2008 R2 x64 ベース システム用の Microsoft .NET Framework 4.5.2 ([KB2901983](https://support.microsoft.com/kb/2901983))"

- Azure ディスク暗号化は、Linux サーバー SKU (Ubuntu、CentOS、SUSE および SUSE Linux Enterprise Server (SLES)) でサポートされています。 Red Hat Enterprise Linux はパブリック プレビュー リリースではサポートされません。

- すべてのリソース (例: Key Vault、ストレージ アカウント、VM など。)。 同じ Azure リージョンとサブスクリプションに属している必要があります。

**注** - Azure ディスク暗号化では、Key Vault と VM が同じ Azure リージョンにある必要があります。 それぞれ別のリージョンで構成すると、Azure ディスク暗号化機能を有効にする場合にエラーが発生します。

- を設定して、Azure ディスクの暗号化の使用の Azure Key Vault を構成するには、セクションをご覧 **ディスク暗号化の使用状況を設定し、Azure の Azure Key Vault を構成する** この記事の付録に記載されています。

- を設定して、Azure ディスクの暗号化の使用の Azure Active directory の Azure AD アプリケーションの構成セクションを参照して **Azure AD アプリケーションを Azure Active Directory で設定** この記事の付録に記載されています。

- を設定して、Azure AD アプリケーションのキー コンテナーへのアクセス ポリシーを構成するには、セクションをご覧 **Azure AD アプリケーションの設定キー資格情報コンテナーへのアクセス ポリシー** この記事の付録に記載されています。

- 事前暗号化済みの Windows VHD を準備するには、セクションを参照してください。 **事前暗号化済みの Windows VHD を準備する** この記事の付録に記載されています。

- Azure プラットフォームは顧客の Azure Key Vault の暗号化キーまたはシークレットにアクセスし、VM での仮想マシン OS ボリュームのブートと暗号化解除に使用できるようにする必要があります。 Key Vault へのアクセス権を Azure プラットフォームに付与するには、この要件に応じて Key Vault で **enabledForDiskEncryption** プロパティを設定する必要があります。 」を参照して **ディスク暗号化の使用状況を設定し、Azure の Azure Key Vault を構成する** の詳細については、この資料の「付録です。

- Key Vault シークレットおよびキー暗号化キー (KEK) の URL のバージョン管理が必要です。 Azure サービス管理では、このバージョン管理制限が適用されます。 有効なシークレットおよび KEK URL については以下の例を参照してください。

    - 有効なシークレット URL の例:

        *https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

    - 有効な KRK KEK の例:

        *https://contosovault.vault.azure.net/keys/diskencryptionkek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*


- Azure ディスク暗号化では、ポート番号を Key Vault シークレットおよび KEK URL の一部として指定することはサポートされません。 サポートされる Key Vault URL については以下の例を参照してください。

    - 受け入れられない Key Vault URL:

        *https://contosovault.vault.azure.net:443/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*

    - 受け入れられる Key Vault URL:

        *https://contosovault.vault.azure.net/secrets/contososecret/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx*


- Azure ディスク暗号化機能を有効にするには、IaaS VM が次のネットワーク エンドポイントの構成要件を満たす必要があります。

    - IaaS VM では、Azure Active Directory エンドポイント \[Login.windows.net\ に接続できる必要があります] の Azure key vault への接続にトークンを取得するには

    - IaaS VM は、暗号化キーを顧客の Key Vault に書き込むために、Azure Key Vault エンドポイントに接続できる必要があります。

    - IaaS VM は、Azure 拡張リポジトリをホストする Azure ストレージ エンドポイントと、VHD ファイルをホストする Azure ストレージ アカウントに接続できる必要があります。

**注:** セキュリティ ポリシーは、Azure Vm からインターネットへのアクセスを限定する接続を必要し、ip アドレスへの送信接続を許可するように特定のルールを構成する上記の URI を解決できます。

- Azure ディスク暗号化 PowerShell コマンドレットのいずれかを実行するには、次のように、最初に Azure PowerShell バージョン 1.0.1 をインストールする必要があります。

    - Azure PowerShell をインストールして、Azure サブスクリプションに関連付ける、を参照してください [をインストールして、Azure PowerShell を構成する方法](powershell-install-configure.md)。

    - 上記のオプションは更新されませんがまだ Azure PowerShell 1.0.1 リリースは、から直接 Azure PowerShell 1.0.1 をインストールすることも場合 [ここ](https://github.com/Azure/azure-powershell/releases/tag/v1.0.1-November2015)

**注:** このドキュメントは、モジュール、コマンドレット、およびセッションなどの基本的な概念を理解していることを前提としています。 詳細については、の概要を参照してください [Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx)。

- Azure CLI コマンドのいずれかを実行して、Azure サブスクリプションに関連付けるには、まず、以下のように Azure CLI バージョンをインストールする必要があります。

    - Azure CLI をインストールして、Azure サブスクリプションに関連付ける、を参照してください [のインストールと Azure CLI を構成する方法](xplat-cli-install.md)。

    - Mac、Linux、および Windows Azure リソース マネージャーでの Azure CLI を使用して、を参照してください [ここ](azure-cli-arm-commands.md)

- Azure ディスク暗号化ソリューションでは、Windows IaaS VM に対して BitLocker 外部キー保護機能を使用します。 VM がドメインに参加している場合は、TPM 保護機能を適用するグループ ポリシーをプッシュしないでください。 参照してください [今回](https://technet.microsoft.com/library/ee706521) "、互換性のある tpm が装備されていない BitLocker を許可する"のグループ ポリシーの詳細。

### 用語集

このテクノロジで使用される一般的な用語をいくつか理解するために、参照としての以下の用語表を使用します。


| 用語集| 定義|
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Azure AD| Azure AD は [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/)します。Azure AD アカウントは、Key Vault のシークレットの認証、格納、および取得を行うための前提条件です。|
| Azure Key Vault (AKV)| Azure Key Vault は、暗号化キー管理サービスのベースを暗号化キーと重要なシークレットを安全に保護するためのハードウェア セキュリティ モジュールの FIPS 検証済みにします。、を参照してください [Key Vault](https://azure.microsoft.com/services/key-vault/) 詳細についてはドキュメントです。|
| ARM| Azure リソース マネージャー|
| BitLocker| [BitLocker](https://technet.microsoft.com/library/hh831713.aspx) 、業界認定の Windows のボリューム暗号化テクノロジが Windows の IaaS Vm でのディスクの暗号化を有効にするために使用されます|
| BEK| BitLocker 暗号化キーは、OS ブート ボリュームとデータ ボリュームの暗号化に使用されます。BitLocker キーは、顧客の Azure Key Vault でシークレットとして保護されます。|
| CLI| [Azure コマンド ライン インターフェイス](xplat-cli-install.md)|
| DM-Crypt| [DM Crypt](https://en.wikipedia.org/wiki/Dm-crypt) のディスクを Linux IaaS Vm 上で暗号化を有効にするために使用上の Linux ベースの透過的なディスク暗号化サブシステム|
| KEK| キー暗号化キーは、必要に応じてシークレットを保護またはラップするために使用される非対称キー (RSA 2048) です。HSM で保護されたキーまたはソフトウェアで保護されたキーを指定できます。詳細についてを参照してください [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ドキュメントで詳細|
| PS コマンドレット| [Azure PowerShell コマンドレット](powershell-install-configure.md)|

## ディスク暗号化のデプロイメント シナリオとユーザー エクスペリエンス

ディスク暗号化を有効にできるシナリオは多数あり、手順はシナリオによって異なる場合があります。 以下のセクションでは、これらのシナリオについて詳しく説明します。

### Azure ギャラリーから作成された新しい IaaS VM での暗号化を有効にする

発行 ARM テンプレートを使用して Azure での Azure ギャラリーから新しい IaaS Windows 仮想マシンで、ディスクの暗号化を有効にすることができます [ここ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image)します。 Azure クイック スタート テンプレートの [Azure にデプロイ] ボタンをクリックし、パラメーター ブレードに暗号化構成を入力して [OK] をクリックします。 サブスクリプション、リソース グループ、リソース グループの場所、法律条項および契約を選択し、[作成] ボタンをクリックして新しい IaaS VM での暗号化を有効にします。

**注:** このテンプレートでは、Windows Server 2012 ギャラリー イメージを使用して、新しい暗号化された Windows VM を作成します。

次の表で、Azure AD クライアント ID を使用して Azure ギャラリーから新しい VM を作成するシナリオ用の ARM テンプレート パラメーターの詳細を参照できます。

| パラメーター| 説明|
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| adminUserName| 仮想マシンの管理者ユーザー名|
| adminPassword| 仮想マシンの管理者ユーザー パスワード|
| newStorageAccountName| OS とデータの vhd を格納するためのストレージ アカウントの名前|
| vmSize| VM のサイズ。現時点では、Standard A、D および G シリーズのみがサポートされています。|
| virtualNetworkName| VM NIC が属している必要がある VNet の名前。|
| subnetName| VM NIC が属している必要がある VNet のサブネットの名前。|
| AADClientID| Key Vault へのシークレットの書き込み権限を持つ Azure AD アプリのクライアント ID|
| AADClientSecret| Key Vault へのシークレットの書き込み権限を持つ Azure AD アプリのクライアント シークレット|
| keyVaultResourceID,ResourceID| ARM で Key Vault リソースを識別します。PowerShell コマンドレット (Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).ResourceId を使用して取得できます。|
| keyVaultURL| BitLocker キーをアップロードする必要がある Key Vault の URL。コマンドレット (Get-AzureRmKeyVault -VaultName,-ResourceGroupName ).VaultURI を使用して取得できます。|
| keyEncryptionKeyURL| 生成された BitLocker キーの暗号化に使用されるキー暗号化キーの URL。これは省略可能です。|
| vmName| 暗号化操作を実行する VM の名前


**注:** KeyEncryptionKeyURL は省略可能なパラメーターです。 独自の KEK を使用して、Key Vault でデータ暗号化キー (パスフレーズ シークレット) の保護を強化することができます。

### 顧客が暗号化した VHD と暗号化キーから作成された新しい IaaS VM での暗号化を有効にする

このシナリオでは、ARM テンプレート、PowerShell コマンドレットまたは CLI コマンドを使用して、暗号化を有効にすることができます。 以下のセクションでは、ARM テンプレートと CLI コマンドについて詳しく説明します。

#### ARM テンプレートの使用

ディスクの暗号化を有効にすることができます顧客暗号化公開 ARM テンプレートを使用して VHD [ここ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-pre-encrypted-vm)します。 Azure クイック スタート テンプレートの [Azure にデプロイ] ボタンをクリックし、パラメーター ブレードに暗号化構成を入力して [OK] をクリックします。 サブスクリプション、リソース グループ、リソース グループの場所、法律条項および契約を選択し、[作成] ボタンをクリックして新しい IaaS VM での暗号化を有効にします。

次の表で、顧客が暗号化した VHD シナリオ用の ARM テンプレート パラメーターについて詳しく説明します。

| パラメーター| 説明|
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| newStorageAccountName| 暗号化された OS vhd を格納するためのストレージ アカウントの名前。このストレージ アカウントは、VM と同じリソース グループおよび同じ場所に既に作成されている必要があります。|
| osVhdUri| ストレージ アカウントからの OS vhd の URI|
| osType| OS 製品の種類 (Windows/Linux)|
| virtualNetworkName| VM NIC が属している必要がある VNet の名前。これは、VM と同じリソース グループおよび同じ場所に既に作成されている必要があります。|
| subnetName| VM NIC が属している必要がある VNet のサブネットの名前。|
| vmSize| VM のサイズ。現時点では、Standard A、D および G シリーズのみがサポートされています。|
| keyVaultResourceID| ARM で Key Vault リソースを識別する ResourceID。PowerShell コマンドレットを使用して取得できます: (Get AzureRmKeyVault VaultName < yourKeyVaultName > ResourceGroupName < [yourresourcegroupname] >)。リソース Id|
| keyVaultSecretUrl| ​Key Vault でプロビジョニングされたディスク暗号化キーの URL|
| keyVaultKekUrl| 生成されたディスク暗号化キーの暗号化に使用されるキー暗号化キーの URL|
| ​vmName| ​IaaS VM の名前



#### PowerShell コマンドレットの使用

ディスクの暗号化を有効にすることができます顧客暗号化公開 PS コマンドレットを使用して VHD を [ここ](https://msdn.microsoft.com/library/azure/mt603746.aspx)します。

#### CLI コマンドの使用

CLI コマンドを使用するこのシナリオでは、次の手順に従ってディスク暗号化を有効にします。

1. 以下のように、Key Vault でアクセス ポリシーを設定します。
    - 'EnabledForDiskEncryption' フラグを設定します"azure key vault のポリシーの設定 - 資格情報コンテナー名 <keyVaultName> -有効になっているため-ディスク-暗号化 true"。
    - KeyVault に機密情報を書き込むアプリケーションを Azure AD へのアクセス許可を設定します"azure key vault のポリシーの設定 - 資格情報コンテナー名 <keyVaultName> --spn <aadClientID> --の許可を、キー [\"all\"]-機密情報へのアクセス許可を [\"all\"]"。
2. 既存の/実行中の VM での暗号化を有効にするには、次のように入力します。 
    * azure vm 有効にする]-ディスク-暗号化-リソース グループ <resourceGroupName> --名 <vmName> --aad クライアント id <aadClientId> --aad クライアント シークレット <aadClientSecret> -ディスク-暗号化のキーの資格情報コンテナーの url <keyVaultURL> -ディスク-暗号化のキーの資格情報コンテナーの id <keyVaultResourceId>*
3. 暗号化の状態を取得します *"azure vm ショー-ディスク-暗号化の状態-リソース グループ <resourceGroupName> --名 <vmName> -json"*。
4. 顧客が暗号化した VHD からの新しい VM での暗号化を有効にするには、“azure vm create” コマンドで以下のパラメーターを使用します。
    - ディスク-暗号化のキーの資格情報コンテナーの id <disk-encryption-key-vault-id>
    - ディスク暗号化キー url <disk-encryption-key-url>
    - キーの暗号化のキー資格情報コンテナーの id <key-encryption-key-vault-id>
    - キーの暗号化キー url <key-encryption-key-url>


### Azure で既存/実行中の IaaS Windows VM での暗号化を有効にする

このシナリオでは、ARM テンプレート、PowerShell コマンドレットまたは CLI コマンドを使用して、暗号化を有効にすることができます。 以下のセクションでは、ARM テンプレートと CLI コマンドを使用して有効にする方法について詳しく説明します。

#### ARM テンプレートの使用

発行 ARM テンプレートを使用して Azure で IaaS Windows の仮想マシンを既存の/実行中で、ディスクの暗号化を有効にすることができます [ここ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-windows-vm)します。 Azure クイック スタート テンプレートの [Azure にデプロイ] ボタンをクリックし、パラメーター ブレードに暗号化構成を入力して [OK] をクリックします。 サブスクリプション、リソース グループ、リソース グループの場所、法律条項および契約を選択し、[作成] ボタンをクリックして既存/実行中の IaaS VM での暗号化を有効にします。

次の表で、Azure AD クライアント ID を使用する既存/実行中の VM シナリオ用の ARM テンプレート パラメーターの詳細を参照できます。

| パラメーター| 説明|
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ​AADClientID| ​Key Vault へのシークレットの書き込み権限を持つ Azure AD アプリのクライアント ID|
| AADClientSecret| ​Key Vault へのシークレットの書き込み権限を持つ Azure AD アプリのクライアント シークレット|
| **​**keyVaultResourceID| ARM で Key Vault リソースを識別する ResourceID。PowerShell コマンドレットを使用して取得できます: (Get AzureRmKeyVault VaultName < yourKeyVaultName > ResourceGroupName < [yourresourcegroupname] >)。リソース Id|
| ​keyVaultURL| ​BitLocker キーをアップロードする必要がある Key Vault の URL。コマンドレットを使用して入手できます: (Get AzureRmKeyVault VaultName < yourKeyVaultName > ResourceGroupName < [yourresourcegroupname] >)。VaultURI|
| ​ keyEncryptionKeyURL| ​生成された BitLocker キーの暗号化に使用されるキー暗号化キーの URL。これは省略可能です。|
| ​volumeType| ​暗号化操作を実行するボリュームの種類。有効な値は "OS"、"Data"、"All" です。|
| sequenceVersion| BitLocker 操作のシーケンス バージョン。ディスク暗号化操作が同じ VM で実行されるたびに、このバージョン番号をインクリメントします。|
| ​vmName| ​暗号化操作を実行する VM の名前


**注:** KeyEncryptionKeyURL は省略可能なパラメーターです。 独自の KEK を使用して、Key Vault でデータ暗号化キー (BitLocker 暗号化シークレット) の保護を強化することができます。

#### PowerShell コマンドレットの使用

参照してください、 **Azure PowerShell を使用した探索 Azure ディスク暗号化** ブログの投稿 [パート 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) と [パート 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) PS コマンドレットを使用して Azure ディスクの暗号化を使用して暗号化を有効にする方法の詳細。

#### CLI コマンドの使用

CLI コマンドを使用して、Azure で既存/実行中の IaaS Windows VM での暗号化を有効にするには、次の手順に従います。

1. 以下のように、Key Vault でアクセス ポリシーを設定します。
    - 'EnabledForDiskEncryption' フラグを設定します"azure key vault のポリシーの設定 - 資格情報コンテナー名 <keyVaultName> -有効になっているため-ディスク-暗号化 true"。
    - KeyVault に機密情報を書き込むアプリケーションを Azure AD へのアクセス許可を設定します"azure key vault のポリシーの設定 - 資格情報コンテナー名 <keyVaultName> --spn <aadClientID> --の許可を、キー [\"all\"]-機密情報へのアクセス許可を [\"all\"]"。
2. 既存の/実行中の VM での暗号化を有効にするには、次のように入力します。 
    * azure vm 有効にする]-ディスク-暗号化-リソース グループ <resourceGroupName> --名 <vmName> --aad クライアント id <aadClientId> --aad クライアント シークレット <aadClientSecret> -ディスク-暗号化のキーの資格情報コンテナーの url <keyVaultURL> -ディスク-暗号化のキーの資格情報コンテナーの id <keyVaultResourceId>*
3. 暗号化の状態を取得します *"azure vm ショー-ディスク-暗号化の状態-リソース グループ <resourceGroupName> --名 <vmName> -json"*。
4. 顧客が暗号化した VHD からの新しい VM での暗号化を有効にするには、“azure vm create” コマンドで以下のパラメーターを使用します。
    - ディスク-暗号化のキーの資格情報コンテナーの id <disk-encryption-key-vault-id>
    - ディスク暗号化キー url <disk-encryption-key-url>
    - キーの暗号化のキー資格情報コンテナーの id <key-encryption-key-vault-id>
    - キーの暗号化キー url <key-encryption-key-url>


### Azure で既存/実行中の IaaS Linux VM での暗号化を有効にする

発行 ARM テンプレートを使用して Azure で IaaS Linux VM を既存の/実行中で、ディスクの暗号化を有効にすることができます  [ここ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm)します。 Azure クイック スタート テンプレートの [Azure にデプロイ] ボタンをクリックし、パラメーター ブレードに暗号化構成を入力して [OK] をクリックします。 サブスクリプション、リソース グループ、リソース グループの場所、法律条項および契約を選択し、[作成] ボタンをクリックして既存/実行中の IaaS VM での暗号化を有効にします。

次の表で、Azure AD クライアント ID を使用する既存/実行中の VM シナリオ用の ARM テンプレート パラメーターについて詳しく説明します。

| パラメーター| 説明|
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ​AADClientID| ​Key Vault へのシークレットの書き込み権限を持つ Azure AD アプリのクライアント ID|
| AADClientSecret| ​Key Vault へのシークレットの書き込み権限を持つ Azure AD アプリのクライアント シークレット|
| **​**keyVaultResourceID| ARM で Key Vault リソースを識別する ResourceID。PowerShell コマンドレットを使用して取得できます: (Get AzureRmKeyVault VaultName < yourKeyVaultName > ResourceGroupName < [yourresourcegroupname] >)。リソース Id|
| ​keyVaultURL| ​BitLocker キーをアップロードする必要がある Key Vault の URL。コマンドレットを使用して入手できます: (Get AzureRmKeyVault VaultName < yourKeyVaultName > ResourceGroupName < [yourresourcegroupname] >)。VaultURI|
| ​ keyEncryptionKeyURL| ​生成された BitLocker キーの暗号化に使用されるキー暗号化キーの URL。これは省略可能です。|
| ​volumeType| ​暗号化操作を実行するボリュームの種類。有効なサポートされている値は "Data" です。Linux VM では、実行中の Linux VM の OS ボリュームでの暗号化を有効にすることはサポートされません。
| sequenceVersion| BitLocker 操作のシーケンス バージョン。ディスク暗号化操作が同じ VM で実行されるたびに、このバージョン番号をインクリメントします。|
| passPhrase| データ暗号化キーとして強力なパスフレーズを入力します。|
| ​vmName| ​暗号化操作を実行する VM の名前


**注:** KeyEncryptionKeyURL は省略可能なパラメーターです。 独自の KEK を使用して、Key Vault でデータ暗号化キー (パスフレーズ シークレット) の保護を強化することができます。

#### CLI コマンド

顧客から CLI コマンドを使用して暗号化された VHD がインストールされているディスクの暗号化を有効にすることができます [ここ](xplat-cli-install.md)します。 CLI コマンドを使用して、Azure で既存/実行中の IaaS Linux VM での暗号化を有効にするには、次の手順に従います。

1. 以下のように、Key Vault でアクセス ポリシーを設定します。
    - 'EnabledForDiskEncryption' フラグを設定します"azure key vault のポリシーの設定 - 資格情報コンテナー名 <keyVaultName> -有効になっているため-ディスク-暗号化 true"。
    - KeyVault に機密情報を書き込むアプリケーションを Azure AD へのアクセス許可を設定します"azure key vault のポリシーの設定 - 資格情報コンテナー名 <keyVaultName> --spn <aadClientID> --の許可を、キー [\"all\"]-機密情報へのアクセス許可を [\"all\"]"。
2. 既存の/実行中の VM での暗号化を有効にするには、次のように入力します。
    * azure vm 有効にする]-ディスク-暗号化-リソース グループ <resourceGroupName> --名 <vmName> --aad クライアント id <aadClientId> --aad クライアント シークレット <aadClientSecret> -ディスク-暗号化のキーの資格情報コンテナーの url <keyVaultURL> -ディスク-暗号化のキーの資格情報コンテナーの id <keyVaultResourceId>*
3. 暗号化の状態を取得します"azure vm ショー-ディスク-暗号化の状態-リソース グループ <resourceGroupName> --名 <vmName> -json"。
4. 顧客が暗号化した VHD からの新しい VM での暗号化を有効にするには、“azure vm create” コマンドで以下のパラメーターを使用します。
    - * ディスクの暗号化のキー コンテナーの id <disk-encryption-key-vault-id>*
    - * ディスクの暗号化のキーの url <disk-encryption-key-url>*
    - * キーの暗号化のキー資格情報コンテナーの id <key-encryption-key-vault-id>*
    - * キーの暗号化のキーの url <key-encryption-key-url>*

### 暗号化された IaaS VM の暗号化状態の取得

Azure Management (プレビュー) ポータルを使用した暗号化の状態を取得する [PowerShell コマンドレット](https://msdn.microsoft.com/library/azure/mt622700.aspx) または CLI コマンドです。 以下のセクションでは、Azure 管理 (プレビュー) ポータルと CLI コマンドを使用して、暗号化状態を取得する方法を説明します。

#### Azure 管理プレビュー ポータルを使用して暗号化された IaaS VM の暗号化状態を取得する

Azure 管理プレビュー ポータルから IaaS VM の暗号化状態を取得できます。 Https://portal.azure.com/で Azure プレビュー ポータルにログオンして、サブスクリプション内の仮想マシンの概要を確認して、左側のメニューでの仮想マシンのリンクをクリックします。 仮想マシン ビューは、サブスクリプション ドロップダウン リストからサブスクリプション名を選択してフィルター処理できます。 仮想マシン ページのメニューの最上部にある列をクリックします。 選択列ブレードからディスク暗号化の列を選択し、[更新] をクリックします。 次の図に示すように、各 VM の [有効] または [無効] の暗号化状態を表示するディスク暗号化列が表示されます。

![Azure での Microsoft マルウェア対策](./media/azure-security-disk-encryption/disk-encryption-fig2.JPG)

#### ディスク暗号化 PS コマンドレットを使用して暗号化された IaaS VM の暗号化状態を取得する

ディスク暗号化 PS コマンドレット “Get-AzureRmVMDiskEncryptionStatus” から IaaS VM の暗号化状態を取得できます。 VM の暗号化設定を取得するには、Azure PowerShell セッションで以下のように入力します。

    PS C:\Windows\System32\WindowsPowerShell\v1.0> Get-AzureRmVMDiskEncryptionStatus -ResourceGroupName <yourResourceGroupName> -VMName <yourVMName>
    
    OsVolumeEncrypted: True
    OsVolumeEncryptionSettings : {
      "DiskEncryptionKey": {
       SecretUrl":"https://contosovault.vault.azure.net/secrets/BitLockerEncryptionSecretWithKek/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "SourceVault": {
            "ReferenceUri": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx/resourceGroups/xxxxxxx/providers/Mi                            crosoft.KeyVault/vaults/xxxxxxx"
                                }
                            },
                    "KeyEncryptionKey": null
                             }
    DataVolumesEncrypted: True

OSVolumeEncrypted と DataVolumesEncrypted の設定値は "True" に設定されています。これは、両方のボリュームが Azure ディスク暗号化を使用して暗号化されることを示します。 参照してください、 **Azure PowerShell を使用した探索 Azure ディスク暗号化** ブログの投稿 [パート 1](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/17/explore-azure-disk-encryption-with-azure-powershell.aspx) と [パート 2](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx) PS コマンドレットを使用して Azure ディスクの暗号化を使用して暗号化を有効にする方法の詳細。

#### ディスク暗号化 CLI コマンドから IaaS VM の暗号化状態を取得する

ディスク暗号化 CLI コマンド *azure vm show-disk-encryption-status* から、IaaS VM の暗号化状態を取得できます。 VM の暗号化設定を取得するには、Azure CLI セッションで以下のように入力します。

    azure vm show-disk-encryption-status --resource-group <yourResourceGroupName> --name <yourVMName> --json  

## 付録

### サブスクリプションへの接続

先に進む前に、必ず、このドキュメントの前提条件セクションを確認してください。 前提条件がすべて満たされていることを確認したら、以下の手順に従ってサブスクリプションに接続します。

1.Azure PowerShell セッションを開始し、次のコマンドで Azure アカウントにサインインします。

    ログイン AzureRmAccount

2.複数のサブスクリプションありを使用して特定の 1 つを指定する場合は、アカウントのサブスクリプションを表示するには、次を入力します。

    Get AzureRmSubscription

3.使用するサブスクリプションを指定するには、次のように入力します。

    [AzureRmSubscription SubscriptionName <Yoursubscriptionname>

4.構成されているサブスクリプションが正しいことを確認するには、次のように入力します。

    Get AzureRmSubscription

5.Azure ディスク暗号化コマンドレットがインストールされていることを確認するには、次のように入力します。

    Get コマンド *diskencryption*

6.表示する、Azure ディスク暗号化 PowerShell のインストールを確認する出力の下。

    PS C:\Windows\System32\WindowsPowerShell\v1.0 > get コマンド *diskencryption*
    CommandType 名のバージョンのソース                                                             
    コマンドレット Get AzureRmVMDiskEncryptionStatus 1.1.0 AzureRM.Compute                                                    
    コマンドレット削除 AzureRmVMDiskEncryptionExtension 1.1.0 AzureRM.Compute                                                    
    コマンドレットのセット AzureRmVMDiskEncryptionExtension 1.1.0 AzureRM.Compute


### Azure ディスク暗号化の使用に関する Azure Key Vault の設定と構成

Azure ディスク暗号化では、Azure Key Vault のディスク暗号化キーとシークレットを保護します。 Azure ディスク暗号化の使用について Key Vault をセットアップするには、以下のセクションのいずれかの手順に従います。

**注:** Key Vault は VM と同じリージョンにある必要があります。

### 新しい Key Vault の作成

新しい Key Vault を作成するには、以下にリストされている 2 つのオプションのいずれかを使用します。

- テンプレートを使用して、"101-作成-KeyVault"ARM ある [ここ](https://github.com/Azure/azure-quickstart-templates/blob/master/101-create-key-vault/azuredeploy.json)
- Azure PowerShell の Key Vault のコマンドレットを使用して説明として [ここ](key-vault-get-started.md)

**注:** サブスクリプションについて既に Key Vault をセットアップしている場合は、次のセクションに進んでください。

### キー暗号化キーのプロビジョニング (省略可能)

キー暗号化キー (KEK) を使用して、BitLocker 暗号化キーをラップするためのセキュリティ層を追加する場合は、プロビジョニング プロセスで使用するために Key Vault に KEK を追加する必要があります。 使用して、 [Add-azurekeyvaultkey](https://msdn.microsoft.com/library/dn868048.aspx) コマンドレットを Key Vault に新しいキーの暗号化キーを作成します。 詳細については、次を参照してください。 [Key Vault のドキュメント](https://azure.microsoft.com/documentation/services/key-vault/)します。

    Add-AzureKeyVaultKey [-VaultName] <string> [-Name] <string> -Destination <string> {HSM | Software}

### Azure プラットフォームからキーとシークレットにアクセスできるように Key Vault のアクセス許可を設定する

Azure プラットフォームは Azure Key Vault の暗号化キーまたはシークレットにアクセスし、VM でのボリュームのブートと暗号化解除に使用できるようにする必要があります。 Key Vault にアクセスできるように Azure プラットフォームに権限を付与するには、Key Vault で *enabledForDiskEncryption* プロパティを設定する必要があります。 以下の Key Vault PS コマンドレットを使用して、Key Vault に enabledForDiskEncryption プロパティを設定することができます。

    Set-AzureRmKeyVaultAccessPolicy -VaultName <yourVaultName> -ResourceGroupName <yourResourceGroup> -EnabledForDiskEncryption

前述のとおり、Key Vault には *enabledForDiskEncryption* プロパティを設定する必要があります。 Https://resources.azure.com にアクセスして、プロパティを設定できます。 上記に詳しく説明されているプロパティが正しく設定されていることを確認してください。正しく設定されていない場合、デプロイメントは失敗します。

### Azure Active Directory での Azure AD アプリケーションのセットアップ

Azure で実行中の VM に対して暗号化を有効にする必要がある場合、Azure ディスク暗号化機能により、暗号化キーが生成され、Key Vault に書き込まれます。 Key Vault の暗号化キーを管理するには、Azure AD 認証が必要です。

そのため、Azure AD アプリケーションを作成する必要があります。 詳細な手順はアプリケーションを登録することができます、「を取得、識別情報の「アプリケーション」セクションのセクションでこの [ブログの投稿](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx)します。 この投稿には、Key Vault のプロビジョニングおよび構成に関する役立つ例も多数含まれています。 認証目的で、クライアント シークレット ベースの認証またはクライアント証明書ベースの Azure AD 認証を使用できます。

#### Azure AD のクライアント シークレット ベースの認証

以降のセクションでは、Azure AD のクライアント シークレット ベースの認証を構成するために必要な手順が示されています。

##### Azure PowerShell を使用して新しい Azure AD アプリを作成する

新しい Azure AD アプリを作成するには、以下の PowerShell コマンドレットを使用します。

    $aadClientSecret = “yourSecret”
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -Password $aadClientSecret
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

**注:** $azureAdApplication.ApplicationId は Azure AD の ClientID、$aadClientSecret クライアント シークレットを ADE を後で使用する必要があります。Azure の AD クライアント シークレットを適切に保護する必要があります。


##### Azure サービス管理ポータルからの Azure AD のクライアント ID とシークレットのプロビジョニング

Azure の AD クライアント ID とシークレット https://manage.windowsazure.com で、Azure サービス管理ポータルを使用してプロビジョニングできる、このタスクを実行するのには、次の手順に従います。

1.次の図に示すように、Active Directory] タブをクリックします。

![Azure ディスク暗号化](./media/azure-security-disk-encryption\disk-encryption-fig3.JPG)

2.[アプリケーションの追加] をクリックし、次のようにアプリケーション名を入力します。

![Azure ディスク暗号化](./media/azure-security-disk-encryption\disk-encryption-fig4.JPG)

3.矢印ボタンをクリックし、次のようにアプリケーションのプロパティを構成します。

![Azure ディスク暗号化](./media/azure-security-disk-encryption\disk-encryption-fig5.JPG)

4.[完了] を左下隅のチェック マークをクリックします。 アプリの構成ページが表示されます。 次の図に示されているように、Azure AD のクライアント ID がページの下部にあることがわかります。

![Azure ディスク暗号化](./media/azure-security-disk-encryption\disk-encryption-fig6.JPG)

5.[保存] ボタンをクリックして、Azure の AD クライアント シークレットを保存します。 [保存] をクリックし、キーのテキスト ボックスのシークレットをメモします。これが Azure AD のクライアント シークレットです。 Azure AD のクライアント シークレットは適切に保護する必要があります。

![Azure ディスク暗号化](./media/azure-security-disk-encryption\disk-encryption-fig7.JPG)


**注:** 上のこのフローはプレビュー ポータルではサポートされません。

##### 既存のアプリの使用

次のコマンドを実行するために Azure AD PowerShell モジュールから取得できる必要があります [ここ](https://technet.microsoft.com/library/jj151815.aspx)します。

**注:** 以下のコマンドは新しい PowerShell ウィンドウから実行する必要があります。 これらのコマンドを実行する場合は、Azure PowerShell や Azure リソース マネージャーのウィンドウを使用しないでください。 このように推奨するのは、これらのコマンドレットが MSOnline モジュールまたは Azure AD PowerShell にあるためです。

    $clientSecret = ‘<yourAadClientSecret>’ 
    $aadClientID = '<Client ID of your AAD app>'
    connect-msolservice
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type password -Value $clientSecret

#### Azure AD の証明書ベースの認証

以降のセクションには、Azure AD の証明書ベースの認証を構成するために必要な手順が示されています。

##### 新しい Azure AD アプリの作成

新しい Azure AD アプリを作成するには、以下の PowerShell コマンドレットを実行します。

**注:** 以下の "yourpassword" 文字列をセキュリティで保護されたパスワードに置き換え、パスワードを保護してください。

    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\certificates\examplecert.pfx", "yourpassword")
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $azureAdApplication = New-AzureRmADApplication -DisplayName "<Your Application Display Name>" -HomePage "<https://YourApplicationHomePage>" -IdentifierUris "<https://YouApplicationUri>" -KeyValue $keyValue -KeyType AsymmetricX509Cert
    $servicePrincipal = New-AzureRmADServicePrincipal –ApplicationId $azureAdApplication.ApplicationId

この手順が完了したら、.pfx ファイルを Key Vault にアップロードし、VM にその証明書をデプロイするために必要なアクセス ポリシーを有効にします。

##### 既存の Azure AD アプリの使用

既存のアプリの証明書ベースの認証を構成する場合は、次の PowerShell コマンドレットを使用します。 必ず、新しい PowerShell ウィンドウから実行してください。

    $certLocalPath = 'C:\certs\myaadapp.cer' 
    $aadClientID = '<Client ID of your AAD app>'
    connect-msolservice
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate
    $cer.Import($certLocalPath)
    $binCert = $cer.GetRawCertData()
    $credValue = [System.Convert]::ToBase64String($binCert);
    New-MsolServicePrincipalCredential -AppPrincipalId $aadClientID -Type asymmetric -Value $credValue -Usage verify

この手順が完了したら、.pfx ファイルを Key Vault にアップロードし、VM にその証明書をデプロイするために必要なアクセス ポリシーを有効にします。

##### PFX ファイルを Key Vault にアップロードする

この記事を読むことができます [ブログの投稿](http://blogs.technet.com/b/kv/archive/2015/07/14/vm_2d00_certificates.aspx) の詳細については、このプロセスの動作方法についてです。 ただし、このタスクで必要なものは以下の PowerShell コマンドレットだけです。 必ず、Azure PowerShell コンソールから実行してください。

**注:** 以下の "yourpassword" 文字列をセキュリティで保護されたパスワードに置き換え、パスワードを保護してください。

    $certLocalPath = 'C:\certs\myaadapp.pfx' 
    $certPassword = "yourpassword"
    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    
    $fileContentBytes = get-content $certLocalPath -Encoding Byte
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    
    $jsonObject = @"
    {
    "data": "$filecontentencoded",
    "dataType" :"pfx",
    "password": "$certPassword"
    }
    "@
    
    $jsonObjectBytes = [System.Text.Encoding]::UTF8.GetBytes($jsonObject)
    $jsonEncoded = [System.Convert]::ToBase64String($jsonObjectBytes)
    
    Switch-AzureMode -Name AzureResourceManager
    $secret = ConvertTo-SecureString -String $jsonEncoded -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName -SecretValue $secret 
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName –EnabledForDeployment

##### 既存の VM に Key Vault の証明書をデプロイする

PFX のアップロードが完了したら、以下の手順を使用して、Key Vault の証明書を既存の VM にデプロイします。

    $resourceGroupName = ‘yourResourceGroup’
    $keyVaultName = ‘yourKeyVaultName’
    $keyVaultSecretName = ‘yourAadCertSecretName’
    $vmName = ‘yourVMName’
    $certUrl = (Get-AzureKeyVaultSecret -VaultName $keyVaultName -Name $keyVaultSecretName).Id
    $sourceVaultId = (Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $resourceGroupName).ResourceId
    $vm = Get-AzureRmVM -ResourceGroupName $resourceGroupName -Name $vmName
    $vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $sourceVaultId -CertificateStore "My" -CertificateUrl $certUrl
    Update-AzureRmVM -VM $vm  -ResourceGroupName $resourceGroupName

### Azure AD アプリケーションの Key Vault アクセス ポリシーの設定

Azure AD アプリケーションには、Vault 内のキーまたはシークレットへのアクセス権が必要です。 使用して、 [Set-azurekeyvaultaccesspolicy](https://msdn.microsoft.com/library/azure/dn903607.aspx) コマンドレットを – サービス プリンシパル名のパラメーター値として、クライアント Id (アプリケーションの登録時に生成された) を使用して、アプリケーションへのアクセス許可を付与します。 読み取ることができます [このブログの投稿](http://blogs.technet.com/b/kv/archive/2015/06/02/azure-key-vault-step-by-step.aspx) に例をいくつかのです。 PowerShell を使用してこのタスクを実行する方法の以下の例も参照してください。

    $keyVaultName = ‘yourKeyVaultName’
    $aadClientID = '<youAadAppClientID>'
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -ServicePrincipalName $aadClientID -PermissionsToKeys all -PermissionsToSecrets all

### 事前に暗号化された Windows VHD の準備

以下のセクションは、Azure IaaS で暗号化された VHD としてデプロイメントする場合に事前に暗号化された Windows VHD を準備するために必要です。 この手順は、Hyper-V または Azure で新しい Windows VM (vhd) を準備し、ブートするために使用します。

#### グループ ポリシーを更新して非 TPM で OS を保護できるようにする

Local Computer Policy \Computer Configuration\Administrative Templates\Windows Components の下にある、BitLocker ドライブ暗号化という BitLocker グループ ポリシー設定を構成する必要があります。 以下の図に示すように、*[オペレーティング システムのドライブ]、[スタートアップ時に追加の認証を要求する]、[互換性のある TPM が装備されていない BitLocker を許可する]* の順に選択して、この設定を変更します。

![Azure での Microsoft マルウェア対策](./media/azure-security-disk-encryption/disk-encryption-fig8.JPG)

#### BitLocker 機能コンポーネントのインストール

Windows Server 2012 以降の場合は、次のコマンドを使用します。

    dism /online /Enable-Feature /all /FeatureName:Bitlocker /quiet /norestart

Windows Server 2008 R2 の場合は、次のコマンドを使用します。

    ServerManagerCmd -install BitLockers

#### bdehdcfg を使用して BitLocker の OS ボリュームを準備する

OS のパーティションを圧縮して、BitLocker のコンピューターを準備するには、次のコマンドを実行します。

    bdehdcfg -target c: shrink -quiet

#### BitLocker を使用して OS ボリュームを保護する

使用して、 [から manage-bde](https://technet.microsoft.com/library/ff829849.aspx) 外部キー保護機能を使用してブート ボリュームの暗号化を有効にし、外部ドライブまたはボリュームに対する外部キー (.bek ファイル) を配置するコマンドです。 暗号化は、次回のリブート後にシステム/ブート ボリュームで有効になります。

    manage-bde -on %systemdrive% -sk [ExternalDriveOrVolume]
    reboot

**注:** BitLocker を使用して外部キーを取得する場合は、別個のデータ/リソース vhd を使用して VM を準備する必要があります。

#### 事前に暗号化された Linux VHD の準備

##### Ubuntu 14

1.次のスクリプトの内容を/usr/local/sbin/azure_crypt_key.sh、下のファイルを作成します。 KeyFileName に注意してください。これは、Azure によって書き込まれるパスフレーズ ファイル名です。

    #!/bin/sh
    マウント ポイント =/tmp keydisk のマウント
    KeyFileName LinuxPassPhraseFileName =
    echo「... のディスクからキーを取得しようとしています」> & 2
    mkdir-p $MountPoint
    modprobe vfat > デベロッパー/null 2 > & 1
    スリープ 2
    開かれた = 0
    /sys/ブロック/で SFS の sd * です。操作を行います
        デベロッパー =`basename $SFS`
        F = $SFS/${デベロッパー} 1/開発者
        echo"> 中のデバイス: $DEV..."> & 2
        マウント/dev/は省略 ${デベロッパー} 1 $MountPoint-t vfat-r > デベロッパー/null
        場合 [-f $MountPoint/$KeyFileName] です。し、
                cat $MountPoint/$KeyFileName
                umount $MountPoint 2 >/デベロッパー/null
                開かれた = 1
                break
        fi
        umount $MountPoint 2 >/デベロッパー/null
    Promise.done メソッドに関するページ

      if [ $OPENED -eq 0 ]; then
        echo "FAILED to find suitable passphrase file ..." >&2
        echo -n "Try to enter your password: " >&2
        read -s -r A </dev/console
        echo -n "$A"

     else
        "キー ファイルを読み込んで Success!"をエコー > & 2
    fi

2.暗号化構成を変更 */etc/crypttab*します。 次のようになります。

    Sda5_crypt uuid xxxxxxxxxxxxxxxxxxxxx = none luks,discard,keyscript=/usr/local/sbin/azure_crypt_key.sh

3.編集する場合は、 *azure_crypt_key.sh* Windows の Linux にコピーし、実行することを忘れないで *dos2unix/usr/local/sbin/azure_crypt_key.sh*します。
4.実行 *更新 initramfs-u-k すべて* keyscript を有効にする initramfs を更新します。

##### openSUSE 13.2

1./Etc/dracut.conf add_drivers + =「vfat nls_cp437 nls_iso8859-1」の編集します。

2.ファイルの終わりまでにこれらの行をコメント アウト"/usr/lib/dracut/modules.d/90crypt/module-setup.sh"。

    # inst_multiple-o \
    # $systemdutildir/システムのジェネレーター/systemd-cryptsetup-ジェネレーター \
    # $systemdutildir/systemd-cryptsetup \
    # $systemdsystemunitdir/systemd-確認のパスワード-console.path \
    # $systemdsystemunitdir/systemd-確認のパスワード-console.service \
    # $systemdsystemunitdir/cryptsetup.target \
    # $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    # systemd 確認パスワード systemd tty-要求-パスワードのエージェント
    # inst_script"$moddir"/crypt 実行-generator.sh/sbin/crypt-run-generator

3.DRACUT_SYSTEMD を追加、ファイルの先頭に 0 を ="/usr/lib/dracut/modules.d/90crypt/parse-crypt.sh"し、すべての変更"場合は [-z"$DRACUT_SYSTEMD"];then"を"場合は [1] です。then"

4./Usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh を編集し、これを「# 開く LUKS デバイス」の後に追加

    マウント ポイント =/tmp keydisk のマウント
    KeyFileName LinuxPassPhraseFileName =
    echo「... のディスクからキーを取得しようとしています」> & 2
    mkdir-p $MountPoint > & 2
    modprobe vfat > デベロッパー/null > & 2
    開発/SFS の sd * です。操作を行います
       echo"> 中のデバイス: $SFS..."> & 2
       マウント ${SFS} 1 $MountPoint-t vfat-r > & 2
       場合 [-f $MountPoint/$KeyFileName] です。し、
          echo"> キーファイルが..."> & 2
          luksfile = $MountPoint/$KeyFileName
          break
       fi
    Promise.done メソッドに関するページ

5."Dracut – f-v"を実行して initrd を更新するには

##### CentOS 7

1.編集/etc/dracut.conf add_drivers + ="vfat nls_cp437 nls_iso8859 1"

2.ファイルの終わりまでにこれらの行をコメント アウト"/usr/lib/dracut/modules.d/90crypt/module-setup.sh"。

    # inst_multiple-o \
    # $systemdutildir/システムのジェネレーター/systemd-cryptsetup-ジェネレーター \
    # $systemdutildir/systemd-cryptsetup \
    # $systemdsystemunitdir/systemd-確認のパスワード-console.path \
    # $systemdsystemunitdir/systemd-確認のパスワード-console.service \
    # $systemdsystemunitdir/cryptsetup.target \
    # $systemdsystemunitdir/sysinit.target.wants/cryptsetup.target \
    # systemd 確認パスワード systemd tty-要求-パスワードのエージェント
    # inst_script"$moddir"/crypt 実行-generator.sh/sbin/crypt-run-generator

3.DRACUT_SYSTEMD を追加、ファイルの先頭に 0 を ="/usr/lib/dracut/modules.d/90crypt/parse-crypt.sh"し、すべての変更"場合は [-z"$DRACUT_SYSTEMD"];then"を"場合は [1] です。then"

4./Usr/lib/dracut/modules.d/90crypt/cryptroot-ask.sh を編集し、これを「# 開く LUKS デバイス」の後に追加

    マウント ポイント =/tmp keydisk のマウント
    KeyFileName LinuxPassPhraseFileName =
    echo「... のディスクからキーを取得しようとしています」> & 2
    mkdir-p $MountPoint > & 2
    modprobe vfat > デベロッパー/null > & 2
    開発/SFS の sd * です。操作を行います
    echo"> 中のデバイス: $SFS..."> & 2
    マウント ${SFS} 1 $MountPoint-t vfat-r > & 2
    場合 [-f $MountPoint/$KeyFileName] です。し、
        echo"> キーファイルが..."> & 2
        luksfile = $MountPoint/$KeyFileName
        break
    fi
    Promise.done メソッドに関するページ

5.実行、"/usr/sbin/dracut-f v"initrd を更新します。

### 暗号化された VHD を Azure ストレージ アカウントにアップロードする

BitLocker 暗号化または DM-Crypt 暗号化を有効にしたら、ローカルで暗号化された VHD をストレージ アカウントにアップロードする必要があります。

    Add-AzureRmVhd [-Destination] <Uri> [-LocalFilePath] <FileInfo> [[-NumberOfUploaderThreads] <Int32> ] [[-BaseImageUriToPatch] <Uri> ] [[-OverWrite]] [ <CommonParameters>] 

### Key Vault に事前に暗号化された VM のディスク暗号化シークレットをアップロードする

前に取得したディスク暗号化シークレットを、Key Vault にシークレットとしてアップロードする必要があります。

#### KEK で暗号化されないディスク暗号化シークレット

使用 [セット AzureKeyVaultSecret](https://msdn.microsoft.com/library/dn868050.aspx) をキー コンテナーにシークレットをプロビジョニングします。 Windows 仮想マシンの場合は、bek ファイルが base64 文字列としてエンコードされてから、Set-AzureKeyVaultSecret コマンドレットを使用して Key Vault にアップロードされます。 Linux の場合は、パスフレーズが base64 文字列としてエンコードされてから、Key Vault にアップロードされます。 さらに、Key Vault でのシークレットの作成時に、以下のタグが設定されていることを確認してください。

    "tags":
    {
       “DiskEncryptionKeyEncryptionAlgorithm”: “RSA-OAEP (optional)”
       "DiskEncryptionKeyFileName": "Bek file name (windows) or Passphrase filename (linux)"
    }
    
    param(
      [Parameter(Mandatory=$True)]
      [String]$BekFilePath = "C:\vm\nbox\2640EE52-41B3-426C-87B9-484232452CE4.BEK",
      [String]$VaultName = "DiskEncryptionTestAus",
      [String]$SecretName = "BitLockerKey"
      ) 
    
    #"EAN//ojeIQk="
    $bekFileName = split-path $BekFilePath -leaf
    echo "Bek file name = $bekFileName"
    
    $secretBytes = [System.IO.File]::ReadAllBytes($BekFilePath);
    $secret = [Convert]::ToBase64String($secretBytes);
    echo "Secret = $secret"
    
    $secureSecret = ConvertTo-SecureString $secret -AsPlainText -Force
    $tags = @{"DiskEncryptionKeyFileName" = "$bekFileName"} 
    
    echo "Tags = $tags"
    echo "Vault = $VaultName"
    echo "Secret name = $SecretName"
    echo "Adding secret to Key vault"
    
    Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secureSecret -tags $tags

#### KEK で暗号化されるディスク暗号化シークレット

シークレットは、Key Vault にアップロードする前に、必要に応じてキー暗号化キーで暗号化できます。 ラップを使用して [API](https://msdn.microsoft.com/library/azure/dn878066.aspx) 最初にキーの暗号化キーを使用して、シークレットを暗号化します。 この wrap 操作の出力は、シークレットを使用して、アップロードされる base64 URL でエンコードされた文字列、 [セット AzureKeyVaultSecret](https://msdn.microsoft.com/library/dn868050.aspx) コマンドレットです。


## このガイドのダウンロード

このガイドをダウンロードすることができます、 [TechNet ギャラリー](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)します。


## BLOB の詳細

[Azure PowerShell を使用した Azure ディスクの暗号化を表示します。](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/16/explore-azure-disk-encryption-with-azure-powershell.aspx?wa=wsignin1.0)

[第 2 部 Azure PowerShell を使用した Azure ディスクの暗号化を表示します。](http://blogs.msdn.com/b/azuresecurity/archive/2015/11/21/explore-azure-disk-encryption-with-azure-powershell-part-2.aspx)






