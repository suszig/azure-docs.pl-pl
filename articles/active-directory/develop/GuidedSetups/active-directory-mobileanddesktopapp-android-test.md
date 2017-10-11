---
title: "Pobieranie systemu Android w wersji 2 usługi Azure AD rozpoczęte — testowanie | Dokumentacja firmy Microsoft"
description: "Jak uzyskać token dostępu i wywołania interfejsu API programu Microsoft Graph lub interfejsów API, które wymagają tokenów dostępu z punktu końcowego w wersji 2 usługi Azure Active Directory aplikacji systemu Android"
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
ms.openlocfilehash: 6df64f4820f8409bd8897d5ac24f81bffeeef102
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
## <a name="test-your-code"></a>Testowanie kodu

1. Wdrażanie kodu na urządzeniu/emulatorze.
2. Gdy wszystko jest gotowe do testowania, Microsoft Azure Active Directory (konto organizacyjne) lub konta Account Microsoft (live.com, outlook.com) można używać do logowania. 

![Zrzut ekranu przedstawiający przykładowy](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
<br/><br/>
![Logowanie](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)

### <a name="consent"></a>Zgody
Użytkownik loguje się do aplikacji, po raz pierwszy one zobaczy zgody ekran podobny do poniżej, gdzie muszą zaakceptować jawnie: 

![Zgody](media/active-directory-mobileanddesktopapp-android-test/androidconsent.png)


### <a name="expected-results"></a>Oczekiwanych rezultatów
Powinny pojawić się wyniki wywołania interfejsu API programu Microsoft Graph 'me' do uzyskania profil użytkownika — https://graph.microsoft.com/v1.0/me używane do punktu końcowego. Listę typowych Microsoft Graph punktów końcowych, zobacz to [artykułu](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Więcej informacji o zakresach i uprawnień delegowanych

Wymaga interfejsu API programu Microsoft Graph `user.read` zakresu odczytać profilu użytkownika. Ten zakres jest automatycznie dodawany domyślnie w każdej aplikacji, jest zarejestrowany w portalu rejestracji. Niektóre inne interfejsy API dla programu Microsoft Graph, a także niestandardowych interfejsów API dla serwera wewnętrznej bazy danych może wymagać dodatkowych zakresów. Na przykład dla programu Microsoft Graph, zakres `Calendars.Read` jest wymagana, aby wyświetlić listę kalendarzy użytkownika. Aby uzyskać dostęp do kalendarza użytkownika w kontekście aplikacji, musisz dodać `Calendars.Read` delegowane uprawnienia do informacji o rejestracji aplikacji, a następnie dodaj `Calendars.Read` zakres do `acquireTokenSilentAsync` wywołania. Użytkownik może zostać wyświetlony monit dla dodatkowych zgody, jak zwiększyć liczbę zakresów.

<!--end-collapse-->
