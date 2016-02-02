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

## <a name="enable-vpn"> </a>を有効にする VPN 接続

>VPN 接続は、**Premium** レベルでのみ利用できます。 に切り替えるには、これで API Management サービスを開きます、 [Azure 旧ポータルの][] クリックして、 **スケール** ] タブをクリックします。 **[全般]** セクションで、Premium レベルを選択し、[保存] をクリックします。

VPN 接続を有効にするで API Management サービスを開きます、 [Azure 旧ポータルの][] に切り替えると、 **構成** ] タブをクリックします。

[VPN] セクションで、**[VPN 接続]** を **[オン]** に切り替えます。

![API Management インスタンスの ][api-management-setup-vpn-configure]

API Management サービスがプロビジョニングされているすべてのリージョンの一覧が表示されます。

すべてのリージョンについて、VPN とサブネットを選択します。 VPN の一覧は、構成しているリージョンで設定された Azure サブスクリプションで使用できる仮想ネットワークに基づいて設定されます。

![VPN の選択][api-management-setup-vpn-select]

ページの下部にある **[保存]** をクリックします。 更新中は、Azure クラシック ポータルで API Management サービスに対して他の操作を実行できません。 サービス ゲートウェイは使用可能な状態に保持されるため、実行時の呼び出しは影響を受けません。

ゲートウェイの VIP アドレスは VPN を有効または無効にするたびに変化することに注意してください。

## <a name="connect-vpn"> </a>VPN の背後にある web サービスへの接続

API Management サービスが VPN に接続された後で仮想ネットワーク内の Web サービスにアクセスする方法は、パブリック サービスにアクセスする方法と同じです。 単に、新しい API を作成するときや既存の API を編集するときに Web サービスのローカル アドレスまたはホスト名 (Azure Virtual Network に対して DNS サーバーが構成されている場合) を **[Web サービスの URL]** ボックスに入力するだけです。

![VPN からの API の追加][api-management-setup-vpn-add-api]


## <a name="related-content"> </a>関連コンテンツ

* [チュートリアル: サイト対サイト接続の間のクロスプレミス仮想ネットワークを作成します。][]
* [Azure API Management ので呼び出しをトレース API Inspector を使用する方法][]


[api-management-setup-vpn-configure]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-configure.png 
[api-management-setup-vpn-select]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-select.png 
[api-management-setup-vpn-add-api]: ./media/api-management-howto-setup-vpn/api-management-setup-vpn-add-api.png 
[enable vpn connections]: #enable-vpn 
[connect to a web service behind vpn]: #connect-vpn 
[related content]: #related-content 
[azure classic portal]: https://manage.windowsazure.com/ 
[tutorial: create a cross-premises virtual network for site-to-site connectivity]: ../virtual-networks-create-site-to-site-cross-premises-connectivity 
[how to use the api inspector to trace calls in azure api management]: api-management-howto-api-inspector.md 

