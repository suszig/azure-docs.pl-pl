<properties
    pageTitle="MVC プロジェクト (Visual Studio Azure Active Directory 接続済みサービス) の変更点 | Microsoft Azure "
    description="Visual Studio 接続済みサービスを使用して Azure AD に接続するときに MVC プロジェクトで何が起こるかについて説明します。"
    services="active-directory"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="tglee"/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/03/2015"
    ms.author="tarcher"/>

# MVC プロジェクト (Visual Studio Azure Active Directory 接続済みサービス) の変更点

> [AZURE.SELECTOR]
> - [Getting Started (概要)](vs-active-directory-dotnet-getting-started.md)
> - [変更内容](vs-active-directory-dotnet-what-happened.md)



## リファレンスが追加されました

### NuGet パッケージのリファレンス

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel.Tokens.Jwt**

### .NET のリファレンス

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel**
- **System.IdentityModel.Tokens.Jwt**
- **System.Runtime.Serialization**

## コードが追加されました

### コード ファイルがプロジェクトに追加された

認証スタートアップ クラス、 **App_Start/Startup.Auth.cs** Azure AD 認証のスタートアップ ロジックを含むプロジェクトに追加されました。 また、コント ローラー クラス Controllers/AccountController.cs が含むを追加するされました **SignIn()** と **SignOut()** メソッドです。 最後に、部分ビュー **Views/Shared/_LoginPartial.cshtml** Signin/signout のアクション リンクを含むようになりました。

### スタートアップ コードがプロジェクトに追加された

プロジェクトに Startup クラスが既に、 **構成** メソッドの呼び出しを含めるが更新されて **ConfigureAuth(app)**します。 それ以外の場合は、Startup クラスがプロジェクトに追加されました。

### app.config または web.config に新しい構成値が含まれる

次の構成エントリが追加されました。


    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### Azure Active Directory (AD) アプリが作成された
ウィザードで選択したディレクトリに Azure AD アプリケーションが作成されました。

##オンになっている場合 *個々 のユーザー アカウント認証を無効にする*, 、自分のプロジェクトにどのような変更が加えられたか。
NuGet パッケージのリファレンスが削除されるほか、ファイルが削除およびバックアップされます。 プロジェクトの状態によっては、追加のリファレンスやファイルを手動で削除したり、必要に応じてコードを変更しなければならない場合があります。

### 削除される NuGet パッケージのリファレンス (存在する場合)

- **Microsoft.AspNet.Identity.Core**
- **Microsoft.AspNet.Identity.EntityFramework**
- **Microsoft.AspNet.Identity.Owin**

### バックアップおよび削除されるコード ファイル (存在する場合)

以下の各ファイルがバックアップされ、プロジェクトから削除されます。 バックアップ ファイルは、プロジェクト ディレクトリのルートにある "Backup" フォルダーに配置されます。

- **App_Start\IdentityConfig.cs**
- **Controllers\ManageController.cs**
- **Models\IdentityModels.cs**
- **Models\ManageViewModels.cs**

### バックアップされるコード ファイル (存在する場合)

以下の各ファイルがバックアップされてから置き換えられます。 バックアップ ファイルは、プロジェクト ディレクトリのルートにある "Backup" フォルダーに配置されます。

- **Startup.cs**
- **App_Start\Startup.Auth.cs**
- **Controllers\AccountController.cs**
- **Views\Shared\_LoginPartial.cshtml**

## オンになっている場合 *ディレクトリ データを読み取る*, 、自分のプロジェクトにどのような変更が加えられたか。

リファレンスがさらに追加されました。

###NuGet パッケージの追加リファレンス

- **EntityFramework**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **System.Spatial**

###追加の .NET リファレンス

- **EntityFramework**
- **EntityFramework.SqlServer**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
- **System.Spatial**

###コード ファイルがプロジェクトにさらに追加された

トークン キャッシュをサポートする 2 つのファイルが追加されました: **Models\ADALTokenCache.cs** と **Models\ApplicationDbContext.cs**します。  Azure Graph API を使用するユーザー プロファイル情報へのアクセスについて説明するために、コント ローラーとビューがさらに追加されました。  これらのファイルは **Controllers\UserProfileController.cs** と **Views\UserProfile\Index.cshtml**します。

###スタートアップ コードがプロジェクトにさらに追加された

 **Startup.auth.cs** ファイルでは、新しい **OpenIdConnectAuthenticationNotifications** に追加されたオブジェクト、 **通知** のメンバー、 **OpenIdConnectAuthenticationOptions**します。  これは、OAuth コードを受信できるようにして、アクセス トークンと交換するためのものです。

###app.config または web.config にさらに変更が加えられた

次の構成エントリがさらに追加されました。

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

次の構成セクションと接続文字列が追加されました。

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


###Azure Active Directory アプリが更新された
Azure Active Directory アプリが追加されましたが、 *ディレクトリ データを読み取る* アクセス許可と追加のキーとして使用されたを作成した、 *ida: ClientSecret* で、 **web.config** ファイルです。

[Azure Active Directory の詳細を確認する](http://azure.microsoft.com/services/active-directory/)

