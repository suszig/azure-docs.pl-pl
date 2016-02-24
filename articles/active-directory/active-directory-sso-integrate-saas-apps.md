<properties
    pageTitle="SaaS アプリと Azure Active Directory シングル サインオン統合 | Microsoft Azure"
    description="Azure Active Directory での SaaS アプリのシングル サインオン認証およびユーザー プロビジョニングによるアクセス管理の一元化を有効にします。 Azure Active Directory を SaaS アプリに統合する方法の概要について説明します。"
    services="active-directory"
      keywords="integrate Azure AD with SaaS apps"
    documentationCenter=""
    authors="curtand"
    manager="stevenpo"
    editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="12/01/2015"
      ms.author="curtand"/>

# SaaS アプリと Azure Active Directory シングル サインオンを統合する  

[AZURE.INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

組織に展開するアプリのためのシングル サインオンのセットアップ作業では、Azure Active Directory (Azure AD) 内にある既存のディレクトリを使用します。 Microsoft Azure、Office 365、または Windows Intune を通じて取得した Azure AD ディレクトリを使用できます。 これらの 2 つ以上の場合を参照してください。 [Azure AD ディレクトリの管理](active-directory-administer.md) どれを使用して決定します。

## 認証

プロトコル SAML 2.0、Ws-federation、または OpenID Connect をサポートしているアプリケーションは、Azure Active Directory を使用して署名証明書を信頼関係を確立できます。 詳細については、次を参照してください。 [でのシングル サインオン用の証明書を管理するフェデレーション](active-directory-sso-certs.md)します。

HTML フォーム ベースのサインインのみをサポートするアプリケーションの場合、Azure Active Directory では "パスワード保管" を使用して信頼関係を確立します。 これにより、Azure AD は SaaS アプリケーションから取得したユーザー アカウント情報を使用して、組織のユーザーを SaaS アプリケーションに自動的にサインインします。 ユーザー アカウント情報および関連するパスワードは Azure AD によって収集され、安全に格納されます。 詳細については、次を参照してください。 [シングル サインオンをパスワードに基づく](active-directory-appssoaccess-whatis.md\#password-based-single-sign-on)します。

## Authorization

プロビジョニングされたアカウントを使用すると、ユーザーがシングル サインオンを介して認証された後にアプリケーションの使用を許可できます。 ユーザーのプロビジョニングは手動で実行できます。また、Azure Active Directory で行われた変更に基づいて、SaaS アプリケーションに対してユーザー情報の追加や削除を実行できる場合もあります。 既存の Azure AD コネクタを使用して自動プロビジョニングの詳細については、次を参照してください  [自動ユーザー プロビジョニングと SaaS アプリケーションのプロビジョニング解除。](active-directory-saas-app-provisioning.md)

それ以外の場合は、アプリにユーザー情報を手動で追加するか、市販されている他のプロビジョニング ソリューションを使用できます。

## アクセス

Azure AD には、組織内のエンド ユーザーにアプリケーションをデプロイするためのカスタマイズ可能な方法が複数用意されています。 特定のデプロイ ソリューションやアクセス ソリューションに限定されているわけではありません。 使用する [ニーズに最適なソリューション](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users)します。

## 使用中のアプリケーションに関する追加の考慮事項

組織が既に使用しているアプリケーションにシングル サインオンをセットアップするプロセスは、新しいアプリケーションに新しいアカウントを作成するプロセスとは異なります。 いくつかの準備手順: アプリケーション内のユーザー ID を Azure AD の ID にマップする、統合後にユーザーがアプリケーションへのログインを経験する方法を把握する、など。

> [AZURE.NOTE] 既存のアプリケーションに対して SSO を設定するには両方の Azure AD でのグローバル管理者権限を持っている必要があり、SaaS アプリケーションです。

### ユーザー アカウントのマップ

ユーザー ID には、通常、一意の識別子が含まれます。それには電子メール アドレスまたは UPN (Universal Personal Name) が使用可能です。 ユーザーの各アプリケーション ID は、ユーザーの各 Azure AD ID にリンク (マップ) する必要があります。 アプリケーション認証の要件に応じて、これを実現する方法はいくつかあります。

Azure AD の id とアプリケーション id のマッピングの詳細については、次を参照してください。 [SAML トークンで発行されたクレームをカスタマイズする](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) と [プロビジョニング用の属性マッピングのカスタマイズ](active-directory-saas-customizing-attribute-mappings.md)します。

### ユーザーのログイン エクスペリエンスの理解

既に使用しているアプリケーションの SSO を統合するときは、ユーザー エクスペリエンスが影響を受けることを認識しておくことが重要です。 すべてのアプリケーションで、ユーザーは Azure AD の資格情報を使用してサインインすることになります。 また、アプリケーションにアクセスする際に別のポータルを使用することが必要な場合もあります。

インターフェイスでアプリケーションのサインインで一部のアプリケーションに対して SSO を実行できますが、他のアプリケーションなど、一元的なポータルを通過する、ユーザーは必要 ([My Apps](http://myapps.microsoft.com) または [office 365](http://portal.office.com/myapps)) にサインインします。 SSO とユーザー エクスペリエンスのさまざまな種類の詳細について [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン](active-directory-appssoaccess-whatis.md)します。

他の役に立つリソースは *ユーザーの同意を抑制する* で、 [Guiding 開発者](active-directory-applications-guiding-developers-for-lob-applications.md) 記事です。

## 次のステップ


アプリケーション ギャラリーで表示されている SaaS アプリケーションでは、Azure AD は、さまざまな [SaaS アプリを統合する方法に関するチュートリアル](active-directory-saas-tutorial-list.md)します。

アプリがアプリケーション ギャラリーにない場合を実行できます [カスタムとして Azure AD アプリ ギャラリーに追加
アプリケーション](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)します。

これらの問題、Azure.com ライブラリ内のすべての詳細があります。
始まる [アプリケーションへのアクセスと Azure Active Directory でのシングル サインオン。](active-directory-appssoaccess-whatis.md)します。

