<properties
    pageTitle="Azure AD B2C プレビュー: Graph API を使用する |Microsoft Azure"
    description="アプリケーション ID を使用して B2C テナント用の Graph API を呼び出してプロセスを自動化する方法。"
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="msmbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/08/2015"
    ms.author="dastrock"/>


# Azure AD B2C プレビュー: Graph API の使用

Azure AD B2C テナントは、非常に大きくなる傾向があります。これは、多くの一般的なテナント管理タスクをプログラムで実行する必要があることを意味します。  
主な例は、ユーザーの管理 - B2C テナントに既存のユーザー ストアを移行する必要がありますか、ホストのユーザーをしたいです。 
バック グラウンドでの Azure AD でユーザー アカウントの作成、独自のページに登録します。 このようなタスクを作成する機能が必要
読み取り、更新、および実行することができます - ユーザー アカウントを削除、Azure AD Graph API を使用します。

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

B2C テナントでは、主に 2 つのモードで Graph API と通信します。

- 対話型の一度だけ実行されるタスクでは、多くの場合、B2C テナントの管理者アカウントとして機能する管理タスクを実行します。
  このモードでは、管理者は、Graph API への呼び出しを実行する前に、資格情報を使用してログインする必要があります。
- 何らかの種類を許可するために必要なサービス アカウントを使用して管理タスクを実行する対象は、自動化された継続的なタスクの
権限です。 Azure AD でできるようにアプリケーションを登録して、"アプリケーション id"を使用して、使用して Azure AD への認証
[OAuth 2.0 クライアント資格情報付与](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)します。 この場合、 
appilication では、特定のユーザーとしてではなく、それ自体として機能する Graph API を呼び出します。

この記事では、後者の自動化された事例を実行する方法を示します。 デモンストレーションとして、ユーザーの CRUD 操作を実行する .NET 4.5 "B2CGraphClient" を構築します。
いろいろなことを試すために、クライアントにはさまざまなメソッドを呼び出すことができる Windows コマンド ライン インターフェイスがあるものとします。 ただし、コードに書き込まれます 
非対話型の自動化された方式で動作します。 それでは始めましょう。

## Azure AD B2C テナントを取得する

Azure AD B2C テナントとは、グローバル管理者アカウントが必要、ユーザーのアプリケーションを作成したり、すべての Azure AD との対話、前に
そのテナントです。 場合は、既に 1 つがあるない、次のガイドを [Azure AD B2C 入門](active-directory-b2c-get-started.md)します。

## サービス アプリケーションをテナントに登録する

B2C テナントが用意できたら、Azure AD Powershell コマンドレットを使用してサービス アプリケーションを作成する必要があります。
最初に、ダウンロードし、インストール、 [Microsoft Online Services サインイン アシスタント](http://go.microsoft.com/fwlink/?LinkID=286152)します。ダウンロードして使用できます。
(& a) をインストール、 [64 ビット Azure Active Directory Module for Windows Powershell](http://go.microsoft.com/fwlink/p/?linkid=236297)します。
> [AZURE.NOTE]
B2C テナントで Graph API を使用するには、以下の指示に従い、PowerShell を使用して専用のアプリケーションを登録する必要があります。 Azure ポータルに登録済みの既存の B2C アプリケーションを再利用することはできません。 これは Azure AD B2C プレビューの制限であり、近い将来、この制限は削除されます。その時点で、この記事が更新される予定です。 

Powershell モジュールをインストールした後、Powershell を開き、B2C テナントに接続します。 [実行後 `Get-credential`, ができます
ユーザー名とパスワードの入力を求め、B2C テナントの管理者アカウントのものを入力します。

```
> $msolcred = Get-Credential
> Connect-MsolService -credential $msolcred
```

アプリケーションを作成する前に、新しい "クライアント シークレット" を生成する必要があります。 アプリケーションはクライアント シークレットを使用して Azure に対して認証するには
AD アクセス トークンを取得します。 Powershell で、有効なシークレットを生成できます。

```
> $bytes = New-Object Byte[] 32
> $rand = [System.Security.Cryptography.RandomNumberGenerator]::Create()
> $rand.GetBytes($bytes)
> $rand.Dispose()
> $newClientSecret = [System.Convert]::ToBase64String($bytes)
> $newClientSecret
```

上記の最後のコマンドによって、新しいクライアント シークレットが出力されます。 それを安全な場所にメモしておいてください。すぐに必要になります。 作成することができますので、 
このアプリケーションでは、資格情報として、アプリの新しいクライアント シークレットを提供する:

```
> New-MsolServicePrincipal -DisplayName "My New B2C Graph API App" -Type password -Value $newClientSecret

DisplayName           : My New B2C Graph API App
ServicePrincipalNames : {dd02c40f-1325-46c2-a118-4659db8a55d5}
ObjectId              : e2bde258-6691-410b-879c-b1f88d9ef664
AppPrincipalId        : dd02c40f-1325-46c2-a118-4659db8a55d5
TrustedForDelegation  : False
AccountEnabled        : True
Addresses             : {}
KeyType               : Password
KeyId                 : a261e39d-953e-4d6a-8d70-1f915e054ef9
StartDate             : 9/2/2015 1:33:09 AM
EndDate               : 9/2/2016 1:33:09 AM
Usage                 : Verify
```

アプリケーションの作成が成功すると、上記のようなアプリケーションの一部のプロパティが出力されます。 どちらもする必要があります、 `ObjectId` と
`AppPrincipalId`, 、同様に、ダウンそれらの値をできるようにコピーします。

これで B2C テナント内にアプリケーションが作成されたので、ユーザーの CRUD 操作を実行するために必要なアクセス許可をそのアプリケーションに割り当てる必要があります。 準備した
アプリケーションの 3 つの異なる役割を割り当てる: ディレクトリのリーダー (ユーザーの読み取り)、ディレクトリのライター (の作成とユーザーを更新)、およびユーザー アカウント
管理者 ([ユーザーの削除) します。 これらの役割が実行できるように、よく知られた識別子をして、次のコマンドは、置換、 `RoleMemberObjectId`
パラメーターと、 `ObjectId` 上からです。 実行してみてください、すべてのディレクトリ ロールの一覧を表示する `Get-msolrole`します。

```
> Add-MsolRoleMember -RoleObjectId 88d8e3e3-8f55-4a1e-953a-9b9898b8876b -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId 9360feb5-f418-4baa-8175-e2a00bac4301 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
> Add-MsolRoleMember -RoleObjectId fe930be7-5e62-47db-91af-98c3a49a38b1 -RoleMemberObjectId <Your-ObjectId> -RoleMemberType servicePrincipal
```

B2C テナントに対してユーザーの作成、読み取り、更新、削除を実行する権限を持つアプリケーションが用意されました。次に、それを使用するコードを記述していきましょう。

## サンプル コードをダウンロード、構成、ビルドする

まず、サンプル コードをダウンロードして実行します。 その後、バック グラウンドで何が起こっているかを調べることができます。 Visual Studio Online アカウントは 
[.zip とサンプル コードをダウンロード](https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet/archive/master.zip) または挿入すること
好みのディレクトリ。

```
git clone https://github.com/AzureADQuickStarts/B2C-GraphAPI-DotNet.git
```

開いている、 `B2CGraphClient\B2CGraphClient.sln` Visual Studio で Visual Studio のソリューションです。  `B2CGraphClient` プロジェクト、ファイルを開きます `App.config`します。 *Views\\Home\\AllDates.cshtml*
独自の値を次の 3 つのアプリケーション設定のようにします。

```
<appSettings>
    <add key="b2c:Tenant" value="contosob2c.onmicrosoft.com" />
    <add key="b2c:ClientId" value="{The AppPrincipalId from above}" />
    <add key="b2c:ClientSecret" value="{The client secret you generated above}" />
</appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

これで、右クリック、 `B2CGraphClient` ソリューションと、サンプルを再構築します。 実行可能ファイルのようになりますが成功すると、 `B2C.exe` にある `B2CGraphClient\bin\Debug`します。

## Graph API でユーザーの CRUD を使用する

使用するには、B2CGraphClient cmd Windows コマンド プロンプトとするために cd を開く、 `デバッグ` ディレクトリ。 実行して、 `B2C ヘルプ` コマンドです。

```
> cd B2CGraphClient\bin\Debug
> B2C Help
```

各コマンドの短い説明が表示されます。 これらのコマンドのいずれかを呼び出すと、B2CGraphClient が Azure AD Graph API に要求を行います。

### アクセス トークンの取得

Graph API へのすべての要求は、認証用のアクセス トークンを必要とします。 B2CGraphClient では、アクセス トークンの取得を支援するためにオープン ソースである Active Directory Authentication Library (ADAL) を使用します。
常に ADAL を使用してトークンを取得する必要はありません。トークンは、HTTP 要求を自分で作成することで取得できます。 ADAL は、単純な API を提供しの注意を払ってトークンの取得を少し簡単に
トークンのキャッシュなど、いくつかの重要な詳細情報にアクセスします。
> [AZURE.NOTE]
    このコード サンプルでは、一般に利用できる ADAL のバージョンである ADAL v2 を意図的に使用しています。 Azure AD B2C で動作するように設計された ADAL v4 は使用していないことに注意してください。
    Azure AD B2C プレビューでは、Graph API と通信を行うには ADAL v2 を使用する必要があります。 時間の経過と共にはできるように ADAL v4 による Graph API アクセスできるように、2 つを使用する必要はありません。
    完全な Azure AD B2C ソリューションで、ADAL の異なるバージョンにします。

インスタンスを作成して、B2CGraphClient を実行すると、 `B2CGraphClient` クラスです。 このクラスのコンス トラクターは、ADAL の認証のスキャフォールディングを設定します。

```C#
public B2CGraphClient(string clientId, string clientSecret, string tenant)
{
    // The client_id, client_secret, and tenant are provided in Program.cs, which pulls the values from App.config
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.tenant = tenant;

    // The AuthenticationContext is ADAL's primary class, in which you indicate the tenant to use.
    this.authContext = new AuthenticationContext("https://login.microsoftonline.com/" + tenant);

    // The ClientCredential is where you pass in your client_id and client_secret, which are 
    // provided to Azure AD in order to receive an access_token using the app's identity.
    this.credential = new ClientCredential(clientId, clientSecret);
}
```

使用してみましょう、 `B2C Get-user` ようなコマンドです。  `Get-user` せずに、追加の入力を CLI 呼び出し呼び出される、 `B2CGraphClient.GetAllUsers(...)` メソッドです。
このメソッドを呼び出す `B2CGraphClient.SendGraphGetRequest(...)`, 、Graph API に対する HTTP GET 要求を送信します。 GET 要求を送信する前に、最初のアクセスを取得します。
ADAL を使用してトークン:

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    // First, use ADAL to acquire a token using the app's identity (the credential)
    // The first parameter is the resource we want an access_token for; in this case, the Graph API.
    AuthenticationResult result = authContext.AcquireToken("https://graph.windows.net", credential);

    ... 
```

ご覧のとおり、することができますアクセス トークンを取得、Graph API の ADAL を呼び出すことによって `AuthenticationContext.AcquireToken(...)` メソッドです。 ADAL は、access_token を表すを返す
アプリケーションの id。

### ユーザーの読み取り

Graph API からユーザーの一覧を取得または特定のユーザーを取得する場合は、HTTP GET 要求を送信することができます、 `"/users"` エンドポイント。 テナントのすべてのユーザーの要求
ようになります。

```
GET https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=beta
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

この要求の実行を確認するには、次を実行してみます。

 ```
 > B2C Get-User
 ```

ここで注意すべき重要な点は 2 つあります。

- ADAL 経由で取得したアクセス トークンに追加されて、 `承認` ヘッダーを使用して、 `ベアラー` スキームです。
- B2C テナント用には、クエリ パラメーターを使用する必要があります `api バージョン ベータ版を =`します。


> [AZURE.NOTE]
    Azure AD Graph API のベータ版の機能はプレビュー段階です。 具体的な 
    [この Graph API チームのブログ投稿](http://blogs.msdn.com/b/aadgraphteam/archive/2015/04/10/graph-api-versioning-and-the-new-beta-version.aspx) 
    ベータ版の詳細。

これらの詳細の両方の処理、 `B2CGraphClient.SendGraphGetRequest(...)` メソッド。

```C#
public async Task<string> SendGraphGetRequest(string api, string query)
{
    ...

    // For B2C user managment, be sure to use the beta Graph API version.
    HttpClient http = new HttpClient();
    string url = "https://graph.windows.net/" + tenant + api + "?" + "api-version=beta";
    if (!string.IsNullOrEmpty(query))
    {
        url += "&" + query;
    }

    // Append the access token for the Graph API to the Authorization header of the request, using the Bearer scheme.
    HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, url);
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
    HttpResponseMessage response = await http.SendAsync(request);

    ... 
```

### コンシューマー ユーザー アカウントの作成

HTTP POST 要求を送信するには、B2C テナントにユーザー アカウントを作成するときに、 `"/users"` エンドポイント。

```
POST https://graph.windows.net/contosob2c.onmicrosoft.com/users?api-version=beta
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 338

{
    // These properties are all required for creating consumer users.

    "accountEnabled": true,
    "alternativeSignInNamesInfo": [             // controls what identifier the user uses to sign into their account
        {
            "type": "emailAddress",             // can be 'emailAddress' or 'userName'
            "value": "joeconsumer@gmail.com"
        }
    ],
    "creationType": "NameCoexistence",          // always set to 'NameCoexistence'
    "displayName": "Joe Consumer",              // a value that can be used for diplaying to the end-user
    "mailNickname": "joec",                     // a mail alias for the user
    "passwordProfile": {
        "password": "P@ssword!",
        "forceChangePasswordNextLogin": false   // always set to false
    },
    "passwordPolicies": "DisablePasswordExpiration"
}
```

上記の要求に含まれる各プロパティは、コンシューマー ユーザーを作成するために必要です。  `//` コメントに図に示されている - しません。
実際の要求に含めます。

この要求の実行を確認するには、次のコマンドのいずれかを実行してみます。

```
> B2C Create-User ..\..\..\usertemplate-email.json
> B2C Create-User ..\..\..\usertemplate-username.json
```

`Create User` コマンドを受け取り、 `.json` ユーザー オブジェクトの JSON 表現を含む入力パラメーターとしてファイルです。 2 つの例がある `.json` ファイル
サンプル コードに含まれている `usertemplate email.json` と `usertemplate username.json` -ことは、ニーズに合わせて変更できます。 他に、 
必須のフィールド、使用できるこれらのファイルに含まれる省略可能なフィールドの数があります。 これらの他のフィールドの詳細については
記載する、 [Azure AD Graph API のエンティティの参照](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#UserEntity)します。

この POST 要求はの contructed `B2CGraphClient.SendGraphPostRequest(...)`, であります。

- アクセス トークンをアタッチ、 `承認` 要求のヘッダー。
- セット `api バージョン ベータ版を =`します。
- JSON ユーザー オブジェクトを要求の本文に含めます。

### コンシューマー ユーザー アカウントの更新

ユーザー オブジェクトの更新は、ユーザー オブジェクトの作成とよく似ていますが、更新では HTTP PATCH 動詞を使用します。

```
PATCH https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=beta
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
Content-Type: application/json
Content-Length: 37

{
    "displayName": "Joe Consumer",              // this request only updates the user's displayName
}
```

JSON ファイルを新しいデータで更新し、B2CGraphClient を使用して次のコマンドのいずれかを実行することで、ユーザーを更新できます。

```
> B2C Update-User <user-object-id> ..\..\..\usertemplate-email.json
> B2C Update-User <user-object-id> ..\..\..\usertemplate-username.json
```

検査、 `B2CGraphClient.SendGraphPatchRequest(...)` メソッドの詳細については、この要求を送信する方法です。

### ユーザーの削除

ユーザーの削除は単純な処理です。HTTP DELETE 動詞を使用して、適切なオブジェクト ID を持つ URL を構築するだけです。

```
DELETE https://graph.windows.net/contosob2c.onmicrosoft.com/users/<user-object-id>?api-version=beta
Authorization: Bearer eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJod...
```

例を確認するには、次のコマンドを実行し、コンソールに出力される DELETE 要求の結果を観察します。

```
> B2C Delete-User <object-id-of-user>
```

検査、 `B2CGraphClient.SendGraphDeleteRequest(...)` メソッドの詳細については、この要求を送信する方法です。

ユーザー管理に加え、Azure AD Graph API で実行できるアクションは他にも多数あります。 この 
[Azure AD Graph API リファレンス](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) サンプル要求と共に、各アクションの詳細について説明します。


## カスタム属性の使用

ほぼすべてのコンシューマー アプリケーションは、何らかの種類のカスタム ユーザー プロファイル情報を格納する必要があります。 これを行う方法の 1 つは、B2C テナントのカスタム属性を定義するには
ユーザー オブジェクトに、他のプロパティと同じようにその属性を処理できるようにします。 属性の更新、属性、属性でクエリを削除 
属性をトークンの署名で要求として送信します。

B2C テナントでは、カスタム属性を定義するを参照してください。、 [B2C プレビューのカスタム属性のリファレンス](active-directory-b2c-reference-custom-attr.md)します。

B2C テナント内に定義されたカスタム属性は、B2CGraphClient を使用して表示できます。

```
> B2C Get-B2C-Application
> B2C Get-Extension-Attribute <object-id-in-the-output-of-the-above-command>
```

これらの関数の出力によって、各カスタム属性の詳細が明らかになります。

```JSON
{
      "odata.type": "Microsoft.DirectoryServices.ExtensionProperty",
      "objectType": "ExtensionProperty",
      "objectId": "cec6391b-204d-42fe-8f7c-89c2b1964fca",
      "deletionTimestamp": null,
      "appDisplayName": "",
      "name": "extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number",
      "dataType": "Integer",
      "isSyncedFromOnPremises": false,
      "targetObjects": [
        "User"
      ]
}
```

など、完全名を使用できる `extension_55dc0861f9a44eb999e0a8a872204adb_Jersey_Number` ユーザー オブジェクトのプロパティとして。 単に更新、 `.json` で新しいファイル
プロパティ、および実行の値のプロパティ:

```
> B2C Update-User <object-id-of-user> <path-to-json-file>
```

これで終わりです。 B2CGraphClient を使用して B2C テナントのユーザーをプログラムで管理できるサービス アプリケーションが作成されました。 独自のアプリケーション id を使用してください。 
Azure AD Graph API に対して認証して、client_secret を使用してトークンを取得します。 独自のアプリケーションにこの機能を組み込む場合に重要な点に注意してください。
B2C アプリ。

- テナントの適切なアクセス許可をアプリケーションに付与する必要があります。
- 現時点では、アクセス トークンを取得するには ADAL v2 を使用する必要があります (または、ライブラリなしでプロトコル メッセージを直接送信します)。
- Graph API を呼び出すときに使用して [`api バージョン ベータ版を =`] (http://blogs.msdn.com/b/aadgraphteam/archive/2015/04/10/graph-api-versioning-and-the-new-beta-version.aspx)。
- コンシューマー ユーザーの作成と更新を行うときは、上記で説明したいくつかの必須プロパティがあります。

> [AZURE.IMPORTANT]
必要なアカウントに、ディレクトリのレプリケーションの特性は、基になる Azure AD B2C をサービス (読み取り [この](http://blogs.technet.com/b/ad/archive/2014/09/02/azure-ad-under-the-hood-of-our-geo-redundant-highly-available-geo-distributed-cloud-directory.aspx) 詳細資料) B2C アプリで Azure AD Graph API を使用する場合。 コンシューマーが **サインアップ** ポリシーを使用して B2C にサインアップしたすぐ後に、アプリ内で Azure AD Graph API を使用してユーザー オブジェクトを読み取ろうとしてもできない場合があります。 その際は、レプリケーション プロセスが完了するまで数秒待つ必要があります。 一般公開時には、Azure AD Graph API とディレクトリ サービスによって提供される "書き込みと読み取りの整合性保証" について、より具体的なガイダンスを公開する予定です。

Graph API を使用した B2C テナントに対する操作に関して、ご質問やご要望がある場合は、いつでもご遠慮なくお知らせください。 記事にコメントを記入してください。
または、コード サンプルの GitHub リポジトリ内、問題を報告します。





