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

管理者は、Azure クラシック ポータルで Azure Active Directory (Azure AD) のユーザーのパスワードをリセットできます。 [ユーザー] ページと、ディレクトリの名前をクリックし、ポータルの下部にあるユーザーの名前をクリックして **パスワードのリセット**します。

このトピックの以降の部分では、Azure Active Directory がサポートする次のようなパスワード管理機能について詳しく説明します。

- **セルフ サービス パスワード** 変更により、エンドユーザーまたは管理者のサポートについては、管理者またはヘルプデスクを呼び出さずに、有効期限が切れた、または有効期限が切れていないパスワードを変更します。
- **セルフ サービス パスワード** リセットは、エンドユーザーまたは管理者のサポートは管理者またはヘルプデスクを呼び出さずに自分のパスワードを自動的にリセットされることができます。 セルフ サービスのパスワード リセットには、Azure AD Premium または Basic が必要です。 詳細については、次を参照してください。 [Azure Active Directory のエディション](active-directory-editions.md)します。
- **管理者によるパスワードのリセット** Azure クラシック ポータル内からエンドユーザーまたは別の管理者のパスワードをリセットすることができます。
- **パスワード管理アクティビティ レポート** 管理者社内で発生した、パスワードのリセットと登録のアクティビティに提供します。
- **パスワード ライトバック** 、クラウドからオンプレミス パスワードを管理できるように、上記のシナリオすべてを実行するかの代わりに、フェデレーション ユーザーまたはパスワード同期済みユーザー。 パスワード ライトバックには Azure AD Premium が必要です。 詳細については、次を参照してください。 [Azure Active Directory Premium の概要](active-directory-get-started-premium.md)します。

> [AZURE.NOTE]
> Azure AD のワールドワイド インスタンスを使用している中国のお客様は、Azure AD Premium を利用できます。 中国の 21Vianet が運営する Microsoft Azure サービスでは、Azure AD Premium は現在サポートされていません。 詳細については、下記までお問い合わせください、 [Azure Active Directory フォーラム](http://feedback.azure.com/forums/169401-azure-active-directory)します。

次のリンクを使用して、最も関心のあるドキュメントに移動してください。

- [概要: Azure AD でのパスワード管理](active-directory-passwords-how-it-works.md)
- [Azure AD のセルフ サービス パスワードのリセット: セルフ サービスのパスワード リセットを有効化、構成、テストする方法](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords)
- [Azure AD のセルフ サービス パスワードのリセット: ニーズに合わせてパスワード リセットをカスタマイズする方法](active-directory-passwords-customize.md)
- [Azure AD のセルフ サービス パスワードのリセット: 展開と管理のベスト プラクティス](active-directory-passwords-best-practices.md)
- [Azure AD でのパスワード管理のレポート: テナントでパスワード管理アクティビティを表示する方法](active-directory-passwords-get-insights.md)
- [パスワード ライトバック: オンプレミスのパスワードの管理用２に Azure AD を構成する方法](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords)
- [Azure AD のパスワード管理に関するトラブルシューティング](active-directory-passwords-troubleshoot.md)
- [Azure AD のパスワード管理に関する FAQ](active-directory-passwords-faq.md)


## 参照トピック

- [Administer your Azure AD directory](active-directory-administer.md)
- [Azure AD でのユーザーの作成または編集](active-directory-create-users.md)
- [Azure AD でのグループの管理](active-directory-manage-groups.md)

