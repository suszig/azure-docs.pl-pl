4. 別のブラウザー ウィンドウまたはタブで、 [Azure プレビュー ポータル](https://portal.azure.com)します。

3. 移動して、 **API アプリ** Dropbox コネクタのブレードです。 (表示されている場合でも、 **リソース グループ** ブレードで、図の Dropbox コネクタをクリックします)。

4. をクリックして **設定**, 、および、 **設定** ブレードをクリックして **認証**します。

    ![Click Settings](./media/app-service-api-exchange-dropbox-settings/clicksettings.png)

    ![[認証] のクリック](./media/app-service-api-exchange-dropbox-settings/clickauth.png)

5. [認証] ブレードで、クライアント ID とクライアント シークレットを Dropbox サイトを入力し、クリックして **保存**します。

    ![設定の入力と [保存] のクリック](./media/app-service-api-exchange-dropbox-settings/authblade.png)

3. コピー、 **リダイレクト URI** (クライアント ID とクライアント シークレットの上の灰色のボックス) と前の手順で開いたままにしていたページに値を追加します。 

    リダイレクト URI のパターンは次のとおりです。

        [gatewayurl]/api/consent/redirect/[connectorname]

    次に例を示します。

        https://dropboxrgaeb4ae60b7.azurewebsites.net/api/consent/redirect/DropboxConnector

    ![リダイレクト URI の取得](./media/app-service-api-exchange-dropbox-settings/redirecturi.png)

    ![Dropbox アプリの作成](./media/app-service-api-exchange-dropbox-settings/dbappsettings2.png)
