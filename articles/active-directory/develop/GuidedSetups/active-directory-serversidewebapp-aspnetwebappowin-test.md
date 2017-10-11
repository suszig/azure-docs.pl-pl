---
title: "Sieci Web ASP.NET w wersji 2 usługi Azure AD serwera pobieranie rozpoczęte — testowanie | Dokumentacja firmy Microsoft"
description: "Implementowanie logowania firmy Microsoft dla rozwiązania ASP.NET z aplikacji opartych na przeglądarce sieci web tradycyjnych przy użyciu standardowego protokołu OpenID Connect"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 00cb963e85111274c36c3a84489894811ad2dabd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a>Testowanie kodu

Naciśnij klawisz `F5` do uruchomienia projektu w programie Visual Studio. W przeglądarce zostanie otworzyć i przekierować użytkownika do *http://localhost: {port}* gdy zobaczysz *Zaloguj się przy użyciu programu Microsoft* przycisku. Przejdź dalej i kliknij go, aby zarejestrować.

Gdy wszystko jest gotowe do testowania, użyj pracy lub szkoły (Azure Active Directory) lub konto osobiste (live.com, outlook.com) do logowania. 

![Zaloguj się przy okna przeglądarki Microsoft](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin.png)

![Zaloguj się przy okna przeglądarki Microsoft](media/active-directory-serversidewebapp-aspnetwebappowin-test/aspnetbrowsersignin2.png)

#### <a name="expected-results"></a>Oczekiwanych rezultatów
Po zalogowaniu zostanie przekierowany użytkownik do strony głównej witryny sieci web, czyli adres URL HTTPS określony w aplikacji informacje rejestracyjne w portalu rejestracji aplikacji firmy Microsoft. Ta strona zawiera obecnie *Hello {użytkownika}* i łącze do wylogowania i łącze, aby wyświetlić oświadczeń użytkownika — czyli łącze do kontrolera autoryzacji utworzony wcześniej.

### <a name="see-users-claims"></a>Zobacz oświadczenia użytkownika
Wybierz, aby zobaczyć oświadczenia użytkownika. To prowadzi do kontrolera i widoku, który jest dostępny tylko dla użytkowników, którzy zostali uwierzytelnieni.

#### <a name="expected-results"></a>Oczekiwanych rezultatów
 Powinny pojawić się tabeli zawierającej podstawowe właściwości zalogowanego użytkownika:

| Właściwość | Wartość | Opis|
|---|---|---|
| Nazwa | {Pełna nazwa użytkownika} | Użytkownik miał na imię i nazwisko
|Nazwa użytkownika | <span>user@domain.com</span>| Nazwa użytkownika używana do identyfikacji zalogowanego użytkownika
| Temat| {Podmiotu}|Ciąg do unikatowego identyfikowania logowania użytkownika sieci web|
| Identyfikator dzierżawy| {Guid}| A *guid* jednoznacznie reprezentujący użytkownika usługi Azure Active Directory organizacji.|

Ponadto zobaczysz tabeli, w tym wszystkie oświadczenia użytkownika dołączana do uwierzytelniania żądań. Aby uzyskać listę wszystkich oświadczeń w identyfikator tokenu wyjaśnienie i zapoznaj [artykułu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims "lista oświadczeń w identyfikatorze tokenu").


### <a name="test-accessing-a-method-that-has-an-authorize-attribute-optional"></a>Test podczas uzyskiwania dostępu do metody, która ma *[Authorize]* atrybutu (opcjonalnie)
W tym kroku zostanie przetestować uzyskiwanie dostępu do kontrolera uwierzytelniony jako użytkownik anonimowy:<br/>
Wybierz łącze do wylogowania użytkownika i ukończyć proces wylogowywania.<br/>
Teraz w przeglądarce, wpisz http://localhost: {port} / uwierzytelnionego dostępu kontrolerze, który jest chroniony za pomocą do `[Authorize]` atrybutu

#### <a name="expected-results"></a>Oczekiwanych rezultatów
Powinien zostać wyświetlony monit wymaga uwierzytelniania, aby wyświetlić widok.

## <a name="additional-information"></a>Dodatkowe informacje

<!--start-collapse-->
### <a name="protect-your-entire-web-site"></a>Ochrona całej witryny sieci web
Aby chronić całej witryny sieci web, Dodaj `AuthorizeAttribute` do `GlobalFilters` w `Global.asax` `Application_Start` metody:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

<div></div>
<br/>

> [!NOTE]
> **Jak uniemożliwić użytkownikom tylko jednej z organizacji logować się do aplikacji**

> Domyślnie osobiste konta (w tym outlook.com, live.com i inne) oraz pracy i kont służbowych z firmy lub organizacji, która ma być zintegrowane z usługą Azure Active Directory mogą zalogować się do aplikacji. 

> Aplikacja do akceptowania logowania z usługi Azure Active Directory tylko jednej z organizacji, Zamień `Tenant` parametru w *web.config* z `Common` nazwę dzierżawy organizacji — przykład *contoso.onmicrosoft.com*. Po wykonaniu tej zmiany `ValidateIssuer` argumentu w Twojej *klasy początkowej OWIN* do `true`.

> Aby zezwolić użytkownikom tylko listę określonym organizacjom, `ValidateIssuer` na true i użyj `ValidIssuers` parametr, aby określić listę organizacji.

> Innym rozwiązaniem jest implementować niestandardowe metodę do sprawdzania poprawności wystawcy za pomocą parametru IssuerValidator. Aby uzyskać więcej informacji na temat `TokenValidationParameters`, można znaleźć pod adresem [to](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx "artykuł w witrynie MSDN TokenValidationParameters") artykuł w witrynie MSDN.

