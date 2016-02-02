## <a name="register-app-aad"></a>Azure Active Directory でクライアント アプリを登録します。

1. 移動 **Active Directory** で、 [Azure クラシック ポータル](https://manage.windowsazure.com/), 、目的のディレクトリをクリックします。

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-select-aad.png)

2. 上部にある **[アプリケーション]** タブをクリックし、アプリケーションの **[追加]** をクリックします。

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-aad-applications-tab.png)

3. **[組織で開発中のアプリケーションを追加]** をクリックします。

4. アプリケーションの追加ウィザードで入力、 **名** ] をクリックしてアプリケーションを  **ネイティブ クライアント アプリケーション** 型です。 その後、クリックして続行します。

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-native-selection.png)

5. **[リダイレクト URI]** ボックスで、モバイル サービスの /login/done エンドポイントを入力します。 この値は、https://todolist.azure-mobile.net/login/done と同様です。

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-native-redirect-uri.png)

6. ネイティブ アプリケーションの **[構成]** タブをクリックし、**クライアント ID** をコピーします。 この情報は後で必要になります。

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-native-client-id.png)

7. **[他のアプリケーションに対するアクセス許可]** セクションまで下にスクロールし、 **[アプリケーションの追加]** ボタンをクリックします。 [表示]メニューから **[その他]** を選択して todo を検索します。 **[TodoList]** をクリックして以前に登録したモバイル サービスに追加し、チェックマークをクリックします。 モバイル サービス アプリケーションへのアクセスを許可します。 その後、**[保存]** をクリックします。

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-native-add-permissions.png)

これで、モバイル サービスが、アプリケーションからシングル サインオン ログインを受信するように AAD で構成されます。





