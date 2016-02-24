ドメイン ネーム システム (DNS) は、インターネット上で要素を検索するために使用されます。 たとえば、ブラウザーにアドレスを入力するか、Web ページでリンクをクリックすると、DNS を使用してドメインが IP アドレスに変換されます。 IP アドレスは住所に似ていますが、人間にとってわかりやすい情報ではありません。 たとえばのように DNS 名には **contoso.com** 192.168.1.88 や 2001:0:4137:1f67:24a2:3888:9cce:fea3 など、IP アドレスを保存するよりもします。

DNS システムはに基づいた *レコード*します。 レコードに関連付ける特定 *名前*, など **contoso.com**, IP アドレスまたは別の DNS 名のいずれかを指定しています。 Web ブラウザーなどのアプリケーションは、DNS で名前を検索し、レコードを検索して、そのレコードが指すアドレスを使用します。 レコードが指す値が IP アドレスの場合、ブラウザーはその値を使用します。 レコードが別の DNS 名を指す場合、アプリケーションは、もう一度名前を解決する必要があります。 こうして最終的には、すべての名前が解決され、IP アドレスが取得されます。

DNS 名は、Azure Web サイトの作成時に、サイトに自動的に割り当てられます。 この名前は、フォームの **& lt; yoursitename & gt;。*.azurewebsites.net**します。 Azure Traffic Manager エンドポイントとして web サイトを追加するときに、web サイト経由にアクセス、 **& lt; yourtrafficmanagerprofile & gt;。"trafficmanager.net"** ドメイン。

> [AZURE.NOTE] Web サイトが Traffic Manager エンドポイントとして構成されている場合は、使用、 **. <myapp>.trafficmanager.net** DNS レコードを作成する際に対処します。

> Traffic Manager では、CNAME レコードのみ使用できます。

それぞれに独自の機能と制限については、レコードの複数の種類もありますが、Traffic Manager エンドポイントとして構成された web サイト、重要なのうちの 1 つです。 *CNAME* レコードです。

###CNAME レコードまたはエイリアス レコード

CNAME レコードは、マップ、 *特定* DNS 名などを **mail.contoso.com** または **www.contoso.com**, 、別の (正規の) ドメイン名にします。 Traffic Manager を使用して、Azure Websites の場合、正規のドメイン名は、 **& lt; myapp >. <myapp>.trafficmanager.net** Traffic Manager プロファイルのドメイン名です。 作成すると、CNAME のエイリアスを作成、 **& lt; myapp >. <myapp>.trafficmanager.net** ドメイン名です。 CNAME エントリの IP アドレスに解決するには、 **& lt; myapp > です。"trafficmanager.net"** ため、web サイトの IP アドレスが変更された場合は、操作を行う必要はありません、ドメイン名、自動的にします。

Traffic Manager に到着したトラフィックは、構成された負荷分散方法を使用して、Web サイトにルーティングされます。 これは、Web サイトにアクセスするユーザーに対して透過的です。 ユーザーのブラウザーには、カスタム ドメイン名のみが表示されます。

> [AZURE.NOTE] 一部のドメイン レジストラーで CNAME レコードをなどを使用する場合は、サブドメインをマップすることが **www.contoso.com**, 、などのルート名ではなく、 **contoso.com**します。 CNAME レコードの詳細については、レジストラーが提供するドキュメント、<a href="http://en.wikipedia.org/wiki/CNAME_record">CNAME レコードに関するウィキペディアの項目</a>、または <a href="http://tools.ietf.org/html/rfc1035">IETF ドメイン名 - 実装と仕様書</a>を参照してください。

