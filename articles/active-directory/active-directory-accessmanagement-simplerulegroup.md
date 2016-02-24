<properties
    pageTitle="単純なルールを作成してグループの動的メンバーシップ管理を行う| Microsoft Azure"
    description="単純なルールを作成してグループの動的メンバーシップ管理を行う方法について説明します。"
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
    ms.date="11/17/2015"
    ms.author="curtand"/>


# 単純なルールを作成してグループの動的メンバーシップ管理を行う

特定のグループの動的メンバーシップ管理を有効にするには、次の手順を実行します。

1. Azure ポータルで下にある、 **グループ** ] タブで、編集するグループを選択し、このグループの [ **構成** ] タブで、設定、 **[動的メンバーシップを** に切り替える **[はい]**します。

2. ここで、このグループに対する動的メンバーシップの働きを制御する単純なルールを 1 つグループに設定することができます。 確認、 **ユーザーを追加、** オプションが選択されているし、(たとえば、department、jobTitle など)、一覧からユーザー プロパティを選択

3. 次に、条件 (等しくない、等しい、指定値で始まらない、指定値で始まる、指定値を含まない、指定値を含む、一致しない、一致する) を選び、選択したユーザー プロパティの値を指定します。 グループを SaaS アプリケーションに割り当てられ、ルールを設定して、このグループの動的メンバーシップを有効にする場合など **ユーザーを追加、** Equals(-eq) Sales Rep、役職が Sales Rep に設定、Azure AD ディレクトリ内のすべてのユーザーがあるこの SaaS アプリケーションへのアクセス、jobTitle に設定されています。

次の記事は、Azure Active Directory に関する追加情報を示します。

* [Azure Active Directory グループによるリソースへのアクセス管理](active-directory-manage-groups.md)
* [Azure Active Directory とは](active-directory-whatis.md)
* [オンプレミス ID と Azure Active Directory の統合](active-directory-aadconnect.md)

