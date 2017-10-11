---
title: Azure AD w wersji 2 dla systemu iOS Getting Started - Test | Dokumentacja firmy Microsoft
description: "Jak aplikacje systemu iOS (Swift) można wywołać interfejsu API, które wymagają tokenów dostępu przez punkt końcowy w wersji 2 usługi Azure Active Directory"
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
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 4a88096d2b0a23708acdbc1798eac528599b4f71
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>Testowanie zapytań interfejsu API programu Microsoft Graph z aplikacji systemu iOS

Naciśnij klawisz `Command`  +  `R` na uruchamianie kodu w symulatorze.

![Zrzut ekranu przedstawiający przykładowy](media/active-directory-mobileanddesktopapp-ios-test/iostestscreenshot.png)

Gdy wszystko jest gotowe do testowania, naciśnij przycisk *"Wywołaj Microsoft interfejsu API programu Graph"* i pojawi się monit o wpisanie nazwy użytkownika i hasła.

### <a name="consent"></a>Zgody
Podczas pierwszego logowania się do aplikacji, użytkownik zobaczy zgody ekran podobny do poniżej, gdzie należy jawnie akceptować:

![Zgoda ekranu](media/active-directory-mobileanddesktopapp-ios-test/iosconsentscreen.png)

### <a name="expected-results"></a>Oczekiwanych rezultatów
Powinny pojawić się informacje o profilu użytkownika zwracane przez interfejs API programu Microsoft Graph wywołanie w *rejestrowanie* sekcji.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Więcej informacji o zakresach i uprawnień delegowanych

Wymaga interfejsu API programu Microsoft Graph `user.read` zakresu odczytać profilu użytkownika. Ten zakres jest automatycznie dodawany domyślnie w każdej aplikacji, jest zarejestrowany w portalu rejestracji. Niektóre inne interfejsy API dla programu Microsoft Graph, a także niestandardowych interfejsów API dla serwera wewnętrznej bazy danych może wymagać dodatkowych zakresów. Na przykład dla programu Microsoft Graph, zakres `Calendars.Read` jest wymagana, aby wyświetlić listę kalendarzy użytkownika. Aby uzyskać dostęp do kalendarza użytkownika w kontekście aplikacji, musisz dodać `Calendars.Read` delegowane uprawnienia do informacji o rejestracji aplikacji, a następnie dodaj `Calendars.Read` zakres do `acquireTokenSilent` wywołania. Użytkownik może zostać wyświetlony monit dla dodatkowych zgody, jak zwiększyć liczbę zakresów.

<!--end-collapse-->



