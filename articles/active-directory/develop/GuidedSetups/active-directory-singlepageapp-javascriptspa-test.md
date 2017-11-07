---
title: "Usługi Azure AD w wersji 2 JS SPA instrukcje konfiguracji - Test | Dokumentacja firmy Microsoft"
description: "Jak aplikacje JavaScript SPA można wywołać interfejsu API, które wymagają tokenów dostępu przez punkt końcowy w wersji 2 usługi Azure Active Directory"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: e10e5bbb035878eb62d9295b91263602f8128230
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
## <a name="test-your-code"></a>Testowanie kodu

> ### <a name="testing-with-visual-studio"></a>Testowanie za pomocą programu Visual Studio
> Jeśli używasz programu Visual Studio, naciśnij klawisz `F5` do uruchomienia projektu: w przeglądarce zostanie otwarty i kieruje użytkownika do *http://localhost: {port}* której występuje *wywołać Microsoft interfejsu API programu Graph* przycisku.

<p/><!-- -->

> ### <a name="testing-with-python-or-another-web-server"></a>Testowanie za pomocą języka Python lub inny serwer sieci web
> Jeśli nie używasz programu Visual Studio, upewnij się, serwer sieci web jest uruchomiona i został on skonfigurowany do nasłuchiwania na portach TCP, oparte na folder zawierający Twoje *index.html* pliku. Dla języka Python, można uruchomić nasłuchiwanie na porcie, uruchamiając w poleceniu monitu / terminali, z folderu instalacji aplikacji:
> 
> ```bash
> python -m http.server 8080
> ```
>  Następnie otwórz przeglądarkę i typ *adresem http://localhost: 8080* lub *http://localhost: {port}* — gdy *portu* odnosi się do portu nasłuchiwania serwera sieci web. Powinna zostać wyświetlona zawartość strony index.html z *wywołać Microsoft interfejsu API programu Graph* przycisku.

## <a name="test-your-application"></a>Testowanie aplikacji

Po przeglądarki ładuje Twojej *index.html*, kliknij przycisk *wywołać Microsoft interfejsu API programu Graph* przycisku. Jeśli po raz pierwszy, przeglądarka przekierowuje do punktu końcowego v2 Microsoft Azure Active Directory, gdy zostanie wyświetlony monit o Zaloguj się.
 
![Zrzut ekranu przedstawiający przykładowy](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a>Zgody
Podczas pierwszego logowania się do aplikacji, jest wyświetlana zgody ekran podobny do następującego, gdy użytkownik musi zaakceptować:

 ![Zgoda ekranu](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)


### <a name="expected-results"></a>Oczekiwanych rezultatów
Powinny zostać wyświetlone informacje o profilu użytkownika zwracane przez odpowiedź wywołania interfejsu API programu Microsoft Graph.
 
 ![Wyniki](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

Zobacz też podstawowe informacje o tokenie nabyte w *Token dostępu* i *identyfikator tokenu oświadczeń* pola.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Więcej informacji o zakresach i uprawnień delegowanych

Wymaga interfejsu API programu Microsoft Graph `user.read` zakresu odczytać profilu użytkownika. Ten zakres jest automatycznie dodawany domyślnie w każdej aplikacji, jest zarejestrowany w portalu rejestracji. Niektóre inne interfejsy API dla programu Microsoft Graph, a także niestandardowych interfejsów API dla serwera wewnętrznej bazy danych może wymagać dodatkowych zakresów. Na przykład dla programu Microsoft Graph, zakres `Calendars.Read` jest wymagana, aby wyświetlić listę kalendarzy użytkownika. Aby uzyskać dostęp do kalendarza użytkownika w kontekście aplikacji, musisz dodać `Calendars.Read` delegowane uprawnienia do informacji o rejestracji aplikacji, a następnie dodaj `Calendars.Read` zakres do `acquireTokenSilent` wywołania. Użytkownik może zostać wyświetlony monit dla dodatkowych zgody, jak zwiększyć liczbę zakresów.

Jeśli interfejs API zaplecza nie wymaga zakresu (niezalecane), możesz użyć `clientId` jako zakres w `acquireTokenSilent` i/lub `acquireTokenRedirect` wywołania.

<!--end-collapse-->

[!INCLUDE  [Help and Support Options](../../../../includes/active-directory-develop-help-support-include.md)]
