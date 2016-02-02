<properties
    pageTitle="Azure AD を使用してアカウントを共有 | Microsoft Azure"
    description="Azure Active Directory を使用してオンプレミスのアプリケーションおよびコンシューマー クラウド サービス用のアカウントを組織で安全に共有できるようにする方法について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="msStevenPo"
    manager="stevenpo"
    editor=""/>

 <tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/16/2015"  
    ms.author="stevenpo"/>


# Azure AD とのアカウントの共有

## 概要

組織では、複数のユーザーに対して 1 組のユーザー名とパスワードを使用することが必要な場合があります。 このような状況は、通常、次の 2 つのケースで発生します。

- オンプレミスのアプリケーションであろうと、コンシューマー クラウド サービス (企業のソーシャル メディア アカウントなど) であろうと、ユーザーごとに一意のログインおよびパスワードを必要とするアプリケーションにアクセスする場合。
- マルチユーザー環境を作成する場合。 昇格された特権を有し中心となるセットアップ、管理、および回復アクティビティに使用できる単一のローカル アカウントを持つことができます (たとえば、Office 365 のローカル「グローバル管理者」アカウントまたは Salesforce のルート アカウント)。

これまで、このようなアカウントを共有するためには、権限のあるユーザーに資格情報 (ユーザー名/パスワード) を配布するか、または信頼済みの複数のエージェントがアクセスできる共有の場所に当該アカウントを格納するということを行ってきました。

従来の共有のモデルには、以下に示すようないくつかの欠点があります。

- 新しいアプリケーションへのアクセスを有効にするには、アクセスを必要としているすべてのユーザーに資格情報を配布する必要があります。
- 各共有アプリケーションでそれぞれ固有の共有資格情報セットを必要とすることがあります。このような場合、ユーザーは複数の資格情報セットを覚えておく必要があります。 ユーザーが多くの資格情報を保存する場合、危険なプラクティスに並び替えはそれらのリスクが高まります。 (例: パスワードを書き留める)。
- どのユーザーがアプリケーションにアクセスできるのかはっきりしません。
- アプリケーションに*アクセスした*ユーザーをはっきりさせることができません。
- アプリケーションへのアクセス権を削除する必要がある場合は、資格情報を更新して、アプリケーションへのアクセスを必要としているすべてのユーザーに再配布する必要があります。

## Azure Active Directory アカウントの共有

Azure AD では、共有のアカウントを使用する上で、上記の欠点を排除した新しい方法を提供します。

Azure AD 管理者は、アクセス パネルを使用して、ユーザーがどのアプリケーションにアクセスできるかを構成し、そのアプリケーションに最適なシングル サインオンの種類を選択します。 その中の 1 つである*パスワードベースのシングル サインオン*を選択した場合、Azure AD はアプリケーションに対するサインオン プロセス中に一種のブローカーとしての役割を果たします。

ユーザーは組織アカウントを使用して 1 度、ログインします。 このアカウントは、ユーザーがデスクトップまたは電子メールにアクセスするためによく使用するのと同じものです。 ユーザーは自分が割り当てられているアプリケーションだけを検出し、アクセスすることができます。 共有アカウントの場合は、このアプリケーション リストに任意の数の共有資格情報を含めることができます。 エンド ユーザーは、使用する可能性があるさまざまなアカウントを覚えることも、書き留めておくことも必要ありません。

共有アカウントは、管理作業の強化、使いやすさの向上だけでなく、セキュリティも強化します。 資格情報の使用権限を持つユーザーには、共有パスワードが表示されるのではなく、パスワードを調整された認証フローの一部として使用する権限が与えられます。 さらに、一部のパスワード SSO アプリケーションの場合、大規模で複雑なパスワードを使用して Azure AD に定期的にパスワードをロールオーバー (更新) させて、アカウントのセキュリティを強化するオプションもあります。 管理者は、アプリケーションへのアクセス権の付与または取り消しを簡単に行うことができ、また、アカウントへのアクセス権を有するユーザーおよび過去にアプリケーションにアクセスしたユーザーを把握できます。

Azure AD では、アプリケーションに対するあらゆる種類のパスワード シングル サインオンで、エンタープライズ モビリティ スイート (EMS)、Premium、または Basic ライセンス ユーザーを対象にした共有アカウントをサポートします。 数千のアプリケーション ギャラリーに事前に統合されたアプリケーションのいずれかのアカウントを共有することができで独自のパスワード認証のアプリケーションを追加できます [カスタム SSO アプリ](active-directory-single-sign-on-newly-acquired-saas-apps.md)します。

アカウントの共有を有効にする Azure AD の機能は、次のとおりです。

- [パスワード シングル サインオン](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
- パスワード シングル サインオン エージェント
- [グループの割り当て](active-directory-accessmanagement-self-service-group-management.md)
- カスタム パスワード アプリケーション
- [アプリケーション使用状況ダッシュ ボードのレポート](active-directory-passwords-get-insights.md)
- エンド ユーザー アクセス ポータル
- [アプリケーション プロキシ](active-directory-application-proxy-get-started.md)
- [Active Directory Marketplace](http://azure.microsoft.com/marketplace/active-directory/all/)

## アカウントの共有

Azure AD を使用してアカウントを共有するには、次の操作が必要です。

- アプリケーションの追加 [アプリ ギャラリー](https://azure.microsoft.com/marketplace/active-directory/) または [カスタム アプリケーション](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)
- パスワード シングル サインオン (SSO) に対応するようにアプリケーションを構成する
- 使用 [グループ ベースの割り当て](active-directory-accessmanagement-group-saasapps.md) 共有資格情報を入力するオプションを選択
- 省略可能: Facebook、Twitter、LinkedIn などの一部のアプリケーションで有効にできますオプション [Azure AD の自動パスワード ロール オーバー](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

作成することも、共有アカウントより安全な多要素認証 (MFA) と (詳細について [Azure AD でアプリケーションをセキュリティで保護する](multi-factor-authentication-get-started.md)) を使用して、アプリケーションへのアクセス権を管理する権限を委任して [Azure AD のセルフ サービス](active-directory-accessmanagement-self-service-group-management.md) グループの管理。

## 関連記事:

- [条件付きアクセスとアプリケーションを保護します。](active-directory-conditional-access.md)
- [セルフ サービス グループ管理/SSAA](active-directory-accessmanagement-self-service-group-management.md)





