<properties 
    pageTitle="ユーザーの Azure AD 参加の設定 | Microsoft Azure" 
    description="管理者が、オンプレミス ディレクトリとデバイス登録のために Azure AD 参加を設定する方法について説明します。"
    services="active-directory" 
    documentationCenter="" 
    authors="femila" 
    manager="stevenpo" 
    editor=""
    tags="azure-classic-portal"/>

<tags 
    ms.service="active-directory" 
    ms.workload="identity" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/19/2015" 
    ms.author="femila"/>


# 組織での Azure AD 参加の設定

Azure AD 参加を設定する前に、ユーザーのオンプレミスのディレクトリをクラウドに同期するか、Azure AD に管理アカウントを手動で作成する必要があります。

詳細については、内部設置型のユーザーを Azure AD を同期するための手順 [内部設置型 id と Azure Active Directory の統合](active-directory-aadconnect.md)します。


参照して手動で作成し、Azure AD でユーザーを管理、 [Azure AD でユーザー管理](https://msdn.microsoft.com/library/azure/hh967609.aspx)します。

## デバイスの登録の設定

1. Azure ポータルに管理者としてログオンします。
2. 左ウィンドウで、[Active Directory] を選択します。
3. **[ディレクトリ]** タブで、ディレクトリを選択します。
4. **[構成]** タブをクリックします。
5. **[デバイス]** セクションまでスクロールします。
6. **[デバイス]**タブで、次のように設定します。
   * **[ユーザーあたりのデバイスの最大数]**: Azure AD でユーザーが持つことができるデバイスの最大数を選択します。 ユーザーがこのクォータに達した場合、1 つ以上の既存のデバイスを削除するまでデバイスを追加できなくなります。
   * **[デバイスを参加させるには Multi-factor Auth が必要]**: 有効にした場合、ユーザーはデバイスを Azure AD に参加させるために 2 つ目の認証要素を提供する必要があります。 多要素認証の詳細については、を参照してください [クラウドでの Azure Multi-factor Authentication の概要](multi-factor-authentication-get-started-cloud/)。
   * **[ユーザーはデバイスの Azure AD 参加を実行できます]**: デバイスを Azure AD に参加させることができるユーザーとグループを選択します。
   * **[Azure AD 参加済みデバイスの追加の管理者]**: Azure AD Premium または Enterprise Mobility Suite (EMS) では、デバイスに対するローカル管理者権限が許可されるユーザーを選択できます。 全体管理者とデバイスの所有者には、既定でローカル管理者権限が付与されます。

<center>![](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png) </center>

ユーザーの Azure AD 参加を設定すると、ユーザーは、会社のデバイスまたは個人のデバイスから Azure AD に接続できます。

Azure AD 参加をユーザーが設定できるようにする 3 つのシナリオを次に示します。

- ユーザーが会社所有のデバイスを直接 Azure AD に参加させる。
- ユーザーが、会社所有のデバイスをオンプレミスの Active Directory にドメイン参加させ、Azure AD に拡張する。
- ユーザーが個人のデバイス上の Windows に仕事用アカウントを追加する。

## 追加情報

* [企業向け Windows 10: 作業用デバイスを使用する方法](active-directory-azureadjoin-windows10-devices-overview.md)
* [クラウドの機能を Azure Active Directory 参加を使用して Windows 10 デバイスへの拡張](active-directory-azureadjoin-user-upgrade.md)
* [Azure AD 参加の使用シナリオについてください。](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [ドメインに参加しているデバイスを Windows 10 エクスペリエンス用に Azure AD に接続します。](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD 参加の設定します。](active-directory-azureadjoin-setup.md)







