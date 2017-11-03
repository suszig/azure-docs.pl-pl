---
title: "Jak diagnozowanie błędów przy użyciu usługi Azure Active Directory połączenia Kreatora"
description: "Kreator połączenia usługi active directory wykryto typ uwierzytelniania niezgodne"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/05/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 4f29f62b2996cae98b02c1ed5fcb59eca09301ef
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connection-wizard"></a>Diagnozowanie błędów przy użyciu usługi Azure Active Directory połączenia Kreatora
Podczas wykrywania poprzedni kod uwierzytelniania, Kreator wykrył typ uwierzytelniania niezgodne.   

## <a name="what-is-being-checked"></a>Co to jest sprawdzany?
**Uwaga:** poprawnie wykryć poprzedni kod uwierzytelniania w projekcie, projektu muszą zostać skompilowane.  Jeśli wystąpił błąd i nie masz poprzedni kod uwierzytelniania w projekcie, skompiluj ponownie i spróbuj ponownie.

### <a name="project-types"></a>Typy projektów
Kreator sprawdza, czy typ projektu, który projektujesz tak go wstrzyknąć logika uwierzytelniania prawo do projektu.  W przypadku dowolnego kontrolera, która jest pochodną `ApiController` projektu jest uznawany za projektu WebAPI w projekcie.  Jeśli występują tylko te kontrolery, które pochodzą z `MVC.Controller` w projekcie, projektu jest uznawany za projekt platformy MVC.  Cokolwiek innego nie jest obsługiwana przez kreatora.

### <a name="compatible-authentication-code"></a>Kod uwierzytelniania zgodne
Kreator sprawdza również ustawienia uwierzytelniania, które zostały wcześniej skonfigurowane przy użyciu kreatora lub są zgodne z kreatorem.  Jeśli wszystkie ustawienia są obecne, jest on uznawany za przypadek wielobieżnej i otwiera kreatora należy wyświetlić ustawienia.  Jeśli tylko niektóre ustawienia są obecne, jest uznawane za przypadek błędu.

W projekcie programu MVC Kreator sprawdza, czy dla każdego z następujących ustawień, wynikające z poprzednich Użyj kreatora:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Ponadto Kreator sprawdza, czy dla każdego z następujących ustawień w projekcie interfejsu API sieci Web, wynikające z poprzednich Użyj kreatora:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

### <a name="incompatible-authentication-code"></a>Kod uwierzytelniania niezgodne
Ponadto kreator próbuje wykryć wersji kod uwierzytelniania, które zostały skonfigurowane z poprzednich wersji programu Visual Studio. Jeśli wystąpił ten błąd, oznacza to, że projekt zawiera typ uwierzytelniania niezgodne. Kreator wykrywa następujące typy uwierzytelniania z poprzednich wersji programu Visual Studio:

* Uwierzytelnianie systemu Windows 
* Indywidualne konta użytkowników 
* Konta organizacyjne 

Do wykrywania uwierzytelniania systemu Windows w projekcie programu MVC, Kreator sprawdza `authentication` element na podstawie Twojej **web.config** pliku.

<pre>
    &lt;configuration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;authentication mode="Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

Aby wykrywać uwierzytelniania systemu Windows w projekcie interfejsu API sieci Web, Kreator wyszukuje `IISExpressWindowsAuthentication` element z projektu **.csproj** pliku:

<pre>
    &lt;Project&gt;
        &lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;enabled&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup>
    &lt;/Project&gt;
</pre>

Do wykrywania uwierzytelniania indywidualnych kont użytkowników, Kreator sprawdza element pakietu z sieci **Packages.config** pliku.

<pre>
    &lt;packages&gt;
        <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /&gt;</span>
    &lt;/packages&gt;
</pre>

Aby wykrywać stare formy uwierzytelniania konto organizacyjne, Kreator wygląda dla następującego elementu z **web.config**:

<pre>
    &lt;configuration&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

Aby zmienić typ uwierzytelniania, Usuń typ uwierzytelniania niezgodne, a następnie ponownie uruchom kreatora.

Aby uzyskać więcej informacji, zobacz [scenariusze uwierzytelniania dla usługi Azure AD](active-directory-authentication-scenarios.md).

#<a name="next-steps"></a>Następne kroki
- [Scenariusze uwierzytelniania dla usługi Azure AD](active-directory-authentication-scenarios.md)