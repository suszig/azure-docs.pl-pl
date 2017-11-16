---
title: "Omówienie diagnostyki usługi Azure App Service | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak sposoby rozwiązywania problemów z aplikacją sieci web w diagnostyce aplikacji usługi."
keywords: "usługi aplikacji, usługi azure app service, diagnostyki, obsługi, aplikacji sieci web, rozwiązywania problemów, samodzielnej pomocy"
services: app-service
documentationcenter: 
author: jen7714
manager: cfowler
editor: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: jennile
ms.openlocfilehash: f027e7fbc5866a85e7f55460192a1c99a71e368e
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="azure-app-service-diagnostics-overview"></a>Omówienie diagnostyki usługi aplikacji Azure 

Jeśli korzystasz z aplikacji sieci web, ma należy przygotować wszystkie problemy, które mogą powstać z 500 błędów informujący użytkowników z lokacji jest wyłączony. Diagnostyka usługi aplikacji to inteligentnego i interaktywne środowisko ułatwiające rozwiązywanie problemów aplikacji sieci web za pomocą wymagana żadna konfiguracja. Jeśli wystąpiły problemy z aplikacją sieci web, Diagnostyka aplikacji usługi wskaże co to jest niewłaściwy prowadzące do właściwe informacje, aby łatwo i szybko rozwiązać problem. 
 
Mimo że to środowisko jest najbardziej przydatne, gdy występują problemy z aplikacją sieci web w ciągu ostatnich 24 godzin, diagnostycznych wykresy będzie dostępny do analizowania przez cały czas. Dodatkowe narzędzia do rozwiązywania problemów oraz linki do dokumentacji przydatne i fora znajdują się w kolumnie po prawej stronie.

![Strona główna](./media/app-service-diagnostics/Homepage1.png)

## <a name="health-checkup"></a>Checkup kondycji

Jeśli nie wiadomo, co to jest problem z aplikacji sieci web lub nie wiadomo, jak zacząć Rozwiązywanie problemów związanych z checkup kondycji jest dobrym miejscem do rozpoczęcia. Checkup kondycji będzie analizować aplikacji sieci web, które pozwalają na szybkie i interaktywne Przegląd, wskazujący co to jest w dobrej kondycji i co to jest błąd informujący, gdzie należy zbadać problem. Interfejs inteligentnego i interaktywne zawiera wskazówki dotyczące rozwiązywania problemów proces.  

![Checkup kondycji](./media/app-service-diagnostics/HealthCheckup2.png)

Jeśli zostanie wykryty problem z kategorią problem występuje w ciągu ostatnich 24 godzin, można wyświetlić raport diagnostyczny pełnej i diagnostyka usługi aplikacji może monitować o wyświetlić porady więcej rozwiązywania problemów i kolejne kroki w celu obsługi bardziej z przewodnikiem.

![Kroki rozwiązywania problemów i dalej](./media/app-service-diagnostics/Troubleshooting3.png)

## <a name="tile-shortcuts"></a>Skróty kafelka

Jeśli znasz dokładnie jakiego rodzaju informacje, których szukasz dotyczące rozwiązywania problemów, skróty kafelka spowoduje przejście bezpośrednio do pełnej diagnostyki raport kategorię problemu, która Cię. W porównaniu do checkup kondycji skrótów fragmentu są więcej bezpośrednio, ale mniejsze z przewodnikiem sposób uzyskiwania dostępu do sieci diagnostycznych metryki.  

![Skróty kafelka](./media/app-service-diagnostics/TileShortcuts4.png)

## <a name="diagnostic-report"></a>Raport diagnostyczny

Określa, czy potrzebujesz dodatkowych informacji po uruchomieniu [checkup kondycji](#health-checkup) lub kliknięcia na jednym z [Kafelek skróty](#tile-shortcuts), pełnej diagnostyki raport będzie zawierał odpowiednie wykresie metryki z ostatnich 24 godzin. Jeśli aplikacja napotyka żadnych przestojów, jest on reprezentowany przez pomarańczowy pasek poniżej osi czasu. Można wybrać jedną z awariami uzyskanie przeanalizowane uwagi o przestoju oraz sugerowane rozwiązania. 

![Raport diagnostyczny](./media/app-service-diagnostics/DiagnosticReport5.png)

## <a name="open-app-service-diagnostics"></a>Otwórz diagnostykę usługi aplikacji

Aby uzyskać dostęp do diagnostyki aplikacji usługi, przejdź do aplikacji sieci web usługi aplikacji w [portalu Azure](https://portal.azure.com). 

Na lewym pasku nawigacyjnym kliknij **diagnozowanie i rozwiązywanie problemów**.