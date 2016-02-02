<properties 
    pageTitle="Azure MFA Server と Windows Server 2012 R2 AD FS を使用したクラウドおよびオンプレミスのリソースのセキュリティ保護" 
    description="Windows Server 2012 R2 で Azure MFA および AD FS を開始する方法について説明する Azure Multi-Factor Authentication のページです。" 
    services="multi-factor-authentication" 
    documentationCenter="" 
    authors="billmath" 
    manager="stevenpo" 
    editor="curtland"/>

<tags 
    ms.service="multi-factor-authentication" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/19/2015" 
    ms.author="billmath"/>



# Azure Multi-Factor Authentication Server と Windows Server 2012 R2 AD FS を使用したクラウドおよびオンプレミスのリソースのセキュリティ保護

お客様の組織が Azure AD とフェデレーションされており、オンプレミスまたはクラウドにセキュリティ保護したいリソースがある場合は、Azure Multi-Factor Authentication Sever を使用して、AD FS と連動するよう構成し、価値の高いエンド ポイントで多要素認証がトリガーされるようにすることによってセキュリティ保護を行えます。

このドキュメントでは、Azure Multi-Factor Authentication Server と Windows Server 2012 R2 の AD FS の使用について説明します。 Azure を使用して AD FS 2.0 による多要素認証について [AD FS 2.0 による Azure Multi-factor Authentication Server を使用してクラウドと内部設置型のリソースをセキュリティで保護された](multi-factor-authentication-get-started-adfs-adfs2.md)します。

## Windows Server 2012 R2 AD FS を Azure Multi-Factor Authentication Server でセキュリティ保護する

Azure Multi-Factor Authentication Server をインストールする際、次の 2 つのオプションがあります。

- AD FS と同じサーバーに Azure Multi-factor Authentication Server をローカル インストールする
- AD FS サーバーに Azure Multi-factor Authentication Adapter をローカル インストールし、MFA Server を別のコンピューターにインストールする

開始する前に、次の情報に注意してください。

- Azure Multi-Factor Authentication Server を AD FS フェデレーション サーバーにインストールすることは必須ではありませんが、AD FS 用の Multi-Factor Authentication Adapter は、AD FS を実行する Windows Server 2012 R2 にインストールする必要があります。 サポートされているバージョンであれば、サーバーを別のコンピューターにインストールすることができ、AD FS アダプターを AD FS フェデレーション サーバーに別個にインストールすることができます。 アダプターを別個にインストールする手順については、以下の手順を参照してください。

- サインオン アカウントには、Active Directory でセキュリティ グループを作成する権限が必要です。

- Multi-Factor Authentication AD FS Adapter インストール ウィザードは、PhoneFactor Admins という名前のセキュリティ グループを Active Directory に作成し、フェデレーション サービスの AD FS サービス アカウントをこのグループに追加します。ドメイン コントローラーで、PhoneFactor Admins グループが本当に作成されていること、および AD FS サービス アカウントがこのグループのメンバーであることを確認するようお勧めします。 必要に応じて、AD FS サービス アカウントをドメイン コント ローラーの PhoneFactor Admins グループに手動で追加します。


### AD FS と同じサーバーに Azure Multi-factor Authentication Server をローカル インストールするには

1. AD FS フェデレーション サーバーに Azure Multi-Factor Authentication Server をダウンロードしてインストールします。 Azure Multi-factor Authentication server のインストールの詳細については、を参照してください [Azure Multi-factor Authentication Server の概要](multi-factor-authentication-get-started-server.md)
2. Azure Multi-factor Authentication Server ユーザー インターフェイスで、AD FS アイコンをクリックし、ユーザー登録の許可とユーザーの許可のオプションを選択し、方法を選択します。
3. その他のオプションがあれば、選択します。
4. [AD FS アダプターのインストール] をクリックします。
<center>![クラウド](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. コンピューターがドメインに参加しており、AD FS Adapter と Multi-Factor Authentication サービスの間の通信をセキュア保護するための Active Directory 構成が完了していない場合、Active Directory の手順が表示されます。 [次へ] ボタンをクリックしてこの構成を自動的に完了するか、[自動 Active Directory 構成をスキップして設定を手動で構成する] にチェックマークを付け、[次へ] をクリックします。
6. コンピューターがドメインに参加しておらず、AD FS Adapter と Multi-Factor Authentication サービスの間の通信をセキュア保護するためのローカル グループ構成が完了していない場合、ローカル グループの手順が表示されます。 [次へ] ボタンをクリックしてこの構成を自動的に完了するか、[自動ローカル グループ構成をスキップして設定を手動で構成する] にチェックマークを付け、[次へ] をクリックします。
7. これによりインストール ウィザードが開始します。[次へ] をクリックして Multi-Factor Authentication Server が PhoneFactor 管理グループを作成し、AD FS サービス アカウントを PhoneFactor 管理グループに追加できるようにします。
<center>![クラウド](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. [インストーラーの起動] の手順で [次へ] をクリックします。
9. Multi-Factor Authentication AD FS Adapter インストーラーで、[次へ] をクリックします。
10. インストールが完了したら、[閉じる] をクリックします。
11. アダプターがインストールされた後、AD FS に登録する必要があります。 Windows PowerShell を開き、次を実行します。 
    C:\Program \multi-factor Authentication server \register-multifactorauthenticationadfsadapter.ps1
   <center>![クラウド](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. ここで、新しく登録されたアダプターを使用するよう AD FS のグローバル認証ポリシーを編集する必要があります。 AD FS 管理コンソールで、認証ポリシー ノードに移動し、[Multi-factor Authentication] セクションで、[グローバル設定] サブセクションの隣の [編集] リンク をクリックします。 [グローバル認証ポリシーの編集] ウィンドウで、Multi-Factor Authentication を追加の認証方式として選択し、[OK] をクリックします。 アダプターが WindowsAzureMultiFactorAuthentication として登録されます。 登録を有効にするには、AD FS サービスを再起動する必要があります。

<center>![クラウド](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

この時点で、Multi-factor Authentication Server は、AD FS で使用する追加の認証プロバイダーとしてセットアップされます。

## Web サービス SDK を使用して AD FS Adapter をスタンドアロンでインストールするには

1. Multi-Factor Authentication Server を実行しているサーバーに Web Service SDK をインストールします。
2. MultiFactorAuthenticationAdfsAdapterSetup64.msi、Register-MultiFactorAuthenticationAdfsAdapter.ps1、Unregister-MultiFactorAuthenticationAdfsAdapter.ps1、および MultiFactorAuthenticationAdfsAdapter.config の各ファイルを \Program Files\Multi-Factor Authentication Server ディレクトリから、AD FS Adapter をインストールする予定のサーバーにコピーします。
3. MultiFactorAuthenticationAdfsAdapterSetup64.msi を実行します。
4. Multi-Factor Authentication AD FS Adapter インストーラーで、[次へ] をクリックし、インストールを実行します。
5. インストールが完了したら、[閉じる] ボタンをクリックします。
6. 次の操作を行って、MultiFactorAuthenticationAdfsAdapter.config ファイルを編集します。

 MultiFactorAuthenticationAdfsAdapter.config の手順| サブ手順
:------------- | :------------- |
 UseWebServiceSdk ノードを true に設定します。| |
 WebServiceSdkUrl をMulti-Factor Authentication Web サービス SDK の URL に設定します。| |
 オプション 1 - Web サービス SDK をユーザー名とパスワードを使って構成します。| <ol><li>WebServiceSdkUsername を PhoneFactor Admins セキュリティ グループのメンバーであるアカウントに設定します。Use <domain>\<username> format.<li>WebServiceSdkPassword を該当するアカウントのパスワードに設定します。</li></ol>
 オプション 2 - Web Service SDK をクライアントの証明書を使って構成します。| <ol><li>Web サービス SDK を実行しているサーバーの証明機関からクライアント証明書を取得します。</li><li>Web サービス SDK を実行しているサーバーのローカル コンピューターの個人証明書ストアにクライアント証明書をインポートします。注: 証明機関の公開証明書が信頼されたルート証明書を確認します。</li><li>クライアント証明書の公開および秘密キーを .pfx ファイルにエクスポートします</li>。<li>Base 64 形式の公開キーを .cer ファイルにエクスポートします</li>。<li>サーバー マネージャーでは、Web サーバー (IIS) \Web Server\Security\Client 証明書マッピング認証機能がインストールされていることを確認します</li>。<li>インストールされていない場合は、この機能を追加するには、役割の追加および機能を選択します</li>。<li>IIS マネージャーで、Web サービス SDK 仮想ディレクトリが存在する web サイトの構成エディター] をダブルクリックします。注: この非常に仮想ディレクトリ レベルではなく、web サイトのレベルで行う必要があります。</li><li>System.webServer/security/authentication/iisClientCertificateMappingAuthentication セクションに移動します</li>。<li>Enabled を true に設定します</li>。<li>OneToOneCertificateMappingsEnabled を true に設定します</li>。<li>] をクリックします... onetoonemappings の隣の横にあるボタンをクリックします</li>。<li>リンクの追加] をクリックします</li>。<li>開く前、base 64 形式の .cer ファイルにエクスポートします。-----BEGIN CERTIFICATE-----、-----END CERTIFICATE-----、および改行を削除します。結果の文字列をコピーします。</li><li>証明書文字列に設定が、前の手順でコピーします</li>。<li>Enabled を true に設定します</li>。<li>UserName を PhoneFactor Admins セキュリティ グループのメンバーであるアカウントを設定します。Use <domain>\<username> format.</li><li>、適切なアカウントのパスワードにパスワードを設定します</li>。<li>コレクション エディターを閉じます</li>。<li>適用] リンクをクリックします</li>。<li>Web サービス SDK 仮想ディレクトリに移動します</li>。<li>認証] をダブルクリックします</li>。<li>ASP.NET の偽装と基本認証を有効にされ、その他のすべての項目が無効になっていることを確認します</li>。<li>、Web サービス SDK 仮想ディレクトリに再度移動します</li>。<li>SSL 設定] をダブルクリックします</li>。<li>Accept にクライアント証明書の設定適用] をクリックします</li>。<li>コピー前、AD FS アダプターを実行するサーバーに .pfx ファイルにエクスポートします</li>。<li>.Pfx ファイルをローカル コンピューターの個人証明書ストアにインポートします</li>。<li>選択秘密キーの管理を右クリック メニューと許可の読み取りアクセスから別のユーザーとして上の Active Directory フェデレーション サービスのサービスのログオンがアカウントに</li><li>クライアント証明書を開き、[詳細] タブから拇印をコピー</li><li>で、MultiFactorAuthenticationAdfsAdapter.config ファイルを WebServiceSdkCertificateThumbprint 前の手順でコピーした文字列に設定します。</li></ol>
 -Configurationfilepath Register-multifactorauthenticationadfsadapter.ps1 スクリプトを編集 <path> コマンド Register-adfsauthenticationprovider の末尾に <path> 、MultiFactorAuthenticationAdfsAdapter.config ファイルへの完全なパスです。|


次に、\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 スクリプトを PowerShell で実行し、アダプターを登録します。 アダプターが WindowsAzureMultiFactorAuthentication として登録されます。 登録を有効にするには、AD FS サービスを再起動する必要があります。


























































































































































