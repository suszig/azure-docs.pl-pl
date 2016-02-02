<properties
    pageTitle="Cloud Services のカスタム ドメイン名を構成する | Microsoft Azure"
    description="DNS 設定を構成して、カスタム ドメインで Azure のアプリケーションやデータを公開する方法について説明します。"
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/22/2015"
    ms.author="adegeo"/>


# Azure クラウド サービスのカスタム ドメイン名の構成

> [AZURE.SELECTOR]
- [Azure classic portal](cloud-services-custom-domain-name.md)
- [Azure portal](cloud-services-custom-domain-name-portal.md)



クラウド サービスを作成するときに、Azure は cloudapp.net のサブドメインにそのアプリを割り当てます。 など、クラウド サービスは、"contoso"という名前の場合、ユーザーはアプリケーション http://contoso.cloudapp.net ような URL にアクセスすることのようになります。 また Azure によって仮想 IP アドレスも割り当てられます。

ただし、contoso.com のような独自のドメイン名を使用してアプリケーションを公開することもできます。 この記事では、クラウド サービス Web ロールのカスタム ドメイン名を予約または構成する方法について説明します

CNAME レコードと A レコードについて既に理解している場合は、 [読まず](#add-a-cname-record-for-your-custom-domain)します。
> [AZURE.NOTE]
> 速く進める — 新しい Azure の使用 [チュートリアル ガイド](http://support.microsoft.com/kb/2990804)! Azure Cloud Services または Azure Websites を使用したカスタム ドメイン名の関連付けおよび通信 (SSL) のセキュリティ保護がすばやく行えます。

<p/>
> [AZURE.NOTE]
> このタスクの手順は、Azure Cloud Services に適用されます。 Web サイトの場合、次を参照してください。 [Azure App Service Web アプリのカスタム ドメイン名の構成](../web-sites-custom-domain-name.md)します。 ストレージ アカウントを参照してください。 [Azure ストレージ アカウントのカスタム ドメイン名の構成](../storage-custom-domain-name.md)します。


## CNAME レコードと A レコードについて

CNAME レコード (またはエイリアス レコード) および A レコードはどちらもドメイン名を特定のサーバー (この場合、またはサービス) に関連付けることができますが、機能は異なります。 また、Azure のクラウド サービスで A レコードを使用する場合には固有の考慮事項もいくつかあるため、どちらのレコードを使用するかを決定する前に、これらの点を検討しておく必要があります。

### CNAME レコードまたはエイリアス レコード

CNAME レコードは、*contoso.com* や **www.contoso.com** などの**特定の**ドメインを正規のドメイン名にマップします。 この場合、正規のドメイン名は Azure ホステッド アプリケーションの **[myapp].cloudapp.net** ドメイン名です。 作成すると、CNAME は **[myapp].cloudapp.net** のエイリアスを作成します。 CNAME エントリは **[myapp].cloudapp.net** サービスの IP アドレスを自動的に解決するため、クラウド サービスの IP アドレスが変更されても、特別な対応をする必要はありません。
> [AZURE.NOTE]
> いくつかのドメイン レジストラーでは、CNAME レコードを使用する場合にマップすることが許可されるのは、ルート名 (contoso.com など) ではなく、サブドメイン (www.contoso.com など) のみです。 CNAME レコードの詳細については、レジストラーが提供するドキュメントを参照してください。 [CNAME レコードに関するウィキペディア](http://en.wikipedia.org/wiki/CNAME_record), 、または [IETF ドメイン名 - 実装と仕様書](http://tools.ietf.org/html/rfc1035) ドキュメントです。

### A レコード

A レコードがなど、ドメインをマップ **contoso.com** または **www.contoso.com**, 、*またはワイルドカード ドメイン* など **\*.contoso.com**, 、IP アドレスにします。 Azure のクラウド サービスの場合は、サービスの仮想 IP です。 CNAME レコードの A レコードの主な利点が 1 つのエントリなど、ワイルドカードを使用することができますので \***. contoso.com**, などの複数のサブドメインの要求を処理 **mail.contoso.com**, 、**login.contoso.com**, 、または **www.contso.com**します。
> [AZURE.NOTE]
> A レコードは静的 IP にマップされるため、変更をクラウド サービスの IP アドレスに自動的に解決することはできません。 クラウド サービスによって使用される IP アドレスは、空のスロット (運用またはステージング) に初めてデプロイしたときに割り当てられます。 スロットのデプロイを削除すると、IP アドレスは Azure によって解放され、そのスロットへの今後のデプロイは新しい IP アドレスに与えられます。
>
> 都合が良いのは、ステージング デプロイと運用デプロイとの間のスワッピングまたは既存のデプロイのインプレース アップグレードを実行する場合に、所定のデプロイ スロット (運用またはステージング) の IP アドレスが保持されることです。 これらのアクションを実行する方法の詳細については、次を参照してください。 [をクラウド サービスを管理する方法](cloud-services-how-to-manage.md)します。


## カスタム ドメインの CNAME レコードの追加

CNAME レコードを作成するには、レジストラーから提供されるツールを使用して、カスタム ドメイン用の DNS テーブルに新しいエントリを追加する必要があります。 それぞれのレジストラーでは CNAME レコードを指定するための同様の (多少異なる) 手段が用意されていますが、その概念は同じです。

1. これらの手段のいずれかを使用して、クラウド サービスに割り当てられた **.cloudapp.net** ドメイン名を見つけます。

    * ログイン [Azure クラシック ポータル] に、クラウド サービスをクリックし、 **ダッシュ ボード**, を見つけて、 **サイトの URL** 内のエントリ、 **[概要** セクションです。

        ![サイトの URL を表示する ][csurl]

        **OR**

    * インストールし、構成 [Azure Powershell](../install-configure-powershell.md), 、次のコマンドを使用します。

        ```powershell
        Get-AzureDeployment -ServiceName yourservicename | Select Url
        ```

    CNAME レコードを作成する場合に必要になるため、いずれかの方法で返された URL で使用されているドメイン名を保存します。

1.  DNS レジストラーの Web サイトにログオンし、DNS の管理ページに移動します。 **[ドメイン名]**、**[DNS]**、**[ネーム サーバー管理]** というラベルが付いたサイトのリンクまたは領域を探します。

2.  ここで CNAME レコードを選択または入力する場所を探します。 一覧からレコードの種類を選択するか、詳細設定ページに進まなければならない場合があります。 **CNAME**、**エイリアス**、または **サブドメイン**という単語を探します。

3.  また、**www.customdomain.com** のエイリアスを作成する場合は、CNAME のドメインまたはサブドメイン エイリアス (**www** など) を指定する必要があります。 ルート ドメインのエイリアスを作成する場合は、レジストラーの DNS ツールに "**@**" シンボルとして示される場合があります。

4. 次に、正規のホスト名 (ここではアプリケーションの **cloudapp.net** ドメイン) を指定します。

たとえば、次の CNAME レコードでは、すべてのトラフィックが **www.contoso.com** から、デプロイされたアプリケーションのカスタム DNS 名である **contoso.cloudapp.net** に転送されます。

| エイリアス/ホスト名/サブドメイン| 正規のドメイン|
| ------------------------- | -------------------- |
| www| contoso.cloudapp.net|

訪問者 **www.contoso.com** 本当のホストは参照できません
(contoso.cloudapp.net) を認識しないため、転送プロセスは
エンド ユーザーに表示されません。
> [AZURE.NOTE]
> 上の例は、**www** サブドメインのトラフィックのみに該当します。 CNAME レコードにはワイルドカードを使用できないため、各ドメインおよびサブドメインに 1 つの CNAME を作成する必要があります。 \*.Contoso.com などのサブドメインのトラフィックを cloudapp.net アドレスにする場合は、構成、 **URL リダイレクト** または **URL 転送** 、DNS 設定のエントリまたは A レコードを作成します。


## カスタム ドメインの A レコードの追加

A レコードを作成するには、まず、クラウド サービスの仮想 IP アドレスを見つける必要があります。 次に、レジストラーから提供されるツールを使用して、カスタム ドメイン用の DNS テーブルに新しいエントリを追加します。 それぞれのレジストラーでは A レコードを指定するための同様の (多少異なる) 手段が用意されていますが、その概念は同じです。

1. 次の手段のいずれかを使用して、クラウド サービスの IP アドレスを取得します。

    * ログイン [Azure クラシック ポータル] に、クラウド サービスをクリックし、 **ダッシュ ボード**, を見つけて、 **パブリック仮想 IP (VIP) アドレス** 内のエントリ、 **[概要** セクションです。

        ![VIP を表示する ][vip]

        **OR**

    * インストールし、構成 [Azure Powershell](../install-configure-powershell.md), 、次のコマンドを使用します。

        ```powershell
        get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
        ```

    複数のエンドポイントがクラウド サービスに関連付けられている場合は、IP アドレスを含んだ複数の行を受け取りますが、すべての行に同じアドレスが表示されます。

    A レコードを作成する場合に必要になるため、IP アドレスを保存します。

1.  DNS レジストラーの Web サイトにログオンし、DNS の管理ページに移動します。 **[ドメイン名]**、**[DNS]**、**[ネーム サーバー管理]** というラベルが付いたサイトのリンクまたは領域を探します。

2.  ここで A レコードを選択または入力する場所を探します。 一覧からレコードの種類を選択するか、詳細設定ページに進まなければならない場合があります。

3. この A レコードを使用するドメインまたはサブドメインを選択または入力します。 たとえば、**www.customdomain.com** のエイリアスを作成する場合は **www** を選択します。 すべてのサブドメインのワイルドカード エントリを作成する場合は、入力 '__*__' です。 これは、**mail.customdomain.com**、**login.customdomain.com**、**www.customdomain.com** などすべてのサブドメインを対象とします。

    ルート ドメインに A レコードを作成する場合は、レジストラーの DNS ツールに ''**@**" シンボルとして示される場合があります。

4. 表示されたフィールドのクラウド サービスの IP アドレスを入力します。 これによって、A レコードで使用されるドメイン エントリがクラウド サービスのデプロイの IP アドレスに関連付けられます。

たとえば、次の A レコードでは、すべてのトラフィックが **contoso.com** から、デプロイされたアプリケーションの IP アドレス名である **137.135.70.239** に転送されます。

| ホスト名/サブドメイン| IP アドレス|
| ------------------- | -------------- |
| @| 137.135.70.239|


この例では、ルート ドメインの A レコードを作成する方法を示します。 すべてのサブドメインをカバーするワイルドカード エントリを作成する場合は、入力 '__*__'、サブドメインとして。
>[AZURE.WARNING]
>Azure の IP アドレスは、既定では動的です。 使用する場合は、 [予約済み IP アドレス](..\virtual-network\virtual-networks-reserved-public-ip.md) を IP アドレスが変更されないことを確認します。

## 次のステップ

* [クラウド サービスを管理する方法](cloud-services-how-to-manage.md)
* [CDN コンテンツをカスタム ドメインにマップする方法](cdn-map-content-to-custom-domain.md)
* [、クラウド サービスの一般的な構成](cloud-services-how-to-configure.md)します。
* 学習方法 [クラウド サービス デプロイ](cloud-services-how-to-create-deploy.md)します。
* 構成 [ssl 証明書](cloud-services-configure-ssl-certificate.md)します。





[expose your application on a custom domain]: #access-app 
[add a cname record for your custom domain]: #add-cname 
[expose your data on a custom domain]: #access-data 
[vip swaps]: http://msdn.microsoft.com/library/ee517253.aspx 
[create a cname record that associates the subdomain with the storage account]: #create-cname 
[azure classic portal]: https://manage.windowsazure.com 
[validate custom domain dialog box]: http://i.msdn.microsoft.com/dynimg/IC544437.jpg 
[vip]: ./media/cloud-services-custom-domain-name/csvip.png 
[csurl]: ./media/cloud-services-custom-domain-name/csurl.png 

