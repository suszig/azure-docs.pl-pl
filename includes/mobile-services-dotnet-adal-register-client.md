## <a name="register-app-aad"></a>Azure Active Directory へのクライアント アプリケーションの登録

1. 移動 **Active Directory** で、 [Azure クラシック ポータル](https://manage.windowsazure.com/), 、目的のディレクトリをクリックします。

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-select-aad.png)

2. クリックして、 **アプリケーション** 上部にあるタブをクリック **追加** アプリです。 

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-aad-applications-tab.png)

3. クリックして **[組織が開発中のアプリケーションを追加**します。

4. アプリケーションの追加ウィザードで入力、 **名** ] をクリックしてアプリケーションを  **ネイティブ クライアント アプリケーション** 型です。 その後、クリックして続行します。

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-native-selection.png)

5.  **リダイレクト URI** ボックスで、「ログイン/モバイル サービス用のエンドポイントを実行します。 この値は、https://todolist.azure-mobile.net/login/done と同様です。

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-native-redirect-uri.png)

6. クリックして、 **構成** ネイティブ クライアント アプリケーションとコピーのタブ、 **クライアント ID**します。 この情報は後で必要になります。

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-native-client-id.png)

7. ページを下へスクロール、 **他のアプリケーションに対するアクセス許可** セクションを **アプリケーションの追加** ] ボタンをクリックします。 選択 **他の** todo を検索して表示] メニューからです。 クリックして **TodoList** に以前に登録したモバイル サービスを追加し、については、チェック マークをクリックします。 モバイル サービス アプリケーションへのアクセスを許可します。 クリックして **保存**

   ![](./media/mobile-services-dotnet-adal-register-client/mobile-services-native-add-permissions.png)

これで、モバイル サービスが、アプリケーションからシングル サインオン ログインを受信するように AAD で構成されます。

