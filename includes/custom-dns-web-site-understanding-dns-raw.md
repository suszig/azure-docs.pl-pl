ドメイン ネーム システム (DNS) は、インターネット上でリソースを検索するために使用されます。 たとえば、ブラウザーに Web アプリのアドレスを入力するか、Web ページでリンクをクリックすると、DNS を使用してドメインが IP アドレスに変換されます。 IP アドレスは住所に似ていますが、人間にとってわかりやすい情報ではありません。 たとえば、**contoso.com** のような DNS 名の方が、192.168.1.88 や 2001:0:4137:1f67:24a2:3888:9cce:fea3 などの IP アドレスよりはるかに覚えやすい情報です。

DNS システムは*レコード*に基づいたしくみです。 *contoso.com* のような特定の**名前**が、レコードによって IP アドレスまたは別の DNS 名に関連付けられます。 Web ブラウザーなどのアプリケーションは、DNS で名前を検索し、レコードを検索して、そのレコードが指すアドレスを使用します。 レコードが指す値が IP アドレスの場合、ブラウザーはその値を使用します。 レコードが別の DNS 名を指す場合、アプリケーションは、もう一度名前を解決する必要があります。 こうして最終的には、すべての名前が解決され、IP アドレスが取得されます。

DNS 名は、App Service で Web アプリを作成すると、その Web アプリに自動的に割り当てられます。 この名前は、フォームの **< yourwebappname >. azurewebsites.net**します。 また DNS レコードの作成時に仮想 IP アドレスを使用することもできるため、**.azurewebsites.net** を指すレコードを作成するか、または IP アドレスをポイントできます。
> [AZURE.NOTE] Web アプリを削除して再作成した場合や、**Basic** モード、**Shared** モード、または **Standard** モードに設定されている Web アプリを **Free** モードに変更した場合は、IP アドレスが変更されます。

レコードにも複数の種類があり、それぞれに独自の機能と制約がありますが、Web アプリに関して重要なのは *A* レコードと *CNAME* レコードの 2 種類です。

### アドレス レコード (A レコード)

A レコードがなど、ドメインをマップ **contoso.com** または **www.contoso.com**, 、*またはワイルドカード ドメイン* など **\*.contoso.com**, 、IP アドレスにします。 App Service の Web アプリの場合、サービスの仮想 IP または Web アプリ用に購入した特定の IP アドレスです。

CNAME レコードと比較したときの A レコードの主な利点は次のとおりです。

* **contoso.com** などのルート ドメインを IP アドレスにマップすることができます。多くのレジストラーでは、このようなマッピングには A レコードの使用が必須です。

* など、ワイルドカードを使用する 1 つのエントリがあることができます **\*.contoso.com**, などの複数のサブドメインの要求を処理 **mail.contoso.com**, 、**blogs.contoso.com**, 、または **www.contso.com**します。

> [AZURE.NOTE] A レコードは静的 IP アドレスにマップされるため、変更を Web アプリの IP アドレスに自動的に解決することはできません。 A レコードと共に使用する IP アドレスは、Web アプリのカスタム ドメイン名を設定する場合に提供されます。ただし、この値は Web アプリを削除または再作成する場合や App Service のプランを **Free** モードに戻す場合に変更されることがあります。

### エイリアス レコード (CNAME レコード)

CNAME レコードは、*mail.contoso.com* や **www.contoso.com** などの**特定の** DNS 名をドメインを別の (正規の) ドメイン名にマップします。 正規のドメイン名は、App Service Web アプリの場合、 **< yourwebappname >. azurewebsites.net** web アプリのドメイン名です。 作成すると、CNAME のエイリアスを作成、 **< yourwebappname >. azurewebsites.net** ドメイン名です。 CNAME エントリの IP アドレスに解決するには、 **< yourwebappname >. azurewebsites.net** ため web アプリの IP アドレスが変更された場合は、操作を行う必要はありません、ドメイン名、自動的にします。
> [AZURE.NOTE] いくつかのドメイン レジストラーでは、CNAME レコードを使用する場合にマップすることが許可されるのは、ルート名 (**contoso.com** など) ではなく、サブドメイン (**www.contoso.com** など) のみです。 CNAME レコードの詳細については、レジストラーが提供するドキュメントを参照してください。 <a href="http://en.wikipedia.org/wiki/CNAME_record">CNAME レコードに関するウィキペディア</a>, 、または <a href="http://tools.ietf.org/html/rfc1035">IETF ドメイン名 - 実装と仕様書</a> ドキュメントです。

### Web アプリ DNS 固有情報

Web アプリで A レコードを使用するには、まず次の CNAME レコードの 1 つを作成する必要があります。

* **、ルート ドメインまたはワイルドカード サブドメインの場合** -の DNS 名 **awverify** に  **awverify. < yourwebappname >. azurewebsites.net**します。

* **特定のサブドメインの** -の DNS 名 **awverify. < サブドメイン >** に **awverify. < yourwebappname >. azurewebsites.net**します。 たとえば、A レコードが **blogs.contoso.com** に対応する場合には、**awverify.blogs** となります。

この CNAME レコードを使用して、目的のドメインを自分が所有していることを確認します。 また、Web アプリの仮想 IP アドレスを指す A レコードを作成する必要があります。

IP アドレスだけでなく、Web アプリの **awverify** 名と **.azurewebsites.net** 名が、次の手順で検索できます。

1. ブラウザーで開く、 [Azure ポータル](https://portal.azure.com)します。

2. **[Web Apps]** ブレードで、Web アプリの名前をクリックし、**[すべての設定]** を選択し、ページ下部の **[カスタム ドメインと SSL]** をクリックします。

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. **カスタム ドメインと SSL** ブレードで、をクリックして **外部ドメインの表示**します。

    ![](./media/custom-dns-web-site/dncmntask-cname-7.png)
    > [AZURE.NOTE] カスタム ドメイン名を使用することはできません、 **Free** web アプリで、App Service プランをアップグレードする必要があります **Shared**, 、**基本的な**, 、**標準**, 、または **Premium** 層です。 詳細については、App Service プランの価格設定、web アプリの価格レベルを変更する方法など、階層を参照してください [web アプリをスケーリングする方法](../articles/web-sites-scale.md)します。

6. **外部ドメインの表示** ブレードを確認、 **awverify** については、現在割り当てられている **. azurewebsites.net** ドメイン名、および仮想 IP アドレスです。 DNS レコードを作成する際に使用するため、この情報を保存します。

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)





