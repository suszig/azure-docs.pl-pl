<properties
    pageTitle="Azure Active Directory への Windows ドメイン参加済みデバイスの自動デバイス登録 | Microsoft Azure"
    description="IT 管理者は、ドメイン参加済みの Windows デバイスをサイレント モードで自動的に Azure Active Directory (Azure AD) に登録できます。"
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/24/2015"
    ms.author="femila"/>

# Azure Active Directory への Windows ドメイン参加済みデバイスの自動デバイス登録

IT 管理者は、ドメイン参加済みの Windows デバイスをサイレント モードで自動的に Azure Active Directory (Azure AD) に登録できます。 これは、デバイスに基づいて条件付きでアクセスを許可するポリシーを Office 365 アプリケーションまたは AD FS によってオンプレミスで管理されているアプリケーションに対して構成している場合に役立ちます。 学べるデバイス登録シナリオの詳細を読み取って、 [Azure Active Directory デバイス登録概要](active-directory-conditional-access-device-registration-overview.md)します。

Azure Active Directory への自動デバイス登録は、Active Directory ドメインに参加している Windows 7 および Windows 8.1 のマシンに使用できます。 通常、これらはインフォメーション ワーカーに提供されている、企業が所有するマシンです。

ドメイン参加済みの Windows デバイスの Azure AD への登録を開始するには、次の前提条件に従います。 前提条件を満たしたら、ドメイン参加済みの Windows デバイスの自動デバイス登録を構成します。

## ドメイン参加済みの Windows デバイスを Azure Active Directory に自動デバイス登録するための前提条件

Azure Active Directory Connect を使用して、AD FS をデプロイして Azure Active Directory に接続する
----------------------------------------------------------------------------------------------
1. Azure Active Directory Connect を使用して Active Directory フェデレーション サービス (AD FS) を Windows Server 2012 R2 にデプロイし、Azure Active Directory とのフェデレーション関係をセットアップします。
2. 追加の Azure Active Directory 証明書利用者信頼の要求規則を構成する
3. AD FS 管理コンソールを開きに移動 **AD FS**>**信頼関係 > 証明書利用者信頼**します。 Microsoft Office 365 Id プラットフォーム証明書利用者のパーティの信頼オブジェクトを右クリックし、選択 **要求規則の編集.**
4.  **発行変換規則** ] タブで [ **ルールの追加**します。
5. 選択 **カスタム規則を使用して要求を送信** から、 **要求規則** テンプレートのドロップ ダウン ボックス。 選択 **次**します。
6. 型 *Auth Method Claim Rule* で、 **要求規則名:** テキスト ボックスです。
7. 次の要求規則の入力、 **要求規則:** テキスト ボックス。

        c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
        => issue(claim = c);
    
8. クリックして **OK** ] ダイアログ ボックスに 2 回クリックします。

追加の Azure Active Directory 証明書利用者信頼の認証クラス参照を構成する
-----------------------------------------------------------------------------------------------------
フェデレーション サーバーで、Windows PowerShell コマンド ウィンドウを開き、次のように入力します。


  `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`
   
Where <RPObjectName> 信頼オブジェクトを利用する Azure Active directory 証明書利用者のパーティ オブジェクト名です。 通常、このオブジェクトは "Microsoft Office 365 ID プラットフォーム" という名前です。

AD FS グローバル認証ポリシー
-----------------------------------------------------------------------------
イントラネットで Windows 統合認証を許可するように AD FS グローバル プライマリ認証ポリシーを構成します (これは既定の構成です)。


Internet Explorer の構成
------------------------------------------------------------------------------
Windows デバイスで、Internet Explorer のローカル イントラネットのセキュリティ ゾーンに対して、次の設定を構成します。

- 唯一の証明書が存在する場合にクライアント証明書選択メッセージを表示しない:  **を有効にします。**
- スクリプティングを使用する:  **を有効にします。**
- イントラネット ゾーンでのみ自動ログオン:  **チェック**

これらは、Internet Explorer のローカル イントラネットのセキュリティ ゾーンの既定の設定です。 表示したり、Internet Explorer でこれらの設定の管理に移動して **インターネット オプション** > **セキュリティ** > イントラネット > レベルのカスタマイズ。 また、Active Directory グループ ポリシーを使用して、これらの設定を構成することもできます。

ネットワーク接続
-------------------------------------------------------------
ドメイン参加済みの Windows デバイスを Azure AD に自動的に登録するには、これらのデバイスが AD FS および Active Directory ドメイン コントローラーに接続されている必要があります。 このことは、一般的には、マシンが企業ネットワークに接続されている必要があることを意味します。 この接続には、ワイヤード (有線) 接続、Wi-Fi 接続、DirectAccess、または VPN を使用できます。

## Azure Active Directory Device Registration の検出を構成する
Windows 7 デバイスおよび Windows 8.1 デバイスでは、ユーザー アカウント名と既知のデバイス登録サーバー名を組み合わせた、デバイス登録サーバーが検出されます。 Azure Active Directory Device Registration サービスに関連付けられた A レコードを参照する DNS CNAME レコードを作成する必要があります。 CNAME レコードは、既知のプレフィックスを使用する必要があります **enterpriseregistration** 後に、組織のユーザー アカウントで使用される UPN のサフィックスです。 組織で複数の UPN サフィックスを使用している場合は、DNS に複数の CNAME レコードを作成する必要があります。

たとえば、組織で @contoso.com と @region.contoso.com の 2 つの UPN サフィックスを使用している場合は、次の DNS レコードを作成します。

| エントリ                                     | 型  | Address                            |
|-------------------------------------------|-------|------------------------------------|
| enterpriseregistration.contoso.com        | CNAME | enterpriseregistration.windows.net |
| enterpriseregistration.region.contoso.com | CNAME | enterpriseregistration.windows.net |

##Windows 7 および Windows 8.1 のドメイン参加済みデバイスの自動デバイス登録を構成する

以下のリンクを参照して、Windows 7 および Windows 8.1 のドメイン参加済みデバイスの自動デバイス登録を構成します。 続行する前に、上記の前提条件が満たされているようにしてください。

* [Windows 8.1 ドメイン参加済みデバイスの自動デバイス登録の構成](active-directory-conditional-access-automatic-device-registration-windows8_1.md)

* [Windows 7 ドメイン参加済みデバイスの自動デバイス登録の構成](active-directory-conditional-access-automatic-device-registration-windows7.md)

その他のメモ
--------------------------------------------------------------------

Azure AD へのデバイス登録により、広範なデバイスの機能が提供されます。 Azure AD Device Registration を使用すると、個人 (BYOD) のモバイル デバイスと、企業が所有するドメイン参加済みデバイスの両方を登録できます。 これらのデバイスは、Office 365 などのホストされるサービスと、AD FS によってオンプレミスで管理されているサービスの両方で使用できます。 

モバイル デバイスと従来型デバイスの両方を使用する企業、または Office 365 や Azure AD などの Microsoft サービスを利用する企業は、Azure AD Device Registration サービスを使用して、デバイスを Azure AD に登録する必要があります。企業でモバイル デバイスや Office 365、Azure AD、Microsoft Intune などの Microsoft サービスを利用せずに、代わりにオンプレミス アプリケーションのみをホストする場合、AD FS を使用してデバイスを Active Directory に登録することができます。 

デバイスの登録と AD FS を展開する方法について学習できる [ここ](https://technet.microsoft.com/library/dn486831.aspx)します。

## 関連トピック

- [Azure Active Directory Device Registration の概要](active-directory-conditional-access-device-registration-overview.md)
- [Windows 7 ドメイン参加済みデバイスの自動デバイス登録の構成](active-directory-conditional-access-automatic-device-registration-windows7.md)
- [Windows 8.1 ドメイン参加済みデバイスの自動デバイス登録の構成](active-directory-conditional-access-automatic-device-registration-windows8_1.md)
