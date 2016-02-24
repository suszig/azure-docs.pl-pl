
<properties
    pageTitle="Azure App Service Web Apps でのカスタム ドメイン名の購入方法"
    description="Azure App Service の Web アプリでカスタム ドメイン名を購入する方法について説明します。"
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
    ms.date="11/20/2015"
    ms.author="robmcm"/>

# Azure App Service でのカスタム ドメイン名の購入と構成

> [AZURE.SELECTOR]
- [Web アプリのドメインを購入します。](custom-dns-web-site-buydomains-web-app.md)
- [外部ドメインで web アプリ](web-sites-custom-domain-name.md)
- [Traffic Manager 付きの Web アプリ](web-sites-traffic-manager-custom-domain-name.md)
- [GoDaddy](web-sites-godaddy-custom-domain-name.md)




[AZURE.INCLUDE [websites-cloud-services-css-guided-walkthrough](../../includes/websites-cloud-services-css-guided-walkthrough.md)]

Web アプリを作成するときに、Azure は azurewebsites.net のサブドメインにそのアプリを割り当てます。 たとえば、web アプリの名前が **contoso**, 、URL は **contoso.azurewebsites.net**します。 また Azure によって仮想 IP アドレスも割り当てられます。

運用 Web アプリでは、通常、カスタム ドメイン名をユーザーに表示します。 この記事は、購入してカスタム ドメインを構成する方法を説明 [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)します。 

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## 概要

> [AZURE.NOTE] しないように関連付けられているアクティブなクレジット_カードがないサブスクリプションを使用してドメインを購入します。 お使いのサブスクリプションが無効になる可能性があります。 

Web アプリのドメイン名を取得していない場合に簡単に購入できますに [Azure ポータル](https://portal.azure.com)します。 購入プロセス中に、WWW とルート ドメインの DNS レコードを自動的に Web アプリにマップできます。 Azure Portal 内のドメインの権利を管理することもできます。


次の手順を使用してドメイン名を購入し、Web アプリに割り当てます。

1. ブラウザーで開く、 [Azure ポータル](https://portal.azure.com)します。

2.  **Web Apps** ] タブで、選択に、web アプリの名前をクリックして **設定**, 、し、[ **カスタム ドメインと SSL**

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

3.  **カスタム ドメインと SSL** ブレードで、をクリックして **ドメインを購入**します。

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

4.  **ドメインを購入** ブレードで、テキスト ボックスを使用して購入し、Enter キーを押してするドメイン名を入力します。 推奨される使用可能なドメインがテキスト ボックスの下に表示されます。 購入したいドメインを選択します。 一度に複数のドメインを購入できます。 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

5. クリックして、 **の連絡先情報** し、ドメインの連絡先情報フォームに入力します。

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-3.png)

> [AZURE.NOTE] すべての必須フィールドにできるだけ正確に限り、特にメール アドレスを入力することが非常に重要です。 "プライバシー保護" のないドメインを購入する場合は、ドメインがアクティブになる前に、メールの確認が求められる可能性があります。 連絡先情報に誤ったデータを入力した場合は、ドメインを購入できないことがあります。 

6. 次を選択できるようになります。

    a) [Auto renew]: ドメインを毎年自動更新します
    
    b) [Privacy protection]: 無料の購入価格に含まれる、プライバシー保護のためのオプトインです
    
    c) [Assign default hostnames]: 現在の Web アプリに WWW とルート ドメインの既存のホスト名を割り当てます 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.5.png)
  
> [AZURE.NOTE] オプション C は、DNS のバインドと自動的にホスト名のバインドを構成します。  これにより、Web アプリは、購入が完了するとすぐにカスタム ドメインを使用してアクセスできます (DNS の伝達が遅れている場合を除く)。 Web アプリが Azure Traffic Manager の背後にある場合、A レコードは Traffic Manager で使用できないため、ルート ドメインを割り当てるオプションが表示されません。 
>
>Web アプリから購入したドメインやサブドメインは、常に別の Web アプリに割り当てることができます。また、その逆の操作も可能です。 詳細については、手順 8. をご覧ください。 

    
7. クリックして、 **選択** で **ドメインを購入** ブレードで後に、購入情報が表示されます **購入内容の確認** ブレードです。 法律条項に同意をクリックすると **購入**, 、注文が送信されで購入プロセスを監視できます **通知**します。 ドメインの購入は完了するまでに数分かかります。 

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-4.png)

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-5.png)

8. ドメインを正常に購入した場合は、ドメインを管理し、Web アプリに割り当てることができます。 クリックして、 **「...」** 、ドメインの右側にあります。 ことができますし、 **購入をキャンセル** または **管理ドメイン**します。 クリックして **管理ドメイン**, 、バインド **サブドメイン** を web アプリに **管理ドメイン** ブレードです。 バインドする場合、  **サブドメイン** 別の Web アプリにし、それぞれの Web アプリケーションのコンテキスト内からこの手順を実行します。 ここでは、ドロップ ダウン メニューから Traffic Manager の名前を選択するだけで、ドメインを Traffic Manager エンドポイントに割り当てることができます (Web App が TM の背後にある場合)。 これにより、ドメインとサブドメインは、Traffic Manager エンドポイントの背後にあるすべての Web Apps に自動的に割り当てられます。 

    ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-6.png)

> [AZURE.NOTE] ことができます「購入をキャンセル」払戻の 5 日以内です。 5 日を経過すると "購入をキャンセル" できなくなり、ドメインを "削除" するオプションが表示されます。 ドメインを削除すると、返金されずにサブスクリプションが解除され、このドメインは使用可能なドメインになります。 

構成が完了したら、カスタム ドメイン名に一覧表示、 **ホスト名のバインド** web アプリのセクションです。

この時点でブラウザーにカスタム ドメイン名を入力して、対象の Web アプリに正常にアクセスできることを確認できます。
 

