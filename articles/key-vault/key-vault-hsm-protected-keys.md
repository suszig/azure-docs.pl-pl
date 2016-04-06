<properties
    pageTitle="Azure Key Vault の HSM 保護キーを生成し、転送する方法 | Microsoft Azure"
    description="この記事は Azure Key Vault と共に使用する独自の HSM 保護キーを計画、生成、転送する際に役立ちます。"
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
    ms.topic="article" 
    ms.date="09/18/2015"
    ms.author="cabailey"/>
#Azure Key Vault の HSM 保護キーを生成し、転送する方法

##はじめに

Azure Key Vault の使用時にさらに安心感を高める場合、ハードウェア セキュリティ モジュール (HSM) でキーをインポートしたり、生成したりできます。キーは HSM の境界内から出ることはありません。 このシナリオとして呼ば *独自のキーを取り込む*, 、または BYOK です。 HSM は、FIPS 140-2 レベル 2 で検証済みです。 Azure Key Vault は HSM の Thales nShield ファミリを使用してキーを保護します。

このトピックの情報は Azure Key Vault と共に使用する独自の HSM 保護キーを計画、生成、転送する際に役立ちます。 

>[AZURE.NOTE] Azure Key Vault の詳細については、次を参照してください。 [Azure Key Vault は何ですか。](key-vault-whatis.md)  
>
>入門チュートリアル、key vault の HSM 保護キーの作成も含め、次を参照してください。 [Azure Key Vault を使ってみる](key-vault-get-started.md)します。

HSM 保護キーを生成し、インターネットで転送する方法:

- 攻撃を減らすオフライン ワークステーションからキーを生成します。

- キーは Key Exchange Key (KEK) で暗号化されます。Azure Key Vault HSM に転送されるまで暗号化が維持されます。 暗号化されたキーだけが元のワークステーションを離れます。

- ツールセットにより、キーを Azure Key Vault セキュリティ ワールドにバインディングするテナント キーのプロパティが設定されます。 そのため、Azure Key Vault HSM がキーを受け取り、復号化すると、これらの HSM のみでそれを使用できます。 キーはエクスポートできません。 このバインディングは Thales HSM により適用されます。

- キーの暗号化に使用される Key Exchange Key (KEK) は Azure Key Vault HSM 内で生成され、エクスポートできません。 HSM により、HSM の外部では平文の KEK がありません。 また、ツールセットには、KEK はエクスポート不可であり、Thales で作成された本物の HSM 内で生成されたことを示す Thales からの証明が含まれます。

- ツールセットには、Azure Key Vault セキュリティ ワールドも Thales で作成された本物の HSM 内で生成されたことを示す Thales からの証明が含まれます。 これにより、Microsoft が本物のハードウェアを使用していることが証明されます。

- Microsoft は地域ごとに個別の KEK と個別のセキュリティ ワールドを使用します。それにより、キーを暗号化したリージョンのデータ センターでのみキーを使用できます。 たとえば、欧州のお客様からのキーは北米やアジアのデータ センターでは使用できません。

##Thales HSM と Microsoft のサービスの詳細

Thales e-Security は、金融サービス、ハイテク、製造、政府、テクノロジ セクターにデータ暗号化とサイバー セキュリティのソリューションを提供することで世界をリードする企業です。 企業と政府の情報を 40 年間保護してきた実績を持つ Thales ソリューションはエネルギー/航空宇宙産業の大企業 5 社のうち 4 社で採用され、22 の NATO 加盟国で利用されています。世界中の支払処理の 80% 以上を保護しています。

Microsoft は Thales と連携し、HSM の最新技術を強化しています。 この改良により、キーの管理を放棄することなく、ホステッド サービスの一般的な長所を得ることができます。 具体的には、この改良により、Microsoft があなたの代わりに HSM を管理できます。 クラウド サービスとしては、Azure Key Vault は短期間でスケールアップされ、組織の利用率急増に対応します。 同時に、キーは Microsoft の HSM 内で保護されます。あなたがキーを生成し、それを Microsoft の HSM に転送するため、キーが存続する間、あなたがそれを管理します。

##Azure Key Vault の Bring Your Own Key (BYOK) の実装

独自の HSM 保護キーを生成し、それを Azure Key Vault に転送する場合 (Bring Your Own Key (BYOK) シナリオ)、次の情報と手順を利用します。


##BYOK の前提条件

Azure Key Vault の Bring Your Own Key (BYOK) の前提条件の一覧については、次の表を参照してください。

|要件|詳細情報|
|---|---|
|Azure のサブスクリプション|Azure Key Vault を作成するには、Azure サブスクリプションを必要: [無料評価版にサインアップ](http://azure.microsoft.com/pricing/free-trial/)|
|HSM をサポートする Azure Key Vault|Azure Key Vault のサービス階層と機能の詳細については、次を参照してください。、 [Azure Key Vault 価格](http://azure.microsoft.com/pricing/details/key-vault/) web サイトです。|
|Thales HSM、スマート カード、サポート ソフトウェア|Thales ハードウェア セキュリティ モジュールにアクセスできることと Thales HSM の基本操作知識が必要です。 参照してください [Thales ハードウェア セキュリティ モジュール](https://www.thales-esecurity.com/msrms/buy) 一覧については、互換性のあるモデルの場合、またはサブスクリプションがあるない場合は、HSM を購入します。|
|次のハードウェアとソフトウェア:<ol><li>オフラインの x64 Windows 7 と Thales nShield ソフトウェアには、少なくともの以降の Windows オペレーティング システムを実行できるワークステーション バージョン 11.50 です。<br/><br/>このワークステーションでは、Windows 7 を実行する場合は、まず [Microsoft .NET Framework 4.5 のインストール](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe)します。</li><li>インターネットに接続されているし、Windows 7 の最小の Windows オペレーティング システムを実行できるワークステーション。</li><li>USB ドライブまたは 16 MB 以上の空き領域を持つその他のポータブル ストレージ デバイスです。</li></ol>|セキュリティ上の理由から、最初のワークステーションをネットワークに接続しないことをお勧めします。 ただし、これはプログラムで適用されません。<br/><br/>次の手順でこのワークステーションと呼んでいますに未接続ワークステーションに注意してください。</p></blockquote><br/>さらに、テナント キーが実稼動ネットワークにある場合は、ツールセットをダウンロードして、テナント キーのアップロードには、別の 2 つ目のワークステーションを使用することを勧めします。 テスト目的で、1 つ目として同じワークステーションを使用することができます。<br/><br/>で次の手順、この予備のワークステーションと呼んでいますにインターネット接続ワークステーションに注意してください。</p></blockquote><br/>|

##キーを生成し、Azure Key Vault HSM に転送する

次の 5 つの手順でキーを生成し、Azure Key Vault HSM に転送します。 

- [手順 1: インターネット接続ワークステーションを準備する](#step-1-prepare-your-internet-connected-workstation)
- [手順 2: 未接続ワークステーションを準備する](#step-2-prepare-your-disconnected-workstation)
- [手順 3: キーを生成する](#step-3-generate-your-key)
- [手順 4: キーの転送準備をする](#step-4-prepare-your-key-for-transfer)
- [手順 5: Azure Key Vault にキーを転送する](#step-5-transfer-your-key-to-azure-key-vault)

## 手順 1: インターネット接続ワークステーションを準備する
この最初の手順では、インターネットに接続されているワークステーションで次の手順を実行します。


###手順 1.1: Azure PowerShell をインストールする

インターネット接続ワークステーションから、Azure Key Vault を管理するためのコマンドレットを含む Azure PowerShell をダウンロードしてインストールします。 これには 0.8.13 以降の最小バージョンが必要です。

インストール手順については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md)します。

###手順 1.2: Azure サブスクリプション ID を取得する

Azure PowerShell セッションを開始し、次のコマンドで Azure アカウントにサインインします。

        Add-AzureAccount
ポップアップ ブラウザー ウィンドウで、Azure アカウントのユーザー名とパスワードを入力します。 次に、使用、 [Get-azuresubscription](https://msdn.microsoft.com/library/azure/dn790366.aspx) コマンド。

        Get-AzureSubscription
出力から、Azure Key Vault で使用するサブスクリプションの ID を見つけます。 このサブスクリプション ID は後で必要になります。

Azure PowerShell ウィンドウを閉じないでください。

###手順 1.3: Azure Key Vault の BYOK ツールセットをダウンロードする

Microsoft ダウンロード センターにアクセスし、 [Azure Key Vault BYOK ツールセットをダウンロード](http://www.microsoft.com/download/details.aspx?id=45345) お住まいの地域。

|リージョン|パッケージ名|SHA-256 パッケージ ハッシュ|
|---|---|---|
|北米|KeyVault-BYOK-Tools-UnitedStates.zip|D9FDA9F5A34E1388CD6C9138E5B75B7051FB7D6B11F087AFE0553DC85CCF0E36|
|ヨーロッパ|KeyVault-BYOK-Tools-Europe.zip|881DCA798305B8408C06BAE7B3EFBC1E9EA6113A8D6EC443464F3744896F32C3|
|アジア|KeyVault-BYOK-Tools-AsiaPacific.zip|0C76967B3AC76687E4EA47EB96174EE6B25AB24E3114E28A90D9B93A2E6ABF6E|
|ラテン アメリカ|KeyVault-BYOK-Tools-LatinAmerica.zip|B38015990D4D1E522B8367FF78E78E0234BF9592663470426088C44C3CAAAF48|
|日本|KeyVault-BYOK-Tools-Japan.zip|DB512CD9472FDE2FD610522847DF05E4D7CD49A296EE4A2DD74D43626624A113|
|オーストラリア|KeyVault-BYOK-Tools-Australia.zip|8EBC69E58E809A67C036B50BB4F1130411AD87A7464E0D61A9E993C797915967|

使用して、Azure PowerShell セッションから、ダウンロードした BYOK ツールセットの整合性を検証する、 [Get-filehash](https://technet.microsoft.com/library/dn520872.aspx) コマンドレットです。

    Get-FileHash KeyVault-BYOK-Tools-*.zip

ツールセットには次が含まれます。

- 名前の先頭を含む Key Exchange Key (KEK) パッケージ **BYOK に付きます。**
- 名前の先頭を含むセキュリティ ワールド パッケージ **BYOK-SecurityWorld 付きます。**
- Python スクリプト。 名前 v**erifykeypackage.py します。**
- コマンドライン実行可能ファイル **KeyTransferRemote.exe** および関連 Dll。
- Visual C 再頒布可能パッケージ、名前付き **vcredist_x64.exe します。**

USB ドライブまたはその他のポータブル ストレージにパッケージをコピーします。

##手順 2: 未接続ワークステーションを準備する

この 2 つ目の手順では、ネットワーク (インターネットまたは内部ネットワーク) に接続されていないワークステーションで次の手順を実行します。


###手順 2.1: Thales HSM で未接続ワークステーションを準備する

Windows コンピューターに nCipher (Thales) サポート コンピューターをインストールし、そのコンピューターに Thales HSM をアタッチします。 

Thales ツールがパスにあることを確認 (**%nfast_home%\bin** と **%nfast_home%\python\bin**)。 たとえば、次を入力します。

        set PATH=%PATH%;”%nfast_home%\bin”;”%nfast_home%\python\bin”

詳細については、Thales HSM に付属のユーザー ガイドを参照してください。

###手順 2.2: 未接続ワークステーションに BYOK ツールセットをインストールします。

USB ドライブまたはその他のポータブル ストレージから BYOK ツールセット パッケージをコピーし、次の操作します。

1. ダウンロードしたパッケージから任意のフォルダーにファイルを抽出します。
2. そのフォルダーから vcredist_x64.exe を実行します。
3. 指示に従い、Visual Studio 2012 用の Visual C++ ランタイム コンポーネントをインストールします。

##手順 3: キーを生成する

この 3 つ目の手順では、未接続ワークステーションで次の手順を実行します。

###手順 3.1: セキュリティ ワールドを作成する

コマンド プロンプトを起動し、Thales new-world プログラムを実行します。

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

このアプリケーションを作成、 **セキュリティ ワールド** ファイル %nfast_kmdata%\local\world、C:\ProgramData\nCipher\Key Management data \local フォルダーに対応します。 クォーラムにはさまざまな値を使用できますが、今回の例では、3 枚の空白カードと各カードのピンを入力するように求められます。 いずれかの 2 枚のカードがセキュリティ ワールドに完全アクセスを与えます。 これらのカードになる、 **管理者カード セット** 、新しいセキュリティ ワールドにします。 

次に、次を実行します。

- ワールド ファイルをバックアップします。 ワールド ファイル、管理者カード、そのピンを保護します。1 人の人間が複数のカードにアクセスできないようにします。

###手順 3.2: ダウンロードしたパッケージを検証します。

この手順は省略可能ですが、次を検証できるので推奨されます。

- ツールセットに含まれる Key Exchange Key が本物の Thales HSM から生成されていること。
- ツールセットに含まれているセキュリティ ワールドのハッシュが本物の Thales HSM から生成されていること。
- Key Exchange Key がエクスポート不可であること。

>[AZURE.NOTE]ダウンロードしたパッケージを検証するには、HSM は接続する必要があります電源がオンでされ、セキュリティ ワールド (先ほど作成したもの) などがあります。

ダウンロードしたパッケージを検証するには:

1.  リージョンによっては、次のいずれかを関連付け、verifykeypackage.py スクリプトを実行します。
    - 北米:

            python verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
    - ヨーロッパ:

            python verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
    - アジア:

            python verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
    - ラテン アメリカ:

            python verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
    - 日本:

            python verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
    - オーストラリア:

            python verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1

    >[AZURE.TIP]Thales ソフトウェアには %NFAST_HOME%\python\bin に python が含まれています
    
2.  確認が正常に検証を示す、次を参照してください: **結果: 成功**

このスクリプトは Thales ルート キーまで署名者のチェーンを検証します。 このルート キーのハッシュがスクリプトに埋め込まれていて、その値にする必要があります **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**します。 別途にアクセスしてもこの値を確認することができます、 [Thales の web サイト](http://www.thalesesec.com/)します。

これで新しいキーを作成する準備が整いました。

###手順 3.3: 新しいキーを作成する

Thales を使用してキーを生成 **generatekey** プログラムです。

次のコマンドを実行してキーを生成します。

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

このコマンドを実行するとき、次の指示に従います。

- 値を置換 *contosokey* の **ident** と **plainname** 文字列値。 管理費を最小限に抑え、エラーを犯す可能性を減らすために、両方に同じ値を使用することをお勧めします。  **Ident** 値は、数字、ダッシュ、および文字の小文字のみを含める必要があります。

- pubexp はこの例では空白のまま (既定) ですが、特定の値を指定できます。 詳細については、Thales の文書を参照してください。

このコマンドで始まる名前の %NFAST_KMDATA%\local フォルダーにトークン化されたキー ファイルを作成する **key_simple _** コマンドで指定された ident が続きます。 例: **key_simple_contosokey**します。 このファイルには暗号化されたキーが含まれます。 

安全な場所でこのトークン化されたキーのファイルをバックアップします。

>[AZURE.IMPORTANT] 後で Azure Key Vault にキーを転送するときに Microsoft は、安全に、キーとセキュリティ ワールドをバックアップすることが非常に重要になるようにこのキーをエクスポートできません。 キーのバックアップ方法と最良事例については Thales にお問い合わせください。

これで Azure Key Vault にキーを転送する準備ができました。

##手順 4: キーの転送準備をする

この 4 つ目の手順では、未接続ワークステーションで次の手順を実行します。

###手順 4.1: アクセス権が制限されたキーのコピーを作成します。

キーのアクセス権を制限するには、コマンド プロンプトから、ご自分のリージョンに基づいて次のいずれかを実行します。

- 北米:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
- ヨーロッパ:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
- アジア:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
- ラテン アメリカ:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
- 日本:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
- オーストラリア:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1

このコマンドを実行するときに置換 *contosokey* で指定した値では、同じ **手順 3.3: 新しいキーを作成** から、 [キーを生成する](#step-3-generate-your-key) 手順です。

セキュリティ ワールドの管理者カードを差し込むように求められます。

コマンドが完了したとき、わかります **結果: 成功** と権限が制限されたキーのコピーが、key_xferacid _ という名前のファイルに<contosokey>.

###手順 4.2: キーの新しいコピーを検査する

必要に応じて、Thales ユーティリティを実行し、新しいキーの最小アクセス権を確認します。

- aclprint.py:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
- kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
これらのコマンドを実行するときに、contosokey を置き換えますで指定した同じ値を持つ **手順 3.3: 新しいキーを作成** から、 [キーを生成する](#step-3-generate-your-key) 手順です。

###手順 4.3: Microsoft の Key Exchange Key を使用してキーを暗号化する

ご自分のリージョンに基づいて次のコマンドのいずれかを実行します。

- 北米:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- ヨーロッパ:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- アジア:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- ラテン アメリカ:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- 日本:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- オーストラリア:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey

このコマンドを実行するとき、次の指示に従います。

- 置換 *contosokey* でキーを生成するために使用する識別子を持つ **手順 3.3: 新しいキーを作成** から、 [キーを生成する](#step-3-generate-your-key) 手順です。

- 置換 *SubscriptionID* key vault を含む Azure サブスクリプションの id。 この値を取得したでこれまでは、 **手順 1.2: Azure サブスクリプション ID を取得する** から、 [インターネット接続ワークステーションを準備する](#step-1-prepare-your-internet-connected-workstation) 手順です。

- 置換 *ContosoFirstHSMKey* の出力ファイル名に使用されるラベルです。

ときにこれが正常に完了して表示 **結果: 成功** と次の名前を持つ現在のフォルダーに新しいファイルが、: transferpackage-*ContosoFirstHSMkey*.byok

###手順 4.4: キー転送パッケージをインターネット接続ワークステーションにコピーします。 

USB ドライブまたはその他のポータブル ストレージを使用し、前の手順の出力ファイル (KeyTransferPackage-ContosoFirstHSMkey.byok) をインターネット接続ワークステーションにコピーします。

##手順 5: Azure Key Vault にキーを転送する

この最後の手順で、インターネット接続ワークステーションで使用して、 [Add-azurekeyvaultkey](https://msdn.microsoft.com/library/azure/dn868048.aspx) コマンドレットを Azure Key Vault HSM に未接続ワークステーションからコピーしたキー転送パッケージをアップロードします。

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\TransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

アップロードされると、追加したキーのプロパティが表示されます。

##次のステップ

これでこの HSM 保護キーを Key Vault で使用できます。 詳細については、次を参照してください。、 **ハードウェア セキュリティ モジュール (HSM) を使用する場合は** セクション、 [Azure Key Vault の概要](key-vault-get-started.md) チュートリアルです。


