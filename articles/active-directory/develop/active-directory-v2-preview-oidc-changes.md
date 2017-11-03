---
title: "Zmienia się z punktem końcowym v2.0 usługi Azure AD | Dokumentacja firmy Microsoft"
description: "Opis zmian wprowadzonych w protokołach app model v2.0 publicznej wersji zapoznawczej."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: e6c5b891-0b5d-47dc-8184-5d0ef6a1a006
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: ae73833a68db14804dc40eaf07ff7d3effaa9052
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="important-updates-to-the-v20-authentication-protocols"></a>Ważne aktualizacje protokoły uwierzytelniania w wersji 2.0
Deweloperzy uwagi! W ciągu przyszłych dwóch tygodni wprowadzamy kilka aktualizacji do naszej v2.0 protokoły uwierzytelniania, które może oznaczać fundamentalne zmiany dla wszystkich aplikacji, zapisane w okresie podglądu.  

## <a name="who-does-this-affect"></a>Kto to wpływa?
Wszystkie aplikacje, które zostały zapisane do użycia w wersji 2.0 zbieżność punktu końcowego uwierzytelniania,

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Można znaleźć więcej informacji na temat punktu końcowego v2.0 [tutaj](active-directory-appmodel-v2-overview.md).

Jeśli utworzono aplikację, używając kodowania bezpośrednio do protokołu v2.0 punktu końcowego v2.0, za pomocą dowolnej z naszych OpenID Connect i OAuth middlewares sieci web lub innych 3 bibliotek strony do uwierzytelniania, musisz powinna być przygotowana do testowania projektów i wprowadzić zmiany, jeśli to konieczne.

## <a name="who-doesnt-this-affect"></a>Kto nie to mieć wpływ na?
Wszystkie aplikacje, które zostały napisane względem punktu końcowego uwierzytelniania usługi Azure AD w środowisku produkcyjnym,

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Ten protokół jest ustawiany w kamienie, a nie występują wszystkie zmiany.

Ponadto jeśli aplikacja **tylko** używa naszego biblioteki ADAL do uwierzytelniania, nie będziesz mieć wprowadzanie zmian.  Biblioteka ADAL ma osłonę aplikacji z zmiany.  

## <a name="what-are-the-changes"></a>Jakie są zmiany?
### <a name="removing-the-x5t-value-from-jwt-headers"></a>Usunięcie z nagłówków JWT wartości x5t
Punktu końcowego v2.0 korzysta z tokenów JWT często, która zawiera sekcja Parametry nagłówka o odpowiednie metadane dotyczące tokenu.  Jeśli dekodowanie nagłówka jednego z naszych bieżącego tokenów Jwt, może znaleźć wyglądać mniej więcej tak:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Gdzie "x5t" i "kid" właściwości identyfikacji klucza publicznego, który powinien być używany do weryfikacji podpisu tokenu, ponieważ pobrana z punktu końcowego metadanych OpenID Connect.

Zmiana, którą czynione tutaj jest Usuń właściwość "x5t".  Może nadal używać te same mechanizmy do sprawdzania poprawności tokenów, ale polegać wyłącznie na właściwość "kid" można pobrać prawidłowy klucz publiczny, jak określono w protokołu OpenID Connect. 

> [!IMPORTANT]
> **Zadania: Upewnij się, że aplikacja nie zależy od istnienia wartość x5t.**
> 
> 

### <a name="removing-profileinfo"></a>Usuwanie profile_info
Wcześniej punktu końcowego v2.0 ma zostały zwracając obiekt JSON kodowany w standardzie base64 w odpowiedzi tokenu o nazwie `profile_info`.  Podczas żądania tokenu dostępu z punktem końcowym v2.0, wysyłając żądanie:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Odpowiedź będzie wyglądać w następujący obiekt JSON:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

`profile_info` Wartość informacji o użytkowniku, która zarejestrowała się w aplikacji - ich nazwę wyświetlaną, imię, nazwisko, adres e-mail, identyfikator i tak dalej.  Przede wszystkim `profile_info` była używana do buforowania tokenu i wyświetlić celów.

Teraz zostaną usunięte `profile_info` wartość — nie martw się, nadal zapewniamy te informacje dla deweloperów w miejscu nieco inne.  Zamiast `profile_info`, teraz zwróci punktu końcowego v2.0 `id_token` w każdej odpowiedzi tokenu:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Może zdekodować i przeanalizować żądaniu, aby pobrać informacje, które odebrano od profile_info.  W żądaniu jest JSON Web Token (JWT), o treści określonej przez OpenID Connect.  Kod może powinien być tak bardzo podobne — należy po prostu wyodrębnić środkowej segmentu (jednostka) żądaniu i base64 zdekodować dostępu do obiektu JSON w.

W ciągu przyszłych dwóch tygodni, powinien kodu aplikację, aby pobrać informacje o użytkowniku z albo `id_token` lub `profile_info`; zależności jest obecny.  Dzięki temu podczas wprowadzania zmian aplikacji bezproblemowo może obsłużyć przejście od `profile_info` do `id_token` bez przeszkód.

> [!IMPORTANT]
> **Zadania: Upewnij się, że aplikacja nie zależy od istnienia `profile_info` wartość.**
> 
> 

### <a name="removing-idtokenexpiresin"></a>Usuwanie id_token_expires_in
Podobnie jak `profile_info`, zostaną również usunięte `id_token_expires_in` parametr z odpowiedzi.  Wcześniej punktu końcowego v2.0 zwróci wartość `id_token_expires_in` wraz z każdym odpowiedzi żądaniu, na przykład w odpowiedzi autoryzacji:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Lub w odpowiedzi tokenu:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

`id_token_expires_in` Wartość wskazuje liczbę sekund żądaniu pozostanie dotyczy.  Teraz, zostaną usunięte `id_token_expires_in` wartości całkowite.  Zamiast tego można użyć standardowego protokołu OpenID Connect `nbf` i `exp` oświadczeń do sprawdzenia poprawności żądaniu.  Zobacz [odwołania do tokenu v2.0](active-directory-v2-tokens.md) Aby uzyskać więcej informacji na temat tych oświadczeń.

> [!IMPORTANT]
> **Zadania: Upewnij się, że aplikacja nie zależy od istnienia `id_token_expires_in` wartość.**
> 
> 

### <a name="changing-the-claims-returned-by-scopeopenid"></a>Zmiana oświadczeń zwrócony przez zakres = openid
Ta zmiana będzie najbardziej znaczących — w rzeczywistości, będzie miało wpływ na prawie każdej aplikacji, który używa punktu końcowego v2.0.  Wiele aplikacji wysyłać żądania przy użyciu punktu końcowego v2.0 `openid` zakresu, takie jak:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Obecnie, gdy użytkownik udziela zgody na `openid` zakresu, aplikacja odbiera szereg informacji o użytkowniku w żądaniu wynikowy.  Oświadczenia te mogą obejmować nazwy, preferowane nazwy użytkownika, adres e-mail i identyfikator obiektu.

W ramach tej aktualizacji, zmieniamy informacje który `openid` zakresu daje aplikacji dostęp do, aby lepiej comform ze specyfikacją OpenID Connect.  `openid` Zakres będzie tylko umożliwić zalogować użytkownika w aplikacji, a wyświetlony identyfikator specyficzny dla aplikacji dla użytkownika w `sub` oświadczeń o żądaniu.  Oświadczenia w żądaniu tylko z `openid` zakres przyznane będzie pozbawione żadnych informacji umożliwiających identyfikację użytkownika.  Przykład żądaniu oświadczenia są:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Jeśli chcesz uzyskać dane osobowe lub informacje o użytkowniku w aplikacji, aplikacji należy zażądać dodatkowych uprawnień od użytkownika.  Wprowadzono obsługę dwa nowe zakresy z OpenID Connect specyfikacji — `email` i `profile` zakresy — które umożliwiają zrobić.

* `email` Zakres jest bardzo prosta — umożliwia dostęp aplikacji do użytkownika podstawowego adresu e-mail za pośrednictwem `email` oświadczeń w żądaniu.  Należy pamiętać, że `email` oświadczenia nie zawsze będą obecne w id_tokens — tylko zostanie on uwzględniony jeśli są dostępne w profilu użytkownika.
* `profile` Zakresu daje aplikacji dostęp do wszystkich innych podstawowe informacje o użytkowniku — ich nazwy, preferowane username identyfikator obiektu i tak dalej.

Dzięki temu można kod aplikacji w sposób minimalnego ujawnienie — użytkownik może uzyskać tylko w zestawie danych, czy aplikacja wymaga, aby rozpocząć tworzenie.  Jeśli chcesz kontynuować pobieranie pełny zestaw informacje o użytkowniku, który obecnie otrzymuje aplikacji, należy uwzględnić wszystkie trzy zakresy w swoje żądania autoryzacji:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Aplikację można rozpocząć wysyłanie `email` i `profile` natychmiast zakresy i punktu końcowego v2.0 będzie zaakceptować te dwa zakresy i rozpocząć żądanie uprawnień użytkowników w razie potrzeby.  Jednak zmiana interpretacji `openid` zakres nie zostanie zastosowana dla kilka tygodni.

> [!IMPORTANT]
> **Zadania: Dodaj `profile` i `email` zakresów, jeśli aplikacja wymaga informacji o użytkowniku.**  Należy pamiętać, że ADAL będzie zawierać oba te uprawnienia żądań domyślnie. 
> 
> 

### <a name="removing-the-issuer-trailing-slash"></a>Usuwanie wystawcy ukośnikiem na końcu.
Wcześniej wartość wystawcy, która jest wyświetlana w tokenach z punktem końcowym v2.0 trwało formularza

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

W przypadku, gdy identyfikatora dzierżawy usługi Azure AD został identyfikator guid dzierżawy, która wystawiła token.  Wprowadzone zmiany staje się wartości wystawcy

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

w obu tokeny i w dokumencie odnajdywania OpenID Connect.

> [!IMPORTANT]
> **Zadania: Upewnij się, że aplikacja akceptuje wartości wystawcy, zarówno z i bez ukośnika podczas sprawdzania poprawności wystawcy.**
> 
> 

## <a name="why-change"></a>Dlaczego mam zmienić?
Główną motywacją do wprowadzenia tych zmian jest zgodny ze specyfikacją standardowe OpenID Connect.  Będąc OpenID Connect zgodne mamy nadzieję zminimalizować różnice między integracji z usługami tożsamość firmy Microsoft oraz z innymi usługami tożsamościami w branży.  Chcemy umożliwiają deweloperom używanie bibliotek uwierzytelniania ich ulubionych typu open source bez konieczności zmiany w bibliotekach w celu uwzględnienia różnic między Microsoft.

## <a name="what-can-you-do"></a>Co należy zrobić?
Obecnie można rozpocząć wprowadzania wszystkie zmiany opisane powyżej.  Należy natychmiast:

1. **Usuń wszelkie zależności na `x5t` parametr nagłówka.**
2. **Obsługiwać przejście od `profile_info` do `id_token` w odpowiedzi tokenu.**
3. **Usuń wszelkie zależności na `id_token_expires_in` parametr odpowiedzi.**
4. **Dodaj `profile` i `email` zakresów do aplikacji, jeśli aplikacja potrzebuje informacji użytkownika podstawowego.**
5. **Zaakceptuj wartości wystawcy tokenów zarówno z i bez ukośnika.**

Nasze [dokumentacji protokołu v2.0](active-directory-v2-protocols.md) już została zaktualizowana w celu odzwierciedlenia tych zmian, mogą używać go jako odwołanie pomagające, zaktualizuj kod.

Jeśli masz dodatkowe pytania na zakres zmian, proszę Możesz dotrzeć do nas w serwisie Twitter na @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Jak często będą protokołu zmian?
Firma Microsoft nie przewiduje się, że dodatkowe fundamentalne zmiany protokoły uwierzytelniania.  Te zmiany w jednej wersji celowo są zostać grupowania, dzięki czemu nie trzeba przechodzić przez ten typ procesu aktualizacji ponownie kiedykolwiek wkrótce.  Oczywiście Będziemy nadal dodawania do usługi uwierzytelniania konwergentnej v2.0, co użytkownik może skorzystać z funkcji, ale te zmiany należy dodatku i nie podziału istniejący kod.

Wreszcie chcemy powiedzieć Dziękujemy za wypróbowanie kwestii okresie używania wersji zapoznawczej.  Szczegółowe informacje i doświadczenia naszych pilotażowe zostały nieoceniony w dotychczasowych i mamy nadzieję, że nastąpi przejście do udostępniania opinie i pomysłów.

Kodowanie przyjemność!

Dzielenie tożsamość firmy Microsoft

