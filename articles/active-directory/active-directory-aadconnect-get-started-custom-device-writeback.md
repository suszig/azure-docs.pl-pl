<properties
    pageTitle="Azure AD Connect: デバイスの書き戻しの有効化 | Microsoft Azure"
    description="Azure AD Connect を使用するデバイスの書き戻しを有効にする方法について詳しく説明します"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="StevenPo"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/12/2015"
    ms.author="billmath;andkjell"/>


# Azure AD Connect でのデバイスの書き戻しの有効化

ここでは、Azure AD Connect においてデバイスの書き戻し機能を有効にする方法について説明します。 デバイスの書き戻しは、次のシナリオで使用されます。

AD FS (2012 R2 以降) で保護されたアプリケーション (証明書利用者の信頼) へのデバイスに基づく条件付きアクセスを有効にします。

これにより、セキュリティが強化され、アプリケーションへのアクセスが信頼されたデバイスに対してのみ許可されることが保証されます。 条件付きアクセスの詳細については、次を参照してください。 [条件付きアクセスによるリスク管理](active-directory-conditional-access.md) と [Azure Active Directory Device Registration を使用して内部設置型の条件付きアクセスを設定して](https://msdn.microsoft.com/library/azure/dn788908.aspx)します。
>[AZURE.NOTE] デバイスの書き戻しには、Azure AD Premium に対するサブスクリプションが必要です。


## パート 1: Azure AD Connect のインストール

1. カスタム設定または簡単設定を使用して Azure AD Connect をインストールします。 すべてのユーザーとグループの同期に成功してから、デバイスの書き戻しを有効にすることをお勧めします。

## パート 2: Active Directory の準備

デバイスの書き戻しの使用を準備するには、次の手順を使用します。

1.  Azure AD Connect がインストールされているコンピューターから、管理者特権モードで PowerShell を起動します。

2.  Active Directory PowerShell モジュールがインストールされていない場合。 次のコマンドを使用してインストールします。

    `Install-WindowsFeature –Name AD-DOMAIN-Services –IncludeManagementTools`

3.  エンタープライズ管理者の資格情報で次のコマンドを実行した後、PowerShell を終了します。

    `インポート モジュール 'C:\Program files \microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'`

    `Initialize ADSyncDeviceWriteback {省略可能: – DomainName [name] 省略可能:-AdConnectorAccount [アカウント]}`


![Powershell](./media/active-directory-aadconnect-get-started-custom-device-writeback/powershell.png)

説明:

- CN=Device Registration Configuration,CN=Services,CN=Configuration,[forest-dn] の下にコンテナーとオブジェクトが存在しない場合は、新しく作成されて構成されます。
- CN=RegisteredDevices,[domain-dn] の下にコンテナーとオブジェクトが存在しない場合は、新しく作成されて構成されます。 このコンテナーにデバイス オブジェクトが作成されます。
- Active Directory でデバイスを管理するために必要なアクセス許可を Azure AD コネクタ アカウントに設定します。
- Azure AD Connect が複数のフォレストにインストールされている場合であっても、1 つのフォレストで実行すれば十分です。

パラメーター:

- DomainName: デバイス オブジェクトが作成される Active Directory ドメイン。 注: 特定の Active Directory フォレストのデバイスはすべて、1 つのドメインに作成されます。
- AdConnectorAccount: ディレクトリ内のオブジェクトを管理するために Azure AD Connect によって使用される Active Directory アカウント。 Azure AD Connect Sync が AD に接続するために使用するアカウントです。 簡単設定を使用してインストールした場合、アカウントには MSOL_ というプレフィックスが付きます。

## パート 3: Azure AD Connect でのデバイスの書き戻しを有効にする

Azure AD Connect でデバイスの書き戻しを有効にするには、次の手順を使用します。

1.  インストール ウィザードをもう一度実行します。 選択 **同期オプションをカスタマイズする** 追加のタスクからページを **次**します。
![カスタム インストール](./media/active-directory-aadconnect-get-started-custom-device-writeback/devicewriteback2.png)
2.  [オプション機能] ページで、デバイスの書き戻しを変更できるようになります。 Azure AD Connect の準備手順が完了していない場合は、[オプション機能] ページでデバイスの書き戻しの設定を変更できないことに注意してください。 [デバイスの書き戻し] チェックボックスをオンにして、**[次へ]** をクリックします。 チェック ボックスがまだ無効にした場合を参照してください、 [トラブルシューティング](#the-writeback-checkbox-is-still-disabled)します。
![デバイスの書き戻し](./media/active-directory-aadconnect-get-started-custom-device-writeback/devicewriteback3.png)
3.  書き戻し] ページで、既定のデバイスの書き戻しのフォレストと指定されたドメインが表示されます。
![カスタム インストール](./media/active-directory-aadconnect-get-started-custom-device-writeback/devicewriteback4.png)
4.  その他の構成は変更しないで、ウィザードのインストールを完了します。 必要な場合、を参照してください [Azure AD Connect のカスタム インストールします。](active-directory-aadconnect-get-started-custom.md)。


## 条件付きアクセスを有効にする

このシナリオを有効にする詳細な手順については、内で [Azure Active Directory Device Registration を使用して内部設置型の条件付きアクセスを設定して](https://msdn.microsoft.com/library/azure/dn788908.aspx)します。

## デバイスが Active Directory に同期されていることを確認する

デバイスの書き戻しは正常に動作するようになっています。 デバイス オブジェクトを AD に書き戻すには、最大 3 時間かかる可能性があります。 デバイスが正しく同期されていることを確認するには、同期規則が完了した後で次のようにします。

1.  Active Directory 管理センターを起動します。
2.  フェデレーションされているドメイン内の RegisteredDevices を展開します。
![カスタム インストール](./media/active-directory-aadconnect-get-started-custom-device-writeback/devicewriteback5.png)
3.  現在登録されているデバイスが一覧表示されます。

![カスタム インストール](./media/active-directory-aadconnect-get-started-custom-device-writeback/devicewriteback6.png)

## トラブルシューティング

### 書き戻しのチェックボックスがオフのままです。

前述の手順を実行しても、デバイスの書き戻しのチェックボックスがオンにならない場合は、次の手順を実行し、ボックスがオンになる前にインストール ウィザードで検証が実行されるようにします。

まず次の手順を実行します。

- 少なくとも 1 つのフォレストに Windows Server 2012R2 があることを確認します。 このデバイス オブジェクトの種類が必要です。
- インストール ウィザードが既に実行中の場合、変更があっても検出されません。 この場合は、インストール ウィザードを完了してから、再実行してください。
- 初期化スクリプトで指定したアカウントが、Active Directory Connector に使用されている正しいユーザーであることを確認します。 確認する手順は次のとおりです。
    - [スタート] メニューから **[同期サービス]** を開きます。
    - **[コネクタ]** タブを開きます。
    - 種類が Active Directory ドメイン サービスのコネクタを探して選択します。
    - **[アクション]** の **[プロパティ]** を選択します。
    - **[Active Directory フォレストに接続]** を選択します。 ドメインとユーザー名を指定画面照合結果をスクリプトに指定されたアカウントを確認します。
![Connector アカウント](./media/active-directory-aadconnect-get-started-custom-device-writeback/connectoraccount.png)

Active Directory の構成を確認します。
- デバイス登録サービスを次の場所であることを確認 (CN DeviceRegistrationService、CN = Device Registration Services、CN = デバイスの登録の構成、CN = Services, CN = = 構成) 構成名前付けコンテキストの下にあります。

![トラブルシューティング 1](./media/active-directory-aadconnect-get-started-custom-device-writeback/troubleshoot1.png)

- configuration 名前空間を検索して、configuration オブジェクトが 1 つのみであることを確認します。 複数ある場合は、重複するオブジェクトを削除します。

![トラブルシューティング 2](./media/active-directory-aadconnect-get-started-custom-device-writeback/troubleshoot2.png)

- Device Registration Service オブジェクトに、属性 msDS-DeviceLocation が存在し、値が指定されていることを確認します。 その場所を参照し、その場所と objectType msDS-DeviceContainer が存在することを確認します。

![トラブルシューティング 3](./media/active-directory-aadconnect-get-started-custom-device-writeback/troubleshoot3.png)

![トラブルシューティング 4](./media/active-directory-aadconnect-get-started-custom-device-writeback/troubleshoot4.png)

- Active Directory Connector には、前の手順で検索した Registered Devices コンテナーに対するアクセス許可が必要です。 このコンテナーには、次のようなアクセス許可があります。

![トラブルシューティング 5](./media/active-directory-aadconnect-get-started-custom-device-writeback/troubleshoot5.png)

- Active Directory アカウントに、CN=Device Registration Configuration,CN=Services,CN=Configuration オブジェクトに対するアクセス許可があることを確認します。

![トラブルシューティング 6](./media/active-directory-aadconnect-get-started-custom-device-writeback/troubleshoot6.png)

## 追加情報

- [条件付きアクセスとリスクを管理します。](active-directory-conditional-access.md)
- [Azure Active Directory Device Registration を使用して内部設置型の条件付きアクセスを設定します。](https://msdn.microsoft.com/library/azure/dn788908.aspx)

## 次のステップ

詳細について [内部設置型 id と Azure Active Directory の統合](active-directory-aadconnect.md)します。





