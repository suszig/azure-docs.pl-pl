<properties
    pageTitle="Azure API Management で VPN 接続を設定する方法"
    description="Azure API Management で VPN 接続を設定して Web サービスにアクセスする方法について説明します。"
    services="api-management"
    documentationCenter=""
    authors="antonba"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/03/2015"
    ms.author="antonba"/>

# Azure API Management で VPN 接続を設定する方法

API Management の VPN のサポートにより、API Management ゲートウェイを Azure Virtual Network に接続することができます。 これにより、API Management のお客様は、オンプレミスのバックエンド Web サービスまたはパブリック インターネットからアクセスできないバックエンド Web サービスに安全に接続できます。

## <a name="enable-vpn"> </a>VPN 接続を有効にする

>VPN 接続は、 **Premium** 層です。 に切り替えるには、これで API Management サービスを開きます、 [Azure Classic Portal][] クリックして、 **スケール** ] タブをクリックします。 下にある、 **全般** セクションでは、Premium レベルを選択し、[保存] をクリックします。

VPN 接続を有効にするで API Management サービスを開きます、 [Azure Classic Portal][] に切り替えると、 **構成** ] タブをクリックします。 

[VPN] セクションで、切り替える **VPN 接続** に **に**します。

![API Management インスタンスの [構成] タブ][api-management-setup-vpn-configure]

API Management サービスがプロビジョニングされているすべてのリージョンの一覧が表示されます。

すべてのリージョンについて、VPN とサブネットを選択します。 VPN の一覧は、構成しているリージョンで設定された Azure サブスクリプションで使用できる仮想ネットワークに基づいて設定されます。

![VPN の選択][api-management-setup-vpn-select]

クリックして **保存** 、画面の下部にあります。 更新中は、Azure クラシック ポータルで API Management サービスに対して他の操作を実行できません。 サービス ゲートウェイは使用可能な状態に保持されるため、実行時の呼び出しは影響を受けません。

ゲートウェイの VIP アドレスは VPN を有効または無効にするたびに変化することに注意してください。

## <a name="connect-vpn"> </a>VPN の背後にある Web サービスへの接続

API Management サービスが VPN に接続された後で仮想ネットワーク内の Web サービスにアクセスする方法は、パブリック サービスにアクセスする方法と同じです。 ローカル アドレスまたは web サービスへのホスト名 (DNS サーバーは、Azure の仮想ネットワーク用に構成された) 場合に入力するだけの **Web サービスの URL** 新しい API を作成するときにフィールドまたは既存のものを編集します。

![VPN からの API の追加][api-management-setup-vpn-add-api]


## <a name="related-content"></a>関連コンテンツ


* [チュートリアル: サイト間接続用のクロスプレミス仮想ネットワークの作成][]
* [Azure API Management で API Inspector を使用して呼び出しをトレースする方法][]

[api-management-setup-vpn-configure]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png
[api-management-setup-vpn-select]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[Azure Classic Portal]: https://manage.windowsazure.com/

[Tutorial: Create a Cross-Premises Virtual Network for Site-to-Site Connectivity]: ../virtual-networks-create-site-to-site-cross-premises-connectivity
[How to use the API Inspector to trace calls in Azure API Management]: api-management-howto-api-inspector.md


