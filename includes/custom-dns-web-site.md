#Azure の Web サイトのカスタム ドメイン名の構成

Web サイトを作成するときに Azure は、ユーザーが http://&lt;mysite>.azurewebsites.net ような URL を使用して web サイトにアクセスできるように、azurewebsites.net ドメイン上フレンドリ サブドメインを提供します。 ただし、Web サイトを Shared モードまたは Standard モード用に構成すると、Web サイトを独自のドメイン名にマップできます。

必要に応じて、Azure Traffic Manager を使用して Web サイトへの着信トラフィックを負荷分散できます。 Web サイトが Traffic Manager のしくみの詳細については、次を参照してください。 [Azure Traffic Manager での Azure Web サイト トラフィックの制御][trafficmanager]します。

> [AZURE.NOTE] このタスクの手順は、Azure の web サイトに適用します。クラウド サービスを参照してください。 <a href="/develop/net/common-tasks/custom-dns/">Azure でのカスタム ドメイン名の構成</a>します。

> [AZURE.NOTE] このタスクの手順を Shared モードまたは標準モードで、サブスクリプションに対して課金されるがどの程度変わる可能性がある web サイトを構成する必要があります。 詳細については、<a href="/pricing/details/web-sites/">Websites の料金詳細</a>に関するページを参照してください。

この記事の内容:

-   [CNAME レコードと A レコードについて](#understanding-records)
-   [Web サイトの共有または標準モード用の構成](#bkmk_configsharedmode)
-   [Web サイトの Traffic Manager への追加](#trafficmanager)
-   [カスタム ドメインの CNAME レコードの追加](#bkmk_configurecname)
-   [カスタム ドメインの A レコードの追加](#bkmk_configurearecord)

<h2><a name="understanding-records"></a>Cname レコードと A レコードのレコードをについてください。</h2>

CNAME レコード (またはエイリアス レコード) および A レコードはどちらもドメイン名を Web サイトに関連付けることができますが、それぞれ機能は異なります。

###CNAME レコードまたはエイリアス レコード

CNAME レコードは、マップ、 *特定* ドメインなど **contoso.com** または **www.contoso.com**, 、正規のドメイン名にします。 ここでは、正規のドメイン名では、いずれか、 **& lt; myapp >. azurewebsites.net** Azure website のドメイン名または **(& a) lt; myapp >. <myapp>.trafficmgr.com** Traffic Manager プロファイルのドメイン名です。 作成されると、CNAME のエイリアスを作成、 **& lt; myapp >. azurewebsites.net** または **(& a) lt; myapp >. <myapp>.trafficmgr.com** ドメイン名です。 CNAME エントリは、の IP アドレスを解決する、 **& lt; myapp >. azurewebsites.net** または **(& a) lt; myapp >. <myapp>.trafficmgr.com** ため、web サイトの IP アドレスが変更された場合は、操作を行う必要はありません、ドメイン名、自動的に。

> [AZURE.NOTE] ドメイン レジストラーの一部では、www.contoso.com、ルート名ではなく、contoso.com などのなどの CNAME レコードを使用した場合に、サブドメインをマップすることのみ許可します。 CNAME レコードの詳細については、レジストラーが提供するドキュメント、<a href="http://en.wikipedia.org/wiki/CNAME_record">CNAME レコードに関するウィキペディアの項目</a>、または <a href="http://tools.ietf.org/html/rfc1035">IETF ドメイン名 - 実装と仕様書</a>を参照してください。

###A レコード

A レコードがなど、ドメインをマップ **contoso.com** または **www.contoso.com**, 、*またはワイルドカード ドメイン* など **\*.contoso.com**, 、IP アドレスにします。 Azure Web サイトの場合、サービスの仮想 IP または Web サイト用に購入した特定の IP アドレスです。 CNAME レコードの A レコードの主な利点が 1 つのエントリなど、ワイルドカードを使用することができますので ***. contoso.com**, などの複数のサブドメインの要求を処理 **mail.contoso.com**, 、**login.contoso.com**, 、または **www.contso.com**します。

> [AZURE.NOTE] A レコードが静的 IP アドレスにマップされているため、web サイトの IP アドレスに自動的に変更を解決する、ことはできません。 A レコードと共に使用する IP アドレスは、Web サイトのカスタム ドメイン名を設定する場合に提供されます。ただし、この値は Web サイトを削除または再作成する場合や Web サイトを無料モードに戻すように変更する場合に変更されることがあります。

> [AZURE.NOTE] Traffic Manager での負荷分散は、a レコードを使用することはできません。 詳細については、次を参照してください。 [Azure Traffic Manager での Azure Web サイト トラフィックの制御][trafficmanager]します。

<a name="bkmk_configsharedmode"></a><h2>Websites の Shared または Standard モード用の構成</h2>

Web サイトのカスタム ドメイン名は、Azure Websites の Shared モードおよび Standard モードでのみ設定できます。 Website を Free Website モードから Shared モードまたは Standard モードに切り替える場合、最初に Website サブスクリプションに設定されている使用制限を解除する必要があります。 共有モードと標準モードの料金の詳細については、次を参照してください。 [の料金詳細][PricingDetails]します。

1. ブラウザーで開く、 [管理ポータル][portal]します。
2.  **Websites** ] タブで、サイトの名前をクリックします。

    ![][standardmode1]

3. クリックして、 **スケール** ] タブをクリックします。

    ![][standardmode2]


4.  **全般** 」をクリックして web サイトのモードを設定 **SHARED**します。

    ![][standardmode3]

    > [AZURE.NOTE] この web サイトで Traffic Manager を使用する場合は、共有ではなく Standard モードを使わなければなりません。

5. クリックして **保存**します。
6. 共有モード (または同様の標準モード) のコストの増加に関するメッセージが表示されたら、クリックして **はい** 同意する場合。

    <!--![][standardmode4]-->

    **注**<br />
    "'Web サイト名' の Web サイトのスケールの構成に失敗しました" というエラーが発生する場合は、詳細ボタンを使用して詳細情報を表示できます。

<a name="trafficmanager"></a><h2>(省略可能) Websites の Traffic Manager への追加</h2>

Website で Traffic Manager を使用する場合は、次の手順を実行します。

1. Traffic Manager プロファイルがあるない場合は、内の情報を使用して [簡易作成による Traffic Manager プロファイルを作成する][createprofile] を作成します。 注、 **. <myapp>.trafficmgr.com** Traffic Manager プロファイルに関連付けられているドメイン名です。 これは、後の手順で使用します。

2. 内の情報を使用して [の追加と削除エンドポイント][addendpoint] Traffic Manager プロファイルでエンドポイントとして、web サイトを追加します。

    > [AZURE.NOTE] エンドポイントを追加するときに、web サイトが表示されていない場合は、標準モードで構成されていることを確認します。 Web サイトを Traffic Manager で操作するには、Standard モードを使用する必要があります。

3. DNS レジストラーの Web サイトにログオンし、DNS の管理ページに移動します。 としてというラベルが付いたサイトのリンクまたは領域を探します **ドメイン名**, 、**DNS**, 、または **ネーム サーバー管理**します。

4. ここで CNAME レコードを選択または入力する場所を探します。 一覧からレコードの種類を選択するか、詳細設定ページに進まなければならない場合があります。 単語を探します **CNAME**, 、**エイリアス**, 、または **サブドメイン**します。

5. また、CNAME のドメインまたはサブドメイン エイリアスを指定する必要があります。 たとえば、 **www** のエイリアスを作成する場合は、 **www.customdomain.com**します。

5. また、この CNAME エイリアスの正規のドメイン名であるホスト名を指定する必要があります。 これは、 **. <myapp>.trafficmgr.com** 、web サイトの名前。

たとえば、次の CNAME レコードがからすべてのトラフィックを転送 **www.contoso.com** に **contoso.trafficmgr.com**, 、web サイトのドメイン名。

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>エイリアス/ホスト名/サブドメイン</strong></td>
<td><strong>正規のドメイン</strong></td>
</tr>
<tr>
<td>www</td>
<td>contoso.trafficmgr.com</td>
</tr>
</table>

訪問者 **www.contoso.com** 本当のホストは参照できません
(contoso.azurewebsite.net) を認識できないため、転送プロセスはエンド ユーザーに表示されません。

> [AZURE.NOTE] Web サイトで Traffic Manager を使用している場合は、次のセクションの手順を実行する必要はありません '**カスタム ドメインの cname レコードの追加**'と'**カスタム ドメインの A レコードの追加**' です。 前の手順で作成された CNAME レコードは、着信トラフィックを Traffic Manager にルーティングし、これは Web サイトのエンドポイントにルーティングされます。

<a name="bkmk_configurecname"></a><h2>カスタム ドメインの CNAME レコードの追加</h2>

CNAME レコードを作成するには、レジストラーから提供されるツールを使用して、カスタム ドメイン用の DNS テーブルに新しいエントリを追加する必要があります。 それぞれのレジストラーでは CNAME レコードを指定するための同様の (多少異なる) 手段が用意されていますが、その概念は同じです。

1. これらのメソッドのいずれかを使用して検索、 **. azurewebsite.net** 、web サイトに割り当てられたドメイン名です。

    * ログイン、 [Azure 管理ポータル][portal], を自分の web サイトを選択し、選択 **ダッシュ ボード**, 、見つけて、 **サイトの URL** 内のエントリ、 **概要** セクションです。

    * インストールし、構成 [Azure Powershell](/manage/install-and-configure-windows-powershell/), 、次のコマンドを使用します。

            get-azurewebsite yoursitename | select hostnames

    * インストールし、構成、 [Azure コマンド ライン インターフェイス](/manage/install-and-configure-cli/), 、次のコマンドを使用します。

            azure site domain list yoursitename

    この保存 **. azurewebsite.net** 名前を次の手順で使用するためです。

3. DNS レジストラーの Web サイトにログオンし、DNS の管理ページに移動します。 としてというラベルが付いたサイトのリンクまたは領域を探します **ドメイン名**, 、**DNS**, 、または **ネーム サーバー管理**します。

4. ここで CNAME レコードを選択または入力する場所を探します。 一覧からレコードの種類を選択するか、詳細設定ページに進まなければならない場合があります。 単語を探します **CNAME**, 、**エイリアス**, 、または **サブドメイン**します。

5. また、CNAME のドメインまたはサブドメイン エイリアスを指定する必要があります。 たとえば、 **www** のエイリアスを作成する場合は、 **www.customdomain.com**します。 として示されるありますルート ドメインのエイリアスを作成する場合、'**@**'、レジストラーの DNS ツール内の記号。

5. また、この CNAME エイリアスの正規のドメイン名であるホスト名を指定する必要があります。 これは、 **. azurewebsite.net** 、web サイトの名前。

たとえば、次の CNAME レコードがからすべてのトラフィックを転送 **www.contoso.com** に **contoso.azurewebsite.net**, 、web サイトのドメイン名。

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>エイリアス/ホスト名/サブドメイン</strong></td>
<td><strong>正規のドメイン</strong></td>
</tr>
<tr>
<td>www</td>
<td>contoso.azurewebsite.net</td>
</tr>
</table>

訪問者 **www.contoso.com** 本当のホストは参照できません
(contoso.azurewebsite.net) を認識できないため、転送プロセスは
エンド ユーザーに表示されません。

> [AZURE.NOTE] 上記の例でのトラフィックのみに該当、 __www__ サブドメインです。 CNAME レコードにはワイルドカードを使用できないため、各ドメインおよびサブドメインに 1 つの CNAME を作成する必要があります。 などのサブドメインでは、トラフィックを送信するかどうか *. contoso.com を azurewebsite.net アドレスを構成できます、 __URL リダイレクト__ または __URL 転送__ 、DNS 設定のエントリまたは A レコードを作成します。

> [AZURE.NOTE] Cname の DNS システムを通じて伝達されるまでに時間がかかることができます。 CNAME が反映されるまで、Website に対して CNAME を設定することはできません。 <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> などのサービスを使用すると、CNAME を利用できるかどうかを確認できます。

###Web サイトへのドメイン名の追加

ドメイン名の CNAME レコードが反映されたら、それを Website に関連付ける必要があります。 Azure コマンド ライン インターフェイス (Azure CLI) または Azure 管理ポータルを使用して、CNAME レコードで定義されたカスタム ドメイン名を Web サイトに追加することができます。

**コマンド ライン ツールを使用してドメイン名を追加するには**

インストールし、構成、 [Azure コマンド ライン インターフェイス](/manage/install-and-configure-cli/), 、次のコマンドを使用します。

    azure site domain add customdomain yoursitename

たとえば、次がのカスタム ドメイン名を追加 **www.contoso.com** に、 **contoso.azurewebsite.net** web サイト。

    azure site domain add www.contoso.com contoso

次のコマンドを使用してカスタム ドメイン名が Web サイトに追加されたことを確認できます。

    azure site domain list yoursitename

このコマンドで返される一覧は、既定値と同様に、カスタム ドメイン名を含める必要があります **. azurewebsite.net** エントリです。

**Azure の管理ポータルを使用してドメイン名を追加するには**

1. ブラウザーで開く、 [Azure 管理ポータル][portal]します。

2.  **Websites** ] タブで、サイトの名前をクリックして **ダッシュ ボード**, 、し、[ **[ドメインの管理** 、ページの下部にあります。

    ![][setcname2]

6.  **ドメイン名** テキスト ボックスに、構成したドメイン名を入力します。

    ![][setcname3]

6. チェック マークをクリックしてドメイン名を受け入れます。

構成が完了したら、カスタム ドメイン名に一覧表示、 **ドメイン名** のセクションで、 **構成** 、web サイトのページです。

<a name="bkmk_configurearecord"></a><h2>カスタム ドメインの A レコードの追加</h2>

A レコードを作成するには、まず、Website の IP アドレスを見つける必要があります。 次に、レジストラーから提供されるツールを使用して、カスタム ドメイン用の DNS テーブルにエントリを追加します。 それぞれのレジストラーでは A レコードを指定するための同様の (多少異なる) 手段が用意されていますが、その概念は同じです。 また、A レコードの作成に加え、A レコードを確認するために使用される CNAME レコードを作成する必要があります。

1. ブラウザーで開く、 [Azure 管理ポータル][portal]します。

2.  **Websites** ] タブで、サイトの名前をクリックして **ダッシュ ボード**, 、し、[ **[ドメインの管理** 画面の下部にあります。

    ![][setcname2]

5.  **カスタム ドメインを管理する** ダイアログ ボックスで、検索 **A レコードを構成するときに使用する IP アドレス**です。 IP アドレスをコピーします。 これは、A レコードの作成時に使用されます。

5.  **カスタム ドメインを管理する** ダイアログ ボックスで、ダイアログ ボックスの上部にあるテキストの末尾の awverify ドメイン名をメモします。  **Awverify.mysite.azurewebsites.net** 場所 **mysite** 、web サイトの名前を指定します。 検証 CNAME レコードを作成する場合に使用されるドメイン名であるため、これをコピーします。

6. DNS レジストラーの Web サイトにログオンし、DNS の管理ページに移動します。 としてというラベルが付いたサイトのリンクまたは領域を探します **ドメイン名**, 、**DNS**, 、または **ネーム サーバー管理**します。

6. A レコードと CNAME レコードを選択または入力する場所を探します。 一覧からレコードの種類を選択するか、詳細設定ページに進まなければならない場合があります。

7. A レコードを作成するには、次のステップを実行します。

    1. A レコードを使用するドメインまたはサブドメインを選択または入力します。 たとえば、[ **www** のエイリアスを作成する場合は、 **www.customdomain.com**します。 すべてのサブドメインのワイルドカード エントリを作成する場合は、入力 '__*__' です。 これはすべてのサブドメインなど対象 **mail.customdomain.com**, 、**login.customdomain.com**, 、および **www.customdomain.com**します。

        として示されるありますルート ドメインの A レコードを作成する場合、'**@**'、レジストラーの DNS ツール内の記号。

    2. 表示されたフィールドのクラウド サービスの IP アドレスを入力します。 これによって、A レコードで使用されるドメイン エントリがクラウド サービスのデプロイの IP アドレスに関連付けられます。

        たとえば、次のレコードからのすべてのトラフィックを転送する **contoso.com** に **137.135.70.239**, 、デプロイされたアプリケーションの IP アドレス。

        <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
        <tr>
        <td><strong>ホスト名/サブドメイン</strong></td>
        <td><strong>IP アドレス</strong></td>
        </tr>
        <tr>
        <td>@</td>
        <td>137.135.70.239</td>
        </tr>
        </table>

        この例では、ルート ドメインの A レコードを作成する方法を示します。 すべてのサブドメインをカバーするワイルドカード エントリを作成する場合は、入力 '__*__'、サブドメインとして。

7. 次のエイリアスがある CNAME レコードを作成 **awverify**, 、および正規のドメインを **awverify.mysite.azurewebsites.net** 既に取得してあります。

    > [AZURE.NOTE] Awverify のエイリアスは、レジストラーの一部の動作が、awverify.www.customdomainname.com または awverify.customdomainname.com の完全エイリアス ドメイン名がある他のユーザーがあります。

    たとえば、次は A レコード構成を確認するために使用する CNAME レコードを作成します。

    <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
    <tr>
    <td><strong>エイリアス/ホスト名/サブドメイン</strong></td>
    <td><strong>正規のドメイン</strong></td>
    </tr>
    <tr>
    <td>awverify</td>
    <td>awverify.contoso.azurewebsites.net</td>
    </tr>
    </table>

> [AZURE.NOTE] Awverify CNAME が DNS システムを経由して反映されるまで時間がかかることができます。 awverify CNAME が反映されるまで、Website に対して A レコードで定義されたカスタム ドメイン名を設定することはできません。 <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> などのサービスを使用すると、CNAME を利用できるかどうかを確認できます。

###Web サイトへのドメイン名の追加

後に、 **awverify** ドメイン名の CNAME レコードが反映されたら、web サイトで A レコードで定義されたカスタム ドメインを関連付けることができます。 Azure CLI または Azure 管理ポータルを使用して、A レコードで定義されたカスタム ドメイン名を Web サイトに追加することができます。

**Azure コマンド ライン インターフェイス (Azure CLI) を使用してドメイン名を追加するには**

インストールし、構成、 [Azure CLI](/manage/install-and-configure-cli/), 、次のコマンドを使用します。

    azure site domain add customdomain yoursitename

たとえば、次がのカスタム ドメイン名を追加 **contoso.com** に、 **contoso.azurewebsite.net** web サイト。

    azure site domain add contoso.com contoso

次のコマンドを使用してカスタム ドメイン名が Web サイトに追加されたことを確認できます。

    azure site domain list yoursitename

このコマンドで返される一覧は、既定値と同様に、カスタム ドメイン名を含める必要があります **. azurewebsite.net** エントリです。

**Azure の管理ポータルを使用してドメイン名を追加するには**

1. ブラウザーで開く、 [Azure 管理ポータル][portal]します。

2.  **Websites** ] タブで、サイトの名前をクリックして **ダッシュ ボード**, 、し、[ **[ドメインの管理** 、ページの下部にあります。

    ![][setcname2]

6.  **ドメイン名** テキスト ボックスに、構成したドメイン名を入力します。

    ![][setcname3]

6. チェック マークをクリックしてドメイン名を受け入れます。

構成が完了したら、カスタム ドメイン名に一覧表示、 **ドメイン名** のセクションで、 **構成** 、web サイトのページです。

> [AZURE.NOTE] Web サイトに A レコードで定義されたカスタム ドメイン名を追加した後は、レジストラーから提供されるツールを使用して awverify CNAME レコードを削除できます。 ただし、今後、別の A レコードを追加するには、新しい A レコードで定義された新しいドメイン名を Website に関連付ける前に awverify レコードを再作成する必要があります。

## 次のステップ

-   [Web サイトの管理方法](/manage/services/web-sites/how-to-manage-websites/)

-   [Web サイトの SSL 証明書の構成](/develop/net/common-tasks/enable-ssl-web-site/)


<!-- Bookmarks -->

[Configure your web sites for shared mode]: #bkmk_configsharedmode
[Configure the CNAME on your domain registrar]: #bkmk_configurecname
[Configure a CNAME verification record on your domain registrar]: #bkmk_configurecname
[Configure an A record for the domain name]:#bkmk_configurearecord
[Set the domain name in management portal]: #bkmk_setcname

<!-- Links -->

[PricingDetails]: /pricing/details/
[portal]: http://manage.windowsazure.com
[digweb]: http://www.digwebinterface.com/
[cloudservicedns]: ../articles/custom-dns.md
[trafficmanager]: ../articles/app-service-web/web-sites-traffic-manager.md
[addendpoint]: ../articles/traffic-manager/traffic-manager-endpoints.md
[createprofile]: ../articles/traffic-manager/traffic-manager-manage-profiles.md

<!-- images -->

[setcname1]: ../media/dncmntask-cname-5.png


<!-- images -->
[standardmode1]: ./media/custom-dns-web-site/dncmntask-cname-1.png
[standardmode2]: ./media/custom-dns-web-site/dncmntask-cname-2.png
[standardmode3]: ./media/custom-dns-web-site/dncmntask-cname-3.png
[standardmode4]: ./media/custom-dns-web-site/dncmntask-cname-4.png


[setcname2]: ./media/custom-dns-web-site/dncmntask-cname-6.png
[setcname3]: ./media/custom-dns-web-site/dncmntask-cname-7.png


