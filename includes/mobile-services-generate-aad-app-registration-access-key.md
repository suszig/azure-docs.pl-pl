1. クリックして **アプリケーション** ] タブでディレクトリ ページで、 [Azure クラシック ポータル](https://manage.windowsazure.com/)します。

2. 統合アプリケーションの登録をクリックします。

3. アプリケーション ページで **[構成]** をクリックし、ページの **[キー]** セクションまで下にスクロールします。
4. 新しいキーの期間として、**[1 年]** をクリックします。 その後、**[保存]** をクリックすると、ポータルに新しいキー値が表示されます。
5. 保存後に表示された **[クライアント ID]** と **[キー]** をコピーします。 キー値は保存後に 1 回しか表示されないことに注意してください。

    ![](./media/mobile-services-generate-aad-app-registration-access-key/client-id-and-key.png)

6. 統合アプリケーションの構成ページの最下部までスクロールして、アプリケーションの **[ディレクトリ データの読み取り]** アクセス許可を有効にし、**[保存]** をクリックします。

    ![](./media/mobile-services-generate-aad-app-registration-access-key/app-perms.png)

7. [Azure クラシック ポータル](https://manage.windowsazure.com/), 、モバイル サービスに戻り] をクリックして、 **構成** ] タブをクリックします。 **[アプリケーション設定]** セクションまで下にスクロールし、次のアプリケーション設定を追加して、**[保存]** をクリックします。

    <table border="1">
    <tr>
    <th>アプリケーション設定の名前</th><th>の説明</th>
    </tr>
    <tr>
    <td>AAD_CLIENT_ID</td><td>前の手順で統合アプリケーションからコピーしたクライアント id。</td>
    </tr>
    <tr>
    <td>AAD_CLIENT_KEY</td><td>統合前の手順でアプリの AAD で生成したアプリケーション キー。</td>
    </tr>
    <tr>
    <td>AAD_TENANT_DOMAIN</td><td>AAD ドメイン名です。"Mydomain.onmicrosoft.com"のようになります</td>
    </tr>
    </table><br/>

    ![](./media/mobile-services-generate-aad-app-registration-access-key/aad-app-settings.png)





