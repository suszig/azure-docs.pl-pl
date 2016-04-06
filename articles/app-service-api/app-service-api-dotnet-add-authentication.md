<properties 
    pageTitle="Azure API アプリの保護" 
    description="Visual Studio を使用して Azure API アプリを保護する方法について説明します。" 
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
    ms.date="10/27/2015" 
    ms.author="tdykstra"/>

# API アプリの保護: Azure Active Directory またはソーシャル プロバイダーの認証の追加

[AZURE.INCLUDE [app-service-api-v2-note](../../includes/app-service-api-v2-note.md)]

## 概要

このチュートリアルでは、認証されたユーザーのみがアクセスできるように API アプリを保護する方法を示します。 さらに、ASP.NET API アプリで使用できる、ログオンしているユーザーに関する情報を取得するためのコードも紹介します。

以下の手順を実行します。

- API アプリを呼び出して、動作していることを確認します。
- API アプリに認証ルールを適用します。
- API アプリをもう一度呼び出して、認証されていない要求が拒否されることを確認します。
- 構成したプロバイダーにログインします。
- API アプリをもう一度呼び出して、認証されたアクセスが機能することを確認します。
- ログオン ユーザーの要求を取得するコードを作成してテストします。

Azure App Service での認証についての詳細については、次を参照してください。 [API apps と mobile apps の認証](../app-service/app-service-authentication-overview.md)します。

## 前提条件

このチュートリアルで作成した API アプリでは [API アプリの作成](app-service-dotnet-create-api-app.md) にデプロイ [API アプリのデプロイ](app-service-dotnet-deploy-api-app.md)します。

## ブラウザーを使用した API アプリの呼び出し 

API アプリにパブリックにアクセスできることを確認する最も簡単な方法は、ブラウザーから呼び出すことです。

1. ブラウザーでに移動、 [Azure preview portal]します。

3. ホーム ページからクリックして **参照 > API Apps** し保護する API アプリの名前をクリックします。

    ![参照](./media/app-service-api-dotnet-add-authentication/browse.png)

    ![API アプリの選択](./media/app-service-api-dotnet-add-authentication/select.png)

3.  **API アプリ** ブレードで、をクリックして、 **URL** 、API アプリを呼び出すブラウザー ウィンドウを開きます。

    ![API App blade](./media/app-service-api-dotnet-add-authentication/chooseapiappurl.png)

2. ブラウザーのアドレス バーに表示されている URL に `/api/contacts/get/` を追加します。

    たとえば、次の URL が API アプリの URL だとします。

        https://microsoft-apiappeeb5bdsasd744e188be7fa26f239bd4b.azurewebsites.net/

    最終的な URL は次のようになります。

        https://microsoft-apiappeeb5bdsasd744e188be7fa26f239bd4b.azurewebsites.net/api/contacts/get/

    API 呼び出しの処理方法は、ブラウザーによって異なります。 次の図は、Chrome ブラウザーからの呼び出しが成功したようすを示しています。

    ![Chrome の GET 応答](./media/app-service-api-dotnet-add-authentication/chromeget.png)

2. 使用した URL は、このチュートリアルの後半でもう一度使用するため、保存してください。

## API アプリの保護

API アプリをデプロイしたときに、リソース グループにデプロイしました。 Web アプリと他の API アプリを同じリソース グループに追加することができます。リソース グループ内の各 API アプリには、次の 3 種類のアクセス レベルのいずれかを設定することができます。
<!--todo: diagram showing different accessibility settings-->

- **パブリック (匿名)** -ログインせずに、リソース グループの外部から API アプリを呼び出すすべてのユーザーことができます。
- **パブリック (認証済み)** -認証されたユーザーのみがリソース グループの外部から API アプリを呼び出すことができます。
- **内部** -同じリソース グループ内の他の API アプリのみが API アプリを呼び出すことができます。 (Web アプリからの呼び出しは、Web アプリが同じリソース グループにある場合でも外部と見なさます)。

Visual Studio では、リソース グループを作成するため、これも作成、 *ゲートウェイ*します。  ゲートウェイは、リソース グループ内の API アプリ宛てのすべての要求を処理する、専用の Web アプリです。

リソース グループのブレードに移動すると、 [Azure preview portal], 、図内のゲートウェイと API アプリを確認できます。

![リソース グループの図](./media/app-service-api-dotnet-add-authentication/rgdiagram.png)

### <a id="apiapp"></a>認証を要求するように API アプリを構成する

認証された要求のみを受け入れるように API アプリを構成するのアクセス レベルを設定します **パブリック (認証済み)** し、Azure Active Directory、Google、Facebook などのプロバイダーからの認証を要求するようにゲートウェイを構成します。

[AZURE.INCLUDE [app-service-api-config-auth](../../includes/app-service-api-config-auth.md)]

これで、認証されていないアクセスから API アプリが保護されます。 次に、ゲートウェイを構成して、使用する認証プロバイダーを指定する必要があります。

### <a id="gateway"></a>認証プロバイダーを使用するようにゲートウェイを構成する

[AZURE.INCLUDE [app-service-api-gateway-config-auth](../../includes/app-service-api-gateway-config-auth.md)]

## 認証の動作の確認

**注:** 次の手順を実行する際にログインで問題がある場合は、プライベートまたは匿名でウィンドウを開いてみてください。
 
1. ブラウザーのウィンドウを開き、アドレス バーに、API アプリの `Get` メソッドを呼び出す URL を、先ほどと同様に入力します。

    今回は、API アプリへのアクセスを試みると、エラー メッセージが表示されます。

    ![Chrome の GET 応答に失敗](./media/app-service-api-dotnet-add-authentication/chromegetfail.png)

2. ブラウザーで、次のログイン URL に移動します。 URL のパターンは次のとおりです。 

        http://[gatewayurl]/login/[providername]

    ゲートウェイの URL を取得できます、 **ゲートウェイ** ブレードで、 [Azure preview portal]します。 (に移動する、 **ゲートウェイ** ブレードに表示される図内のゲートウェイをクリックして、 **リソース グループ** ブレードです)。

    ![ゲートウェイ URL](./media/app-service-api-dotnet-add-authentication/gatewayurl.png)

    [providername] には、次の値のいずれかを指定する必要があります。
    
    * "microsoftaccount"
    * "facebook"
    * "twitter"
    * "google"
    * "aad"

    Azure Active Directory のサンプルのログイン URL を次に示します。

        https://dropboxrgaeb4ae60b7cb4f3d966dfa43.azurewebsites.net/login/aad/

    先ほどの URL とは異なりこの 1 つ含まれていません API アプリの名前に注意してください。 ゲートウェイは、ユーザーを認証して、API アプリではありません。  ゲートウェイは、リソース グループ内のすべての API アプリの認証を処理します。

3. ブラウザーにログイン ページが表示されたら資格情報を入力します。 
 
    Azure Active Directory ログインを構成した場合に表示されるユーザーのいずれかを使用、 **ユーザー** タブの [Azure Active Directory] タブで作成したアプリケーションを [Azure portal], 、admin@contoso.onmicrosoft.com などです。

    ![AAD ユーザー](./media/app-service-api-dotnet-add-authentication/aadusers.png)

    ![ログイン ページ](./media/app-service-api-dotnet-add-authentication/ffsignin.png)

4. "ログインが完了しました" というメッセージが表示されたら、API アプリの Get メソッドの URL をもう一度入力します。

    今回は認証済みであるため、呼び出しは成功します。 ゲートウェイは、ユーザーが認証されたユーザーであると認識し、ユーザーの要求を API アプリに渡します。

    ![ログイン完了](./media/app-service-api-dotnet-add-authentication/logincomplete.png)

    ![Chrome の GET 応答](./media/app-service-api-dotnet-add-authentication/chromeget.png)

    Swagger UI を有効にしている場合、Swagger UI ページにも移動できるようになりました。 ただし、赤色がわかります **エラー** 下にあるアイコンが、ページの上を右、およびアイコンをクリックする場合は、Swagger JSON ファイルにアクセスできることを示すメッセージが表示されます。 これは、Swagger で Zumo トークンを含めずに AJAX を呼び出し、JSON ファイルを取得しようとすることが原因です。 これは、Swagger UI ページの動作を妨げません。

## Postman を使用した Post 要求の送信

ゲートウェイにログインすると、ゲートウェイは認証トークンを返信します。  このトークンは、ゲートウェイを経由する外部のソースからのすべての要求に含まれている必要があります。 ブラウザーで API にアクセスすると、通常、ブラウザーはトークンを cookie に格納し、後続のすべての呼び出しと共にこのトークンを API に送信します。

このため、バックグラウンドで行われている処理を確認することができます。チュートリアルこのセクションでは、ブラウザー ツールを使用して Post 要求を作成および送信し、cookie から認証トークンを取得して HTTP ヘッダーに含めます。 このセクションは省略可能です。前のセクションで既に、API アプリが認証済みアクセスのみを受け付けることを検証済みです。

以下の手順では、Chrome ブラウザーで Postman ツールを使用する方法を示していますが、任意の REST クライアント ツールとブラウザー開発者ツールでもこれと同じ操作を実行できます。

1. Chrome ブラウザーのウィンドウで、前のセクションで示した認証の手順を実行してから、開発者ツールを開きます (F12)。

    ![[リソース] タブに移動](./media/app-service-api-dotnet-add-authentication/resources.png)

3.  **リソース** タブ Chrome 開発者ツール、ゲートウェイの cookie を検索しの値をトリプル クリックして、 **x zumo 認証** cookie をすべて選択します。

    **注:**  すべての cookie の値を取得するかどうかを確認します。 ダブルクリックでは、最初の部分しか取得されません。

5. 右クリックし、 **値** の **x zumo 認証** クリックして、クッキー **コピー**します。

    ![認証トークンのコピー](./media/app-service-api-dotnet-add-authentication/copyzumotoken.png)

4. Postman 拡張機能をまだ Chrome ブラウザーにインストールしていない場合は、インストールします。

6. Postman 拡張機能を開きます。

7. 要求 URL のフィールドに、先ほど使用した、API アプリの Get メソッドの URL を入力しますが、末尾の `get/` は省略します。
 
        http://[apiappurl]/api/contacts
    
8. クリックして **ヘッダー**, 、追加し、 *x zumo 認証* ヘッダー。 クリップボードからトークンの値を貼り付け、 **値** フィールドです。

9. 追加、 *Content-type* 値を持つヘッダー *-application/json*します。

10. クリックして **フォーム データ**, 、し、追加、 *にお問い合わせください* 次の値を持つキー。

        {   "Id": 0,   "Name": "Li Yan",   "EmailAddress": "yan@contoso.com" }

11. [送信] をクリックします。

    API アプリの戻り値を *201 Created* 応答します。

    ![ヘッダーと本文の追加](./media/app-service-api-dotnet-add-authentication/addcontact.png)

12. この要求が認証トークンなしでは実行されないことを確認するために、認証ヘッダーを削除してから、もう一度 [送信] をクリックします。

    取得する、 *403 アクセス不可* 応答します。

    ![応答 403 Forbidden](./media/app-service-api-dotnet-add-authentication/403forbidden.png)

## ログオンしているユーザーに関する情報の取得

このセクションでは、ログオン ユーザーの名前と電子メール アドレスを取得して返すように ContactsList API アプリのコードを変更します。  

1. Visual Studio に配置されている API アプリ プロジェクトを開く [API アプリのデプロイ](app-service-dotnet-deploy-api-app.md) およびチュートリアルではこの呼び出しされました。

3. apiapp.json ファイルを開き、API アプリで Azure Active Directory 認証を使用することを示す行を追加します。

        "authentication": [{"type": "aad"}]

    最終的な apiapp.json ファイルは、次の例のようになります。

        {
            "$schema": "http://json-schema.org/schemas/2014-11-01/apiapp.json#",
            "id": "ContactsList",
            "namespace": "microsoft.com",
            "gateway": "2015-01-14",
            "version": "1.0.0",
            "title": "ContactsList",
            "summary": "",
            "author": "",
            "endpoints": {
                "apiDefinition": "/swagger/docs/v1",
                "status": null
            },
            "authentication": [{"type": "aad"}]
        }

    このチュートリアルでは、例として Azure Active Directory を使用しています。 その他のプロバイダーについては、適切な識別子を使用します。 有効なプロバイダー値を次に示します。

    * "aad"
    * "microsoftaccount"
    * "google"
    * "twitter"
    * "facebook" 

3.  *ContactsController.cs* ファイルに追加し、 `using` ステートメントをファイルの先頭にします。

        using Microsoft.Azure.AppService.ApiApps.Service;

2. コードで置き換え、 `Get` メソッドを次のコードです。

        var runtime = Runtime.FromAppSettings(Request);
        var user = runtime.CurrentUser;
        TokenResult token = await user.GetRawTokenAsync("aad");
        var name = (string)token.Claims["name"];
        var email = (string)token.Claims["http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"];
        return new Contact[]
        {
            new Contact { Id = 1, EmailAddress = email, Name = name }
        };

    このコードは、3 つのサンプルの連絡先ではなく、ログオン ユーザーの連絡先情報を返します。 

    サンプル コードでは、Azure Active Directory を使用しています。 その他のプロバイダーについては、前の手順に示すように、適切なトークン名と要求識別子を使用します。

    利用できる Azure Active Directory の要求の詳細については、次を参照してください。 [サポートされているトークンとクレームの種類](https://msdn.microsoft.com/library/dn195587.aspx)します。

3. `Microsoft.Azure.AppService.ApiApps.Service` の using ステートメントを追加します。

        using Microsoft.Azure.AppService.ApiApps.Service;

3. プロジェクトを再デプロイします。  

    Visual Studio はに従ってプロジェクトをデプロイしたときの設定を保存すること、 [展開](app-service-dotnet-deploy-api-app.md) チュートリアルです。  プロジェクトを右クリックし、をクリックして **発行**, 、クリックして **発行** で、 **Web の発行** ダイアログ。

6. 保護されている API アプリに Get 要求を送信したときの手順に従います。 

    応答メッセージに、ログインするときに使用したユーザーの名前と ID が表示されます。

    ![Response message with logged on user](./media/app-service-api-dotnet-add-authentication/chromegetuserinfo.png)

## 次のステップ

ここまでは、Azure Active Directory またはソーシャル プロバイダーの認証を必要とすることによって Azure API アプリを保護する方法について説明しました。 詳細については、次を参照してください。 [API apps と mobile apps の認証](../app-service/app-service-authentication-overview.md)します。 

[Azure portal]: https://manage.windowsazure.com/
[Azure preview portal]: https://portal.azure.com/


