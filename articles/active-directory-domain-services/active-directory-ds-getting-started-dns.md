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
    ms.author="mahesh-unnikrishnan"/>

# Azure AD ドメイン サービス *(プレビュー)* -作業の開始

## 手順 4: Azure 仮想ネットワークの DNS 設定を更新する
ディレクトリの Azure AD ドメイン サービスが正常に有効化されました。次に、仮想ネットワーク内のコンピューターがこれらのサービスに接続して使用できるようにします。 そのためには、仮想ネットワーク上で Azure AD ドメイン サービスを使用できる IP アドレスを指すように、仮想ネットワークの DNS サーバー設定を更新する必要があります。

> [AZURE.NOTE] 表示される Azure AD ドメイン サービスの IP アドレスを書き留めて、 **構成** ディレクトリの Azure AD ドメイン サービスを有効にした後、ディレクトリのタブをクリックします。

Azure AD ドメイン サービスを有効にした仮想ネットワークの DNS サーバー設定を更新するには、次の構成手順を実行します。

1. 移動し、 **Azure 管理ポータル** ([https://manage.windowsazure.com](https://manage.windowsazure.com))。
2. 選択、 **ネットワーク** 左側のウィンドウでノードです。

    ![Virtual networks node](./media/active-directory-domain-services-getting-started/virtual-network-select.png)

3.  **仮想ネットワーク** ] タブで、そのプロパティを表示する Azure AD ドメイン サービスを有効になる仮想ネットワークを選択します。
4. をクリックして、 **構成** ] タブをクリックします。

    ![Virtual networks node](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)

5.  **DNS サーバー** セクションで、Azure AD ドメイン サービスの IP アドレスを入力します。
6. 表示されていた、両方の IP アドレスを入力することを確認、 **ドメイン サービス** セクションで、 **構成** ディレクトリのタブをクリックします。
7. をクリックして **保存** タスク ウィンドウでこの仮想ネットワークの DNS サーバーの設定を保存するために、ページの下部にあります。

   ![仮想ネットワークの DNS サーバー設定を更新します。](./media/active-directory-domain-services-getting-started/update-dns.png)

> [AZURE.NOTE] 仮想ネットワークの DNS サーバーの設定を更新した後に、更新された DNS 構成を取得するネットワーク上の仮想マシンには少しをかかる場合があります。 仮想マシンがドメインに接続できない場合は、DNS キャッシュを (例: フラッシュできます。 ipconfig/flushdns) 仮想マシンの DNS 設定の更新を強制するために、仮想マシンにします。

---
[**次のステップ - Azure AD ドメイン サービスへのパスワード同期を有効にします。**](active-directory-ds-getting-started-password-sync.md)

