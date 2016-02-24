<properties
    pageTitle="App Services アプリケーションに Twitter 認証を構成する方法"
    description="App Services アプリケーションに Twitter 認証を構成する方法について説明します。"
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
    ms.date="10/29/2015"
    ms.author="mahender"/>

# Twitter ログインを使用するように App Service アプリケーションを構成する方法

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]
&nbsp;

[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

このトピックでは、認証プロバイダーとして Twitter を使用するように Azure App Services を構成する方法を示します。

このトピックの手順を完了するには、検証済みの電子メール アドレスを持つ Twitter アカウントが必要になります。 新しい Twitter アカウントを作成するには、 <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.


> [AZURE.NOTE]
このトピックでは、App Service 認証/承認の使用例を示します。 ほとんどのアプリケーションで、これが App Service ゲートウェイに取って代わっています。 ゲートウェイを使用する場合との相違点は、トピック全体を通してメモに記載しています。


## <a name="register"> </a>Twitter にアプリケーションを登録する


1. ログオン、 [Azure portal], 、アプリケーションに移動します。 コピー、 **URL**します。 この URL は、Twitter アプリの構成で使用します。

2. 移動し、 [Twitter Developers] web サイト、Twitter アカウント資格情報でサインインし、をクリックして **Create New App**します。

3. 入力、 **名前** と **説明** 新しいアプリにします。 アプリケーションの貼り付け **URL** の **web サイト** 値。 次に、 **コールバック URL**, 、貼り付け、 **コールバック URL** 先ほどコピーしました。 これは、パスが追加されたモバイル アプリ ゲートウェイ _/.auth/login/twitter/callback_します。 たとえば、「`https://contoso.azurewebsites.net/.auth/login/twitter/callback`」のように入力します。 HTTPS スキームを使用していることを確認します。

    > [AZURE.NOTE]
    アプリ サービス認証ではなく、App Service ゲートウェイを使用しているかどうかは認証機能は、代わりには使用するとゲートウェイの URL のリダイレクト URL、 _/signin-twitter が末尾_ パス。

3.  ページの下部で、条項を読み、同意します。 クリックして **Twitter アプリケーションを作成する**です。 これでアプリケーションが登録され、アプリケーションの詳細が表示されます。

4. をクリックして、 **設定** ] タブで、チェック **sign in with Twitter を使用するには、このアプリケーションを許可する**, 、] をクリックし、 **の設定の更新**します。

5. 選択、 **Keys and Access Tokens** ] タブをクリックします。 値をメモしておいてください **Consumer Key (API Key)** と **Consumer secret (API Secret)**します。

    > [AZURE.NOTE] コンシューマー シークレットは、重要なセキュリティ資格情報です。 このシークレットは、他のユーザーと共有したり、アプリケーションと共に配布したりしないでください。


## <a name="secrets"> </a>アプリケーションに Twitter 情報を追加します。

> [AZURE.NOTE]
App Service ゲートウェイを使用する場合は、このセクションの内容は無視し、ポータル内で目的のゲートウェイに移動します。 選択 **設定**, 、**Identity**, 、し **Twitter**します。 先に取得した値に貼り付け、クリックして **保存**します。


13. 戻り、 [Azure portal], 、アプリケーションに移動します。 クリックして **設定**, 、し **認証/承認**します。

14. 場合は、認証/承認の機能が有効になっていない、スイッチを有効に **に**します。

15. クリックして **Twitter**します。 以前に入手したアプリ ID とアプリ シークレットの値を貼り付けます。 クリックして **OK**します。

    ![][1]

    App Service は既定では認証を行いますが、サイトのコンテンツと API へのアクセス承認については制限を設けていません。 アプリケーション コードでユーザーを承認する必要があります。 

17. (省略可能)Twitter によって認証されたユーザーのみに、サイトへのアクセスを制限するには設定 **要求が認証されていない場合に実行するアクション** に **Twitter**します。 この場合、要求はすべて認証される必要があり、認証されていない要求はすべて認証のために Twitter にリダイレクトされます。

17. クリックして **保存**します。 

これで、アプリケーションで認証に Twitter を使用する準備ができました。

## <a name="related-content"> </a>関連コンテンツ

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter Developers]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
