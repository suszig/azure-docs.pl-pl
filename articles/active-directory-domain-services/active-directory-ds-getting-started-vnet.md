<properties
    pageTitle="Azure Active Directory ドメイン サービス プレビュー - 作業の開始 | Microsoft Azure"
    description="Azure Active Directory ドメイン サービスの概要"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="udayh"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/09/2015"
    ms.author="maheshu"/>

# Azure AD ドメイン サービス *(プレビュー)* -作業の開始

## Azure Virtual Network を選択するためのガイドライン
Azure AD ドメイン サービスで使用する仮想ネットワークは、次のガイドラインに従って選んでください。

- Azure AD ドメイン サービスでサポートされているリージョンの仮想ネットワークを選択します。 現在サポートされている Azure リージョンの一覧が [利用可能な [リージョン ページ](https://azure.microsoft.com/regions/#services)します。
- 既存の仮想ネットワークを使用する場合は、リージョン仮想ネットワークであることを確認してください。 従来のアフィニティ グループ機構を使った仮想ネットワークは、Azure AD ドメイン サービスでは使用できません。 必要があります [従来の仮想ネットワークを地域仮想ネットワークに移行](../virtual-networks-migrate-to-regional-vnet.md)します。
- Azure AD ドメイン サービスにアクセスする必要のある仮想マシンを現在ホストしている (または今後ホストする予定の) 仮想ネットワークを選択します。 ドメイン サービスを後から他の仮想ネットワークに移動することはできません。
- Azure AD ドメイン サービスは、Azure リソース マネージャーを使用して作成した仮想ネットワークではサポートされていません。


## ステップ 2: Azure の仮想ネットワークを作成する
次に、Azure AD ドメイン サービスが利用できる Azure Virtual Network を作成します。 希望する仮想ネットワークが既にある場合は、この手順を省略してかまいません。

> [AZURE.NOTE] Azure の仮想ネットワークを作成または Azure AD ドメイン サービスとの使用を選択するが、Azure AD ドメイン サービスでサポートされている Azure リージョンに属していることを確認します。 Azure AD ドメイン サービスが使用可能な Azure リージョンの一覧を参照してください、 [領域](active-directory-ds-regions.md) ページです。

仮想ネットワークの名前は書き留めておいてください。この後の構成手順で Azure AD ドメイン サービスを有効にする際、適切な仮想ネットワークを選択する必要があります。

Azure AD ドメイン サービスが利用できる Azure Virtual Network を作成するには、次の構成手順を実行します。

1. 移動し、 **Azure 管理ポータル** ([https://manage.windowsazure.com](https://manage.windowsazure.com))。
2. 選択、 **ネットワーク** 左側のウィンドウでノードです。
3. クリックして **新規** タスク ウィンドウで、ページの下部にあります。

    ![Virtual networks node](./media/active-directory-domain-services-getting-started/virtual-networks.png)

4.  **ネットワーク サービス** ノードで、選択 **仮想ネットワーク**します。
5. をクリックして **簡易作成** 仮想ネットワークを作成するためにします。

    ![Virtual network - quick create](./media/active-directory-domain-services-getting-started/virtual-network-quickcreate.png)

6. 指定する **名前** 、仮想ネットワークのです。 構成できます、 **アドレス空間** または **最大 VM 数** このネットワークにします。 DNS サーバーの設定は差し当たり、[なし] のままでかまいません。 この設定は、Azure AD ドメイン サービスを有効にした後で更新されます。
7. サポートされている Azure リージョンを選択するようにして、 **場所** ドロップダウンです。 Azure AD ドメイン サービスが使用可能な Azure リージョンの一覧を参照してください、 [領域](active-directory-ds-regions.md) ページです。 これは重要な手順です。 Azure AD ドメイン サービスでサポートされていない Azure リージョンの仮想ネットワークを選択した場合、その仮想ネットワークでドメイン サービスを有効にすることができません。
8. クリックして、 **仮想ネットワークの作成** クリックすると、仮想ネットワークを作成します。

    ![Azure AD Domain Services 用の仮想ネットワークを作成します。](./media/active-directory-domain-services-getting-started/create-vnet.png)

---
[**次の手順で Azure AD ドメインを有効にするサービス。**](active-directory-ds-getting-started-enableaadds.md)

