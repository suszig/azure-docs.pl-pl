<properties
    pageTitle="Active Directory 認証ライブラリのシングル サインオンによるアプリケーションの認証 (iOS) | Microsoft Azure"
    description="iOS アプリケーションで ADAL を使用してシングル サインオンのユーザーを認証する方法について説明します。"
    documentationCenter="ios"
    authors="mattchenderson"
    manager="dwrede"
    editor=""
    services="mobile-services"/>

<tags
    ms.service="mobile-services"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="mahender"/>

# Active Directory 認証ライブラリのシングル サインオンによるアプリケーションの認証

[AZURE.INCLUDE [mobile-service-note-mobile-apps](../../includes/mobile-services-note-mobile-apps.md)]

&nbsp;


[AZURE.INCLUDE [mobile-services-selector-adal-sso](../../includes/mobile-services-selector-adal-sso.md)]

##概要

このチュートリアルでは、Active Directory 認証ライブラリを使用して、クイック スタート プロジェクトに認証を追加します。

ユーザーを認証できるようにするには、Azure Active Directory (AAD) にアプリケーションを登録する必要があります。 この処理は 2 段階の手順で実行されます。 まず、モバイル サービスを登録し、モバイル サービスに対するアクセス許可を公開する必要があります。 次に、iOS アプリケーションを登録してこれらのアクセス許可へのアクセス権を付与する必要があります。


>[AZURE.NOTE] このチュートリアルは、モバイル サービスを使用してシングル サインオン Azure Active Directory 認証 iOS アプリを実行する方法を理解するのに役立ちます。 チュートリアルの完了モバイル サービスの初めての場合は、[モバイル サービスの開始を取得] です。


##前提条件


このチュートリアルには、次のものが必要です。

* XCode 4.5 および iOS 6.0 (またはそれ以降のバージョン)
* 完了、[モバイル サービスの開始を取得] チュートリアルです。
* 完了、[Azure Active Directory アカウント ログインを使用するアプリケーションの登録]
* Microsoft Azure Mobile Services SDK
* [Active Directory 認証ライブラリ iOS の場合]

[AZURE.INCLUDE [mobile-services-dotnet-adal-register-client](../../includes/mobile-services-dotnet-adal-register-client.md)]

##認証を要求するようにモバイル サービスを構成する

[AZURE.INCLUDE [mobile-services-restrict-permissions-dotnet-backend](../../includes/mobile-services-restrict-permissions-dotnet-backend.md)]

##クライアント アプリケーションに認証コードを追加する

1. [Active Directory 認証ライブラリ ios] をダウンロードし、プロジェクトに追加します。 また、ADAL ソースからストーリーボードを追加します。

2. QSTodoListViewController で、ADAL を次に含めます。

        #import "ADALiOS/ADAuthenticationContext.h"

2. その後、次のメソッドを追加します。

        - (void) loginAndGetData
        {
            MSClient *client = self.todoService.client;
            if (client.currentUser != nil) {
                return;
            }

            NSString *authority = @"<INSERT-AUTHORITY-HERE>";
            NSString *resourceURI = @"<INSERT-RESOURCE-URI-HERE>";
            NSString *clientID = @"<INSERT-CLIENT-ID-HERE>";
            NSString *redirectURI = @"<INSERT-REDIRECT-URI-HERE>";

            ADAuthenticationError *error;
            ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
            NSURL *redirectUri = [[NSURL alloc]initWithString:redirectURI];

            [authContext acquireTokenWithResource:resourceURI clientId:clientID redirectUri:redirectUri completionBlock:^(ADAuthenticationResult *result) {
                if (result.tokenCacheStoreItem == nil)
                {
                    return;
                }
                else
                {
                    NSDictionary *payload = @{
                        @"access_token" : result.tokenCacheStoreItem.accessToken
                    };
                    [client loginWithProvider:@"windowsazureactivedirectory" token:payload completion:^(MSUser *user, NSError *error) {
                        [self refresh];
                    }];
                }
            }];
        }


6. コードで、 `loginAndGetData` 代わりに、上記のメソッドを **挿入機関ここでは** 、形式をアプリケーションをプロビジョニングしたテナントの名前、https://login.windows.net/tenant-name.onmicrosoft.com でなければなりません。 この値は、[Azure クラシック ポータル] の Azure Active Directory の [ドメイン] タブからコピーできます。

7. コードで、 `loginAndGetData` 代わりに、上記のメソッドを **挿入リソース URI ここ** で、 **App ID URI** 、モバイル サービスのです。 [Azure Active Directory に登録する方法] を実行している場合のトピック、アプリケーション ID URI が https://todolist.azure-mobile.net/login/aad と同様にあります。

8. コードで、 `loginAndGetData` 代わりに、上記のメソッドを **INSERT CLIENT ID ここ** をネイティブ クライアント アプリケーションからコピーしたクライアント ID。

9. コードで、 `loginAndGetData` 代わりに、上記のメソッドを **挿入リダイレクト URI ここ** とログイン/モバイル サービス用のエンドポイントを実行します。 これは、https://todolist.azure-mobile.net/login/done のような文字列です。


3. QSTodoListViewController で、`[self refresh]` を次に置き換えて、`ViewDidLoad` を変更します。

        [self loginAndGetData];

##認証を使用してクライアントをテストする

1. [Product] メニューの [Run] をクリックして、アプリケーションを開始します。
2. Azure Active Directory にログインするための画面が表示されます。
3. アプリケーションが認証を実行し、Todo 項目を返します。

   ![](./media/mobile-services-dotnet-backend-ios-adal-sso-authentication/mobile-services-app-run.png)



<!-- URLs. -->
[Get started with Mobile Services]: mobile-services-dotnet-backend-ios-get-started.md
[Register your apps to use an Azure Active Directory Account login]:mobile-services-how-to-register-active-directory-authentication.md
[How to Register with the Azure Active Directory]: mobile-services-how-to-register-active-directory-authentication.md
[Azure classic portal]: https://manage.windowsazure.com/
[Active Directory Authentication Library for iOS]: https://github.com/MSOpenTech/azure-activedirectory-library-for-ios

