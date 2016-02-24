1. モバイル アプリ バックエンドを Azure Active Directory テナントに登録、 [How to configure your Mobile App with Azure Active Directory] トピックです。

2. 移動 **Active Directory** で、 [Azure classic portal]

   ![](./media/app-service-mobile-adal-register-app/app-service-navigate-aad.png)

3. ディレクトリを選択してから、 **アプリケーション** 上部にあるタブをクリックします。 クリックして **追加** 下部には、新しいアプリケーションの登録を作成します。 

4. クリックして **[組織が開発中のアプリケーションを追加**します。

5. アプリケーションの追加ウィザードで入力、 **名** ] をクリックしてアプリケーションを  **ネイティブ クライアント アプリケーション** 型です。 その後、クリックして続行します。

6.  **リダイレクト URI** ボックスで、「ログイン/App Service ゲートウェイ用エンドポイントを実行します。 この値は、https://contoso.azurewebsites.net/login/done のようにする必要があります。

7. ネイティブ アプリケーションが追加されると、クリックして、 **構成** ] タブをクリックします。 コピー、 **クライアント ID**します。 この情報は後で必要になります。

8. ページを下へスクロール、 **他のアプリケーションに対するアクセス許可** セクションを **アプリケーションの追加**します。

9. 前に登録した Web アプリケーションを検索し、プラス アイコンをクリックします。 次に、チェックをクリックしてダイアログ ボックスを閉じます。

10. 新しいエントリを追加した上で開きます、 **委任されたアクセス許可** ドロップダウンを選択して **アクセス (appName)**します。 クリックして **保存**

   ![](./media/app-service-mobile-adal-register-app/aad-native-client-add-permissions.png)

これで、アプリケーションが AAD 内に構成され、ユーザーは AAD サインオンを使用してログインできます。

[Azure classic portal]: https://manage.windowsazure.com/
[How to configure your Mobile App with Azure Active Directory]: ../articles/app-service-how-to-configure-active-directory-authentication.md

