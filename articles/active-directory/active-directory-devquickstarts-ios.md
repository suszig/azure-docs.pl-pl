<properties
    pageTitle="Azure AD iOS の概要 | Microsoft Azure"
    description="サインインと、OAuth を用いた Azure AD の保護 API の呼び出しのために Azure AD と統合する、iOS アプリケーションの構築方法を説明します。"
    services="active-directory"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/13/2015"
    ms.author="brandwe"/>


# Azure AD の iOS アプリへの統合

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD には、保護されたリソースにアクセスする必要がある iOS クライアント向けに、Active Directory 認証ライブラリ (ADAL) が用意されています。 ADAL の唯一の目的は、アプリがアクセス トークンを容易に取得できるようにすることです。 それがどれほど簡単であるかを示すために、ここで、次のような、Objective-C の To-Do List アプリを構築します。

-   アクセスを使用して、Azure AD Graph API を呼び出すためのトークンを取得、 [OAuth 2.0 認証プロトコル](https://msdn.microsoft.com/library/azure/dn645545.aspx)します。
-   指定されたエイリアスを持つユーザーをディレクトリで検索します。

完全に動作するアプリケーションを構築するには、次の手順を行う必要があります。

2. Azure AD にアプリケーションを登録する
3. ADAL をインストールおよび構成する
5. ADAL を使用して、Azure AD からトークンを取得する

最初に、 [アプリのスケルトンをダウンロードする](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) または [、完全なサンプルをダウンロード](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip)します。 また、ユーザーを作成し、アプリケーションを登録することを可能にするための Azure AD テナントも必要です。 場合は、テナントをまだ持っていない [いずれかを取得する方法について](active-directory-howto-tenant.md)します。

## *1.IOS のリダイレクト URI を決定します。*

特定の SSO シナリオでアプリケーションを安全に起動するには、**リダイレクト URI** を特定の形式で作成する必要があります。 リダイレクト URI は、トークンがそのトークンを要求した適切なアプリケーションに返されるようにするために使用します。

iOS のリダイレクト URI の形式は次のとおりです。

```
<app-scheme>://<bundle-id>
```

-   **aap-scheme** - This is registered in your XCode project. It is how other applications can call you. You can find this under Info.plist -> URL types -> URL Identifier. You should create one if you don't already have one or more configured.
-   **bundle-id** - This is the Bundle Identifier found under "identity" un your project settings in XCode.

An example for this QuickStart code would be: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## *2. Register the DirectorySearcher Application*
To enable your app to get tokens, you'll first need to register it in your Azure AD tenant and grant it permission to access the Azure AD Graph API:

-   Sign into the Azure Management Portal
-   In the left hand nav, click on **Active Directory**
-   Select a tenant in which to register the application.
-   Click the **Applications** tab, and click **Add** in the bottom drawer.
-   Follow the prompts and create a new **Native Client Application**.
    -   The **Name** of the application will describe your application to end-users
    -   The **Redirect Uri** is a scheme and string combination that Azure AD will use to return token responses.  Enter a value specific to your application based on the information above.
-   Once you've completed registration, AAD will assign your app a unique client identifier.  You'll need this value in the next sections, so copy it from the **Configure** tab.
- Also in **Configure** tab, locate the "Permissions to Other Applications" section.  For the "Azure Active Directory" application, add the **Access Your Organization's Directory** permission under **Delegated Permissions**.  This will enable your application to query the Graph API for users.

## *3. Install & Configure ADAL*
Now that you have an application in Azure AD, you can install ADAL and write your identity-related code.  In order for ADAL to be able to communicate with Azure AD, you need to provide it with some information about your app registration.
-   Begin by adding ADAL to the DirectorySearcher project using Cocapods.
```
$ vi ポッドファイル
```
Add the following to this podfile:
```
ソース 'https://github.com/CocoaPods/Specs.git'
link_with [QuickStart]
xcodeproj 'クイック スタート'

pod 'ADALiOS'
```

Now load the podfile using cocoapods. This will create a new XCode Workspace you will load.
```
$ pod をインストールします。
...
$ 開く QuickStart.xcworkspace
```

-   In the QuickStart project, open the plist file `settings.plist`.  Replace the values of the elements in the section to reflect the values you input into the Azure Portal.  Your code will reference these values whenever it uses ADAL.
    -   The `tenant` is the domain of your Azure AD tenant, e.g. contoso.onmicrosoft.com
    -   The `clientId` is the clientId of your application you copied from the portal.
    -   The `redirectUri` is the redirect url you registered in the portal.

## *4.  Use ADAL to Get Tokens from AAD*
The basic principle behind ADAL is that whenever your app needs an access token, it simply calls a completionBlock `+(void) getToken : `, and ADAL does the rest.  

-   In the `QuickStart` project, open `GraphAPICaller.m` and locate the `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` comment near the top.  This is where you pass ADAL the coordinates through a CompletionBlock to communicate with Azure AD and tell it how to cache tokens.

```ObjC
+(void) getToken : (BOOL) clearCache
           parent:(UIViewController*) parent
completionHandler:(void (^) (NSString*, NSError*))completionBlock;
{
    AppData* data = [AppData getInstance];
    if(data.userItem){
        completionBlock(data.userItem.accessToken, nil);
        return;
    }

    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:data.resourceId
                                 clientId:data.clientId
                              redirectUri:redirectUri
                           promptBehavior:AD_PROMPT_AUTO
                                   userId:data.userItem.userInformation.userId
                     extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                          completionBlock:^(ADAuthenticationResult *result) {

                              if (result.status != AD_SUCCEEDED)
                              {
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                              }
                          }];
}
```

- 次に、このトークンを使用して、グラフでユーザーを検索する必要があります。 検索、 `//TODO: SearchUsersList を実装する` コメント メソッドをクエリに Azure AD Graph API GET 要求を指定された検索用語で UPN が始まるユーザー用です。 Graph API をクエリするためで、access_token を含める必要がありますが、 `承認` ヘッダー、要求のこれは、adal です。

```ObjC
+(void) searchUserList:(NSString*)searchString
                parent:(UIViewController*) parent
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];


    [self craftRequest:[self.class trimString:graphURL]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

         if (error != nil)
         {
             completionBlock(nil, error);
         }
         else
         {

             NSOperationQueue *queue = [[NSOperationQueue alloc]init];

             [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                 if (error == nil && data != nil){

                     NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

                     // We can grab the top most JSON node to get our graph data.
                     NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                     // Don't be thrown off by the key name being "value". It really is the name of the
                     // first node. :-)

                     //each object is a key value pair
                     NSDictionary *keyValuePairs;
                     NSMutableArray* Users = [[NSMutableArray alloc]init];

                     for(int i =0; i < graphDataArray.count; i++)
                     {
                         keyValuePairs = [graphDataArray objectAtIndex:i];

                         User *s = [[User alloc]init];
                         s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                         s.name =[keyValuePairs valueForKey:@"givenName"];

                         [Users addObject:s];
                     }

                     completionBlock(Users, nil);
                 }
                 else
                 {
                     completionBlock(nil, error);
                 }

             }];
         }
     }];

}
```
- アプリが呼び出すことによって、トークンを要求したときに `getToken(...)`, 、ADAL を資格情報のユーザーを求めることがなく、トークンを返そうとします。 ADAL は、トークンを取得するにはユーザーのサインインが必要であると判断した場合、ログイン ダイアログを表示し、ユーザーの資格情報を収集し、認証が成功するとトークンを返します。 スロー返せないが何らかの理由でトークンを返せない場合、 `AdalException`します。
- 注意して、 `AuthenticationResult` オブジェクトを含む、 `tokenCacheStoreItem` アプリが必要になる情報を収集するために使用するオブジェクト。 QuickStart では、 `tokenCacheStoreItem` 認証が既に行われたかどうかを判断するために使用します。


## 手順 5. アプリケーションをビルドして実行する

ご利用ありがとうございます。 これで、作業中の iOS アプリケーションでは、ユーザーの認証、OAuth 2.0 を使用した Web API の安全な呼び出し、ユーザーに関する基本情報の取得が可能になりました。 テナントに一連のユーザーを設定します (設定していない場合)。 QuickStart アプリを実行し、そのユーザーの 1 人としてサインインします。 UPN に基づいて、他のユーザーを検索します。 アプリを閉じて、再び実行します。 ユーザーのセッションがそのままに維持されていることに注意します。

ADAL を使用することにより、これらの共通 ID 機能のすべてを容易にアプリケーションに組み込むことができます。 キャッシュ管理、OAuth プロトコル サポート、ログイン UI を使用してのユーザーの提示、有効期限切れとなったトークンの更新など、面倒な操作を容易に実装できます。 本当に知っておきたいは、単一の API 呼び出し、 `getToken`します。

リファレンスについては、完全なサンプル (構成値) を除くが提供される [ここ](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip)します。 ここからは、さらなるシナリオに進むことができます。 次のチュートリアルを試してみてください。

[Node.JS Web API のセキュリティ保護 >>](../active-directory-devquickstarts-webapi-nodejst.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]




