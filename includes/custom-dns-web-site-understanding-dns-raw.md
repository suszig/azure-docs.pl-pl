ドメイン ネーム システム (DNS) は、インターネット上でリソースを検索するために使用されます。 たとえば、ブラウザーに Web アプリのアドレスを入力するか、Web ページでリンクをクリックすると、DNS を使用してドメインが IP アドレスに変換されます。 IP アドレスは住所に似ていますが、人間にとってわかりやすい情報ではありません。 たとえばのように DNS 名には **contoso.com** 192.168.1.88 や 2001:0:4137:1f67:24a2:3888:9cce:fea3 など、IP アドレスを保存するよりもします。

DNS システムはに基づいた *レコード*します。 レコードに関連付ける特定 *名前*, など **contoso.com**, IP アドレスまたは別の DNS 名のいずれかを指定しています。 Web ブラウザーなどのアプリケーションは、DNS で名前を検索し、レコードを検索して、そのレコードが指すアドレスを使用します。 レコードが指す値が IP アドレスの場合、ブラウザーはその値を使用します。 レコードが別の DNS 名を指す場合、アプリケーションは、もう一度名前を解決する必要があります。 こうして最終的には、すべての名前が解決され、IP アドレスが取得されます。

DNS 名は、App Service で Web アプリを作成すると、その Web アプリに自動的に割り当てられます。 この名前は、フォームの **& lt; yourwebappname & gt;。*.azurewebsites.net**します。 仮想 IP アドレスが使用可能な DNS レコードの作成時に指すレコードを作成するか、 **. azurewebsites.net**, 、または IP アドレスをポイントしていることができます。

> [AZURE.NOTE] 削除し、web アプリを再作成または App Service プラン モードに変更を実行する web アプリの IP アドレスが変更 **Free** 設定されている **基本的な**, 、**共有**, 、または **標準**します。

それぞれに独自の機能と制限については、レコードの複数の種類もありますが、web アプリに関して重要な 2 つの *A* と *CNAME* レコードです。

###アドレス レコード (A レコード)

A レコードがなど、ドメインをマップ **contoso.com** または **www.contoso.com**, 、*またはワイルドカード ドメイン* など **\*.contoso.com**, 、IP アドレスにします。 App Service の Web アプリの場合、サービスの仮想 IP または Web アプリ用に購入した特定の IP アドレスです。

CNAME レコードと比較したときの A レコードの主な利点は次のとおりです。

* などのルート ドメインをマップする **contoso.com** ; IP アドレスに多くのレジストラーのみを許可する A レコードを使用してこの

* など、ワイルドカードを使用する 1 つのエントリがあることができます **\*.contoso.com**, などの複数のサブドメインの要求を処理 **mail.contoso.com**, 、**blogs.contoso.com**, 、または **www.contso.com**します。

> [AZURE.NOTE] A レコードは静的 IP アドレスにマップされる、ため web アプリの IP アドレスに自動的に変更を解決するそのことはできません。 Web アプリ; のカスタム ドメイン名の設定を構成するときに、A レコードと共に使用する IP アドレスが提供されます。ただし、この値が変わってしまう場合削除し、web アプリを再作成または App Service プラン モードに戻すを変更する **Free**します。

###エイリアス レコード (CNAME レコード)

CNAME レコードは、マップ、 *特定* DNS 名などを **mail.contoso.com** または **www.contoso.com**, 、別の (正規の) ドメイン名にします。 正規のドメイン名は、App Service Web アプリの場合、 **& lt; yourwebappname >. azurewebsites.net** web アプリのドメイン名です。 作成すると、CNAME のエイリアスを作成、 **& lt; yourwebappname >. azurewebsites.net** ドメイン名です。 CNAME エントリは、の IP アドレスを解決する、 **& lt; yourwebappname >. azurewebsites.net** ため web アプリの IP アドレスが変更された場合は、操作を行う必要はありません、ドメイン名、自動的にします。

> [AZURE.NOTE] 一部のドメイン レジストラーで CNAME レコードをなどを使用する場合は、サブドメインをマップすることが **www.contoso.com**, 、などのルート名ではなく、 **contoso.com**します。 CNAME レコードの詳細については、レジストラーが提供するドキュメント、<a href="http://en.wikipedia.org/wiki/CNAME_record">CNAME レコードに関するウィキペディアの項目</a>、または <a href="http://tools.ietf.org/html/rfc1035">IETF ドメイン名 - 実装と仕様書</a>を参照してください。

###Web アプリ DNS 固有情報

Web アプリで A レコードを使用するには、まず次の CNAME レコードの 1 つを作成する必要があります。

* **ルート ドメインまたはワイルドカード サブドメインの場合** -の DNS 名 **awverify** に  **awverify. & lt; yourwebappname & gt;。*.azurewebsites.net**します。

* **特定のサブドメインの** -の DNS 名 **awverify. & lt; サブドメイン >** に **awverify. & lt; yourwebappname & gt;。*.azurewebsites.net**します。 たとえば、 **awverify.blogs** 場合は A レコードが **blogs.contoso.com**します。

この CNAME レコードを使用して、目的のドメインを自分が所有していることを確認します。 また、Web アプリの仮想 IP アドレスを指す A レコードを作成する必要があります。

IP アドレスを確認するだけでなく、 **awverify** 名と **. azurewebsites.net** 、次の手順を実行することによって web アプリの名前。

1. ブラウザーで開く、 [Azure ポータル](https://portal.azure.com)します。

2.  **Web Apps** ブレードで、web アプリの名前をクリックして、選択 **設定をすべて**, 、し、[ **カスタム ドメインと SSL** 、ページの下部にあります。

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3.  **カスタム ドメインと SSL** ブレードで、をクリックして **外部ドメインの表示**します。

    ![](./media/custom-dns-web-site/dncmntask-cname-7.png)

    > [AZURE.NOTE] カスタム ドメイン名を使用することはできません、 **Free** web アプリで、App Service プランをアップグレードする必要があります **Shared**, 、**基本的な**, 、**標準**, 、または **Premium** 層です。 詳細については、App Service プランの価格設定、web アプリの価格レベルを変更する方法など、階層を参照してください [web アプリをスケーリングする方法](../articles/web-sites-scale.md)します。

6.  **外部ドメインの表示** ブレードを確認、 **awverify** については、現在割り当てられている **. azurewebsites.net** ドメイン名、および仮想 IP アドレスです。 DNS レコードを作成する際に使用するため、この情報を保存します。

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

