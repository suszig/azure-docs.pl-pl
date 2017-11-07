---
title: "Usługi Azure AD v2 Windows Desktop pobieranie rozpoczęte — testowanie | Dokumentacja firmy Microsoft"
description: "Jak aplikacje .NET pulpitu systemu Windows (XAML) można wywołać interfejsu API, które wymagają tokenów dostępu przez punkt końcowy w wersji 2 usługi Azure Active Directory"
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
ms.openlocfilehash: e9fbfc301fb987c72605f8b16a707513661a1b65
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
## <a name="test-your-code"></a>Testowanie kodu

Aby przetestować aplikację, naciśnij klawisz `F5` do uruchomienia projektu w programie Visual Studio. Powinna zostać wyświetlona okno główne:

![Zrzut ekranu przedstawiający przykładowy](media/active-directory-mobileanddesktopapp-windowsdesktop-test/samplescreenshot.png)

Gdy wszystko jest gotowe do testowania, kliknij przycisk *wywołać Microsoft interfejsu API programu Graph* i używane do logowania Microsoft Azure Active Directory (konto organizacyjne) lub konta Account Microsoft (live.com, outlook.com). Jego jest po raz pierwszy, zostanie wyświetlone okno z zapytaniem logowanie użytkownika:

![Logowanie](media/active-directory-mobileanddesktopapp-windowsdesktop-test/signinscreenshot.png)

### <a name="consent"></a>Zgody
Podczas pierwszego logowania się do aplikacji, użytkownik zobaczy zgody ekran podobny do poniżej, gdzie należy jawnie akceptować:

![Zgoda ekranu](media/active-directory-mobileanddesktopapp-windowsdesktop-test/consentscreen.png)

### <a name="expected-results"></a>Oczekiwanych rezultatów
Powinny zostać wyświetlone informacje o profilu użytkownika zwrócony przez wywołanie interfejsu API programu Microsoft Graph na ekranie wyniki wywołania interfejsu API.

Podstawowe informacje o tokenie nabyte za pośrednictwem powinno również zostać wyświetlone `AcquireTokenAsync` lub `AcquireTokenSilentAsync` w polu informacji o tokenie:

|Właściwość  |Format  |Opis |
|---------|---------|---------|
|Nazwa | {Pełna nazwa użytkownika} |Użytkownik miał na imię i nazwisko|
|Nazwa użytkownika |<span>user@domain.com</span> |Nazwa użytkownika używana do identyfikacji użytkownika|
|Wygaśnięcia tokenu |{DateTime}         |Czas wygaśnięcia tokenu. MSAL będzie przedłużyć datę wygaśnięcia dla Ciebie odnowienie token, gdy jest to konieczne|
|Token dostępu |{Ciąg}         |Ciąg tokena wysłane który będą wysyłane na żądania HTTP, które wymagają nagłówek autoryzacji|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Więcej informacji o zakresach i uprawnień delegowanych
Interfejs API Graph wymaga `user.read` zasięg odczytuj profil użytkownika. Ten zakres jest automatycznie dodawany domyślnie w każdej aplikacji, jest zarejestrowany w portalu rejestracji. Niektóre inne interfejsy API Graph, a także niestandardowych interfejsów API dla serwera wewnętrznej bazy danych wymaga dodatkowych zakresów. Na przykład do wykresu `Calendars.Read` jest wymagany do listy kalendarzach. Aby uzyskać dostęp do kalendarza użytkownika w kontekście aplikacji, musisz dodać `Calendars.Read` delegowane informacji o rejestracji aplikacji, a następnie dodaj `Calendars.Read` do `AcquireTokenAsync` wywołania. Użytkownik może zostać wyświetlony monit dla dodatkowych zgody, jak zwiększyć liczbę zakresów.

<!--end-collapse-->

[!INCLUDE  [Help and Support Options](../../../../includes/active-directory-develop-help-support-include.md)]