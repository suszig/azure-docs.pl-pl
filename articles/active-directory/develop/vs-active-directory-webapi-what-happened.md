---
title: "Zmiany wprowadzone do projektu WebApi podczas łączenia z usługą Azure AD | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, co się dzieje z projektu WebApi podczas łączenia z usługą Azure AD za pomocą programu Visual Studio"
services: active-directory
documentationcenter: 
author: kraigb
manager: mtillman
editor: 
ms.assetid: 57630aee-26a2-4326-9dbb-ea2a66daa8b0
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 8a0f6e1838bcc550829c0da92dc224e1df859e9c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Co się stało z projektu WebApi (Visual Studio usługi Azure Active Directory połączenia usługi)
> [!div class="op_single_selector"]
> * [Wprowadzenie](vs-active-directory-webapi-getting-started.md)
> * [Co się stało](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a>Odwołania zostały dodane
### <a name="nuget-package-references"></a>Odwołania do pakietu NuGet
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

### <a name="net-references"></a>Odwołania do .NET
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

## <a name="code-changes"></a>Zmiany kodu
### <a name="code-files-were-added-to-your-project"></a>Pliki kodu zostały dodane do projektu
Klasa uruchamiania uwierzytelniania, **App_Start/Startup.Auth.cs** został dodany do projektu zawierającego Logika uruchamiania dla uwierzytelniania usługi Azure AD.

### <a name="startup-code-was-added-to-your-project"></a>Kod uruchomienia został dodany do projektu
Jeśli użytkownik ma już klasy uruchamiania w projekcie, **konfiguracji** metoda została zaktualizowana na obejmują wywołania `ConfigureAuth(app)`. W przeciwnym razie klasa uruchamiania został dodany do projektu.

### <a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Plik app.config lub web.config zawiera nowe wartości konfiguracji.
Dodano następujące pozycje konfiguracji.

```
    <appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

### <a name="an-azure-ad-app-was-created"></a>Utworzono aplikację usługi Azure AD
Aplikacja Azure AD został utworzony w katalogu, który wybrano w kreatorze.

[Dowiedz się więcej o usłudze Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Jeśli zaznaczone I *wyłączenie uwierzytelniania indywidualnych kont użytkowników*, dodatkowe zmiany wprowadzone do projektu?
Odwołania do pakietu NuGet zostały usunięte, a pliki zostały usunięte i kopii zapasowej. W zależności od stanu projektu może być konieczne ręczne usunięcie dodatkowe informacje lub pliki, lub zmodyfikuj kod zależnie od potrzeb.

### <a name="nuget-package-references-removed-for-those-present"></a>Odwołania do pakietu NuGet usunięte (dla tych obecny)
* `Microsoft.AspNet.Identity.Core`
* `Microsoft.AspNet.Identity.EntityFramework`
* `Microsoft.AspNet.Identity.Owin`

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Pliki kodu kopii zapasowej i usunąć (dla tych obecny)
Każdy z następujących plików został kopii zapasowej i usunięte z projektu. Pliki kopii zapasowej znajdują się w folderze "Kopia zapasowa" w głównym katalogu projektu.

* `App_Start\IdentityConfig.cs`
* `Controllers\AccountController.cs`
* `Controllers\ManageController.cs`
* `Models\IdentityModels.cs`
* `Providers\ApplicationOAuthProvider.cs`

### <a name="code-files-backed-up-for-those-present"></a>Pliki kodu kopii zapasowej (przez te obecny)
Każdy z następujących plików utworzono kopię zapasową przed figury geometrycznej. Pliki kopii zapasowej znajdują się w folderze "Kopia zapasowa" w głównym katalogu projektu.

* `Startup.cs`
* `App_Start\Startup.Auth.cs`

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Jeśli zaznaczone I *odczytuj dane katalogu*, dodatkowe zmiany wprowadzone do projektu?
### <a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>Dodatkowe zmian do pliku web.config lub app.config
Dodano następujące wpisy dodatkowe czynności konfiguracyjne.

```
    <appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

### <a name="your-azure-active-directory-app-was-updated"></a>Zaktualizowano aplikację usługi Azure Active Directory
Zaktualizowano aplikację usługi Azure Active Directory do uwzględnienia *odczytuj dane katalogu* uprawnień i dodatkowe klucz został utworzony, który następnie została użyta jako *ida: hasło* w `web.config` pliku.

## <a name="next-steps"></a>Następne kroki
- [Dowiedz się więcej o usłudze Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

