---
title: "Przegląd kondycji usługi platformy Azure | Dokumentacja firmy Microsoft"
description: "Spersonalizowane informacje o aplikacji Azure wpływu problemów aktualnych i przyszłych usługi Azure i konserwacji."
services: Resource health
documentationcenter: 
author: rboucher
manager: 
editor: 
ms.assetid: 
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/07/2017
ms.author: robb
ms.openlocfilehash: 5f2b68e800f484a64e79f965fc855aebe090186a
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="azure-service-health"></a>Azure Service Health
Kondycja usługi Azure zawiera informacje o odpowiednim i spersonalizowane, gdy problemy w usługach Azure mieć wpływ na usługi.  Można go również przygotować do najbliższej planowanej konserwacji.

## <a name="service-health-events"></a>Zdarzenia kondycji usługi
Kondycja usługi śledzi trzy typy zdarzeń kondycji, które mogą mieć wpływ na zasoby:
1. **Usługa problemów** -problemy z usług Azure wpływających od razu. 
2. **Zaplanowanej konserwacji** -najbliższej konserwacji, które mogą mieć wpływ na dostępność usług w przyszłości.  
3. **Klasyfikatory kondycji** -zmiany w usługach Azure, które wymagają Twojej uwagi. Przykłady obejmują funkcje platformy Azure są przestarzałe lub przekracza przydział użycia.

    ![Zdarzenia kondycji usługi](./media/service-health-overview/azure-service-health-overview-7.png)

## <a name="get-started-with-service-health"></a>Wprowadzenie do usługi kondycji
Aby uruchomić Pulpit nawigacyjny kondycji usługi, wybierz Kafelek kondycja usługi na pulpicie nawigacyjnym portalu. 

![Wprowadzenie do usługi kondycji](./media/service-health-overview/azure-service-health-overview-1.png)

Jeśli Kafelek wcześniej zostały usunięte lub jest używany niestandardowy pulpit nawigacyjny, wyszukaj usługi usługi kondycji w "**wszystkie usługi**" (z góry po lewej na pulpicie nawigacyjnym).

![Wprowadzenie do usługi kondycji](./media/service-health-overview/azure-service-health-overview-1a.png)

## <a name="see-current-issues-which-impact-your-services"></a>Zobacz bieżących problemach mogących mieć wpływ na usługi
**Usługi problemów** widok zawiera wszystkie bieżące problemy w usług Azure, które wpływają na Twoich zasobów. Można zrozumieć, kiedy rozpoczął się problem i jakie usługi i regiony są w pełni funkcjonalne. Można również odczytać najnowszą aktualizację, aby zrozumieć Azure czynności aby rozwiązać ten problem. 
![Problem dotyczący usługi zarządzania](./media/service-health-overview/azure-service-health-overview-2.png)

Wybierz **potencjalny wpływ** kartę, aby zapoznać się z listą określonych zasobów własnej, które może występować problem. Możesz pobrać listę CSV tych zasobów w celu udostępnienia zespołowi.
![Zarządzanie problem dotyczący usługi — wpływ](./media/service-health-overview/azure-service-health-overview-4.png)

## <a name="get-links-and-downloadable-explanations"></a>Korzystać z linków i wyjaśnienia do pobrania 
Możesz uzyskać łącze problemu do użycia w systemie zarządzania problem. Możesz pobrać plików PDF i czasami pliki CSV można udostępniać osobom, które nie mają dostępu do portalu Azure.   
![Zarządzanie problem dotyczący usługi — Zarządzanie problemami](./media/service-health-overview/azure-service-health-overview-3.png)

## <a name="get-support-from-microsoft"></a>Uzyskiwanie pomocy technicznej firmy Microsoft
Jeśli zasób jest w złym stanie nawet po problem został rozwiązany, skontaktuj się z pomocą techniczną.  Użyj łącza pomocy technicznej po prawej stronie.  

## <a name="pin-a-personalized-health-map-to-your-dashboard"></a>Przypnij mapę spersonalizowane kondycji do pulpitu nawigacyjnego
Filtr o krytycznym znaczeniu subskrypcji, regiony i typów zasobów kondycji usługi. Zapisz filtr i Przypnij mapę world spersonalizowane kondycji do pulpitu nawigacyjnego portalu. 
![Mapa spersonalizowane kondycji filtru](./media/service-health-overview/azure-service-health-overview-6a.png)
![Przypnij mapę spersonalizowane kondycji](./media/service-health-overview/azure-service-health-overview-6b.png)

## <a name="configure-service-health-alerts"></a>Skonfiguruj alerty dotyczące kondycji usługi
Kondycja usługi Azure integruje się z monitorem Azure za pośrednictwem wiadomości e-mail, wiadomości SMS i powiadomień elementu webhook otrzymywanie powiadomień wpływ na zasoby biznesowych o znaczeniu krytycznym. Konfigurowanie alertu dziennika aktywności odpowiedniego zdarzenia kondycji usługi. Trasy alertu do odpowiednich osób w organizacji za pomocą grup akcji. Aby uzyskać więcej informacji, zobacz [skonfigurować alerty dotyczące kondycji usługi](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md)

 
