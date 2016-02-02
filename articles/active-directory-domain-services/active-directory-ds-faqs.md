<properties
    pageTitle="Azure Active Directory ドメイン サービス プレビュー: FAQ | Microsoft Azure"
    description="Azure Active Directory ドメイン サービスについてよく寄せられる質問"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="udayh"
    editor="inhenk"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/16/2015"
    ms.author="maheshu"/>


# Azure Active Directory ドメイン サービス プレビュー: FAQ

このページでは、Azure Active Directory ドメイン サービス プレビューに関してよく寄せられる質問への回答が記載されています。 常に最新情報をチェックしてください。

### トラブルシューティング ガイド

参照してください、 [トラブルシューティング ガイド](active-directory-ds-troubleshooting.md) 構成または Azure AD ドメイン サービスを管理するときに発生する一般的な問題の解決方法です。


### 構成

#### 1 つの Azure AD ディレクトリに対して複数のドメインを作成することはできますか。

番号 作成できるは、1 つの Azure AD ドメイン サービスによって処理された 1 つのドメインのみ Azure AD ディレクトリです。

#### Azure AD Domain Services をサブスクリプション内の複数の仮想ネットワークで利用できまですか。

このサービスそのものが、そのようなシナリオを直接サポートすることはありません。 Azure AD Domain Services は一度に 1 つの仮想ネットワークでのみ利用できます。 ただし、Azure AD Domain Services を他の仮想ネットワークに公開するために複数の仮想ネットワーク間の接続を構成できます。 この記事では、する方法について説明 [Azure における仮想ネットワークを接続](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)します。

#### PowerShell を使用して Azure AD ドメイン サービスを有効にできますか。

PowerShell/Azure AD Domain Services の自動デプロイは、現時点では利用できません。

#### Azure AD Domain Services は新しい Azure ポータルで利用できますか。

番号 Azure AD ドメイン サービスは、以前の Azure 管理ポータル (https://manage.windowsazure.com など) でのみ構成できます。 今後、新しい Microsoft Azure 管理ポータル (https://portal.azure.com など) のサポートを拡張しようと思っています。


### 管理と操作

#### Azure AD Domain Services を有効にしています。このドメインに参加しているドメイン コンピューターでは、どのユーザー アカウントを使用できますか。

管理グループに追加したユーザー アカウント (つまり、AAD DC 管理者) を、ドメインに参加しているコンピューターで使用できます。 さらに、このグループのユーザーには、ドメインに参加しているコンピューターへのリモート デスクトップ アクセス権が付与されます。

#### Azure AD Domain Services によって提供されるドメインでドメイン管理者特権を使用できますか。

番号 これは、管理されたサービスであるためは提供されず管理者特権ドメイン上です。 つまり、‘ドメイン管理者’ 特権と ‘エンタープライズ管理者’ 特権は、どちらもドメインでは利用できません。 Azure AD directory 内の既存のドメイン管理者グループまたはエンタープライズ管理者グループにも、ドメインに関するドメイン/エンタープライズ管理者特権は付与されません。

#### LDAP または他の AD 管理ツールを使用して、Azure AD Domain Services によって用意されるドメインのグループ メンバーシップを変更できますか。

番号 Azure AD ドメイン サービスによって処理されたドメインでは、グループ メンバーシップを変更できません。 ユーザー属性に対しても同様です。 ただし、Azure AD またはオンプレミスのドメインのいずれかで、グループ メンバーシップまたはユーザー属性を変更できます。 このような変更は、Azure AD Domain Services に自動的に同期されます。


### 課金と可用性

#### これは有料のサービスですか。

このサービスは、パブリック プレビュー期間中は特別割引価格で利用できます。 正規料金の課金は、サービスが一般公開 (GA) されたときに開始されます。 詳細については、価格に関するページを参照してください。

#### サービスの無料試用版はありますか。

このサービスは、Azure の無料試用版に含まれています。 サインアップできる、 [無料の Azure 1 か月の評価版](https://azure.microsoft.com/pricing/free-trial/)します。

#### Enterprise Mobility Suite (EMS) の一部として Azure AD Domain Services を取得できますか。

いいえ。Azure AD Domain Services は従量課金の Azure サービスであり、EMS には含まれていません。 Azure AD Domain Services は、Azure AD のすべての SKU (無料、Basic、Premium) で利用でき、使用状況に応じて 1 時間単位で課金されます。

#### このサービスは、どの Azure のリージョンで利用できますか。

参照してください、 [リージョン ページ](active-directory-ds-regions.md) Azure AD ドメイン サービスが利用可能な Azure リージョンの一覧を表示します。

#### Azure AD Domain Services はいつ一般公開される予定ですか。

現時点では、このサービスの一般公開予定は決まっていません。





