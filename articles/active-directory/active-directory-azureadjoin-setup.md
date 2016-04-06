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
3.  **ディレクトリ** ] タブで、ディレクトリを選択します。
4. 選択、 **構成** ] タブをクリックします。
5. 呼ばれるセクションまでスクロール **デバイス**します。
6.  **デバイス** ] タブで、次の設定。  
   * **ユーザーごとのデバイスの最大数**: Azure AD でユーザーがいないデバイスの最大数を選択します。  ユーザーがこのクォータに達した場合、1 つ以上の既存のデバイスを削除するまでデバイスを追加できなくなります。
   * **デバイスに参加する多要素認証を必要と**: ユーザーが自分のデバイスを Azure AD に参加する認証の 2 つ目の要素を提供する必要があるときに有効にします。 多要素認証の詳細については、次を参照してください [クラウドでの Azure Multi-factor Authentication の概要。](multi-factor-authentication-get-started-cloud/)
   * **ユーザーが Azure AD 参加デバイス**: ユーザーおよびデバイスを Azure AD に参加できるはグループを選択します。
   * **Azure AD 参加済みデバイスで追加の管理者**: Azure AD Premium または Enterprise Mobility Suite (EMS) では、デバイスにローカルの管理者権限を付与するユーザーを選択できます。 全体管理者とデバイスの所有者には、既定でローカル管理者権限が付与されます。

<center>![](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png) </center>
 
ユーザーの Azure AD 参加を設定すると、ユーザーは、会社のデバイスまたは個人のデバイスから Azure AD に接続できます。 

Azure AD 参加をユーザーが設定できるようにする 3 つのシナリオを次に示します。

- ユーザーが会社所有のデバイスを直接 Azure AD に参加させる。
- ユーザーが、会社所有のデバイスをオンプレミスの Active Directory にドメイン参加させ、Azure AD に拡張する。
- ユーザーが個人のデバイス上の Windows に仕事用アカウントを追加する。 

## 追加情報
* [エンタープライズ向け Windows 10: デバイスを仕事に使用する方法](active-directory-azureadjoin-windows10-devices-overview.md)
* [Azure Active Directory 参加を使用したクラウド機能の Windows 10 デバイスへの拡張](active-directory-azureadjoin-user-upgrade.md)
* [Azure AD 参加の使用シナリオについて](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Windows 10 エクスペリエンスのためにドメイン参加済みデバイスを Azure AD に接続する](active-directory-azureadjoin-devices-group-policy.md)
* [Azure AD 参加の設定](active-directory-azureadjoin-setup.md)




