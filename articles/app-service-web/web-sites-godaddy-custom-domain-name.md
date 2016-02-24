<properties
    pageTitle="Azure App Service でのカスタム ドメイン名の構成 (GoDaddy)"
    description="GoDaddy から購入したカスタム ドメイン名を Azure Web Apps で使用する方法"
    services="app-service"
    documentationCenter=""
    authors="erikre"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/23/2015"
    ms.author="erikre"/>

# Azure App Service でのカスタム ドメイン名の構成 (GoDaddy から直接購入した場合)

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[AZURE.INCLUDE [intro](../../includes/custom-dns-web-site-intro.md)]

Azure App Service Web Apps を使用してドメインを購入した場合はの最後の手順を参照してください [Web アプリ用のドメインの購入](custom-dns-web-site-buydomains-web-app.md)します。

この記事から直接購入したカスタム ドメイン名の使用方法については [GoDaddy](https://godaddy.com) と [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)します。

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>
##DNS レコードについて

[AZURE.INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-raw.md)]

<a name="bkmk_configurecname"></a>
## カスタム ドメインの DNS レコードの追加

カスタム ドメインを App Service の Web アプリケーションに関連付けるには、GoDaddy のツールを使用して、新しいエントリをカスタム ドメインの DNS テーブルに追加する必要があります。 次の手順を使用して GoDaddy.com の DNS ツールを見つけます。

1. GoDaddy.com のアカウントにログオンし、選択 **マイ アカウント** し **自分のドメインを管理する**です。 最後に、Azure web アプリで使用し、選択するドメイン名のドロップダウン メニューを選択 **Manage DNS**します。

    ![GoDaddy のカスタム ドメイン ページ](./media/web-sites-godaddy-custom-domain-name/godaddy-customdomain.png)

2.  **Domain details** ] ページで、スクロールして、 **DNS Zone File** ] タブをクリックします。 このセクションを使用して、ドメイン名の DNS レコードを追加したり変更したりします。

    ![[DNS Zone File] タブ](./media/web-sites-godaddy-custom-domain-name/godaddy-zonetab.png)

    選択 **Add Record** を既存のレコードを追加します。

     **編集** 、既存のレコードがペンし、紙のレコードの横にあるアイコン。

    > [AZURE.NOTE] 新しいレコードを追加する前に GoDaddy の一般的なサブドメインの DNS レコードが既に作成ことに注意してください (と呼ばれる **ホスト** エディターで) など、 **電子メール**, 、**ファイル**, 、**メール**, 、およびその他のです。 使用する名前が既に存在する場合は、新しいレコードを作成せずに既存のレコードを変更してください。

4. レコードを追加する場合は、まずレコードの種類を選択する必要があります。

    ![レコードの種類の選択](./media/web-sites-godaddy-custom-domain-name/godaddy-selectrecordtype.png)

    次に、提供する必要があります、 **ホスト** (カスタム ドメインまたはサブドメイン) および **指す**します。

    ![ゾーン レコードの追加](./media/web-sites-godaddy-custom-domain-name/godaddy-addzonerecord.png)

    * 追加するとき、 **(ホスト) レコード** -設定する必要があります、 **ホスト** フィールドが **@** (などのルート ドメイン名を表すこの **contoso.com**,、) * (複数のサブドメインに一致するワイルドカード) またはサブドメインを使用する場合 (たとえば、 **www**.)設定する必要があります、 **指す** フィールドを Azure web アプリの IP アドレスにします。

    * 追加するときに、 **CNAME (エイリアス) レコード** に設定する必要があります、 **ホスト** フィールドを使用するサブドメインにします。 たとえば、 **www**します。 設定する必要があります、 **指す** フィールドを **. azurewebsites.net** Azure web アプリのドメイン名です。 たとえば、 **contoso.azurwebsites.net**します。

5. クリックして **もう 1 つ追加**します。
6. 選択 **CNAME** レコードの種類を指定し、 **ホスト** の値 **awverify** と **を指す** の値 **awverify. & lt; yourwebappname & gt;。*.azurewebsites.net**します。

    > [AZURE.NOTE] この CNAME レコードは Azure 使用を検証する A レコードまたは最初の CNAME レコードで参照されるドメインを所有しているします。 ドメインが Azure ポータルで web アプリにマップされた後、 **awverify** エントリを削除することができます。

5. 追加が完了したらまたはレコードを変更します] をクリックすると **完了** 変更を保存します。

<a name="enabledomain"></a>
## Web アプリケーションでのドメイン名の有効化

[AZURE.INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-web-site.md)]

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

