---
title: "Usługi domenowe Azure AD - Sprawdź kondycję domeny zarządzanej | Dokumentacja firmy Microsoft"
description: "Sprawdź kondycję domeny zarządzanej za pomocą strony kondycji w portalu Azure."
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 8999eec3-f9da-40b3-997a-7a2587911e96
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: ergreenl
ms.openlocfilehash: 4adbce0305bdc1a9b261f5cf644fad876d101bc6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="azure-ad-domain-services---check-the-health-of-your-managed-domain"></a>Usługi domenowe Azure AD - Sprawdź kondycję domeny zarządzanej

## <a name="your-domains-health"></a>Kondycja Twojej domeny

Na stronie kondycji portalu Azure, jest możliwe zapewnić aktualność na tym, co dzieje się w domenie zarządzanej. W tym artykule kroki do wszystkich elementów kondycji strony i omawiające jak upewnić się, że domena jest tip-top kształtu.

### <a name="view-your-health-page"></a>Wyświetl stronę kondycji

1. Przejdź do strony usługi domenowe Azure AD [portalu Azure](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices)
2. Polecenie domeny, który chcesz wyświetlić kondycję.
3. W obszarze nawigacji po lewej stronie kliknij przycisk "Kondycję".

Poniżej jest przykładową stronę kondycji.

![Przykładowa strona kondycji](.\media\active-directory-domain-services-alerts\health-page.png)

>[!NOTE]
> Oceny kondycji Twojej domeny wokół co godzinę. Po wprowadzeniu zmian do domeny zarządzanej, należy zaczekać na następny cykl oceny wyświetlić domeny zaktualizowane kondycji. Możesz sprawdzić, kiedy domenę ostatniej oceny użyciem sygnatury czasowej "Ostatniej oceny" znajduje się w prawym górnym rogu.
>

### <a name="status-of-your-managed-domain"></a>Stan domeny zarządzanej

Stan w górnym rogu kondycji Twojej strony wskazuje ogólną kondycję domenę. Stan uwzględnia wszystkie istniejące alerty w domenie. Można również wyświetlić stan domenę na stronie przeglądu usług domenowych Azure AD.

Stany domeny zarządzanej:

| Stan | Ikona | Wyjaśnienie |
| --- | :----: | --- |
| Działa | <img src= ".\media\active-directory-domain-services-alerts\running-icon.png" width = "15"> | Domeny zarządzanej sprawnego działania i nie ma żadnych alertów krytyczna lub ostrzeżenie. Ta domena może być alerty informacyjne. |
| Należy zwrócić uwagę (ostrzeżenie) | <img src= ".\media\active-directory-domain-services-alerts\warning-icon.png" width = "15"> | Nie ma żadnych alertów krytycznych na domeny zarządzanej, ale istnieją alerty ostrzegawcze, które należy rozważyć. |
| Należy zwrócić uwagę (krytyczne) | <img src= ".\media\active-directory-domain-services-alerts\critical-icon.png" width = "15"> | Brak jednego lub wielu alertów krytycznych na domeny zarządzanej. Ponadto klientowi mogą przysługiwać alerty informacyjne i/lub ostrzeżenia. |
| Wdrażanie | <img src= ".\media\active-directory-domain-services-alerts\deploying-icon.png" width = "15"> | Domeny jest w trakcie wdrażania. |

## <a name="monitors"></a>Monitory

Monitory szczegółowo niektórych aspektów o Twojej domeny zarządzanej, który nadzoruje usług domenowych Azure AD. Najlepszy sposób, aby zachować monitory w dobrej kondycji jest Rozwiąż wszystkie alerty na domeny zarządzanej.

Monitory dostępne są:
 - Backup
 - Synchronizacji przy użyciu usługi AAD

### <a name="backup"></a>Backup

Monitoruje to, jak często możemy korzystać z kopii zapasowych domeny zarządzanej. Poniżej znajduje się tabela przedstawiająca kolumny szczegółów Monitora kopii zapasowej i jakie wartości powinny być zgodne z oczekiwaniami.

| Wartość szczegółów | Wyjaśnienie |
| --- | --- |
|"Nigdy nie wykonano kopii" | Ten stan jest nowo utworzony domeny. Pierwsza kopia zapasowa jest zazwyczaj tworzony po 24 godzinach. Jeśli Twoje domeny zarządzanej nie jest nowo utworzony lub jesteś w tym stanie nietypowe ilość czasu, [się z pomocą techniczną](active-directory-ds-contact-us.md). |
| Ostatnia kopia zapasowa została wykonana 1 do 14 dni temu | Ogólnie rzecz biorąc jest to wartość oczekiwana monitora kopii zapasowej. |
| Ostatnia kopia zapasowa została wykonana więcej niż 14 dni temu. | Wszelkie czas dłuższy niż dwa tygodnie jest zbyt długi czas od ostatniej kopii zapasowej. Po pierwsze, rozwiąż wszystkie alerty, które są wyświetlane na domeny zarządzanej, a następnie, jeśli problem będzie nadal występować, [się z pomocą techniczną](active-directory-ds-contact-us.md). |


### <a name="synchronization-with-azure-ad"></a>Synchronizacja z usługą Azure AD

Przechowuje informacje o Microsoft, jak często domeny zarządzanej jest zsynchronizowany z usługą Azure Active Directory. Liczba użytkowników z domeny zarządzanej, a także ilość zmiany wprowadzone od ostatniej synchronizacji zarówno wpływają na jak długo może trwać okres synchronizacji. Ogólnie rzecz biorąc, jeżeli był dłuższy niż trzy dni od ostatniej synchronizacji, zaleca się [się z pomocą techniczną](active-directory-ds-contact-us.md).

## <a name="alerts"></a>Alerty

Alerty są problemy na Twojej domeny zarządzanej, które należy rozważyć w kolejności dla usług domenowych Azure AD uruchomić. Każdy alert problem i zawiera adres URL, który opisano wykonania określonych kroków w celu rozwiązania problemu. Aby wyświetlić wszystkie alerty i ich rozwiązania, odwiedź stronę [rozwiązywać problemy dotyczące alertów](active-directory-ds-troubleshoot-alerts.md) artykułu.

### <a name="severity"></a>Ważność
Alerty są podzielone na trzy różne poziomy ważności: krytyczne, ostrzeżenie i informacyjne.

 * **Alerty krytyczne** problemy, które poważnie obniżyć domeny zarządzanej. Te alerty powinny być kierowane natychmiast, jak Microsoft nie można monitorować, zarządzanie poprawki i synchronizacji domeny zarządzanej.
 * **Alerty ostrzegawcze** może mieć problemy, które mogą mieć wpływ na domenę w przyszłości, ale są nie zawsze "krytyczne" usługi. Te alerty konspektu najlepszych rozwiązań i nadaj sugestie dotyczące ochrony domeny zarządzanej.
 * **Alerty informacyjne** są powiadomienia, które nie są niekorzystnego wpływu na domenie. Alerty informacyjne są przeznaczone do pozwalają na odpowiednią wiedzę na temat działania wykonywane w domenie i domenowych Azure AD usług.

## <a name="next-steps"></a>Kolejne kroki
- [Rozwiązywanie alertów dotyczących domeny zarządzanej](active-directory-ds-troubleshoot-alerts.md)
- [Więcej informacji na temat usług domenowych Azure AD](active-directory-ds-features.md)
- [Skontaktuj się z nami](active-directory-ds-contact-us.md)
