1. クリックして **アプリケーション** ] タブでディレクトリ ページで、 [Azure クラシック ポータル](https://manage.windowsazure.com/)します。
  
2. 統合アプリケーションの登録をクリックします。

3. クリックして **構成** 下へスクロールして [アプリケーション] ページで、 **キー** ページのセクションです。 
4. クリックして **1 年間** 新しいキーの期間。 クリックして **保存** と、ポータルは新しいキー値を表示します。
5. コピー、 **クライアント ID** と **キー** を保存した後に示すようにします。 キー値は保存後に 1 回しか表示されないことに注意してください。 

    ![](./media/mobile-services-generate-aad-app-registration-access-key-rbac/client-id-and-key.png)

6. 統合アプリケーションの構成] ページの一番下までスクロールし、有効にする、 **ディレクトリ データを読み取る** をクリックして、アプリケーションに対するアクセス許可 **保存**します。

    ![](./media/mobile-services-generate-aad-app-registration-access-key-rbac/app-perms.png)


7.  [Azure クラシック ポータル](https://manage.windowsazure.com/), 、モバイル サービスに戻り] をクリックして、 **構成** ] タブをクリックします。 下へスクロールして、 **アプリ設定** セクションで、次のアプリケーション設定を追加し、] をクリックして **保存**します。 

    <table border="1">
    <tr>
    <th>アプリケーション設定の名前</th><th>説明</th>
    </tr>
    <tr>
    <td>AAD_CLIENT_ID</td><td>前の手順で統合アプリケーションからコピーしたクライアント ID。</td>
    </tr>
    <tr>
    <td>AAD_CLIENT_KEY</td><td>前の手順で AAD 統合アプリケーションで生成されたアプリケーション キー。</td>
    </tr>
    <tr>
    <td>AAD_TENANT_DOMAIN</td><td>AAD ドメイン名。 "mydomain.onmicrosoft.com" のような形式にする必要があります。</td>
    </tr>
    <tr>
    <td>AAD_GROUP_ID</td><td>前のセクションで書き留めた Sales グループのグループ ID</td>
    </tr>
    </table><br/>

 
    ![](./media/mobile-services-generate-aad-app-registration-access-key-rbac/aad-app-settings.png)
  
