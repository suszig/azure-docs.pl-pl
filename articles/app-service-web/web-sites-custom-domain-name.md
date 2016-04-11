<properties
    pageTitle="Azure App Service のカスタム ドメイン名の構成"
    description="Azure App Service の Web アプリでカスタム ドメイン名を使用する方法について説明します。"
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"
    tags="top-support-issue"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/23/2015"
    ms.author="cephalin"/>

# Azure App Service のカスタム ドメイン名の構成

> [AZURE.SELECTOR]
- [Web アプリのドメインを購入します。](custom-dns-web-site-buydomains-web-app.md)
- [外部ドメインで web アプリ](web-sites-custom-domain-name.md)
- [Traffic Manager 付きの Web アプリ](web-sites-traffic-manager-custom-domain-name.md)
- [GoDaddy](web-sites-godaddy-custom-domain-name.md)

Web アプリを作成するときに、Azure は azurewebsites.net のサブドメインにそのアプリを割り当てます。 たとえば、web アプリの名前が **contoso**, 、URL は **contoso.azurewebsites.net**します。 また Azure によって仮想 IP アドレスも割り当てられます。

運用 Web アプリでは、通常、カスタム ドメイン名をユーザーに表示します。 ここでカスタム ドメインを構成する方法について説明 [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)します。

この記事のどの時点でもその他のヘルプを必要がある場合で Azure のエキスパートに連絡することができます [MSDN Azure とスタック オーバーフロー フォーラム](http://azure.microsoft.com/support/forums/)します。 または、Azure サポート インシデントを送信できます。 移動して、 [Azure サポート サイト](http://azure.microsoft.com/support/options/) ] をクリック **サポートにお問い合わせ**します。

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

## 概要

外部ドメイン名をまだ登録していないかどうか (つまりありません *. azurewebsites.net) で直接購入するカスタム ドメインを設定する最も簡単な方法は、既に、 [Azure ポータル](https://portal.azure.com)します。 このプロセスでは、Web アプリのドメイン名を GoDaddy などのサードパーティ サイトにアクセスして管理する代わりに、ポータルで直接管理できます。 同様に、web アプリでのドメイン名の構成が大幅に簡略化、web アプリで使用するかどうか [Azure Traffic Manager](web-sites-traffic-manager-custom-domain-name.md) か。 詳細については、次を参照してください。 [購入して Azure App Service でのカスタム ドメイン名の構成](custom-dns-web-site-buydomains-web-app.md)します。

既にドメイン名があるか予約する場合のドメインから他のドメイン レジストラーでは、web アプリのカスタム ドメイン名を表示する一般的な手順をここでは (を参照してください [GoDaddy.com の詳しい説明](web-sites-godaddy-custom-domain-name.md))。

1. ドメイン名を予約します。 使用できるレジストラーの種類は数多く存在するため、 ここでは、この手順については説明しません。 サインアップすると、サイトによってプロセスが順に表示されます。
1. ドメインを Azure Web アプリにマップする DNS レコードを作成します。
1. 内のドメイン名を追加、 [Azure ポータル](https://portal.azure.com)します。

基本的な手順は以上ですが、特に注意が必要な場合があります。

- ルート ドメインのマッピング。 ルート ドメインとは、ドメイン レジストラーで予約したドメインです。 たとえば、 **contoso.com**します。
- サブドメインのマッピング。 たとえば、 **blogs.contoso.com**します。  複数のサブドメインをそれぞれ異なる Web アプリにマップできます。
- ワイルドカードのマッピング。 たとえば、 **\*.contoso.com**します。 ワイルドカードのエントリは、ドメインのすべてのサブドメインに適用されます。

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-modes.md)]


## DNS レコードの種類

ドメイン ネーム システム (DNS) では、データ レコードを使用してドメイン名が IP アドレスにマップされます。 DNS レコードには、複数の種類があります。 Web アプリを作成するか、 *A* レコードまたは *CNAME* レコードです。

- A **(アドレス)** レコードでは、ドメイン名を IP アドレスにマップします。
- A **CNAME (Canonical Name)** レコードでは、ドメイン名を別のドメイン名にマップします。 DNS は、2 番目の名前を使用してアドレスを検索します。 その場合も、ユーザーのブラウザーには 1 番目のドメイン名が表示されます。 たとえば、contoso.com をマップする *& lt; yourwebapp & gt;*します。*.azurewebsites.net します。

IP アドレスが変更された場合、CNAME エントリはそのまま有効ですが、A レコードは更新する必要があります。 ただし、ドメイン レジストラーによっては、ルート ドメインやワイルドカード ドメインに CNAME レコードを使用できない場合があります。 その場合は A レコードを使用する必要があります。

> [AZURE.NOTE] IP アドレスは変わることを削除し、web アプリを再作成または web を変更するアプリ モードに戻すを解放します。


## 仮想 IP アドレスの検索

CNAME レコードを作成している場合は、この手順をスキップしてください。 A レコードを作成するには、Web アプリの仮想 IP アドレスを見つける必要があります。 IP アドレスを見つけるには:

1.  ブラウザーで開く、 [Azure ポータル](https://portal.azure.com)します。
2.  クリックして、 **参照** ページの左側にあるオプション。
3.  クリックして、 **Web Apps** ブレードです。
4.  Web アプリの名前をクリックします。
5.   **Essentials** ] ページで [ **設定をすべて**です。
6.  クリックして **カスタム ドメインと SSL**します。
7.   **カスタム ドメインと SSL** ブレードで、をクリックして **[外部ドメイン"**します。 IP アドレスは、この部分の下部にあります。

## DNS レコードの作成

ドメイン レジストラーにログインし、ツールを使用して A レコードまたは CNAME レコードを追加します。 レジストラーの Web アプリはすべて少しずつ異なりますが、一般的なガイドラインは次のとおりです。

1.  DNS レコードの管理ページを探します。 というラベルが付いたサイトのリンクまたは領域を探します **ドメイン名**, 、**DNS**, 、または **ネーム サーバー管理**します。 リンクのある多くの場合、アカウント情報を表示しなどのリンクを検索して **My domains**します。
2.  管理ページが見つかったら、DNS レコードを追加または編集できるリンクを探します。 これは、として示される可能性があります、 **ゾーン ファイル**, 、**DNS レコード**, 、または、 **詳細** 構成リンクします。

このページには、A レコードと CNAME レコードが別々に表示されたり、レコードの種類を選択するためのドロップダウンが表示されたりする場合があります。 また、使用して、レコードの種類の他の名前など、 **IP アドレス レコード** A レコードまたは **エイリアス レコード** CNAME レコードの代わりにします。  通常、レジストラーで作成レコードの一部をする可能性がある既にのルート ドメインや共通サブドメインのレコードなど **www**します。

レコードを作成または編集すると、フィールドでドメイン名を IP アドレス (A レコードの場合) または別のドメイン (CNAME レコードの場合) に割り当てられるようになります。 マップする CNAME レコード *から* 、カスタム ドメイン *に* azurewebsites.net サブドメインです。

多くのレジストラー ツールでは、ドメイン名すべてではなく、ドメインのサブドメイン部分のみを入力します。 また多くのツールでは、ルート ドメインを "@" で表します。 次に例を示します。

<table cellspacing="0" border="1">
  <tr>
    <th>ホスト</th>
    <th>レコードの種類</th>
    <th>IP アドレスまたは URL</th>
  </tr>
  <tr>
    <td>@</td>
    <td>A (アドレス)</td>
    <td>168.62.48.183</td>
  </tr>
  <tr>
    <td>www</td>
    <td>CNAME (エイリアス)</td>
    <td>contoso.azurewebsites.net</td>
  </tr>
</table>

ここでカスタム ドメイン名を "contoso.com" とすると、次のレコードが作成されます。

- **contoso.com** 168.62.48.183 にマップします。
- **www.contoso.com** にマップされている **contoso.azurewebsites.net**します。

>[AZURE.NOTE] Azure DNS を使用すると、web アプリのために必要なドメイン レコードをホストします。 カスタム ドメインを構成し、Azure DNS では、レコードを作成する、次を参照してください。 [web アプリのカスタム DNS レコードを作成する](../dns-web-sites-custom-domain)です。

<a name="awverify" />
## awverify レコードの作成 (A レコードのみ)

A レコードを作成する場合、使用しようとするドメインを自分が所有していることを確認するための特別な CNAME レコードも Web アプリに作成する必要があります。 この CNAME レコードの形式は次のとおりです。

- *A レコードがルート ドメインまたはワイルドカード ドメイン マップするかどうか:* からマップする CNAME レコードを作成する **awverify. & lt; yourdomain & gt;** に **awverify. & lt; yourwebappname & gt;。*.azurewebsites.net**します。  たとえば、A レコードが **contoso.com**, 、CNAME レコードを作成 **awverify.contoso.com**します。
- *A レコードが特定のサブドメインをマップする場合:* からマップする CNAME レコードを作成する **awverify. & lt; サブドメイン、& gt;** に **awverify. & lt; yourwebappname & gt;。*.azurewebsites.net**します。 たとえば、A レコードが **blogs.contoso.com**, 、CNAME レコードを作成 **awverify.blogs.contoso.com**します。

awverify サブドメインは、Web　アプリの訪問者に対して表示されることはなく、Azure がドメインの所有者を確認するためだけに使用されます。

## Web アプリケーションでのドメイン名の有効化

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## DNS への反映を確認する

構成手順が終了した後、DNS プロバイダーによっては、変更が反映されるまでしばらく時間がかかることがあります。 DNS の伝達を使用して正しく動作することを確認する [http://digwebinterface.com/](http://digwebinterface.com/)します。 サイトを参照した後は、ホスト名を指定] をクリックし、テキスト ボックスに **掘り下げる**します。 結果を確認して、最新の変更が有効になっているかどうかを確認します。  

![](./media/web-sites-custom-domain-name/1-digwebinterface.png)

> [AZURE.NOTE] DNS エントリの伝達には、最大 48 時間 (なったことがあります) がかかります。 すべてを正しく構成している場合でも、反映が正常に行われるまで待つ必要があります。

## 次のステップ

詳細については次を参照してください: [Azure DNS の概要](../dns/dns-getstarted-create-dnszone.md) と [Azure DNS へのドメインの委任](../dns/dns-domain-delegation.md)

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- Anchors. -->
[Overview]: #overview
[DNS record types]: #dns-record-types
[Find the virtual IP address]: #find-the-virtual-ip-address
[Create the DNS records]: #create-the-dns-records
[Enable the domain name on your web app]: #enable-the-domain-name-on-your-web-app

<!-- Images -->
[subdomain]: media/web-sites-custom-domain-name/azurewebsites-subdomain.png

