<properties
    pageTitle="Azure Active Directory 内のパスワードを管理する | Microsoft Azure"
    description="Azure Active Directory のパスワードを管理する方法。"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/01/2015"
    ms.author="curtand"/>


# Azure Active Directory のパスワードを管理する

管理者は、Azure クラシック ポータルで Azure Active Directory (Azure AD) のユーザーのパスワードをリセットできます。 ディレクトリの名前をクリックし、[ユーザー] ページでユーザーの名前をクリックして、ポータルの下部にある **[パスワードのリセット]** をクリックします。

このトピックの以降の部分では、Azure Active Directory がサポートする次のようなパスワード管理機能について詳しく説明します。

- **セルフ サービスによるパスワード**の変更では、エンド ユーザーまたは管理者は、管理者またはヘルプ デスクにサポートを依頼することなく、有効期限が切れた、または有効期限が切れていないパスワードを変更できます。
- **セルフ サービスによるパスワード**のリセットでは、エンド ユーザーまたは管理者は、管理者またはヘルプ デスクにサポートを依頼することなく、パスワードを自動的にリセットできます。 セルフ サービスのパスワード リセットには、Azure AD Premium または Basic が必要です。 詳細については、次を参照してください。 [Azure Active Directory のエディション](active-directory-editions.md)します。
- **管理者によるパスワードのリセット**では、管理者は、Azure クラシック ポータル内からエンド ユーザーまたは別の管理者のパスワードをリセットできます。
- **パスワード管理アクティビティ レポート**では、組織内で発生したパスワードのリセットおよび登録アクティビティの詳細が管理者に提供されます。
- **パスワード ライトバック**では、クラウドからオンプレミスのパスワードを管理できるので、フェデレーション ユーザーまたはパスワード同期済みユーザーは、またはこれらのユーザーに代わって、上記のシナリオをすべて実行できます。 パスワード ライトバックには Azure AD Premium が必要です。 詳細については、次を参照してください。 [Azure Active Directory Premium の概要](active-directory-get-started-premium.md)します。

> [AZURE.NOTE]
> Azure AD のワールドワイド インスタンスを使用している中国のお客様は、Azure AD Premium を利用できます。 中国の 21Vianet が運営する Microsoft Azure サービスでは、Azure AD Premium は現在サポートされていません。 詳細については、下記までお問い合わせください、 [Azure Active Directory フォーラム](http://feedback.azure.com/forums/169401-azure-active-directory)します。

次のリンクを使用して、最も関心のあるドキュメントに移動してください。

- [Azure AD でのパスワード管理の概要:](active-directory-passwords-how-it-works.md)
- [セルフ サービス パスワードが Azure AD でリセット: を有効にする、構成、およびセルフ サービス パスワード リセットをテストする方法](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords)
- [セルフ サービス パスワードが Azure AD でリセット: ニーズに合わせてパスワードをカスタマイズする方法をリセット](active-directory-passwords-customize.md)
- [セルフ サービス パスワードが Azure AD でリセット: 展開と管理のベスト プラクティス](active-directory-passwords-best-practices.md)
- [Azure AD でのパスワード管理レポート: テナントでパスワード管理アクティビティを表示する方法](active-directory-passwords-get-insights.md)
- [パスワード ライトバック: Azure AD の管理を構成する方法、オンプレミスのパスワード](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)
- [Azure AD のパスワード管理のトラブルシューティング](active-directory-passwords-troubleshoot.md)
- [Azure AD パスワード管理に関するよく寄せられる質問](active-directory-passwords-faq.md)


## 参照トピック

- [Azure AD の管理](active-directory-administer.md)
- [作成または Azure AD でユーザーの編集](active-directory-create-users.md)
- [Azure AD でグループを管理します。](active-directory-manage-groups.md)





