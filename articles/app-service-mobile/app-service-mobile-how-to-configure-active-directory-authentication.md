<properties 
    pageTitle="App Services アプリケーションに Azure Active Directory 認証を構成する方法" 
    description="App Services アプリケーションに Azure Active Directory 認証を構成する方法について説明します。" 
    authors="mattchenderson" 
    services="app-service\mobile" 
    documentationCenter="" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-mobile" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="11/20/2015" 
    ms.author="mahender"/>

# Azure Active Directory ログインを使用するように App Service アプリケーションを構成する方法

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]
&nbsp;

[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

このトピックでは、認証プロバイダーとして Azure Active Directory を使用するように Azure App Services を構成する方法を示します。 

> [AZURE.NOTE] このトピックの説明、アプリ サービスの認証の使用/認証機能します。 ほとんどのアプリケーションで、これが App Service ゲートウェイに取って代わっています。 ゲートウェイを使用する場合を参照してください、 [alternative method]します。 ゲートウェイを使用する場合との相違点については、そのセクションの注に記載されています。


## <a name="express"> </a>簡単設定を使用して Azure Active Directory を構成します。

13.  [Azure portal], 、アプリケーションに移動します。 クリックして **設定**, 、し **認証/承認**します。

14. 場合は、認証/承認の機能が有効になっていない、スイッチを有効に **に**します。

15. クリックして **Azure Active Directory**, 、順にクリック **Express** [ **管理モード**します。

16. クリックして **OK** Azure Active Directory でアプリケーションを登録します。 これで新しい登録が作成されます。 代わりに既存の登録を選択する場合は、クリックして **既存のアプリを選択して** し、テナント内で以前に作成した登録の名前を検索します。 それを選択して [登録] をクリックして **OK**します。 をクリックし、 **OK** Azure Active Directory 設定] ブレードでします。

    ![][0]
    
    App Service は既定では認証を行いますが、サイトのコンテンツと API へのアクセス承認については制限を設けていません。 アプリケーション コードでユーザーを承認する必要があります。 

17. (省略可能)Azure Active Directory によって認証されたユーザーのみに、サイトへのアクセスを制限するには設定 **要求が認証されていない場合に実行するアクション** に **Azure Active Directory**します。 この場合、要求はすべて認証される必要があり、認証されていない要求はすべて認証のために Azure Active Directory にリダイレクトされます。

17. クリックして **保存**します。 

これで、アプリケーションで認証に Azure Active Directory を使用する準備ができました。

## <a name="advanced"> </a>(代替方法)高度な設定を手動で Azure Active Directory を構成します。
構成設定を手動で行うこともできます。 これは、使用する AAD テナントが Azure へのサインインに使用するテナントと異なる場合に推奨されるソリューションです。 構成を完了するには、まず Azure Active Directory で登録を作成し、登録の一部の詳細を App Service に提供する必要があります。

### <a name="register"> </a>Azure Active Directory にアプリケーションを登録する

1. ログオン、 [Azure portal], 、アプリケーションに移動します。 コピー、 **URL**します。 これを使用して、Azure Active Directory アプリケーションを構成します。

3. サインイン、 [Azure classic portal] に移動して **Active Directory**します。

    ![][2] 

4. ディレクトリを選択してから、 **アプリケーション** 上部にあるタブをクリックします。 クリックして **追加** 下部には、新しいアプリケーションの登録を作成します。 

5. クリックして **[組織が開発中のアプリケーションを追加**します。

6. アプリケーションの追加ウィザードで入力、 **名** ] をクリックしてアプリケーションを  **Web アプリケーションや Web API** 型です。 その後、クリックして続行します。

7.  **サインオン URL** ボックスで、先ほどコピーしたアプリケーションの URL を貼り付けます。 その同じ URL を入力、 **App ID URI** ボックス。 その後、クリックして続行します。

8. アプリケーションが追加されると、クリックして、 **構成** ] タブをクリックします。 編集、 **応答 URL** [ **でのシングル サインオン** を末尾のパス、アプリケーションの URL _/.auth/login/aad/callback_します。 たとえば、「`https://contoso.azurewebsites.net/.auth/login/aad/callback`」のように入力します。 HTTPS スキームを使用していることを確認します。

    ![][3]
    
    
    > [AZURE.NOTE]
    If you are using the App Service Gateway instead of the App Service Authentication / Authorization feature, your Reply URL instead uses the gateway URL with the _/signin-aad_ path.


9. クリックして **保存**します。 コピーして、 **クライアント ID** アプリです。 後で、これを使用するようにアプリケーションを構成します。

10. 下部にあるコマンド バーで、クリックして **エンドポイントの表示**, 、し、コピー、 **フェデレーション メタデータ ドキュメント** URL とダウンロードを文書化、またはブラウザーでを選択します。

11. ルート内 **EntityDescriptor** 要素、必要があります、 **entityID** フォームの属性 `https://sts.windows.net/` ("テナント ID"と呼ばれます)、テナントに書き込み、続けて、GUID を特定します。 この値をコピー - にはなる、 **発行者 URL**します。 後で、これを使用するようにアプリケーションを構成します。

### <a name="secrets"> </a>Azure Active Directory の情報をアプリケーションに追加します。

> [AZURE.NOTE]
App Service ゲートウェイを使用する場合は、このセクションの内容は無視し、ポータル内で目的のゲートウェイに移動します。 選択 **設定**, 、**Identity**, 、し **Azure Active Directory**します。 ClientID に貼り付け、テナント ID を追加、 **許可されたテナント** ] ボックスの一覧です。 クリックして **保存**します。


13. 戻り、 [Azure portal], 、アプリケーションに移動します。 クリックして **設定**, 、し **認証/承認**します。

14. 認証/承認の機能が有効になっていない場合は、スイッチに、 **に**します。

15. をクリックして **Azure Active Directory**, 、] をクリックし、 **詳細** [ **管理モード**します。 以前に取得したクライアント ID と発行者の URL の値を貼り付けます。 クリックして **OK**します。

    ![][1]
    
    App Service は既定では認証を行いますが、サイトのコンテンツと API へのアクセス承認については制限を設けていません。 アプリケーション コードでユーザーを承認する必要があります。 

17. (省略可能)Azure Active Directory によって認証されたユーザーのみに、サイトへのアクセスを制限するには設定 **要求が認証されていない場合に実行するアクション** に **Azure Active Directory**します。 この場合、要求はすべて認証される必要があり、認証されていない要求はすべて認証のために Azure Active Directory にリダイレクトされます。

17. クリックして **保存**します。 

これで、アプリケーションで認証に Azure Active Directory を使用する準備ができました。 

## (省略可能) ネイティブ クライアント アプリケーションの構成

Azure Active Directory では、ネイティブ クライアントを登録して、アクセス許可のマッピングをさらに詳細に制御することもできます。 などのライブラリを使用してログインを実行する場合に必要、 **Active Directory 認証ライブラリ**します。

1. 移動 **Active Directory** で、 [Azure classic portal]します。

2. ディレクトリを選択してから、 **アプリケーション** 上部にあるタブをクリックします。 クリックして **追加** 下部には、新しいアプリケーションの登録を作成します。 

3. クリックして **[組織が開発中のアプリケーションを追加**します。

4. アプリケーションの追加ウィザードで入力、 **名** ] をクリックしてアプリケーションを  **ネイティブ クライアント アプリケーション** 型です。 その後、クリックして続行します。

5.  **リダイレクト URI** ボックスで、サイトの「 _/.auth/login/done_ HTTPS スキームを使用して、エンドポイント。 この値はのような _https://contoso.azurewebsites.net/.auth/login/done_します。

6. ネイティブ アプリケーションが追加されると、クリックして、 **構成** ] タブをクリックします。 検索、 **クライアント ID** この値をメモしておきます。

7. ページを下へスクロール、 **他のアプリケーションに対するアクセス許可** セクションを **アプリケーションの追加**します。

8. 前に登録した Web アプリケーションを検索し、プラス アイコンをクリックします。 次に、チェックをクリックしてダイアログ ボックスを閉じます。

9. 新しいエントリを追加した上で開きます、 **委任されたアクセス許可** ドロップダウンを選択して **アクセス (appName)**します。 クリックして **保存**します。

以上の手順で、App Service アプリケーションにアクセスできるネイティブ クライアント アプリケーションが構成されます。

## <a name="related-content"> </a>関連コンテンツ

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Azure classic portal]: https://manage.windowsazure.com/
[ios-adal]: ../app-service-mobile-xamarin-ios-aad-sso.md
[alternative method]:#advanced
