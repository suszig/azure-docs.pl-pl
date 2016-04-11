<properties
   pageTitle="Azure Active Directory アプリケーション ギャラリーでのアプリケーションの表示"
   description="シングル サインオンをサポートするアプリケーションを Azure Active Directory ギャラリーに表示する方法 | Microsoft Azure"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/29/2015"
   ms.author="mbaldwin"/>


# Azure Active Directory アプリケーション ギャラリーでのアプリケーションの表示

Azure Active Directory でのシングル サインオンをサポートするアプリケーションを一覧表示する、 [Azure AD ギャラリー](http://azure.microsoft.com/marketplace/active-directory/all/), 、アプリケーションはまず、次の統合モードのいずれかを実装する必要があります。

* **OpenID Connect** の直接的な認証と Azure AD は、構成用 API を同意の OpenID Connect を使用して Azure AD と統合します。 統合を始めたばかりで、アプリケーションが SAML をサポートしていない場合、このモードが推奨されます。

* **SAML** – 既にアプリケーションが、SAML プロトコルを使用して、サード パーティ製 id プロバイダーを構成する機能です。

各モードの要件は次のとおりです。

##OpenID Connect 統合

次の Azure AD とアプリケーションを統合する、 [開発者指示](active-directory-authentication-scenarios.md)します。 次の項目を完了し、waadpartners@microsoft.com に送信します。

* Azure AD チームが統合のテストに使用できるアプリケーションのテスト テナントまたはアカウントの資格情報を記載してください。  

* Azure AD チームにサインインし、Azure AD のインスタンスを使用して、アプリケーションに接続する方法について説明します [Azure AD の同意フレームワーク](https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/#overview-of-the-consent-framework)します。 

* Azure AD チームがアプリケーションでシングル サインオンをテストするために必要な詳細な手順があれば、説明してください。 

* 次の情報を入力してください。

> 会社名:
> 
> 会社の Web サイト:
> 
> アプリケーション名:
> 
> アプリケーションの説明 (256 文字以内):
> 
> アプリケーションの Web サイト (情報):
> 
> アプリケーションのテクニカル サポートの Web サイト、または連絡先情報:
> 
> Https://manage.windowsazure.com で、アプリケーションの詳細に示すように、アプリケーションのクライアント ID:
> 
> 顧客がアプリケーションにサインアップしたり、アプリケーションを購入したりする際にアクセスする、アプリケーションのサインアップ URL:
> 
> アプリケーションを表示するカテゴリを最大 3 つ選択してください (使用できるカテゴリについては、Azure Active Directory Marketplace を参照してください)。
> 
> アプリケーションの小さいアイコン (PNG ファイル、45 x 45px、単色の背景色) を添付してください:
> 
> アプリケーションの大きいアイコン (PNG ファイル、215 x 215px、単色の背景色) を添付してください:
> 
> アプリケーションのロゴ (PNG ファイル、150 x 122px、透明の背景色) を添付してください:

##SAML の統合

SAML 2.0 をサポートするすべてのアプリを使用して Azure AD テナントと直接統合できます [カスタム アプリケーションを追加する手順](active-directory-saas-custom-apps.md)します。 Azure AD とアプリケーションの統合が機能する手順について、次の情報を送信 <waadpartners@microsoft.com>します。

* Azure AD チームが統合のテストに使用できるアプリケーションのテスト テナントまたはアカウントの資格情報を記載してください。  

* 説明に従って、アプリケーションの SAML サインオン URL、発行者の URL (エンティティ ID) と応答 URL (アサーション コンシューマー サービス) の値を提供 [ここ](active-directory-saas-custom-apps.md)します。 通常、SAML メタデータ ファイルの一部としてこれらの値を入力している場合は、それも送信してください。

* SAML 2.0 を使用してアプリケーションの ID プロバイダーとして Azure AD を構成する方法を簡単に説明してください。 セルフサービス管理ポータルで ID プロバイダーとしての Azure AD の構成がアプリケーションでサポートされている場合、前述の資格情報にそれを設定する機能を含めてください。

* 次の情報を入力してください。

> 会社名:
> 
> 会社の Web サイト:
> 
> アプリケーション名:
> 
> アプリケーションの説明 (256 文字以内):
> 
> アプリケーションの Web サイト (情報):
> 
> アプリケーションのテクニカル サポートの Web サイト、または連絡先情報:
> 
> 顧客がアプリケーションにサインアップしたり、アプリケーションを購入したりする際にアクセスする、アプリケーションのサインアップ URL:
> 
> アプリケーションを表示するカテゴリを最大 3 つのカテゴリを選択して (利用可能なカテゴリを参照してください、 [Azure Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/)))。
> 
> アプリケーションの小さいアイコン (PNG ファイル、45 x 45px、単色の背景色) を添付してください:
> 
> アプリケーションの大きいアイコン (PNG ファイル、215 x 215px、単色の背景色) を添付してください:
> 
> アプリケーションのロゴ (PNG ファイル、150 x 122px、透明の背景色) を添付してください:

