ドメイン名のレコードが反映されると、カスタム ドメイン名を使用して対象の Azure App Services の Web アプリにアクセスできることをブラウザーで確認できます。

> [AZURE.NOTE] Cname の DNS システムを通じて伝達されるまでに時間がかかることができます。 <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> などのサービスを使用すると、CNAME を利用できるかどうかを確認できます。

カスタム ドメイン名がトラフィック マネージャーにルーティングされているため、Web アプリがトラフィック マネージャーのエンドポイントとしてまだ追加されていない場合は、名前を適切に解決するためにエンドポイントとして追加する必要があります。 これにより、トラフィック マネージャーによって対象の Web アプリにルーティングされます。 内の情報を使用して [の追加と削除エンドポイント](../traffic-manager/traffic-manager-endpoints.md) Traffic Manager プロファイルでエンドポイントとして web アプリを追加します。

> [AZURE.NOTE] エンドポイントを追加するときに、web アプリが表示されていない場合は、用に構成されていることを確認 **標準** App Service プラン モードです。 使用する必要があります **標準** Traffic Manager で操作するために web アプリのモードです。

1. ブラウザーで開く、 [Azure ポータル](https://portal.azure.com)します。

2.  **Web Apps** ] タブで、選択に、web アプリの名前をクリックして **設定**, 、し、[ **カスタム ドメインと SSL**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3.  **カスタム ドメインと SSL** ブレードで、をクリックして **外部ドメインの表示**します。

    ![](./media/custom-dns-web-site/dncmntask-cname-7.png)

4. 使用して、 **ドメイン名** ボックスにこの web アプリに関連付ける Traffic Manager ドメイン名 (contoso.trafficmanager.net) を入力します。

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

5. クリックして **保存** ドメイン名の構成を保存します。

    構成が完了したら、カスタム ドメイン名に一覧表示、 **ドメイン名** web アプリのセクションです。

この時点でブラウザーにトラフィック マネージャー ドメイン名 (contoso.trafficmanager.net) を入力して、対象の Web アプリに正常にアクセスできることを確認できます。

