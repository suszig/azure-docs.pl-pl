<properties
    pageTitle="Azure Active Directory ドメイン サービス プレビュー - 作業の開始 | Microsoft Azure"
    description="Azure Active Directory ドメイン サービスの概要"
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
    ms.date="10/12/2015"
    ms.author="maheshu"/>

# Azure AD ドメイン サービス *(プレビュー)* -作業の開始

この記事では、Azure AD テナントの Azure AD ドメイン サービスを有効にするために必要な構成手順について説明します。

## 手順 1: "AAD DC 管理者" グループを作成する
最初の手順は、Azure Active Directory テナントでの管理グループの作成です。 この特別な管理グループと呼ばれる **AAD DC 管理者**します。 このグループのメンバーには、セットアップする Azure AD ドメイン サービス ドメインにドメイン参加するコンピューターへの管理特権が付与されます。 ドメインへの参加後、ドメイン参加しているコンピューターの "Administrators" グループにこのグループが追加されます。 さらに、このグループのメンバーは、リモート デスクトップを使用して、ドメインに参加しているコンピューターにリモート接続できます。  

> [AZURE.NOTE] Azure AD ドメイン サービスを使用して作成されたドメイン内のドメイン管理者またはエンタープライズ管理者の特権を使いこなすことはできません。 このドメインは管理対象ドメインであるため、これらの権限はサービスによって予約されており、テナント内でユーザーが使用することはできません。 ただし、この構成手順で作成する特別な管理者グループでは、ドメインへのコンピューターの追加、ドメインに参加しているコンピューター上の Administrators グループへの所属、グループ ポリシーの構成など、特権が必要な操作の一部を実行できます。

この構成手順では、グループを作成し、そのグループにテナント内の 1 人以上のユーザーを追加します。 Azure AD ドメイン サービスの管理グループを作成するには、次の手順を実行します。

1. 移動し、 **Azure 管理ポータル** (つまり、 [https://manage.windowsazure.com](https://manage.windowsazure.com))
2. 選択、 **Active Directory** 左側のウィンドウでノードです。
3. Azure AD Domain Services を有効にする Azure AD テナント (ディレクトリ) を選択します。 Azure AD ディレクトリごとに作成できるドメインは 1 つです。

    ![Azure AD ディレクトリの選択](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. をクリックして、 **グループ** ] タブをクリックします。
5. をクリックして **グループの追加** ディレクトリに、グループを追加するために、ページの下部にあるタスク ペインからです。
6. という名前のグループを作成する **AAD DC 管理者**します。

    > [AZURE.WARNING] Azure AD ドメイン サービスへのアクセスを有効にするために、この厳密な名前のグループを作成する必要があります。

    ![Create administrator group](./media/active-directory-domain-services-getting-started/create-admin-group.png)

7. Azure AD ドメイン サービス内で管理者特権を付与するためにこのグループが使用されることを Azure AD テナント内の他のユーザーが理解できるように、このグループの説明を追加できます。
8. グループを作成したら、グループの名前をクリックして、このグループのプロパティを表示します。 をクリックして、 **メンバーを追加する** ユーザーをこのグループのメンバーとして追加するために、下部のパネルでボタンをクリックします。
9.  **メンバーを追加する** ダイアログ ボックスで、このグループのメンバーであるし、したら、チェック ボックスを選択する必要がありますユーザーを選択します。

    ![管理者グループへのユーザーの追加](./media/active-directory-domain-services-getting-started/add-group-members.png)

---
[**次の手順では、作成または Azure の仮想ネットワークを選択します。**](active-directory-ds-getting-started-vnet.md)

