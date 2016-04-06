<properties
    pageTitle="App Services アプリケーションに Facebook 認証を構成する方法"
    description="App Services アプリケーションに Facebook 認証を構成する方法について説明します。"
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

# App Service アプリケーションを Facebook ログインを使用するように構成する方法

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]
&nbsp;

[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

このトピックでは、認証プロバイダーとして Facebook を使用するように Azure App Service を構成する方法を示します。

このトピックの手順を完了するには、検証済みの電子メール アドレスを持つ Facebook アカウントおよび携帯電話番号が必要になります。 新しい Facebook アカウントを作成するには [facebook.com]します。

> [AZURE.NOTE]
このトピックでは、App Service 認証/承認の使用例を示します。 ほとんどのアプリケーションで、これが App Service ゲートウェイに取って代わっています。 ゲートウェイを使用する場合との相違点は、トピック全体を通してメモに記載しています。


## <a name="register"> </a>Facebook にアプリケーションを登録する

1. ログオン、 [Azure portal], 、アプリケーションに移動します。 コピー、 **URL**します。 この URL は、Facebook アプリの構成で使用します。
 
2. 別のブラウザー ウィンドウに移動し、 [Facebook Developers] web サイトと、Facebook にログイン アカウントの資格情報。

3. (省略可能)既に登録していない場合、クリックして **アプリ** > **開発者として登録する**, 、ポリシーに同意し、登録手順に従います。

4. クリックして **My Apps** > **新しいアプリの追加** > **web サイト** 、アプリの一意の名前を入力し、クリックして **Create New Facebook App ID**します。

6. ドロップダウン リストで、アプリケーションのカテゴリを選択し、クリックして **Create App ID** 次のページでをクリックして **クイック スタートをスキップ**します。 これで、アプリケーションの開発者向けダッシュボードに移動します。

8.  **App Secret** フィールドで、をクリックして **表示**, 、要求されると、次の値をメモしておいてください場合のパスワードを入力 **アプリ ID** と **App Secret**します。 後で、これらを使用するようにアプリケーションを構成します。

    > [AZURE.NOTE] **セキュリティに関する注意**
    アプリケーション シークレットは、重要なセキュリティ資格情報です。 このシークレットを他のユーザーと共有したり、クライアント アプリケーション内で配信したりしないでください。

9. 左側のナビゲーション バーでをクリックして **設定**, 、型、 **URL** でモバイル アプリの **App Domains**, を入力し、 **Contact Email**します。 

    ![][0]

10. 以下の web サイト セクションが見つからない場合にをクリックして **プラットフォームを追加** > **web サイト**, 、入力、 **URL** でモバイル アプリの **サイトの URL** フィールドで、をクリックして **変更の保存**します。

11. をクリックして、 **詳細** ] タブで、アプリケーションの追加 **リダイレクト URI** に **Valid OAuth redirect URIs**, 、] をクリックし、 **変更の保存**します。 URI は、末尾のパス、アプリケーションの URL リダイレクト _/.auth/login/facebook/callback_します。 たとえば、「`https://contoso.azurewebsites.net/.auth/login/facebook/callback`」のように入力します。 HTTPS スキームを使用していることを確認します。


    > [AZURE.NOTE]
    アプリ サービス認証ではなく、App Service ゲートウェイを使用しているかどうかは認証機能は、代わりには使用するとゲートウェイの URL のリダイレクト URL、 _/signin-facebook_ パス。


12. アプリケーションの登録に使用した Facebook アカウントがアプリケーションの管理者になります。 この時点では、管理者のみがこのアプリケーションにサインインできます。 その他の Facebook アカウントを認証する] をクリックして **Status & Review** 左側のナビゲーション バーにします。 をクリックし、 **はい** 汎用パブリック アクセスを有効にします。


## <a name="secrets"> </a>Facebook の情報をアプリケーションに追加する

> [AZURE.NOTE]
App Service ゲートウェイを使用する場合は、このセクションの内容は無視し、ポータル内で目的のゲートウェイに移動します。 選択 **設定**, 、**Identity**, 、し **Facebook**します。 先に取得した値に貼り付け、クリックして **保存**します。


13. 戻り、 [Azure portal], 、アプリケーションに移動します。 をクリックして **設定** > **認証/承認**, 、ことを確認し、 **アプリ サービスの認証** は **に**します。

15. クリックして **Facebook**, 、貼り付け以前に入手したアプリ ID と App Secret の値で、必要に応じて、アプリケーションで必要なすべてのスコープを有効にするクリックして **OK**します。

    ![][1]
    
    App Service は既定では認証を行いますが、サイトのコンテンツと API へのアクセス承認については制限を設けていません。 アプリケーション コードでユーザーを承認する必要があります。 

17. (省略可能)Facebook によって認証されたユーザーのみに、サイトへのアクセスを制限するには設定 **要求が認証されていない場合に実行するアクション** に **Facebook**します。 この場合、要求はすべて認証される必要があり、認証されていない要求はすべて認証のために Facebook にリダイレクトされます。

17. クリックして **保存**します。 

これで、アプリケーションで認証に Facebook を使用する準備ができました。

## <a name="related-content"> </a>関連コンテンツ

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/app-service-facebook-dashboard.png
[1]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook Developers]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure portal]: https://portal.azure.com/

