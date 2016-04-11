<properties
    pageTitle="App Services アプリケーションに Google 認証を構成する方法"
    description="App Services アプリケーションに Google 認証を構成する方法について説明します。"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson" 
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

# Google ログインを使用するように App Service アプリケーションを構成する方法

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]
&nbsp;

[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

このトピックでは、認証プロバイダーとして Google を使用するように Azure App Services を構成する方法を示します。

このトピックの手順を完了するには、検証済みの電子メール アドレスを持つ Google アカウントが必要になります。 新しい Google アカウントを作成するには、[accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302) にアクセスしてください。

> [AZURE.NOTE]
このトピックでは、App Service 認証/承認の使用例を示します。 ほとんどのアプリケーションで、これが App Service ゲートウェイに取って代わっています。 ゲートウェイを使用する場合との相違点は、トピック全体を通してメモに記載しています。


## <a name="register"> </a>Google にアプリケーションを登録する

1. ログオン、 [Azure portal], 、アプリケーションに移動します。 コピー、 **URL**します。 この URI は、Google アプリの構成に使用します。
 
2. 移動、 [Google api](http://go.microsoft.com/fwlink/p/?LinkId=268303) 、Google アカウント資格情報でサインインの web サイト] をクリックして **プロジェクトの作成**, 、提供、 **プロジェクト名**, 、] をクリックし、 **作成**します。

3. 左側のナビゲーション バーで、クリックして **API と認証**, 、[ **ソーシャル Api** クリックして **Google + API** > **を有効にする API**します。

4. をクリックして **API と認証** > **資格情報** > **OAuth 同意画面**, 選択してから、 **電子メール アドレス**,  、入力、 **製品名**, 、] をクリック **保存**します。

5.  **資格情報** ] タブ、[ **資格情報を追加** > **OAuth 2.0 クライアント ID**, 選択してから、 **Web アプリケーション**します。

6. App Service を貼り付ける **URL** を先ほどコピーした **Authorized JavaScript Origins**, 、貼り付けて、 **リダイレクト URI** を先ほどコピーした **Authorized Redirect URI**します。 リダイレクト URI は、末尾のパス、アプリケーションの URL _/.auth/login/google/callback_します。 たとえば、「`https://contoso.azurewebsites.net/.auth/login/google/callback`」のように入力します。 HTTPS スキームを使用していることを確認します。 クリックし、 **作成**します。


    > [AZURE.NOTE]
    アプリ サービス認証ではなく、App Service ゲートウェイを使用しているかどうかは認証機能は、代わりには使用するとゲートウェイの URL のリダイレクト URL、 _/signin-google_ パス。


7. 次の画面で、表示されているクライアント ID とクライアント シークレットの値を書き留めます。


    > [AZURE.IMPORTANT]
    クライアント シークレットは、重要なセキュリティ資格情報です。 このシークレットを他のユーザーと共有したり、クライアント アプリケーション内で配信したりしないでください。


## <a name="secrets"> </a>Google の情報をアプリケーションに追加する

> [AZURE.NOTE]
App Service ゲートウェイを使用する場合は、このセクションの内容は無視し、ポータル内で目的のゲートウェイに移動します。 選択 **設定**, 、**Identity**, 、し **Google**します。 先に取得した値に貼り付け、クリックして **保存**します。


8. 戻り、 [Azure portal], 、アプリケーションに移動します。 クリックして **設定**, 、し **認証/承認**します。

9. 場合は、認証/承認の機能が有効になっていない、スイッチを有効に **に**します。

10. クリックして **Google**します。 前の手順で取得した App ID と App Secret の値を貼り付けます。アプリケーションで必要なスコープを有効にします (省略可能)。 クリックして **OK**します。

    ![][1]

    App Service は既定では認証を行いますが、サイトのコンテンツと API へのアクセス承認については制限を設けていません。 アプリケーション コードでユーザーを承認する必要があります。 

17. (省略可能)Google によって認証されたユーザーのみに、サイトへのアクセスを制限するには設定 **要求が認証されていない場合に実行するアクション** に **Google**します。 この場合、要求はすべて認証される必要があり、認証されていない要求はすべて認証のために Google にリダイレクトされます。

12. クリックして **保存**します。 

これで、アプリケーションで認証に Google を使用する準備ができました。

## <a name="related-content"> </a>関連コンテンツ

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure portal]: https://portal.azure.com/
 


