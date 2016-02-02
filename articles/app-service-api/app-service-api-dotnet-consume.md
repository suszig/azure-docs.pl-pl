<properties 
    pageTitle="Azure App Service で .NET クライアントから API アプリ使用する" 
    description="App Service SDK を使用して .NET クライアントから API アプリを使用する方法について説明します。" 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/22/2015" 
    ms.author="tdykstra"/>


# Azure App Service で .NET クライアントから API アプリ使用する

[AZURE.INCLUDE [app-service-api-v2-note](../../includes/app-service-api-v2-note.md)]

## 概要

このチュートリアルでは SDK を使用して、アプリケーション サービスを呼び出すコードを記述する方法、 [API アプリ](app-service-api-apps-why-best-platform.md) に対して構成されている **パブリック (匿名)** または **パブリック (認証済み)** アクセス レベル。 この記事では、次のシナリオを取り上げます。

- コンソール アプリケーションから**パブリック (匿名)** API アプリを呼び出す
- Windows デスクトップ アプリケーションから**パブリック (認証済み)** API アプリを呼び出す

このチュートリアルのセクションはそれぞれ独立した内容であるため、2 つ目のシナリオの手順を実行する際に、最初のシナリオの手順を完了しておく必要はありません。

呼び出す方法については、 **内部** API アプリを参照してください [.NET クライアントから内部 API アプリの使用](app-service-api-dotnet-consume-internal.md)します。

## 前提条件

このチュートリアルでは、プロジェクトを作成し、Visual Studio で、それらにコードを追加する方法について理解しているとハウツー [Azure プレビュー ポータルで API アプリを管理](app-service-api-apps-manage-in-portal.md)します。

この記事のプロジェクトとコードのサンプルは、次の記事で作成、デプロイ、保護した API アプリ プロジェクトに基づいています。

- [API アプリを作成します。](app-service-dotnet-create-api-app.md)
- [API アプリをデプロイします。](app-service-dotnet-deploy-api-app.md)
- [API アプリを保護します。](../app-service-api-dotnet-add-authentication.md)

[AZURE.INCLUDE [install-sdk-2013-only](../../includes/install-sdk-2013-only.md)]

このチュートリアルでは、Azure SDK for .NET 2.6 以降のバージョンが必要です。

## コンソール アプリケーションからの認証されていない呼び出し

このセクションでは、コンソール アプリケーション プロジェクトを作成し、そのプロジェクトに、認証を要求しない API アプリを呼び出すコードを追加します。

### API アプリの設定とプロジェクトの作成

1. 実行していない場合はに従って、 [API アプリのデプロイ](app-service-dotnet-deploy-api-app.md) 、Azure サブスクリプションに API アプリに ContactsList サンプル プロジェクトを展開します。

    そのチュートリアルでは、Visual Studio の[発行] ダイアログ ボックスでアクセス レベルを **[すべてのユーザーが利用できる]** に設定します。これは、ポータルにおける **[パブリック (匿名)]** と同じです。 ただし、した場合、 [API アプリの保護](../app-service-api-dotnet-add-authentication.md) チュートリアルではその後、アクセス レベルに設定されて **パブリック (認証済み)**, 、その場合は、次の手順で指示されたとおりに変更する必要があります。

2. [Azure プレビュー ポータル](https://portal.azure.com/), で、 **API アプリ** を呼び出すに移動する API アプリのブレード **設定 > アプリケーション設定** 設定と **アクセス レベル** に **パブリック (匿名)**します。

    ![](./media/app-service-api-dotnet-consume/setpublicanon.png)

2. Visual Studio で、コンソール アプリケーション プロジェクトを作成します。

### <a id="addclient"></a>App Service SDK で生成されたクライアント コードを追加します。

[AZURE.INCLUDE [app-service-api-dotnet-add-generated-client](../../includes/app-service-api-dotnet-add-generated-client.md)]

### API アプリを呼び出すコードの追加

API アプリを呼び出すには、次の例のように、クライアント オブジェクトを作成し、そのオブジェクトに対してメソッドを呼び出す必要があります。

        var client = new ContactList();
        var contacts = client.Contacts.Get();

1. 開いている *Program.cs*, 、次のコードを追加し、 `Main` メソッドです。

     var client = new ContactsList();
    
     // Send GET request.
     var contacts = client.Contacts.Get();
     foreach (var c in contacts)
     {
         Console.WriteLine("{0}: {1} {2}",
             c.Id, c.Name, c.EmailAddress);
     }
    
     // Send POST request.
     client.Contacts.Post(new Models.Contact
     {
         EmailAddress = "lkahn@contoso.com",
         Name = "Loretta Kahn",
         Id = 4
     });
     Console.WriteLine("Finished");
     Console.ReadLine();

3. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

    ![生成完了画面](./media/app-service-api-dotnet-consume/consoleappoutput.png)

## Windows デスクトップ アプリケーションからの認証された呼び出し

このセクションでは、Windows デスクトップ アプリケーション プロジェクトを作成し、そのプロジェクトに、認証を要求する API アプリを呼び出すコードを追加します。

### API アプリの設定とプロジェクトの作成

1. 次の [API アプリの保護](../app-service-api-dotnet-add-authentication.md) する API アプリをセットアップするチュートリアル **パブリック (認証済み)** レベルにアクセスします。

1. Visual Studio で、Windows フォーム デスクトップ プロジェクトを作成します。

2. フォーム デザイナーで、次のコントロールを追加します。

    * ボタン コントロール
    * テキスト ボックス コントロール
    * Web ブラウザー コントロール

3. テキスト ボックス コントロールを複数行に設定します。

    フォームは次の例のようになります。

    ![](./media/app-service-api-dotnet-consume/form.png)

### App Service SDK で生成されたクライアント コードの追加

3. **ソリューション エクスプローラー**で、プロジェクト (ソリューションではありません) を右クリックします。**[追加]、[Azure API アプリ クライアント]** の順にクリックします。

3. **[Azure API アプリ クライアントの追加]** ダイアログ ボックスで、**[Azure API アプリからダウンロード]** をクリックします。

5. ドロップダウン リストから、呼び出す API アプリを選択し、**[OK]** をクリックします。

### API アプリを呼び出すコードの追加

5. Azure プレビュー ポータルで、API アプリのゲートウェイの URL をコピーします。 次の手順でこの値を使用します。

    ![](./media/app-service-api-dotnet-consume/gatewayurl.png)

4. *Form1.cs* ソース コードは、前に、次のコードを追加、 `トラクター` コンス トラクター、GATEWAY_URL の値を前の手順でコピーした値に置換します。 末尾のスラッシュ (/) を含めていることを確認します。

        private const string GATEWAY_URL = "https://resourcegroupnameb4f3d966dfa43b6607f30.azurewebsites.net/";
        private const string URL_TOKEN = "#token=";

4. フォーム デザイナーで、ボタンをダブルクリックしてクリック ハンドラーを追加し、ハンドラー メソッドで、ゲートウェイのログイン URL に移動するコードを追加します。次に例を示します。

        webBrowser1.Navigate(string.Format(@"{0}login/[authprovider]", GATEWAY_URL));

    "[authprovider]" を、ゲートウェイで構成した ID サービス プロバイダーのコードに置き換えます。たとえば、"aad"、"twitter"、"google"、"microsoftaccount"、"facebook" です。 次に例を示します。

        webBrowser1.Navigate(string.Format(@"{0}login/aad", GATEWAY_URL));

7. 追加、 `DocumentCompleted` の web ブラウザーのイベント ハンドラーを制御して、ハンドラー メソッドに次のコードを追加します。

     if (e.Url.AbsoluteUri.IndexOf(URL_TOKEN) > -1)
     {
         var encodedJson = e.Url.AbsoluteUri.Substring(e.Url.AbsoluteUri.IndexOf(URL_TOKEN) + URL_TOKEN.Length);
         var decodedJson = Uri.UnescapeDataString(encodedJson);
         var result = JsonConvert.DeserializeObject<dynamic>(decodedJson);
         string userId = result.user.userId;
         string userToken = result.authenticationToken;
    
         var appServiceClient = new AppServiceClient(GATEWAY_URL);
         appServiceClient.SetCurrentUser(userId, userToken);
    
         var contactsListClient = appServiceClient.CreateContactsList();
         var contacts = contactsListClient.Contacts.Get();
         foreach (Contact contact in contacts)
         {
             textBox1.Text += contact.Name + " " + contact.EmailAddress + System.Environment.NewLine;
         }
         //appServiceClient.Logout();
         //webBrowser1.Navigate(string.Format(@"{0}login/aad", GW_URL));
     }

 追加したコードは、ユーザーが Web ブラウザー コントロールを使用してログインした後に実行されます。 ログインに成功すると、応答 URL にはユーザー ID とパスワードが含まれます。 このコードは、URL から抽出したユーザー ID とパスワードを App Service クライアント オブジェクトに提供した後、そのオブジェクトを使用して API アプリ クライアント オブジェクトを作成します。 この API アプリ クライアント オブジェクトでメソッドを呼び出して、API を呼び出すことができます。

8. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

9. ボタンをクリックし、ブラウザー コントロールにログイン ページが表示されたら、API アプリの呼び出しが許可されたユーザーの資格情報を入力します。

    Azure で認証されると、アプリケーションによって API アプリが呼び出され、応答が表示されます。

    ![](./media/app-service-api-dotnet-consume/formaftercall.png)

### <a id="client-flow"></a>サーバー フローとクライアント フローの比較

サンプル アプリケーションを示しています [サーバー フロー](../app-service/app-service-authentication-overview.md#server-flow), 、これは、ゲートウェイが id プロバイダーのアクセス トークンを取得します。  [クライアント フロー](../app-service/app-service-authentication-overview.md#client-flow), 、クライアント アプリケーションが id プロバイダーから直接アクセス トークンを取得、ゲートウェイに送信を呼び出します `LoginAsync` なく `SetCurrentUser`します。

次のコード例では、という名前の文字列変数に id プロバイダーのアクセス トークンであると想定 `providerAccessToken` と、id プロバイダーのインジケーター ("aad"、"microsoftaccount"、"google"、"twitter"、または"facebook") という名前の文字列変数に `idProvider`:

        var appServiceClient = new AppServiceClient(GATEWAY_URL);
        var providerAccessTokenJSON = new JObject();
        providerAccessTokenJSON["access_token"] = providerAccessToken;
        var appServiceUser = await appServiceClient.LoginAsync(idProvider, providerAccessTokenJSON);
    
        var contactsListClient = appServiceClient.CreateContactsList();
        var contacts = contactsListClient.Contacts.Get();
        foreach (Contact contact in contacts)
        {
            textBox1.Text += contact.Name + " " + contact.EmailAddress + System.Environment.NewLine;
        }

## 次のステップ

この記事では、アクセス レベルが**パブリック (匿名)** または**パブリック (認証済み)** に設定された API アプリについて、.NET クライアントから API アプリを使用する方法について説明しました。

その他の例を .NET クライアントから API アプリを呼び出すコードでは、ダウンロード、 [Azure Cards](https://github.com/Azure-Samples/API-Apps-DotNet-AzureCards-Sample) サンプル アプリケーションです。

API アプリで認証を使用する方法については、次を参照してください。 [API apps と Azure App Service でのモバイル アプリ認証](../app-service/app-service-authentication-overview.md)します。






