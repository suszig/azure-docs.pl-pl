<properties
    pageTitle="App Services アプリケーションに Microsoft アカウント認証を構成する方法"
    description="App Services アプリケーションに Microsoft アカウント認証を構成する方法について説明します。"
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

# Microsoft アカウント ログインを使用するように App Service アプリケーションを構成する方法

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]
&nbsp;

[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

このトピックでは、認証プロバイダーとして Microsoft アカウントを使用するように Azure App Services を構成する方法を示します。


> [AZURE.NOTE]
このトピックでは、App Service 認証/承認の使用例を示します。 ほとんどのアプリケーションで、これが App Service ゲートウェイに取って代わっています。 ゲートウェイを使用する場合との相違点は、トピック全体を通してメモに記載しています。


## <a name="register"> </a>Microsoft アカウントにアプリケーションを登録する

1. ログオン、 [Azure portal], 、アプリケーションに移動します。 コピー、 **URL**します。 この URL は、Microsoft アカウント アプリを構成するのに使用します。

2. 移動し、 [My Applications] Microsoft アカウント デベロッパー センター ページし、必要な場合は、Microsoft アカウントでログオンします。

4. をクリックして **アプリケーションを作成する**, 、入力、 **アプリケーション名** ] をクリック **同意**します。

5. クリックして **API 設定**します。 選択 **はい** の **モバイル クライアント アプリ/デスクトップ クライアント アプリ**します。  **リダイレクト URL** フィールドに「アプリケーションの **リダイレクト URL** ] をクリック **保存**します。 URI は、末尾のパス、アプリケーションの URL リダイレクト _/.auth/login/microsoftaccount/callback_します。 たとえば、「`https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`」のように入力します。 HTTPS スキームを使用していることを確認します。

    ![][0]


    > [AZURE.NOTE]
    アプリ サービス認証ではなく、App Service ゲートウェイを使用しているかどうかは認証機能は、代わりには使用するとゲートウェイの URL のリダイレクト URL、 _/signin-microsoft が末尾_ パス。


6. クリックして **アプリ設定** の値をメモしておきます、 **クライアント ID** と **クライアント シークレット**します。


    > [AZURE.NOTE] クライアント シークレットは、重要なセキュリティ資格情報です。 クライアント シークレットを他のユーザーと共有したり、クライアント アプリケーション内で配信したりしないでください。
    

## <a name="secrets"> </a>Microsoft アカウントの情報をアプリケーションに追加する

> [AZURE.NOTE]
App Service ゲートウェイを使用する場合は、このセクションの内容は無視し、ポータル内で目的のゲートウェイに移動します。 選択 **設定**, 、**Identity**, 、し **Microsoft アカウント**します。 先に取得した値に貼り付け、クリックして **保存**します。


7. 戻り、 [Azure portal], 、アプリケーションに移動します。 クリックして **設定**, 、し **認証/承認**します。

8. 場合は、認証/承認の機能が有効になっていない、スイッチを有効に **に**します。

9. クリックして **Microsoft アカウント**します。 前の手順で取得した App ID と App Secret の値を貼り付けます。アプリケーションで必要なスコープを有効にします (省略可能)。 クリックして **OK**します。

    ![][1]
    
    App Service は既定では認証を行いますが、サイトのコンテンツと API へのアクセス承認については制限を設けていません。 アプリケーション コードでユーザーを承認する必要があります。 

17. (省略可能)Microsoft アカウントによって認証されたユーザーのみに、サイトへのアクセスを制限するには設定 **要求が認証されていない場合に実行するアクション** に **Microsoft アカウント**します。 この場合、要求はすべて認証される必要があり、認証されていない要求はすべて認証のために Microsoft アカウントにリダイレクトされます。

11. クリックして **保存**します。 


これで、アプリケーションで認証に Microsoft アカウントを使用する準備ができました。

## <a name="related-content"> </a>関連コンテンツ

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Authenticate your app with Live Connect Single Sign-On: [Windows](windows-liveconnect) -->



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure portal]: https://portal.azure.com/


