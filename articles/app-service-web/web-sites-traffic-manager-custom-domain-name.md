<properties 
    pageTitle="Traffic Manager を利用する Azure App Service Web アプリのカスタム ドメイン名の構成"wpickett"Use a custom domain name for an a web app in Azure App Service that includes Traffic Manager for load balancing." 
    description="負荷分散のための Traffic Manager を含む Azure App Service Web アプリのカスタム ドメイン名の使用" 
    services="app-service\web" 
    documentationCenter="" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/18/2015" 
    ms.author="robmcm"/>

#Traffic Manager を使用して Azure App Service Web アプリのカスタム ドメイン名を構成する

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[AZURE.INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

この記事では、負荷分散に Traffic Manager を利用する Azure App Service でカスタム ドメイン名を使用する一般的な手順を示します。

[AZURE.INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>
## DNS レコードについて

[AZURE.INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>
## Web アプリの標準モード用の構成

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>
## カスタム ドメインの DNS レコードの追加

> [AZURE.NOTE] Azure App Service Web Apps を使用してドメインを購入した場合は、[次の手順をスキップ [の最後の手順を参照してください [Web アプリ用のドメインの購入](custom-dns-web-site-buydomains-web-app.md) 記事です。 

カスタム ドメインを Azure App Service の Web アプリに関連付けるには、ドメイン名を購入したドメイン レジストラーのツールを使用して、新しいエントリをカスタム ドメインの DNS テーブルに追加する必要があります。 次の手順を使用して DNS ツールを見つけて利用します。

1. ドメイン レジストラーのアカウントにログオンし、DNS レコードの管理ページを探します。 としてというラベルが付いたサイトのリンクまたは領域を探します **ドメイン名**, 、**DNS**, 、または **ネーム サーバー管理**します。 このページへのリンクのある多くの場合、アカウント情報を表示しなどのリンクを検索して **My domains**します。

4. ドメイン名の管理ページが見つかったら、DNS レコードを編集できるリンクを探します。 これは、として示される可能性があります、 **ゾーン ファイル**, 、**DNS レコード**, 、または、 **詳細** 構成リンクします。

    * いくつかのレコードを関連付けるエントリなど、既に作成した多くの場合 '**@**' または ' \ *' を「ドメイン パーキング」ページです。 また、一般的なサブドメインのレコードなど **www**します。
    * ページにも使用 **CNAME レコード**, 、またはレコードの種類を選択するドロップダウンが表示されます。 これは、可能性がありますも言うまでも他のレコードなど **レコード** と **MX レコード**します。 場合によっては、CNAME レコードはで呼ばれての別の名前など、 **エイリアス レコード**します。
    * ページには、するためのフィールドがあります。 **マップ** から、 **ホスト名** または **ドメイン名** 別のドメイン名にします。

5. 各レジストラーの仕様によって異なりますが、一般的にはマッピング *から* 、カスタム ドメイン名 (など **contoso.com**,、) *に* Traffic Manager ドメイン名 (**contoso.trafficmanager.net**) web アプリのために使用されます。

6. レジストラーで DNS レコードの追加または変更が完了したら、変更を保存します。

<a name="enabledomain"></a>
## Traffic Manager を有効にする

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 
 


