---
title: "Zmiany wprowadzone do projektu MVC podczas łączenia z usługą Azure AD | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, co się dzieje z projektu MVC podczas łączenia z usługą Azure AD za pomocą usługi Visual Studio połączone"
services: active-directory
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 8b24adde-547e-4ffe-824a-2029ba210216
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: b17c5fe500f3e2a8370ec5c4a09b62737d9afb84
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Co się stało z projektu MVC (usługa Visual Studio usługi Azure Active Directory połączono)?

> [!div class="op_single_selector"]
> - [Wprowadzenie](vs-active-directory-dotnet-getting-started.md)
> - [Co się stało](vs-active-directory-dotnet-what-happened.md)

W tym artykule identyfikuje dokładne zmiany am ASP.NET MVC projektu podczas dodawania [usługi Azure Active Directory połączenia usługi przy użyciu programu Visual Studio](vs-active-directory-add-connected-service.md).

Aby uzyskać informacje na temat pracy z podłączonej usługi, zobacz [wprowadzenie](vs-active-directory-dotnet-getting-started.md).

## <a name="added-references"></a>Dodano odwołań

Wpływa na *.NET odwołań pliku projektu) i `packages.config` (NuGet odwołania).

| Typ | Informacje ogólne |
| --- | --- |
| .NET; NuGet | Microsoft.IdentityModel.Protocol.Extensions |
| .NET; NuGet | Microsoft.Owin |
| .NET; NuGet | Microsoft.Owin.Host.SystemWeb |
| .NET; NuGet | Microsoft.Owin.Security |
| .NET; NuGet | Microsoft.Owin.Security.Cookies |
| .NET; NuGet | Microsoft.Owin.Security.OpenIdConnect |
| .NET; NuGet | Owin |
| .NET        | System.IdentityModel |
| .NET; NuGet | System.IdentityModel.Tokens.Jwt |
| .NET        | System.Runtime.Serialization |

Dodatkowe informacje w przypadku wybrania **odczytuj dane katalogu** opcji:

| Typ | Informacje ogólne |
| --- | --- |
| .NET; NuGet | EntityFramework |
| .NET        | EntityFramework.SqlServer (Visual Studio 2015 tylko) |
| .NET; NuGet | Microsoft.Azure.ActiveDirectory.GraphClient |
| .NET; NuGet | Microsoft.Data.Edm |
| .NET; NuGet | Microsoft.Data.OData |
| .NET; NuGet | Microsoft.Data.Services.Client |
| .NET; NuGet | Microsoft.IdentityModel.Clients.ActiveDirectory |
| .NET        | Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms (Visual Studio 2015 tylko) |
| .NET; NuGet | System.Spatial |

Zostaną usunięte następujące odwołania (ASP.NET 4 projektów, jak w programie Visual Studio 2015):

| Typ | Informacje ogólne |
| --- | --- |
| .NET; NuGet | Microsoft.AspNet.Identity.Core |
| .NET; NuGet | Microsoft.AspNet.Identity.EntityFramework |
| .NET; NuGet | Microsoft.AspNet.Identity.Owin |

## <a name="project-file-changes"></a>Zmiany w pliku projektu

- Ustaw właściwość `IISExpressSSLPort` różne liczby.
- Ustaw właściwość `WebProject_DirectoryAccessLevelKey` 0 lub 1 w przypadku wybrania **odczytuj dane katalogu** opcji.
- Ustaw właściwość `IISUrl` do `https://localhost:<port>/` gdzie `<port>` odpowiada `IISExpressSSLPort` wartość.

## <a name="webconfig-or-appconfig-changes"></a>zmiany w pliku Web.config lub app.config

- Dodano następujące pozycje konfiguracji:

    ```xml
    <appSettings>
        <add key="ida:ClientId" value="<ClientId from the new Azure AD app>" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="<your selected Azure domain>" />
        <add key="ida:TenantId" value="<the Id of your selected Azure AD tenant>" />
        <add key="ida:PostLogoutRedirectUri" value="<project start page, such as https://localhost:44335>" />
    </appSettings>
    ```

- Dodaje `<dependentAssembly>` elementy w obszarze `<runtime><assemblyBinding>` węzeł `System.IdentityModel.Tokens.Jwt` i `Microsoft.IdentityModel.Protocol.Extensions`.

Dodatkowe zmiany, w przypadku wybrania **odczytuj dane katalogu** opcji:

- Dodano następujący wpis konfiguracyjny w obszarze `<appSettings>`:

    ```xml
    <add key="ida:ClientSecret" value="<Azure AD app's new client secret>" />
    ```

- Dodano następujące elementy w obszarze `<configuration>`; wartości różnią się pliku mdf projektu i identyfikator w przypadku katalogu projektu:

    ```xml
    <configSections>
      <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
      <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>

    <connectionStrings>
      <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\<project-mdf-file>.mdf;Initial Catalog=<project-catalog-id>;Integrated Security=True" providerName="System.Data.SqlClient" />
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
    ```

- Dodaje `<dependentAssembly>` elementy w obszarze `<runtime><assemblyBinding>` węzeł `Microsoft.Data.Services.Client`, `Microsoft.Data.Edm`, i `Microsoft.Data.OData`.

## <a name="code-changes-and-additions"></a>Zmiany kodu

- Dodaje `[Authorize]` atrybutu `Controllers/HomeController.cs` i istniejących kontrolerów.

- Dodaje klasę uruchamiania uwierzytelniania `App_Start/Startup.Auth.cs`, zawierający Logika uruchamiania dla uwierzytelniania usługi Azure AD. W przypadku wybrania **odczytuj dane katalogu** opcji, ten plik zawiera także kod, aby otrzymać kod OAuth i exchange on dla tokenu dostępu.

- Dodaje klasę kontrolera `Controllers/AccountController.cs`, zawierającego `SignIn` i `SignOut` metody.

- Dodano widok częściowy `Views/Shared/_LoginPartial.cshtml`, zawierającą łącze akcji dla `SignIn` i `SignOut`.

- Dodano widok częściowy `Views/Account/SignoutCallback.cshtml`, zawiera kod HTML wylogowania interfejsu użytkownika.

- Zaktualizowano `Startup.Configuration` metodę w celu uwzględnienia wywołanie `ConfigureAuth(app)` Jeśli klasa już istnieje; w przeciwnym razie dodawane `Startup` klasa, która zawiera wywołania metody.

- Dodaje `Connected Services/AzureAD/ConnectedService.json` (Visual Studio 2017 r) lub `Service References/Azure AD/ConnectedService.json` (Visual Studio 2015), zawierający informacje używanym przez program Visual Studio do śledzenia Dodawanie podłączonej usługi.

- W przypadku wybrania **odczytuj dane katalogu** opcja dodaje `Models/ADALTokenCache.cs` i `Models/ApplicationDbContext.cs` do obsługi buforowaniem tokena. Dodano także dodatkowego kontrolera i widoku w celu zilustrowania podczas uzyskiwania dostępu do informacji profilu użytkownika przy użyciu wykresu Azure interfejsów API: `Controllers/UserProfileController.cs`, `Views/UserProfile/Index.cshtml`, i `Views/UserProfile/Relogin.cshtml`

### <a name="file-backup-visual-studio-2015"></a>Tworzenie kopii zapasowej plików (Visual Studio 2015)

Podczas dodawania podłączonej usługi, Visual Studio 2015 tworzy kopię zapasową plików zmienione i usunięte. Wszystkie pliki dotyczy są zapisywane w folderze `Backup/AzureAD`. Visual Studio 2017 nie tworzy kopie zapasowe.

- `Startup.cs`
- `App_Start\IdentityConfig.cs`
- `App_Start\Startup.Auth.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Models\ManageViewModels.cs`
- `Views\Shared\_LoginPartial.cshtml`

## <a name="changes-on-azure"></a>Zmiany na platformie Azure

- Utworzona aplikacja Azure AD w domenie wybranego podczas dodawania podłączonej usługi.
- Zaktualizowano aplikację do uwzględnienia **odczytuj dane katalogu** uprawnienia, jeśli ta opcja została wybrana.

[Dowiedz się więcej o usłudze Azure Active Directory](https://azure.microsoft.com/services/active-directory/).

## <a name="next-steps"></a>Kolejne kroki

- [Scenariusze uwierzytelniania usługi Azure Active Directory](active-directory-authentication-scenarios.md)
- [Dodaj logowanie z firmy Microsoft do aplikacji sieci web platformy ASP.NET](guidedsetups/active-directory-aspnetwebapp-v1.md)
