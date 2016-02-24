<properties
    pageTitle="Azure Active Directory の専用グループ | Microsoft Azure"
    description="Azure Active Directory の専用グループの機能と作成方法の概要について説明します。"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="stevenpo"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/17/2015"
    ms.author="curtand"/>

# Azure Active Directory の専用グループ

Azure Active Directory では、専用グループが自動的に作成され、そのグループ メンバーシップも自動的に作成されます。 専用グループのメンバーは、Azure ポータル、Windows PowerShell コマンドレット、プログラムのいずれの方法でも追加したり削除したりすることはできません。 専用グループを有効にするには、Azure ポータルで、[構成] タブで、設定、 **専用グループを有効にするには、[はい] に切り替える**します。

設定すると、専用のグループを有効にするスイッチ **[はい]**, 、自動的に設定して All Users 専用グループを作成するディレクトリをさらに有効にすることができます、 **"All Users"を有効にするグループ** に切り替える **[はい]**します。 編集することもこの専用グループの名前で」と入力して、 **"All Users"の表示名グループ** フィールドです。

All Users 専用グループは、ディレクトリ内のすべてのユーザーに同じアクセス許可を割り当てる場合に便利です。 たとえば、ディレクトリ内のすべてのユーザーに SaaS アプリケーションへのアクセス権を与えるには、All Users 専用グループに対してこのアプリケーションへのアクセス権を割り当てます。

次の記事は、Azure Active Directory に関する追加情報を示します。

* [Azure Active Directory グループによるリソースへのアクセス管理](active-directory-manage-groups.md)
* [Azure Active Directory とは](active-directory-whatis.md)
* [オンプレミス ID と Azure Active Directory の統合](active-directory-aadconnect.md)

