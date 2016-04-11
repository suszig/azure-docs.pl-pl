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
    ms.date="10/16/2015"
    ms.author="maheshu"/>

# Azure AD ドメイン サービス *(プレビュー)* -作業の開始

## ステップ 3: Azure AD Domain Services を有効にする
この手順では、ディレクトリに対して Azure AD Domain Services を有効にすることができます。 ディレクトリに対して Azure AD Domain Services を有効にするには、次の構成手順を実行します。

1. 移動し、 **Azure 管理ポータル** ([https://manage.windowsazure.com](https://manage.windowsazure.com))。
2. 選択、 **Active Directory** 左側のウィンドウでノードです。
3. Azure AD Domain Services を有効にする Azure AD テナント (ディレクトリ) を選択します。

    ![Azure AD ディレクトリの選択](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. をクリックして、 **構成** ] タブをクリックします。

    ![ディレクトリの [構成] タブ](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. というタイトルのセクションまで下へスクロール **ドメイン サービス**します。

    ![Domain Services 構成セクション](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)

6. 」というタイトルのオプションを切り替える **このディレクトリのドメイン サービスを有効にする** に **はい**します。 Azure AD Domain Services 用の構成オプションがページに表示されます。

    ![Domain Services の有効化](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

    > [AZURE.NOTE] テナントの Azure AD ドメイン サービスを有効にすると、Azure AD を生成し、ユーザーを認証するために必要な Kerberos と NTLM 資格情報のハッシュを保存します。

7. 指定の **ドメイン サービスの DNS ドメイン名**します。
   - ディレクトリの既定のドメイン名 (で始まり、つまり、 **. onmicrosoft.com** ドメイン サフィックス) 既定で選択されます。
   - 一覧には、Azure AD ディレクトリに対して構成されたすべてのドメインが含まれます。検証対象のドメインおよび [ドメイン] タブで構成する検証対象外のドメインなどがあります。
   - さらに、この一覧には、カスタム ドメイン名を入力して追加することもできます。

     > [AZURE.WARNING] (例: 指定したドメイン名のドメインのプレフィックスことを確認します。 'contoso' ['contoso.local' ドメイン名) は、15 文字未満です。 ドメイン プレフィックスが 15 文字より長いと、Azure AD Domain Services ドメインを作成することはできません。

8. 次の手順は、Azure AD Domain Services を利用できる仮想ネットワークを選択することです。 「ドロップダウン リストで、作成した仮想ネットワークを選択して **ドメイン サービスをこの仮想ネットワークに接続**します。
   - 指定した仮想ネットワークが Azure AD Domain Services でサポートされている Azure リージョンに属することを確認します。
   - 参照してください、 [リージョン ページ](active-directory-ds-regions.md) サポートされている Azure リージョンの一覧を表示します。

9. 終了したら、上記のオプションを選択すると、をクリックして **保存** を Azure AD ドメイン サービスを有効にする] ページの下部のタスク ウィンドウからです。
10. ページが表示されます、' 保留中] ' Azure AD ドメイン サービスが有効になって、ディレクトリの中には、状態です。

    ![Domain Services の有効化 - 保留中状態](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

    > [AZURE.NOTE] Azure の AD ドメイン サービスでは、管理対象ドメインの高可用性を提供します。 Azure AD Domain Services を初めて有効にすると、仮想ネットワーク上で Domain Services を利用できる IP アドレスが 1 つづつ表示されます。 2 番目の IP アドレスは、このサービスがドメインに対する高可用性を有効にするとすぐに表示されます。 高可用性が構成されているドメインのアクティブにして表示されますで 2 つの IP アドレス、 **ドメイン サービス** のセクションで、 **構成** ] タブをクリックします。

11. 約 20 ~ 30 分後にドメイン サービスでは、仮想ネットワーク上に可用性を最初の IP アドレスが表示されます、 **IP アドレス** フィールドに、 **構成** ページです。

    ![Domain Services が有効化された - 最初の IP がプロビジョニングされた](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)

12. 高可用性がドメインで運用されると、2 つの IP アドレスがページに表示されます。 これらは、選択した仮想ネットワーク上で Azure AD Domain Services を利用できる IP アドレスです。 仮想ネットワークの DNS 設定を更新できるように、これらの IP アドレスをメモしておきます。 この手順は、仮想ネットワーク上の仮想マシンが、ドメインへの参加などの操作のためにドメインに接続できるようにします。

    ![Domain Services が有効化された - 2 つの IP がプロビジョニングされた](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [AZURE.NOTE] Azure AD ディレクトリのサイズによっては (ユーザーの数などをグループ化) の Azure AD ドメイン サービスで使用できるようにディレクトリの内容には少しがかかります。 この同期処理は、バック グラウンドで発生します。 数万のオブジェクトがある大きなディレクトリの場合、すべてのユーザー、グループ メンバーシップ、資格情報を同期して Azure AD Domain Services で利用できるようになるまで、1 ～ 2 日かかることがあります。


---
[**次の手順では、Azure の仮想ネットワークの DNS を更新設定します。**](active-directory-ds-getting-started-dns.md)

