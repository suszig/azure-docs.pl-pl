<properties
    pageTitle="Web Api プロジェクトに変更内容 (Visual Studio の Azure Active Directory には、サービスが接続されている) |Microsoft Azure "
    description="MVC プロジェクト Visual Studio のサービスを使用して Azure AD に接続する web Api の動作について説明 ="active-directory"
    services="active-directory"
    documentationCenter=""
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

# Web API プロジェクトの変更点 (Visual Studio Azure Active Directory 接続済みサービス)

> [AZURE.SELECTOR]
> - [Getting Started (概要)](vs-active-directory-webapi-getting-started.md)
> - [変更内容](vs-active-directory-webapi-what-happened.md)

##リファレンスが追加されました

###NuGet パッケージのリファレンス

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###.NET のリファレンス

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

##コードの変更

###コード ファイルがプロジェクトに追加された

認証スタートアップ クラス、 **App_Start/Startup.Auth.cs** Azure AD 認証のスタートアップ ロジックを含むプロジェクトに追加されました。

###スタートアップ コードがプロジェクトに追加された

プロジェクトに Startup クラスが既に、 **構成** メソッドの呼び出しを含めるが更新されて `ConfigureAuth(app)`します。 それ以外の場合は、Startup クラスがプロジェクトに追加されました。


###app.config ファイルまたは web.config ファイルに新しい構成値が含まれる

次の構成エントリが追加されました。
```
    `<appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

###Azure AD アプリが作成された

ウィザードで選択したディレクトリに Azure AD アプリケーションが作成されました。

[Azure Active Directory の詳細を確認する](http://azure.microsoft.com/services/active-directory/)

##オンになっている場合 *個々 のユーザー アカウント認証を無効にする*, 、自分のプロジェクトにどのような変更が加えられたか。
NuGet パッケージのリファレンスが削除されるほか、ファイルが削除およびバックアップされます。 プロジェクトの状態によっては、追加のリファレンスやファイルを手動で削除したり、必要に応じてコードを変更しなければならない場合があります。

###削除される NuGet パッケージのリファレンス (存在する場合)

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###バックアップおよび削除されるコード ファイル (存在する場合)

以下の各ファイルがバックアップされ、プロジェクトから削除されます。 バックアップ ファイルは、プロジェクト ディレクトリのルートにある "Backup" フォルダーに配置されます。

- `App_Start\IdentityConfig.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Providers\ApplicationOAuthProvider.cs`

###バックアップされるコード ファイル (存在する場合)

以下の各ファイルがバックアップされてから置き換えられます。 バックアップ ファイルは、プロジェクト ディレクトリのルートにある "Backup" フォルダーに配置されます。

- `Startup.cs`
- `App_Start\Startup.Auth.cs`

##オンになっている場合 *ディレクトリ データを読み取る*, 、自分のプロジェクトにどのような変更が加えられたか。

###app.config または web.config にさらに変更が加えられた

次の構成エントリがさらに追加されました。

```
    `<appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

###Azure Active Directory アプリが更新された
Azure Active Directory アプリが追加されましたが、 *ディレクトリ データを読み取る* アクセス許可と追加のキーとして使用されたを作成した、 *Ida:password:* で、 `web.config` ファイルです。

[Azure Active Directory の詳細を確認する](http://azure.microsoft.com/services/active-directory/)


