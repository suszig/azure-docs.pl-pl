<properties
   pageTitle="Azure AD Connect: 前提条件とハードウェア |Microsoft Azure"
   description="ランディング ページやほとんどの検索結果に表示されるアーティクルの説明"
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="stevenpo"
   editor="curtand"/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="11/16/2015"
   ms.author="andkjell;billmath"/>

# Azure AD Connect の前提条件
このトピックでは、Azure AD Connect を使用するための前提条件とハードウェア要件について説明します。

## Azure AD Connect をインストールする前に
Azure AD Connect をインストールする前に、いくつか必要な項目があります。

**Azure AD**

- Azure サブスクリプションまたは [Azure 試用版サブスクリプション](http://azure.microsoft.com/pricing/free-trial/)します。 Azure ポータルへのアクセスにのみ必要です。Azure AD Connect の使用には不要です。  PowerShell または Office 365 を使用している場合は、Azure サブスクリプションがなくても Azure AD Connect を使用できます。 Office 365 ライセンスを持っている場合は、Office 365 ポータルも使用できます。 有料の Office 365 ライセンスを使用して、Office 365 ポータルから Azure ポータルにアクセスすることもできます。
- [追加して、ドメインの確認](active-directory-add-domain.md) Azure AD で使用する予定です。 たとえばユーザー向けに contoso.com を使用する予定の場合は、そのドメインが検証されていること、および使用しているのドメインが既定のドメインである contoso.onmicrosoft.com だけではないことを確認します。
- Azure AD ディレクトリでは、既定で 50,000個のオブジェクトが許可されます。 ドメインを検証すると、制限が 300,000 個のオブジェクトに増加します。 Azure AD でさらに多くのオブジェクトが必要な場合は、制限をさらに増加させるサポート ケースを開く必要があります。 500,000 個を超えるオブジェクトが必要な場合は、Office 365、Azure AD Basic、Azure AD Premium、または Enterprise Mobility Suite などのライセンスが必要です。

**オンプレミスのサーバーと環境**

- AD スキーマのバージョンとフォレストの機能レベルは、Windows Server 2003 以降である必要があります。 ドメイン コントローラーは、スキーマとフォレスト レベルの要件を満たしていれば、任意のバージョンを実行できます。
- 機能を使用する予定の場合 **パスワード ライトバック** (最新の SP) で Windows Server 2008 以降のドメイン コント ローラーがあります。
- Small Business Server または Windows Server Essentials には、Azure AD Connect をインストールできません。 サーバーは Windows Server Standard 以上を使用する必要があります。
- Azure AD Connect は、Windows Server 2008 以降にインストールする必要があります。  このサーバーをドメイン コントローラーにすることができます。Express 設定を使用する場合はメンバー サーバーにすることもできます。 カスタム設定を使用する場合、サーバーはスタンドアロンにすることもでき、ドメインに参加する必要はありません。
- Azure AD Connect を Windows Server 2008 にインストールする場合は、Windows Update から最新の修正プログラムが適用されていることを確認してください。 修正プログラムが適用されていないサーバーでインストールを開始することはできません。
- 機能を使用する予定の場合 **パスワード同期**, 、Azure AD Connect サーバーが Windows Server 2008 R2 SP1 またはそれ以降にする必要があります。
- Azure AD Connect サーバーが必要 [.Net 4.5.1](#component-prerequisites) またはそれ以降、 [PowerShell 3.0](#component-prerequisites) 以降がインストールされています。
- Active Directory Federation Services をデプロイする場合、AD FS または Web アプリケーション プロキシがインストールされるサーバーは、Windows Server 2012 R2 以降である必要があります。 [Windows リモート管理](#windows-remote-management) リモート インストールの場合、これらのサーバーで有効にする必要があります。
- Active Directory フェデレーション サービスが展開されている場合は済みます [SSL 証明書](#ssl-certificate-requirements)します。
- Azure AD Connect には、ID データを格納する SQL Server データベースが必要です。 既定では、SQL Server 2012 Express LocalDB (SQL Server Express の簡易バージョン) がインストールされ、サービスのサービス アカウントがローカル コンピューターに作成されます。 SQL Server Express のサイズ制限は 10 GB で、約 100,000 オブジェクトを管理できます。 さらに多くのディレクトリ オブジェクトを管理する必要がある場合は、インストール プロセスで別のバージョンの SQL Server を指定する必要があります。 Azure AD Connect では、SQL Server 2008 (SP4) から SQL Server 2014 まで、すべてのエディションの Microsoft SQL Server がサポートされています。

**アカウント**

- 統合する Azure AD ディレクトリの Azure AD グローバル管理者アカウント。
- Express 設定を使用する、または DirSync からアップグレードする場合は、ローカルの Active Directory のエンタープライズ管理者アカウント。
- [アカウントが Active Directory](active-directory-aadconnect-accounts-permissions.md) カスタム設定のインストール パスを使用する場合。

**接続**

- 次の設定、インターネットに接続するため、送信プロキシを使用している場合、 **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** インストール ウィザードと Azure AD は、インターネットおよび Azure AD に接続できる同期ファイルを追加する必要があります。 このテキストは、ファイルの末尾に入力する必要があります。  このコードの &lt;PROXYADRESS&gt; は実際のプロキシ IP アドレスまたはホスト名を表します。

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

プロキシ サーバーで認証が必要な場合、セクションは次のようになります。

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

この machine.config の変更によって、インストール ウィザードと同期エンジンは、プロキシ サーバーからの認証要求に応答します。 除くすべてのインストール ウィザード ページで、 **構成** ページの符号付きで、ユーザーの資格情報を使用します。  **構成** コンテキストで、インストール ウィザードの最後のページに切り替えられます、 [サービス アカウント](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) 作成対象となった。

の詳細については MSDN を参照して、 [既定のプロキシ要素](https://msdn.microsoft.com/library/kd3cf2ex.aspx)します。

- ある、プロキシとなる Url を制限されている場合に記載されている Url にアクセスし、 [Office 365 の Url および IP アドレス範囲 ](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) プロキシで開く必要があります。

**その他**

- 省略可能: 同期を検証するテスト ユーザー アカウント。

## コンポーネントの前提条件

Azure AD Connect は、PowerShell と .Net 4.5.1 に依存しています。 Windows Server のバージョンに応じて、次の操作を行います。

- Windows Server 2012R2
  - PowerShell は既定でインストールされているため、操作は必要ありません。
  - .Net 4.5.1 以降のリリースは、Windows Update によって提供されます。 コントロール パネルで、Windows Server に最新の更新プログラムがインストールされていることを確認します。
- Windows Server 2008R2 と Windows Server 2012
  - PowerShell の最新バージョンが含まれている **Windows Management Framework 4.0**, で利用できる [Microsoft ダウンロード センター](http://www.microsoft.com/downloads)します。
  - .Net 4.5.1 および今後のリリースで使用可能な [Microsoft ダウンロード センター](http://www.microsoft.com/downloads)します。
- Windows Server 2008
  - PowerShell のサポートされる最新のバージョンが含まれている **Windows Management Framework 3.0**, で利用できる [Microsoft ダウンロード センター](http://www.microsoft.com/downloads)します。
 - .Net 4.5.1 および今後のリリースで使用可能な [Microsoft ダウンロード センター](http://www.microsoft.com/downloads)します。

## Windows リモート管理

 Azure AD Connect を使用して Active Directory フェデレーション サービスまたは Web アプリケーション プロキシをデプロイする場合、接続と構成が正常に完了するように、以下の要件を確認します。

 - ターゲット サーバーがドメインに参加している場合は、Windows リモート管理が有効であることを確認します。
    - 管理者特権の PSH コマンド ウィンドウで、`Enable-PSRemoting –force` コマンドを使用します。
 - ターゲット サーバーが、ドメインに参加していない WAP コンピューターである場合は、いくつかの追加の要件があります。
    - ターゲット コンピューター (WAP コンピューター) での要件
         - サービス スナップインから winrm (Windows Remote Management / WS-Management) サービスが実行されていることを確認します。
         - 管理者特権の PSH コマンド ウィンドウで、`Enable-PSRemoting –force` コマンドを使用します。
    - ウィザードを実行しているコンピューターでの要件 (ターゲット コンピューターがドメインに参加していないか、信頼されていないドメインにある場合)
        - 管理者特権の PSH コマンド ウィンドウで、`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate` コマンドを使用します。
        - サーバー マネージャーでの要件
             - DMZ WAP ホストをコンピューターのプールに追加します ([サーバー マネージャー]、[管理]、[サーバーの追加] の順にクリックし、[DNS] タブを使用)。
             - サーバー マネージャーの [すべてのサーバー] タブで、 WAP サーバーを右クリックし、[管理に使用する資格情報] を選択し、WAP コンピューターのローカルの資格情報 (ドメインの資格情報ではない) を入力します。
             - リモートの PSH 接続を検証するには、サーバー マネージャーの [すべてのサーバー] タブで WAP サーバーを右クリックし、[Windows PowerShell] を選択します。  リモート PSH セッションが開き、リモート PowerShell セッションを確立できます。

## SSL 証明書の要件

**重要:** が、AD FS ファームのすべてのノードだけではなく、すべての Web アプリケーション プロキシ サーバーの間で同じ SSL 証明書を使用してに強くお勧めします。

- この証明書は x509 証明書である必要があります。
- テスト ラボ環境では、フェデレーション サーバーで自己署名証明書を使用できます。 ただし、運用環境では、パブリック CA から証明書を取得することを勧めします。
    - 公的に信頼されていない証明書を使用する場合は、各 Web アプリケーション プロキシ サーバーにインストールされている証明書がローカル サーバーとすべてのフェデレーション サーバーで信頼されていることを確認します。
- 証明書の ID は、フェデレーション サービス名 (fs.contoso.com など) と一致する必要があります。
    - ID は、dNSName タイプのサブジェクト代替名 (SAN) 拡張、または SAN エントリがない場合は共通名として指定されたサブジェクト名のどちらかになります。  
    - 複数の SAN エントリを証明書に表示できますが、そのうちの 1 つはフェデレーション サービス名に一致させます。
    - ワークプ レース ジョインを使用しようとする場合の SAN が値を持つ必要 **enterpriseregistration します。** 後に、組織のユーザー プリンシパル名 (UPN) サフィックス **enterpriseregistration.contoso.com**します。
- CryptoAPI Next Generation (CNG) キーとキー記憶域プロバイダーに基づく証明書はサポートされません。 つまり、KSP (キー記憶域プロバイダー) ではなく CSP (暗号化サービス プロバイダー) に基づく証明書を使用する必要があります。
- ワイルドカード証明書がサポートされます。

## Azure AD Connect でサポートされるコンポーネント

Azure AD Connect によって Azure AD Connect のインストール先にインストールされるコンポーネントの一覧を次に示します。 この一覧は、基本的な高速インストール用です。  [同期サービスのインストール] ページで異なる SQL Server を使用することを選択した場合、SQL Express LocalDB はローカルにインストールされません。

- Microsoft SQL Server 2012 のコマンド ライン ユーティリティ
- Microsoft SQL Server 2012 Native Client
- Microsoft SQL Server 2012 Express LocalDB
- Windows PowerShell 用の Azure Active Directory モジュール
- IT プロフェッショナル向け Microsoft Online Services サインイン アシスタント
- Microsoft Visual C++ 2013 再配布パッケージ


## Azure AD Connect のハードウェア要件
次の表は、Azure AD Connect Sync コンピューターの最小要件を示しています。

| Active Directory 内のオブジェクトの数 | CPU | メモリ | ハード ドライブのサイズ |
| ------------------------------------- | --- | ------ | --------------- |
| 10,000 未満 | 1.6 GHz | 4 GB | 70 GB |
| 10,000 ～ 50,000 | 1.6 GHz | 4 GB | 70 GB |
| 50,000 ～ 100,000 | 1.6 GHz | 16 GB | 100 GB |
| オブジェクトが 100,000 個以上の場合は完全バージョンの SQL Server が必要|  |  |  |
| 100,000 ～ 300,000 | 1.6 GHz | 32 GB | 300 GB |
| 300,000 ～ 600,000 | 1.6 GHz | 32 GB | 450 GB |
| 600,000 を超過 | 1.6 GHz | 32 GB | 500 GB |

AD FS または Web アプリケーション サーバーを実行するコンピューターの最小要件を次に示します。

- CPU: デュアル コア 1.6 GHz 以上
- メモリ: 2 GB 以上
- Azure VM: A2 構成またはそれ以上


## 次のステップ
詳細について [内部設置型 id と Azure Active Directory の統合](active-directory-aadconnect.md)します。


